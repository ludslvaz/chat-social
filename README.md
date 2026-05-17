# Chat Social

Rede social com feed de posts, mensagens diretas e grupos com chat em tempo real.

Construída com **Next.js 16**, **Supabase** e **TypeScript**, seguindo o padrão arquitetural **MVVM** adaptado para React.

---

## Stack

| Camada | Tecnologia |
|---|---|
| Framework | Next.js 16 (App Router, Turbopack) |
| Backend / Banco | Supabase (PostgreSQL + Auth + Storage + Realtime) |
| Linguagem | TypeScript 5 |
| Estilização | Tailwind CSS 4 |
| Componentes UI | shadcn/ui + Radix UI |
| Ícones | Lucide React |

---

## Pré-requisitos

- Node.js 18+
- Uma conta no [Supabase](https://supabase.com) com projeto criado

---

## Instalação

```bash
# 1. Clone o repositório
git clone git@github.com:ludslvaz/chat-social.git
cd chat-social

# 2. Instale as dependências
npm install

# 3. Configure as variáveis de ambiente
cp .env.example .env.local
# Edite .env.local com suas chaves do Supabase

# 4. Rode o servidor de desenvolvimento
npm run dev
```

Acesse [http://localhost:3000](http://localhost:3000).

---

## Variáveis de Ambiente

Crie um arquivo `.env.local` na raiz do projeto com:

```bash
NEXT_PUBLIC_SUPABASE_URL=sua_url_aqui
NEXT_PUBLIC_SUPABASE_ANON_KEY=sua_chave_anon_aqui
```

> **Nunca versione o `.env.local`.** Use `.env.example` (sem valores) como referência para outros devs.

---

## Scripts

| Comando | Descrição |
|---|---|
| `npm run dev` | Servidor de desenvolvimento com Turbopack |
| `npm run build` | Build de produção |
| `npm start` | Inicia o servidor de produção |
| `npm run lint` | Lint com ESLint |

---

## Funcionalidades

- **Feed** — criação, curtida e exclusão de posts em tempo real
- **Mensagens Diretas** — chat 1:1 com atualização automática via Supabase Realtime
- **Grupos** — criação de grupos, ingresso/saída e chat coletivo em tempo real
- **Perfil** — edição de nome, bio e avatar (upload para Supabase Storage)
- **Autenticação** — registro e login com e-mail/senha via Supabase Auth

---

## Arquitetura

O projeto segue o padrão **MVVM**:

```
View        →  _components/     (UI pura, sem lógica)
ViewModel   →  hooks/           (estado + lógica de cada página)
Model       →  lib/types.ts     (contratos de dados)
             + lib/services/    (acesso ao Supabase)
```

Cada página tem um hook dedicado (ex: `useFeedPage`, `useGroupChatPage`) que encapsula todo o estado e os efeitos colaterais. Os componentes recebem dados e callbacks via props e não conhecem o Supabase diretamente.

Consulte [`ARCHITECTURE.md`](./ARCHITECTURE.md) para a documentação completa da arquitetura, fluxo de dados, autenticação, Realtime e loading.

---

## Estrutura de Pastas (resumo)

```
chat-social/
├── middleware.ts              # Proteção de rotas (redireciona para /auth/login)
└── app/
    ├── not-found.tsx          # Página 404 global
    ├── (app)/                 # Rotas protegidas (feed, grupos, mensagens, perfil)
    ├── auth/                  # Rotas públicas (login, registro)
    ├── context/               # AppProvider — supabase, user, profile globais
    ├── hooks/                 # ViewModels — um hook por página
    ├── _components/           # Componentes de UI organizados por domínio
    │   └── ui/skeletons.tsx   # Skeletons de carregamento por tela
    └── lib/
        ├── types.ts           # Tipos centrais de domínio
        ├── supabase/          # Clientes browser e server
        └── services/          # Acesso ao banco de dados
```
