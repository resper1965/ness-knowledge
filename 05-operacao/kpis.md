---
titulo: Catálogo de KPIs do ness.brain
responsavel: Diretoria
status: ativo
versao: 1.0
ultima_revisao: 2026-10-08
---

# Catálogo de KPIs do ness.brain

Cada indicador do ness.brain é definido uma vez aqui: id estável, fórmula, filtro, fonte, dono e a seção dos
racionais (`05-operacao/racionais.md`) que explica o cálculo. O painel, o chat e o MCP (ferramenta `kpis`) usam o
mesmo id. Cada número nos telões leva ao seu KPI em "Como é calculado". Mudar uma fórmula exige mudar o código e este
arquivo no mesmo PR. Os limiares ficam em `05-operacao/parametros.md`.

```yaml
- id: saldo_caixa
  nome: Saldo em caixa
  formula: saldo das contas correntes no fim do dia anterior à data-base
  filtro: todas as contas correntes da ness. no Omie
  fonte: Omie, resumo financeiro
  dono: Financeiro
  secao: Saldo e caixa de 13 semanas
- id: cobertura_caixa
  nome: Cobertura de caixa
  formula: semanas até a primeira semana com saldo projetado negativo
  filtro: títulos em aberto com vencimento nas próximas caixa_semanas semanas
  fonte: Omie, contas a pagar e a receber
  dono: Financeiro
  secao: Saldo e caixa de 13 semanas
- id: caixa_estimado
  nome: Caixa estimado
  formula: saldo projetado mais as entradas estimadas de clientes recorrentes e de contratos aprovados
  filtro: cliente recorrente em recorrente_minimo dos recorrente_meses meses
  fonte: Omie e contratos aprovados na ingestão
  dono: Financeiro
  secao: Caixa estimado (linha tracejada)
- id: receber_vencido_pct
  nome: A receber vencido
  formula: títulos a receber vencidos ÷ total a receber em aberto
  filtro: títulos não cancelados
  fonte: Omie, contas a receber
  dono: Financeiro
  secao: Aging de recebíveis
- id: faturamento_mes
  nome: Faturamento do mês
  formula: soma dos títulos a receber emitidos do dia 1 até hoje
  filtro: títulos não cancelados
  fonte: Omie, contas a receber
  dono: Financeiro
  secao: Faturamento do mês e ritmo
- id: ritmo_faturamento
  nome: Ritmo de faturamento
  formula: faturamento do mês ÷ faturamento no mesmo número de dias do mês anterior − 1
  filtro: títulos não cancelados
  fonte: Omie, contas a receber
  dono: Financeiro
  secao: Faturamento do mês e ritmo
- id: receita_operacional
  nome: Receita operacional
  formula: títulos a receber dos departamentos de classe área produtiva, na janela
  filtro: janela_meses meses completos, por mês de emissão
  fonte: datalake (títulos do Omie) e regras do Omie
  dono: Diretoria
  secao: Resultado por área
- id: custo_areas
  nome: Custo das áreas
  formula: títulos a pagar dos departamentos de classe área produtiva, com sócios que entregam na área e rateios
  filtro: janela_meses meses completos, por mês de emissão
  fonte: datalake e regras do Omie
  dono: Diretoria
  secao: Resultado por área
- id: margem_area
  nome: Margem da área
  formula: (receita da área − custo direto da área) ÷ receita da área; sem overhead
  filtro: janela_meses meses completos
  fonte: datalake e regras do Omie
  dono: Diretoria
  secao: Resultado por área
- id: resultado_operacional
  nome: Resultado operacional
  formula: receita operacional − custo das áreas − backoffice − diretoria − imposto sobre faturamento − Inovação e P&D
  filtro: janela_meses meses completos
  fonte: datalake e regras do Omie
  dono: Diretoria
  secao: Resultado por área
- id: estrutura_custos
  nome: Estrutura de custos
  formula: despesa por classe do departamento ÷ soma das despesas com regra
  filtro: sem transferências entre contas
  fonte: datalake e regras do Omie
  dono: Diretoria
  secao: Estrutura de custos
- id: imposto_faturamento_pct
  nome: Imposto sobre faturamento
  formula: imposto sobre faturamento ÷ receita operacional
  filtro: janela_meses meses completos
  fonte: datalake e regras do Omie
  dono: Financeiro
  secao: Overhead
- id: overhead_corporativo
  nome: Overhead corporativo
  formula: despesas de backoffice + diretoria e gestão (com sócios, conforme a opção)
  filtro: sem Inovação e P&D, financiamento, itens financeiros e transferências
  fonte: datalake e regras do Omie
  dono: Diretoria
  secao: Overhead
- id: overhead_sobre_custo
  nome: Overhead sobre o custo direto
  formula: overhead corporativo ÷ custo das áreas produtivas
  filtro: janela_meses meses completos
  fonte: datalake e regras do Omie
  dono: Diretoria
  secao: Overhead
- id: overhead_sobre_receita
  nome: Overhead sobre a receita
  formula: overhead corporativo ÷ receita operacional
  filtro: janela_meses meses completos
  fonte: datalake e regras do Omie
  dono: Diretoria
  secao: Overhead
- id: remuneracao_socios
  nome: Remuneração de sócios
  formula: soma da remuneração de sócios (DL e PL), tratada como custo
  filtro: valores somados, sem abrir por pessoa
  fonte: datalake e regras do Omie
  dono: Board
  secao: Remuneração de sócios (DL e PL)
- id: financiamento
  nome: Financiamento pago e recebido
  formula: saídas e entradas da classe empréstimos e financiamentos
  filtro: fora do resultado operacional
  fonte: datalake e regras do Omie
  dono: Financeiro
  secao: Estrutura de custos
- id: preco_referencia
  nome: Preço de referência
  formula: custo direto × (1 + overhead sobre o custo) ÷ (1 − imposto − margem alvo)
  filtro: taxa da área ou média da empresa
  fonte: apuração do overhead
  dono: Diretoria
  secao: Calculadora de preço
- id: meta_area
  nome: Meta por área
  formula: para a margem_alvo, a receita necessária, o custo máximo ou o overhead máximo (alavancas isoladas)
  filtro: janela_meses meses completos
  fonte: apuração do resultado por área
  dono: Board
  secao: Metas por área
- id: ofensores_caixa
  nome: Maiores ofensores de caixa
  formula: categorias com maior total a pagar na janela
  filtro: sem sócios, financiamento e transferências
  fonte: datalake e regras do Omie
  dono: Board
  secao: "Board: ofensores de caixa"
- id: despesa_fora_do_padrao
  nome: Despesa fora do padrão
  formula: pico mensal acima de ofensor_multiplo_mediana × a mediana mensal da categoria
  filtro: categorias com pelo menos ofensor_min_titulos títulos
  fonte: datalake e regras do Omie
  dono: Board
  secao: "Board: ofensores de caixa"
- id: inovacao_pd
  nome: Inovação e P&D
  formula: despesa e receita da classe Inovação e P&D
  filtro: fora do overhead rateado e da margem das áreas
  fonte: datalake e regras do Omie
  dono: Board
  secao: Inovação e P&D
```
