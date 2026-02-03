# Setup & Installation Guide

## Prerequisites

### Required Software

| Software | Version | Purpose |
|----------|---------|---------|
| **Node.js** | ^24.5.0 | Runtime for frontend and backend |
| **Yarn** | >=4.0.2 | Package manager (Yarn 4 required) |
| **PostgreSQL** | 14+ | Primary database |
| **Redis** | 6+ | Caching and session management |
| **Docker** | 20+ | Containerization (optional but recommended) |
| **Docker Compose** | 2+ | Multi-container orchestration |

### System Requirements

**Minimum (Development):**
- CPU: 4 cores
- RAM: 8 GB
- Disk: 20 GB free space
- OS: macOS, Linux, or WSL2 on Windows

**Recommended (Production):**
- CPU: 8+ cores
- RAM: 16+ GB
- Disk: 50+ GB SSD
- OS: Linux (Ubuntu 20.04+ or similar)

### Development Tools (Optional)

- **IDE:** VS Code with ESLint, Prettier extensions
- **API Testing:** Postman, Insomnia, or GraphiQL
- **Database Tools:** pgAdmin, DBeaver, or Postico
- **Git:** Version control

## Local Development Setup

### Option 1: Docker Compose (Recommended)

This is the fastest way to get started with all dependencies pre-configured.

#### Step 1: Clone the Repository

```bash
git clone https://github.com/twentyhq/twenty.git
cd twenty
```

#### Step 2: Configure Environment Variables

The development environment script will set up `.env` files, but you can customize them:

```bash
# Setup development environment (creates .env files, builds dependencies)
bash packages/twenty-utils/setup-dev-env.sh
```

This script:
- Creates `.env` files from templates
- Installs dependencies
- Builds shared packages
- Sets up the database
- Seeds initial data

#### Step 3: Start Development Environment

```bash
# Start all services (frontend, backend, worker)
yarn start
```

This command runs:
- `twenty-server` on http://localhost:3000 (backend GraphQL API)
- `twenty-front` on http://localhost:3001 (frontend React app)
- `twenty-server:worker` (background job processor)

#### Step 4: Access the Application

- **Frontend:** http://localhost:3001
- **Backend API:** http://localhost:3000/graphql
- **API Documentation:** http://localhost:3000/rest/api/docs

**Default Credentials:**
- Email: `dev@twenty.com`
- Password: `Applecar2025`

### Option 2: Manual Setup (Without Docker)

If you prefer to run services directly on your host machine:

#### Step 1: Install Dependencies

```bash
# Install PostgreSQL (macOS with Homebrew)
brew install postgresql@14
brew services start postgresql@14

# Install Redis
brew install redis
brew services start redis

# Install Node.js 24 (using nvm)
nvm install 24
nvm use 24

# Install Yarn 4
corepack enable
corepack prepare yarn@4.9.2 --activate
```

#### Step 2: Clone and Configure

```bash
# Clone repository
git clone https://github.com/twentyhq/twenty.git
cd twenty

# Install dependencies
yarn install
```

#### Step 3: Configure Environment Variables

Create `.env` files for server and frontend:

**packages/twenty-server/.env:**
```env
# Database
PG_DATABASE_URL=postgres://postgres:postgres@localhost:5432/default

# Redis
REDIS_URL=redis://localhost:6379

# Server
PORT=3000
FRONT_BASE_URL=http://localhost:3001

# Authentication
ACCESS_TOKEN_SECRET=replace_me_with_a_random_string_access
REFRESH_TOKEN_SECRET=replace_me_with_a_random_string_refresh
LOGIN_TOKEN_SECRET=replace_me_with_a_random_string_login
FILE_TOKEN_SECRET=replace_me_with_a_random_string_file
API_TOKEN_SECRET=replace_me_with_a_random_string_api

# Storage (local for development)
STORAGE_TYPE=local
STORAGE_LOCAL_PATH=.local-storage

# Email (optional for development)
EMAIL_FROM_ADDRESS=noreply@twenty.com
EMAIL_SYSTEM_ADDRESS=system@twenty.com
EMAIL_DRIVER=logger

# Feature Flags
IS_BILLING_ENABLED=false
IS_SSO_ENABLED=false
IS_MULTIWORKSPACE_ENABLED=false
```

