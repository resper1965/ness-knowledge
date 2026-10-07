# Regras do Omie

As regras que adaptam a estrutura do Omie à visão de custo e de caixa da ness.. Até 05/10/2026 não havia regra escrita:
cada uma dependia do tema e de quem lançava. A partir daqui, toda interpretação dos dados do Omie usada pelo ness.brain
(custo por cliente, DRE gerencial, caixa, unidades) vem destes arquivos. O que não tem regra aparece como **sem regra**
no ness.brain, nunca como uma suposição.

- **Fonte da verdade**: este diretório no `ness-knowledge` (GitHub). **Cópia**: o D1 do ness.brain, sincronizada a
  partir do `main`, que alimenta a tela Configuração → Regras do Omie e as calculadoras. Decisão do CEO de 05/10/2026.
- **Como muda**: o agente de configuração do ness.brain conversa por tema, consulta o cadastro do Omie e propõe regras;
  o CEO aprova na fila de Conhecimento; a aprovação vira PR aqui. Também é possível editar por PR direto.
- **Uma regra nunca é apagada**: para mudar, escreva uma nova com `substitui: <id>` e um `desde` posterior.

## Temas (um arquivo por tema)

| Arquivo | Tema |
| --- | --- |
| `categorias.md` | Classificação das categorias do Omie: grupo de custo, natureza, se entra no DRE e no custo por cliente |
| `unidades.md` | Conta corrente ou departamento → unidade de negócio (ex.: n.secops) |
| `atribuicao.md` | Custo atribuído a cliente (por departamento, categoria ou fornecedor), com rateio |
| `excecoes.md` | Lançamentos que não são o que parecem (transferências entre contas, fatura de cartão, adiantamentos, reembolsos) |
| `lancamento.md` | Como o Omie é operado (despesas lançadas como agenda, entradas não lançadas) e o que o sistema faz com isso |

## Formato de uma regra

Um título com o id e um nome curto, um bloco ` ```regra ` (YAML) e o motivo em texto:

````markdown
### R-categorias-0001 · Licenças de software revendidas
```regra
tema: categorias
quando:
  categoria: "2.04.01"
entao:
  grupo: licencas
  natureza: custo_direto
  entra_dre: true
  entra_custo_cliente: true
desde: 2026-10-01
fonte: conversa com o financeiro em 05/10/2026
```
Motivo: licença comprada para revenda é custo do contrato do cliente, não despesa administrativa.
````

- `quando` (todas as condições juntas): `tipo` (pagar ou receber), `categoria`, `categoria_prefixo`, `fornecedor`
  (código do Omie), `departamento`, `conta_corrente`, `descricao_contem`.
- `entao`: `unidade`, `grupo`, `natureza` (`custo_direto`, `despesa_operacional`, `imposto`, `financeira`,
  `transferencia`, `investimento`, `nao_operacional`), `cliente`, `rateio` (lista de `{cliente, percentual}`),
  `entra_dre`, `entra_custo_cliente`, `ignorar`.
- `desde` (AAAA-MM-DD), `fonte` (quem decidiu ou o documento) e, se for o caso, `substitui`.
- Quando duas regras valem para o mesmo lançamento, vale a mais específica (mais condições); empate é erro e aparece
  na tela de regras para ser resolvido.

## Método das três dimensões (07/10/2026)
O Omie da ness. classifica cada lançamento por **três dimensões usadas juntas**, sempre escritas pelo nome:
- **Departamento** (`departamentos.md`): onde o gasto acontece. Classes: área produtiva, backoffice (indireto),
  diretoria, imposto sobre faturamento, financiamento, financeira.
- **Projeto** (`projetos.md`): a que o gasto se liga. `CPS-…` é contrato de prestação de serviços; `PPS-…` é proposta;
  `Geral_…` é custo da área que não se atribui a contrato sem critério de rateio; há ainda agrupamentos (empréstimos,
  edifício, despesas financeiras).
- **Categoria** (`categorias.md`): a natureza (pessoal, tributo, "Cliente - X" = linha de oferta, DL/PL = remuneração
  de sócios).

Em `quando`, valores são texto ou lista (qualquer um serve), sem diferenciar maiúsculas; há `projeto_prefixo` e
`categoria_prefixo`. Em `entao`, além dos efeitos antigos: `classe`, `vinculo` (contrato, proposta, geral_da_area,
compartilhado) e a natureza `remuneracao_socios`. A regra mais específica vence.

Fixos do CEO: LAW é subdivisão da ness. e a dívida foi incorporada; DL e PL são remuneração de sócios e entram no
custo (não são lucro distribuído).
