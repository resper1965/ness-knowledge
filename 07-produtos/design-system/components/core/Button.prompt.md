Botão de ação: use `primary` para a ação principal da tela (uma por banda de título), `secondary` para as demais, `ghost` para ações de baixo peso e `danger` para destrutivas.

```jsx
<Button variant="primary" onClick={exportar}>Exportar SoA</Button>
<Button variant="secondary">Colunas</Button>
<Button disabled disabledReason="Aplicabilidade exige justificativa individual">Marcar N/A</Button>
```

Regras: nunca esconda um botão que ficou indisponível — desabilite e explique no `disabledReason`. O preenchimento ciano sempre leva tinta `--accent-ink`; branco sobre BlueDot reprova em contraste. Sem raio, sem gradiente.
