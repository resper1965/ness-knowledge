---
titulo: Plano de maturidade do ness.brain
responsavel: Diretoria
status: ativo
versao: 1.0
ultima_revisao: 2026-10-08
---

# Plano de maturidade do ness.brain

Plano aprovado pelo CEO em 08/10/2026 para fechar os gaps de `00-governanca/analise-maturidade-ness-brain.md`,
seguindo o roteiro da seção 6 e as respostas da seção 7. Referências a arquivos e linhas são do código do `ness-brain`
nessa data. O andamento fica na seção "Andamento", ao fim.

## Contexto

A análise `00-governanca/analise-maturidade-ness-brain.md` (seção 6) propõe quatro etapas: A seed fora do código, B
qualidade medida, C MCP completo e D operação. As respostas do CEO de 08/10 ajustam o escopo:

- **Board vê tudo.** Os parâmetros de negócio são aprovados pelo board.
- **Orçamento** = Previsto x Realizado do Omie.
- **MCP** para os operadores de gestão, inclusive para ingerir propostas.
- **Fora do plano:** homologação e casos de ouro.
- **Pessoas só no cadastro.** O código guarda apenas resper como acesso de emergência.
- **Fechamento mensal:** o resumo aprovado vira arquivo no knowledge.
- **Operação:** alerta de falha, painel de saúde e deploy pelo GitHub.

O objetivo é que mudar um limiar, uma meta ou uma pessoa vire um PR aprovado ou uma edição na tela, sem deploy. O MCP
passa a ser a porta única para os operadores, e falhas são avisadas em minutos.

Cada etapa é entregue em commits pequenos no `ness-brain` (main) e PRs no `ness-knowledge`. O deploy é feito no fim de
cada etapa.

---

## Etapa A: seed fora do código

### A1. Parâmetros de negócio aprovados pelo board

**No knowledge**, um novo arquivo `05-operacao/parametros.md` traz um bloco YAML com chave, valor, unidade, dono,
racional e data de aprovação:

| Chave | Valor |
| --- | --- |
| `janela_meses` | 12 |
| `ofensor_multiplo_mediana` | 2 |
| `ofensor_min_titulos` | 6 |
| `margem_alvo` | 0.20 |
| `recorrente_meses` / `recorrente_minimo` | 6 / 5 |
| `caixa_semanas` | 13 |
| `cobertura_regras_meses` | 6 |
| `aviso_custo` | 0.8 |
| `rateio_overhead` | custo |

O arquivo de racionais passa a citar as chaves.

**No código:**

1. Criar `cloudflare/src/parametros.ts`, copiando o padrão de `racionais.ts:12` (`carregar`: busca no GitHub, cache em
   `configuracao` e servir a última cópia se o GitHub falhar).
   - Reaproveitar o parser YAML de `regras-omie-regras.ts` (`lerTexto`).
   - `PADRAO` traz os valores atuais como rede de segurança.
   - `validar` impõe tipos e faixas (por exemplo, margem entre 0 e 0.8). Um valor inválido mantém o padrão e gera aviso.
2. Sincronizar junto com as regras do Omie: chamar a partir de `talvezSincronizar` (`regras-omie.ts:72`) e no POST
   `/configuracao/regras-omie`.
3. Trocar os literais no código:
   - `dre-dados.ts:7,13` (janela).
   - `dre-gerencial.ts:230-231` (2× e 6 títulos), que passam a ser argumentos de `ofensores`.
   - `index.ts:283,298` (margem padrão das metas e da calculadora).
   - `regras-omie.ts:86` (cobertura).
   - `workflow-regras.ts:4` (aviso de custo).
4. Python: `brain/calculos.py:152,385` recebe `semanas`, `meses` e `minimo_meses` do Worker. Os parâmetros entram no
   payload do contêiner, como `config-modelos.paraContainer` já faz com os preços.
5. Tela `/configuracao/parametros` (somente leitura): valor vigente, origem (arquivo ou padrão), commit e avisos, com o
   link "propor mudança".
