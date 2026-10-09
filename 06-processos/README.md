---
titulo: Processos do ness.brain
responsavel: Diretoria
status: ativo
versao: 1.0
ultima_revisao: 2026-10-09
---

# Processos do ness.brain

Os processos que rodam no ness.brain (módulo Pessoas e RH e, depois, Comercial e Governança) são definidos aqui, um
arquivo por processo. O ness.brain lê o bloco ` ```processo ` de cada arquivo (cópia de até 10 minutos) e roda os
pedidos com essa definição. Mudar um processo é um PR neste repositório, sem deploy. Pedido que já começou segue com a
definição com que começou.

## Formato

````markdown
```processo
id: ferias                 # identificador estável (minúsculas)
nome: Pedido de férias
modulo: rh
prefixo: FER               # número do pedido: FER-0001
calculadora: ferias        # pré-checagem determinística (opcional)
descricao: ...
campos:                    # formulário: data, inteiro, sim_nao ou texto
  - { chave: inicio, nome: Início, tipo: data, obrigatorio: true, ajuda: ... }
etapas:                    # em ordem
  - { id: rh_valida, nome: RH confere o direito, quem: rh, acao: validar }
  - { id: gestor_aprova, nome: Gestor aprova, quem: gestor_do_solicitante, acao: aprovar }
  - { id: rh_informado, nome: RH é informado, quem: rh, acao: informar }
```
````

- **campos**:
  - `tipo`: `data`, `inteiro`, `sim_nao`, `texto`, `pessoa` (alguém do cadastro), `email` (um e-mail novo
    @ness.com.br) ou `lista` (com `opcoes`);
  - `so_clt`: o campo só aparece para quem é CLT.
  - No campo `pessoa` (de quem o pedido trata), o gestor escolhe alguém da própria equipe; o RH escolhe qualquer pessoa.
- **abre**: `todos` (qualquer colaborador), `gestores` (quem tem equipe, ou o RH) ou `rh`.
- **efeito** (opcional): o que muda no cadastro de colaboradores quando o pedido é aprovado (`admissao`,
  `desligamento` ou `alteracao`). A mudança fica na trilha do pedido.
- **quem**:
  - `gestor_do_solicitante`: o gestor direto de quem pede, no organograma (cadastro de colaboradores);
  - `gestor_da_pessoa`: o gestor de quem o pedido trata (campo `pessoa`; na admissão, o gestor escolhido);
  - `rh`: quem tem o nível "decidir" no módulo Pessoas e RH. Enquanto ninguém tiver, decidem os administradores;
  - `diretoria`: quem tem o papel de diretoria (grupo Diretoria).
- **acao**:
  - `validar` e `aprovar` esperam uma decisão (seguir ou recusar com motivo);
  - `informar` avisa por e-mail e segue sozinho.
- Ninguém decide etapa do próprio pedido. A etapa do gestor é dispensada, e isso fica registrado, quando não há gestor
  ou quando quem pede é o próprio gestor.

## Processos

| Arquivo | Processo | Quem abre | Caminho |
| --- | --- | --- | --- |
| `ferias.md` | Pedido de férias | todos | RH confere → gestor aprova → RH informado |
| `admissao.md` | Admissão | gestores e RH | diretoria aprova → RH confere → entra no cadastro |
| `desligamento.md` | Desligamento | gestores e RH | RH confere → diretoria aprova → gestor informado → saída no cadastro |
| `alteracao.md` | Alteração de cadastro | gestores e RH | gestor da pessoa aprova → RH confere → cadastro muda |
- A trilha de cada pedido (quem, quando, decisão e comentário) não se altera nem se apaga.
