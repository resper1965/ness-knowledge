# NESS knowledge seed

Estrutura inicial para separar fundamentos de marca, direção criativa dos websites e padrões de interface dos produtos SaaS.

O quadro mestre incorpora o **Status n.secops de 11/09/2026**, incluindo clientes, modelos contratuais, cobertura operacional e histórico de alertas. Esse recorte permanece interno até autorização específica.

## Árvore de conhecimento

Este repositório é uma **árvore de notas ligadas**, no formato do Obsidian: pode ser aberto direto como um vault, e o
ness.brain lê a mesma estrutura (árvore, links, "citado por" e busca). O git guarda o histórico e a aprovação por PR;
ninguém precisa usar o GitHub: as mudanças podem ser propostas pelo ness.brain ou pelo Obsidian com o plugin de git.

**Entradas da árvore (mapas por área):** [[00-governanca/_mapa|Governança]] · [[01-marca/_mapa|Marca]] ·
[[02-estrutura/_mapa|Estrutura]] · [[03-provas/_mapa|Provas]] · [[04-websites/_mapa|Websites]] ·
[[05-operacao/_mapa|Operação]] · [[06-processos/_mapa|Processos]] · [[07-produtos/_mapa|Produtos]]

**Convenções de toda nota:**

- **Cabeçalho (frontmatter)**, entre `---`:
  - `tipo`: politica, processo, kpi, parametro, decisao, contrato-de-dados, marca, prova, plano, fechamento, referencia
    ou mapa;
  - `titulo`, `responsavel` (dono), `status`, `versao`, `ultima_revisao`;
  - opcionais: `tags` (lista) e `relacionados` (lista de `[[links]]`).
- **Links** entre notas com `[[caminho/nota]]` ou `[[nota|texto]]`. Ligar sempre que uma nota depende de outra (a
  política cita o processo; o KPI cita o racional; a decisão cita o que mudou).
- **Mapa** (`_mapa.md`) em cada área, com a lista das notas. Nota nova entra no mapa da sua área.
- **Ativos** (logos, fontes, imagens) em `01-marca/ativos/`, descritos em [[01-marca/ativos/logos|logos]].
- **Blocos que o ness.brain lê** (```processo, ```soa, ```yaml de KPIs e parâmetros, ```vocabulario, ```regra) ficam
  dentro das notas e são validados a cada sincronização; problemas aparecem em Saúde no brain.
- **Exceções:** `05-operacao/regras-omie/` e `07-produtos/design-system/` seguem formato próprio (lido por ferramenta) e
  não levam cabeçalho.
- **Este repositório é interno e deve ser privado.** Nada daqui vai a público sem autorização registrada.

## Regra de precedência

1. `01-marca/brandbook-ecossistema-ness.md` governa a identidade transversal e a relação entre as marcas.
2. `01-marca/brand-foundations.md` resume os fundamentos técnicos confirmados.
3. `03-provas/` controla fatos, métricas e casos usados como evidência.
4. `04-websites/website-creative-direction.md` governa os sites institucionais.
5. `05-operacao/Quadro-Clientes-Produtos-Cases-Metricas.xlsx` mantém clientes, contratos, ofertas, métricas, cases e evidências.
6. `07-produtos/design-system/` governa interfaces autenticadas dos produtos.
7. `99-arquivo/` preserva as fontes originais sem torná-las normativas.
8. `forense-io/modusoperandi` é a **fonte da verdade operacional da forense.io** (método, processos, políticas, ferramentas e modelos de caso e de relatório). Este pacote mantém apenas o posicionamento, a voz e a identidade da marca, remetendo ao `modusoperandi` em tudo o que é operacional. Isso inclui o portal do cliente e os dashboards em `portal.forense.io`.

Em caso de conflito, regras de escopo específico prevalecem apenas dentro desse escopo. Uma decisão de interface de produto não limita automaticamente um website, documento ou apresentação.

## Estado deste pacote

- Brandbook do ecossistema: versão 0.1 concluída em Markdown, DOCX e PDF; pranchas técnicas de logotipo pendentes dos vetores oficiais.
- Fundamentos de marca: consolidados como resumo técnico.
- Direção criativa dos websites: princípios e limites, ainda sem layouts finais.
- Design system de produtos: conteúdo original reorganizado e escopo corrigido.
- Roadmap de enriquecimento: em execução.
- Biblioteca de provas: estrutura criada e dados conhecidos classificados para validação.
- Casebook: primeira versão criada com seis casos.
- Quadro de clientes e produtos: versão 1.0 criada com painel e bases relacionais.
- Nomenclatura de produtos: requer validação antes de remover nomes legados.
- ness.brain, o sistema agêntico de gestão: proposta, código e documentação no repositório `resper1965/ness-brain`; as decisões ficam no registro de decisões deste pacote.

## Próximas ações

1. Adicionar os arquivos oficiais de logotipo e fontes, quando disponíveis.
2. Validar a nomenclatura atual da família n. e registrar aliases legados.
3. Criar a direção visual específica de ness.com.br, trustness.com.br e forense.io.
4. Tornar privado o repositório `resper1965/ness-knowledge` (hoje está público).