**packages/twenty-front/.env.local:**
```env
REACT_APP_SERVER_BASE_URL=http://localhost:3000
```

#### Step 4: Initialize Database

```bash
# Navigate to server package
cd packages/twenty-server

# Create database
createdb -U postgres default

# Run migrations
npx nx database:init:prod twenty-server

# Seed development data (optional)
npx nx run twenty-server:command workspace:seed:dev
```

#### Step 5: Build Shared Packages

```bash
# From project root
npx nx build twenty-shared
```

#### Step 6: Start Services

Open three terminal windows:

**Terminal 1 - Backend Server:**
```bash
npx nx start twenty-server
```

**Terminal 2 - Frontend:**
```bash
npx nx start twenty-front
```

**Terminal 3 - Worker:**
```bash
npx nx run twenty-server:worker
```

## Environment Variables Reference

### Backend (twenty-server)

| Variable | Description | Example | Required |
|----------|-------------|---------|----------|
| `PG_DATABASE_URL` | PostgreSQL connection string | `postgres://user:pass@host:5432/db` | Yes |
| `REDIS_URL` | Redis connection string | `redis://localhost:6379` | Yes |
| `PORT` | Server port | `3000` | Yes |
| `FRONT_BASE_URL` | Frontend URL for CORS | `http://localhost:3001` | Yes |
| `ACCESS_TOKEN_SECRET` | JWT access token secret | Random 32+ char string | Yes |
| `REFRESH_TOKEN_SECRET` | JWT refresh token secret | Random 32+ char string | Yes |
| `LOGIN_TOKEN_SECRET` | Login token secret | Random 32+ char string | Yes |
| `FILE_TOKEN_SECRET` | File access token secret | Random 32+ char string | Yes |
| `API_TOKEN_SECRET` | API key secret | Random 32+ char string | Yes |
| `STORAGE_TYPE` | File storage backend | `local`, `s3` | Yes |
| `STORAGE_LOCAL_PATH` | Local storage path | `.local-storage` | If local |
| `STORAGE_S3_REGION` | AWS S3 region | `us-east-1` | If S3 |
| `STORAGE_S3_NAME` | S3 bucket name | `my-bucket` | If S3 |
| `AWS_ACCESS_KEY_ID` | AWS credentials | - | If S3 |
| `AWS_SECRET_ACCESS_KEY` | AWS credentials | - | If S3 |
| `EMAIL_DRIVER` | Email provider | `logger`, `smtp`, `ses` | Yes |
| `EMAIL_SMTP_HOST` | SMTP server | `smtp.gmail.com` | If SMTP |
| `EMAIL_SMTP_PORT` | SMTP port | `587` | If SMTP |
| `IS_BILLING_ENABLED` | Enable Stripe billing | `true`, `false` | No |
| `IS_SSO_ENABLED` | Enable SSO authentication | `true`, `false` | No |
| `IS_MULTIWORKSPACE_ENABLED` | Enable multi-workspace | `true`, `false` | No |
| `GOOGLE_CLIENT_ID` | Google OAuth client ID | - | If Google auth |
| `GOOGLE_CLIENT_SECRET` | Google OAuth secret | - | If Google auth |
| `MICROSOFT_CLIENT_ID` | Microsoft OAuth client ID | - | If MS auth |
| `MICROSOFT_CLIENT_SECRET` | Microsoft OAuth secret | - | If MS auth |

### Frontend (twenty-front)

| Variable | Description | Example | Required |
|----------|-------------|---------|----------|
| `REACT_APP_SERVER_BASE_URL` | Backend API URL | `http://localhost:3000` | Yes |
| `VITE_DISABLE_TYPESCRIPT_CHECKER` | Disable TS checking in build | `true`, `false` | No |
| `VITE_BUILD_SOURCEMAP` | Generate sourcemaps | `true`, `false` | No |

## Running Tests

### Unit Tests

