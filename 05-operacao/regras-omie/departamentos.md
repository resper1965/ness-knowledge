# Departamentos

Fonte do método: "Orientação base de classificação OMIE — perfil NESS" (Rogério Salerno, financeiro). Departamento diz
**onde** o gasto acontece. Os nomes são os do Omie (com o prefixo `D_`), escritos como a equipe os vê.

### R-departamentos-0001 · Áreas produtivas
```regra
tema: departamentos
quando:
  departamento: [D_Infraestrutura, D_Forense, D_SecOps, D_DEV, D_ERP, D_Lgpd, D_NO CODE, D_nPrivacy, D_Trustness]
entao:
  classe: area_produtiva
desde: 2026-10-07
fonte: Orientação base de classificação OMIE — perfil NESS (financeiro); decisão do CEO de 07/10/2026
```
Motivo: o custo das áreas que entregam serviço é custo direto; sua ligação com um contrato depende do projeto (ver
`projetos.md`).

### R-departamentos-0002 · Backoffice
```regra
tema: departamentos
quando:
  departamento: D_Backoffice
entao:
  classe: backoffice
desde: 2026-10-07
fonte: Orientação base de classificação OMIE — perfil NESS (financeiro); decisão do CEO de 07/10/2026
```
Motivo: custo indireto, de apoio. Não se atribui a contrato sem critério de rateio.

### R-departamentos-0003 · Diretoria e gestão
```regra
tema: departamentos
quando:
  departamento: D_Diretoria/Gestão
entao:
  classe: diretoria
desde: 2026-10-07
fonte: Orientação base de classificação OMIE — perfil NESS (financeiro); decisão do CEO de 07/10/2026
```
Motivo: custo de gestão. Remuneração dos sócios (DL, PL) também cai aqui; a natureza vem da categoria.

### R-departamentos-0004 · Imposto sobre faturamento
```regra
tema: departamentos
quando:
  departamento: D_Imposto Faturamento
entao:
  classe: imposto_faturamento
desde: 2026-10-07
fonte: Orientação base de classificação OMIE — perfil NESS (financeiro); decisão do CEO de 07/10/2026
```
Motivo: agrupamento específico de tributos que incidem sobre a receita.

### R-departamentos-0005 · Empréstimos e financiamentos
```regra
tema: departamentos
quando:
  departamento: D_Emprestimos_Financiamentos
entao:
  classe: financiamento
desde: 2026-10-07
fonte: Orientação base de classificação OMIE — perfil NESS (financeiro); decisão do CEO de 07/10/2026
```
Motivo: financiamento interno e de sócios (projetos RS, GIRO e GIRO_NESS). A LAW é uma subdivisão da ness. e a
dívida dela foi incorporada; por isso o GIRO entra aqui, sem separar por empresa.

### R-departamentos-0006 · Receitas e despesas financeiras
```regra
tema: departamentos
quando:
  departamento: D_Receitas e Despesas Financeiras
entao:
  classe: financeira
desde: 2026-10-07
fonte: Orientação base de classificação OMIE — perfil NESS (financeiro); decisão do CEO de 07/10/2026
```
Motivo: juros, tarifas e rendimentos; resultado financeiro, fora do custo operacional.
