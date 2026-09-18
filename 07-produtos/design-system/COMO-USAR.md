# Como usar este design system

Este pacote é um **design system completo**, mas ele só vira um sistema *anexável* (aquele que aparece na aba Design System e pode ser ligado a outros projetos) quando vive na **raiz de um projeto do tipo Design System**. Aqui ele está numa pasta, o que serve para download, versionamento em repositório e leitura por outra IA.

## Para torná-lo anexável

1. Crie um projeto novo e defina o tipo como **Design System** no menu Compartilhar.
2. Copie o conteúdo desta pasta para a **raiz** desse projeto — `styles.css` precisa ficar na raiz, não dentro de subpasta.
3. Um compilador indexa tokens, fontes e componentes automaticamente. Não crie `_ds_bundle.js`, `_ds_manifest.json` nem `index.js`: são gerados.
4. No menu Compartilhar, defina o tipo de arquivo como **Design System** para que a organização possa ver e anexar.

Feito isso, qualquer projeto novo pode anexar o sistema e eu passo a desenhar dentro dele sem você repetir regra nenhuma.

## Para usar no Claude Code

`SKILL.md` já está no formato de Agent Skill. Comite a pasta num repositório (ou em `~/.claude/skills/ness-design/`) e invoque pelo nome.

## Para usar como referência de código

`styles.css` + `tokens/` funcionam sozinhos em qualquer stack: linke o CSS e use as custom properties. Os componentes em `components/` são React sem dependência — se o seu produto não usa React (como o nISO, que é JavaScript puro), leia-os como especificação: os valores estão todos lá.

## Estrutura

```
styles.css              ← só @import; é o que o consumidor linka
tokens/                 ← colors, typography, spacing, elevation, motion, base
components/core/        ← Button, Chip, StatusBadge, Toast (+ .d.ts, card)
components/shell/       ← BrandMark, NavItem, PageHeader (+ .d.ts, card)
guidelines/             ← cartões de fundamento: cor, tipo, espaço
ui_kits/n-iso/          ← telas completas do produto (shell, SoA, autenticação, wireframes)
readme.md               ← o guia: conteúdo, visual, iconografia, regras de produto
SKILL.md                ← versão Agent Skill
```

## O que falta (e é honesto dizer)

- **Logotipo**: não há arquivo de marca nas fontes. A marca é composta tipograficamente. Se existir um SVG oficial, substitua e atualize o `readme.md`.
- **UI kits**: `ui_kits/n-iso/` traz as telas completas do produto (shell, SoA, autenticação, wireframes). Ao montar o projeto Design System, elas aparecem como telas de partida.
- **Fontes**: carregadas via Google Fonts. Se a ness. licenciar arquivos próprios, troque o `@import` por `@font-face` com os binários.
