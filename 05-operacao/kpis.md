---
tipo: kpi
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
- id: headcount
  nome: Pessoas ativas
  formula: colaboradores do cadastro sem desligamento ou com desligamento depois de hoje
  filtro: por empresa, vínculo (CLT ou PJ) e área
  fonte: ness.brain, cadastro de colaboradores
  dono: RH
  secao: Pessoas e RH
- id: turnover_12m
  nome: Turnover em 12 meses
  formula: desligamentos nos últimos 12 meses ÷ headcount médio (início e fim da janela)
  filtro: datas de admissão e desligamento do cadastro
  fonte: ness.brain, cadastro de colaboradores
  dono: RH
  secao: Pessoas e RH
- id: ferias_vencidas
  nome: Férias vencidas
  formula: saldo dos períodos aquisitivos cujo prazo concessivo já passou (pagos em dobro)
  filtro: só CLT com admissão no cadastro
  fonte: ness.brain, cadastro e pedidos de férias aprovados e em andamento
  dono: RH
  secao: Pessoas e RH
- id: ferias_a_vencer_90d
  nome: Férias que vencem em 90 dias
  formula: saldo dos períodos com prazo concessivo nos próximos 90 dias
  filtro: só CLT com admissão no cadastro
  fonte: ness.brain, cadastro e pedidos de férias
  dono: RH
  secao: Pessoas e RH
- id: ferias_agenda
  nome: Agenda de férias
  formula: pedidos de férias aprovados ou em aprovação cujo período cruza a janela consultada
  filtro: sem pedidos recusados ou cancelados
  fonte: ness.brain, pedidos de férias
  dono: RH
  secao: Pessoas e RH
- id: ciclo_pedidos
  nome: Ciclo dos pedidos
  formula: média de dias entre a abertura e a conclusão dos pedidos concluídos nos últimos 90 dias
  filtro: sem cancelados; com os em andamento contados à parte
  fonte: ness.brain, pedidos dos processos
  dono: RH
  secao: Pessoas e RH
- id: reembolsos_mes
  nome: Reembolsos pagos no mês
  formula: soma dos reembolsos de despesa aprovados pelo Financeiro no mês corrente, por área (departamento do Omie escolhido no pedido)
  filtro: só pedidos aprovados (pagos), pela data de conclusão no horário de Brasília; os em andamento aparecem à parte como "a pagar"
  fonte: ness.brain, pedidos de reembolso
  dono: Financeiro
  secao: Pessoas e RH
- id: funil_ponderado
  nome: Funil ponderado
  formula: soma, nas oportunidades abertas, do valor total (mensal × prazo, 12 meses se não informado, + único) × a probabilidade da etapa (ou a informada na oportunidade)
  filtro: etapas prospecção, qualificação, proposta e negociação
  fonte: ness.brain, oportunidades do Comercial; probabilidades em 06-processos/politica-comercial.md
  dono: Comercial
  secao: Comercial
- id: receita_prevista
  nome: Receita mensal nova prevista
  formula: soma do valor mensal × probabilidade nas oportunidades abertas; no gráfico, por mês da previsão de fechamento (próximos 6 meses)
  filtro: oportunidades abertas
  fonte: ness.brain, oportunidades do Comercial
  dono: Comercial
  secao: Comercial
- id: taxa_conversao
  nome: Conversão em 12 meses
  formula: ganhas ÷ (ganhas + perdidas), pela data de fechamento nos últimos 12 meses
  filtro: só oportunidades fechadas
  fonte: ness.brain, oportunidades do Comercial
  dono: Comercial
  secao: Comercial
- id: ticket_medio
  nome: Ticket médio
  formula: valor total médio das oportunidades ganhas nos últimos 12 meses
  filtro: ganhas
  fonte: ness.brain, oportunidades do Comercial
  dono: Comercial
  secao: Comercial
- id: ciclo_venda
  nome: Ciclo de venda
  formula: média de dias da criação da oportunidade até o ganho, nos últimos 12 meses
  filtro: ganhas
  fonte: ness.brain, oportunidades do Comercial
  dono: Comercial
  secao: Comercial
- id: funil_atencao
  nome: Oportunidades que pedem atenção
  formula: abertas com previsão de fechamento vencida + abertas sem previsão
  filtro: oportunidades abertas
  fonte: ness.brain, oportunidades do Comercial
  dono: Comercial
  secao: Comercial

- id: sgsi_cobertura
  nome: Cobertura de evidência do SGSI
  formula: controles aplicáveis com ao menos uma evidência válida hoje ÷ controles aplicáveis
  filtro: controles do Anexo A em planejado, em implementação ou implementado (não avaliado e não aplicável ficam de fora); evidência válida = data de referência até hoje e validade não vencida
  fonte: ness.brain, SoA e evidências da Governança
  dono: Dono do SGSI
  secao: Governança

- id: sgsi_implementados
  nome: Controles implementados
  formula: controles aplicáveis no estado implementado, sobre os aplicáveis
  filtro: Declaração de Aplicabilidade (SoA) vigente
  fonte: ness.brain, SoA da Governança
  dono: Dono do SGSI
  secao: Governança

- id: sgsi_evidencias_vencendo
  nome: Evidências vencendo
  formula: controles aplicáveis cujas evidências válidas vencem todas nos próximos 30 dias
  filtro: evidências com validade preenchida
  fonte: ness.brain, evidências da Governança
  dono: Dono do SGSI
  secao: Governança

- id: sgsi_revisoes_atrasadas
  nome: Revisões de controle atrasadas
  formula: controles aplicáveis cuja última revisão + periodicidade já passou
  filtro: controles com ao menos uma revisão registrada
  fonte: ness.brain, SoA da Governança
  dono: Dono do SGSI
  secao: Governança

- id: sgsi_excecoes_ativas
  nome: Exceções ativas
  formula: pedidos de exceção aprovados com validade a partir de hoje
  filtro: processo Exceção a controle (EXC)
  fonte: ness.brain, pedidos de exceção
  dono: Dono do SGSI
  secao: Governança

- id: sgsi_incidentes_abertos
  nome: Incidentes em aberto
  formula: pedidos de incidente em andamento (ainda não encerrados pelo dono do SGSI)
  filtro: processo Incidente de segurança (INC)
  fonte: ness.brain, pedidos de incidente
  dono: Dono do SGSI
  secao: Governança
```
