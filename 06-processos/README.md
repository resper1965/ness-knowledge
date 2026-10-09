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
    @ness.com.br), `lista` (com `opcoes`), `arquivo` (anexo PDF ou imagem, guardado pelo hash SHA-256), `valor`
    (dinheiro em reais, guardado em centavos; `min` e `max` em reais) ou `omie` (um item do cadastro do Omie, com
    `fonte: departamentos` ou `fonte: projetos`; só rótulo, nada é gravado no Omie) ou `sistema` (um sistema do catálogo
    `sistemas.md`);
  - `so_clt`: o campo só aparece para quem é CLT.
  - No campo `pessoa` (de quem o pedido trata), o gestor escolhe alguém da própria equipe; o RH escolhe qualquer pessoa.
- **abre**: `todos` (qualquer colaborador), `gestores` (quem tem equipe, ou o RH), `rh`, `comercial` (quem opera o
  Comercial) ou `governanca` (quem opera a Governança).
- **modulo**: `rh`, `comercial` ou `governanca`.
- **efeito** (opcional): o que muda quando o pedido é aprovado: o cadastro de colaboradores (`admissao`,
  `desligamento` ou `alteracao`), os acessos (`acesso` ou `revogacao`) ou as evidências do SGSI (`politica`). A
  mudança fica na trilha do pedido.
- **quem**:
  - `gestor_do_solicitante`: o gestor direto de quem pede, no organograma (cadastro de colaboradores);
  - `gestor_da_pessoa`: o gestor de quem o pedido trata (campo `pessoa`; na admissão, o gestor escolhido);
  - `rh`: quem tem o nível "decidir" no módulo Pessoas e RH. Enquanto ninguém tiver, decidem os administradores;
  - `heads`: quem tem o papel de heads (grupo Heads, antes chamado Diretoria; `diretoria` continua aceito);
  - `financeiro`: quem tem o nível "decidir" no módulo Finanças. Enquanto ninguém tiver, decidem os administradores;
  - `dono_sistema`: o dono do sistema pedido, no catálogo `sistemas.md` (dispensada sem dono ou quando é ele quem pede);
  - `comercial`: quem tem o nível "decidir" no Comercial (o diretor comercial). Enquanto ninguém tiver, decidem os
    administradores;
  - `ti`: quem tem o nível "decidir" em Governança. Enquanto ninguém tiver, decidem os administradores. Nos pedidos do
    próprio ness.brain, a etapa do TI é dispensada porque o ness.brain aplica sozinho;
  - `sgsi`: o dono do SGSI, quem tem o nível "administrar" em Governança. Enquanto ninguém tiver, decidem os
    administradores.
- **acao**:
  - `validar` e `aprovar` esperam uma decisão (seguir ou recusar com motivo);
  - `informar` avisa por e-mail e segue sozinho.
- Ninguém decide etapa do próprio pedido. A etapa do gestor é dispensada, e isso fica registrado, quando não há gestor
  ou quando quem pede é o próprio gestor.

## Processos

| Arquivo | Processo | Quem abre | Caminho |
| --- | --- | --- | --- |
| `ferias.md` | Pedido de férias | todos | RH confere → gestor aprova → RH informado |
| `admissao.md` | Admissão | gestores e RH | heads aprovam → RH confere → entra no cadastro |
| `desligamento.md` | Desligamento | gestores e RH | RH confere → heads aprovam → gestor informado → saída no cadastro |
| `alteracao.md` | Alteração de cadastro | gestores e RH | gestor da pessoa aprova → RH confere → cadastro muda |
| `ausencia.md` | Atestado ou licença | todos | RH confere → gestor informado |
| `acesso.md` | Pedido de acesso | todos | gestor da pessoa aprova → dono do sistema aprova → TI cria (no ness.brain, aplica sozinho) |
| `revogacao.md` | Revogação de acesso | todos (e sozinho no desligamento e na revisão) | TI retira |
| `desconto.md` | Aprovação de desconto | quem opera o Comercial | diretor comercial aprova → Heads (só acima do limite de `politica-comercial.md`) |
| `reembolso.md` | Reembolso de despesa | todos | gestor aprova → Financeiro confere e paga (política em `politica-despesas.md`) |
| `incidente.md` | Incidente de segurança | todos | TI trata → dono do SGSI encerra com a lição aprendida |
| `excecao.md` | Exceção a controle | todos | gestor aprova → dono do SGSI aceita o risco até a validade |
| `politica.md` | Aprovação de política | quem opera a Governança | dono do SGSI aprova → Heads informados → vira evidência de 5.1 e dos controles citados |
- A trilha de cada pedido (quem, quando, decisão e comentário) não se altera nem se apaga.
