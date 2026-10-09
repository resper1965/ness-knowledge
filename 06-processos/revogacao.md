---
tipo: processo
titulo: Revogação e revisão de acessos
responsavel: TI
status: ativo
versao: 1.0
ultima_revisao: 2026-10-09
---

# Revogação e revisão de acessos

Tirar um acesso: a pessoa, o gestor ou o RH pedem, e o TI retira e confirma. O pedido também abre sozinho em dois casos:
no desligamento e na revisão periódica de acessos.

```processo
id: revogacao
nome: Revogação de acesso
modulo: rh
prefixo: REV
abre: todos
efeito: revogacao
calculadora: revogacao
descricao: Você pede para tirar um acesso (seu ou de alguém da sua equipe); o TI retira e confirma. No desligamento e na revisão de acessos, o pedido abre sozinho.
campos:
  - chave: pessoa
    nome: De quem
    tipo: pessoa
    obrigatorio: true
    ajuda: você ou alguém da sua equipe
  - chave: sistema
    nome: Sistema
    tipo: sistema
    obrigatorio: true
    ajuda: ou todos os acessos
  - chave: perfil
    nome: Perfil
    tipo: texto
    obrigatorio: false
    ajuda: em branco = todos os perfis do sistema
  - chave: motivo
    nome: Motivo
    tipo: texto
    obrigatorio: true
    ajuda: fica na trilha
etapas:
  - id: ti_executa
    nome: TI retira o acesso e confirma
    quem: ti
    acao: validar
```

## Pré-checagem (calculadora `revogacao`)

- Mostra o que vai sair:
  - os acessos do registro que correspondem ao sistema e ao perfil (perfil em branco = todos do sistema);
  - no ness.brain, as permissões dadas direto na pessoa.
- Avisa quando nada no registro corresponde. Nesse caso o TI confere no sistema e confirma.
- Permissões que vêm de grupo saem tirando a pessoa do grupo, em Administração › Pessoas.

## Quando abre sozinho

- **Desligamento aprovado:** abre a revogação de **todos os acessos** da pessoa, em nome de quem pediu o desligamento,
  para o TI executar. O acesso ao próprio ness.brain já termina no dia seguinte ao último dia, pelo desligamento.
- **Revisão de acessos:** quando o gestor marca "tirar" num acesso da equipe, abre a revogação daquele acesso.
- **No ness.brain**, a revogação aprovada tira a permissão sozinha, sem a etapa do TI.

## Revisão periódica (ISO 27001, controle 5.18)

- **Primeira revisão:** é aberta pelo TI, na tela Pessoas e RH › Acessos.
- **Revisões seguintes:** abrem sozinhas a cada **90 dias**.
- **Quem revisa:** o gestor direto revisa os acessos da equipe direta. Os acessos de quem não tem gestor ficam com o
  TI. Cada um recebe um e-mail e tem **30 dias** para responder.
- **O que se revisa:** cada acesso do registro e o resumo do que a pessoa tem no ness.brain.
- **A resposta** é "manter" ou "tirar". "Tirar" abre a revogação.
- **Registro:** fica guardado (quem decidiu, quando e o pedido aberto) e não se altera.

## Registro de acessos que já existiam

O inventário inicial é registrado pelo TI na mesma tela, em "Registrar acesso que já existe". Esses acessos entram na
revisão como os demais.
