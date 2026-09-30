---
titulo: ness.brain — Sistema Agêntico de Gestão da NESS
responsavel: CEO
status: proposta-aprovada-em-principio
versao: 0.2
ultima_revisao: 2026-09-30
escopo:
  - backoffice
  - financeiro
  - comercial
  - marketing
  - inteligencia
---

# ness.brain — sistema agêntico de gestão da NESS

Proposta v0.2. Incorpora as respostas do CEO de 30/09/2026 (seção 10).

## 1. Resumo

O **ness.brain** é o sistema agêntico de gestão da NESS. Proponho que ele seja um **conselho de agentes especialistas** (backoffice, financeiro, comercial, marketing e inteligência/OSINT), coordenado por um **agente chefe de gabinete** que atende o CEO. Todos leem este repositório como constituição, consultam sistemas por API em **modo somente leitura** e entregam análises, alertas e rascunhos em um painel. Nenhum agente executa ação externa na fase 1: ele recomenda, e uma pessoa decide.

Três escolhas estruturam a proposta:

1. **Um núcleo só, em padrões abertos.** Os agentes rodam em Claude Agent SDK, com identidade ("soul"), skills e ferramentas descritas em arquivos versionados: prompts em Markdown, Skills no formato aberto `SKILL.md` e ferramentas via MCP. Nenhum framework de terceiros entra na fase 1 (seção 5). Como esses três artefatos são padrões abertos, uma troca futura de runtime reaproveita quase tudo.
2. **Toda afirmação tem fonte.** O mesmo princípio de `03-provas/` vale para os agentes: número sem origem rastreável não sai do agente, e conteúdo público só usa prova `aprovada-publica`.
3. **Privilégio mínimo por agente.** Cada especialista recebe só as ferramentas e os dados de sua área. O agente de OSINT, que lê a internet aberta, não tem acesso ao Omie nem ao quadro de clientes (contenção de *prompt injection*).

Há ainda um ganho de posicionamento: a trustness. vende governança de IA (ISO/IEC 42001). O ness.brain, governado e auditável, é o primeiro case próprio dessa oferta.

### Nome

`ness.brain`, em caixa baixa e com o ponto da marca em BlueDot, no mesmo padrão de `ness.OS`. É um sistema interno: não usa o prefixo `n.`, reservado a produtos e serviços ofertados a clientes. Se vier a ser oferecido externamente, o nome passa pela validação de nomenclatura do portfólio.

Nomes derivados: repositório `ness-brain` (código, agentes, skills e conectores), painel `ness.brain` e `brain/<área>` para cada agente (ex.: `brain/financeiro`).

## 2. O que significa "não operacional" na fase 1

| Permitido | Não permitido |
| --- | --- |
| ler dados via API (Omie, CRM, Drive, e-mail com escopo restrito) | lançar, alterar ou baixar títulos no Omie |
| cruzar fontes, calcular indicadores, projetar cenários | enviar e-mail, mensagem ou proposta a terceiros |
| redigir propostas, posts, relatórios e pareceres como **rascunho** | publicar conteúdo em site ou rede social |
| abrir alerta e sugerir próxima ação com responsável | aprovar pagamento, desconto ou contratação |
| registrar resultado no painel com fonte e data | alterar este repositório sem PR revisado |

Na prática, as credenciais são de usuários **somente leitura** criados para os agentes. A proteção vale mesmo que um prompt falhe.

## 3. Arquitetura

