DataForge — AI Data Intelligence & Digital Dataset Business
Operational architecture for discovering lawful public data, reviewing licensing, generating B2B datasets/reports, selling them through Stripe, delivering files from object storage, and running recurring updates through a queue worker.
Important compliance gate
This app intentionally does not treat public availability as resale permission. A source must have `licenseStatus=APPROVED` and `allowedForResale=true` before the product generator can use it. Review the source's license/terms and the individual dataset's rights yourself or through qualified counsel. Do not add private/personal data, bypass authentication, evade rate limits, or scrape sites contrary to their terms.
Stack
Next.js 16.3.3 + TypeScript + Tailwind
PostgreSQL + Prisma 7
JWT session cookie + bcrypt password hashing
Stripe Checkout + signed webhook verification
Redis/BullMQ worker
S3-compatible object storage
OpenAI-compatible AI provider
Nodemailer-ready transactional email layer
CSV/XLSX/PDF generators
Production setup
Use Node 20.19+ and PostgreSQL.
Copy `.env.example` to `.env` and set real secrets.
Install dependencies with `npm install`.
Run `npx prisma migrate dev --name init` for development or apply migrations in CI/production.
Run `npm run db:seed`.
Start web: `npm run dev` or `npm run build \&\& npm start`.
Start worker separately with `npm run worker` after Redis is configured.
Configure Stripe webhook endpoint at `/api/webhooks/stripe` and set its signing secret.
Configure S3-compatible storage before enabling paid file delivery.
Configure AI credentials before AI opportunity/product generation.
Real automation flow
`SCAN SOURCES -> LICENSE REVIEW -> APPROVE -> ANALYZE -> BUILD -> QA -> DRAFT PRODUCT -> HUMAN PUBLISH -> STRIPE SALE -> WEBHOOK -> DOWNLOAD -> UPDATE`
The legal approval and final publish gates are deliberately human-controlled. Automated systems should not make unsupported legal conclusions.
Included source discovery
The first discovery connector uses the Data.gov catalog API to find candidate datasets. It stores candidates as PENDING and does not authorize resale. Add additional API/RSS/open-data connectors under `lib/sources.ts` after reviewing their terms and API licenses.
Production hardening still required
Put the app behind HTTPS and a WAF/reverse proxy.
Use Redis-backed rate limiting rather than the in-memory limiter.
Store secrets in the hosting provider's secret manager.
Use private object storage and signed download URLs for scale.
Add backup/restore, monitoring, alerting, log retention, and a managed Postgres HA plan.
Add a full source-license review workflow and per-dataset legal evidence fields.
Add tax/VAT handling appropriate to your jurisdictions.
Configure email templates and unsubscribe/compliance controls before marketing.
Add a human review queue for AI-generated claims and product copy.
