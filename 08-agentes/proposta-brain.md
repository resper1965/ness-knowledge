---
titulo: brain — Sistema Agêntico de Gestão da NESS
responsavel: CEO
status: proposta
versao: 0.1
ultima_revisao: 2026-09-30
escopo:
  - backoffice
  - financeiro
  - comercial
  - marketing
  - inteligencia
---

# brain — sistema agêntico de gestão da NESS

Proposta v0.1.

## 1. Resumo

O **brain** é o sistema agêntico de gestão da NESS. Proponho que ele seja um **conselho de agentes especialistas** (backoffice, financeiro, comercial, marketing e inteligência/OSINT), coordenado por um **agente chefe de gabinete** que atende o CEO. Todos leem este repositório como constituição, consultam sistemas por API em **modo somente leitura** e entregam análises, alertas e rascunhos em um painel. Nenhum agente executa ação externa na fase 1: ele recomenda, e uma pessoa decide.

Três escolhas estruturam a proposta:

1. **Núcleo portátil, interface trocável.** O raciocínio dos agentes fica em Claude Agent SDK + Skills + MCP. OpenClaw, Hermes ou Buzz entram como canal ou sala de trabalho, não como fundação. Assim, trocar de framework não obriga a reescrever os agentes.
2. **Toda afirmação tem fonte.** O mesmo princípio de `03-provas/` vale para os agentes: número sem origem rastreável não sai do agente, e conteúdo público só usa prova `aprovada-publica`.
3. **Privilégio mínimo por agente.** Cada especialista recebe só as ferramentas e os dados de sua área. O agente de OSINT, que lê a internet aberta, não tem acesso ao Omie nem ao quadro de clientes (contenção de *prompt injection*).

Há ainda um ganho de posicionamento: a trustness. vende governança de IA (ISO/IEC 42001). O brain, governado e auditável, é o primeiro case próprio dessa oferta.

### Nome

`brain` é o nome do sistema, grafado em caixa baixa como as demais marcas do ecossistema. É um nome interno: não usa o prefixo `n.`, reservado a produtos e serviços ofertados a clientes. Se o brain vier a ser oferecido externamente, o nome passa pela validação de nomenclatura do portfólio.

Nomes derivados sugeridos: `brain-skills` (biblioteca de skills curadas), `brain-painel` (painel de entregáveis) e `brain/<área>` para cada agente (ex.: `brain/financeiro`).

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
    CEO([CEO e diretoria]) <--> CANAL[Canal: painel web, Slack/WhatsApp via OpenClaw ou sala Buzz]
    CANAL <--> COS[brain · chefe de gabinete<br/>roteia, consolida, prioriza]

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
      CRM[CRM / pipeline]
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
| Modelo | Claude (Opus para chefe de gabinete e financeiro; Sonnet para rotinas; Haiku para triagem) | qualquer modelo via endpoint compatível, se o framework exigir |
| Runtime dos agentes | Claude Agent SDK ou Claude Managed Agents | Hermes Agent para o chefe de gabinete, em piloto |
| Conhecimento | este repositório + `modusoperandi`, lidos como arquivos versionados | índice vetorial só quando o volume justificar |
| Ferramentas | MCP: Omie (MCP oficial), Google Workspace, Composio para o resto | MCPs próprios para APIs sem servidor pronto |
| Habilidades | Skills em pasta versionada (`brain-skills`), com revisão de código | ClawHub e marketplaces só após curadoria (seção 6) |
| Persistência | Postgres (Supabase) ou Cloudflare D1, com tabela única de entregáveis | Notion ou planilha como painel provisório |
| Canal | painel web próprio + resumo diário no Slack/WhatsApp | OpenClaw como gateway de mensagens; Buzz como sala humano-agente |
| Agendamento | rotinas agendadas (cron) por agente | disparo por evento (novo título vencido, nova oportunidade) |
| Auditoria | log de cada chamada de ferramenta, prompt e resposta | identidade criptográfica por agente (Buzz/Nostr) na fase 3 |

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
- **Framework:** candidato natural a piloto com Hermes Agent, pela memória de longo prazo, depois de a fase 1 estabilizar.

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
- **Cuidado:** o MCP oficial do Omie também opera (não só consulta). Usar um usuário Omie com perfil somente leitura, e não depender apenas da instrução do agente.

