---
titulo: Parâmetros de negócio do ness.brain
responsavel: Board
status: ativo
versao: 1.0
ultima_revisao: 2026-10-08
---

# Parâmetros de negócio do ness.brain

Limiares e premissas que os cálculos usam. O ness.brain lê o bloco `yaml` abaixo (painel, chat e MCP) e o aplica sem
deploy. Se o arquivo faltar ou um valor estiver fora da faixa, o sistema usa o valor padrão e mostra um aviso em
Configuração → Parâmetros.

**Como mudar:** abra um PR alterando o valor e a data. A mudança é aprovada pelo board (decisão do CEO de 08/10/2026):
o PR só é mesclado com o ok de um membro do board registrado no próprio PR. O motivo da mudança entra no registro de
decisões.

```yaml
janela_meses: 12
ofensor_multiplo_mediana: 2
ofensor_min_titulos: 6
margem_alvo: 0.20
recorrente_meses: 6
recorrente_minimo: 5
caixa_semanas: 13
cobertura_regras_meses: 6
aviso_custo: 0.8
```

| Parâmetro | Valor | Faixa aceita | Onde é usado | Racional |
| --- | --- | --- | --- | --- |
| `janela_meses` | 12 | 3 a 24 | Resultado por área, estrutura, overhead, sócios, metas, Board | Um ano fechado: tira sazonalidade e deixa de fora o mês corrente, parcial |
| `ofensor_multiplo_mediana` | 2 | 1,2 a 10 | Board: despesas fora do padrão | Pico mensal acima de 2 vezes a mediana destaca o atípico sem marcar a variação normal |
| `ofensor_min_titulos` | 6 | 1 a 60 | Board: despesas fora do padrão | Categoria com poucos títulos não tem padrão para comparar |
| `margem_alvo` | 0,20 | 0 a 0,8 | Metas por área, calculadora de preço (valor inicial) | Margem de referência da área; a calculadora aceita outro valor caso a caso |
| `recorrente_meses` | 6 | 3 a 12 | Caixa estimado | Janela de histórico para reconhecer o cliente recorrente |
| `recorrente_minimo` | 5 | 1 a `recorrente_meses` | Caixa estimado | Cliente com título em pelo menos 5 dos 6 meses é tratado como recorrente |
| `caixa_semanas` | 13 | 4 a 26 | Caixa de 13 semanas, telão, boletim | Um trimestre à frente, padrão de gestão de caixa |
| `cobertura_regras_meses` | 6 | 1 a 24 | Configuração → Regras do Omie (cobertura) | Mede as regras sobre o que está sendo lançado agora |
| `aviso_custo` | 0,8 | 0,5 a 0,95 | Conversa: aviso de custo | Avisa ao chegar a 80% do teto da conversa |

Os racionais de cada cálculo estão em `05-operacao/racionais.md`.
