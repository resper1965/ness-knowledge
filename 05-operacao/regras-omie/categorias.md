# Categorias

Categoria diz a **natureza** do gasto. Prefixos como `DL - ` e `PL - ` identificam remuneração de sócios.

### R-categorias-0001 · DL: remuneração de sócios dentro do custo
```regra
tema: categorias
quando:
  categoria_prefixo: "DL - "
entao:
  natureza: remuneracao_socios
  entra_dre: true
desde: 2026-10-07
fonte: CEO, 07/10/2026
```
Motivo: o "DL" (antecipação de distribuição de lucros) não distribui lucro apurado; é a forma de remunerar os sócios, e
em vários períodos não houve lucro. Por isso **entra no custo** e no resultado, não fica fora dele. Isso vale para o
lançamento no Omie; o tratamento fiscal da distribuição segue como a contabilidade o faz.

### R-categorias-0002 · PL: pró-labore
```regra
tema: categorias
quando:
  categoria_prefixo: "PL - "
entao:
  natureza: remuneracao_socios
  entra_dre: true
desde: 2026-10-07
fonte: CEO, 07/10/2026 (mesma lógica do DL)
```
Motivo: remuneração de sócios, também dentro do custo.

### R-categorias-0003 · Tributos
```regra
tema: categorias
quando:
  categoria: [IRPJ, CSLL, PIS, COFINS, ISS]
entao:
  natureza: imposto
desde: 2026-10-07
fonte: Orientação base de classificação OMIE — perfil NESS (financeiro)
```
Motivo: tributos; a classe do departamento diz se incidem sobre o faturamento.

### R-categorias-0004 · Cliente: linha de oferta
```regra
tema: categorias
quando:
  categoria_prefixo: "Cliente - "
entao:
  natureza: custo_direto
desde: 2026-10-07
fonte: e-mail do financeiro de 07/10/2026
```
Motivo: "Cliente - X" identifica a linha de oferta do contrato (receita e custo); o contrato vem do projeto.

## Remuneração de sócios que entrega na área

O DL e o PL vão para o overhead, exceto quando o sócio trabalha na entrega da área onde foi lançado: aí é custo direto
da área. A regra casa categoria e departamento juntos (mais específica que a regra geral do DL).

### R-categorias-0005 · AG entrega em SecOps
```regra
tema: categorias
quando:
  categoria: "DL - Antecipação Distribuição Lucros - AG"
  departamento: D_SecOps
entao:
  natureza: remuneracao_socios
  entra_dre: true
  alocacao: area
desde: 2026-10-07
fonte: CEO, 07/10/2026 ("AG, MY e TB trabalham na entrega das áreas")
```
Motivo: o sócio AG trabalha na entrega de SecOps; sua remuneração é custo direto da área, não overhead.

### R-categorias-0006 · MY entrega em Trustness
```regra
tema: categorias
quando:
  categoria: "DL - Antecipação Distribuição Lucros - MY"
  departamento: D_Trustness
entao:
  natureza: remuneracao_socios
  entra_dre: true
  alocacao: area
desde: 2026-10-07
fonte: CEO, 07/10/2026 ("AG, MY e TB trabalham na entrega das áreas")
```
Motivo: o sócio MY trabalha na entrega de Trustness; sua remuneração é custo direto da área.

### R-categorias-0007 · TB entrega em DEV
```regra
tema: categorias
quando:
  categoria: "DL - Antecipação Distribuição Lucros - TB"
  departamento: D_DEV
entao:
  natureza: remuneracao_socios
  entra_dre: true
  alocacao: area
desde: 2026-10-07
fonte: CEO, 07/10/2026 ("AG, MY e TB trabalham na entrega das áreas")
```
Motivo: o sócio TB trabalha na entrega de DEV; sua remuneração é custo direto da área.

### R-categorias-0008 · RS: 80% do tempo em Forense
```regra
tema: categorias
quando:
  categoria: ["DL - Antecipação Distribuição Lucros - RS", "PL - Pro Labore - RS"]
entao:
  natureza: remuneracao_socios
  entra_dre: true
  rateio_departamentos:
    - departamento: D_Forense
      percentual: 80
    - departamento: D_Diretoria/Gestão
      percentual: 20
desde: 2026-10-07
fonte: CEO, 07/10/2026 ("RS em forense 80% do tempo")
```
Motivo: o sócio RS dedica 80% do tempo à entrega de Forense e 20% à gestão. 80% da sua remuneração (DL e PL) é custo
direto de Forense; 20% fica na Diretoria, no overhead. Se a divisão mudar, uma regra nova com `desde` e `substitui`.