6. **Aprovação pelo board:** a proposta de mudança é um PR no knowledge.
   - Em `05-operacao/README` e em `00-governanca`, a regra passa a ser: PR de `parametros.md` é mesclado só com o ok de
   um membro do board, registrado no próprio PR.
   - Não muda o mecanismo técnico: quem mescla continua sendo resper.

### A2. Catálogo de KPIs (camada semântica)

- O knowledge ganha `05-operacao/kpis.md`. Cada KPI tem um id estável, nome, fórmula, filtro, fonte, dono e a seção
  correspondente dos racionais. KPIs: saldo, cobertura, a receber vencido, faturamento do mês, ritmo, margem da área,
  overhead sobre custo, overhead sobre receita, imposto, resultado operacional, preço de referência e meta por alavanca.
- No código, cada tile e cada coluna dos telões e do `indicadores` do MCP leva o `kpi` id. O link "Como é calculado"
  abre `/racionais#<seção>`.
  - Arquivos: `ui/telao-custos.ts` e `ui/telao.ts`.
  - Não mexe nas fórmulas. O objetivo é que cada número aponte para uma definição única.
- Um teste novo garante que todo id usado no código existe em `kpis.md`. O arquivo é lido via fixture; se a leitura
  falhar, o teste vira aviso.

### A3. Fatos da empresa fora dos agentes

- O knowledge ganha `01-empresa/estrutura-e-ofertas.md` (ou a pasta equivalente que já existir). O conteúdo vem de:
  - regimes tributários de ness. (Lucro Real) e n.secops (Lucro Presumido);
  - n.secops como conta corrente no Omie;
  - modelo de negócio e ofertas (n.secops, n.infraops, n.360, n.flow, trustness., forense.io);
  - obrigações por empresa.
- Os AGENT.md perdem esses fatos e ganham "leia `conhecimento/01-empresa/estrutura-e-ofertas.md`":
  - `agents/financeiro/AGENT.md:16-19,35-38`;
  - `agents/comercial/AGENT.md:24-30`;
  - `agents/backoffice/AGENT.md:24`;
  - `agents/marketing/AGENT.md:3`.
  - Comportamento e tom ficam no agente.
- `skills-inativas/*` não mudam (continuam inativas).

### A4. Pessoas só no cadastro

O cadastro recebe três papéis novos:

| Papel | Substitui |
| --- | --- |
| `aprovador_conhecimento` | `CONHECIMENTO_APROVADORES` (`conhecimento.ts:24`) |
| `destinatario_briefing` | `BRIEFING_DESTINATARIOS` (`email.ts:67,87`) |
| campo `tratamento` no usuário | `TRATAMENTOS` (`pessoas.ts:4`) |

1. Migração `0017_usuarios_tratamento.sql`: coluna `tratamento`. A lista de papéis em `usuarios.ts:9` ganha os dois
   novos.
2. Todos os leitores passam a consultar `usuarios.temPapel(env, email, papel)`:
   - `governanca.ts:21`;
   - `visao.ts:11`;
   - `agenda.ts:66` (hoje só lê a lista do ambiente);
   - `ingestao.ts:25-31`;
   - `conhecimento.ts:24`;
   - `email.ts:67,87`;
   - `pessoas.ts:4`.
3. Emergência: uma única variável `ADMIN_EMERGENCIA` com resper. Ela vale como admin mesmo com o cadastro vazio ou
   quebrado.
4. As variáveis `GOVERNANCA_ADMINS`, `INGESTAO_OPERADORES`, `INGESTAO_VALIDADORES`, `CONHECIMENTO_APROVADORES`,
   `BRIEFING_DESTINATARIOS` e `TRATAMENTOS` saem do `wrangler.jsonc` e do `env.ts`.
   - Antes do deploy, um script importa os valores atuais para o cadastro: ampliar o import já feito para os 7 usuários.
   - O que for importado é conferido na tela.
5. Membros do board:
   - `MEMBROS_PADRAO` em `visao.ts:18` sai do código.
   - `configuracao.board_membros` é migrado para o papel `board` e deixa de ser lido.
   - Resultado: uma única fonte de pessoas.
