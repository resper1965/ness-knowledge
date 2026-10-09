---
titulo: Política comercial (provisória)
responsavel: Comercial
status: ativo
versao: 0.1
ultima_revisao: 2026-10-09
---

# Política comercial (provisória)

As listas e números que o funil do ness.brain usa. **Os valores são provisórios.** Ajuste por PR, aprovado pelo CEO, e
tire `provisoria: true` quando estiverem certos.

- **ofertas:** o que se vende (ver `01-empresa/estrutura-e-ofertas.md`).
- **origens:** de onde veio a oportunidade.
- **motivos_perda:** obrigatório para marcar uma oportunidade como perdida.
- **probabilidade:** chance de ganhar em cada etapa aberta, em %. O funil ponderado é o valor total × a probabilidade.
  O dono pode informar outra probabilidade na oportunidade.
- **limite_desconto_diretor:** até este desconto sobre o preço de referência, só o diretor comercial aprova. Acima dele,
  os Heads também aprovam.

```politica-comercial
provisoria: true
ofertas: [n.secops, n.infraops, n.360, n.flow, trustness., forense.io, outra]
origens: [indicação, cliente atual, parceiro, evento, "inbound (site, redes)", prospecção ativa, outra]
motivos_perda: [preço, escolheu concorrente, sem orçamento, sem resposta, projeto cancelado, fora do nosso escopo, outro]
probabilidade:
  prospeccao: 10
  qualificacao: 25
  proposta: 50
  negociacao: 75
limite_desconto_diretor: 10
```

## Etapas do funil

| Etapa | O que significa |
| --- | --- |
| Prospecção | contato inicial; ainda não se sabe se há necessidade e orçamento |
| Qualificação | necessidade, orçamento, decisor e prazo conhecidos |
| Proposta | proposta enviada (de preferência, ligada à proposta conferida pela ingestão) |
| Negociação | ajustes de escopo, preço ou contrato |
| Ganha | contrato assinado ou pedido aceito |
| Perdida | com o motivo |
