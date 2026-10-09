---
tipo: marca
titulo: Logos e símbolos (repositório de ativos)
responsavel: Diretoria e Marca
status: provisorio
versao: 0.1
ultima_revisao: 2026-10-09
tags: [marca, logos, ativos]
relacionados: ["[[brandbook-ecossistema-ness]]", "[[brand-foundations]]"]
---

# Logos e símbolos

Fonte única dos logos das marcas do ecossistema. Portal, site, modelos e documentos usam estes arquivos, servidos pelo
ness.brain em URL estável. Ninguém guarda cópia própria.

> **Provisório.** Os SVG vieram do pacote de design do portal (`ness-brain-app`, `docs/handoff/entrega/design-system`).
> Os vetores oficiais ainda faltam (pendência do [[brandbook-ecossistema-ness]]). Ao chegarem, substituem estes arquivos
> com o mesmo nome, e todos os sistemas passam a usar os novos sem mudar link.

## Arquivos

Nome canônico: `<marca>-<versao>.svg`. Versões: **positivo** (sobre fundo claro), **negativo** (sobre fundo escuro),
**mono-claro** e **mono-escuro** (uma cor, para gravação, carimbo e fundos com imagem).

| Marca | Positivo | Negativo | Mono claro | Mono escuro |
| --- | --- | --- | --- | --- |
| ness. | `svg/ness-positivo.svg` | `svg/ness-negativo.svg` | `svg/ness-mono-claro.svg` | `svg/ness-mono-escuro.svg` |
| trustness. | `svg/trustness-positivo.svg` | `svg/trustness-negativo.svg` | `svg/trustness-mono-claro.svg` | `svg/trustness-mono-escuro.svg` |
| forense.io | `svg/forense-io-positivo.svg` | `svg/forense-io-negativo.svg` | `svg/forense-io-mono-claro.svg` | `svg/forense-io-mono-escuro.svg` |

Símbolos: `svg/simbolo-n.svg`, `svg/simbolo-t.svg`, `svg/simbolo-f.svg` e `svg/simbolo-bluedot.svg` (o ponto BlueDot
`#00ADE8`). Favicons: `favicon/ness.svg`, `favicon/trustness.svg`, `favicon/forense-io.svg`.

## Uso

- Marcas em caixa baixa; o ponto da marca é sempre BlueDot `#00ADE8` (regra do [[brandbook-ecossistema-ness]]).
- Não recolorir, esticar, girar nem aplicar sombra, gradiente ou contorno.
- PNG: gerado a partir do SVG quando um sistema pedir (o brain gera e guarda junto), nunca desenhado à mão.
- Área de proteção e tamanho mínimo: pendentes das pranchas técnicas do brandbook.

## Como trocar um arquivo

PR neste repositório substituindo o SVG pelo novo com o mesmo nome. O ness.brain publica a versão nova; o histórico do
git guarda as anteriores.