```mermaid
flowchart TB
    CEO([CEO e diretoria]) <--> CANAL[Painel ness.brain + briefing por e-mail]
    CANAL <--> COS[ness.brain · chefe de gabinete<br/>roteia, consolida, prioriza]

    COS --> FIN[Financeiro]
    COS --> COM[Comercial]
    COS --> MKT[Marketing]
    COS --> BKO[Backoffice]
    COS --> INT[Inteligência / OSINT]
    FIN & COM & MKT & BKO & INT --> REV[Revisor<br/>fonte, marca, confidencialidade]
    REV --> PAINEL[(Painel e base de resultados)]

    subgraph Conhecimento
      K1[ness-knowledge<br/>marca, provas, decisões, quadro de clientes]
      K2[forense-io/modusoperandi]
      K3[Biblioteca de skills curadas]
    end

    subgraph Sistemas via MCP, somente leitura
      OMIE[Omie ERP]
      CRM[CRM do Omie + sistema próprio em desenvolvimento]
      GW[Google Drive, Gmail, Agenda]
      COMP[Composio: demais SaaS]
    end

    FIN --- OMIE
    COM --- CRM
    COM --- GW
    BKO --- GW
    BKO --- OMIE
    MKT --- K1
    INT --- WEB[Fontes públicas]
    COS --- K1
```

### 3.1 Camadas

| Camada | Recomendação | Alternativas aceitas |
| --- | --- | --- |
| Modelo | Claude (Opus para chefe de gabinete, financeiro e KYC; Sonnet para rotinas; Haiku para triagem) | Claude via Bedrock, Vertex AI ou Foundry, se houver exigência de nuvem específica (seção 5.1) |
| Runtime dos agentes | Claude Agent SDK, hospedado pela ness. | Claude Managed Agents, se preferirmos que a Anthropic hospede o loop e agende as rotinas |
| Identidade de cada agente ("soul") | `AGENT.md` por agente: missão, voz, limites, fontes permitidas, formato de saída; herda a constituição comum (`CLAUDE.md`) | — |
| Conhecimento | este repositório + `modusoperandi`, lidos como arquivos versionados | índice vetorial só quando o volume justificar |
| Ferramentas | Omie pela API própria (chaves da empresa), via cliente somente leitura do ness.brain; Google Workspace e Composio via MCP | MCPs próprios para outras APIs sem servidor pronto |
| Habilidades | Skills em pasta versionada no repositório `ness-brain`, com revisão de código | skills de terceiros só após curadoria (seção 6) |
| Persistência | Postgres (Supabase) ou Cloudflare D1, com tabela única de entregáveis | Notion ou planilha como painel provisório |
| Canal | painel ness.brain (registro e aprovação) + briefing semanal por e-mail com link para o painel | mensageria (WhatsApp/Slack) na fase 3, se o e-mail não bastar |
| Agendamento | rotinas agendadas (cron) por agente | disparo por evento (novo título vencido, nova oportunidade) |
| Auditoria | log de cada chamada de ferramenta, prompt e resposta, gravado por hooks do SDK | — |

### 3.2 Contrato único de entregável

Todo agente grava o resultado no mesmo formato. Isso permite um painel só, filtros por área e revisão consistente.

```json
{
  "id": "FIN-2026-10-06-001",
  "agente": "brain/financeiro",
  "tipo": "alerta | analise | rascunho | briefing",
  "titulo": "Inadimplência acima de 60 dias subiu para R$ X",
  "resumo": "…",
  "recomendacao": "Contatar o cliente Y até sexta; responsável sugerido: financeiro",
  "fontes": [{"sistema": "omie", "consulta": "contas_receber?status=vencido", "coletado_em": "2026-10-06T07:00:00-03:00"}],
  "confianca": "alta | media | baixa",
  "classificacao": "interna | confidencial | publicavel",
  "estado": "novo | em-revisao | aceito | descartado",
  "decisor": null
}
```

Regras: campo desconhecido fica vazio, nunca zero (decisão de 18/09/2026); `publicavel` só com prova `aprovada-publica`; o estado `aceito` exige nome de uma pessoa.

## 4. Os agentes

### 4.1 Chefe de gabinete (orquestrador)

