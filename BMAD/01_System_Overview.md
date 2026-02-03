# System Overview

## What is Twenty?

Twenty is the #1 open-source Customer Relationship Management (CRM) system designed to solve three fundamental problems with traditional CRM solutions:

### Core Problems Addressed

1. **Cost & Vendor Lock-in**
   - Traditional CRMs (Salesforce, HubSpot) are expensive with escalating licensing costs
   - Proprietary data storage creates vendor lock-in
   - Companies lose control over their customer data
   - **Twenty's Solution:** Open-source, self-hosted CRM with complete data ownership

2. **Outdated User Experience**
   - Legacy CRMs built on old architectural patterns
   - Clunky interfaces that haven't evolved with modern UX expectations
   - **Twenty's Solution:** Fresh start inspired by modern tools (Notion, Airtable, Linear) with cohesive, contemporary UX

3. **Limited Extensibility**
   - Closed ecosystems with restricted customization
   - Expensive custom development
   - **Twenty's Solution:** Open architecture with plugin ecosystem, GraphQL/REST APIs, and SDK for custom apps

## Who Uses Twenty?

### Primary User Personas

1. **Small to Mid-Size Businesses (SMBs)**
   - Need: Cost-effective CRM without vendor lock-in
   - Benefit: Self-hosted solution with enterprise features at zero licensing cost

2. **Enterprise Organizations**
   - Need: On-premise deployment with data sovereignty
   - Benefit: Complete control over data, security, and infrastructure

3. **Developers & Technical Teams**
   - Need: Customizable CRM with extensibility
   - Benefit: Full source code access, APIs, SDK, and plugin architecture

4. **Sales & Marketing Teams**
   - Need: Flexible workflows and pipeline management
   - Benefit: Customizable objects, fields, views, and automation

5. **Privacy-Conscious Organizations**
   - Need: Data privacy and regulatory compliance
   - Benefit: Self-hosted with complete data control and audit trails

## Key Features

### 1. Core CRM Capabilities

#### Contact Management
- **People Module:** Individual contact records with rich profiles
- **Company Module:** Organization records with hierarchical relationships
- **Relationship Tracking:** Link people to companies with role definitions
- **Activity Timeline:** Unified view of all interactions per contact
- **Custom Fields:** Add unlimited custom fields to contacts and companies

#### Sales Pipeline
- **Opportunity Management:** Track deals through sales stages
- **Kanban Board Views:** Visual pipeline with drag-and-drop
- **Deal Stages:** Customizable stages with probability tracking
- **Value Tracking:** Revenue forecasting and won/lost analysis
- **Activity Linking:** Connect tasks, emails, and meetings to opportunities

#### Task & Activity Management
- **Task Creation:** Assign tasks to team members with due dates
- **Activity Types:** Calls, meetings, emails, notes, custom activities
- **Reminders:** Automated notifications for upcoming tasks
- **Completion Tracking:** Status updates and activity history
- **Team Collaboration:** Share notes and activities across workspace

### 2. Data Customization & Flexibility

#### Custom Objects
- Create entirely new entity types beyond standard CRM objects
- Define relationships between custom and standard objects
- Full CRUD operations via GraphQL API
- Permission management at object level

#### Custom Fields
- **Supported Types:** Text, Number, Date, Select, Multi-select, Boolean, Link, Currency, Phone, Email, URL, Rating, Rich Text
- **Field Configuration:** Required/optional, default values, validation rules
- **Dynamic Schema:** Add/remove fields without code deployment
- **Migration-Free:** Schema changes don't require database migrations

#### Object Metadata System
- Dynamic data model management
- Runtime schema modifications
- Type-safe field definitions
- Automated GraphQL schema generation

### 3. Views & User Interface

#### Multiple View Types
- **Table View:** Spreadsheet-like interface with inline editing
- **Kanban View:** Board-style visualization for pipelines
- **Calendar View:** Date-based visualizations for activities
- **List View:** Compact list with quick actions

#### Advanced Filtering
- Complex filter expressions with AND/OR logic
- Filter by any field type
- Save filters as views
- Share filters with team

#### Sorting & Grouping
- Multi-level sorting
- Group by any field
- Customizable column order
- Frozen columns for key fields

#### Full-Text Search
- Search across all record types
- Field-level search
- Fuzzy matching
- Search result ranking

### 4. Workflow Automation

#### Visual Workflow Builder
- Drag-and-drop interface for building automations
- Visual flow representation with nodes and connections
- Test mode for validation before activation

#### Trigger Types
- **Record Triggers:** On create, update, delete
- **Time-Based Triggers:** Schedule-based (cron), date field triggers
- **Manual Triggers:** User-initiated workflows
- **Webhook Triggers:** External system events

