---
tipo: plano
titulo: Plano de enriquecimento do Documento Mestre
responsavel: Estratégia e Marca
status: aprovado-para-execucao
versao: 0.1
ultima_revisao: 2026-09-18
---

# Objetivo

Enriquecer o Documento Mestre com recursos visuais e definições operacionais que reduzam ambiguidades sem transformá-lo em apresentação comercial ou expor detalhes sensíveis.

O documento atual possui 41 páginas, 544 parágrafos, 18 tabelas e nenhuma figura. A prioridade é aumentar clareza relacional, não simplesmente adicionar texto.

## Diagramas prioritários

| Prioridade | Diagrama | Localização | Uso |
|---|---|---|---|
| P0 | Arquitetura do portfólio NESS | Após Arquitetura do portfólio | Público simplificado e interno completo |
| P0 | Ciclo transversal do cliente | Modelo transversal de entrega | Público e interno |
| P0 | Operação integrada n.infraops, n.secops e ness.OS | Entre n.secops e ness.OS | Público simplificado e interno detalhado |
| P0 | Loop operacional e de melhoria | Seção n.infraops | Público e interno |
| P0 | Comando de crise n.cirt | Antes do ciclo de atuação n.cirt | Público simplificado e interno completo |
| P0 | Governança trustness. e plataforma n.360 | Após Papel de governança | Público e interno |
| P1 | Fronteiras entre detecção, governança, crise e perícia | Relação entre as ofertas | Público |
| P1 | Escala de autonomia dos agentes | Agentes de IA nas ofertas | Público e interno |
| P1 | Governança e cadência da conta | Governança e comunicação | Interno e resumo público |
| P1 | Continuidade do ness.OS | Continuidade do ness.OS | Interno e mensagem pública simplificada |
| P2 | Arquitetura dos três websites | Três domínios, uma atuação coordenada | Interno e editorial |
| P2 | Governança da fonte de verdade | Governança do documento | Interno |

## Conteúdo dos seis diagramas P0

### 1. Arquitetura do portfólio

Cliente no centro; n.infraops e n.secops como operação; n.devarch e n.autoops como engenharia; n.cirt como comando de crise; ness.OS como camada operacional; trustness. como governança; forense.io como investigação; n.360, n.flow e demais SaaS como plataformas. As relações devem usar verbos: opera, integra, governa, investiga e coordena.

### 2. Ciclo transversal do cliente

Qualificação → assessment e descoberta → desenho → onboarding → operação → governança → evolução → transição. Cada etapa deve indicar sua saída: baseline, arquitetura e RACI, integrações, registros, dashboards, roadmap e aceite.

### 3. Operação integrada

Usuários, endpoints, identidade, rede, cloud e aplicações → ferramentas do cliente e stack NESS → n.infraops e n.secops → ness.OS com coleta, deduplicação, correlação, casos, tickets, playbooks e indicadores → service desk, analistas, especialistas e PMO → cliente e stakeholders.

### 4. Loop operacional

Demanda ou evento → classificação → diagnóstico → resolução ou escalonamento → registro → problema, causa ou mudança → documentação → indicadores → backlog e melhoria. O desenho deve mostrar os vínculos ITIL sem tentar representar todo o framework.

### 5. Comando de crise

War room n.cirt como centro de coordenação, conectado à autoridade do cliente, equipes internas, fornecedores, n.secops, forense.io, jurídico, DPO e comunicação. Fluxo: ativação → escopo → contenção e preservação → investigação → decisões e comunicações → recuperação → lições aprendidas. A NESS recomenda e coordena; o cliente decide.

### 6. trustness. e n.360

Standards, requisitos, riscos, vulnerabilidades, terceiros, privacidade e IA → trustness. com assessment, priorização, responsáveis, cadência e validação → n.360 com n.privacy, n.iso, n.tprm, n.grc e n.training → execução pela NESS, cliente ou terceiros → evidências, risco residual, auditoria e decisão executiva.

## Seções estruturais a acrescentar

1. Mapa de fronteiras e responsabilidades: quem detecta, executa, governa, aprova, investiga e comunica.
2. Estado atual e estado pretendido: capacidade funcional, beta, planejada ou condicionada ao contrato.
3. Dicionário de métricas: fórmula, fonte, período, escopo, responsável e autorização de publicação.
4. Catálogo de integrações por categoria, preservando o agnosticismo e evitando marcas internas na versão pública.
5. Modelo de autoridade e escalonamento: playbook, RACI, autonomia dos agentes, HITL, GMUD, aceite de risco e acionamento de crise.
6. Arquitetura de evidências e publicação: fato confirmado, claim publicável, informação interna, case autorizado, case anonimizado e hipótese.
7. Anexos operacionais: arquitetura do ness.OS, incident report, assessment, matriz RACI e cadência de governança.

## Direção visual

- diagramas vetoriais e predominantemente monocromáticos, com BlueDot como acento;
- até 8 ou 10 nós visíveis por figura;
- variantes institucional e operacional quando houver informação sensível;
- legenda para serviço, plataforma, unidade especializada, pessoa e fonte de dados;
- figuras numeradas, tituladas e referenciadas no texto;
- detalhes técnicos deslocados para anexos.

## Ordem de execução

1. Produzir os seis diagramas P0 e inserir referências no texto.
2. Criar mapa de fronteiras e classificação de maturidade das capacidades.
3. Incorporar dicionário de métricas e modelo de autoridade.
4. Produzir diagramas P1.
5. Criar anexos operacionais e diagramas P2.
6. Revisar consistência, sensibilidade e publicabilidade de cada figura.
