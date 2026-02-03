# Twenty CRM - Project Documentation

**Version:** 0.2.1
**License:** AGPL-3.0
**Last Updated:** February 2026
**Documentation Generated:** 2026-02-03

---

## 📑 Table of Contents

| # | Document | Description | Key Topics |
|---|----------|-------------|------------|
| 1 | **[System Overview](./01_System_Overview.md)** | What Twenty does and who it's for | Purpose, user personas, features, core workflows, value proposition |
| 2 | **[Setup & Runbook](./02_Setup_and_Runbook.md)** | Getting started and operations | Prerequisites, installation, environment variables, testing, deployment |
| 3 | **[Architecture](./03_Architecture.md)** | System design and data flow | Components, diagrams, auth flow, scalability, security |
| 4 | **[Code Structure](./04_Code_Structure.md)** | Navigating the codebase | Folder structure, modules, key files, data flow patterns |
| 5 | **[API Documentation](./05_API_Documentation.md)** | API reference and examples | GraphQL/REST endpoints, authentication, error handling |
| 6 | **[Workflows & Examples](./06_Workflows_Examples.md)** | Practical usage scenarios | Lead management, pipeline, email sync, automation, custom objects |
| 7 | **[Dependencies & Environment](./07_Dependencies_Environment.md)** | Tech stack details | Runtime requirements, libraries, external services, Docker |
| 8 | **[Known Issues & Technical Debt](./08_Known_Issues_Tech_Debt.md)** | Risks and limitations | Maturity assessment, bugs, security concerns, improvement roadmap |

---

## 🗺️ Reading Guide

**New to the project?** Follow this path:
1. Start with [System Overview](./01_System_Overview.md) → Understand what Twenty does
2. Then [Architecture](./03_Architecture.md) → See how it's built
3. Then [Setup & Runbook](./02_Setup_and_Runbook.md) → Get it running locally

**Contributing code?** Focus on:
1. [Code Structure](./04_Code_Structure.md) → Navigate the codebase
2. [API Documentation](./05_API_Documentation.md) → Understand the API
3. [Known Issues](./08_Known_Issues_Tech_Debt.md) → See what needs work

**Integrating with Twenty?** Check:
1. [API Documentation](./05_API_Documentation.md) → Endpoints and auth
2. [Workflows & Examples](./06_Workflows_Examples.md) → Code samples

**Deploying to production?** Review:
1. [Setup & Runbook](./02_Setup_and_Runbook.md) → Configuration and deployment
2. [Dependencies](./07_Dependencies_Environment.md) → Requirements
3. [Known Issues](./08_Known_Issues_Tech_Debt.md) → Risk assessment

---

## Document Navigation

This documentation provides comprehensive coverage of the Twenty CRM project following BMAD standards.

### 📚 Documentation Contents

1. **[System Overview](./01_System_Overview.md)** - Project purpose, target users, and key features
2. **[Setup & Runbook](./02_Setup_and_Runbook.md)** - Installation, configuration, and operations
3. **[Architecture](./03_Architecture.md)** - System design, components, and data flow
4. **[Code Structure](./04_Code_Structure.md)** - Repository organization and module breakdown
5. **[API Documentation](./05_API_Documentation.md)** - Endpoints, requests, and responses
6. **[Workflows & Examples](./06_Workflows_Examples.md)** - Common use cases and examples
7. **[Dependencies & Environment](./07_Dependencies_Environment.md)** - Tech stack and requirements
8. **[Known Issues & Technical Debt](./08_Known_Issues_Tech_Debt.md)** - Current limitations and improvements

### 🎯 Quick Links

- **Official Website:** https://www.twenty.com
- **Documentation:** https://docs.twenty.com
- **GitHub Repository:** https://github.com/twentyhq/twenty
- **Discord Community:** https://discord.gg/cx5n4Jzs57
- **Roadmap:** https://github.com/orgs/twentyhq/projects/1

### 🚀 Quick Start

```bash
# Clone the repository
git clone https://github.com/twentyhq/twenty.git
cd twenty

# Install dependencies
yarn install

# Start development environment
yarn start
```

See [Setup & Runbook](./02_Setup_and_Runbook.md) for detailed instructions.

## About Twenty

Twenty is the #1 open-source Customer Relationship Management (CRM) system built with modern technologies. It provides a flexible, self-hosted alternative to commercial CRMs like Salesforce and HubSpot, with complete data ownership and no vendor lock-in.

### Why This Documentation Exists

This documentation was created to provide:
- **Onboarding efficiency** for new developers joining the project
- **Comprehensive reference** for understanding system architecture
- **Operational guidance** for deployment and maintenance
- **Risk awareness** through transparent technical debt documentation
- **BMAD compliance** for brownfield project analysis standards

## Project Status

- **Maturity:** Alpha (0.2.1)
- **Active Development:** Yes (frequent updates)
- **Production Ready:** Suitable for early adopters with self-hosting capabilities
- **Community:** 17,600+ GitHub stars, 1000+ contributors

## Technology Overview

- **Frontend:** React 18 + TypeScript + Recoil + Vite
- **Backend:** NestJS + GraphQL + PostgreSQL + Redis
- **Monorepo:** Nx workspace with Yarn 4
- **Deployment:** Docker, Kubernetes, self-hosted

## Documentation Standards

This documentation follows BMAD (Brownfield Management and Development) standards:

✅ **Complete system overview** with clear value proposition
✅ **Step-by-step setup** with environment configuration
✅ **Architecture diagrams** with component relationships
✅ **Detailed code structure** with navigation guidance
✅ **Comprehensive API documentation** with examples
✅ **Realistic workflow examples** with code samples
✅ **Dependency analysis** with version requirements
✅ **Transparent technical debt** identification and risk assessment

## Contributing to This Documentation

This documentation is maintained alongside the codebase. To contribute:

1. Review the relevant section in `BMAD/`
2. Submit updates via pull request
3. Follow the existing structure and formatting
4. Keep examples current with latest version

## License

This documentation follows the same AGPL-3.0 license as the Twenty project.

---

**Ready to dive in?** Start with [System Overview](./01_System_Overview.md) to understand what Twenty does and why it exists.
