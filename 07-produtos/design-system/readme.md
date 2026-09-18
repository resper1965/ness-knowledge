# ness. — design system de interfaces autenticadas

> **Escopo:** este sistema governa aplicações SaaS e ambientes operacionais autenticados da família `n.`. Ele não governa websites institucionais, campanhas, documentos ou apresentações. Para esses canais, consulte o Brandbook do Ecossistema NESS e a Direção Criativa dos Websites.

Sistema de design das interfaces autenticadas da **ness.** e dos produtos com prefixo `n.`. Os nomes `n.iso`, `n.priv`, `n.risk` e `n.audit` presentes nas fontes são referências históricas ou de trabalho e precisam ser reconciliados com a arquitetura vigente do `n.360` antes da publicação externa.

Este sistema foi derivado de: o guia oficial de branding ness. (v2.0), o design system **Industry** (base estrutural: grade modular, objetos quadrados, hairlines), o repositório `resper1965/nISO` (produto real: Cloudflare Workers + Hono, frontend Vite em JavaScript puro) e as decisões tomadas no desenho do n.iso — wireframes, protótipos de SoA e de autenticação.

> **Sem logotipo fornecido.** Não há arquivo de marca nas fontes: a marca é composta **tipograficamente** (`ness.`, `n.iso`) com o ponto em BlueDot. Nada foi desenhado ou reconstruído de memória. Se existir um SVG oficial, envie para substituirmos.

## Índice

| Caminho | O que é |
| --- | --- |
| `styles.css` | Entrada de CSS — só `@import`. É o arquivo que o consumidor linka. |
| `tokens/` | `colors`, `typography`, `spacing`, `elevation`, `motion`, `base`. |
| `components/core/` | `Button`, `Chip`, `StatusBadge`, `Toast`. |
| `components/shell/` | `BrandMark`, `NavItem`, `PageHeader`. |
| `guidelines/` | Cartões de fundamento (cor, tipo, espaço). |
| `SKILL.md` | Versão Agent Skill, para uso no Claude Code. |

## Content fundamentals

**Português do Brasil**, sempre. O banco pode guardar em inglês; a interface nunca mostra a chave interna — existe dicionário de tradução (`implemented → Implementado`, `high → Alto`).

- **Tom**: direto e técnico, sem entusiasmo de marketing. A interface fala de trabalho a fazer, não de conquistas. `12 gaps abertos`, não `Você está quase lá!`.
- **Voz**: segunda pessoa quando há ação do usuário (`Confirme que você é humano`, `Sua sessão expirou`), impessoal para estado do sistema (`3 controles atribuídos a TI`).
- **Mensagem diz consequência, não sintoma.** `Sem evidência anexada. Este controle não passa na auditoria sem prova.` — não `Nenhum registro encontrado`.
- **Erro é honesto e útil**: credencial inválida é genérica de propósito (`E-mail ou senha incorretos`) para não permitir enumeração de usuário; falha de carga informa que nada foi gravado e traz o código do request.
- **Vazio oferece a próxima ação**, nunca um desenho triste.
- **Caixa**: rótulos em caixa normal; microcopy mono em uppercase com `letter-spacing` (`FASE 14 DE 41 · BLOCO 3`). Marcas sempre em caixa baixa, inclusive no início de frase — reformule se preciso (`A ness. atua desde…`).
- **Sem emoji.** Sem exclamação. Sem "oops".
- **Números com vírgula decimal** e milhar com ponto; datas `dd/mm/aaaa`; hora 24h com fuso quando importa (`16:38 BRT`).
- **Plural pela palavra inteira** (`1 alteração` / `2 alterações`), nunca concatenando sufixo.
- **Nenhum nome de fornecedor de infraestrutura** aparece na interface (anti-abuso, IdP, nuvem). Isso é informação de implementação.

## Visual foundations

**Cor.** Um accent só: BlueDot `#00ADE8`. Ele marca o que é interativo, o estado ativo e o ponto da marca — e o ponto nunca acompanha a cor do texto. Tema escuro é o padrão: ground `#0B1326`, cards e sidebar `#162244`, elevação `#1E2D52`, hairline `rgba(255,255,255,.10)`. Tinta em quatro degraus (`#F1F5F9 → #8FA0B6`), sendo o último o **mínimo** para texto pequeno — abaixo disso reprova em contraste. Semânticos `#10B981 / #F59E0B / #EF4444 / #00ADE8`. Tema claro existe para documento público e impressão.

**Tipografia.** Montserrat para marca (500) e títulos (600, tracking negativo); Inter para corpo (13px, entrelinha 1.5); JetBrains Mono para o que é dado e não prosa: hashes, códigos de request, atalhos, rótulos uppercase, contagens. Nunca Arial, Helvetica ou system font genérica.

**Espaço e geometria.** Base 4px. **Raio 0 em tudo** — o sistema é quadrado, herança do Industry. Banda da marca e banda de título com a mesma altura (64px), fechando na mesma hairline; sidebar 232px que recolhe para 72px; controles com 34px.

