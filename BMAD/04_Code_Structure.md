# Code Structure & Module Breakdown

## Repository Organization

Twenty uses an **Nx monorepo** structure with Yarn 4 workspaces. The repository is organized into multiple packages with clear separation of concerns.

### Root Directory Structure

```
twenty/
├── packages/                    # All application packages
│   ├── twenty-front/           # React frontend
│   ├── twenty-server/          # NestJS backend
│   ├── twenty-ui/              # Shared UI component library
│   ├── twenty-shared/          # Shared types and utilities
│   ├── twenty-emails/          # Email templates (React Email)
│   ├── twenty-sdk/             # SDK for building apps
│   ├── twenty-apps/            # Sample apps
│   ├── twenty-cli/             # CLI tools
│   ├── create-twenty-app/      # App scaffolding tool
│   ├── twenty-zapier/          # Zapier integration
│   ├── twenty-website/         # Next.js website
│   ├── twenty-docs/            # Documentation
│   ├── twenty-e2e-testing/     # Playwright E2E tests
│   ├── twenty-utils/           # Utilities and scripts
│   └── twenty-eslint-rules/    # Custom ESLint rules
├── .github/                    # GitHub Actions CI/CD
├── nx.json                     # Nx workspace configuration
├── package.json                # Root package configuration
├── tsconfig.base.json          # Base TypeScript config
├── .eslintrc.js               # ESLint configuration
├── yarn.lock                   # Dependency lock file
└── README.md                   # Project documentation
```

## Package Dependency Graph

```
twenty-shared (foundation)
    ↓
    ├── twenty-ui
    ├── twenty-sdk
    └── twenty-emails
        ↓
        ├── twenty-front → twenty-ui, twenty-sdk, twenty-shared
        ├── twenty-server → twenty-emails, twenty-shared
        ├── twenty-website
        └── twenty-zapier
```

**Build Order:**
1. `twenty-shared` (must build first)
2. `twenty-ui`, `twenty-emails`, `twenty-sdk` (parallel)
3. `twenty-front`, `twenty-server` (parallel)

## Frontend Package (twenty-front)

### Directory Structure

```
packages/twenty-front/
├── src/
│   ├── modules/                # Feature modules
│   │   ├── auth/              # Authentication
│   │   ├── object-record/     # Record CRUD operations
│   │   ├── object-metadata/   # Metadata management
│   │   ├── views/             # View configuration
│   │   ├── workflow/          # Workflow builder
│   │   ├── settings/          # Settings UI
│   │   ├── activities/        # Activity timeline
│   │   ├── companies/         # Company-specific views
│   │   ├── people/            # Person-specific views
│   │   ├── opportunities/     # Opportunity views
│   │   ├── tasks/             # Task management
│   │   ├── favorites/         # Favorites system
│   │   ├── search/            # Search functionality
│   │   ├── command-menu/      # Command palette
│   │   ├── ui/                # UI components
│   │   ├── accounts/          # Connected accounts
│   │   └── ...
│   ├── pages/                 # React Router pages
│   │   ├── auth/              # Login, signup pages
│   │   ├── objects/           # Object list/detail pages
│   │   ├── settings/          # Settings pages
│   │   └── not-found/         # 404 page
│   ├── generated/             # Generated GraphQL types
│   ├── __generated__/         # GraphQL operations
│   ├── state/                 # Recoil state atoms
│   ├── testing/               # Test utilities
│   ├── utils/                 # Utility functions
│   └── App.tsx                # Root component
├── public/                    # Static assets
├── vite.config.ts            # Vite configuration
├── tsconfig.json             # TypeScript config
├── jest.config.mjs           # Jest configuration
└── package.json              # Package dependencies
```

### Key Frontend Modules

#### 1. **auth/** - Authentication
- Login/signup components
- OAuth flow handlers
- Token management
- Password reset

**Key Files:**
- `src/modules/auth/sign-in-up/components/SignInUpForm.tsx`
- `src/modules/auth/states/authProvidersState.ts`
- `src/modules/auth/hooks/useAuth.ts`

#### 2. **object-record/** - Record Management
- Generic record display components
- CRUD operations
- Inline editing
- Record selection

**Key Files:**
- `src/modules/object-record/record-table/RecordTable.tsx`
- `src/modules/object-record/record-board/RecordBoard.tsx`
- `src/modules/object-record/hooks/useCreateOneRecord.ts`

#### 3. **object-metadata/** - Metadata System
- Object and field metadata hooks
- Dynamic form generation
- Schema management

**Key Files:**
- `src/modules/object-metadata/hooks/useObjectMetadataItem.ts`
- `src/modules/object-metadata/hooks/useFieldMetadataItem.ts`

