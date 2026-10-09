---
tipo: contrato-de-dados
titulo: Contrato de dados — o que o ness.brain expõe
responsavel: Ricardo Esper (CEO e CTO)
status: rascunho
versao: 0.1
ultima_revisao: 2026-10-09
tags: [integracoes, brain, portal]
relacionados: ["[[README]]", "[[portal]]"]
---

# Contrato de dados: o que o ness.brain expõe

O caminho inverso do [[portal]]: o que outros sistemas (hoje, o portal) leem do brain. Segue o [[README|padrão de
conexão]]. Nenhum sistema calcula por conta própria um número que o brain já calcula.

## Acesso

- API de leitura `https://brain.ness.com.br/api/v1/...`, com conta de máquina por sistema (no brain, credencial com
  escopo; no Cloudflare Access, token de serviço).
- **A pessoa vai junto:** cada chamada informa o e-mail de quem está usando o sistema, e o brain aplica as permissões
  **dessa pessoa** (módulo e nível; visão board). O sistema chamador nunca vê mais que o usuário veria no brain.

## O que é exposto

| Recurso | Uso no portal | Situação |
| --- | --- | --- |
| Conhecimento (árvore: notas, links, busca) | tela Conhecimento, marca, design system, modelos, provas | a fazer |
| Ativos de marca (`01-marca/ativos/`) | Kit de marca, site, modelos | a fazer |
| Políticas vigentes (despesas, comercial, catálogo de sistemas, SoA) | aplicar limites e listas sem copiar regra | a fazer |
| Pergunte à nessie | caixa de pergunta no portal, com a permissão de quem pergunta | a fazer |
| Indicadores por papel (equipe, funil, caixa, SGSI) | Início do portal | a fazer |
| Boletins enviados | histórico para quem recebe | a fazer |
| Situação do espelho no Omie por oportunidade | selo na oportunidade | a fazer |

## Fora (decisão do CEO de 09/10/2026)

- **Cadastros do Omie no portal:** o Omie sai do portal por completo; o portal não guarda nem escolhe código do Omie.
- **Relatórios analíticos no portal:** margem, custo e resultado são só do brain. No portal ficam os relatórios
  operacionais (horas do mês, fechamento, exportação à contabilidade).
- **Sinais cruzados** (contrato sem receita, proposta ganha sem contrato): fora por ora.