**Fundo.** Superfície chapada. **Sem gradiente, sem textura, sem imagem de fundo, sem `backdrop-filter`.** A separação entre camadas vem da superfície e da hairline, não de vidro.

**Cartões.** Retângulo com hairline de 1px, sem raio e sem sombra. Sombra só no que flutua de verdade: popover de conta, drawer, barra de ação em lote (`--shadow-lg`).

**Bordas e réguas.** Hairline de 1px em `--border-hairline`. Tabela: régua entre linhas, **última linha sem régua** — a tabela termina em ar. Cabeçalho fixo com fundo da página.

**Sombra interna.** Não existe. O estado ativo de navegação é `inset 2px 0 0` do accent — uma barra, não um preenchimento.

**Transparência e blur.** Transparência só para tinta em fundo de badge (`color-mix` 16%) e para tints de accent (12%). Blur: nunca.

**Animação.** Curta e com easing `cubic-bezier(.16,1,.3,1)`: `rise` 200ms para o que entra por baixo (toast, barra de lote, popover), `slide-in` 220ms para drawer, `fade-in` 160ms para overlay, `shake` 260ms para erro de credencial, `pulse` 1400ms no skeleton. Nada acima de 400ms. `prefers-reduced-motion` respeitado globalmente.

**Hover.** Mudança de cor de texto e de borda; nunca escala, nunca sombra que cresce. **Press**: sem transformação — o feedback é imediato no dado.

**Foco.** Anel accent de 2px com offset 2px, obrigatório em todo interativo. Nunca `outline: none`.

**Imagem.** O produto praticamente não usa: é interface de dado. Quando houver (login, página pública), duotone no accent, sem foto de banco de imagens genérica.

**Layout.** Sidebar fixa que não rola, com a navegação como região que encolhe; header fixo; cabeçalho de tabela fixo abaixo do header. Paginação explícita, não rolagem infinita.

## Iconography

**Lucide**, stroke 1.5, 11–16px, sempre `currentColor` — herda a cor do contexto e assume o accent quando o item está ativo. Sem preenchimento, sem ícone colorido, sem emoji, sem caractere unicode fazendo papel de ícone.

Distinção que o sistema faz: **ícone** quando o item é destino de navegação (Dashboard, SoA, Riscos, Evidências); **ponto de 8px** quando o item é um registro com estado (fase da jornada, relatório numerado, sub-navegação de controles) — ali o glifo informa progresso, não categoria, e um ícone diferente por item viraria ruído.

Ícones em uso: `layout-dashboard`, `route`, `list-checks`, `triangle-alert`, `file-check`, `file-text`, `users`, `database`, `shield-alert`, `server`, `landmark`, `clipboard-check`, `bookmark`, `search`, `check`, `building`, `key-round`, `moon`, `history`, `log-out`.

Favicon: o BlueDot puro — círculo `#00ADE8`, sem letra. `theme-color: #00ade8`.

> Os ícones vêm do CDN/pacote Lucide, não há sprite próprio no repositório de origem. Se a equipe adotar um conjunto próprio, substituir aqui.

## Regras de produto que o sistema carrega

Estas não são estéticas — são decisões registradas, e valem para qualquer produto da família:

- **Marca**: produto em caixa baixa com ponto interno em BlueDot. `ness.` aparece **só** na autenticação e no menu de conta; nunca na área de trabalho.
- **Shell**: leitura produto → tenant → navegação → conta. Título nomeia a tela, nunca o cliente. Tenant em uma linha; contexto de registro é a primeira linha do conteúdo.
- **Gravação**: imediato + Desfazer em lista e campo simples; rascunho com barra fixa só em documento longo.
- **Auditoria**: trilha append-only por campo (autor, quando, antes, depois, id da operação). Desfazer é janela pré-commit, nunca delete no log.
- **Autenticação**: acesso por convite; domínio decide senha ou IdP; MFA obrigatório em conta local (TOTP, sem SMS); erro genérico; desafio anti-abuso só a partir da 2ª falha; logout federado quando a sessão vem de IdP.
- **Estados obrigatórios** em toda tela de dados: vazio de busca, vazio de domínio, carregando com skeleton (nunca spinner), erro com código de request, sem permissão explicando o papel.
- **Acessibilidade**: 4.5:1 para texto pequeno, `aria-sort`, região `aria-live` permanente, `aria-label` em controle só-ícone, foco visível, reduced-motion.

## Adições intencionais

Não havia biblioteca de componentes nas fontes (o produto de origem é HTML gerado por template string). Os componentes aqui foram extraídos dos protótipos aprovados, não inventados: `Button`, `Chip`, `StatusBadge` e `Toast` existem literalmente nas telas do n.iso; `BrandMark`, `NavItem` e `PageHeader` são as três peças do shell que se repetem em todas elas.
