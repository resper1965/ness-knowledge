---
titulo: Racionais do ness.brain
responsavel: Diretoria
status: ativo
versao: 0.2
ultima_revisao: 2026-10-08
---

# Racionais do ness.brain

Como cada número do ness.brain é calculado: fórmula, fonte, premissas e as decisões que o sustentam. Este arquivo é a
referência do painel ("Como é calculado"), do chat e do MCP (ferramenta `racionais`). Se um número mudar de regra, a
mudança entra aqui antes, por PR aprovado.

## Princípios gerais

- **Fonte:** o Omie da ness., só leitura. A n.secops é uma conta corrente dentro dele.
- **Período:** os dashboards de custo usam os 12 meses completos (`janela_meses`) anteriores ao mês atual, pelo mês de emissão do
  título. É uma premissa gerencial, não o regime de competência da contabilidade.
- **Títulos cancelados** ficam fora de tudo.
- **Classificação:** departamento, projeto e categoria são lidos pelas regras do Omie (`05-operacao/regras-omie/`). O que
  não tem regra fica fora dos totais e aparece como aviso; o sistema não presume.
- **Cada número tem fonte:** a calculadora devolve a consulta ao Omie que o gerou.
- **Parâmetros:** os limiares citados abaixo (entre parênteses, a chave) ficam em `05-operacao/parametros.md`. Mudar
  um deles é um PR aprovado pelo board (decisão do CEO de 08/10/2026); o ness.brain aplica sem deploy.

## Saldo e caixa de 13 semanas

- **Saldo em caixa:** saldo das contas correntes no fim do dia anterior à data-base, pelo resumo financeiro do Omie.
- **Projeção:** para cada uma das 13 semanas (`caixa_semanas`), soma dos títulos em aberto a receber menos os a pagar com vencimento na
  semana, acumulada a partir do saldo.
- **Vencidos e não pagos** não entram na projeção; aparecem à parte.
- **Cobertura de caixa:** número de semanas até a primeira semana com saldo projetado negativo.

## Caixa estimado (linha tracejada)

Entradas que o Omie ainda não tem lançadas, rotuladas como premissa.

- **Cliente recorrente:** teve títulos a receber com vencimento em pelo menos 5 (`recorrente_minimo`) dos 6
  (`recorrente_meses`) meses completos anteriores.
- **Valor mensal:** mediana dos totais mensais do cliente. **Dia:** mediana do dia de vencimento.
- **Estima-se só a diferença** entre o valor esperado e o que já está lançado para o cliente no mês.
- **Com contrato aprovado,** vale o contrato (mensalidade no dia de faturamento mais o prazo, dentro da vigência) e o
  cliente sai da estimativa pelo histórico. O contrato é ligado ao cliente pelo CNPJ.

## Aging de recebíveis

Títulos a receber em aberto, por faixa de atraso a partir do vencimento: a vencer, 1 a 30, 31 a 60, 61 a 90 e mais de
90 dias. Vencido percentual = vencidos sobre o total a receber.

## Faturamento do mês e ritmo

- **Faturamento do mês:** títulos a receber emitidos do dia 1 até hoje.
- **Ritmo:** comparação com o mesmo número de dias do mês anterior.
- **Maior cliente no mês:** participação do maior cliente no faturamento do mês.

## Resultado por área

- **Receita da área:** títulos a receber dos departamentos de classe área produtiva.
- **Custo direto da área:** títulos a pagar desses departamentos, inclusive a remuneração de sócios que entregam na área
  e os rateios por regra.
- **Margem da área:** (receita − custo direto) ÷ receita. Não inclui o overhead.
- **Grupos:** NO CODE (custo) e nPrivacy (receita) formam o grupo nPrivacy.
- **Nome da área:** o grupo, se houver; senão o nome dado na tela Dados do Omie (`rotulo`); senão o nome do departamento
  no Omie. Departamentos com o mesmo nome somam na mesma área.
- **Resultado operacional:** receita operacional − custo das áreas − backoffice − diretoria − imposto sobre faturamento −
  Inovação e P&D.
- **Resultado financeiro:** receitas financeiras − despesas financeiras, fora do operacional.
- **Custo acima da receita:** a área recebe um aviso com três saídas: quanto a remuneração de sócios explica, apontar horas
  e o faturamento necessário.

## Estrutura de custos

Despesas por classe do departamento: áreas produtivas, backoffice, diretoria e gestão, imposto sobre faturamento,
Inovação e P&D, empréstimos e financiamentos, receitas e despesas financeiras. Transferências entre contas não são
despesa. O percentual é sobre a soma das despesas com regra.

## Overhead

