---
titulo: Atestado ou licença
responsavel: RH
status: ativo
versao: 1.0
ultima_revisao: 2026-10-09
---

# Atestado ou licença

A pessoa informa a ausência e anexa o comprovante, o RH confere e o gestor é informado. Depois de aprovada, a ausência
aparece na agenda do painel RH, do MCP e da conversa como "ausência", sem o tipo. O tipo e o anexo ficam no pedido e só
quem pode ver o pedido tem acesso a eles.

```processo
id: ausencia
nome: Atestado ou licença
modulo: rh
prefixo: AUS
abre: todos
efeito: null
calculadora: ausencia
descricao: Você informa a ausência e anexa o comprovante; o RH confere; seu gestor é informado.
campos:
  - chave: tipo
    nome: Tipo
    tipo: lista
    obrigatorio: true
    ajuda: ""
    opcoes:
      - atestado médico
      - licença maternidade
      - licença paternidade
      - casamento
      - falecimento na família
      - doação de sangue
      - alistamento eleitoral
      - outra
  - chave: inicio
    nome: Primeiro dia
    tipo: data
    obrigatorio: true
    ajuda: ""
  - chave: dias
    nome: Dias
    tipo: inteiro
    obrigatorio: true
    ajuda: dias corridos
    min: 1
    max: 180
  - chave: comprovante
    nome: Comprovante
    tipo: arquivo
    obrigatorio: false
    ajuda: atestado, certidão ou declaração (PDF ou imagem, até 10 MB)
  - chave: observacao
    nome: Observação
    tipo: texto
    obrigatorio: false
    ajuda: opcional
etapas:
  - id: rh_valida
    nome: RH confere
    quem: rh
    acao: validar
  - id: gestor_informado
    nome: Gestor é informado
    quem: gestor_do_solicitante
    acao: informar
```

## Pré-checagem (calculadora `ausencia`)

A pré-checagem não bloqueia o pedido. Quem decide é o RH, na etapa dele.

- **Impedimento:** falta de comprovante. Vale para todo tipo, menos "outra".
- **Dias que a lei prevê.** Um alerta aparece quando o pedido passa deles. O excedente depende de acordo ou de política da
  empresa.

  | Tipo | Dias | Base |
  | --- | --- | --- |
  | Licença maternidade | 120 | CLT art. 392 (até 180 pela Empresa Cidadã, se houver adesão) |
  | Licença paternidade | 5 | ADCT art. 10 |
  | Casamento | 3 | CLT art. 473, II |
  | Falecimento na família | 2 | CLT art. 473, I |
  | Doação de sangue | 1 a cada 12 meses | CLT art. 473, IV |
  | Alistamento eleitoral | 2 | CLT art. 473, V |

- **Outros alertas:**
  - **Atestado médico com mais de 15 dias:** a partir do 16º dia a pessoa CLT passa ao INSS, e o RH encaminha a perícia.
  - **Segunda doação de sangue no mesmo ano.**
  - **Primeiro dia a mais de 60 dias de hoje.**
  - **Datas que se sobrepõem a férias ou a outra ausência,** já aprovadas ou em aprovação.
- **PJ:** a ausência segue o contrato, não a CLT.

## Anexo

- O comprovante é guardado no armazenamento de arquivos do ness.brain e identificado pelo hash SHA-256. O pedido
  guarda só o hash.
- Aceita PDF, PNG ou JPEG, de até 10 MB.
- Quem pode abrir o anexo:
  - quem pode ver o pedido: a própria pessoa, o gestor e o RH;
  - os administradores.
- **Fora do ness.brain:** o abono ou o desconto na folha e o eSocial (afastamentos) ficam com a contabilidade.