- **Missão:** ser a interface única do CEO. Recebe perguntas, delega aos especialistas, consolida e prioriza.
- **Rotina:** briefing de segunda às 7h — caixa e projeção de 13 semanas, recebíveis vencidos, pipeline, renovações nos próximos 90 dias, alertas abertos e decisões pendentes no `registro-de-decisoes.md`.
- **Ferramentas:** nenhuma de sistema externo; fala só com os agentes e lê o painel.
- **Memória:** sessões do SDK para continuidade de conversa; fatos duráveis vão para o painel ou viram PR neste repositório, nunca memória opaca do agente.

### 4.2 Financeiro

- **Fontes:** Omie (contas a pagar e a receber, movimentação bancária, NF-e/NFS-e, categorias), quadro de clientes e contratos.
- **Entregas:**
  - fluxo de caixa realizado × projetado, com cenário de 13 semanas;
  - aging de recebíveis e régua de cobrança sugerida (rascunho, não envio);
  - **margem por contrato e por cliente**, cruzando receita do Omie com licenças e custos de terceiros (ex.: licenças Datto de n.secops);
  - DRE gerencial mensal e variação contra orçamento;
  - alerta de concentração de receita e de reajustes contratuais vencidos;
  - pré-fechamento mensal: checklist de lançamentos sem categoria, notas sem título e títulos sem nota.
- **Skills:** `xlsx`, `pdf`, skills próprias `ness-fechamento-mensal`, `ness-margem-contrato`, `ness-caixa-13-semanas`.
- **Acesso ao Omie:** pela API, com as chaves da empresa, e não pelo MCP oficial (custo e restrições). Como a chave da API dá acesso amplo, a garantia de somente leitura fica no cliente do ness.brain: lista fechada de métodos `Listar*`/`Consultar*`, chaves mantidas fora do alcance do agente e registro de cada consulta.

### 4.3 Comercial

- **Fontes:** CRM do Omie (pipeline atual) e, quando pronto, o sistema comercial próprio em desenvolvimento; quadro de clientes e produtos, casebook, Gmail e agenda (escopo restrito a leitura), relatórios de agente de OSINT.
- **Entregas:**
  - revisão semanal do pipeline: oportunidades paradas, próximas ações, previsão ponderada;
  - **mapa de renovação e expansão**: contratos a vencer em 90 dias, clientes n.secops sem n.infraops ou sem trustness., candidatos a n.360;
  - rascunho de proposta com a skill `ness-brand` e casos do casebook já autorizados;
  - preparação de reunião: dossiê do cliente ou prospect (junto com Inteligência);
  - análise de perdas: por que perdemos, contra quem, em que faixa de preço.
- **Skills:** `ness-brand`, `docx`, `pptx`, `ness-proposta`, `ness-account-plan`.

### 4.4 Marketing

- **Fontes:** este repositório (brandbook, direção de websites, biblioteca de provas, casebook), analytics do site, calendário editorial.
- **Entregas:**
  - pauta editorial mensal por marca (ness., trustness., forense.io), respeitando voz e território de cada uma;
  - rascunhos de posts, artigos e páginas **somente com provas `aprovada-publica`**; qualquer outra prova vira pedido de validação, não texto;
  - lista de provas e cases candidatos a publicação, com o que falta para aprová-los;
  - monitoramento de SEO e GEO (como a ness. aparece em respostas de IA);
  - revisão de conformidade de marca em materiais de terceiros.
- **Skills:** `ness-brand`, `theme-factory`, `canvas-design`, `ness-conteudo-com-prova`, `ness-voz-por-marca`.

### 4.5 Backoffice

- **Fontes:** contratos (Drive), Omie (fornecedores), cadastros internos, políticas.
- **Entregas:**
  - inventário de contratos com vencimentos, reajustes, multas e renovação automática;
  - inventário de fornecedores e assinaturas SaaS com custo e dono (cortes de custo);
  - calendário de obrigações: certidões, seguros, certificados digitais, prazos fiscais;
  - checklist de admissão e desligamento (rascunho para RH, sem executar acessos);
  - apoio à própria conformidade (ISO 27001 e LGPD internos), com a skill `iso27001`.