#### 4. **views/** - View Configuration
- View persistence (filters, sorts, grouping)
- View picker component
- View creation/editing

**Key Files:**
- `src/modules/views/hooks/useViewBar.ts`
- `src/modules/views/components/ViewBar.tsx`

#### 5. **workflow/** - Workflow Builder
- Visual workflow editor
- Trigger and action configuration
- Workflow execution status

**Key Files:**
- `src/modules/workflow/components/WorkflowDiagram.tsx`
- `src/modules/workflow/hooks/useWorkflow.ts`

### Frontend State Management

**Recoil Atoms (Global State):**
```typescript
// Example atoms
currentWorkspaceState        // Active workspace
currentUserState             // Logged-in user
viewFieldsState              // View configuration
selectedRecordIdsState       // Selected records in tables
```

**Component State:**
- Form state with React Hook Form
- Local UI state with useState
- Derived state with useMemo

**GraphQL Cache:**
- Apollo Client manages query cache
- Automatic cache updates on mutations
- Optimistic UI updates

## Backend Package (twenty-server)

### Directory Structure

```
packages/twenty-server/
├── src/
│   ├── engine/                      # Core engine
│   │   ├── core-modules/           # Core functionality
│   │   │   ├── workspace/          # Workspace management
│   │   │   ├── user/               # User management
│   │   │   ├── auth/               # Authentication
│   │   │   ├── api-key/            # API key management
│   │   │   ├── file-storage/       # File storage abstraction
│   │   │   ├── billing/            # Stripe billing
│   │   │   ├── sso/                # Single sign-on
│   │   │   ├── email/              # Email sending
│   │   │   ├── search/             # Full-text search
│   │   │   ├── audit/              # Audit logging
│   │   │   ├── message-queue/      # BullMQ integration
│   │   │   └── ...
│   │   ├── metadata-modules/       # Metadata system
│   │   │   ├── object-metadata/    # Object definitions
│   │   │   ├── field-metadata/     # Field definitions
│   │   │   ├── relation-metadata/  # Relationship definitions
│   │   │   └── ...
│   │   └── twenty-orm/             # Custom ORM layer
│   ├── modules/                    # Business modules
│   │   ├── calendar/               # Calendar integration
│   │   │   ├── calendar-event/     # Event records
│   │   │   └── calendar-sync/      # Google/Outlook sync
│   │   ├── messaging/              # Email/messaging
│   │   │   ├── message/            # Message records
│   │   │   ├── message-channel/    # Email accounts
│   │   │   ├── message-import/     # Email import jobs
│   │   │   └── message-sync/       # Sync logic
│   │   ├── workflow/               # Workflow automation
│   │   │   ├── workflow/           # Workflow definitions
│   │   │   ├── workflow-trigger/   # Trigger configuration
│   │   │   ├── workflow-action/    # Action execution
│   │   │   └── workflow-executor/  # Execution engine
│   │   ├── company/                # Company CRM module
│   │   ├── person/                 # Person CRM module
│   │   ├── opportunity/            # Sales opportunities
│   │   ├── task/                   # Task management
│   │   ├── activity/               # Activity tracking
│   │   ├── favorite/               # Favorites
│   │   ├── attachment/             # File attachments
│   │   ├── view/                   # View persistence
│   │   ├── connected-account/      # External account connections
│   │   ├── contact-creation-manager/ # Auto-create contacts
│   │   └── ...
│   ├── database/                   # Database configuration
│   │   ├── typeorm/                # TypeORM setup
│   │   │   ├── core/               # Core schema migrations
│   │   │   └── metadata/           # Metadata schema migrations
│   │   └── clickhouse/             # ClickHouse analytics (optional)
│   ├── queue-worker/               # Worker process
│   │   └── queue-worker.module.ts  # Worker main module
│   ├── command/                    # CLI commands
│   │   └── command.module.ts       # Command runner
│   ├── main.ts                     # Backend entry point
│   └── app.module.ts               # Root application module
├── scripts/                        # Utility scripts
├── patches/                        # Patch files for dependencies
├── jest.config.mjs                # Jest configuration
├── tsconfig.json                  # TypeScript config
└── package.json                   # Package dependencies
```

### Key Backend Modules

#### 1. **engine/core-modules/** - Core Engine

