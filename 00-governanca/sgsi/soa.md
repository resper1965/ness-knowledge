---
titulo: Declaração de Aplicabilidade (SoA), rascunho
responsavel: Ricardo Esper (dono do SGSI)
status: rascunho
versao: 0.1
ultima_revisao: 2026-10-09
---

# Declaração de Aplicabilidade (SoA), rascunho

Rascunho da SoA do SGSI da ness. pela ISO/IEC 27001:2022, com os 93 controles do Anexo A. O ness.brain lê este
arquivo e mostra as diferenças em Governança › Controles; o dono do SGSI aplica com um botão, e cada mudança fica na
trilha do controle. Depois disso, ajustes finos podem ser feitos na própria tela ou aqui, por PR.

## Como ler

- **estado:** `nao_avaliado`, `nao_aplicavel`, `planejado`, `em_implementacao` ou `implementado`.
- **implementado** só onde o próprio ness.brain já registra o controle (os 12 marcados "no brain"). Os demais entram
  como `planejado` até haver evidência; a evidência é juntada na tela de cada controle.
- **justificativa:** por que o controle se aplica (risco, contrato, lei) ou, quando não aplicável, por que não. É o que
  o auditor lê.
- **dono:** quem revisa o controle e junta as evidências. Precisa ter acesso a Governança no ness.brain.
- **periodicidade_dias:** de quanto em quanto tempo o dono revisa (90 dias nos controles de acesso e de
  vulnerabilidade).

## A confirmar antes do merge

- **6.1:** checagem de antecedentes para quem acessa dados de clientes?
- **7.1 a 7.6, 7.8, 7.11 e 7.12:** há escritório ou área física própria, e quais equipamentos ficam nela? Sem instalação própria, esses controles podem virar não aplicáveis (a proteção física fica com os provedores).
- **8.30:** há desenvolvimento terceirizado? Sem terceiros, vira não aplicável.

## Donos propostos

| Controles | Dono |
| --- | --- |
| 5.1 a 5.4, 5.31, 5.32, 5.35 e 5.36 (políticas, direção, conformidade) | Ricardo Esper |
| Demais organizacionais (5.x) e pessoas (6.x) | Mônica Yoshida (Governança) |
| 5.34 (privacidade e LGPD) | Bárbara Alencar |
| Físicos (7.x), tecnológicos de infraestrutura e operação (8.x) e 5.7, 5.9, 5.17, 5.21, 5.23, 5.25, 5.26, 5.28, 5.30, 5.37, 6.7 | Ismael Araújo |
| Desenvolvimento (8.4, 8.25 a 8.31, 8.33) | Thiago Bertuzzi |

## Controles

