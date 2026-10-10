---
tipo: contrato-de-dados
titulo: Padrão de conexão entre sistemas
responsavel: Ricardo Esper (CEO e CTO)
status: ativo
versao: 1.0
ultima_revisao: 2026-10-09
---

# Padrão de conexão entre sistemas

Como qualquer sistema da ness. (portal, Omie, n.360, SOC, chamados e os que vierem) se conecta ao ness.brain. Vale para
integrações novas e para revisar as existentes. Decisão do CEO de 09/10/2026: **bancos separados, mesmo vocabulário,
aviso na hora, carga de segurança e conferência diária.**

## 1. Papéis

- **Sistema de registro:** cada dado tem um único lugar onde nasce e é corrigido. Exemplos: o portal (pessoas, pedidos,
  oportunidades, propostas, contratos), o Omie (financeiro e fiscal).
- **ness.brain:** lê, consolida e analisa (datalake, KPIs, nessie, MCP, boletins). Não é sistema de registro do dia a dia.
- **Escrita do brain em outro sistema:** só com autorização registrada no `00-governanca/registro-de-decisoes.md`. Hoje
  existe uma: o espelho do funil no CRM do Omie (`crm/oportunidades` e `crm/contas`).
- **Bancos separados:** nenhum sistema lê ou grava o banco de outro. A conversa é sempre pela API da fonte, com conta
  própria. Assim um sistema não derruba o outro e o dado sensível não sai de onde nasce.

## 2. Contrato de dados

Cada fonte tem um arquivo nesta pasta (`<fonte>.md`) com:

- **Entidades** e, em cada uma, os **campos exportáveis** (lista fechada; o resto não sai da fonte).
- **Chave natural:** pessoa = e-mail corporativo em minúsculas; registro = id da fonte, mais o código legível quando
  houver (`OPO-2026-0001`, `PED-2026-0001`, `PPS-01769/2026`).
- **Vocabulários:** as listas fechadas (etapas, status, tipos, motivos) com o **valor gravado** (sem acento, minúsculo,
  hífen) e o **rótulo** mostrado. É a lista única: código da fonte e do brain são conferidos contra ela.
- **Versão do contrato** (`versao:` no cabeçalho).

## 3. Canais, em ordem de preferência

1. **Aviso na hora (webhook).** A fonte, ao gravar, chama `POST https://brain.ness.com.br/integracoes/<fonte>/aviso` com:
   - corpo `{"fonte":"portal","colecao":"oportunidades","id":"123","atualizadoEm":"2026-10-09T19:00:00Z"}`, sem dado de negócio;
   - cabeçalhos `X-Ness-Momento` (epoch em segundos) e `X-Ness-Assinatura` = HMAC-SHA256 em hexadecimal de
     `<momento>.<corpo>` com o segredo compartilhado da fonte;
   - o brain recusa assinatura errada ou momento fora de 5 minutos, e responde 202 sem esperar a cópia.
   Ao receber, o brain busca o registro pela leitura incremental (canal 2). O aviso só diz "mudou"; o dado vem filtrado.
2. **Leitura incremental (pull).** `GET` paginado na fonte, ordenado por data de atualização, com o parâmetro "desde".
   O brain roda a cada 15 minutos como **rede de segurança** (aviso perdido, fonte fora do ar) e na carga inicial.
3. **Arquivo (receita com quarentena).** Quando a fonte não tem API: planilha ou documento pela ingestão do brain, com
   validação humana antes de entrar (`docs/receitas` no ness-brain).

## 4. Autenticação e segredos

- **Conta de máquina por fonte**, com papel próprio e só leitura (no portal, o papel `brain`). Não é pessoa: não abre
  registro, não aprova, não lê pela API comum.
- **Segredos** (chave da conta, segredo do webhook) só no cofre ou em `wrangler secret`. Nunca em código, conhecimento,
  chat ou e-mail.
- **Rotação:** anual, na saída de quem teve acesso, e em qualquer incidente.

## 5. Dado sensível

- **A fonte filtra na saída.** O brain nunca recebe: custo por pessoa e contrato PJ, dado de saúde (atestado, CID),
  denúncias, requisições LGPD, processos judiciais, anexos e comprovantes, contatos pessoais de clientes, observações
  livres.
- **Mínimo necessário:** campo novo no contrato só com o uso descrito (qual KPI ou resposta precisa dele).

## 6. No brain

- **Cópia por fonte:** tabelas `<fonte>_<colecao>` no D1 com os campos do contrato, e o JSON recebido no R2 do datalake
  (`<fonte>/<colecao>/<data>/…`).
- **Idempotência:** gravação por id da fonte; só substitui se `atualizadoEm` for mais novo.
- **Exclusão na fonte:** vira `removido_em` na cópia, nunca apagamento silencioso.
- **Quem lê a cópia:** KPIs, painéis, nessie, MCP e boletins. Nenhuma tela do brain escreve nela.

## 7. Qualidade

- **Conferência diária:** contagem por coleção na fonte e na cópia; diferença gera alerta (Administração › Gastos e
  segurança, e e-mail aos administradores).
- **Paridade de vocabulário:** um teste em cada repositório (fonte e brain) lê o `<fonte>.md` e falha se a lista no
  código divergir.
- **Saúde por fonte:** última carga, último aviso, erros e atraso.

## 8. Mudança de contrato

- **Campo novo:** primeiro o PR do contrato aqui, depois a fonte exporta, depois o brain usa.
- **Remover ou renomear:** versão nova do contrato, com período de convivência (os dois nomes) até o brain migrar.
- **Vocabulário:** valor novo entra dos dois lados no mesmo dia; valor gravado nunca muda de nome (muda só o rótulo).

## 9. Checklist para ligar uma fonte nova

1. Arquivo `<fonte>.md` nesta pasta, aprovado por PR (entidades, campos, chaves, vocabulários, quem é o dono).
2. Conta de máquina só leitura na fonte e segredos no cofre.
3. Leitura incremental na fonte (ou receita de arquivo).
4. Webhook assinado, se a fonte permitir.
5. Tabelas da cópia e carga no brain, com o teste de paridade.
6. Conferência diária e saúde ligadas.
7. Linha no registro de decisões.

## Fontes

| Fonte | Arquivo | Situação |
| --- | --- | --- |
| Portal (Área reservada) | `portal.md` | exportação pronta (PR #1 do ness-brain-app); aviso na hora a fazer |
| Omie | `omie.md` | leitura em produção; escrita só no CRM |
| Arquivos (contratos, propostas, colaboradores) | `ingestao-arquivos.md` | em produção |
| n.360, SOC, chamados | (a criar) | planejado; entram por este checklist |

## Conexões com plataformas externas (por contrato)

Plataformas de atendimento e trabalho (Desk Manager, GLPI, Jira, Azure DevOps…), nossas ou do cliente, entram pelo
cadastro **Conexões** do portal, ligado ao contrato. Nada fica fixo no código: URL, o que coletar, frequência e
mapeamentos são do cadastro; a credencial fica no cofre e o cadastro guarda só o nome dela.

- **Esforço (horas)** vira lançamento "a confirmar" no timesheet do portal, que é a fonte única de horas.
- **Indicadores** (chamados, SLA, backlog) o brain lê direto da plataforma, pela mesma conexão.
- Cada contrato responde o checklist de atendimento (plataforma, API, credencial, extração, indicadores, horas
  faturáveis) antes de ligar uma conexão.
