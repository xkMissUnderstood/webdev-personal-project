# Plan: Learn-by-Doing Web Dev Portfolio/Blog

## TL;DR
Build a personal portfolio/blog from scratch using HTML, CSS, and TypeScript — no PHP. Each 2-week iteration adds features while introducing new infrastructure, security, and performance concepts. Deploy everything for real using free-tier services (GitHub Pages → Cloudflare Pages → Docker → Kubernetes locally). By week 14, you'll have touched semantic HTML, SEO, CDNs, reverse proxies, CAPTCHA, containers, Kubernetes, CI/CD, security hardening, and responsive cross-device design.

## Architecture Decisions
- **No frameworks initially** — raw HTML/CSS/JS to learn fundamentals, TypeScript added in iteration 3
- **Build tool**: Vite (fast, modern, supports TS natively)
- **Hosting**: Cloudflare Pages (free, global CDN, edge functions, custom domain)
- **CI/CD**: GitHub Actions (free for public repos)
- **Containers**: Docker + Nginx (reverse proxy learning)
- **Kubernetes**: Kind (local cluster) for learning orchestration
- **CAPTCHA**: hCaptcha (free, privacy-respecting)
- **Serverless**: Cloudflare Workers (free tier: 100k req/day)

---

## Phase 1: Foundation + First Deploy (Weeks 1–2)

**Goal**: Semantic HTML site live on the internet with a CDN in front of it.

### Steps
1. Create project structure: `index.html`, `css/style.css`, `assets/`
2. Build `index.html` with full semantic HTML5 structure:
   - `<!DOCTYPE html>`, `<html lang="en">`, `<head>` with charset/viewport
   - `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<footer>`
   - Proper heading hierarchy (`h1` → `h2` → `h3`)
   - `<figure>`, `<figcaption>`, `<time>`, `<address>`
3. Add basic CSS: reset/normalize, typography, color variables (CSS custom properties)
4. Initialize Git repo, push to GitHub
5. Deploy to GitHub Pages (Settings → Pages → main branch)
6. Set up Cloudflare (free) as DNS + CDN in front of GitHub Pages
   - Understand: DNS resolution → Cloudflare edge → origin (GitHub)
   - This IS a reverse proxy in action

### Concepts Learned
- Semantic HTML vs div soup
- How CDNs work (edge caching, PoPs across regions)
- How reverse proxies work (Cloudflare sits between user and origin)
- DNS resolution flow
- Git basics

### Files Created
- `index.html` — homepage with semantic structure
- `css/style.css` — base styles with CSS custom properties
- `.gitignore`
- `README.md` — project documentation

---

## Phase 2: Multi-page + SEO + Responsive (Weeks 3–4)

**Goal**: Multiple pages, mobile-first responsive design, SEO-ready.

### Steps
1. Add pages: `about.html`, `projects.html`, `blog/index.html`
2. Create shared navigation component (reusable HTML pattern)
3. Implement CSS Grid for page layout, Flexbox for component layout
4. Mobile-first responsive design:
   - Base styles = mobile
   - `@media (min-width: 768px)` for tablet
   - `@media (min-width: 1024px)` for desktop
5. SEO optimization:
   - `<meta name="description">`, `<meta name="keywords">`
   - Open Graph tags (`og:title`, `og:image`, `og:description`)
   - Twitter Card meta tags
   - Structured data (JSON-LD for Person schema)
   - `<link rel="canonical">`
6. Create `robots.txt` and `sitemap.xml`
7. Add `favicon.ico` + various icon sizes for devices
8. Test cross-device using browser DevTools responsive mode

### Concepts Learned
- SEO: how search engines crawl and index
- `robots.txt` — what crawlers are allowed/disallowed
- `sitemap.xml` — helping crawlers discover pages
- Open Graph — how links look when shared on social media
- Responsive design — one codebase, all devices
- CSS Grid vs Flexbox (when to use which)

### Files Created/Modified
- `about.html`, `projects.html`, `blog/index.html`
- `robots.txt`, `sitemap.xml`
- `css/style.css` — expanded with grid, flexbox, media queries

---

## Phase 3: JavaScript + TypeScript + Performance (Weeks 5–6)

**Goal**: Interactive features, TypeScript build pipeline, performance-optimized.

### Steps
1. Add vanilla JS features:
   - Mobile nav hamburger toggle
   - Dark/light theme switcher (persisted in localStorage)
   - Smooth scroll behavior
2. Set up Vite as build tool:
   - `npm init`, `npm install vite typescript`
   - Convert project to Vite structure (`src/`, `public/`)
   - Configure TypeScript (`tsconfig.json`)
3. Rewrite JS as TypeScript (type safety, interfaces)
4. Image optimization:
   - `<picture>` element with WebP + fallback PNG/JPG
   - `srcset` for responsive images (different sizes per viewport)
   - `loading="lazy"` for below-the-fold images
   - `decoding="async"`
