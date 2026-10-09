---
titulo: Contrato de dados — Omie
responsavel: Ricardo Esper (CEO e CTO)
status: ativo
versao: 1.0
ultima_revisao: 2026-10-09
---

# Contrato de dados: Omie

O Omie é o sistema de registro **financeiro e fiscal** (contas a pagar e a receber, movimentos, departamentos,
categorias, projetos, clientes e fornecedores, orçamento). O brain lê; a única escrita autorizada é o espelho do funil.

## Leitura

- **Canal:** leitura incremental (§3.2 do padrão), pela API `https://app.omie.com.br/api/v1/`, só com métodos de leitura
  (`Listar`, `Consultar`, `Pesquisar`, `Obter`). Lista fechada em `brain/omie_metodos.yaml` no ness-brain.
- **Carga:** histórica de 24 meses e incremental no cron do brain; cadastros (departamentos, projetos, categorias) uma
  vez por dia; orçamento quando há.
- **Regras de leitura** (como cada item do Omie entra no resultado gerencial): `05-operacao/regras-omie/`.
- **Conta:** chaves `OMIE_NESS_APP_KEY` e `OMIE_NESS_APP_SECRET` no Worker do brain.
- **Sem webhook:** o Omie não avisa; vale só a leitura incremental.

## Escrita (autorizada pelo CEO em 09/10/2026)

- **Só** `crm/oportunidades` (`UpsertOportunidade`) e `crm/contas` (`UpsertConta`); qualquer outra escrita é recusada no
  código (`omie-crm-regras.ts`, `chamadaPermitida`).
- **Mão única:** brain → Omie. A fonte do funil é o portal; o brain lê a cópia e espelha. O que for alterado direto no
  Omie é sobrescrito no envio seguinte.
- **Chave de idempotência:** `cCodIntOp` = código da oportunidade (`OPO-…`); conta = `brain-<cnpj>` ou `brain-<nome>`.
- **Mapeamento** de etapas para fases e status do CRM do Omie: em `configuracao.omie_crm_mapa` no brain (decisão do CEO de
  09/10: prospecção → 01 Prospect; qualificação → 02 Qualificação; proposta → 04 Proposta; negociação e perdida → 05
  Conclusão; ganha → 06 Handoff Operações; status Ativo, Conquistado e Perdido).
- **Liga e desliga:** `configuracao.omie_crm_ativo`, com evento de governança.
