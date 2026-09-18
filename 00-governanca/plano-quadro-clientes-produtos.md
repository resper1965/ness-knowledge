# Quadro de Clientes Produtos Cases e Métricas Implementation Plan

> **For agentic workers:** Execute as tarefas sequencialmente e valide cada entrega antes de avançar.

**Goal:** criar um workbook relacional e atualizável para clientes, ofertas, métricas e cases.

**Architecture:** o workbook usa IDs estáveis e abas normalizadas. O Painel lê as tabelas de entrada por fórmulas limitadas e exibe totais, cobertura e pendências sem duplicar registros.

**Tech Stack:** JavaScript, `@oai/artifact-tool` e XLSX.

**Spec:** `00-governanca/especificacao-quadro-clientes-produtos.md`

## Global Constraints

- Não inventar clientes, contratos ou números.
- Manter campos desconhecidos vazios.
- Separar autorização de publicação e confirmação do dado.
- Utilizar BlueDot `#00ADE8` com moderação e preservar legibilidade.
- Limitar fórmulas a faixas definidas.

### Task 1 Estrutura relacional

**Files:**
- Create: `05-operacao/Quadro-Clientes-Produtos-Cases-Metricas.xlsx`

- [x] Definir abas, chaves e colunas.
- [x] Criar Catálogo e Listas.
- [x] Criar Clientes, Contratos, Métricas, Cases e Evidências.

### Task 2 Dados iniciais

**Files:**
- Modify: `05-operacao/Quadro-Clientes-Produtos-Cases-Metricas.xlsx`

- [x] Cadastrar Alupar e Ionic Health sem inferir dados ausentes.
- [x] Cadastrar o portfólio consolidado.
- [x] Registrar cases anônimos e casos nominados conhecidos.

### Task 3 Painel e controles

**Files:**
- Modify: `05-operacao/Quadro-Clientes-Produtos-Cases-Metricas.xlsx`

- [x] Criar indicadores calculados.
- [x] Criar alertas de registros pendentes ou vencidos.
- [x] Aplicar validações e formatação condicional.

### Task 4 Verificação e publicação

**Files:**
- Modify: `05-operacao/Quadro-Clientes-Produtos-Cases-Metricas.xlsx`
- Modify: `README.md`

- [x] Recalcular e inspecionar fórmulas.
- [x] Renderizar todas as abas e corrigir problemas visuais.
- [x] Exportar, integrar ao pacote e salvar a versão final.
