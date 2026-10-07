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