```bash
# Run all tests for a package
npx nx test twenty-front
npx nx test twenty-server

# Run a single test file (fastest - recommended)
npx jest path/to/test.test.ts --config=packages/twenty-front/jest.config.mjs

# Run tests in watch mode
npx jest --watch --config=packages/twenty-front/jest.config.mjs
```

### Integration Tests

```bash
# Run integration tests with database reset
npx nx run twenty-server:test:integration:with-db-reset
```

### E2E Tests (Playwright)

```bash
# Build frontend first
npx nx build twenty-front

# Run E2E tests
npx nx test twenty-e2e-testing
```

### Storybook Tests

```bash
# Build Storybook
npx nx storybook:build twenty-front

# Run Storybook tests
npx nx storybook:test twenty-front
```

## Running Both Frontend & Backend Together

### Using the Convenience Script

```bash
# From project root - starts everything
yarn start
```

This uses `concurrently` to run:
1. Frontend dev server (Vite) on port 3001
2. Backend API server (NestJS) on port 3000
3. Worker process for background jobs

### Stopping Services

Press `Ctrl+C` in the terminal to stop all services.

## Code Quality Commands

### Linting

```bash
# Lint only changed files (fast - prefer this)
npx nx lint:diff-with-main twenty-front
npx nx lint:diff-with-main twenty-server

# Auto-fix lint issues
npx nx lint:diff-with-main twenty-front --configuration=fix

# Lint entire project (slower)
npx nx lint twenty-front
npx nx lint twenty-server
```

### Type Checking

```bash
# Check TypeScript types
npx nx typecheck twenty-front
npx nx typecheck twenty-server
```

### Formatting

```bash
# Format code with Prettier
npx nx fmt twenty-front
npx nx fmt twenty-server
```

## Database Operations

### Reset Database

```bash
# WARNING: Deletes all data and recreates schema
npx nx database:reset twenty-server
```

### Run Migrations

```bash
# Run pending migrations
npx nx run twenty-server:database:migrate:prod
```

### Generate Migration

```bash
# After modifying entity files, generate migration
npx nx run twenty-server:typeorm migration:generate \
  src/database/typeorm/core/migrations/common/[migration-name] \
  -d src/database/typeorm/core/core.datasource.ts

# Replace [migration-name] with kebab-case description
# Example: add-opportunity-stage-field
```

### Sync Metadata

```bash
# Sync object metadata after schema changes
npx nx run twenty-server:command workspace:sync-metadata
```

## GraphQL Schema Operations

### Generate TypeScript Types

```bash
# After GraphQL schema changes, regenerate types
npx nx run twenty-front:graphql:generate

# For metadata API
npx nx run twenty-front:graphql:generate --configuration=metadata
```

## Build for Production

### Build All Packages

```bash
# Build in correct order (twenty-shared must be first)
npx nx build twenty-shared
npx nx build twenty-server
npx nx build twenty-front
```

### Production Environment Variables

Update `.env` files with production values:

**Critical Production Changes:**
- Use strong random secrets for all `_SECRET` variables
- Set `PG_DATABASE_URL` to production database
- Set `REDIS_URL` to production Redis instance
- Configure `STORAGE_TYPE=s3` with AWS credentials
- Set up `EMAIL_DRIVER=smtp` or `ses` with real email service
- Enable `IS_BILLING_ENABLED` if using Stripe
- Set `FRONT_BASE_URL` to production domain

### Run Production Build

```bash
# Start backend
cd packages/twenty-server
yarn start:prod

# Start worker
yarn worker:prod

# Serve frontend (static files)
cd packages/twenty-front
yarn start:prod
```

## Docker Deployment

### Using Docker Compose

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

### Individual Containers

```bash
# Build images
docker build -t twenty-server -f ./packages/twenty-server/Dockerfile .
docker build -t twenty-front -f ./packages/twenty-front/Dockerfile .

# Run containers
docker run -d -p 3000:3000 --env-file .env twenty-server
docker run -d -p 3001:3001 twenty-front
```

## Kubernetes Deployment

Twenty provides Helm charts for Kubernetes deployment:

