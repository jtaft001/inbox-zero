# Inbox Zero - Portainer Deployment Guide

## Quick Deploy via Portainer

### 1. Prerequisites
- Portainer running with Docker Compose support
- Traefik reverse proxy with Cloudflare SSL
- Google Cloud Console project with Gmail API enabled

### 2. Portainer Stack Deployment

1. **Create New Stack in Portainer:**
   - Name: `inbox-zero`
   - Repository URL: `https://github.com/jtaft001/inbox-zero`
   - Compose file path: `docker-compose.portainer.yml`

2. **Environment Variables:**
   Copy from `.env.production.template` and fill in your values:
   - Database password
   - NextAuth secret (generate with `openssl rand -hex 32`)
   - Google API credentials from Google Cloud Console
   - Anthropic API key for Claude integration
   - Redis token (generate with `openssl rand -hex 32`)

### 3. Google Cloud Console Setup

1. **Enable APIs:**
   - Gmail API
   - Google+ API (for profile info)

2. **OAuth 2.0 Configuration:**
   - Client type: Web application
   - Authorized redirect URI: `https://inbox-zero.taft-ranch.com/api/auth/callback/google`

### 4. Access
- Application URL: `https://inbox-zero.taft-ranch.com`
- First login will require Google OAuth consent

### 5. Updates
- Push changes to GitHub
- Redeploy stack in Portainer to pull latest changes

## Configuration

### Domain Configuration
The deployment is configured for `inbox-zero.taft-ranch.com`. Update the following if using a different domain:
- `docker-compose.portainer.yml` - Traefik labels
- `NEXTAUTH_URL` environment variable
- Google OAuth redirect URI

### Security Notes
- All secrets are passed via environment variables
- Database data persists in Docker volumes
- Redis is used for session storage and caching
- Internal API uses salted keys for security