**workspace/**
- Multi-tenant workspace management
- Workspace creation and configuration
- Member management

**auth/**
- JWT authentication
- OAuth providers (Google, Microsoft)
- SAML SSO
- 2FA

**user/**
- User account management
- Profile updates
- Settings

**file-storage/**
- Abstraction over storage backends
- S3 integration
- Local file storage
- Upload/download handling

**Key Files:**
- `src/engine/core-modules/auth/services/auth.service.ts:234` - Authentication logic
- `src/engine/core-modules/workspace/services/workspace.service.ts:156` - Workspace operations
- `src/engine/core-modules/file-storage/file-storage.service.ts:89` - File storage abstraction

#### 2. **engine/metadata-modules/** - Metadata System

**object-metadata/**
- Define custom objects dynamically
- CRUD operations on object definitions
- Schema validation

**field-metadata/**
- Define fields on objects
- Field types and constraints
- Dynamic schema updates

**relation-metadata/**
- Define relationships between objects
- One-to-many, many-to-many relations
- Cascade rules

**Key Files:**
- `src/engine/metadata-modules/object-metadata/object-metadata.service.ts:401` - Object operations
- `src/engine/metadata-modules/field-metadata/field-metadata.service.ts:578` - Field operations

#### 3. **modules/messaging/** - Email Integration

**message-import/**
- Background jobs for email import
- Gmail API integration
- Outlook Graph API integration
- IMAP fallback

**message-sync/**
- Incremental sync logic
- Message deduplication
- Thread detection

**contact-creation-manager/**
- Extract contacts from emails
- Auto-create person records
- Company matching by domain

**Key Files:**
- `src/modules/messaging/message-import-manager/services/message-import-manager.service.ts:302` - Import orchestration
- `src/modules/messaging/message-sync/services/gmail-message-sync.service.ts:198` - Gmail sync
- `src/modules/contact-creation-manager/services/create-contact.service.ts:145` - Contact creation

#### 4. **modules/workflow/** - Workflow Engine

**workflow/**
- Workflow definition storage
- Workflow CRUD operations
- Workflow activation/deactivation

**workflow-trigger/**
- Event-based triggers
- Schedule-based triggers
- Manual triggers

**workflow-executor/**
- Workflow execution engine
- Action chaining
- Error handling and retries

**Key Files:**
- `src/modules/workflow/workflow-executor/workflow-executor.service.ts:421` - Execution logic
- `src/modules/workflow/workflow-trigger/workflow-trigger.service.ts:234` - Trigger handling

#### 5. **modules/calendar/** - Calendar Integration

**calendar-sync/**
- Google Calendar sync
- Outlook Calendar sync
- Event creation/updates

**calendar-event/**
- Event storage
- Participant management
- Conflict detection

**Key Files:**
- `src/modules/calendar/calendar-sync/services/calendar-sync.service.ts:287` - Sync orchestration

### Backend Module Pattern

Each module follows a consistent structure:

```
module-name/
├── module-name.module.ts          # NestJS module definition
├── module-name.controller.ts      # REST endpoints (if any)
├── module-name.resolver.ts        # GraphQL resolvers
├── module-name.service.ts         # Business logic
├── module-name.repository.ts      # Data access (if using repository pattern)
├── dto/                           # Data Transfer Objects
│   ├── create-module-name.dto.ts
│   └── update-module-name.dto.ts
├── entities/                      # TypeORM entities
│   └── module-name.entity.ts
├── types/                         # TypeScript types
├── jobs/                          # Background jobs
│   └── module-name.job.ts
└── __tests__/                     # Unit tests
    └── module-name.service.spec.ts
```

## Shared Packages

### twenty-shared

Common types and utilities used across packages:

```
packages/twenty-shared/
├── src/
│   ├── types/                 # Shared TypeScript types
│   ├── utils/                 # Utility functions
│   │   ├── isDefined.ts      # Type guards
│   │   ├── isNonEmptyString.ts
│   │   └── isNonEmptyArray.ts
│   └── index.ts              # Barrel export
└── package.json
```

**Usage:**
```typescript
import { isDefined, isNonEmptyString } from 'twenty-shared';
```

### twenty-ui

Shared React component library:

```
packages/twenty-ui/
├── src/
│   ├── display/              # Display components (Avatar, Chip, Tag)
│   ├── input/                # Input components (Button, Input, Select)
│   ├── navigation/           # Navigation components (Menu, Tabs)
│   ├── layout/               # Layout components (Card, Modal)
│   ├── feedback/             # Feedback components (Toast, Spinner)
│   ├── theme/                # Theme configuration
│   └── index.ts              # Component exports
└── package.json
```

**Usage:**
```typescript
import { Button, Input, Modal } from 'twenty-ui';
```

### twenty-sdk

SDK for building custom Twenty apps:

```
packages/twenty-sdk/
├── src/
│   ├── client/               # API client
│   ├── types/                # SDK types
│   ├── hooks/                # React hooks
│   └── utils/                # Helper utilities
└── package.json
```

## Data Flow Through Modules

### Example: Creating a Person Record

```
1. Frontend (twenty-front)
   └─> src/modules/people/hooks/useCreatePerson.ts
       └─> GraphQL Mutation: createPerson
           │
2. Backend API (twenty-server)
   └─> src/modules/person/person.resolver.ts:142
       └─> Calls: PersonService.create()
           └─> src/modules/person/person.service.ts:234
               ├─> Validates input
               ├─> TypeORM save to database
               ├─> Emits event: person.created
               └─> Returns created person
                   │
3. Event System
   └─> Event: person.created
       └─> Triggers workflow engine
           └─> src/modules/workflow/workflow-executor/workflow-executor.service.ts
               └─> Queues workflow jobs in BullMQ
                   │
4. Background Worker
   └─> Processes workflow jobs
       └─> Executes actions (send email, create task, etc.)
```

## Testing Structure

### Frontend Tests

```
packages/twenty-front/src/
└── modules/
    └── module-name/
        ├── __tests__/                    # Unit tests
        │   └── Component.test.tsx
        ├── __stories__/                  # Storybook stories
        │   └── Component.stories.tsx
        └── hooks/
            └── __tests__/
                └── useHook.test.ts
```

**Test Command:**
```bash
npx jest src/modules/module-name/__tests__/Component.test.tsx \
  --config=packages/twenty-front/jest.config.mjs
```

### Backend Tests

```
packages/twenty-server/src/
└── modules/
    └── module-name/
        └── __tests__/
            ├── module-name.service.spec.ts    # Unit tests
            └── module-name.integration.spec.ts # Integration tests
```

**Test Commands:**
```bash
# Unit tests
npx nx test twenty-server

# Integration tests
npx nx run twenty-server:test:integration:with-db-reset
```

## Build & Dependency Management

### Nx Build Graph

```
nx.json defines:
- Build targets for each package
- Test targets
- Lint targets
- Task dependencies (e.g., build depends on twenty-shared build)
```

**View Build Graph:**
```bash
npx nx graph
```

### Package Dependencies

**package.json workspaces:**
```json
{
  "workspaces": [
    "packages/twenty-front",
    "packages/twenty-server",
    "packages/twenty-shared",
    // ... other packages
  ]
}
```

**Workspace References:**
```json
{
  "dependencies": {
    "twenty-shared": "workspace:*",
    "twenty-ui": "workspace:*"
  }
}
```

## Code Navigation Tips

### Finding Core Logic

**Frontend:**
- **Record Operations:** `src/modules/object-record/`
- **Authentication:** `src/modules/auth/`
- **Views:** `src/modules/views/`
- **Settings:** `src/modules/settings/`

**Backend:**
- **Authentication:** `src/engine/core-modules/auth/`
- **Business Modules:** `src/modules/[entity-name]/`
- **Metadata:** `src/engine/metadata-modules/`
- **Job Processing:** `src/queue-worker/`

### Key Entry Points

**Frontend:**
- `src/App.tsx` - Application root
- `src/pages/` - Route definitions
- `src/modules/` - Feature modules

**Backend:**
- `src/main.ts` - API server entry
- `src/queue-worker/queue-worker.module.ts` - Worker entry
- `src/app.module.ts` - Root module with all imports

### Finding Specific Features

**Search by Feature:**
```bash
# Find workflow-related code
rg "workflow" --type ts

# Find authentication logic
rg "authenticate" --type ts -g "*.service.ts"

# Find GraphQL resolvers
rg "@Resolver" --type ts
```

## Configuration Files

### TypeScript Configuration

```
tsconfig.base.json          # Base config for all packages
packages/*/tsconfig.json    # Package-specific config
```

### ESLint Configuration

```
.eslintrc.js               # Root ESLint config
packages/*/.eslintrc.js    # Package-specific overrides
```

### Vite Configuration

```
packages/twenty-front/vite.config.ts  # Frontend build config
```

### Jest Configuration

```
packages/*/jest.config.mjs   # Package-specific Jest config
```

## Summary

The Twenty codebase is well-organized with:
- ✅ Clear separation between frontend and backend
- ✅ Shared code extracted into reusable packages
- ✅ Consistent module structure within packages
- ✅ Nx for monorepo orchestration
- ✅ TypeScript throughout for type safety
- ✅ Comprehensive test coverage structure

**Navigation Quick Reference:**
- Frontend features → `packages/twenty-front/src/modules/`
- Backend business logic → `packages/twenty-server/src/modules/`
- Core engine → `packages/twenty-server/src/engine/core-modules/`
- Shared utilities → `packages/twenty-shared/src/`
- UI components → `packages/twenty-ui/src/`

---

**Next:** See [API Documentation](./05_API_Documentation.md) for endpoint reference.
