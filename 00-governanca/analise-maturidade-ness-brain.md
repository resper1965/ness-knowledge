---
titulo: Análise de maturidade do ness.brain
responsavel: Diretoria
status: rascunho
versao: 0.1
ultima_revisao: 2026-10-08
---

# Análise de maturidade do ness.brain

Palco de análise da aplicação completa: o que existe hoje, como se compara a uma aplicação agêntica madura, onde a
aplicação e os dados ainda estão misturados, o papel do ness.brain como servidor MCP e o que deve ser guardado neste
repositório. Base: o código do `ness-brain` em 08/10/2026 e a avaliação anterior (`ness-brain/docs/avaliacao-agentes.md`,
de 30/09/2026). As notas são do estado verificado no código, não de intenção.

## 1. O que o ness.brain é hoje

| Camada | O que existe |
| --- | --- |
| Interface | Painel web (nessie, Semana, Entregáveis, Recorrentes, Conhecimento, Ingestão, Governança, Configuração), telão, seis dashboards (financeiro, resultado por área, estrutura de custos, sócios e financiamento, overhead, metas) e o Board, boletim por e-mail |
| Orquestração | Cloudflare Workflows; chefe de gabinete para perguntas livres; rotinas e agenda que chamam o especialista direto; revisor obrigatório antes de gravar; tetos de custo por conversa e por rotina |
| Agentes | 6 ativos (chefe de gabinete, financeiro, comercial, KYC, configurador, revisor) e 2 inativos (backoffice, marketing); 10 skills |
| Ferramentas | Omie só leitura com lista fechada de métodos e calculadoras determinísticas (caixa de 13 semanas, aging, receita por cliente, títulos em aberto, entradas estimadas, lista de clientes); índices e câmbio do Banco Central; leitura do conhecimento |
| Dados | R2 com o bruto do Omie; D1 com 16 migrações: títulos de 24 meses com departamento, projeto e categoria, cadastros, regras do Omie, contratos, extrações, índices, fotos diárias, usuários |
| APIs e MCP | Endpoint `/mcp` (JSON-RPC sobre HTTP) com 7 ferramentas, 3 rotinas (prompts) e credencial pessoal por escopo |
| Governança | Cloudflare Access; papéis (cadastro de usuários somado às listas do ambiente); visão board; pausa; trilha de auditoria; aprovação de conhecimento por PR |
| Qualidade | Integração contínua no GitHub com 134 testes em TypeScript e 216 em Python; sem avaliação de qualidade das respostas |

Desde 30/09 viraram arquitetura cinco itens que eram promessa: calculadoras determinísticas, verificação de
proveniência das fontes, revisor obrigatório, limites de custo e memória dos entregáveis aceitos.

## 2. Comparação com uma aplicação agêntica madura

Escala: 1 inexistente, 2 inicial, 3 funcional, 4 gerenciado, 5 maduro.

| Dimensão | Referência madura | ness.brain hoje | Nota | Principal gap |
| --- | --- | --- | --- | --- |
| Orquestração e roteamento | Roteador determinístico por tipo de pedido; o modelo só decide o que é aberto; planos com etapas e retomada | Rotinas vão direto ao especialista; perguntas livres dependem do chefe de gabinete; triagem por instrução em texto | 3 | Triagem de perguntas simples ainda é instrução, não código |
| Agentes e ferramentas | Registro de agentes e ferramentas com contrato (entrada, saída, permissões), versão e dono | Agentes em Markdown com lista de ferramentas; calculadoras com saída padronizada e fonte | 3 | Sem versão nem contrato formal por ferramenta; fatos da empresa ainda dentro dos agentes |
| Avaliação de qualidade (evals) | Casos de ouro por rotina, nota automática, nenhuma mudança de prompt ou modelo sem rodar | Testes de código; nenhum caso de ouro | 1 | Não sabemos se uma mudança de modelo ou de skill piora a resposta |
| Observabilidade e custo | Rastreio por execução (etapas, ferramentas, tokens, latência), painel de custo, alertas | Auditoria de chamadas, custo por execução e por conversa, logs do Workers | 3 | Sem painel de saúde nem alerta de falha de rotina e de carga |
| Dados: camadas e linhagem | Bruto, normalizado e curado separados; linhagem de cada número; checagens de qualidade na carga | Bruto no R2 e normalizado no D1; números com fonte; checagens pontuais (cobertura de regras, sem regra) | 3 | Sem camada curada versionada nem checagem automática de qualidade na carga |
| Camada semântica (métricas) | Cada KPI definido uma vez (fórmula, filtro, dono) e usado igual em todo lugar | Fórmulas no código, em pontos diferentes (contêiner Python e Worker) | 2 | Margem, overhead e caixa não têm definição única publicada |
| Conhecimento e memória | Base de conhecimento versionada, consultável com citação; memória de decisões | Este repositório com aprovação por PR; regras sincronizadas para o D1; entregáveis aceitos como memória | 3 | Parâmetros, metas e orçamento ainda fora do conhecimento; sem busca nos documentos ingeridos |
| APIs e MCP | API versionada e documentada; MCP com OAuth, recursos, ferramentas e prompts; mesmas permissões do painel | MCP com 7 ferramentas e credencial pessoal; sem API REST publicada | 2 | MCP não expõe dashboards, regras nem conhecimento; não aplica a visão board |
| Segurança e acesso | SSO, papéis no próprio sistema, revisão periódica de acesso, classificação de dados | Access com Google; papéis em cadastro e no ambiente (duas fontes); visão board; dados só leitura | 3 | Duas fontes de papéis; sem classificação formal de dados; sem revisão periódica |
| Entrega e operação | Ambiente de homologação, deploy automático e reversível, migrações testadas, metas de disponibilidade | CI com testes; deploy manual da máquina de desenvolvimento; migração manual | 2 | Sem homologação; deploy depende de uma sessão com Docker |

