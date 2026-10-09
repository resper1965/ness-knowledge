---
tipo: processo
titulo: Reembolso de despesa
responsavel: Financeiro
status: ativo
versao: 1.0
ultima_revisao: 2026-10-09
---

# Reembolso de despesa

A pessoa lança uma despesa da empresa que pagou do próprio bolso e anexa o comprovante. O gestor aprova. O Financeiro
confere e paga fora do ness.brain e registra o pagamento no comentário da etapa (data e forma). Cada pedido corresponde a
uma despesa e a um comprovante.

```processo
id: reembolso
nome: Reembolso de despesa
modulo: rh
prefixo: REE
abre: todos
efeito: null
calculadora: reembolso
descricao: Você lança uma despesa paga do seu bolso com o comprovante; seu gestor aprova; o Financeiro confere e paga.
campos:
  - chave: data
    nome: Data da despesa
    tipo: data
    obrigatorio: true
    ajuda: ""
  - chave: categoria
    nome: Categoria
    tipo: lista
    obrigatorio: true
    ajuda: ""
    opcoes:
      - transporte
      - hospedagem
      - alimentação
      - quilometragem
      - material
      - cursos e eventos
      - outra
  - chave: valor
    nome: Valor
    tipo: valor
    obrigatorio: true
    ajuda: em reais, como 123,45
    min: 0
    max: 50000
  - chave: descricao
    nome: Descrição
    tipo: texto
    obrigatorio: true
    ajuda: o que foi e para quê (cliente, viagem, evento)
  - chave: area
    nome: Área
    tipo: omie
    fonte: departamentos
    obrigatorio: true
    ajuda: departamento do Omie que arca com a despesa
  - chave: projeto
    nome: Projeto
    tipo: omie
    fonte: projetos
    obrigatorio: false
    ajuda: se a despesa é de um projeto ou cliente
  - chave: comprovante
    nome: Comprovante
    tipo: arquivo
    obrigatorio: true
    ajuda: nota fiscal ou recibo (PDF ou imagem, até 10 MB)
etapas:
  - id: gestor_aprova
    nome: Gestor aprova
    quem: gestor_do_solicitante
    acao: aprovar
  - id: financeiro_paga
    nome: Financeiro confere e paga
    quem: financeiro
    acao: validar
```

## Pré-checagem (calculadora `reembolso`)

A pré-checagem não bloqueia o pedido. Quem decide são o gestor e o Financeiro.

- **Impedimentos:**
  - a data da despesa está no futuro;
  - o valor é zero;
  - falta o comprovante.
- **Alertas:**
  - a despesa é mais antiga que o prazo da política;
  - o valor passa do limite da categoria na política;
  - o comprovante já está em outro pedido;
  - já existe um reembolso da mesma pessoa com a mesma data, categoria e valor.
- **Informações:**
  - o total de reembolsos da pessoa no mês, contando o novo;
  - para PJ, o lembrete de que o reembolso segue o contrato.

## Regras

- **Área e projeto:** vêm do cadastro do Omie, que o ness.brain só lê. Servem para somar os reembolsos por área no
  painel RH (KPI `reembolsos_mes`). Nada é lançado no Omie pelo ness.brain: o Financeiro lança o pagamento no Omie, como
  hoje.
- **Quem paga:** quem tem o nível "decidir" em Finanças. Enquanto ninguém tiver, decidem os administradores.
- **Quem vê:** quem pediu, o gestor, o RH, o Financeiro e os administradores. O comprovante só abre para quem vê o
  pedido.