- **Skills:** `iso27001`, `docx`, `xlsx`, `ness-contratos`.

### 4.6 Inteligência e OSINT: know your client (transversal)

Foco aprovado: **know your client (KYC)**. O agente responde a uma pergunta objetiva: "é seguro e faz sentido contratar, renovar ou ampliar com esta empresa?".

**Dossiê KYC padrão** (skill `ness-kyc`), gerado para prospect qualificado, cliente novo, renovação relevante e, com o mesmo método, fornecedor crítico:

| Bloco | Conteúdo | Fontes típicas |
| --- | --- | --- |
| Identificação | CNPJ, razão social, situação cadastral, CNAE, capital social, endereço, data de abertura | Receita Federal / bases públicas de CNPJ |
| Estrutura societária | quadro de sócios e administradores, grupo econômico, empresas ligadas | QSA, juntas comerciais |
| Sanções e listas restritivas | CEIS, CNEP, CEPIM, listas de sanções internacionais (ex.: OFAC, ONU) | Portal da Transparência, listas oficiais |
| Exposição política | sócios ou administradores PEP | bases públicas de PEP |
| Processos e passivos | ações judiciais relevantes, trabalhistas, recuperação judicial, protestos | tribunais, diários oficiais |
| Mídia adversa | fraude, corrupção, incidentes de segurança e vazamentos noticiados | imprensa, bases de incidentes |
| Saúde aparente | porte, crescimento, contratações, publicações financeiras quando existirem | sites, relatórios públicos |
| Pegada tecnológica e de segurança | stack visível, exposição pública dos domínios, incidentes conhecidos | fontes abertas, somente passivas |
| Parecer | risco baixo, médio ou alto, com os achados que sustentam a nota e as perguntas a esclarecer com o cliente | síntese do agente, revisada por pessoa |

O dossiê alimenta crédito (financeiro), abordagem e proposta (comercial) e conformidade (backoffice).

**Mapa do ecossistema tecnológico** (skill `ness-ecossistema-cliente`): com a mesma coleta passiva, o agente levanta indícios de e-mail e colaboração, nuvem, acesso remoto, segurança, backup, ERP, tamanho do time de TI e gatilhos regulatórios (DNS público, certificados, site, vagas, notícias, licitações). O comercial cruza o mapa com as ofertas da ness.. Varredura ativa, teste de login e qualquer interação com ativos do cliente são proibidos. Usos secundários, com o mesmo isolamento: concorrência (só informação pública) e superfície de exposição dos ativos da própria ness..

- **Limites:** somente coleta passiva em fontes públicas e legais; nada de credencial vazada, varredura ativa de ativos de terceiros ou engenharia social; dados de pessoa física restritos ao necessário para KYC (sócios, administradores, PEP).

- **Isolamento:** o agente de OSINT não tem acesso ao Omie, ao CRM nem ao quadro de clientes. Ele recebe uma pergunta e devolve um relatório. Conteúdo da internet é tratado como dado não confiável.
- **Base legal:** LGPD, com legítimo interesse documentado (prevenção a fraude, análise de crédito e diligência pré-contratual) e registro de fonte e data em cada item do dossiê.
- **Fronteira com a forense.io:** OSINT de gestão não é perícia. Qualquer demanda com potencial probatório segue o `modusoperandi`.

### 4.7 Revisor (controle de qualidade)

Antes de chegar ao painel, todo entregável passa por um agente revisor que verifica:

- se cada número tem fonte e data de coleta;
- se a classificação de confidencialidade está correta (o recorte do n.secops é interno);
- se a marca está grafada corretamente (`ness.`, `n.secops`) e a voz segue o brandbook;
- se o conteúdo público usa somente provas `aprovada-publica`;
- se há conclusão sem evidência ou recomendação sem responsável.

## 5. Frameworks: um núcleo, sem colecionar ferramentas