### 4.3 Comercial

- **Fontes:** CRM/pipeline, quadro de clientes e produtos, casebook, Gmail e agenda (escopo restrito a leitura), relatórios de agente de OSINT.
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

### 4.6 Inteligência e OSINT (transversal)

Cabe, e combina com o DNA de segurança da empresa. Escopo proposto:

| Uso | Exemplo | Limite |
| --- | --- | --- |
| Prospecção B2B | perfil da empresa, porte, stack tecnológica visível, vagas abertas de TI/segurança, notícias, incidentes públicos | dados de pessoa física só no nível profissional público |
| Due diligence | fornecedor, parceiro ou cliente novo: sócios, processos públicos, sanções, saúde financeira aparente | fontes públicas e legais; nada de credencial vazada |
| Concorrência | ofertas, preços publicados, contratações, posicionamento | só informação pública |
| Superfície de exposição da própria ness. | domínios, certificados, e-mails e subdomínios expostos de ness., trustness., forense.io | somente ativos próprios |
| Sinal de venda | incidente público ou nova obrigação regulatória em setor-alvo | vira sugestão ao comercial, com cuidado de abordagem |

- **Isolamento:** o agente de OSINT não tem acesso ao Omie, ao CRM nem ao quadro de clientes. Ele recebe uma pergunta e devolve um relatório. Conteúdo da internet é tratado como dado não confiável.
- **Base legal:** LGPD (legítimo interesse documentado para prospecção B2B), registro de fontes em cada relatório.
- **Fronteira com a forense.io:** OSINT de gestão não é perícia. Qualquer demanda com potencial probatório segue o `modusoperandi`.

### 4.7 Revisor (controle de qualidade)

Antes de chegar ao painel, todo entregável passa por um agente revisor que verifica:

- se cada número tem fonte e data de coleta;
- se a classificação de confidencialidade está correta (o recorte do n.secops é interno);
- se a marca está grafada corretamente (`ness.`, `n.secops`) e a voz segue o brandbook;
- se o conteúdo público usa somente provas `aprovada-publica`;
- se há conclusão sem evidência ou recomendação sem responsável.

## 5. Frameworks agênticos: onde cada um entra

| Framework | O que oferece | Uso recomendado | Risco a controlar |
| --- | --- | --- | --- |
| **Claude Agent SDK / Managed Agents** | runtime de agentes, Skills e MCP nativos | **núcleo** de todos os especialistas | dependência de fornecedor, mitigada porque Skills e MCP são padrões abertos |
| **OpenClaw** | gateway multicanal (WhatsApp, Telegram, Slack), agendamento, grande biblioteca de skills (ClawHub) | canal de conversa do CEO com o chefe de gabinete | skills de terceiros no ClawHub são vetor de cadeia de suprimentos: instalar só skills revisadas e fixadas por versão |
| **Hermes Agent (Nous Research)** | memória de longo prazo, criação de skills a partir da experiência | piloto no chefe de gabinete, fase 2 | autoaprendizado pode desviar comportamento: skills geradas passam por PR revisado |
| **Buzz (Block, jul/2026)** | workspace aberto onde pessoas e agentes dividem canais, com identidade criptográfica por agente | "sala" da diretoria com os agentes, fase 3 | produto novo; avaliar maturidade antes de concentrar comunicação interna nele |
| **Composio** | centenas de integrações SaaS via MCP com gestão de OAuth | conectar CRM, redes, ferramentas que não têm MCP próprio | concentração de tokens em terceiro: escopos mínimos e contas de serviço |

