---
tipo: processo
titulo: Alteração de cadastro
responsavel: RH
status: ativo
versao: 1.0
ultima_revisao: 2026-10-09
---

# Alteração de cadastro

Mudar cargo, área, gestor, empresa ou vínculo: o gestor da pessoa aprova e o RH confere. Aprovada, o cadastro e o organograma mudam sozinhos.

```processo
id: alteracao
nome: Alteração de cadastro
modulo: rh
prefixo: ALT
abre: gestores
efeito: alteracao
calculadora: alteracao
descricao: Mudança de cargo, área, gestor, empresa ou vínculo. O gestor da pessoa aprova e o RH confere; aprovada, o cadastro e o organograma mudam sozinhos.
campos:
  - chave: pessoa
    nome: Pessoa
    tipo: pessoa
    obrigatorio: true
    ajuda: ""
  - chave: cargo
    nome: Novo cargo
    tipo: texto
    obrigatorio: false
    ajuda: "vazio: não muda"
  - chave: area
    nome: Nova área
    tipo: texto
    obrigatorio: false
    ajuda: "vazio: não muda"
  - chave: gestor
    nome: Novo gestor
    tipo: pessoa
    obrigatorio: false
    ajuda: "vazio: não muda"
  - chave: empresa
    nome: Nova empresa
    tipo: lista
    obrigatorio: false
    ajuda: "vazio: não muda"
    opcoes:
      - ness.
      - n.secops
  - chave: vinculo
    nome: Novo vínculo
    tipo: lista
    obrigatorio: false
    ajuda: "vazio: não muda"
    opcoes:
      - clt
      - pj
  - chave: dias_descanso_ano
    nome: Dias de descanso no ano (PJ)
    tipo: inteiro
    obrigatorio: false
    ajuda: "vazio: não muda"
    min: 0
    max: 60
  - chave: a_partir
    nome: A partir de
    tipo: data
    obrigatorio: true
    ajuda: ""
  - chave: motivo
    nome: Motivo
    tipo: texto
    obrigatorio: true
    ajuda: fica na trilha
etapas:
  - id: gestor_aprova
    nome: Gestor da pessoa aprova
    quem: gestor_da_pessoa
    acao: aprovar
  - id: rh_valida
    nome: RH confere
    quem: rh
    acao: validar
```

## Pré-checagem (calculadora `alteracao`)

- **Impedimento:**
  - nada muda (todos os campos vazios ou iguais ao atual);
  - o novo gestor não está ativo ou é a própria pessoa;
  - o novo gestor cria um ciclo no organograma (alguém da equipe da pessoa).
- **Alerta:** troca de CLT para PJ, ou o contrário. Exige desligamento ou admissão formal com a contabilidade.
- **Na aprovação:** o cadastro muda só nos campos alterados, e a trilha guarda o "de → para". A data "a partir de"
  fica registrada; o cadastro muda na aprovação.
