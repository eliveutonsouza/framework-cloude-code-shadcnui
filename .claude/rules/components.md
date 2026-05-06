---
paths:
  - "src/components/**/*.tsx"
---

# shadcn/ui
- Sempre usar CVA para variantes de componentes
- Usar `cn()` de @/lib/utils para classes condicionais
- Nunca hardcodar cores — usar tokens semânticos shadcn/ui
- `flex gap-*` em vez de `space-x-*`; `size-*` para dimensões iguais
- Dialog, Sheet, Drawer: sempre incluir Title (sr-only se visual oculto)
- Sempre incluir AvatarFallback em Avatar
- Ícones: atributo `data-icon` em botões; nunca string keys; sem classes de sizing em ícones dentro de componentes

# TypeScript
- Sempre exportar tipos de props (`export type ButtonProps = ...`)
- Sem `any` — usar `unknown` + type narrowing quando necessário
- Props opcionais com `?` e valor default no destructuring
- Usar `React.ComponentPropsWithoutRef<'button'>` para extensão de elementos nativos

# React (vercel-composition-patterns)
- Evitar boolean prop proliferation — preferir variantes CVA ou compound components
- Componentes puros quando possível; extrair lógica pesada para hooks customizados (`use*`)
- Nunca definir componentes dentro de outros componentes (destrói reconciliação)
- Usar React 19: `use()` para promises, `useOptimistic`, `useFormStatus` onde aplicável

# Acessibilidade
- Incluir atributos aria-* necessários em todos os componentes interativos
- `data-invalid` em Field e `aria-invalid` nos controles de formulário