Leitura geral: a base de segurança e de dados é sólida para a fase 1 (só leitura, fonte em cada número). O que separa
o ness.brain de uma aplicação madura é medir a qualidade das respostas, definir as métricas uma única vez, tirar os
parâmetros de negócio do código e operar com homologação.

## 3. Aplicação e seed: o que dá para separar

Quatro destinos, como na avaliação de 30/09: **arquitetura** (código), **seed comportamental** (agentes, skills e
receitas, no `ness-brain`), **seed estratégica** (este repositório) e **dados** (datalake). Hoje há seed dentro do código
nos pontos abaixo.

| O que está misturado | Onde está hoje | Para onde vai | Dá para separar? |
| --- | --- | --- | --- |
| Pessoas e papéis (administradores, operadores, validadores, aprovadores, destinatários, tratamento) | Variáveis do `wrangler.jsonc` e listas padrão em `visao.ts` | Cadastro de usuários (D1); quem aprova o quê vai para este repositório como regra de governança | Sim. Falta tirar as listas do ambiente e deixar só uma inicial de emergência |
| Parâmetros de negócio: janela de 12 meses, 2 vezes a mediana, mínimo de 6 títulos, margem alvo de 20%, cliente recorrente em 5 de 6 meses, horizonte de 13 semanas, aviso de custo em 80% | `dre-dados.ts`, `dre-gerencial.ts`, `index.ts`, `calculos.py`, `workflow-regras.ts` | Arquivo de parâmetros aprovados neste repositório, com cópia no D1, como as regras do Omie | Sim. O código guarda só um valor de segurança para quando o arquivo não existir |
| Fatos da empresa nos agentes (regimes tributários, n.secops como conta corrente, ofertas) | `agents/financeiro`, `agents/comercial`, `agents/backoffice`, skills | Este repositório; os agentes leem daqui | Sim |
| Catálogo de modelos e preços | `config-modelos.ts`, `precos_modelos.yaml`, variável `MODELOS_EXTRAS` | Configuração operacional (D1), com tela | Sim |
| Rotinas e tetos de custo | `env.ts` | Configuração operacional (D1) | Sim |
| Composição do board e seções restritas | Configuração no D1 | Decisão registrada aqui; o D1 continua aplicando | Sim |
| Linhas iniciais em migrações (pausa, briefing de segunda) | `migrations/0002`, `migrations/0006` | Script de seed separado das migrações de esquema | Sim |
| Lista de classes, naturezas e vínculos das regras | Dois leitores (`regras-omie-regras.ts` e `regras_omie.py`) | Esquema publicado aqui; os dois leitores validam contra ele | Em parte. Os nomes podem virar dado; o significado de cada classe (por exemplo, overhead = backoffice + diretoria) é comportamento e fica no código, em `dre-gerencial.ts` |
| Mapa de campos e métodos do Omie | `omie_campos.yaml`, `omie_metodos.yaml` | Fica no código | Não. É contrato de integração e garantia de só leitura |
| Invariantes de segurança (Omie só leitura, autenticação, escopos do MCP) | Código | Fica no código | Não. Se virassem dado, uma edição poderia abrir escrita |

