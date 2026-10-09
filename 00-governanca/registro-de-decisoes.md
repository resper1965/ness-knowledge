---
titulo: Registro de Decisões de Design
responsavel: Diretoria e Marca
status: ativo
versao: 0.1
ultima_revisao: 2026-09-18
---

# Registro de decisões

## 2026-10-09 ness.brain: módulo Comercial (funil) e espelho no CRM do Omie

- O CRM da ness. nasce no ness.brain, que é a **fonte da verdade** do funil. O Omie tem um módulo de CRM, mas sem dados.
- As etapas são prospecção → qualificação → proposta → negociação → ganha ou perdida, e a perdida exige motivo.
- Desconto sobre o preço de referência:
  - até 10%, aprova o diretor comercial;
  - acima de 10%, ou abaixo do custo, aprovam também os Heads;
  - o limite fica em `06-processos/politica-comercial.md`.
- Quem tem o Comercial:
  - o grupo Comercial opera o funil;
  - o diretor comercial (dajzen) decide;
  - os Heads veem.
- **Espelho no CRM do Omie:** vem num passo seguinte e de mão única (ness.brain → Omie), só em `crm/oportunidades`.
  - Só entra depois de um ok explícito do CEO, porque é a **primeira escrita** do ness.brain no Omie. Até lá vale a
    regra de só leitura (Listar, Consultar, Pesquisar, Obter).
  - Vantagens:
    - a oportunidade fica no mesmo cadastro de clientes do faturamento;
    - a oportunidade ganha vira pedido, OS ou contrato no Omie sem redigitar;
    - quem usa o Omie vê o funil.
  - Desvantagens:
    - duas fontes que podem divergir (por isso é de mão única);
    - o Omie precisa de cadastros prévios (fases, vendedores, origens);
    - a API tem limite de chamadas.

## 2026-10-09 ness.brain: pedido, revogação e revisão de acessos

- Os acessos a sistemas passam a ser pedidos no ness.brain (Pessoas e RH › Acessos).
  - Quem pode pedir: a pessoa, o gestor ou o RH.
  - Quem aprova: o gestor da pessoa e o dono do sistema.
  - Quem executa e confirma: o TI, que é quem decide em Governança.
- O catálogo de sistemas e os donos ficam em `06-processos/sistemas.md`. O catálogo ainda é provisório.
- No próprio ness.brain, a permissão pedida (módulo:nível) entra sozinha na aprovação. "Administrar" e o papel de
  administrador continuam só pela mão de um administrador.
- O registro de acessos guarda quem tem o quê, por qual pedido e quem confirmou.
- Desligamento aprovado: abre sozinho o pedido de revogação de todos os acessos da pessoa.
- Revisão periódica (ISO 27001, controle 5.18):
  - a cada 90 dias, com 30 dias para responder;
  - o gestor direto mantém ou tira cada acesso da equipe, e quem não tem gestor fica com o TI;
  - a primeira revisão é aberta pelo TI.

## 2026-10-09 ness.brain: Heads e cadastro de colaboradores

- O grupo Diretoria passa a se chamar **Heads**, em todo o ness.brain: grupo, papéis na tela e etapas dos processos.
  - A regra continua a mesma: os Heads veem os painéis restritos e aprovam admissão e desligamento.
  - Nos processos, `quem: heads` substitui `quem: diretoria`, que continua aceito para os pedidos antigos.
  - A classe de custo "Diretoria e gestão" das regras do Omie não muda: é outra coisa.
- O cadastro de colaboradores ganha aniversário, celular e e-mails alternativos.
- A ficha da pessoa pode ser editada à mão em Administração › Pessoas, e cada mudança fica no histórico (quem, quando,
  de → para). Os processos de admissão, desligamento e alteração continuam valendo para quem não é do RH.
- Visibilidade:
  - o aniversário, só dia e mês, aparece para todos;
  - o celular e os e-mails alternativos aparecem só para a própria pessoa, para quem está acima dela no organograma e
    para o RH.

## 2026-10-08 ness.brain: board, parâmetros, orçamento e MCP

