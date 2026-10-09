---
titulo: Admissão
responsavel: RH
status: ativo
versao: 1.0
ultima_revisao: 2026-10-09
---

# Admissão

Admitir uma pessoa: quem vai gerir pede, a diretoria aprova a contratação e o RH confere os dados. Aprovada, a pessoa entra no cadastro de colaboradores e ganha acesso ao ness.brain, sem papel.

```processo
id: admissao
nome: Admissão
modulo: rh
prefixo: ADM
abre: gestores
efeito: admissao
calculadora: admissao
descricao: Quem vai gerir a pessoa pede; a diretoria aprova a contratação; o RH confere os dados. Aprovada, a pessoa entra no cadastro e ganha acesso ao ness.brain.
campos:
  - chave: nome
    nome: Nome completo
    tipo: texto
    obrigatorio: true
    ajuda: ""
  - chave: email
    nome: E-mail @ness.com.br
    tipo: email
    obrigatorio: true
    ajuda: o e-mail que a pessoa vai usar
  - chave: inicio
    nome: Data de início
    tipo: data
    obrigatorio: true
    ajuda: vira a data de admissão
  - chave: empresa
    nome: Empresa
    tipo: lista
    obrigatorio: true
    ajuda: ""
    opcoes:
      - ness.
      - n.secops
  - chave: vinculo
    nome: Vínculo
    tipo: lista
    obrigatorio: true
    ajuda: ""
    opcoes:
      - clt
      - pj
  - chave: cargo
    nome: Cargo
    tipo: texto
    obrigatorio: true
    ajuda: ""
  - chave: area
    nome: Área
    tipo: texto
    obrigatorio: true
    ajuda: "departamento, como no Omie (ex.: D_SecOps)"
  - chave: gestor
    nome: Gestor direto
    tipo: pessoa
    obrigatorio: true
    ajuda: ""
  - chave: dias_descanso_ano
    nome: Dias de descanso no ano (PJ)
    tipo: inteiro
    obrigatorio: false
    ajuda: só para PJ, como no contrato
    min: 0
    max: 60
  - chave: observacao
    nome: Observação
    tipo: texto
    obrigatorio: false
    ajuda: opcional
etapas:
  - id: diretoria_aprova
    nome: Diretoria aprova a contratação
    quem: diretoria
    acao: aprovar
  - id: rh_valida
    nome: RH confere os dados e prepara a admissão
    quem: rh
    acao: validar
```

## Pré-checagem (calculadora `admissao`)

- **Impedimento:**
  - o e-mail já está ativo no cadastro;
  - o gestor escolhido não está ativo.
- **Alerta:**
  - a data de início já passou;
  - PJ sem dias de descanso no ano.
- **Informação:**
  - o e-mail de quem já saiu é tratado como recontratação;
  - para CLT, o lembrete de exame admissional, carteira e eSocial, com a contabilidade;
  - o fim do contrato de experiência de 45 dias.
