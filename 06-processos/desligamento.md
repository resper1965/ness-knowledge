---
tipo: processo
titulo: Desligamento
responsavel: RH
status: ativo
versao: 1.0
ultima_revisao: 2026-10-09
---

# Desligamento

Registrar a saída de alguém da equipe: o RH confere aviso prévio e férias a pagar, os heads aprovam e o gestor da pessoa é informado. Aprovado, a saída fica no cadastro e o acesso ao ness.brain termina no dia seguinte ao último dia.

```processo
id: desligamento
nome: Desligamento
modulo: rh
prefixo: DES
abre: gestores
efeito: desligamento
calculadora: desligamento
descricao: O gestor (ou o RH) registra; o RH confere aviso prévio e férias a pagar; os heads aprovam. Aprovado, a pessoa sai do cadastro na data e perde o acesso.
campos:
  - chave: pessoa
    nome: Pessoa
    tipo: pessoa
    obrigatorio: true
    ajuda: ""
  - chave: data
    nome: Último dia
    tipo: data
    obrigatorio: true
    ajuda: ""
  - chave: tipo
    nome: Tipo
    tipo: lista
    obrigatorio: true
    ajuda: ""
    opcoes:
      - pedido de demissão
      - sem justa causa
      - justa causa
      - acordo (art. 484-A)
      - fim de contrato
      - fim do contrato PJ
  - chave: aviso
    nome: Aviso prévio
    tipo: lista
    obrigatorio: false
    ajuda: CLT
    opcoes:
      - trabalhado
      - indenizado
      - dispensado
    so_clt: true
  - chave: motivo
    nome: Motivo
    tipo: texto
    obrigatorio: true
    ajuda: fica na trilha
etapas:
  - id: rh_valida
    nome: RH confere aviso prévio e férias
    quem: rh
    acao: validar
  - id: heads_aprovam
    nome: Heads aprovam
    quem: heads
    acao: aprovar
  - id: gestor_informado
    nome: Gestor da pessoa é informado
    quem: gestor_da_pessoa
    acao: informar
```

## Pré-checagem (calculadora `desligamento`)

- **Impedimento:**
  - a pessoa já saiu;
  - o último dia é anterior à admissão;
  - pessoa CLT com o tipo "fim do contrato PJ".
- **Aviso prévio (CLT, Lei 12.506):**
  - **Sem justa causa:** 30 dias mais 3 por ano completo, até 90.
  - **Acordo (art. 484-A):** metade do aviso, se indenizado, e multa do FGTS de 20%.
  - **Pedido de demissão:** 30 dias de quem pede.
  - **Justa causa:** sem aviso nem férias proporcionais. O motivo precisa estar documentado.
- **Férias a pagar (CLT):**
  - vencidas, pagas em dobro;
  - de período completo;
  - proporcionais: 1/12 por mês do período em curso (fração de 15 dias ou mais conta como mês), 2,5 dias cada, mais 1/3.
  - Férias marcadas depois do último dia geram alerta, para cancelar os pedidos.
- **PJ:** aviso e multa seguem o contrato.
- **Depois da aprovação:**
  - o cadastro guarda a data de saída;
  - no dia seguinte ao último dia, o acesso ao ness.brain termina: o usuário fica inativo e sai dos grupos. Administradores
    não perdem o acesso de forma automática.
- **Fora do ness.brain:** o cálculo dos valores da rescisão e o eSocial ficam com a contabilidade.
