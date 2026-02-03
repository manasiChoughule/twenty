# Dependencies & Environment Requirements

> **Last Verified:** 2026-02-03
> **Twenty Version:** 0.2.1
> **Source:** `package.json`, `.nvmrc`, and codebase analysis
>
> ⚠️ **Note:** Dependency versions change frequently. Always verify against the actual `package.json` files in the repository for the most current requirements.

---

## Runtime Requirements

### Node.js

**Version:** ^24.5.0

**Why Node 24?**
- Native TypeScript support improvements
- Performance optimizations
- Security updates
- Required by some dependencies

**Installation:**
```bash
# Using nvm (recommended)
nvm install 24
nvm use 24

# Verify installation
node --version  # Should output v24.5.0 or higher
```

### Package Manager

**Yarn 4 (Berry)** - Version >=4.0.2

**Why Yarn 4?**
- Modern package manager with better performance
- Zero-installs capability
- Plugin ecosystem
- Improved workspace support

**Installation:**
```bash
corepack enable
corepack prepare yarn@4.9.2 --activate
yarn --version  # Should output 4.9.2
```

### Database

**PostgreSQL 14+**

**Why PostgreSQL?**
- Robust ACID compliance
- JSON/JSONB support for flexible schemas
- Advanced indexing capabilities
- Mature extension ecosystem
- Excellent performance for CRM workloads

**Required Features:**
- JSONB support (for metadata storage)
- UUID extension (for primary keys)
- Full-text search capabilities
- Materialized views

**Installation:**
```bash
# macOS (Homebrew)
brew install postgresql@14
brew services start postgresql@14

# Ubuntu/Debian
sudo apt-get install postgresql-14

# Docker
docker run -d \
  --name twenty-postgres \
  -e POSTGRES_PASSWORD=postgres \
  -e POSTGRES_DB=default \
  -p 5432:5432 \
  postgres:14
```

### Redis

**Version:** 6+

**Why Redis?**
- Fast in-memory caching
- Pub/Sub for GraphQL subscriptions
- BullMQ job queue backend
- Session storage

**Installation:**
```bash
# macOS (Homebrew)
brew install redis
brew services start redis

# Ubuntu/Debian
sudo apt-get install redis-server

# Docker
docker run -d \
  --name twenty-redis \
  -p 6379:6379 \
  redis:6
```

## Major Frontend Dependencies

### Core Framework

