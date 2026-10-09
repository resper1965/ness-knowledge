---
tipo: processo
titulo: Exceção a controle
responsavel: Governança (dono do SGSI)
status: ativo
versao: 1.0
ultima_revisao: 2026-10-09
---

# Exceção a controle

Quando um controle do SGSI não pode ser cumprido por um tempo (sistema legado, fornecedor, custo), a exceção registra o
risco aceito, quem aceitou e até quando. Exceção vencida volta a ser não conformidade.

```processo
id: excecao
nome: Exceção a controle
modulo: governanca
prefixo: EXC
abre: todos
efeito: null
calculadora: excecao
descricao: "Quando um controle do SGSI não pode ser cumprido por um tempo: você descreve o risco e o controle compensatório; seu gestor aprova; o dono do SGSI aceita o risco até a validade."
campos:
  - chave: controles
    nome: Controles do Anexo A
    tipo: texto
    obrigatorio: true
    ajuda: "códigos separados por vírgula, como 8.8, 8.19"
  - chave: motivo
    nome: Por que não dá para cumprir
    tipo: texto
    obrigatorio: true
    ajuda: ""
  - chave: risco
    nome: Risco
    tipo: texto
    obrigatorio: true
    ajuda: o que pode acontecer e com que impacto
  - chave: compensatorio
    nome: Controle compensatório
    tipo: texto
    obrigatorio: false
    ajuda: o que reduz o risco enquanto vale a exceção
  - chave: validade
    nome: Válida até
    tipo: data
    obrigatorio: true
    ajuda: no máximo 1 ano
etapas:
  - id: gestor_aprova
    nome: Gestor aprova
    quem: gestor_do_solicitante
    acao: aprovar
  - id: sgsi_aceita
    nome: Dono do SGSI aceita o risco
    quem: sgsi
    acao: aprovar
```

## Caminho

1. **Gestor de quem pede aprova.** Dispensada quando não há gestor ou quando o próprio gestor pede.
2. **Dono do SGSI aceita o risco** (quem tem "administrar" em Governança; sem ninguém, os administradores).

## Pré-checagem (calculadora `excecao`)

- **Impedimentos:**
  - nenhum controle válido do Anexo A;
  - código que não existe no Anexo A;
  - validade que não está no futuro.
- **Alertas:**
  - validade de mais de 1 ano (o ideal é revisar em até 12 meses);
  - sem controle compensatório (o risco fica aceito integralmente).
- **Informação:** os controles citados, com o título.

## Depois de aprovada

A exceção conta como ativa em cada controle citado até a validade, aparece na página do controle e no painel do SGSI
(KPI `sgsi_excecoes_ativas`).
