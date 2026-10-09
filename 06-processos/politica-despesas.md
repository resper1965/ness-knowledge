---
titulo: Política de despesas (provisória)
responsavel: Financeiro
status: ativo
versao: 0.1
ultima_revisao: 2026-10-09
---

# Política de despesas (provisória)

**Os valores abaixo são provisórios.** A ness. ainda não tem uma política de despesas formal. Estes limites existem só
para o pedido de reembolso avisar quando um valor foge do comum. Para ajustar, mude os números por PR, aprovado pelo
CEO, e tire a linha `provisoria: true` quando a política for oficial.

- O limite vale **por despesa** (por pedido), em reais.
- Passar do limite **gera alerta, não bloqueia**. O gestor e o Financeiro decidem.
- O prazo é o número de dias, contado a partir da data da despesa, para pedir o reembolso. Depois dele, também só há
  alerta.

```politica
provisoria: true
prazo_dias: 90
limites:
  transporte: 500
  hospedagem: 800
  alimentação: 150
  quilometragem: 500
  material: 500
  cursos e eventos: 2000
  outra: 300
```

## Lembretes

- **Quilometragem:** o valor é o total do deslocamento. O cálculo (km × valor por km) vai na descrição até a política
  definir o valor por km.
- **Hospedagem:** um pedido por estadia. Se passar do limite, explique o motivo na descrição.