6. A tela de usuários ganha as colunas novas. Os testes `visao.test.ts` e `governanca` são ajustados.
7. Registro de decisão no knowledge: "pessoas e papéis só no cadastro; quem aprova o quê" (`00-governanca`).

### A5. Configuração operacional fora do código (menor prioridade)

- `ROTINAS` e os tetos (`env.ts:16-27`) passam para `configuracao.rotinas`, editável em `/configuracao/rotinas`, com
  `env.ts` como padrão.
- Corrigir a divergência: `rascunho-proposta` existe em `brain/rotinas.py:41` e falta em `env.ts`.
- `MODELOS_EXTRAS` sai, porque o catálogo já está em `configuracao.modelos`.
- As linhas iniciais das migrações (pausa e briefing de segunda) ficam como estão, já que migração aplicada não se edita.
  Novas linhas iniciais vão para `scripts/seed.sql`, separado do esquema.

---

## Etapa B: qualidade medida

**Fora por decisão do CEO** (casos de ouro desconsiderados por ora). Fica apenas o que vem de graça:

- Os testes de A2 (KPI id existe).
- O teste de paridade em C1 (o número do MCP é igual ao número do painel).

---

## Etapa C: MCP completo para operadores de gestão

### C1. Paridade com o painel, aplicando a visão board

1. Novas ferramentas em `mcp-ferramentas.ts` (padrão `FERRAMENTAS`), todas com escopo `leitura`:
   - `resultado_areas`, `estrutura_custos`, `overhead`, `metas` (argumento `margem`), `board` (ofensores) e
     `preco_referencia`;
   - `regras_omie` (regras vigentes e cobertura), `parametros` e `kpis`.
2. Reaproveitar `dre-dados.carregarApuracao` e as funções de `dre-gerencial.ts`. O resultado sai como JSON com o `kpi`
   id e a consulta de origem.
3. Visão board: antes de executar, chamar `podeVer(env, board, cliente.pessoa, secao)` (`visao.ts`).
   - As seções restritas são `socios`, `overhead`, `metas` e `board`.
   - Quem não pode ver recebe uma recusa clara.
   - `tools/list` esconde o que a pessoa não pode ver.
4. Teste de paridade: a mesma apuração de fixture produz os mesmos números no painel e no MCP.

### C2. Escopo derivado do cadastro

- A credencial MCP continua pessoal, mas o escopo efetivo passa a ser a interseção entre os escopos da credencial e os
  papéis atuais da pessoa no cadastro.
  - Operador: `leitura` + `ingestao:*`.
  - Validador: idem, mais `validar`.
- Quando a pessoa é removida do cadastro, a credencial morre sem precisar de revogação manual.
- Arquivos: `mcp.ts:84,109,153` e `usuarios.ts`.

### C3. Recursos do conhecimento

- `initialize` passa a anunciar `resources`.
- `resources/list` devolve uma lista fechada: racionais, parâmetros, KPIs, regras do Omie, registro de decisões e
  fechamentos mensais.
- `resources/read` lê esses arquivos via GitHub com cache, como `racionais.ts`.
- Só arquivos com `status: ativo`, nunca o repositório inteiro.
- Arquivo: `mcp.ts` (novo case junto a `:87-104`).

### C4. Ingestão de propostas comerciais

O nome é `proposta_comercial`, para não colidir com `propostas_conhecimento` nem com `brain/propostas.py`.

1. Esquema `schema/proposta-comercial.schema.json`. Campos: cliente/CNPJ, ofertas, valores, prazo, validade, status,
   área, responsável e data.
2. Receita `docs/receitas/proposta-comercial.md`. Ela entra em `RECEITAS` (`mcp-ferramentas.ts:15`) e em `ROTINAS_MCP`
   (`extrair-proposta`).