```bash
# Add Helm repository (if available)
helm repo add twenty https://charts.twenty.com
helm repo update

# Install with custom values
helm install twenty twenty/twenty \
  --set postgresql.enabled=true \
  --set redis.enabled=true \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host=crm.example.com
```

## Troubleshooting

### Common Issues

#### Port Already in Use

```bash
# Check what's using port 3000
lsof -i :3000

# Kill the process
kill -9 <PID>
```

#### Database Connection Error

```bash
# Verify PostgreSQL is running
pg_isready

# Check connection string
psql "postgres://postgres:postgres@localhost:5432/default"
```

#### Redis Connection Error

```bash
# Verify Redis is running
redis-cli ping
# Should return: PONG
```

#### Node Version Mismatch

```bash
# Check Node version
node --version
# Should be 24.x.x

# Switch to correct version (if using nvm)
nvm use 24
```

#### Yarn Version Issues

```bash
# Check Yarn version
yarn --version
# Should be 4.x.x

# Enable Corepack and set version
corepack enable
corepack prepare yarn@4.9.2 --activate
```

#### Build Failures

```bash
# Clean build cache
npx nx reset

# Remove node_modules and reinstall
rm -rf node_modules
yarn install

# Build shared packages first
npx nx build twenty-shared
```

#### TypeScript Errors

```bash
# Clear TypeScript cache
rm -rf packages/*/tsconfig.tsbuildinfo

# Regenerate GraphQL types
npx nx run twenty-front:graphql:generate
```

### Getting Help

- **Documentation:** https://docs.twenty.com
- **Discord:** https://discord.gg/cx5n4Jzs57
- **GitHub Issues:** https://github.com/twentyhq/twenty/issues
- **Discussions:** https://github.com/twentyhq/twenty/discussions

## Maintenance Operations

### Backup Database

```bash
# PostgreSQL backup
pg_dump -U postgres -d default -F c -f twenty_backup_$(date +%Y%m%d).dump

# Redis backup (if persistence is enabled)
redis-cli SAVE
cp /var/lib/redis/dump.rdb twenty_redis_backup_$(date +%Y%m%d).rdb
```

### Restore Database

```bash
# PostgreSQL restore
pg_restore -U postgres -d default -c twenty_backup_20260203.dump
```

### Update Dependencies

```bash
# Check for outdated packages
yarn outdated

# Update all packages (be cautious)
yarn upgrade-interactive

# Run tests after updates
npx nx run-many -t test
```

### Monitor Logs

```bash
# Backend logs
npx nx start twenty-server | tee logs/server.log

# Worker logs
npx nx run twenty-server:worker | tee logs/worker.log

# Frontend logs
npx nx start twenty-front | tee logs/front.log
```

## Performance Optimization

### Development Mode Optimizations

```bash
# Disable TypeScript checking for faster builds
export VITE_DISABLE_TYPESCRIPT_CHECKER=true

# Increase Node memory limit
export NODE_OPTIONS=--max-old-space-size=8192
```

### Production Optimizations

- Enable PostgreSQL connection pooling (PgBouncer)
- Configure Redis as cache layer
- Use CDN for frontend static assets
- Enable gzip/brotli compression
- Set up database read replicas for heavy read workloads
- Use ClickHouse for analytics queries (optional)

## Security Checklist

- [ ] Change all default secrets in production
- [ ] Use strong, random JWT secrets (32+ characters)
- [ ] Enable HTTPS with valid SSL certificates
- [ ] Configure CORS to allow only trusted origins
- [ ] Set up firewall rules to restrict database/Redis access
- [ ] Enable 2FA for admin accounts
- [ ] Configure rate limiting on API endpoints
- [ ] Set up audit logging
- [ ] Regular security updates and patching
- [ ] Backup encryption for sensitive data

## Next Steps

After successful setup:
1. Review [Architecture](./03_Architecture.md) to understand system design
2. Explore [Code Structure](./04_Code_Structure.md) for codebase navigation
3. Check [API Documentation](./05_API_Documentation.md) for integration
4. Try [Workflows & Examples](./06_Workflows_Examples.md) for common use cases