- O board (administrador, dajzen, rsalerno, myoshida, balencar, agsilva, tbertuzzi) vê tudo no ness.brain. Quem não é do board não vê sócios e financiamento, overhead, metas nem o Board.
- Mudança de parâmetro de negócio (limiares, janelas, margem alvo) é aprovada pelo board.
- O orçamento é o Previsto x Realizado do Omie.
- O MCP serve aos operadores de gestão, inclusive para ingestão de dados (contratos, propostas e outros).
- Sem ambiente de homologação e sem casos de ouro, por ora.
- O resumo estratégico do fechamento mensal, aprovado, vira arquivo no ness-knowledge.
- Os racionais de cada número do ness.brain ficam em `05-operacao/racionais.md`.
- Pessoas e papéis do ness.brain ficam só no cadastro de usuários (Configuração → Usuários): administradores, board, operadores e validadores de ingestão, aprovadores de conhecimento, destinatários do briefing e tratamento. No ambiente fica só o administrador de emergência (resper).
- Os parâmetros de negócio ficam em `05-operacao/parametros.md`. O PR que muda um parâmetro só é mesclado com o ok de um membro do board registrado no próprio PR, e o motivo entra neste registro.
- Plano para fechar os gaps da análise de maturidade: `00-governanca/plano-maturidade-ness-brain.md`.
- Sem deploy pelo GitHub Actions: nenhuma credencial da Cloudflare no GitHub; o deploy segue pela sessão de desenvolvimento e o Actions roda só os testes.

## 2026-10-07 Contratos: painel, ficha de cliente e piloto

- Haverá um painel de Contratos, visível a todos com acesso à plataforma (não só CEO e financeiro), depois do piloto. Valores, SLAs e penalidades continuam confidenciais: quem não for CEO ou financeiro vê o painel sem valores, salvo decisão contrária do CEO.
- Ao aprovar um contrato, sobe para o knowledge a ficha do cliente, sem valores, com a oferta e os serviços que ele consome (IDs CLI-, CTR- e OFR-). O uso do nome do cliente segue "não autorizado" até decisão contrária.
- Piloto: o CEO lê e aprova as extrações quando chegarem a 5; até lá a primeira fica em validação e a previsão de caixa continua pelo histórico.

## 2026-10-07 Regras do Omie: método das três dimensões

