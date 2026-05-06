---
paths:
  - "src/lib/supabase/**"
---

- Usar `@supabase/supabase-js` (SPA Vite — sem SSR)
- Instanciar client uma única vez em src/lib/supabase/client.ts
- Variáveis de ambiente via `import.meta.env.VITE_SUPABASE_URL` e `VITE_SUPABASE_ANON_KEY`
- Nunca expor `service_role` key no cliente
- Auth state via `onAuthStateChange` em contexto React
- Usar `supabase gen types typescript` para tipar todas as queries
- RLS ativo em todas as tabelas — nunca bypassar no cliente