3. Em `ingestao.ts`, `ESQUEMAS` (`:20`) ganha `proposta_comercial/v1`.
   - As mensagens e a UI (`ui/ingestao.ts:18,47`) passam a ser genéricas: "documento".
   - As rotas de download de receita e esquema (`index.ts:180-184`) passam a aceitar o tipo.
4. As ferramentas `ingerir_extracao` e `estado_ingestao` (`mcp-ferramentas.ts:92-120`) recebem o argumento `receita`.
   - O escopo é por tipo: `ingestao:contrato` e `ingestao:proposta`, com o novo escopo registrado em `ESCOPOS`.
5. Migração `0018_propostas_comerciais.sql`. `validar` (`ingestao.ts:196`) normaliza a proposta aprovada nessa tabela,
   no padrão de `contratos-regras.contratoDeExtracao`.
6. Uso inicial: lista no painel (Ingestão → Propostas) e ferramenta MCP `propostas` (leitura, valores só para o board).
7. A validação continua com o CEO, como nos contratos. A quarentena é a mesma.

### C5. OAuth (fica por último na etapa C)

- A recomendação é implementar o fluxo OAuth do protocolo MCP com o Cloudflare Access como provedor de identidade.
  - Usar a biblioteca `@cloudflare/workers-oauth-provider`, com tokens guardados em D1.
  - O resultado é que qualquer cliente MCP conecta com login Google, sem colar credencial.
- A credencial por Bearer continua valendo para scripts.
- **Exige ok do CEO** para criar o recurso (KV ou tabela) e o app no Access.

### C6. Versão e limite

- Ferramentas com `versao` em `tools/list`.
- Limite de chamadas por credencial por minuto, contado em `mcp_chamadas` (resposta 429).

---

## Etapa D: operação

### D1. Alerta de falha

- Nova função `alertarFalha(env, origem, detalhe)` em `email.ts`, com destino nos admins do cadastro.
- Ela é chamada em quatro pontos:
  - `workflow.ts:223-243`, quando uma execução falha;
  - `carga-omie.ts:84-93`, quando a carga para depois de 3 erros;
  - `regras-omie.sincronizar` e `parametros`, quando há erro;
  - os crons em `index.ts:140-146`, hoje só `console.log`.
- Anti-repetição: no máximo 1 e-mail por origem a cada 6 h, controlado em `configuracao.alertas_enviados`.
- Não envia nada com dados de cliente. Leva só a origem, o horário e a mensagem de erro técnica.

### D2. Painel de saúde

Tela `/governanca/saude` (só admin), em formato telão, com:
- a última carga do Omie e a fila de erros;
- a última sincronização de regras e parâmetros (commit e problemas);
- execuções das últimas 24 h e 7 dias (ok / falhou);
- custo do mês por rotina e por conversa;
- a última versão publicada (`BRAIN_VERSAO`);
- o ping do contêiner (`container.ts:13`).

Tudo já está em D1; não há coleta nova.

### D3. Deploy pelo GitHub

- `.github/workflows/deploy.yml`: ao fazer push em `main`, roda o CI atual (`ci.yml`) e, se passar, aplica as
  migrações (`npm run migrar`) e faz o deploy (`wrangler deploy`, que constrói o contêiner no runner com Docker
  nativo).
- Exige que o CEO crie na Cloudflare um token de API com permissão de Workers, D1 e Containers e o salve como segredo
  do repositório `ness-brain` no GitHub (`CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ACCOUNT_ID`).
  - Eu não manipulo esse token; passo o passo a passo.
- Trava: se houver uma execução ou carga rodando, o deploy espera. Isso é feito por uma etapa que consulta
  `/governanca/estado` com um token só de leitura, ou pelo script de verificação já existente.
- Reverter: `wrangler rollback`, documentado no README.

---

## Pendências ligadas ao roteiro

