---
tipo: contrato-de-dados
titulo: Contrato de dados — portal (Área reservada)
responsavel: Ricardo Esper (CEO e CTO)
status: ativo
versao: 1.2
ultima_revisao: 2026-10-09
---

# Contrato de dados: portal

O portal (repositório `ness-brain-app`, CMS Payload em `ness-cms`) é o **sistema de registro do dia a dia**: pessoas,
jornada, pedidos, reembolsos, propostas, contratos, projetos, horas e oportunidades. O brain lê pela exportação.

## Acesso

- **Leitura incremental:** `GET https://ness-cms.ness.workers.dev/api/brain/exportar?colecao=<slug>&desde=<ISO>&pagina=<n>`,
  200 por página, ordem por `updatedAt`. Resposta: `{colecao, campos, pagina, totalPaginas, total, docs}`.
- **Conta:** usuário de serviço com o papel `brain` e API key (cabeçalho `Authorization: users API-Key <chave>`). A chave
  fica no Worker do brain como `PORTAL_API_KEY`.
- **Aviso na hora:** a fazer (padrão do `README.md`, §3.1), com o segredo `PORTAL_WEBHOOK_SEGREDO` nos dois lados.

## Coleções e campos exportáveis

Sempre vêm `id`, `createdAt` e `updatedAt`. Relação vem como o id do outro registro.

| Coleção | Campos | Chave |
| --- | --- | --- |
| pessoas | email, nome, cargo, vinculo, marcas, supervisor, admissao, saida, capacidadeMensal | email |
| ausencias | pessoa, supervisor, tipo, inicio, fim, status, decididaEm | id |
| reembolsos | solicitante, supervisor, status, valor, dataDespesa, categoria, clienteProjeto, foraDoPrazo, vencimento, pagoEm | id |
| pedidos | protocolo, tipo, departamento, solicitante, supervisor, status, responsavel | protocolo |
| propostas | numero, status, expirada, titulo, cliente, responsavel, origem, enviadaEm, validade, prazoMeses, ofertas, motivo, tenant | numero |
| contratos | tipo, numero, contraparte, objeto, cliente, proposta, responsavel, status, situacao, inicio, fim, valorMensal, reajusteIndice, reajusteBase, renovacaoAuto, renovacaoAviso | numero |
| projetos | codigo, nome, cliente, clienteNome, gestor, inicio, fim, horasPrev, proposta, contrato, status, fonte | codigo |
| clientes | nome, ofertas, vigenciaInicio, vigenciaFim, responsavel, tenant | id |
| periodos | pessoa, mes, supervisor, status, enviadoEm, fechadoEm | pessoa + mes |
| lancamentos | pessoa, data, projeto, atividade, minutos, deslocamento | id |
| oportunidades | codigo, titulo, cliente, clienteNome, cnpj, oferta, origem, responsavel, etapa, valorMensal, valorUnico, prazoMeses, previsao, probabilidade, motivoPerda, proposta, fechadaEm, tenant | codigo |

**O Omie não existe no portal** (decisão de 09/10/2026): nenhum campo nem regra do Omie lá.

**Nunca saem do portal:** custo/hora e contrato PJ, observações e motivos de ausência e reembolso, descrições livres,
comprovantes e anexos, contatos de clientes, dados e comentários de pedidos, denúncias, requisições LGPD, processos e a
auditoria. Atestado e CID nem entram no portal.

## Vocabulários

Valor gravado → rótulo. Fonte do código: `apps/cms/src/lib/*.ts` no portal.

### Oportunidades (`apps/cms/src/lib/oportunidades.ts`)

```vocabulario
etapa:
  prospeccao: Prospecção
  qualificacao: Qualificação
  proposta: Proposta
  negociacao: Negociação
  ganha: Ganha
  perdida: Perdida
origem:
  indicacao: indicação
  cliente-atual: cliente atual
  parceiro: parceiro
  evento: evento
  inbound: inbound (site, redes)
  prospeccao-ativa: prospecção ativa
  outra: outra
motivoPerda:
  preco: preço
  concorrente: escolheu concorrente
  sem-orcamento: sem orçamento
  sem-resposta: sem resposta
  projeto-cancelado: projeto cancelado
  fora-do-escopo: fora do nosso escopo
  outro: outro
probabilidade_padrao:
  prospeccao: 10
  qualificacao: 25
  proposta: 50
  negociacao: 75
  ganha: 100
  perdida: 0
```

Os rótulos de origem e motivo são os da `06-processos/politica-comercial.md`.

### Ausências (`apps/cms/src/lib/ausencias.ts`)

```vocabulario
tipo:
  ferias: férias (CLT)
  pausa: descanso combinado (PJ, estágio, terceiro)
  folga: folga
  medica: licença médica (sem atestado nem CID no sistema)
  legal: licença legal
  remoto: home office
  treino: treinamento
```

### Pessoas

```vocabulario
vinculo:
  clt: CLT
  pj: PJ
  socio: sócio
  estagio: estágio
  terceiro: terceiro
```

Os demais vocabulários (status de reembolso, pedido, proposta e contrato) entram aqui quando o brain passar a usá-los.

## Regras que valem dos dois lados

- **Oportunidade:** perdida exige motivo; ganha e perdida só a Diretoria reabre; valor total = mensal × prazo (12 meses se
  vazio) + único; ponderado = total × probabilidade (a informada ou a padrão da etapa).
- **Férias CLT:** 30 dias de antecedência; até 3 períodos por ano aquisitivo, de ao menos 5 dias, somando até 30.
- **Reembolso:** pedido em até 30 dias da despesa (fora do prazo é alerta, não bloqueio); comprovante único por hash.
