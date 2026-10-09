---
titulo: Pedido de acesso
responsavel: TI
status: ativo
versao: 1.0
ultima_revisao: 2026-10-09
---

# Pedido de acesso

Dar a alguém acesso a um sistema da ness.: a pessoa pede para si ou o gestor pede para alguém da equipe. O RH pode pedir
para qualquer pessoa. Aprovam o gestor da pessoa e o dono do sistema, e o TI cria o acesso e confirma. No próprio
ness.brain, a permissão entra sozinha na aprovação, sem a etapa do TI.

```processo
id: acesso
nome: Pedido de acesso
modulo: rh
prefixo: ACE
abre: todos
efeito: acesso
calculadora: acesso
descricao: Você pede acesso a um sistema para você ou para alguém da sua equipe; o gestor da pessoa e o dono do sistema aprovam; o TI cria e confirma. No ness.brain, o acesso entra sozinho na aprovação.
campos:
  - chave: pessoa
    nome: Para quem
    tipo: pessoa
    obrigatorio: true
    ajuda: você ou alguém da sua equipe
  - chave: sistema
    nome: Sistema
    tipo: sistema
    obrigatorio: true
    ajuda: do catálogo de sistemas
  - chave: perfil
    nome: Perfil
    tipo: texto
    obrigatorio: true
    ajuda: o que precisa (no ness.brain, módulo:nível, como financas:ver)
  - chave: motivo
    nome: Para quê
    tipo: texto
    obrigatorio: true
    ajuda: fica na trilha e na revisão de acessos
etapas:
  - id: gestor_aprova
    nome: Gestor da pessoa aprova
    quem: gestor_da_pessoa
    acao: aprovar
  - id: dono_aprova
    nome: Dono do sistema aprova
    quem: dono_sistema
    acao: aprovar
  - id: ti_executa
    nome: TI cria o acesso e confirma
    quem: ti
    acao: validar
```

## Pré-checagem (calculadora `acesso`)

- **Impedimentos:**
  - a pessoa não está ativa no cadastro;
  - o sistema está fora do catálogo (`sistemas.md`);
  - no ness.brain, o perfil não está no formato `módulo:nível`. Os módulos são financas, rh e governanca, e os níveis
    são ver, operar e decidir. **Administrar** e o papel de administrador só um administrador dá, em Administração ›
    Pessoas.
- **Alertas:**
  - o perfil está fora da lista de perfis do sistema no catálogo;
  - a pessoa já tem esse acesso no registro;
  - no ness.brain, a pessoa já tem o nível pedido, direto ou por grupo.

## Regras

- **Quem pede:** a própria pessoa, o gestor (para alguém da equipe, direta ou indireta) ou o RH (para qualquer pessoa).
- **Dono do sistema:** vem do catálogo. A etapa dele é dispensada quando é ele mesmo quem pede ou quando o sistema não
  tem dono.
- **TI:** quem tem o nível "decidir" em Governança. Enquanto ninguém tiver, decidem os administradores.
- **Aprovado:**
  - o acesso entra no registro de acessos (quem, qual sistema, qual perfil, o pedido e quem confirmou);
  - no ness.brain, a permissão entra direto na pessoa e já vale;
  - nos sistemas de fora, quem cria a conta é o TI, e o ness.brain não toca nesses sistemas.
- **Revisão:** todo acesso do registro entra na revisão periódica (ver `revogacao.md`).