```soa
controles:
  - { codigo: "5.1", estado: implementado, dono: resper@ness.com.br, periodicidade_dias: 365, justificativa: "Exigido pela cláusula 5.2 e pelos contratos de MSSP. Políticas aprovadas pelo processo Política do ness.brain, com hash e validade de 1 ano." }
  - { codigo: "5.2", estado: planejado, dono: resper@ness.com.br, periodicidade_dias: 365, justificativa: "Papéis do SGSI precisam estar definidos e comunicados; base para a cobrança de responsabilidades e para auditoria de clientes." }
  - { codigo: "5.3", estado: planejado, dono: resper@ness.com.br, periodicidade_dias: 365, justificativa: "Risco de fraude e erro em finanças, acessos e operação do SOC; aprovações do ness.brain já separam quem pede de quem decide." }
  - { codigo: "5.4", estado: planejado, dono: resper@ness.com.br, periodicidade_dias: 365, justificativa: "A direção precisa exigir o cumprimento das políticas (cláusula 5.1)." }
  - { codigo: "5.5", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "LGPD (ANPD), polícia e CERT.br em incidentes próprios e de clientes do SOC." }
  - { codigo: "5.6", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Como MSSP, participação em grupos e fóruns de segurança alimenta a operação e a inteligência de ameaças." }
  - { codigo: "5.7", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Essencial para o serviço de SOC (n.secops); a inteligência de ameaças também protege a própria ness." }
  - { codigo: "5.8", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Projetos de clientes e internos tratam informação sensível; segurança precisa entrar desde o início." }
  - { codigo: "5.9", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Base para classificar, proteger e devolver ativos; sem inventário não há gestão de risco." }
  - { codigo: "5.10", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Colaboradores usam ativos e dados de clientes; regras de uso aceitável são exigência contratual comum." }
  - { codigo: "5.11", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Devolução de equipamentos e dados no desligamento; o ness.brain já abre a revogação de acessos." }
  - { codigo: "5.12", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Dados de clientes e pessoais exigem classificação para definir a proteção." }
  - { codigo: "5.13", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Rotulagem segue a classificação (5.12)." }
  - { codigo: "5.14", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Troca de informação com clientes e fornecedores (relatórios do SOC, evidências forenses) precisa de regras e canais seguros." }
  - { codigo: "5.15", estado: implementado, dono: myoshida@ness.com.br, periodicidade_dias: 90, justificativa: "Pedidos de acesso com aprovação do gestor e do dono do sistema, executados pelo TI e registrados no ness.brain." }
  - { codigo: "5.16", estado: implementado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Cadastro único de pessoas e contas no ness.brain, mantido pelos processos de admissão e desligamento." }
  - { codigo: "5.17", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Senhas, MFA e segredos de clientes e sistemas precisam de regra de criação, guarda e troca." }
  - { codigo: "5.18", estado: implementado, dono: myoshida@ness.com.br, periodicidade_dias: 90, justificativa: "Direitos concedidos e retirados por pedido no ness.brain; revisão pelos gestores a cada 90 dias." }
  - { codigo: "5.19", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Dependência de fornecedores de nuvem, software e serviços com acesso a dados." }
  - { codigo: "5.20", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Contratos com fornecedores precisam de cláusulas de segurança e LGPD." }
  - { codigo: "5.21", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Cadeia de suprimentos de TIC (nuvem, SaaS, ferramentas do SOC) é risco relevante para um MSSP." }
  - { codigo: "5.22", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Mudanças em serviços de fornecedores afetam a operação; acompanhamento periódico." }
  - { codigo: "5.23", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "A operação usa nuvem (Cloudflare, Google Workspace, SaaS); configuração e saída precisam de regra." }
  - { codigo: "5.24", estado: implementado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Processo Incidente do ness.brain: registro por qualquer pessoa, tratamento pelo TI e encerramento pelo dono do SGSI." }
  - { codigo: "5.25", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Triagem de eventos é parte do SOC; para incidentes próprios, critérios de classificação no processo Incidente." }
  - { codigo: "5.26", estado: implementado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Tratamento de incidentes com trilha só de inclusão no ness.brain." }
  - { codigo: "5.27", estado: implementado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Lição aprendida registrada no encerramento de cada incidente pelo dono do SGSI." }
  - { codigo: "5.28", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Competência forense da ness. (forense.io); cadeia de custódia para incidentes próprios." }
  - { codigo: "5.29", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "A segurança precisa se manter durante crises e indisponibilidades." }
  - { codigo: "5.30", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Serviços contratados (SOC 24x7) dependem da continuidade de TIC." }
  - { codigo: "5.31", estado: planejado, dono: resper@ness.com.br, periodicidade_dias: 365, justificativa: "LGPD, Marco Civil, contratos com clientes e requisitos regulatórios dos clientes." }
  - { codigo: "5.32", estado: planejado, dono: resper@ness.com.br, periodicidade_dias: 365, justificativa: "Licenças de software e propriedade do código e dos entregáveis." }
  - { codigo: "5.33", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Registros contábeis, trabalhistas e de auditoria têm guarda legal; trilhas do ness.brain são só de inclusão." }
  - { codigo: "5.34", estado: planejado, dono: balencar@ness.com.br, periodicidade_dias: 365, justificativa: "LGPD: dados de colaboradores e de clientes tratados na operação." }
  - { codigo: "5.35", estado: planejado, dono: resper@ness.com.br, periodicidade_dias: 365, justificativa: "Exigido para manter a eficácia do SGSI (auditoria interna e externa)." }
  - { codigo: "5.36", estado: planejado, dono: resper@ness.com.br, periodicidade_dias: 365, justificativa: "Conformidade com as próprias políticas deve ser verificada pelos gestores." }
  - { codigo: "5.37", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Procedimentos operacionais do SOC e da infraestrutura precisam estar documentados." }
  - { codigo: "6.1", estado: implementado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Admissão pelo processo do ness.brain, aprovada pelos Heads. A confirmar: checagem de antecedentes para quem acessa dados de clientes." }
  - { codigo: "6.2", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Contratos CLT e PJ devem trazer responsabilidades de segurança e confidencialidade." }
  - { codigo: "6.3", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Conscientização e treinamento são exigência da cláusula 7.3 e dos clientes." }
  - { codigo: "6.4", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Processo disciplinar para violações de política." }
  - { codigo: "6.5", estado: implementado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Desligamento aprovado abre sozinho a revogação de todos os acessos no ness.brain." }
  - { codigo: "6.6", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "NDA com colaboradores, PJ e terceiros que acessam dados de clientes." }
  - { codigo: "6.7", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Trabalho remoto é a regra; precisa de requisitos de segurança para o equipamento e a rede." }
  - { codigo: "6.8", estado: implementado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Qualquer pessoa registra um evento ou suspeita pelo processo Incidente do ness.brain." }
  - { codigo: "7.1", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Proteção de instalações onde há informação e equipamentos. A confirmar: há escritório ou área física própria e quais equipamentos ficam nela? Sem instalação própria, pode virar não aplicável (a proteção física fica com os provedores)." }
  - { codigo: "7.2", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Proteção de instalações onde há informação e equipamentos. A confirmar: há escritório ou área física própria e quais equipamentos ficam nela? Sem instalação própria, pode virar não aplicável (a proteção física fica com os provedores)." }
  - { codigo: "7.3", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Proteção de instalações onde há informação e equipamentos. A confirmar: há escritório ou área física própria e quais equipamentos ficam nela? Sem instalação própria, pode virar não aplicável (a proteção física fica com os provedores)." }
  - { codigo: "7.4", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Proteção de instalações onde há informação e equipamentos. A confirmar: há escritório ou área física própria e quais equipamentos ficam nela? Sem instalação própria, pode virar não aplicável (a proteção física fica com os provedores)." }
  - { codigo: "7.5", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Proteção de instalações onde há informação e equipamentos. A confirmar: há escritório ou área física própria e quais equipamentos ficam nela? Sem instalação própria, pode virar não aplicável (a proteção física fica com os provedores)." }
  - { codigo: "7.6", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Proteção de instalações onde há informação e equipamentos. A confirmar: há escritório ou área física própria e quais equipamentos ficam nela? Sem instalação própria, pode virar não aplicável (a proteção física fica com os provedores)." }
  - { codigo: "7.7", estado: planejado, dono: myoshida@ness.com.br, periodicidade_dias: 365, justificativa: "Mesa e tela limpas valem no escritório e no trabalho remoto." }
  - { codigo: "7.8", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Proteção de equipamentos onde ficam. A confirmar: há escritório ou área física própria e quais equipamentos ficam nela? Sem instalação própria, pode virar não aplicável (a proteção física fica com os provedores)." }
  - { codigo: "7.9", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Notebooks fora das instalações são a regra no trabalho remoto." }
  - { codigo: "7.10", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Mídias com dados de clientes e evidências forenses." }
  - { codigo: "7.11", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Energia e conectividade dos equipamentos próprios. A confirmar: há escritório ou área física própria e quais equipamentos ficam nela? Sem instalação própria, pode virar não aplicável (a proteção física fica com os provedores)." }
  - { codigo: "7.12", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Cabeamento de rede e energia. A confirmar: há escritório ou área física própria e quais equipamentos ficam nela? Sem instalação própria, pode virar não aplicável (a proteção física fica com os provedores)." }
  - { codigo: "7.13", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Manutenção dos equipamentos para disponibilidade e integridade." }
  - { codigo: "7.14", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Descarte e reuso de notebooks e mídias com dados de clientes." }
  - { codigo: "8.1", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Notebooks e celulares acessam dados de clientes; precisam de proteção (criptografia, bloqueio, gestão)." }
  - { codigo: "8.2", estado: implementado, dono: iaraujo@ness.com.br, periodicidade_dias: 90, justificativa: "Administrar o ness.brain só direto na pessoa, nunca por pedido nem por grupo; outros sistemas a cobrir." }
  - { codigo: "8.3", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Acesso à informação restrito por necessidade; no ness.brain, por módulo e nível." }
  - { codigo: "8.4", estado: planejado, dono: tbertuzzi@ness.com.br, periodicidade_dias: 365, justificativa: "Código-fonte de produtos e de clientes precisa de acesso controlado." }
  - { codigo: "8.5", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "MFA e autenticação forte (Cloudflare Access e contas Google)." }
  - { codigo: "8.6", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Capacidade da operação do SOC e da infraestrutura." }
  - { codigo: "8.7", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Proteção contra malware em estações e servidores." }
  - { codigo: "8.8", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 90, justificativa: "Gestão de vulnerabilidades é serviço da ness. e obrigação própria." }
  - { codigo: "8.9", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Configuração segura e controlada de sistemas e nuvem." }
  - { codigo: "8.10", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Exclusão de dados ao fim do contrato e pela LGPD." }
  - { codigo: "8.11", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Mascaramento de dados pessoais em testes e relatórios." }
  - { codigo: "8.12", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Prevenção de vazamento de dados de clientes." }
  - { codigo: "8.13", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Backup de dados e sistemas para recuperação." }
  - { codigo: "8.14", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Redundância para os serviços contratados." }
  - { codigo: "8.15", estado: implementado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Trilhas só de inclusão no ness.brain (pedidos, cadastro, acessos, evidências); logs dos demais sistemas a cobrir." }
  - { codigo: "8.16", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 90, justificativa: "Monitoramento é o serviço do SOC; aplicado também à própria ness." }
  - { codigo: "8.17", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Sincronização de relógios para correlacionar logs e evidências." }
  - { codigo: "8.18", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Uso de utilitários privilegiados controlado." }
  - { codigo: "8.19", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Instalação de software controlada nas estações." }
  - { codigo: "8.20", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Segurança das redes próprias e de acesso remoto." }
  - { codigo: "8.21", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Serviços de rede contratados (internet, VPN, Cloudflare)." }
  - { codigo: "8.22", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Segregação entre ambientes de clientes e internos." }
  - { codigo: "8.23", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Filtragem web para reduzir risco de malware e phishing." }
  - { codigo: "8.24", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Criptografia em trânsito e em repouso para dados de clientes." }
  - { codigo: "8.25", estado: planejado, dono: tbertuzzi@ness.com.br, periodicidade_dias: 365, justificativa: "A ness. desenvolve software (n.flow, ness.brain, projetos de clientes)." }
  - { codigo: "8.26", estado: planejado, dono: tbertuzzi@ness.com.br, periodicidade_dias: 365, justificativa: "Requisitos de segurança nas aplicações desenvolvidas." }
  - { codigo: "8.27", estado: planejado, dono: tbertuzzi@ness.com.br, periodicidade_dias: 365, justificativa: "Arquitetura segura dos sistemas desenvolvidos." }
  - { codigo: "8.28", estado: planejado, dono: tbertuzzi@ness.com.br, periodicidade_dias: 365, justificativa: "Codificação segura no desenvolvimento." }
  - { codigo: "8.29", estado: planejado, dono: tbertuzzi@ness.com.br, periodicidade_dias: 365, justificativa: "Testes de segurança antes da entrega." }
  - { codigo: "8.30", estado: planejado, dono: tbertuzzi@ness.com.br, periodicidade_dias: 365, justificativa: "A confirmar: há desenvolvimento terceirizado? Sem terceiros, vira não aplicável." }
  - { codigo: "8.31", estado: planejado, dono: tbertuzzi@ness.com.br, periodicidade_dias: 365, justificativa: "Separação entre desenvolvimento, teste e produção." }
  - { codigo: "8.32", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Gestão de mudanças em sistemas e infraestrutura." }
  - { codigo: "8.33", estado: planejado, dono: tbertuzzi@ness.com.br, periodicidade_dias: 365, justificativa: "Dados de teste sem dados reais de clientes." }
  - { codigo: "8.34", estado: planejado, dono: iaraujo@ness.com.br, periodicidade_dias: 365, justificativa: "Testes de auditoria e pentests sem afetar a operação." }
```