Recomendação: começar com o núcleo em Agent SDK e um painel próprio. Adicionar OpenClaw como canal quando o briefing semanal estiver confiável. Testar Hermes e Buzz depois, com critério de saída definido.

## 6. Política de skills confiáveis

Skills são código e instruções que o agente executa. Para uma empresa de segurança, tratá-las como dependência de software é obrigatório.

1. **Fontes aceitas:** skills oficiais da Anthropic (documentos, planilhas, apresentações, PDF, pesquisa), skills da própria ness. (`ness-brand`, `iso27001`) e skills de terceiros aprovadas.
2. **Aprovação de terceiros:** leitura integral do código, verificação de chamadas de rede e de execução de comandos, fixação por hash ou versão, cópia para o repositório `brain-skills`. Nada é instalado direto de marketplace.
3. **Skills próprias a criar na fase 1:** `ness-fechamento-mensal`, `ness-margem-contrato`, `ness-caixa-13-semanas`, `ness-proposta`, `ness-conteudo-com-prova`, `ness-osint-empresa`, `ness-briefing-ceo`.
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
| **0 Fundação** | 2 semanas | usuários somente leitura no Omie e nas demais APIs; repositório `brain-skills`; base de entregáveis e painel mínimo; política de skills aprovada | uma consulta ao Omie devolvida no painel com fonte |
| **1 Financeiro + chefe de gabinete** | 4 semanas | caixa de 13 semanas, aging, margem por contrato, briefing de segunda | CFO/CEO confirma que os números batem com o fechamento de um mês real |
| **2 Comercial + Marketing + OSINT** | 6 semanas | revisão de pipeline, mapa de renovação, rascunho de proposta, pauta editorial com provas, dossiê de prospect | 3 propostas e 1 mês de pauta usados com poucas correções |
| **3 Backoffice + canais** | 4 semanas | inventário de contratos e fornecedores, calendário de obrigações; OpenClaw como canal; piloto Hermes ou Buzz | economia ou risco evitado identificado; decisão sobre framework de canal |
| **4 Operacional assistido** | após avaliação | primeiras ações com aprovação explícita (ex.: enviar régua de cobrança aprovada) | aprovação da diretoria, caso a caso |

Começar pelo financeiro tem motivo: é a área com dado estruturado (Omie), resultado verificável contra o fechamento e valor imediato para decisão.

## 9. Indicadores de sucesso

- horas de análise manual substituídas por semana, por área;
- porcentagem de entregáveis aceitos sem correção;
- divergência entre número do agente e número do fechamento (meta: zero em itens com fonte);
- tempo entre evento (título vencido, contrato a vencer) e alerta;
- zero publicação de prova não aprovada ou dado confidencial.

## 10. Decisões e acessos necessários do CEO

1. Aprovar o princípio "somente leitura" da fase 1 e a política de skills (seções 2 e 6).
2. Confirmar o ERP: entendi "omnie" como **Omie**. Se for outro sistema, a arquitetura se mantém e muda só o conector.
3. Indicar qual CRM guarda o pipeline e onde está o painel em que os resultados devem ser persistidos.
4. Criar ou autorizar a criação de usuários somente leitura no Omie e demais sistemas.
5. Definir quem revisa cada área (dono humano por agente).
6. Escolher o canal preferido para o briefing (painel, e-mail, WhatsApp ou Slack).
7. Autorizar o escopo de OSINT da seção 4.6.

## Referências externas

- Comparações entre Hermes Agent e OpenClaw: https://www.websiterating.com/tools/hermes-agent-vs-openclaw-comparison/ e https://innfactory.ai/en/blog/openclaw-vs-hermes-agent-comparison
- Lançamento do Buzz pela Block: https://forklog.com/en/block-launches-buzz-an-open-source-platform-for-teams-and-ai-agents/
- MCP oficial do Omie: https://ajuda.omie.com.br/pt-BR/articles/17173343-conectando-o-omie-a-um-assistente-de-ia-mcp
