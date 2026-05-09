# 📚 Rocket Manager — Documentação Geral

> Gestão de Projetos — Documentação completa do sistema

---

## 🔗 Repositórios

| Parte | Link |
|---|---|
| 🖥️ Back-end | [manager-system-java-backend-api](https://github.com/manager-system-java/manager-system-java-backend-api) |
| 🎨 Front-end | [manager-system-java](https://github.com/manager-system-java/manager-system-java) |

---

## 📋 Sobre o Projeto

O **Rocket Manager** é um sistema web de gestão de projetos desenvolvido como projeto acadêmico. O sistema permite gerenciar usuários, projetos e equipes com controle de acesso baseado em perfis, garantindo que cada usuário acesse apenas o que é permitido para seu nível.

O projeto foi desenvolvido seguindo boas práticas do mercado, como:
- Arquitetura MVC
- Autenticação stateless com JWT
- Versionamento de código com Git
- Organização por sprints com metodologia ágil
- Separação entre front-end e back-end

---

## 🛠️ Stack Completa

### Back-end
| Tecnologia | Uso |
|---|---|
| Java | Linguagem principal |
| Spring Boot | Framework da API REST |
| Spring Security | Autenticação e autorização |
| JWT | Tokens de acesso |
| PostgreSQL | Banco de dados |
| Spring Data JPA | Mapeamento objeto-relacional |
| Lombok | Redução de boilerplate |
| Maven | Gerenciador de dependências |

### Front-end
| Tecnologia | Uso |
|---|---|
| Angular | Framework de interface |
| TypeScript | Linguagem do front-end |
| VS Code | Editor de código |

### Ferramentas
| Ferramenta | Uso |
|---|---|
| IntelliJ IDEA | IDE do back-end |
| GitHub | Versionamento de código |
| Trello | Organização das sprints |
| Insomnia | Testes dos endpoints |
| Railway | Deploy do back-end e banco |
| Netlify | Deploy do front-end |

---

## 🏗️ Arquitetura Geral

```
┌─────────────────────────────────────┐
│         Front-end (Angular)         │
│           Netlify                   │
└──────────────┬──────────────────────┘
               │ HTTPS + JWT
               ▼
┌─────────────────────────────────────┐
│       API REST (Spring Boot)        │
│            Railway                  │
│                                     │
│  Controller → Service → Repository  │
│  Spring Security + JWT Filter       │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│            PostgreSQL               │
│            Railway                  │
└─────────────────────────────────────┘
```

---

## 👥 Perfis de Acesso

### 👑 Administrador
- Acesso total ao sistema
- Cadastra e gerencia todos os usuários
- Cria projetos e define gerentes responsáveis
- Cria equipes e gerencia membros

### 📋 Gerente
- Gerencia seus próprios projetos
- Visualiza e edita equipes
- Acompanha o status dos projetos

### 👤 Colaborador
- Visualiza os projetos que participa
- Visualiza sua equipe
- Acesso somente leitura

---

## 🗄️ Modelo de Dados

```
┌─────────────────────┐       ┌─────────────────────┐
│        USER         │       │       PROJECT        │
├─────────────────────┤       ├─────────────────────┤
│ id (PK)             │       │ id (PK)             │
│ name                │       │ name                │
│ email (unique)      │       │ description         │
│ cpf (unique)        │◄──────│ manager_id (FK)     │
│ password (BCrypt)   │       │ start_date          │
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

### Status dos Projetos
```
PLANEJADO → EM_ANDAMENTO → CONCLUIDO
                         → CANCELADO
```

---

## 🔐 Segurança

### Por que a segurança foi prioridade?

O sistema lida com dados pessoais sensíveis: **CPF, e-mail e senha**. Por isso, foram implementadas as seguintes camadas de segurança:

**1. Criptografia de senha (BCrypt)**
```
Senha original: "minhasenha123"
                      ↓
Senha no banco: "$2a$10$xKp8b..."  (hash irreversível)
```

**2. Autenticação com JWT**
```
Login → Token gerado → Token enviado em cada requisição
```

**3. Controle de acesso por roles**
```
Requisição recebida
        ↓
SecurityFilter valida o token
        ↓
Verifica a role do usuário
        ↓
Libera ou bloqueia o acesso (403)
```

---

## 📡 Endpoints da API

### Públicos (sem autenticação)
```
POST /auth/register    → Cadastro de usuário
POST /auth/login       → Login e geração do token
```

### Protegidos (requer token JWT)
```
# Usuários
GET    /usuarios              → Lista todos (ADMIN)
GET    /usuarios/{id}         → Busca por ID (ADMIN)
PUT    /usuarios/{id}         → Edita usuário (ADMIN)
DELETE /usuarios/{id}         → Desativa usuário (ADMIN)

# Projetos
POST   /projetos              → Cria projeto (ADMIN)
GET    /projetos              → Lista projetos (todos)
GET    /projetos/{id}         → Detalha projeto (todos)
PUT    /projetos/{id}         → Edita projeto (ADMIN, GERENTE)
PATCH  /projetos/{id}/status  → Altera status (ADMIN, GERENTE)

# Equipes
POST   /equipes                          → Cria equipe (ADMIN, GERENTE)
GET    /equipes                          → Lista equipes (todos)
GET    /equipes/{id}                     → Detalha equipe (todos)
POST   /equipes/{id}/membros             → Adiciona membro (ADMIN)
DELETE /equipes/{id}/membros/{userId}    → Remove membro (ADMIN)
POST   /equipes/{id}/projetos/{projetoId} → Vincula ao projeto (ADMIN, GERENTE)
```

---

## 🗺️ Roadmap do Projeto

### Back-end
- [x] Configuração do Spring Boot e Spring Security
- [x] Autenticação com JWT
- [x] Cadastro e login de usuários
- [x] Controle de acesso por roles
- [ ] CRUD completo de Usuários
- [ ] CRUD completo de Projetos
- [ ] CRUD completo de Equipes
- [ ] Deploy no Railway
- [ ] Documentação Swagger

### Front-end
- [x] Configuração do projeto Angular
- [x] Tela de Login
- [x] Tela de Cadastro
- [ ] Dashboard
- [ ] Tela de Usuários
- [ ] Tela de Projetos
- [ ] Tela de Equipes
- [ ] Guards por role
- [ ] Deploy no Netlify

---

## 🚀 Como Rodar o Projeto Completo

### 1. Sobe o banco de dados
```sql
CREATE DATABASE manager_system;
```

### 2. Sobe o back-end
```bash
git clone https://github.com/manager-system-java/manager-system-java-backend-api.git
cd manager-system-java-backend-api
# configure o application.properties
mvn spring-boot:run
# API disponível em http://localhost:8080
```

### 3. Sobe o front-end
```bash
git clone https://github.com/manager-system-java/manager-system-java.git
cd manager-system-java
npm install
ng serve
# Interface disponível em http://localhost:4200
```

---

## 📅 Organização das Sprints

| Sprint | Descrição | Status |
|---|---|---|
| Sprint 1 | Back-end: Login, registro, JWT e Roles | ✅ Concluída |
| Sprint 2 | Back-end: CRUD de Usuários | 🔄 Em andamento |
| Sprint 3 | Back-end: CRUD de Projetos | ⏳ Pendente |
| Sprint 4 | Back-end: CRUD de Equipes | ⏳ Pendente |
| Sprint 5 | Banco de Dados em Produção | ⏳ Pendente |
| Sprint 6 | Deploy do Back-end | ⏳ Pendente |
| Sprint 7 | Front-end | ⏳ Pendente |
| Sprint 8 | Deploy do Front-end | ⏳ Pendente |
| Sprint 9 | Documentação e Qualidade | ⏳ Pendente |

---

## 👩‍💻 Autora

Desenvolvido por **Letícia Castro**  
Projeto acadêmico — 2026
