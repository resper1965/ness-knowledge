# Regras do Omie

As regras que adaptam a estrutura do Omie à visão de custo e de caixa da ness.. Até 05/10/2026 não havia regra escrita:
cada uma dependia do tema e de quem lançava. A partir daqui, toda interpretação dos dados do Omie usada pelo ness.brain
(custo por cliente, DRE gerencial, caixa, unidades) vem destes arquivos. O que não tem regra aparece como **sem regra**
no ness.brain, nunca como uma suposição.

- **Fonte da verdade**: este diretório no `ness-knowledge` (GitHub). **Cópia**: o D1 do ness.brain, sincronizada a
  partir do `main`, que alimenta a tela Administração › Dados do Omie e as calculadoras. Decisão do CEO de 05/10/2026.
- **Como muda**: o agente de configuração do ness.brain conversa por tema, consulta o cadastro do Omie e propõe regras;
  o CEO aprova na fila de Conhecimento; a aprovação vira PR aqui. A tela Administração › Dados do Omie também propõe
  regras de um item só (ver "Regras geradas pela tela" abaixo). Também é possível editar por PR direto.
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
- `entao` também aceita `rotulo` (como a ness. chama o item; dois departamentos com o mesmo rótulo somam na mesma área
  do resultado) e `no_caixa` (a conta corrente soma no saldo de caixa).
- Quando duas regras valem para o mesmo lançamento, vale a mais específica: cada condição soma um peso. Condição exata de
  um valor só pesa 2; lista de valores (`departamento: [A, B]`) pesa 1,5; prefixo ou trecho (`*_prefixo`,
  `descricao_contem`) pesa 1. Assim a regra de um item nomeado vence a regra escrita para uma lista, que vence a do
  prefixo. Empate é erro e aparece na tela para ser resolvido.

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

Remuneração de sócios (DL e PL): `alocacao: area` mantém no custo direto da área onde foi lançada (o sócio entrega nela);
`rateio_departamentos` divide o lançamento entre departamentos (ex.: 80% Forense, 20% Diretoria). Sem nenhum dos dois, vai
para o overhead.

## Regras geradas pela tela (Dados do Omie)

A tela Administração › Dados do Omie mostra cada item do Omie (departamento, projeto, categoria, conta corrente) com o
nome que a ness. usa e os campos de `campos.yaml`. Ela não altera o Omie nem aplica nada na hora: as mudanças ficam num
rascunho e, ao enviar, viram um PR neste repositório, que só vale depois de aprovado e mesclado.

- A tela escreve só **regras de um item**, no fim do arquivo do tema, abaixo do marcador
  `<!-- regras geradas pela tela Administração › Dados do Omie; não editar à mão abaixo desta linha -->`. Os ids
  começam em `0101` e a `fonte` é `painel · <e-mail> · <data> · OM-nnnn` (o número do envio).
- A regra gerada traz todos os efeitos do item: começa dos valores da regra que vale hoje e troca só o que mudou na tela.
- Item com regra escrita à mão para ele só: a regra gerada leva `substitui: <id>` (as duas pesariam o mesmo). Remover a
  regra da tela devolve a regra à mão.
- Item coberto por uma lista ou por um prefixo: a regra gerada vence pelo peso; a lista continua valendo para os outros.
- Item com regra à mão que usa outra condição, rateio, `ignorar` ou `desde` no futuro aparece na tela só para leitura,
  com o link para a regra. Para mudar, edite a regra aqui.
- Contas correntes casam pelo código do Omie (`conta_corrente`), no tema `unidades`; as demais dimensões, pelo nome.
- Os campos e as opções da tela moram em `campos.yaml`. Mudar um campo também é um PR.
