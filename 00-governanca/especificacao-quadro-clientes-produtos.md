# Especificação do Quadro de Clientes Produtos Cases e Métricas

## Objetivo

Manter uma fonte atualizável que relacione clientes, contratos, ofertas, ambientes, métricas, cases e evidências. O quadro deve apoiar gestão, atualização de indicadores e produção de cases sem duplicar clientes ou publicar informação não autorizada.

## Estrutura

- `Painel`: visão consolidada e itens que exigem atualização.
- `Clientes`: uma linha por cliente ou organização.
- `Contratos`: uma linha por relação cliente e oferta.
- `Catálogo`: uma linha por marca, serviço, plataforma, produto ou módulo.
- `Métricas`: uma linha por cliente, ambiente, métrica e data de referência.
- `Cases`: uma linha por oportunidade editorial.
- `Evidências`: uma linha por fonte ou comprovação.
- `Listas`: valores padronizados usados nas validações.

## Chaves

- Cliente: `CLI-0001`.
- Contrato: `CTR-0001`.
- Oferta: `OFR-0001`.
- Métrica: `MET-0001`.
- Case: `CAS-0001`.
- Evidência: `EVD-0001`.

Os identificadores permanecem estáveis mesmo quando nomes, responsáveis ou estados mudam.

## Regras de agregação

- clientes contam apenas uma vez pelo `cliente_id`;
- ofertas contratadas contam por linha ativa em `Contratos`;
- métricas são agregadas somente quando `incluir_no_painel` for `Sim`;
- uma métrica exige unidade, data de referência e regra de agregação;
- `snapshot` soma somente registros da data ou corte selecionado;
- `estoque` representa a posição atual e não deve ser somado entre períodos;
- `fluxo` pode ser somado no período definido;
- valores desconhecidos permanecem vazios, nunca zero;
- dados de cases não se tornam públicos sem autorização registrada.

## Dados iniciais

Serão cadastrados os clientes nominados durante a descoberta: Alupar e Ionic Health. Os demais relatos permanecerão como cases anonimizados até que seus clientes sejam identificados ou autorizados. O catálogo será preenchido com as marcas, ofertas e produtos já consolidados no Documento Mestre.

## Critérios de conclusão

- todas as abas possuem cabeçalhos legíveis, filtros e linhas de entrada;
- campos categóricos possuem validação;
- o painel deriva das bases e não de números digitados diretamente;
- nenhuma fórmula exibe erro;
- todas as abas são visualmente verificadas;
- o workbook é salvo no pacote `ness-knowledge`.