#### Action Types
- **Record Actions:** Create, update, delete records
- **Communication:** Send emails, notifications
- **Data Operations:** Calculate values, update fields
- **Integrations:** Call external APIs, webhooks
- **Logic Functions:** Custom code execution (serverless)

#### Workflow Execution
- Background job processing with BullMQ
- Retry logic for failed actions
- Execution history and logs
- Performance monitoring

### 5. Integration & Connectivity

#### Email & Calendar Integration
- **Gmail Integration:** OAuth connection, email sync, contact auto-creation
- **Microsoft 365:** Outlook email and calendar sync
- **IMAP/SMTP:** Generic email provider support
- **Calendar Sync:** Two-way sync with Google Calendar and Outlook
- **Email Threading:** Automatic conversation grouping
- **Contact Matching:** Auto-link emails to CRM contacts

#### API Access
- **GraphQL API:** Flexible query/mutation interface with introspection
- **REST API:** Standard CRUD endpoints for integrations
- **Webhooks:** Event subscriptions for external systems
- **Rate Limiting:** Configurable throttling
- **API Keys:** Workspace-specific authentication

#### Browser Extension
- Chrome extension for CRM data capture
- LinkedIn profile import
- Web form integration
- Context menu actions

#### Third-Party Integrations
- **Zapier:** Pre-built app in Zapier marketplace
- **Cal.com:** Meeting scheduling integration
- **Stripe:** Payment and subscription management
- **AWS S3:** File storage backend

### 6. Security & Access Control

#### Authentication
- **Email/Password:** Standard authentication
- **Google OAuth:** Social login
- **Microsoft OAuth:** Azure AD integration
- **SAML SSO:** Enterprise single sign-on
- **Two-Factor Authentication (2FA):** TOTP-based 2FA

#### Authorization
- **Custom Roles:** Define role-based access control (RBAC)
- **Granular Permissions:** Field-level and object-level permissions
- **Record Ownership:** Permission inheritance from owners
- **Workspace Isolation:** Multi-tenant data separation

#### Audit & Compliance
- Audit log for all data changes
- User activity tracking
- Data export capabilities
- GDPR compliance features

### 7. Developer Extensibility

#### Twenty SDK
- **Type-Safe Client:** Auto-generated TypeScript client
- **CLI Tool:** `create-twenty-app` for scaffolding
- **Logic Functions:** Serverless functions for custom business logic
- **Front Components:** UI extensions for custom interfaces
- **Hot Reload:** Development mode with auto-sync

#### Custom App Development
- Deploy custom apps to workspace
- Access Twenty APIs within app context
- Extend data model with custom objects
- Build custom workflows and automations

### 8. Advanced Capabilities

#### AI Integration
- **AI Providers:** Anthropic (Claude), OpenAI (GPT), XAI
- **Code Interpreter:** E2B code execution sandbox
- **AI-Powered Features:** Smart suggestions, data enrichment
- **Tool Calling:** AI agents can interact with CRM data

#### Analytics & Dashboards
- Custom dashboard builder
- Data visualization (line charts, pie charts, bar charts)
- Metrics and KPIs
- Report scheduling
- Export capabilities

#### File Management
- Attach files to any record
- S3-compatible storage backends (AWS S3, MinIO, local)
- Image preview and thumbnails
- File versioning

#### Rich Text Editing
- BlockNote-based editor
- Markdown support
- Inline images
- Export to DOCX/PDF

## Core Workflows

### Workflow 1: Lead Management Lifecycle

```
Lead Capture → Contact Creation → Company Association →
Opportunity Creation → Activity Tracking → Deal Closure
```

**Steps:**
1. **Lead Capture:** Email signature, LinkedIn profile, or manual entry
2. **Contact Record:** Create person record with details
3. **Company Linking:** Associate person with company record
4. **Opportunity Creation:** Create deal in pipeline
5. **Activity Tracking:** Log calls, emails, meetings
6. **Pipeline Movement:** Move through stages to closure
7. **Analysis:** Review won/lost reasons and metrics

### Workflow 2: Email-Driven CRM Updates

```
Email Received → Contact Auto-Created → Company Matched →
Activity Logged → Workflow Triggered
```

**Steps:**
1. **Connected Account:** Gmail/Outlook connected to workspace
2. **Email Sync:** Emails imported to messaging module
3. **Contact Extraction:** Sender email matched or created
4. **Company Detection:** Domain-based company matching
5. **Activity Creation:** Email logged in timeline
6. **Automation:** Workflows triggered by new email event

### Workflow 3: Sales Pipeline Management