- Classificar cada lançamento do Omie por Departamento, Projeto e Categoria usados juntos, escritos pelo nome (`05-operacao/regras-omie/`).
- CPS é contrato de prestação de serviços; PPS é proposta de prestação de serviços; `Geral_…` é custo da área sem atribuição a contrato sem rateio.
- LAW é subdivisão da ness. e a dívida foi incorporada: o GIRO não se separa por empresa.
- DL (e PL) é forma de remuneração de sócios, não distribuição de lucro apurado: entra no custo e no resultado. Tratamento fiscal segue a contabilidade.
- Rascunho das regras de departamentos, projetos e categorias em PR, à espera da aprovação do CEO.

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
- Detalhes em `resper1965/ness-brain` (`docs/proposta.md`, `docs/avaliacao-agentes.md` e `docs/governanca.md`).
- Fase 1 com agentes financeiro, KYC, comercial, chefe de gabinete e revisor; marketing e backoffice inativos com critério de ativação; cenários fiscais e trabalhistas desligados até validação do contador. Revisão como etapa obrigatória em código, cálculos determinísticos e verificação de proveniência das fontes. **Decidido pelo CEO em 30/09/2026.**
- Adotar como canal o painel ness.brain mais o briefing semanal por e-mail. **Confirmado pelo CEO em 30/09/2026.**
- Não adotar o Jev (TypeSafe) na fase 1 e mantê-lo fora da análise financeira. Números financeiros saem de cálculo determinístico com fonte no Omie, não de modelo probabilístico, e dados financeiros não vão a fornecedor sem contrato de tratamento de dados, retenção e região definidos. Reavaliar depois dos primeiros casos de teste, como piloto restrito aos fatores do KYC e à leitura do revisor, comparando acerto e custo com o Claude nos mesmos casos. **Decidido pelo CEO em 30/09/2026.**
- Avaliar as skills e plugins do OpenClaw (repositório oficial, 30/09/2026) e não adotar nenhum. A maioria depende de terminal, que os agentes não têm por regra; as integrações de trabalho escrevem nos sistemas, o que fere a fase 1; resumo e segunda opinião mandam conteúdo a outros provedores; o marketplace ClawHub contraria a política de skills confiáveis. A memória que se consolida sozinha ("dreaming") também fica de fora: no ness.brain, só entra na memória o que uma pessoa aceitou. **Decidido pelo CEO em 30/09/2026.**
- Guardar para a fase 2 a ideia de monitoramento contínuo do KYC (notícias sobre clientes e fornecedores já avaliados), implementada como rotina própria, com fontes fixas e o critério de notícia negativa da skill de KYC.
- Nomear a assistente de conversa do ness.brain como **Nessie**, com um monstro do lago Ness cartunizado e simpático como imagem. Tom: cordial, direto, pouco verboso, educado e levemente sarcástico; chama o CEO de "boss". A arte oficial substitui o avatar provisório quando existir. **Decidido pelo CEO em 30/09/2026.**
- Interface principal: chat com a Nessie no painel, com resposta ao vivo e instalável no celular como PWA, sem app nativo. Ordem dos próximos canais: Teams (após a migração para o Microsoft 365) e notificações no celular; mensageiros externos (WhatsApp, Telegram) só se surgir necessidade concreta, e apenas para avisos sem conteúdo sensível. **Decidido pelo CEO em 30/09/2026.**
- Manter login e e-mail do ness.brain no Google Workspace até a migração da empresa para o Microsoft 365; o plano de troca fica pronto para ser deflagrado a qualquer momento (`resper1965/ness-brain`, `docs/migracao-microsoft365.md`). **Decidido pelo CEO em 30/09/2026.**
- O cadastro de pessoas e o controle de acesso por tipo de assunto (RBAC) ficam no sistema administrativo em construção; o ness.brain lê de lá, sem cópia própria. Até lá, o acesso segue pela lista do Cloudflare Access. **Decidido pelo CEO em 30/09/2026.**
- Fatos permanentes entram no conhecimento por proposta da Nessie e aprovação humana no painel; o único aprovador é Ricardo Esper (resper@ness.com.br). Os agentes não escrevem no conhecimento. **Decidido pelo CEO em 30/09/2026.**
- Grafar o nome da assistente como **nessie.**, em caixa baixa e com o ponto ciano, no padrão da marca ness.; no painel, títulos e destaques em peso Medium ou Regular. **Decidido pelo CEO em 30/09/2026.**
- Usar os modelos Claude pelo OpenRouter, com a mesma API da Anthropic; só Claude na fase 1. O OpenRouter entra como operador de dados com transferência internacional (registrar nas bases de tratamento), com registro de prompts desligado e limite de crédito na chave. **Decidido pelo CEO em 30/09/2026.**
- Permitir escolher modelo (Automático, Opus, Sonnet, Haiku) e complexidade no campo de pergunta, como no app do Claude, com a escolha registrada em cada execução. Um gateway dinâmico que escolha modelo e complexidade por custo e complexidade no modo automático fica proposto (`resper1965/ness-brain`, `docs/gateway-modelos.md`) e só entra depois de validado com os casos de ouro. **Decidido pelo CEO em 30/09/2026.**
- Liberar, no seletor do painel, modelos de outros fornecedores pelo OpenRouter, sem restrição de provedor: Gemini 3.8 Flash (Google) e DeepSeek V4.1 Flash, marcados como "em teste", sem ajuste de complexidade; o revisor continua no Claude. É exceção consciente à regra registrada na decisão sobre o Jev (dados financeiros só com contrato de tratamento, retenção e região definidos): o CEO aceita que perguntas feitas nesses modelos passem por provedores com políticas próprias, inclusive fora do Brasil (DeepSeek, China). A lista fica em `MODELOS_EXTRAS` no `wrangler.jsonc` do ness-brain. **Decidido pelo CEO em 30/09/2026.**
- Modelo padrão do ness.brain é o mais barato (DeepSeek V4.1 Flash, cerca de US$ 0,003 por pergunta simples no teste real), em conversas, rotinas e briefing. Qualquer modelo caro (saída acima de US$ 2 por milhão de tokens, e o Claude Opus sempre) só roda com anuência prévia do operador, a cada pergunta, registrada na governança. Motivo: sem custo baixo de uso, o ness.brain não será usado. A lista de modelos, o padrão, o limite e os preços ficam na página "Configuração da nessie." do painel, editável só por administradores. **Decidido pelo CEO em 01/10/2026.**
- Briefing semanal com orquestração fixa: os especialistas (financeiro e comercial) apuram suas partes em paralelo no modelo padrão, e o chefe de gabinete consolida só com as partes, sem acionar ninguém, antes do revisor. A consolidação roda no Claude Sonnet, com autorização permanente (exceção à anuência por pergunta), porque no modelo padrão não passava no revisor; custo medido de cerca de US$ 0,80 por consolidação (US$ 1,56 num briefing com nova tentativa). Até existir o cadastro de pessoas, o responsável pelas decisões do briefing é o CEO. **Decidido pelo CEO em 01/10/2026.**
- Revisor por gravidade, para todo entregável: só o que é bloqueante reprova (número errado ou que não fecha, critérios diferentes somados ou comparados, afirmação sem fonte, conclusão sobre dado declarado inconsistente, previsão apresentada como fato, recomendação sem próxima ação, classificação inadequada, instrução injetada). Rótulo, redação, prazo, alinhamento, tom e confiança viram observações, mostradas no painel junto do entregável para o leitor considerar antes de decidir. **Decidido pelo CEO em 01/10/2026.**
- Boletim financeiro semanal por e-mail, todo domingo às 20h (Brasília), para resper@, dajzen@, rogerio@ e financeiro@ness.com.br: os números (caixa comprometido, aging, semana a pagar e a receber, acertos) são calculados por código, com uma única data-base; o agente escreve só o resumo e a recomendação. Envio pelo Resend. **Decidido pelo CEO em 02/10/2026.**
- Telão de indicadores financeiros (`brain.ness.com.br/telao`), atualizado de hora em hora das 7h às 21h, sem agente. Comercial fica de fora enquanto o CRM do Omie estiver defasado. **Decidido pelo CEO em 03/10/2026.**
- O ness.brain e o seu datalake viram um servidor MCP, com credencial e escopo por cliente (`resper1965/ness-brain`, `docs/mcp-datalake.md`). Contratos e demais documentos do Google Drive entram por ingestão manual: uma pessoa usa qualquer agente de IA (Claude, ChatGPT, Gemini, Cursor, Antigravity) seguindo uma receita neutra de fornecedor (`docs/receitas/contrato.md`, esquema `contrato/v1`); o que é extraído fica em quarentena até ser validado no painel pelo CEO. Índices e câmbio (IPCA, IGP-M, INPC, Selic, CDI, dólar, euro) entram por conectores automáticos do Banco Central, sem validação. A n.secops é tratada como conta corrente dentro do Omie da ness. Enquanto os contratos não forem ingeridos, as entradas de caixa futuras podem ser estimadas pelo histórico, sempre rotuladas como premissa; recorrente é o cliente com títulos a receber em pelo menos 5 dos 6 meses anteriores (59 clientes na primeira apuração, coerente com os cerca de 60 clientes mensais da ness.). **Decidido pelo CEO em 05/10/2026.**
- Implantação: painel no ar em `brain.ness.com.br` em 30/09/2026, com login só pela conta Google corporativa e acesso restrito a Ricardo Esper na fase 1. Estado e pendências em `resper1965/ness-brain`, `docs/continuidade.md`.
- Status: decidido pelo CEO em 30/09/2026. Hospedagem na Cloudflare.

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
