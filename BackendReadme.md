# 🚀 Rocket Manager — Back-end API

> Sistema de Gestão de Projetos — API REST desenvolvida em Java com Spring Boot

**Repositório Front-end:** [manager-system-java](https://github.com/manager-system-java/manager-system-java)  
**Repositório Back-end:** [manager-system-java-backend-api](https://github.com/manager-system-java/manager-system-java-backend-api)

---

## 📋 Visão Geral

O **Rocket Manager** é um sistema web de gestão de projetos que permite gerenciar usuários, projetos e equipes com controle de acesso baseado em perfis. O sistema garante que cada usuário acesse apenas o que seu perfil permite.

### Perfis de acesso

| Perfil | Permissões |
|---|---|
| 👑 Administrador | Acesso total — gerencia usuários e as suas permissões|
| 📋 Gerente | Gerencia seus próprios projetos e equipes |
| 👤 Colaborador | Visualização dos projetos e equipes que participa |

---

## 🛠️ Tecnologias Utilizadas

- **Java** — linguagem principal
- **Spring Boot** — framework para construção da API REST
- **Spring Security** — autenticação e controle de acesso
- **JWT (JSON Web Token)** — autenticação stateless
- **PostgreSQL** no banco de dados **Supabase** — banco de dados relacional
- **Spring Data JPA / Hibernate** — mapeamento objeto-relacional
- **Lombok** — redução de código repetitivo
- **Maven** — gerenciador de dependências
- **Insomnia** — testes dos endpoints durante o desenvolvimento

---

## 🏗️ Arquitetura do Sistema

```
┌─────────────────────────────────────┐
│         Front-end (Angular)         │
│         TypeScript + Angular        │
└──────────────┬──────────────────────┘
               │ HTTP + JWT
               ▼
┌─────────────────────────────────────┐
│          API REST (Spring Boot)     │
│                                     │
│  Controller → Service → Repository  │
│                                     │
│  Spring Security + JWT Filter       │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│            PostgreSQL               │
│         Banco de Dados              │
└─────────────────────────────────────┘
```

---
## 🗄️ Diagrama do Banco de Dados

```
┌─────────────────────┐       ┌─────────────────────┐
│        USER         │       │       PROJECT        │
├─────────────────────┤       ├─────────────────────┤
│ id (PK)             │       │ id (PK)             │
│ name                │       │ name                │
│ email (unique)      │       │ description         │
│ cpf (unique)        │◄──────│ manager_id (FK)     │
│ password            │       │ start_date          │
│ cargo               │       │ end_date            │
│ role                │       │ status              │
│ active              │       └──────────┬──────────┘
└──────────┬──────────┘                  │
           │                             │
           │    ┌────────────────────┐   │
           │    │        TEAM        │   │
           │    ├────────────────────┤   │
           └────│ id (PK)            │───┘
  N:N           │ name               │  N:N
           ┌────│ description        │────┐
           │    └────────────────────┘    │
           │                              │
    ┌──────▼──────┐            ┌──────────▼──────┐
    │  TEAM_USER  │            │  TEAM_PROJECT   │
    │  (N:N)      │            │  (N:N)          │
    └─────────────┘            └─────────────────┘
```

---

## 🔐 Fluxo de Autenticação

```
1. Usuário faz login (POST /auth/login)
            │
            ▼
2. Sistema valida email + senha (BCrypt)
            │
            ▼
3. Sistema gera token JWT com as roles do usuário
            │
            ▼
4. Token é retornado para o front-end
            │
            ▼
5. Front-end envia o token no header de cada requisição
   Authorization: Bearer {token}
            │
            ▼
6. SecurityFilter valida o token
            │
            ▼
7. Acesso liberado ou negado (403)
```

## 🗺️ Roadmap

- [x] Autenticação com JWT
- [x] Cadastro e login de usuários
- [x] Controle de acesso por roles (ADMIN, GERENTE, COLABORADOR)
- [x] CRUD completo de Usuários
- [x] CRUD completo de Projetos
- [x] CRUD completo de Equipes
- [X] Deploy da API (Railway)
- [x] Documentação
- [x] Integração com Front-end

---

## 🌐 Deploy 
[Rocket Manager](https://gestao-de-projetos.netlify.app/)

---

## Autora
Desenvolvido por **Letícia Castro**  
Projeto acadêmico — 2026