```
Opportunity Creation → Stage Progression → Activity Assignment →
Team Collaboration → Revenue Forecasting → Closure
```

**Steps:**
1. **Create Opportunity:** Link to contact and company
2. **Set Value:** Estimated deal value and close date
3. **Kanban Movement:** Drag through pipeline stages
4. **Task Assignment:** Create follow-up tasks for team
5. **Collaboration:** Add notes and internal comments
6. **Forecasting:** Track pipeline value by stage
7. **Close Deal:** Mark as won/lost with reason

### Workflow 4: Workflow Automation Setup

```
Define Trigger → Configure Actions → Test Workflow →
Activate → Monitor Execution
```

**Steps:**
1. **Workflow Builder:** Open automation interface
2. **Add Trigger:** Select event type (e.g., "Opportunity created")
3. **Add Actions:** Chain actions (e.g., "Send email", "Create task")
4. **Configure Logic:** Set conditions and branching
5. **Test Mode:** Validate with sample data
6. **Activate:** Turn on workflow
7. **Monitor:** Check execution logs and retry failures

### Workflow 5: Custom CRM Configuration

```
Define Custom Object → Add Custom Fields → Configure Views →
Set Permissions → Build Workflows
```

**Steps:**
1. **Object Creation:** Define new entity type (e.g., "Project")
2. **Field Configuration:** Add relevant fields with types
3. **Relationship Setup:** Link to existing objects
4. **View Creation:** Create table/kanban views
5. **Permission Assignment:** Set role-based access
6. **Workflow Integration:** Build automations for new object

## Deployment Options

### Local Development
- Docker Compose with PostgreSQL and Redis
- Hot-reload for frontend and backend
- Seeded development data

### Self-Hosted Production
- **Docker:** Single or multi-container deployment
- **Kubernetes:** Helm charts for scalable deployments
- **Bare Metal:** Direct installation with systemd

### Scalability Features
- Horizontal scaling with worker processes
- Redis caching layer
- PostgreSQL read replicas
- Optional ClickHouse for analytics
- Multi-workspace support with tenant isolation

## Project Maturity & Roadmap

### Current Status (v0.2.1)
- **Stability:** Alpha release, suitable for early adopters
- **Feature Completeness:** Core CRM features implemented
- **Production Usage:** Used by early adopter companies
- **Breaking Changes:** Possible in future releases

### Roadmap Highlights
- Enhanced plugin marketplace
- Mobile applications (iOS/Android)
- Advanced reporting and BI
- More third-party integrations
- Improved performance and scalability
- GraphQL federation support

### Community & Support
- **GitHub Stars:** 17,600+
- **Contributors:** 1,000+
- **Discord:** Active community support
- **Documentation:** Comprehensive docs at docs.twenty.com
- **Releases:** Frequent updates with changelog

## Unique Value Proposition

### Compared to Commercial CRMs (Salesforce, HubSpot)

| Aspect | Twenty | Commercial CRMs |
|--------|--------|-----------------|
| **Cost** | Free (AGPL-3.0) | $25-$300+ per user/month |
| **Data Ownership** | Complete control | Vendor-hosted |
| **Customization** | Full source access | Limited to platform |
| **Vendor Lock-in** | None | High |
| **Deployment** | Self-hosted | Cloud-only |
| **API Access** | Unlimited | Rate-limited/tiered |
| **Extensibility** | SDK, plugins | Marketplace apps |
| **Modern UX** | Built 2023+ | Legacy interfaces |

### Compared to Other Open-Source CRMs

| Aspect | Twenty | SuiteCRM/EspoCRM |
|--------|--------|------------------|
| **Tech Stack** | Modern (React, NestJS, GraphQL) | Legacy (PHP, jQuery) |
| **Architecture** | Microservices-ready | Monolithic |
| **User Experience** | Notion/Linear-inspired | Traditional |
| **Developer Experience** | TypeScript, hot reload, SDK | PHP, manual deployment |
| **Extensibility** | Plugin ecosystem (planned) | Module system |
| **Active Development** | Very active | Moderate |

## Summary

Twenty is positioned as a **modern, developer-friendly, open-source CRM** that combines:
- ✅ **Enterprise features** (RBAC, SSO, workflows, APIs)
- ✅ **Modern UX** (inspired by Notion, Airtable, Linear)
- ✅ **Complete ownership** (self-hosted, open source)
- ✅ **Extensibility** (SDK, APIs, plugins)
- ✅ **No vendor lock-in** (data portability, open standards)
- ✅ **Active community** (frequent updates, responsive maintainers)

The project is best suited for teams that value data ownership, require customization beyond typical SaaS limitations, or need a self-hosted solution for regulatory/security requirements.
