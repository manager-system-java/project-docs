# 🚀 Rocket Manager — Front-end

> Sistema de Gestão de Projetos — Interface desenvolvida em Angular com TypeScript

**Repositório Front-end:** [manager-system-java](https://github.com/manager-system-java/manager-system-java)  
**Repositório Back-end:** [manager-system-java-backend-api](https://github.com/manager-system-java/manager-system-java-backend-api)

---

## 📋 Visão Geral

Interface web do **Rocket Manager**, um sistema de gestão de projetos que permite gerenciar usuários, projetos e equipes com controle de acesso baseado em perfis. O front-end consome a API REST do back-end e exibe apenas as funcionalidades permitidas para cada perfil de usuário.

---

## 🛠️ Tecnologias Utilizadas

- **Angular** — framework principal para construção da interface
- **TypeScript** — linguagem utilizada no desenvolvimento
- **VS Code** — editor de código
- **Axios / HttpClient** — comunicação com a API REST
- **JWT** — armazenado e enviado no header de cada requisição autenticada

---

## 🏗️ Arquitetura

```
┌─────────────────────────────────────┐
│         Front-end (Angular)         │
│                                     │
│  Pages → Components → Services      │
│                                     │
│  Guards de rota por Role            │
│  Interceptor JWT no header          │
└──────────────┬──────────────────────┘
               │ HTTP + JWT
               ▼
┌─────────────────────────────────────┐
│       API REST (Spring Boot)        │
│    http://localhost:8080            │
└─────────────────────────────────────┘
```
---

## 🔐 Fluxo de Autenticação no Front-end

```
1. Usuário preenche login e senha
            │
            ▼
2. Requisição POST para /auth/login
            │
            ▼
3. Token JWT recebido e salvo
            │
            ▼
4. Interceptor adiciona token em todas as requisições
   Authorization: Bearer {token}
            │
            ▼
5. Guard verifica role antes de exibir cada rota
            │
            ▼
6. Usuário vê apenas o que seu perfil permite
```

---

## 🖥️ Telas do Sistema

| Tela | Rota | Acesso |
|---|---|---|
| Login | `/login` | Público |
| Cadastro | `/cadastro` | Público |
| Dashboard | `/dashboard` | Todos autenticados |
| Usuários | `/usuarios` | ADMIN |
| Projetos | `/projetos` | Todos autenticados |
| Equipes | `/equipes` | Todos autenticados |

---

## 🚀 Como Rodar Localmente

### Pré-requisitos

- Node.js 18+
- Angular CLI instalado
- VS Code (recomendado)
- Back-end rodando em `http://localhost:8080`

### Passo a passo

**1. Clone o repositório**
```bash
git clone https://github.com/manager-system-java/manager-system-java.git
cd manager-system-java
```

**2. Instale as dependências**
```bash
npm install
```

**3. Configure a URL da API**

No arquivo `environment.ts`:
```typescript
export const environment = {
  production: false,
  apiUrl: 'http://localhost:8080'
};
```

**4. Rode o projeto**
```bash
ng serve
```

**5. Acesse no navegador:**
```
http://localhost:4200
```

> ⚠️ O back-end precisa estar rodando para o sistema funcionar

---

## 🗺️ Roadmap

- [x] Configuração inicial do projeto Angular
- [x] Tela de Login
- [x] Tela de Cadastro
- [x] Integração com autenticação JWT
- [ ] Dashboard principal
- [ ] Tela de Usuários
- [ ] Tela de Projetos
- [ ] Tela de Equipes
- [ ] Guards de rota por role
- [ ] Deploy (Netlify)

---

## 🌐 Deploy

### Em desenvolvimento — o deploy será realizado na plataforma **Netlify**

**Variável de ambiente necessária para produção:**
```
API_URL=url_da_api_em_producao
```

---

## 🔗 Repositórios

| Parte | Link |
|---|---|
| 🖥️ Back-end | [manager-system-java-backend-api](https://github.com/manager-system-java/manager-system-java-backend-api) |
| 🎨 Front-end | [manager-system-java](https://github.com/manager-system-java/manager-system-java) |

---

## Autora

Desenvolvido por **Letícia Castro**  
Projeto acadêmico — 2026
