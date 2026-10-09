---
tipo: processo
titulo: Aprovação de política
responsavel: Governança (dono do SGSI)
status: ativo
versao: 1.0
ultima_revisao: 2026-10-09
---

# Aprovação de política

Política nova ou revisada do SGSI (segurança da informação, controle de acesso, uso aceitável, backup, resposta a
incidentes…). Quem abre é quem opera a Governança. A aprovação fica registrada com o documento e o hash dele.

```processo
id: politica
nome: Aprovação de política
modulo: governanca
prefixo: POL
abre: governanca
efeito: politica
calculadora: politica
descricao: "Política nova ou revisada do SGSI: o dono do SGSI aprova; os Heads são informados. Aprovada, o documento vira evidência (válida por 1 ano) do controle 5.1 e dos controles que ela cobre."
campos:
  - chave: titulo
    nome: Política
    tipo: texto
    obrigatorio: true
    ajuda: como Política de controle de acesso
  - chave: versao
    nome: Versão
    tipo: texto
    obrigatorio: true
    ajuda: como 1.0 ou 2026-10
  - chave: documento
    nome: Documento
    tipo: arquivo
    obrigatorio: true
    ajuda: a política em PDF (até 10 MB)
  - chave: controles
    nome: Controles que cobre
    tipo: texto
    obrigatorio: false
    ajuda: "códigos do Anexo A, como 5.15, 5.18; o 5.1 entra sempre"
  - chave: resumo
    nome: O que muda
    tipo: texto
    obrigatorio: true
    ajuda: para quem vai aprovar
etapas:
  - id: sgsi_aprova
    nome: Dono do SGSI aprova
    quem: sgsi
    acao: aprovar
  - id: heads_informados
    nome: Heads são informados
    quem: heads
    acao: informar
```

## Caminho

1. **Dono do SGSI aprova** (quem tem "administrar" em Governança; sem ninguém, os administradores).
2. **Heads são informados** por e-mail, sem decisão.

## Efeito (`politica`)

Aprovada, o documento vira evidência, válida por 1 ano a partir da aprovação, do controle 5.1 (políticas de segurança
da informação) e de cada controle citado no campo "Controles que cobre". A evidência aponta para o pedido de origem e
guarda o hash (sha256) do arquivo. Para renovar, abra uma nova aprovação com a versão revisada.

## Pré-checagem (calculadora `politica`)

- **Impedimentos:** sem documento anexado; código de controle que não existe no Anexo A.
- **Informação:** os controles de que o documento vai virar evidência.