Decisão da v0.2: **não adotar OpenClaw, Hermes, Buzz ou similares na fase 1.** Os três foram citados como exemplos e avaliados; nenhum resolve um problema que o núcleo não resolva, e cada um acrescenta superfície de ataque, operação e curva de aprendizado.

| Necessidade | Como o núcleo atende | Quando reavaliar um framework |
| --- | --- | --- |
| identidade e voz de cada agente ("soul") | prompt de sistema + `AGENT.md` por agente + `CLAUDE.md` comum | — |
| habilidades | Skills (`SKILL.md`), carregadas pelo SDK | — |
| ferramentas e sistemas | MCP (Omie, Google Workspace, Composio, MCPs próprios) | — |
| orquestração | subagentes do SDK, chamados pelo chefe de gabinete | — |
| controle e auditoria | permissões por agente e hooks que registram cada chamada | — |
| agendamento | cron do servidor ou rotinas agendadas | — |
| conversa por WhatsApp/Slack | fora do escopo da fase 1 | fase 3, se o briefing por e-mail não bastar |
| trocar de modelo (LLM) | ver 5.1 | se houver exigência de modelo local ou de outro fornecedor |

**Composio** continua como opção de conector (não é framework de agente): útil para SaaS sem MCP próprio, com contas de serviço e escopos mínimos.

### 5.1 Agnosticismo de modelo

O Claude Agent SDK **não é agnóstico de LLM**: ele roda modelos Claude, acessados pela API da Anthropic ou por Amazon Bedrock, Google Vertex AI e Microsoft Foundry. Frameworks como OpenClaw e Hermes aceitam vários modelos.

A proposta aceita esse acoplamento de forma consciente:

- a qualidade de raciocínio em finanças e análise é o fator dominante na fase 1;
- o que tem valor durável fica em padrões abertos: prompts e `AGENT.md` em Markdown, Skills em `SKILL.md`, ferramentas em MCP, entregáveis no contrato JSON da seção 3.2. Nada disso depende do SDK;
- se um dia for preciso outro modelo (custo, exigência de dado local), troca-se o runtime e reaproveitam-se skills, conectores, prompts e painel.

## 6. Política de skills confiáveis

Skills são código e instruções que o agente executa. Para uma empresa de segurança, tratá-las como dependência de software é obrigatório.

1. **Fontes aceitas:** skills oficiais da Anthropic (documentos, planilhas, apresentações, PDF, pesquisa), skills da própria ness. (`ness-brand`, `iso27001`) e skills de terceiros aprovadas.
2. **Aprovação de terceiros:** leitura integral do código, verificação de chamadas de rede e de execução de comandos, fixação por hash ou versão, cópia para o repositório `ness-brain`. Nada é instalado direto de marketplace.
3. **Skills próprias a criar na fase 1:** `ness-fechamento-mensal`, `ness-margem-contrato`, `ness-caixa-13-semanas`, `ness-proposta`, `ness-conteudo-com-prova`, `ness-kyc`, `ness-briefing-ceo`.
4. **Avaliação:** cada skill própria tem um conjunto de casos de teste (entrada e saída esperada) rodado a cada mudança.

## 7. Segurança e governança

- credenciais de serviço somente leitura, por agente, guardadas em cofre de segredos; nada em prompt ou repositório;
- dados confidenciais (quadro de clientes, n.secops, financeiro) não saem para agentes que leem a internet;
- log completo de chamadas de ferramenta, com retenção definida;
- revisão humana obrigatória para qualquer item marcado `publicavel` ou enviado a terceiros;
- inventário de sistemas de IA e análise de risco no formato ISO/IEC 42001, conduzido pela trustness.;
- registro de toda mudança de escopo de agente no `registro-de-decisoes.md`.

## 8. Roteiro

