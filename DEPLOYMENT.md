# DEPLOYMENT.md

## 1. Phase 1 Platforms

```text
Frontend: Vercel
Backend: Railway
Database: Managed PostgreSQL
Storage: Cloudflare R2
Edge protection: Cloudflare WAF / Rate Limiting
```

## 2. Frontend Deployment

Repository:

```text
wood-showroom-web
```

Deploy to:

```text
Vercel
```

Branches:

```text
develop → staging
main → production
```

## 3. Backend Deployment

Repository:

```text
wood-showroom-api
```

Deploy to:

```text
Railway
```

Runtime:

```text
Docker/container or Node runtime
```

## 4. Database

Use managed PostgreSQL.

Runtime env:

```text
DATABASE_URL = pooled URL if provider supports
DIRECT_DATABASE_URL = direct URL for migration if needed
```

## 5. Storage

Use Cloudflare R2.

Backend stores:

```text
R2 endpoint
R2 access key
R2 secret key
R2 bucket
R2 public base URL
```

Never expose R2 secrets to frontend.

## 6. Release Order

Because FE and BE are separate repos:

1. Deploy backward-compatible BE first.
2. Deploy FE second.
3. Avoid breaking response changes in `/api/v1`.
4. Breaking API changes require version bump or migration plan.


## 7. Same-origin API Proxy

Production browser requests must use the frontend origin.

Frontend calls:

```text
/api/v1/*
```

Next.js rewrites to:

```text
${BACKEND_API_URL}/api/v1/*
```

This prevents cross-domain cookie issues between Vercel and Railway.

`next.config.ts` must contain a rewrite equivalent to:

```ts
async rewrites() {
  return [
    {
      source: "/api/v1/:path*",
      destination: `${process.env.BACKEND_API_URL}/api/v1/:path*`,
    },
  ];
}
```

Browser code must never call the Railway backend URL directly.

---

## 8. Local/Staging Docker + Ngrok Runtime Runbook

For local staging or end-to-end testing, the full stack can be run containerized on a unified Docker bridge network with the Next.js port exposed publicly via ngrok over HTTPS.

### Network Configuration
- **PostgreSQL**: Bound to `127.0.0.1:5432` on the host (private, not exposed publicly).
- **NestJS API**: Internal container `wood-showroom-api` on port `3001` (private, no host port mapping).
- **Next.js Web**: Standalone container `wood-showroom-web` on port `3002` (exposed locally to host).
- **ngrok Tunnel**: Points to `3002` (public HTTPS url). Same-origin relative paths `/api/v1` rewrite proxy requests through the Next.js server to the internal API container.

### Step-by-Step Execution

1. **Build and Run Containers**:
   ```bash
   # From root directory:
   docker compose up -d --build
   ```

2. **Sync Database Schema & Seed Data** (from Host machine):
   ```bash
   cd wood-showroom-api
   npx prisma migrate deploy
   npm run seed
   ```

3. **Expose Public Web Tunnel**:
   ```bash
   ngrok http 3002
   ```
   *Note: Never commit the ngrok URL or authtokens into the source code.*
