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

- **quem**:
  - `gestor_do_solicitante`: o gestor direto no organograma (cadastro de colaboradores);
  - `rh`: quem tem o nível "decidir" no módulo Pessoas e RH. Enquanto ninguém tiver, decidem os administradores.
- **acao**:
  - `validar` e `aprovar` esperam uma decisão (seguir ou recusar com motivo);
  - `informar` avisa por e-mail e segue sozinho.
- Ninguém decide etapa do próprio pedido. Etapa do gestor de quem não tem gestor é dispensada e fica registrada.
- A trilha de cada pedido (quem, quando, decisão e comentário) não se altera nem se apaga.
