---
tipo: processo
titulo: Incidente de segurança
responsavel: Governança (dono do SGSI)
status: ativo
versao: 1.0
ultima_revisao: 2026-10-09
---

# Incidente de segurança

Qualquer colaborador registra um incidente de segurança ou uma suspeita, em Pessoas e RH › Novo pedido ou no botão
"Registrar incidente" do painel de Governança. Registrar cedo vale mais que registrar completo: o TI completa o resto.
Atende aos controles 5.24 a 5.28 e 6.8 do Anexo A da ISO/IEC 27001:2022.

```processo
id: incidente
nome: Incidente de segurança
modulo: governanca
prefixo: INC
abre: todos
efeito: null
calculadora: incidente
descricao: "Qualquer pessoa registra um incidente ou suspeita; o TI trata e registra a resposta; o dono do SGSI encerra com a lição aprendida."
campos:
  - chave: titulo
    nome: O que aconteceu
    tipo: texto
    obrigatorio: true
    ajuda: uma frase
  - chave: quando
    nome: Quando foi percebido
    tipo: data
    obrigatorio: true
    ajuda: ""
  - chave: tipo
    nome: Tipo
    tipo: lista
    obrigatorio: true
    ajuda: ""
    opcoes: ["acesso indevido", "vazamento de dados", "malware ou ransomware", "phishing", "indisponibilidade", "perda ou roubo de equipamento", "violação de política", "outro"]
  - chave: impacto
    nome: Impacto
    tipo: lista
    obrigatorio: true
    ajuda: sua avaliação; o TI pode rever
    opcoes: ["baixo", "médio", "alto", "crítico"]
  - chave: dados_pessoais
    nome: "Envolve dados pessoais?"
    tipo: sim_nao
    obrigatorio: false
    ajuda: "de clientes, colaboradores ou terceiros"
  - chave: sistemas
    nome: Sistemas ou ativos
    tipo: texto
    obrigatorio: false
    ajuda: quais foram afetados
  - chave: descricao
    nome: Descrição
    tipo: texto
    obrigatorio: true
    ajuda: "o que você viu, sem apagar nada"
  - chave: evidencia
    nome: Evidência
    tipo: arquivo
    obrigatorio: false
    ajuda: "print, registro ou e-mail (PDF ou imagem, até 10 MB)"
etapas:
  - id: ti_trata
    nome: TI trata e registra a resposta
    quem: ti
    acao: validar
  - id: sgsi_encerra
    nome: Dono do SGSI encerra com a lição aprendida
    quem: sgsi
    acao: validar
```

## Caminho

1. **TI trata** (quem tem "decidir" em Governança): contém o incidente, corrige e registra a resposta no comentário.
2. **Dono do SGSI encerra** (quem tem "administrar" em Governança): registra a lição aprendida e o que muda nos
   controles (5.27).

Sem ninguém com esses níveis, decidem os administradores. Ninguém decide etapa do próprio pedido.

## Pré-checagem (calculadora `incidente`)

- **Impedimento:** data do incidente no futuro.
- **Alertas:**
  - envolve dados pessoais: avaliar a comunicação à ANPD e aos titulares (LGPD, art. 48; Resolução CD/ANPD nº
    15/2024: em até 3 dias úteis do conhecimento, quando houver risco ou dano relevante);
  - impacto alto ou crítico: avisar os Heads e avaliar o plano de continuidade (5.29 e 5.30).
- **Informação:** guardar as evidências (registros, prints, hashes) antes de corrigir (5.28).

## Quem vê

Quem registrou, o TI, o dono do SGSI e quem tem acesso ao módulo Governança. O painel do SGSI mostra os incidentes em
aberto e os dos últimos 90 dias.
