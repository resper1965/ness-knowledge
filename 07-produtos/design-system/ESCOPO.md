# Escopo do NESS Product Design System

Este pacote governa interfaces autenticadas dos produtos SaaS da família n. Ele não governa automaticamente websites institucionais, landing pages, documentos, apresentações ou campanhas.

## Aplica-se a

- Shells autenticados;
- Dashboards e telas operacionais;
- Tabelas, filtros, formulários e estados de dados;
- Autenticação e controle de acesso;
- Auditoria, gravação, desfazer e histórico;
- Componentes reutilizáveis dos produtos;
- Protótipos e novos módulos SaaS.

## Não se aplica automaticamente a

- ness.com.br;
- trustness.com.br;
- forense.io;
- Apresentações e propostas;
- Relatórios e documentos;
- Materiais de campanha;
- Conteúdo editorial.

O portal de acompanhamento e os dashboards da forense.io (`portal.forense.io`) são interfaces autenticadas, mas **não** fazem parte da família n.:
- **adotam a paleta** deste design system (`tokens/colors.css`);
- no restante, seguem o capítulo da forense.io no brandbook e a implementação mantida em `forense-io/modusoperandi`.

Regras transversais de marca estão em `01-marca/brand-foundations.md`. Diretrizes para os sites estão em `04-websites/website-creative-direction.md`.

## Nomenclatura pendente

O material original menciona `n.priv`, `n.risk` e `n.audit`. A arquitetura atual utiliza `n.privacy`, `n.iso`, `n.tprm`, `n.grc` e `n.training`, reunidos no n.360. Os nomes originais devem ser preservados até a validação de seu status como nomes legados, módulos internos ou produtos descontinuados.

