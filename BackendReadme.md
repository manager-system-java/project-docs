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
| 👑 Administrador | Acesso total — gerencia usuários, projetos e equipes |
| 📋 Gerente | Gerencia seus próprios projetos e equipes |
| 👤 Colaborador | Visualização dos projetos e equipes que participa |

---

## 🛠️ Tecnologias Utilizadas

- **Java** — linguagem principal
- **Spring Boot** — framework para construção da API REST
- **Spring Security** — autenticação e controle de acesso
- **JWT (JSON Web Token)** — autenticação stateless
- **PostgreSQL** — banco de dados relacional
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

---

## 🚀 Como Rodar Localmente

### Pré-requisitos

- Java 17+
- Maven
- PostgreSQL instalado e rodando
- IntelliJ IDEA (recomendado)

### Passo a passo

**1. Clone o repositório**
```bash
git clone https://github.com/manager-system-java/manager-system-java-backend-api.git
cd manager-system-java-backend-api
```

**2. Configure o banco de dados**

Crie um banco no PostgreSQL:
```sql
CREATE DATABASE manager_system;
```

**3. Configure o `application.properties`**
```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/manager_system
spring.datasource.username=seu_usuario
spring.datasource.password=sua_senha
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect

api.security.token.secret=sua_chave_secreta_jwt
```

**4. Rode o projeto**

Pelo IntelliJ: clique em **Run** na classe principal

Ou pelo terminal:
```bash
mvn spring-boot:run
```

**5. A API estará disponível em:**
```
http://localhost:8080
```

---

## 📡 Endpoints Disponíveis

### Autenticação (público)

| Método | Endpoint | Descrição |
|---|---|---|
| POST | `/auth/register` | Cadastrar novo usuário |
| POST | `/auth/login` | Fazer login e receber token |

### Usuários (autenticado)

| Método | Endpoint | Descrição | Role |
|---|---|---|---|
| GET | `/usuarios` | Listar todos | ADMIN |
| GET | `/usuarios/{id}` | Buscar por ID | ADMIN |
| PUT | `/usuarios/{id}` | Editar usuário | ADMIN |
| DELETE | `/usuarios/{id}` | Desativar usuário | ADMIN |

### Projetos (autenticado)

| Método | Endpoint | Descrição | Role |
|---|---|---|---|
| POST | `/projetos` | Criar projeto | ADMIN |
| GET | `/projetos` | Listar projetos | Todos |
| GET | `/projetos/{id}` | Detalhar projeto | Todos |
| PUT | `/projetos/{id}` | Editar projeto | ADMIN, GERENTE |
| PATCH | `/projetos/{id}/status` | Alterar status | ADMIN, GERENTE |

### Equipes (autenticado)

| Método | Endpoint | Descrição | Role |
|---|---|---|---|
| POST | `/equipes` | Criar equipe | ADMIN, GERENTE |
| GET | `/equipes` | Listar equipes | Todos |
| GET | `/equipes/{id}` | Detalhar equipe | Todos |
| POST | `/equipes/{id}/membros` | Adicionar membro | ADMIN |
| DELETE | `/equipes/{id}/membros/{userId}` | Remover membro | ADMIN |

---

## 🗺️ Roadmap

- [x] Autenticação com JWT
- [x] Cadastro e login de usuários
- [x] Controle de acesso por roles (ADMIN, GERENTE, COLABORADOR)
- [ ] CRUD completo de Usuários
- [ ] CRUD completo de Projetos
- [ ] CRUD completo de Equipes
- [ ] Deploy da API (Railway)
- [ ] Documentação Swagger
- [ ] Integração com Front-end

---

## 🌐 Deploy

### Em desenvolvimento — o deploy será realizado na plataforma **Railway**

**Variáveis de ambiente necessárias para produção:**
```
DATABASE_URL=
DATABASE_USERNAME=
DATABASE_PASSWORD=
JWT_SECRET=
```

---

## Autora
Desenvolvido por **Letícia Castro**  
Projeto acadêmico — 2026