5. Performance fallbacks:
   - `<noscript>` content for JS-disabled users
   - Progressive enhancement pattern (HTML works → CSS enhances → JS enriches)
   - `preload` critical CSS, `prefetch` next-page resources
   - Font loading strategy (`font-display: swap`)
6. Run Lighthouse audit, target 90+ on all metrics
7. Set up `package.json` scripts: `dev`, `build`, `preview`

### Concepts Learned
- Progressive enhancement (the web's resilience philosophy)
- How browsers load resources (critical rendering path)
- TypeScript — type safety, catching bugs at build time
- Build tools — why we bundle/minify/treeshake
- Image formats and responsive images across devices
- Performance budgets

### Files Created/Modified
- `src/main.ts`, `src/theme.ts`, `src/nav.ts`
- `vite.config.ts`, `tsconfig.json`, `package.json`
- Restructure: static assets → `public/`, source → `src/`

---

## Phase 4: Docker + CI/CD + Security Headers (Weeks 7–8)

**Goal**: Containerized site with automated deployment and security headers.

### Steps
1. Create `Dockerfile`:
   - Multi-stage build: Node (build) → Nginx (serve)
   - Minimal image size (alpine-based)
2. Create `nginx.conf` with security headers:
   - `Content-Security-Policy` (CSP) — control what resources load
   - `X-Content-Type-Options: nosniff`
   - `X-Frame-Options: DENY`
   - `Strict-Transport-Security` (HSTS)
   - `Referrer-Policy: strict-origin-when-cross-origin`
   - `Permissions-Policy` (disable unused browser APIs)
3. Create `docker-compose.yml` for local development
4. Set up GitHub Actions CI/CD pipeline:
   - On push: lint HTML (htmlhint), lint CSS (stylelint), lint TS (eslint)
   - Build with Vite
   - Deploy to Cloudflare Pages via Wrangler
5. Add `.github/workflows/deploy.yml`
6. Understand Nginx as reverse proxy:
   - Configure `proxy_pass` (even if proxying to self for learning)
   - Caching headers (`Cache-Control`, `ETag`, `Last-Modified`)
   - Gzip/Brotli compression

### Concepts Learned
- Docker — containerization, reproducible environments
- Multi-stage builds — keep images small
- Nginx — the most common reverse proxy/web server
- Security headers — defense in depth against XSS, clickjacking, MIME sniffing
- CI/CD — automated testing and deployment
- Forward proxy vs reverse proxy (conceptual comparison)
- Caching strategies (browser cache, CDN cache, origin cache)

### Files Created
- `Dockerfile`, `.dockerignore`
- `nginx/nginx.conf` — with security headers + caching
- `docker-compose.yml`
- `.github/workflows/deploy.yml`
- `.htmlhintrc`, `.stylelintrc.json`, `eslint.config.js`

---

## Phase 5: Blog Engine + CAPTCHA + Serverless (Weeks 9–10)

**Goal**: Markdown blog posts, contact form with CAPTCHA, serverless backend.

### Steps
1. Build static blog engine:
   - Write posts in Markdown (`blog/posts/*.md`)
   - Build script converts Markdown → HTML (using `marked` library)
   - Template system (simple HTML template injection)
   - Blog index page auto-generated from posts
2. Create contact form (`contact.html`):
   - Accessible form with proper `<label>`, `aria-*` attributes
   - Client-side validation (HTML5 + TypeScript)
   - Integrate hCaptcha (free CAPTCHA):
     - How it works: challenge issued → user solves → token sent to server → server verifies with hCaptcha API
3. Serverless form handler (Cloudflare Worker):
   - Receives form POST
   - Validates hCaptcha token server-side
   - Rate limiting (using Cloudflare KV for counting)
   - Sends notification (email or webhook)
4. Understand CAPTCHA deeper:
   - Why CAPTCHAs exist (bot prevention)
   - How they work (proof-of-work, image recognition, behavioral analysis)
   - Accessibility concerns (audio alternatives)
5. Add `<meta http-equiv="Content-Security-Policy">` fallback for when headers aren't available

### Concepts Learned
- CAPTCHA: how they work, why they exist, implementation
- Serverless functions — code without managing servers
- Rate limiting — protecting endpoints from abuse
- Form security — validation, CSRF tokens, input sanitization
- Static site generation concepts
- Accessibility in forms

### Files Created
- `blog/posts/*.md` — sample blog posts
- `src/build-blog.ts` — Markdown → HTML build script
- `contact.html` — accessible contact form
- `workers/contact-handler.ts` — Cloudflare Worker
- `wrangler.toml` — Cloudflare Worker config

---

## Phase 6: Kubernetes + Advanced Infrastructure (Weeks 11–12)

**Goal**: Run the site in a local Kubernetes cluster, understand orchestration.

### Steps
1. Install Kind (Kubernetes in Docker) locally
2. Create Kubernetes manifests:
   - `Deployment` — run multiple replicas of the Nginx container
   - `Service` — internal load balancing
   - `Ingress` — route external traffic (with Nginx Ingress Controller)
   - `ConfigMap` — externalize nginx.conf
   - `HorizontalPodAutoscaler` — auto-scale based on CPU
3. Deploy to local Kind cluster:
   - `kind create cluster`
   - `kubectl apply -f k8s/`
   - Observe pods, scaling, self-healing (kill a pod, watch it restart)
4. Understand concepts:
   - Why Kubernetes? (scaling, self-healing, declarative)
   - Pod → Service → Ingress traffic flow
   - How CDN + K8s work together (CDN caches, K8s handles dynamic)
   - Multi-region: how companies run clusters in multiple regions
   - Ingress Controller = reverse proxy within K8s
5. Add health check endpoint (`/healthz`) for K8s liveness/readiness probes
6. Document the infrastructure diagram in README

### Concepts Learned
- Kubernetes fundamentals (pods, deployments, services, ingress)
- Container orchestration — why it matters at scale
- Load balancing — distributing traffic across replicas
- Self-healing — automatic restart on failure
- Horizontal scaling — adding more instances vs bigger instances
- How Ingress Controllers are reverse proxies
- Multi-region deployment concepts

### Files Created
- `k8s/deployment.yaml`
- `k8s/service.yaml`
- `k8s/ingress.yaml`
- `k8s/configmap.yaml`
- `k8s/hpa.yaml`
- `k8s/README.md` — infra documentation

---

## Phase 7: Security Hardening + Final Polish (Weeks 13–14)

**Goal**: Harden against attacks, accessibility audit, final documentation.

### Steps
1. Security hardening:
   - Subresource Integrity (SRI) on all external scripts/styles
   - CSP nonce-based script loading
   - XSS prevention audit (no `innerHTML` with user data)
   - CSRF protection on forms
   - Input sanitization review
   - `Permissions-Policy` header (disable camera, mic, geolocation)
2. Advanced CDN configuration:
   - Cloudflare Page Rules / Cache Rules
   - Understand cache invalidation strategies
   - Edge caching vs origin caching
   - Stale-while-revalidate pattern
3. Accessibility (a11y) audit:
   - WCAG 2.1 AA compliance check
   - Screen reader testing
   - Keyboard navigation
   - Color contrast ratios
   - `prefers-reduced-motion`, `prefers-color-scheme` media queries
4. Error handling / fallbacks:
   - Custom 404 page
   - Offline fallback with Service Worker (basic)
   - Graceful degradation when CDN/JS fails
5. Final documentation:
   - Architecture diagram
   - Deployment runbook
   - Security measures documented
   - Performance benchmarks (Lighthouse scores)

### Concepts Learned
- Defense in depth — multiple security layers
- OWASP Top 10 awareness
- SRI — ensuring CDN-served files aren't tampered with
- Service Workers — offline capability, PWA concepts
- Accessibility — the web is for everyone
- Cache invalidation (one of the two hard problems in CS)

### Files Created/Modified
- `src/sw.ts` — Service Worker for offline fallback
- `public/offline.html` — offline fallback page
- `public/404.html` — custom error page
- `ARCHITECTURE.md` — system design documentation
- `SECURITY.md` — security measures documentation

---

## Verification (ongoing each phase)

1. **Lighthouse audit** after each phase (target: 90+ all categories)
2. **HTML validation** via W3C validator (https://validator.w3.org/)
3. **Security headers check** via securityheaders.com
4. **Responsive testing** via browser DevTools (320px → 1920px)
5. **Git history** — clean commits showing progression
6. **Docker build** succeeds: `docker build -t portfolio .`
7. **K8s deploy** succeeds: `kubectl get pods` shows Running
8. **CAPTCHA test** — form rejects without valid token
9. **CI/CD pipeline** — push triggers automated deploy

---

## Free Services Used
| Service | Purpose |
|---------|---------|
| GitHub | Code hosting, Actions CI/CD |
| Cloudflare Pages | Hosting + CDN + custom domain (free) |
| Cloudflare Workers | Serverless form handler (100k req/day free) |
| hCaptcha | CAPTCHA (free tier) |
| Kind | Local Kubernetes cluster |
| Docker Desktop | Containerization |

---

## Scope Boundaries
**Included**: HTML, CSS, TypeScript, Vite, Docker, Nginx, Kubernetes (local), Cloudflare (CDN + Workers), GitHub Actions, hCaptcha, SEO, security headers, responsive design, accessibility, performance optimization, progressive enhancement.

**Excluded**: PHP/Laravel, paid hosting, production Kubernetes clusters (cloud), databases (not needed for static portfolio), backend frameworks, React/Vue/Angular (raw HTML/CSS/TS is the goal).