## 4. O ness.brain como servidor MCP

Hoje: `/mcp` com 7 ferramentas (`listar_receitas`, `ler_receita`, `indicadores`, `indice`, `corrigir_valor`,
`ingerir_extracao`, `estado_ingestao`), 3 rotinas e credencial pessoal com escopo (`leitura`, `ingestao:contrato`).

Para ser a porta única de dados e decisões, faltam:

1. **Paridade com o painel**: ferramentas para resultado por área, estrutura de custos, overhead, metas, Board e
   regras do Omie, com a mesma visão board (seções restritas só para quem pode).
2. **Papéis únicos**: o escopo da credencial MCP derivado do cadastro de usuários, não uma lista à parte.
3. **Recursos**: este repositório (decisões, regras, parâmetros) exposto como recursos MCP de leitura.
4. **Consulta ao datalake**: títulos agregados e busca nos documentos ingeridos, com a fonte de cada número.
5. **Autenticação padrão do protocolo**: OAuth, para que qualquer cliente MCP conecte sem colar credencial.
6. **Versão e limites**: versão das ferramentas e limite de chamadas por credencial.

## 5. O que deve ser guardado neste repositório

Critério: vai para o `ness-knowledge` tudo o que é decisão, definição ou parâmetro aprovado, que outra pessoa ou agente
precisa ler para agir igual. Fica no datalake o que é número vivo ou confidencial; fica no D1 o que é chave operacional.

| Este repositório (aprovado por PR) | Datalake (D1 e R2) | Configuração (D1) |
| --- | --- | --- |
| Registro de decisões | Títulos, cadastros, índices | Usuários e papéis |
| Regras do Omie (já) | Contratos com valores e extrações | Visão board ligada ou desligada |
| Parâmetros de negócio (janela, limiares, margem alvo) | Fotos diárias dos indicadores | Modelos e preços |
| Metas por área aprovadas | Arquivos originais | Agenda e tetos |
| Orçamento aprovado, se for criado aqui | Trilha de auditoria | Credenciais do MCP (só o hash) |
| Centros de custo e áreas (inclusive Inovação e P&D) | | |
| Catálogo de KPIs: fórmula, filtro e dono | | |
| Quem aprova o quê (governança) | | |
| Ficha de cliente sem valores, depois do piloto | | |
| Resumo estratégico do fechamento mensal, depois de aprovado | | |

O mecanismo já existe para as regras do Omie: proposta, PR, aprovação do CEO e cópia no D1. A proposta é estender o
mesmo caminho para parâmetros, metas, orçamento e catálogo de KPIs.

## 6. Roteiro proposto

| Etapa | Entregas | Resultado |
| --- | --- | --- |
| A. Seed fora do código | Arquivo de parâmetros e catálogo de KPIs aqui, com cópia no D1; fatos dos agentes para cá; listas de pessoas só no cadastro | Mudar um limiar ou uma meta vira PR aprovado, sem deploy |
| B. Qualidade medida | Casos de ouro para as rotinas financeiras e para o Board; rodada automática antes de trocar modelo ou skill | Mudanças com nota, não com impressão |
| C. MCP completo | Ferramentas de paridade com o painel, recursos do conhecimento, papéis do cadastro, OAuth | Qualquer agente da empresa usa o ness.brain como fonte única |
| D. Operação | Homologação, deploy pelo GitHub, alerta de falha de rotina e de carga, painel de saúde | Deploy sem depender de uma sessão; falha avisada em minutos |

## 7. Respostas do CEO (08/10/2026)

1. **Classificação de dados:** foi criado o board; o board vê tudo. Quem não é do board não vê as seções restritas.
2. **Aprovação de parâmetros:** pelo board.
3. **Orçamento:** Previsto x Realizado do Omie.
4. **Consumidores do MCP:** operadores de gestão, inclusive para ingestão de dados (contratos, propostas e outros).
5. **Homologação:** não é necessária.
6. **Casos de ouro:** desconsiderado por ora.
7. **Fonte de pessoas:** em aberto (a pergunta será refeita).
8. **Fechamento mensal:** sim, o resumo estratégico aprovado vira arquivo neste repositório.

Os racionais de cada número ficam em `05-operacao/racionais.md`, lido pelo painel, pelo chat e pelo MCP.