- **Overhead corporativo:** despesas de backoffice + diretoria e gestão.
- **Sobre o custo direto:** overhead ÷ custo das áreas produtivas. É a taxa usada no preço.
- **Sobre a receita:** overhead ÷ receita operacional.
- **Imposto sobre faturamento:** imposto ÷ receita operacional.
- **Rateio por área:** proporcional ao custo de cada área (opção: à receita).
- **Ficam fora do overhead:** Inovação e P&D, financiamento, itens financeiros e transferências.
- **Só cerca de 8% do custo das áreas está ligado a contrato (CPS)**; o resto está em agrupamentos (salários, licenças,
  PJ, sócios). Por isso o overhead é medido sobre o custo total das áreas, não sobre o custo de contratos.

## Remuneração de sócios (DL e PL)

- **É custo,** não distribuição de lucro (decisão do CEO de 07/10/2026).
- **Padrão:** vai para o overhead, exceto quando o sócio entrega numa área.
- **Sócios que entregam na área** (custo direto dela): AG em SecOps, MY em Trustness, TB em DEV.
- **Rateios:** RS 80% Forense e 20% Diretoria; RE 50% Inovação e P&D e 50% Diretoria.
- **Opções de visão:** onde foi lançada, tudo no overhead (padrão) ou fora da apuração.
- **Valores somados,** sem abrir por pessoa nos dashboards.

## Calculadora de preço

Preço de referência = custo direto × (1 + overhead sobre o custo) ÷ (1 − imposto sobre faturamento − margem alvo).
É ponto de partida, não proposta: não considera risco, prazo de pagamento nem a política comercial.

## Metas por área

Três alavancas, cada uma isolada, para a área chegar à margem alvo (padrão 20%, `margem_alvo`):

- **Faturamento:** receita necessária = (custo direto + overhead rateado) ÷ (1 − margem alvo).
- **Despesas diretas:** custo máximo = receita × (1 − margem alvo) − overhead rateado.
- **Overhead:** overhead máximo = receita × (1 − margem alvo) − custo direto.

As alavancas não se somam. Quando o limite fica negativo, aquela alavanca sozinha não basta.

## Board: ofensores de caixa

- **Maiores ofensores:** categorias com maior total a pagar nos 12 meses, sem sócios, sem financiamento e sem
  transferências.
- **Fora do padrão:** pico mensal acima de 2 vezes (`ofensor_multiplo_mediana`) a mediana mensal da categoria,
  contando como zero os meses sem lançamento, em categorias com pelo menos 6 títulos (`ofensor_min_titulos`).
- **Sócios por área:** remuneração de sócios tratada como mão de obra de cada área.
- **Orçamento:** quando o Previsto x Realizado do Omie estiver lido, o padrão passa a ser o orçamento; até lá, o histórico.

## Inovação e P&D

Área criada pelo CEO em 08/10/2026. Classe própria: o custo entra no resultado operacional e fica fora do overhead
rateado e da margem das áreas produtivas. Hoje recebe 50% da remuneração de RE.

## Contratos e previsão por contrato

Só valem depois da validação do CEO. Entra na previsão o contrato com CNPJ e mensalidade; o dia de faturamento não é
presumido; o reajuste é sinalizado, não aplicado.

## Lista de clientes

Clientes são as contrapartes com título a receber emitido nos últimos 12 meses, pelo nome do cadastro do Omie. Só os
nomes, sem valores.

## Pessoas e RH

- **Base:** o cadastro de colaboradores (carga pela receita `colaboradores/v1`, depois mantido pelos processos) e os
  pedidos de férias (`06-processos/ferias.md`). É uma fotografia do dia, não histórico.
- **Pessoas ativas:** sem data de desligamento ou com desligamento futuro, por empresa, vínculo e área.
- **Turnover de 12 meses:** desligamentos na janela ÷ média entre o headcount do início e o de hoje.
- **Férias CLT:**
  - o saldo por período aquisitivo segue a pré-checagem do pedido, contando aprovadas e em andamento;
  - **vencidas** são o saldo fora do prazo concessivo, pagas em dobro;
  - **vencem em 90 dias** é o saldo de períodos com prazo nos próximos 90 dias.
  - PJ fica fora desses números: segue o descanso do contrato.
- **Agenda:** férias aprovadas ou em aprovação que cruzam a janela.
- **Ciclo dos pedidos:** média de dias da abertura à conclusão nos últimos 90 dias, sem cancelados.

## Acesso

- **Board** (administrador, dajzen, rsalerno, myoshida, balencar, agsilva, tbertuzzi): vê tudo.
- **Demais usuários:** não veem sócios e financiamento, overhead, metas nem o Board.
- **Pessoas e papéis** só em Configuração → Usuários (fonte única, decisão do CEO de 08/10/2026): administrador,
  board, operador e validador de ingestão, aprovador de conhecimento e destinatário do briefing. Fora do cadastro só
  existe o administrador de emergência, no ambiente.