- **Orçamento (Previsto x Realizado):**
  - Descobrir o método do Omie com `recurso_livre` (`brain/omie.py:206`), que só aceita leitura (Listar, Consultar,
    Pesquisar, Obter). Candidatos: endpoints `financas/orcamento` / `geral/orcamento`.
  - Achando o método, adicioná-lo em `brain/omie_metodos.yaml`, criar a calculadora `orcamento_vs_realizado` e a
    seção no Board.
  - Os ofensores "fora do padrão" passam a usar o orçamento quando houver, com o histórico como padrão (como diz os
    racionais).
  - Se a API não expuser o orçamento, a alternativa é um orçamento aprovado em `05-operacao/orcamento.md`, pelo
    mesmo mecanismo de A1.
- **Fechamento mensal no knowledge:**
  - Rotina `fechamento-mensal` (dia 5 de cada mês) gera o resumo estratégico, sem nomes de cliente nem valores por
    pessoa.
  - O revisor confere e o resumo vira proposta de conhecimento (`conhecimento.aprovar`) em
    `00-governanca/fechamentos/AAAA-MM.md`. resper aprova.
- **Inovação e P&D como centro de custo:** a área já existe nas regras. O e-mail ao board fica pendente de
  esclarecimento.

---

## Ordem de execução

| # | Entrega | Depende de |
| --- | --- | --- |
| 1 | A4 pessoas só no cadastro | — |
| 2 | A1 parâmetros (knowledge PR + código) | — |
| 3 | D1 alerta de falha + D2 painel de saúde | A4 (destinatários) |
| 4 | C1 paridade MCP + C2 escopo do cadastro | A1, A4 |
| 5 | C4 propostas comerciais | C2 |
| 6 | A2 catálogo de KPIs + C3 recursos MCP | A1 |
| 7 | A3 fatos da empresa fora dos agentes | — |
| 8 | Orçamento (descoberta e calculadora) | — |
| 9 | Fechamento mensal | A2 |
| 10 | D3 deploy pelo GitHub | token criado pelo CEO |
| 11 | C5 OAuth, C6 versão e limite, A5 rotinas | ok do CEO |

## Verificação

- Por entrega: `npx tsc --noEmit`, `npm run testar` e `pytest`. Cada mudança traz testes novos:
  - parâmetros (fallback e validação);
  - papéis (sem a variável de ambiente; emergência funciona);
  - MCP (paridade, recusa pela visão board, escopo derivado);
  - ingestão de proposta (esquema, quarentena, normalização);
  - alerta (anti-repetição).
- Telas e telões: screenshot local com Playwright e dados reais (só a estrutura, sem dados de cliente no chat).
- MCP: chamada `tools/list`, `tools/call` e `resources/read` com `curl` em `/mcp`, usando uma credencial de teste de
  um membro e de um não membro do board.
- Pós-deploy: o painel de saúde mostra a versão nova. Um erro forçado de sincronização (arquivo de parâmetros
  inválido numa branch de teste) dispara um único e-mail de alerta.
- Arquivos do knowledge passam pelo PR, com aprovação do CEO (parâmetros com o ok do board registrado).

## Andamento

| # | Entrega | Situação |
| --- | --- | --- |
| 1 | A4 pessoas só no cadastro | Feito em 08/10/2026 (migração 0017; no ambiente só `ADMIN_EMERGENCIA`) |
| 2 | A1 parâmetros | Código pronto; vale quando este PR (com `05-operacao/parametros.md`) for mesclado |
| 3 | D1 alerta de falha + D2 painel de saúde | Feito em 08/10/2026 (Governança → Saúde; e-mail aos administradores) |
| 4 | C1 paridade MCP + C2 escopo do cadastro | Feito em 08/10/2026 (7 ferramentas novas; visão board e escopo pelo cadastro) |
| 5 | C4 propostas comerciais | Feito em 08/10/2026 (receita proposta_comercial/v1; validação pelo CEO) |
| 6 | A2 catálogo de KPIs + C3 recursos MCP | Feito em 08/10/2026 (vale com este PR: `05-operacao/kpis.md`) |
| 7 | A3 fatos da empresa fora dos agentes | Feito em 08/10/2026 (vale com este PR: `02-estrutura/empresas-e-ofertas.md`) |
| 8–11 | Demais entregas | A fazer, na ordem acima |
