# BARD AInalytics — Cost-Efficient Hosting Guide for a HEOR Startup

## Guiding Principle
As a bootstrapped startup, your goal is to pay near-zero until you have real users,
then scale costs linearly as you grow. The architecture below lets you do exactly that.

---

## Recommended Stack (Budget-First)

### Phase 1 — Beta (0–50 users) | Est. Cost: ~$5–20/month

| Layer | Recommended Service | Monthly Cost |
|---|---|---|
| **Front-end hosting** | Vercel (Hobby) or Cloudflare Pages | **$0** |
| **Back-end API** | Railway.app or Render (free tier) | **$0–5** |
| **Database** | Supabase (free tier — PostgreSQL) | **$0** |
| **Authentication** | Supabase Auth or Clerk (free tier) | **$0** |
| **File/data storage** | Cloudflare R2 (10 GB free) | **$0** |
| **Email** | Resend (3,000 emails/month free) | **$0** |
| **CDN + DDoS protection** | Cloudflare (free plan) | **$0** |
| **Analytics** | Plausible (self-hosted) or Umami | **$0** |
| **Error monitoring** | Sentry (free tier, 5K events/mo) | **$0** |

**Total Beta Phase: ~$0–20/month**

---

### Phase 2 — Early Traction (50–500 users) | Est. Cost: ~$50–150/month

As beta users become paying customers, upgrade selectively:

| Layer | Upgrade To | Monthly Cost |
|---|---|---|
| **Front-end** | Vercel Pro or Cloudflare Pages (still cheap) | $20 |
| **Back-end** | Railway Starter Plan | $5–20 |
| **Database** | Supabase Pro ($25/mo) | $25 |
| **Auth** | Clerk Growth (~$25/mo) or Supabase Auth (included) | $0–25 |
| **Storage** | Cloudflare R2 ($0.015/GB) — pay as you grow | ~$5 |
| **Email** | Resend (paid, $20/mo for more volume) | $20 |

**Total Early Traction: ~$75–115/month**

---

### Phase 3 — Growth (500+ users / Enterprise clients) | Est. Cost: ~$300–800/month

Once you have paying enterprise clients (pharma companies), you'll need:

- **AWS or GCP** for HIPAA-compliant infrastructure (BAAs available on both)
- **AWS RDS PostgreSQL** with encryption at rest
- **AWS S3** with private buckets and presigned URLs for data files
- **Auth0 or AWS Cognito** for enterprise SSO/SAML support
- **CloudFront CDN** in front of your front-end
- SOC 2 Type II audit preparation (budget $15–30K one-time, then $5–10K/year)

At this stage, you should have revenue to justify the spend.

---

## Key Cost-Saving Decisions for HEOR Startups

### 1. Use Supabase as your Swiss Army knife
Supabase gives you PostgreSQL + auth + file storage + realtime + REST API in one free tier.
For a beta, this replaces what would otherwise be 4–5 separate paid services. It's also
open-source, so you can self-host later if needed.

### 2. Use Cloudflare everywhere you can
Cloudflare R2 (object storage) has zero egress fees — compared to AWS S3's $0.09/GB
egress, this is a massive saving for a data-heavy HEOR platform where users download
large datasets and reports frequently.

### 3. Use Railway or Render for your API (not AWS Lambda or EC2 yet)
AWS is powerful but complex and surprisingly expensive at small scale. Railway gives you
a containerised Node.js or Python API with a generous free tier and dead-simple deployment
from GitHub. Move to AWS when you have the engineering capacity to manage it properly.

### 4. Avoid Heroku
Heroku removed its free tier and is now expensive relative to alternatives. Railway,
Render, and Fly.io all offer better value for early-stage startups.

### 5. Front-end: Vercel is free and superb for Next.js
Since BARD AInalytics is likely to use Next.js (best for SEO + dashboard hybrid),
Vercel's free tier is a natural fit. Cloudflare Pages is a strong free alternative.

---

## HIPAA Compliance Note (Critical for HEOR)

If any of your clients share **Protected Health Information (PHI)** with you — even
de-identified patient-level data from clinical trials — you may need a HIPAA Business
Associate Agreement (BAA). The following free-tier services **do NOT offer BAAs**:

- Vercel (Hobby tier) — ❌ No BAA
- Supabase (free tier) — ❌ No BAA on free plan
- Railway (free tier) — ❌ No BAA

**Services that offer BAAs (usually on paid plans):**
- AWS (all services, paid plans)
- Google Cloud (paid plans)
- Cloudflare (Business plan, $200/mo)
- Auth0 (enterprise)
- Supabase (Enterprise plan)

**Recommendation for beta phase:** Keep all beta data to de-identified,
synthetic, or non-PHI datasets. As soon as you sign your first pharma enterprise
client, budget for HIPAA-compliant infrastructure (typically $200–500/month
in AWS costs, plus BAA paperwork).

---

## Recommended Tech Stack for BARD AInalytics

```
Front-end:    Next.js 14 (React) + Tailwind CSS
Back-end:     FastAPI (Python) — natural fit for HEOR/analytics workloads
Database:     PostgreSQL via Supabase
Auth:         Supabase Auth (start) → Auth0/Clerk (scale)
File storage: Cloudflare R2 → AWS S3 (when HIPAA needed)
Compute:      Railway (start) → AWS ECS/Fargate (scale)
CDN:          Cloudflare (free tier)
Monitoring:   Sentry (errors) + Plausible (analytics) + Uptime Robot (uptime)
CI/CD:        GitHub Actions (free for public repos, 2,000 min/mo for private)
```

---

## Deployment Checklist (Before Going Live)

- [ ] HTTPS enforced everywhere (Cloudflare handles this for free)
- [ ] HTTP Security Headers configured (CSP, HSTS, X-Frame-Options)
- [ ] Environment variables stored in platform secrets (never in code)
- [ ] `.env` file added to `.gitignore`
- [ ] Database backups enabled (Supabase does daily backups on free tier)
- [ ] Error monitoring set up (Sentry)
- [ ] Uptime monitoring set up (UptimeRobot — free)
- [ ] Rate limiting enabled on login and API endpoints
- [ ] Privacy policy and Terms of Service pages live
- [ ] Cookie consent banner if targeting EU users (GDPR)
- [ ] Dependency vulnerability scanning enabled (GitHub Dependabot — free)

---

## Estimated Total Cost Timeline

| Stage | Users | Monthly Cost |
|---|---|---|
| Beta launch | 0–50 | ~$0–20 |
| Post-beta | 50–200 | ~$50–100 |
| Series A prep | 200–1000 | ~$150–400 |
| Enterprise-ready (HIPAA) | 1000+ | ~$500–1,500 |

The beauty of this stack: you pay essentially nothing until you have paying customers,
then costs scale proportionally with revenue.
