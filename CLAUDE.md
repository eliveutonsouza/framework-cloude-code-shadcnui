# Framework Claude Code — Design System

## Project Overview

SPA (Single Page Application) com design system completo baseado em shadcn/ui. Stack: Vite 6 + React 19 + TypeScript strict + Tailwind CSS v4 + Supabase JS v2.

## Tech Stack

- **Framework**: Vite 6 + React 19 (SPA — sem SSR)
- **Linguagem**: TypeScript 5.x com `strict: true`
- **UI**: shadcn/ui com Tailwind CSS v4
- **Banco de dados**: Supabase JS v2 (`@supabase/supabase-js`)
- **Ícones**: Lucide React (padrão shadcn)
- **Variantes**: class-variance-authority (CVA)
- **Classes condicionais**: `cn()` de `@/lib/utils`

## Build & Dev Commands

```bash
npm run dev       # Inicia servidor de desenvolvimento (Vite)
npm run build     # Build de produção
npm run preview   # Preview do build de produção
npm run test      # Executa testes
npm run typecheck # npx tsc --noEmit
```

## Project Architecture

```
src/
├── components/
│   ├── ui/          # Componentes shadcn/ui (gerados pelo CLI)
│   └── ...          # Componentes de produto (compostos de ui/)
├── hooks/           # Custom hooks (use*)
├── lib/
│   ├── utils.ts     # cn(), helpers
│   └── supabase/
│       └── client.ts  # Instância única do Supabase client
├── pages/           # Páginas da aplicação
├── types/           # Tipos TypeScript globais e DB types
└── context/         # React contexts (Auth, Theme, etc.)
```

## Design System — shadcn/ui

### Regras críticas (SKILL.md oficial)

- Sempre rodar `npx shadcn@latest info` antes de qualquer trabalho de componentes
- Usar `npx shadcn@latest search` antes de criar componentes customizados
- Usar `--dry-run` e `--diff` antes de qualquer `npx shadcn@latest add`
- Sempre rodar `npx shadcn@latest docs <component>` antes de criar ou corrigir
- Init para Vite: `npx shadcn@latest init --template vite --preset nova`

### Tokens e estilo

- **Nunca** hardcodar cores — usar tokens semânticos: `bg-primary`, `text-muted-foreground`, `border-destructive`, etc.
- `flex gap-*` em vez de `space-x-*` ou `space-y-*`
- `size-*` para dimensões iguais (largura = altura)
- `className` apenas para layout; nunca sobrescrever cores/tipografia do componente

### Componentes

- CVA para todas as variantes de componentes
- Dialog, Sheet, Drawer: **sempre** incluir `Title` (usar `sr-only` se visualmente oculto)
- Sempre incluir `AvatarFallback` em Avatar
- Ícones: atributo `data-icon` em botões; nunca string keys; sem classes de sizing dentro de componentes
- `isRSC=false` neste projeto (Vite SPA) — não adicionar `"use client"` desnecessariamente

### Formulários

- `FieldGroup` + `Field` para layouts de formulário (não raw divs)
- `data-invalid` em `Field` e `aria-invalid` nos controles
- `ToggleGroup` para conjuntos de 2 a 7 opções

## TypeScript — Boas Práticas

- `strict: true` sempre ativo no tsconfig
- Sem `any` — usar `unknown` com type guards, `never` para casos impossíveis
- Sem `as` cast desnecessário — preferir type narrowing com `if`, `in`, `instanceof`
- Exportar tipos de props explicitamente: `export type ButtonProps = ...`
- Props opcionais com `?` e default no destructuring
- `React.ComponentPropsWithoutRef<'button'>` para extensão de elementos nativos
- Preferir `const` objects com `as const` em vez de `enum`
- Imports: externas → internas (`@/`) → relativas (separados por linha em branco)

## React — Boas Práticas

- Evitar boolean prop proliferation — preferir variantes CVA ou compound components
- Extrair lógica pesada para hooks customizados (`use*`)
- **Nunca** definir componentes dentro de outros componentes (quebra reconciliação)
- React 19: usar `use()` para promises, `useOptimistic`, `useFormStatus` onde aplicável
- Componentes puros sempre que possível (mesmas props → mesmo output)
- Evitar waterfalls de data fetching — buscar dados em paralelo

## Supabase (SPA Client-side)

- Client instanciado **uma única vez** em `src/lib/supabase/client.ts`
- Variáveis via `import.meta.env.VITE_SUPABASE_URL` e `VITE_SUPABASE_ANON_KEY`
- **Nunca** expor `service_role` key no cliente
- Auth state via `supabase.auth.onAuthStateChange()` em contexto React
- Tipos gerados com: `supabase gen types typescript --project-id <id> > src/types/database.ts`
- RLS ativo em todas as tabelas — nunca bypassar no cliente

## Acessibilidade

- WCAG 2.1 AA mínimo
- Semântica HTML correta em todos os componentes
- Foco visível em todos os elementos interativos
- Contraste de cores ≥ 4.5:1 (texto normal) e ≥ 3:1 (texto grande)
- `aria-*` corretos; `data-invalid` + `aria-invalid` nos formulários

## Naming Conventions

- Componentes: `PascalCase` (Button, UserCard, DashboardLayout)
- Hooks: `camelCase` com prefixo `use` (useAuth, useTheme)
- Variantes CVA: `kebab-case` (primary, outline, ghost)
- Props: `camelCase` (onClick, isDisabled, ariaLabel)
- Arquivos de componentes: `PascalCase.tsx`
- Arquivos de hooks/utils: `camelCase.ts`

## Agent Teams

Este projeto usa **Agent Teams** (experimental) do Claude Code. Ative com:
```json
{ "env": { "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1" } }
```

### Especialistas disponíveis em `.claude/agents/`

| Agente | Especialidade | Quando acionar |
|---|---|---|
| `ui-designer` | UI/UX, Figma → código, responsividade | Design de novas telas, revisão visual |
| `design-system` | shadcn/ui, CVA, Tailwind v4, tokens | Novos componentes, arquitetura do DS |
| `database-architect` | Supabase, RLS, migrations, tipos TS | Schema, auth, queries, performance |
| `accessibility-auditor` | WCAG 2.1 AA, ARIA, contraste, teclado | Auditoria de acessibilidade |

### Skills ativas (instaladas em `.claude/skills/`)

- `supabase` + `supabase-postgres-best-practices` — melhores práticas Supabase
- `shadcn` — CLI, componentes, variantes (ativa com `components.json`)
- `vercel-react-best-practices` — performance React, evitar waterfalls
- `vercel-composition-patterns` — compound components, React 19, sem boolean props
- `web-design-guidelines` — 100+ regras de UI: a11y, formulários, dark mode

## Git Workflow

- Branch naming: `feat/component-name`, `fix/issue-description`, `chore/task`
- Commits convencionais: `feat:`, `fix:`, `chore:`, `docs:`, `refactor:`
- Exemplo: `git commit -m "feat: add Button component with CVA variants"`