| Fase | Prazo indicativo | Entregas | Critério de saída |
| --- | --- | --- | --- |
| **0 Fundação** | 2 semanas | usuários somente leitura no Omie e nas demais APIs; repositório `ness-brain`; base de entregáveis e painel mínimo; política de skills aprovada | uma consulta ao Omie devolvida no painel com fonte |
| **1 Financeiro + chefe de gabinete** | 4 semanas | caixa de 13 semanas, aging, margem por contrato, briefing de segunda | CFO/CEO confirma que os números batem com o fechamento de um mês real |
| **2 Comercial + Marketing + KYC** | 6 semanas | revisão de pipeline, mapa de renovação, rascunho de proposta, pauta editorial com provas, dossiê KYC | 3 propostas e 1 mês de pauta usados com poucas correções |
| **3 Backoffice + canais** | 4 semanas | inventário de contratos e fornecedores, calendário de obrigações; avaliação de mensageria | economia ou risco evitado identificado; decisão sobre canal adicional |
| **4 Operacional assistido** | após avaliação | primeiras ações com aprovação explícita (ex.: enviar régua de cobrança aprovada) | aprovação da diretoria, caso a caso |

Começar pelo financeiro tem motivo: é a área com dado estruturado (Omie), resultado verificável contra o fechamento e valor imediato para decisão.

## 9. Indicadores de sucesso

- horas de análise manual substituídas por semana, por área;
- porcentagem de entregáveis aceitos sem correção;
- divergência entre número do agente e número do fechamento (meta: zero em itens com fonte);
- tempo entre evento (título vencido, contrato a vencer) e alerta;
- zero publicação de prova não aprovada ou dado confidencial.

## 10. Decisões do CEO (30/09/2026)

| # | Tema | Decisão |
| --- | --- | --- |
| 1 | Fase 1 somente leitura e política de skills | aprovadas |
| 2 | ERP | **Omie** confirmado |
| 3 | CRM e painel | pipeline no CRM do Omie; sistema comercial próprio em desenvolvimento entra como segunda fonte quando tiver API |
| 4 | Usuários somente leitura | autorizados |
| 5 | Responsável humano | **Ricardo Esper** revisa e aceita os entregáveis de todas as áreas na fase 1 |
| 6 | Canal do briefing | painel ness.brain + e-mail semanal, confirmado |
| 7 | OSINT | aprovado, com foco em know your client (seção 4.6) |
| — | Nome | `ness.brain` |
| — | Frameworks de terceiros | fora da fase 1 (seção 5) |

## 11. Repositórios e ferramentas de apoio

- **`ness-knowledge` (este repositório)** continua sendo a fonte da verdade do *conhecimento*: marca, provas, casebook, quadro de clientes e decisões. O ness.brain lê daqui e só propõe mudanças por PR revisado. Esta proposta fica aqui porque é uma decisão de governança.
- **`ness-brain` (repositório privado `resper1965/ness-brain`, criado em 30/09/2026)** guarda o *sistema*: `CLAUDE.md` comum, `AGENT.md` de cada agente, skills, configuração de MCP, hooks, esquema do banco, código do painel e testes. Separar evita misturar ciclo de vida de conteúdo e de software, mantém segredos e deploy longe da base de conhecimento e permite permissões distintas.
- **Notion:** não é necessário. O conhecimento já está versionado aqui e os entregáveis ficam no painel. O Notion só faria sentido se a equipe já trabalhasse nele e quisesse ler ou comentar os entregáveis ali; nesse caso entra como destino de leitura, não como fonte.

## Referências externas

- Claude Agent SDK (capacidades: skills, subagentes, MCP, hooks, permissões, sessões): https://code.claude.com/docs/en/agent-sdk/overview

- Comparações entre Hermes Agent e OpenClaw: https://www.websiterating.com/tools/hermes-agent-vs-openclaw-comparison/ e https://innfactory.ai/en/blog/openclaw-vs-hermes-agent-comparison
- Lançamento do Buzz pela Block: https://forklog.com/en/block-launches-buzz-an-open-source-platform-for-teams-and-ai-agents/
- API do Omie: https://developer.omie.com.br/service-list/
