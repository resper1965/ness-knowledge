# Projetos

Projeto diz **a que o gasto se liga**: um contrato, uma proposta ou um agrupamento. `CPS` é contrato de prestação de
serviços; `PPS` é proposta de prestação de serviços (decisão do CEO, 07/10/2026).

### R-projetos-0001 · Contrato (CPS)
```regra
tema: projetos
quando:
  projeto_prefixo: "CPS-"
entao:
  vinculo: contrato
  entra_custo_cliente: true
desde: 2026-10-07
fonte: Orientação base de classificação OMIE — perfil NESS (financeiro); CEO, 07/10/2026
```
Motivo: o lançamento com projeto CPS pertence ao contrato daquele número; entra no custo do cliente.

### R-projetos-0002 · Proposta (PPS)
```regra
tema: projetos
quando:
  projeto_prefixo: "PPS-"
entao:
  vinculo: proposta
  entra_custo_cliente: false
desde: 2026-10-07
fonte: CEO, 07/10/2026
```
Motivo: proposta ainda não é contrato; o custo fica registrado, mas não é atribuído a cliente até virar CPS.

### R-projetos-0003 · Geral da área
```regra
tema: projetos
quando:
  projeto_prefixo: "Geral_"
entao:
  vinculo: geral_da_area
  entra_custo_cliente: false
desde: 2026-10-07
fonte: Orientação base de classificação OMIE — perfil NESS (financeiro); CEO, 07/10/2026
```
Motivo: o custo permanece no departamento, mas não se atribui a contratos sem um critério de rateio documentado.

### R-projetos-0004 · Empréstimos de sócios, GIRO e GIRO_NESS
```regra
tema: projetos
quando:
  projeto: [Empréstimos Sócios, RS, GIRO, GIRO_NESS]
entao:
  vinculo: compartilhado
  classe: financiamento
desde: 2026-10-07
fonte: e-mail do financeiro de 07/10/2026; CEO, 07/10/2026
```
Motivo: RS é empréstimo de sócios; GIRO (cerca de R$ 34,4 mil por mês, 28 mil ness. e 5,8 mil LAW) é capital de giro.
A LAW é subdivisão da ness. e a dívida foi incorporada: não se separa por empresa.

### R-projetos-0005 · Agrupamentos de custo
```regra
tema: projetos
quando:
  projeto: [Edifício_LWM, Despesas Financeiras]
entao:
  vinculo: compartilhado
desde: 2026-10-07
fonte: Orientação base de classificação OMIE — perfil NESS (financeiro)
```
Motivo: agrupamentos que não se ligam a contrato. Os demais agrupamentos (Salários e Encargos, Benefícios, Licenças e
Assinaturas, Impostos) seguem o mesmo critério e entram aqui quando o cadastro trouxer o nome exato.
