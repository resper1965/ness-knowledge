---
tipo: politica
titulo: Catálogo de sistemas (provisório)
responsavel: TI
status: ativo
versao: 0.1
ultima_revisao: 2026-10-09
---

# Catálogo de sistemas (provisório)

Os sistemas que se pedem em **Pedido de acesso**, com o dono de cada um e os perfis comuns.

**A lista e os donos são provisórios.** Inclua os sistemas que faltam, troque os donos e tire `provisorio: true` por PR.

- **Dono:** aprova os pedidos de acesso ao sistema.
- **Quem executa:** sempre o TI, ou seja, quem decide em Governança.
- **ness.brain:** o sistema `ness_brain` é o próprio ness.brain. Os perfis dele são `módulo:nível` e entram sozinhos na
  aprovação.
- **Perfis:** guiam o pedido. Um perfil fora da lista gera só alerta.
- **`outro`:** serve para o que ainda não está no catálogo. Não tem dono, então a aprovação fica com o gestor e o TI.

```sistemas
provisorio: true
sistemas:
  - id: ness_brain
    nome: ness.brain
    dono: resper@ness.com.br
    tipo: brain
  - id: google_workspace
    nome: Google Workspace (e-mail, Drive, Agenda)
    dono: resper@ness.com.br
    perfis: [conta, grupo de e-mail, drive compartilhado]
  - id: omie
    nome: Omie (ERP)
    dono: rsalerno@ness.com.br
    perfis: [consulta, financeiro, administrador]
  - id: github
    nome: GitHub
    dono: resper@ness.com.br
    perfis: [leitura, escrita, administrador]
  - id: cloudflare
    nome: Cloudflare
    dono: resper@ness.com.br
    perfis: [leitura, administrador]
  - id: outro
    nome: Outro sistema
```
