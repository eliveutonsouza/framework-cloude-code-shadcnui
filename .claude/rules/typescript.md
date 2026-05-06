---
paths:
  - "src/**/*.ts"
  - "src/**/*.tsx"
---

- TypeScript strict mode sempre ativo (`strict: true` no tsconfig)
- Sem `any` — usar `unknown` com type guards, `never` para casos impossíveis
- Sem `as` cast desnecessário — preferir type narrowing com `if`, `in`, `instanceof`
- Tipar retornos de funções explicitamente quando não óbvios
- Preferir `const` objects com `as const` em vez de `enum`
- Organização de imports: externas → internas (`@/`) → relativas (separados por linha em branco)
- Não usar `namespace` — usar ES modules
