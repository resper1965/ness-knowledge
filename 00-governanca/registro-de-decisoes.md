---
titulo: Registro de Decisões de Design
responsavel: Diretoria e Marca
status: ativo
versao: 0.1
ultima_revisao: 2026-09-18
---

# Registro de decisões

## 2026-09-30 ness.brain: sistema agêntico de gestão

- Nomear o sistema como `ness.brain`, no padrão de `ness.OS`, sem o prefixo `n.`, por ser de uso interno.
- Aprovar a fase 1 somente leitura: agentes analisam, alertam e redigem; pessoas decidem e executam.
- Aprovar a política de skills confiáveis e a criação de usuários somente leitura no Omie e demais sistemas.
- Confirmar o Omie como ERP e o CRM do Omie como fonte do pipeline, acessados pela API própria com as chaves da empresa, sem o MCP oficial (custo e restrições); o sistema comercial próprio, em desenvolvimento, entra quando tiver API.
- Designar Ricardo Esper como responsável pela revisão e aceite dos entregáveis na fase 1.
- Aprovar o agente de OSINT com foco em know your client.
- Adotar núcleo único em Claude Agent SDK + Skills + MCP; OpenClaw, Hermes e Buzz ficam fora da fase 1.
- Manter este repositório como fonte do conhecimento e usar o repositório privado `resper1965/ness-brain` para o sistema. Repositório criado e esqueleto da fase 0 publicado em 30/09/2026.
- Registrar a estrutura do grupo: ness. no Lucro Real e n.secops como pessoa jurídica própria no Lucro Presumido. A n.secops ainda não tem Omie próprio e aparece como conta corrente no Omie da ness.; a separação está prevista.
- Limitar os agentes, em tributos, folha e eSocial, à montagem e validação de cenários para decisão. Apuração, recolhimento, eventos do eSocial e agenda fiscal ficam com a contabilidade.
- Estender a governança de provas aos agentes: número sem fonte não sai do agente; conteúdo público usa só prova `aprovada-publica`.
- Detalhes em `08-agentes/proposta-ness-brain.md`.
- Adotar como canal o painel ness.brain mais o briefing semanal por e-mail. **Confirmado pelo CEO em 30/09/2026.**
- Status: decidido pelo CEO em 30/09/2026. Hospedagem em avaliação (Cloudflare ou Vercel).

## 2026-09-24 forense.io: portal de acompanhamento, interfaces e presença digital

- Registrar no capítulo da forense.io o **portal de acompanhamento do cliente** (`portal.forense.io`), com a regra de mensagem: transparência sobre o andamento, sem expor a prova. A operação está em `forense-io/modusoperandi` (`05-pmo/portal-e-dashboard.md`, POL-08).
- Registrar que as interfaces da forense.io (portal, dashboards internos e tela de TV) seguem o Manual da Marca NESS v1.0 e ficam fora do escopo do design system da família n.
- **Paleta da forense.io: a do design system NESS** (`07-produtos/design-system/tokens/colors.css`), em interfaces, relatórios e TV. A paleta índigo proposta foi **recusada** pela liderança, e a forense.io não terá paleta complementar própria. **Decidido em 24/09/2026.**
- Nomear `#0B1326` como Noite, conforme o Manual v1.0.
- Observação para decisão, sobre o site `forense.io` (página própria, na navegação compartilhada da NESS):
  - alinhar os nomes das ofertas às modalidades Preservação e Preservação + Análise;
  - incluir o acesso a `portal.forense.io`;
  - explicitar a independência técnica.

  Detalhes em `04-websites/website-creative-direction.md`.
- Contexto operacional, sem decisão de marca: o hub anterior da forense.io (`api.hub.forense.io`, `tecsomobi.forense.io`) foi informado como fora de uso e é candidato a desativação (A18 no `modusoperandi`). O `portal.forense.io` passou a abrigar o novo portal.
- Status: a paleta está decidida. Os demais itens são propostas, e os ajustes do site aguardam decisão.

## 2026-09-23 forense.io: fonte da verdade operacional e posicionamento

- Declarar o repositório `forense-io/modusoperandi` fonte da verdade operacional da forense.io (método, processos, políticas, ferramentas, modelos de caso e de relatório); este pacote remete a ele e mantém posicionamento, voz e identidade.
- Registrar a forense.io como unidade autônoma e acrescentar ao capítulo da marca os territórios Independência técnica, Preservação e autenticidade, e Revisão e método visíveis.
- Explicitar as modalidades Preservação e Preservação + Análise e a retenção de 30 dias após a entrega.
- Acrescentar mensagens por público e regra de IA na perícia ao capítulo da forense.io.
- Registrar provas PRV-029 a PRV-034 como internas ou em validação; nenhuma é publicável ainda.
- Status: proposta, aguardando aprovação da Diretoria (alterações de território de marca).

## 2026-09-18 Ingestão do Status n.secops de 11/09/2026

- Registrar os 12 projetos do relatório como relacionamentos ativos de n.secops, preservando os modelos comerciais informados.
- Tratar o recorte de 2.961 endpoints como snapshot operacional específico; ele não substitui o consolidado anterior de 3.800 endpoints associados ao n.secops.
- Manter nomes, métricas, alertas e candidatos a case como informação interna até autorização específica de publicação.
- Normalizar `IONIC` para `Ionic Health` e `RZK` para `RZK Holding`, mantendo `RZK Agro` como organização distinta.
- Preservar e sinalizar a inconsistência original de RZK Agro — 11 licenças Datto totais e 14 em uso — para validação humana.
- Incorporar o histórico mensal de alertas e registrar que o detalhamento por classificação só está disponível entre julho e setembro.

## 2026-09-18 Quadro de clientes produtos cases e métricas

- Manter clientes, contratos, ofertas, métricas, cases e evidências em bases separadas relacionadas por IDs estáveis.
- Agregar no painel somente métricas explicitamente marcadas para inclusão.
- Tratar campos desconhecidos como vazios, nunca como zero.
- Separar validação do dado, autorização de uso do nome e autorização de publicação do case.
- Iniciar o quadro com Alupar, Ionic Health e quatro organizações anonimizadas derivadas dos casos conhecidos.

## 2026-09-18 Governança de provas

- Criar uma biblioteca canônica de fatos, métricas e casos.
- Permitir publicação somente para registros com status `aprovada-publica`.
- Separar veracidade, autorização e adequação de canal como validações independentes.
- Revisar métricas operacionais trimestralmente e casos semestralmente.

## 2026-09-18 Brandbook do ecossistema

- Adotar um Brandbook Mestre com capítulos próprios para ness., trustness. e forense.io.
- Manter o Documento Mestre como autoridade sobre estratégia, serviços, processos e provas.
- Manter regras de websites e de interfaces autenticadas em documentos separados.
- Tratar a versão 0.1 como normativa para estratégia e linguagem, com pranchas de logotipo pendentes dos vetores oficiais.
- Usar BlueDot `#00ADE8` como elo visual confirmado do ecossistema.

| Data | Decisão | Motivo | Escopo |
|---|---|---|---|
| 18/09/2026 | Separar fundamentos de marca, websites e produtos | Evitar que decisões de aplicações limitem a criação dos sites | Ecossistema |
| 18/09/2026 | Classificar o pacote extraído como design system de produtos | O material foi derivado de n.iso e de interfaces SaaS autenticadas | Produtos |
| 18/09/2026 | Preservar o ZIP recebido como fonte histórica | Manter rastreabilidade e permitir comparação futura | Governança |
| 18/09/2026 | Permitir direção criativa própria para os websites | Os sites possuem finalidade editorial e comercial distinta | Websites |
