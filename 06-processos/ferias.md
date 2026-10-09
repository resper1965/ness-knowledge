---
titulo: Pedido de férias
responsavel: RH
status: ativo
versao: 1.0
ultima_revisao: 2026-10-09
---

# Pedido de férias

O colaborador preenche as datas; o RH confere se ele tem direito; o gestor direto aprova ou não; o RH é informado.
Decisão do CEO de 09/10/2026 (processo que não existia formalmente).

```processo
id: ferias
nome: Pedido de férias
modulo: rh
prefixo: FER
calculadora: ferias
descricao: "Você preenche as datas; o RH confere se você tem direito; seu gestor aprova ou não; o RH é informado."
campos:
  - { chave: inicio, nome: Início, tipo: data, obrigatorio: true, ajuda: primeiro dia de férias }
  - { chave: dias, nome: Dias corridos, tipo: inteiro, obrigatorio: true, ajuda: de 5 a 30, min: 1, max: 30 }
  - { chave: abono, nome: Vender dias (abono), tipo: inteiro, obrigatorio: false, ajuda: até 10 dias; só CLT, min: 0, max: 10, so_clt: true }
  - { chave: observacao, nome: Observação, tipo: texto, obrigatorio: false, ajuda: opcional }
etapas:
  - { id: rh_valida, nome: RH confere o direito, quem: rh, acao: validar }
  - { id: gestor_aprova, nome: Gestor aprova, quem: gestor_do_solicitante, acao: aprovar }
  - { id: rh_informado, nome: RH é informado, quem: rh, acao: informar }
```

## Pré-checagem (calculadora `ferias`)

A pré-checagem é um cálculo fixo, sem IA. Ela não decide: separa **impedimentos** (o que a regra não permite) de
**alertas** (o que pede atenção). O RH confere e decide. O saldo desconta as férias aprovadas e as que estão em
andamento.

**CLT**
- **Período aquisitivo:**
  - 12 meses a partir da admissão, com direito a 30 dias;
  - os dias são tirados do período mais antigo primeiro;
  - antes de completar o primeiro período não há saldo.
- **Período concessivo:** os 12 meses seguintes ao aquisitivo. Saldo fora dele é **férias vencidas**, pagas em dobro
  (alerta). Isso vale também para o pedido que termina depois do prazo.
- **Fracionamento:**
  - até 3 partes por período;
  - nenhuma com menos de 5 dias corridos;
  - uma delas com 14 dias ou mais.
- **Abono pecuniário:** venda de até 1/3 do período (10 dias).
- **Início:** não pode cair na sexta ou no sábado, os 2 dias antes do repouso semanal. Os feriados o RH confere.
- **Aviso:** com menos de 30 dias de antecedência, o pedido recebe um alerta.
- **Sobreposição:** datas que se sobrepõem a férias já pedidas são impedimento.

**PJ**
- **Descanso:** vale o descanso combinado no contrato, contado em dias por ano de contrato (`dias_descanso_ano` no
  cadastro). Os dias não acumulam de um ano para o outro.
- **Dias além do previsto:** pedir mais do que o contrato prevê gera alerta.
- **Abono:** não existe para PJ.

Fora do ness.brain: o cálculo dos valores, o pagamento e o eSocial ficam com a contabilidade.
