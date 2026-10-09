---
titulo: Aprovação de desconto
responsavel: Comercial
status: ativo
versao: 1.0
ultima_revisao: 2026-10-09
---

# Aprovação de desconto

Proposta abaixo do preço de referência precisa de aprovação antes de ir ao cliente. O pedido sai da oportunidade
(Comercial › Funil › oportunidade › Pedir aprovação de desconto). A decisão aparece em "Para eu decidir" de quem
aprova, e o resultado fica no histórico da oportunidade.

```processo
id: desconto
nome: Aprovação de desconto
modulo: comercial
prefixo: DSC
abre: comercial
efeito: null
calculadora: desconto
descricao: "Proposta abaixo do preço de referência: até o limite da política, o diretor comercial aprova; acima dele ou abaixo do custo, também os Heads."
campos:
  - chave: oportunidade
    nome: Oportunidade
    tipo: texto
    obrigatorio: true
    ajuda: o número, como OPO-0001
  - chave: valor_referencia
    nome: Preço de referência
    tipo: valor
    obrigatorio: true
    ajuda: o preço de tabela ou de referência, no mesmo período do proposto (mensal ou total)
    min: 0
    max: 100000000
  - chave: valor_proposto
    nome: Preço proposto
    tipo: valor
    obrigatorio: true
    ajuda: o que vai na proposta
    min: 0
    max: 100000000
  - chave: custo_estimado
    nome: Custo estimado
    tipo: valor
    obrigatorio: false
    ajuda: no mesmo período; ajuda a ver a margem
    min: 0
    max: 100000000
  - chave: justificativa
    nome: Justificativa
    tipo: texto
    obrigatorio: true
    ajuda: por que o desconto vale a pena
etapas:
  - id: diretor_aprova
    nome: Diretor comercial aprova
    quem: comercial
    acao: aprovar
  - id: heads_aprovam
    nome: Heads aprovam (acima do limite)
    quem: heads
    acao: aprovar
```

## Quem aprova

- **Até o limite da política** (`limite_desconto_diretor` em `politica-comercial.md`, hoje 10%): só o diretor
  comercial, que é quem tem "decidir" no Comercial. A etapa dos Heads é dispensada e isso fica registrado.
- **Acima do limite, ou abaixo do custo estimado:** o diretor comercial e, depois, os Heads.
- **Quando é o diretor comercial quem pede:** a etapa dele é dispensada e os Heads aprovam, já que ninguém decide o
  próprio pedido.
- **Quem pede:** quem opera o Comercial.

## Pré-checagem (calculadora `desconto`)

- **Impedimentos:**
  - a oportunidade não existe ou já está fechada (ganha ou perdida);
  - falta o preço de referência ou o preço proposto.
- **Alertas:**
  - o preço proposto não é menor que a referência;
  - o preço proposto fica abaixo do custo estimado.
- **Informações:**
  - o desconto em %;
  - quem vai aprovar;
  - a margem sobre o custo, quando o custo foi informado.
- O preço de referência e o proposto são do mesmo período (os dois mensais ou os dois totais). O "Metas e preço" de
  Finanças dá o preço de referência pelo custo e pela margem alvo.
