# Carrier Onboarding (Supabase + Lovable)

Backend-first, production-ready carrier onboarding for trucking dispatch.

Ships:
- `onboarding_v2` schema with RLS (carriers, agreements, documents, submissions)
- Private Storage bucket (`carrier-docs`) for W‑9, COI, NOA, etc.
- Edge Function `carrier-submit` (multipart form → DB + Storage) with CORS handled
- GitHub Action that applies migrations and deploys the function on every push

**Default fee is 7% (700 bps). Override per-link to 4–12% using `?fee=4..12`.**

## One‑time setup
1. Supabase Storage → create **private** bucket `carrier-docs`.
2. Supabase Studio → Edge Functions → **Secrets**:
   - `SUPABASE_URL` (Project Settings → API → URL)
   - `SUPABASE_SERVICE_ROLE_KEY` (service_role key)
   - `DEFAULT_WEEKLY_FEE_BPS=700`
3. GitHub repo → Settings → Secrets and variables → Actions:
   - `SUPABASE_ACCESS_TOKEN` (Account → Access Tokens)
   - `SUPABASE_PROJECT_REF` (short ref from project URL)
4. Push to `main` → CI runs: db push + function deploy.
5. Copy function URL from Supabase Studio:
   `https://<project-ref>.supabase.co/functions/v1/carrier-submit`
   and put it into `form/lovable-snippet.html` as `ACTION_URL`.