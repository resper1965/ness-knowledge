---
tipo: contrato-de-dados
titulo: Contrato de dados — ingestão por arquivo
responsavel: Ricardo Esper (CEO e CTO)
status: ativo
versao: 1.0
ultima_revisao: 2026-10-09
---

# Contrato de dados: ingestão por arquivo

Para o que não tem API (§3.3 do padrão): um agente ou uma pessoa extrai o documento seguindo uma receita, o brain valida
contra o esquema e a extração fica em **quarentena** até a validação humana.

| Receita | Esquema (ness-brain) | Quem valida | Situação |
| --- | --- | --- | --- |
| Contrato de cliente (`contrato/v1`) | `schema/contrato.schema.json` | o CEO | em produção |
| Proposta comercial (`proposta_comercial/v1`) | `schema/proposta-comercial.schema.json` | o CEO | em produção |
| Colaboradores (`colaboradores/v1`) | `schema/colaboradores.schema.json` | — | **substituída**: pessoas nascem no portal |

- **Envio:** MCP do brain (`ingerir_extracao`, escopos `ingestao:contrato` e `ingestao:proposta`) ou tela de ingestão.
- **Original:** guardado no R2 pelo hash SHA-256.
- **Contratos e propostas** também existem no portal. Enquanto as duas portas coexistirem, a do portal é a fonte para o
  dia a dia e a receita serve para documentos antigos e de terceiros (carga histórica).
