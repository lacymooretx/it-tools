# Claude Run Log - IT Tools Setup

## 2026-02-25 — Initial Setup & Deployment

### Step 1: Fork Repository
- **Goal:** Fork CorentinTh/it-tools to lacymooretx GitHub
- **What:** Used `gh repo fork CorentinTh/it-tools --clone=false`
- **Result:** Public fork created at https://github.com/lacymooretx/it-tools
- **Status:** Complete

### Step 2: Clone & Local Setup
- **Goal:** Clone fork locally and install dependencies
- **What:** `git clone` to `/Users/lacy/code/it-tools`, installed pnpm globally, ran `pnpm install`
- **Result:** 1310 packages installed successfully
- **Files:** `/Users/lacy/code/it-tools/` (full repo)
- **Status:** Complete

### Step 3: SSL Certificate
- **Goal:** Get Let's Encrypt cert for it-tools.aspendora.com via DNS challenge
- **What:** Set up certbot DNS-Cloudflare plugin credentials at `/etc/letsencrypt/cloudflare.ini` on server, ran `certbot certonly --dns-cloudflare`
- **Result:** Certificate issued, expires 2026-05-26
  - Cert: `/etc/letsencrypt/live/it-tools.aspendora.com/fullchain.pem`
  - Key: `/etc/letsencrypt/live/it-tools.aspendora.com/privkey.pem`
- **Status:** Complete

### Step 4: Cloudflare DNS
- **Goal:** Create A record for it-tools.aspendora.com
- **What:** Cloudflare API POST to zone `a06c2491527a5d50b5e85a572886b589`
- **Result:** A record created, proxied=true, record ID: `ad1c54ddcf73684fdc19f305a4db0aca`
- **Status:** Complete

### Step 5: Docker Deployment
- **Goal:** Deploy it-tools container on docker.aspendora.com
- **What:**
  1. Added `it-tools` service to `/opt/docker/docker-compose.yml` (image: `corentinth/it-tools:latest`, network: `aspendora-net`, watchtower enabled)
  2. Added nginx server blocks to `/opt/docker/nginx/conf.d/default.conf` (HTTP redirect + HTTPS proxy to it-tools:80)
  3. `docker compose up -d it-tools`
  4. `nginx -t && nginx -s reload`
- **Result:** Container running, nginx serving, HTTPS verified (HTTP/2 200)
- **Status:** Complete

### Verification
- `curl -sI https://it-tools.aspendora.com` → HTTP/2 200
- Container status: Up, port 80/tcp
- Cloudflare proxied: Yes (cf-ray header present)