| Dependency | Version | Purpose | Documentation |
|------------|---------|---------|---------------|
| **react** | ^18.2.0 | UI framework | [React Docs](https://react.dev/) |
| **react-dom** | ^18.2.0 | React DOM renderer | [React DOM](https://react.dev/) |
| **typescript** | 5.9.2 | Type safety | [TypeScript](https://www.typescriptlang.org/) |
| **vite** | ^7.0.0 | Build tool and dev server | [Vite](https://vitejs.dev/) |

**Why React 18?**
- Concurrent rendering
- Automatic batching
- Suspense improvements
- Modern hooks API

**Why Vite?**
- Lightning-fast HMR (Hot Module Replacement)
- ESM-native development
- Optimized production builds
- Plugin ecosystem

### State Management

| Dependency | Version | Purpose | Why Chosen |
|------------|---------|---------|------------|
| **recoil** | ^0.7.7 | Global state management | Simple API, minimal boilerplate, great DX |
| **@apollo/client** | ^3.7.17 | GraphQL client & cache | Industry standard, normalized cache, excellent TypeScript support |

**Recoil Usage:**
- Atoms for primitive state
- Selectors for derived state
- Atom families for dynamic collections

**Apollo Client Usage:**
- GraphQL query/mutation execution
- Normalized cache management
- Optimistic UI updates
- GraphQL subscriptions (WebSocket)

### UI & Styling

| Dependency | Version | Purpose |
|------------|---------|---------|
| **@emotion/react** | ^11.11.1 | CSS-in-JS styling |
| **@emotion/styled** | ^11.11.0 | Styled components |
| **@tabler/icons-react** | ^3.31.0 | Icon library |
| **framer-motion** | ^11.18.0 | Animation library |

**Why Emotion?**
- Zero-runtime CSS extraction
- TypeScript support
- Theme capabilities
- Performance

### Forms & Validation

| Dependency | Version | Purpose |
|------------|---------|---------|
| **react-hook-form** | ^7.45.1 | Form state management |
| **@hookform/resolvers** | ^5.2.2 | Validation resolvers |
| **zod** | ^4.1.11 | Schema validation |

**Why React Hook Form?**
- Minimal re-renders
- Built-in validation
- Easy integration with Zod
- Excellent performance

### Rich Text & Documents

| Dependency | Version | Purpose |
|------------|---------|---------|
| **@blocknote/react** | ^0.31.1 | Rich text editor |
| **@tiptap/react** | 3.4.2 | Extensible editor framework |
| **marked** | ^17.0.1 | Markdown parser |
| **react-pdf** | ^4.1.6 | PDF rendering |

### Data Visualization

| Dependency | Version | Purpose |
|------------|---------|---------|
| **@nivo/core** | ^0.99.0 | Chart library core |
| **@nivo/line** | ^0.99.0 | Line charts |
| **@nivo/pie** | ^0.99.0 | Pie charts |
| **@xyflow/react** | ^12.4.2 | Workflow diagram visualization |

### Utilities

| Dependency | Version | Purpose |
|------------|---------|---------|
| **date-fns** | ^2.30.0 | Date manipulation |
| **lodash.*** | Various | Utility functions |
| **uuid** | ^9.0.0 | UUID generation |
| **immer** | ^10.1.1 | Immutable state updates |

## Major Backend Dependencies

### Core Framework

| Dependency | Version | Purpose | Documentation |
|------------|---------|---------|---------------|
| **@nestjs/core** | 11.1.9 | Application framework | [NestJS](https://nestjs.com/) |
| **@nestjs/common** | 11.1.9 | Common utilities | [NestJS Common](https://docs.nestjs.com/) |
| **typescript** | 5.9.2 | Type safety | [TypeScript](https://www.typescriptlang.org/) |

**Why NestJS?**
- Enterprise-ready architecture
- Dependency injection
- Modular structure
- Built-in testing utilities
- Extensive ecosystem

### GraphQL

| Dependency | Version | Purpose |
|------------|---------|---------|
| **graphql** | 16.8.1 | GraphQL implementation |
| **graphql-yoga** | 4.0.5 | GraphQL server |
| **@nestjs/graphql** | 12.1.1 | NestJS GraphQL integration |
| **@graphql-tools/schema** | 10.0.4 | Schema utilities |

**Why GraphQL Yoga?**
- Modern GraphQL server
- Built-in subscriptions
- Plugin system
- TypeScript support

### Database & ORM

| Dependency | Version | Purpose |
|------------|---------|---------|
| **typeorm** | 0.3.20 | ORM and query builder |
| **@nestjs/typeorm** | 11.0.0 | NestJS TypeORM integration |
| **pg** | 8.12.0 | PostgreSQL driver |
| **ioredis** | 5.6.0 | Redis client |

**Why TypeORM?**
- TypeScript-first ORM
- Migration support
- Decorators for entities
- Active record and data mapper patterns
- Good PostgreSQL support

**TypeORM Patches:**
- Custom patches applied for performance improvements
- See `patches/typeorm+0.3.20.patch`

### Job Queue

| Dependency | Version | Purpose |
|------------|---------|---------|
| **bullmq** | 5.40.0 | Job queue implementation |
| **@nestjs/bull** | (via BullMQ) | NestJS integration |

**Why BullMQ?**
- Redis-backed queuing
- Job retries and delays
- Job prioritization
- Rate limiting
- Job progress tracking
- Built-in UI (Bull Board)

### Authentication & Security

| Dependency | Version | Purpose |
|------------|---------|---------|
| **@nestjs/jwt** | 11.0.1 | JWT token handling |
| **@nestjs/passport** | 11.0.5 | Authentication strategies |
| **passport-jwt** | 4.0.1 | JWT strategy |
| **passport-google-oauth20** | 2.0.0 | Google OAuth |
| **passport-microsoft** | 2.1.0 | Microsoft OAuth |
| **@node-saml/passport-saml** | ^5.1.0 | SAML SSO |
| **bcrypt** | 5.1.1 | Password hashing |
| **otplib** | ^12.0.1 | 2FA/OTP generation |

### Email & Calendar Integration

| Dependency | Version | Purpose |
|------------|---------|---------|
| **googleapis** | 105.0.0 | Google APIs (Gmail, Calendar) |
| **@microsoft/microsoft-graph-client** | 3.0.7 | Microsoft Graph API |
| **imapflow** | 1.2.1 | IMAP email client |
| **mailparser** | 3.9.1 | Email parsing |
| **nodemailer** | ^7.0.11 | Email sending |

**Email Providers Supported:**
- Gmail (via Google APIs)
- Microsoft 365 (via Graph API)
- Generic IMAP/SMTP

### AI Integration

| Dependency | Version | Purpose |
|------------|---------|---------|
| **ai** | 5.0.52 | AI SDK core |
| **@ai-sdk/anthropic** | ^2.0.17 | Anthropic (Claude) integration |
| **@ai-sdk/openai** | ^2.0.30 | OpenAI (GPT) integration |
| **@ai-sdk/xai** | ^2.0.19 | XAI integration |
| **@e2b/code-interpreter** | ^1.0.4 | Code execution sandbox |

### File Storage

| Dependency | Version | Purpose |
|------------|---------|---------|
| **@aws-sdk/client-s3** | 3.967.0 | AWS S3 client |
| **@aws-sdk/client-sts** | 3.967.0 | AWS STS (credentials) |
| **sharp** | 0.32.6 | Image processing |
| **archiver** | 7.0.1 | File compression |

### Payment Processing

| Dependency | Version | Purpose |
|------------|---------|---------|
| **stripe** | 19.3.1 | Stripe payment API |

### Monitoring & Observability

| Dependency | Version | Purpose |
|------------|---------|---------|
| **@sentry/node** | ^10.27.0 | Error tracking (backend) |
| **@sentry/react** | ^10.27.0 | Error tracking (frontend) |
| **@opentelemetry/sdk-node** | ^0.202.0 | OpenTelemetry SDK |
| **@opentelemetry/exporter-prometheus** | ^0.211.0 | Prometheus metrics |

## Shared Dependencies

### twenty-shared Package

| Dependency | Purpose |
|------------|---------|
| **type-fest** | 4.10.1 | TypeScript utilities |
| **zod** | ^4.1.11 | Schema validation |

### twenty-ui Package

| Dependency | Purpose |
|------------|---------|
| **@emotion/react** | CSS-in-JS |
| **@tabler/icons-react** | Icons |
| **react** | UI framework |

## Development Dependencies

### Build Tools

| Dependency | Version | Purpose |
|------------|---------|---------|
| **nx** | 22.3.3 | Monorepo orchestration |
| **vite** | ^7.0.0 | Frontend bundler |
| **esbuild** | ^0.25.10 | Fast JavaScript bundler |
| **tsup** | (via package) | TypeScript build tool |

**Why Nx?**
- Monorepo task orchestration
- Caching for faster builds
- Dependency graph visualization
- Code generation schematics
- Plugin ecosystem

### Testing

| Dependency | Version | Purpose |
|------------|---------|---------|
| **jest** | 29.7.0 | Test runner |
| **@testing-library/react** | ^16.3.0 | React testing utilities |
| **@playwright/test** | ^1.56.1 | E2E testing |
| **vitest** | ^4.0.17 | Vite-native test runner |
| **msw** | ^2.12.7 | API mocking |

**Why Jest?**
- Industry standard
- Snapshot testing
- Coverage reports
- Great ecosystem

**Why Playwright?**
- Cross-browser E2E testing
- Auto-waiting
- Network interception
- Debugging tools

### Code Quality

| Dependency | Version | Purpose |
|------------|---------|---------|
| **eslint** | ^9.32.0 | Linting |
| **@typescript-eslint/parser** | ^8.39.0 | TypeScript ESLint |
| **prettier** | ^3.1.1 | Code formatting |
| **@lingui/cli** | ^5.1.2 | i18n extraction |

### Storybook

| Dependency | Version | Purpose |
|------------|---------|---------|
| **storybook** | ^10.1.11 | Component documentation |
| **@storybook/react-vite** | ^10.1.11 | React + Vite integration |
| **@storybook/test-runner** | ^0.24.2 | Storybook testing |

## External Services

### Required External Services

| Service | Purpose | Free Tier | Documentation |
|---------|---------|-----------|---------------|
| **PostgreSQL** | Database | Self-hosted | [PostgreSQL](https://www.postgresql.org/) |
| **Redis** | Cache & Queue | Self-hosted | [Redis](https://redis.io/) |

### Optional External Services

| Service | Purpose | Free Tier | Required For |
|---------|---------|-----------|--------------|
| **AWS S3** | File storage | 5GB | File attachments (production) |
| **Google Cloud** | OAuth & APIs | Free tier | Gmail/Calendar sync |
| **Microsoft Azure** | OAuth & Graph API | Free tier | Outlook/Calendar sync |
| **Stripe** | Payment processing | Test mode | Billing features |
| **Sentry** | Error tracking | 5K events/month | Error monitoring |

## Environment Configuration

### Minimum Environment (.env)

```env
# Database
PG_DATABASE_URL=postgres://postgres:postgres@localhost:5432/default

# Redis
REDIS_URL=redis://localhost:6379

# Server
PORT=3000
FRONT_BASE_URL=http://localhost:3001

# Secrets (generate with: openssl rand -base64 32)
ACCESS_TOKEN_SECRET=your_secret_here
REFRESH_TOKEN_SECRET=your_secret_here
LOGIN_TOKEN_SECRET=your_secret_here
FILE_TOKEN_SECRET=your_secret_here
API_TOKEN_SECRET=your_secret_here

# Storage
STORAGE_TYPE=local
STORAGE_LOCAL_PATH=.local-storage
```

### Full Environment Variables

See [Setup Guide](./02_Setup_and_Runbook.md#environment-variables-reference) for complete list.

## Dependency Management

### Package Resolution Strategy

**Resolutions in package.json:**
```json
{
  "resolutions": {
    "graphql": "16.8.1",
    "type-fest": "4.10.1",
    "typescript": "5.9.2"
  }
}
```

**Why resolutions?**
- Ensure consistent versions across packages
- Avoid dependency conflicts
- Security patches

### Patched Dependencies

Some dependencies are patched for bug fixes or performance:

```
patches/
├── typeorm+0.3.20.patch
├── @nestjs+graphql+12.1.1.patch
└── @graphql-yoga+nestjs+2.1.0.patch
```

**To apply patches:**
```bash
yarn install  # Patches are applied automatically
```

### Updating Dependencies

```bash
# Check for outdated packages
yarn outdated

# Update all packages
yarn upgrade-interactive

# Update specific package
yarn upgrade <package-name>

# After updates, run tests
npx nx run-many -t test
```

## System Resource Requirements

### Development

| Resource | Minimum | Recommended |
|----------|---------|-------------|
| **CPU** | 4 cores | 8+ cores |
| **RAM** | 8 GB | 16+ GB |
| **Disk** | 20 GB free | 50+ GB SSD |
| **Network** | Broadband | Broadband |

### Production (Per Instance)

| Component | CPU | RAM | Disk | Notes |
|-----------|-----|-----|------|-------|
| **Frontend** | 1 core | 512 MB | 5 GB | Can be CDN-hosted |
| **Backend API** | 2-4 cores | 4-8 GB | 10 GB | Scales horizontally |
| **Worker** | 2-4 cores | 4-8 GB | 10 GB | Multiple instances recommended |
| **PostgreSQL** | 4+ cores | 8-16 GB | 50+ GB SSD | Use managed service if possible |
| **Redis** | 1-2 cores | 2-4 GB | 10 GB | Persistent storage needed |

## Docker Images

### Official Images Used

```yaml
# PostgreSQL
postgres:14

# Redis
redis:6

# Node.js base
node:24-alpine
```

### Twenty Docker Images

```bash
# Build backend image
docker build -t twenty-server -f packages/twenty-server/Dockerfile .

# Build frontend image
docker build -t twenty-front -f packages/twenty-front/Dockerfile .
```

## Dependency Security

### Security Scanning

```bash
# Audit dependencies
yarn audit

# Check for vulnerabilities
yarn npm audit --all
```

### Known Security Considerations

- Keep Node.js updated to latest 24.x LTS
- Update dependencies monthly
- Review Dependabot alerts
- Use `npm audit fix` cautiously (test after fixes)

## Performance Considerations

### Frontend Bundle Size

**Target Sizes:**
- Initial bundle: < 500 KB (gzipped)
- Lazy-loaded routes: < 200 KB each
- Total initial load: < 2s on 3G

**Optimization Tools:**
- Vite code splitting
- React lazy loading
- Tree shaking
- Minification

### Backend Performance

**Key Metrics:**
- API response time: < 200ms (p95)
- Database query time: < 50ms (average)
- Job processing: < 5s per job (average)

**Optimization:**
- Database indexing
- Redis caching
- Query optimization
- Connection pooling

## License Compliance

**Primary License:** AGPL-3.0

**Dependency Licenses:**
- Most dependencies: MIT, Apache-2.0 (permissive)
- Some GPL/LGPL libraries (compatible with AGPL)
- No known license conflicts

**To audit licenses:**
```bash
npx license-checker --summary
```

---

**Next:** See [Known Issues & Technical Debt](./08_Known_Issues_Tech_Debt.md) for current limitations.
