# API Documentation

## Overview

Twenty provides two primary API interfaces:
1. **GraphQL API** (Primary) - Flexible, type-safe data queries and mutations
2. **REST API** (Secondary) - File operations, webhooks, OAuth callbacks

## Authentication

### API Authentication Methods

#### 1. JWT Access Token (User Authentication)

```http
Authorization: Bearer <access_token>
```

**Obtaining Token:**
```graphql
mutation SignIn {
  signIn(input: {
    email: "user@example.com"
    password: "password123"
  }) {
    tokens {
      accessToken
      refreshToken
    }
    user {
      id
      email
      firstName
      lastName
    }
  }
}
```

**Response:**
```json
{
  "data": {
    "signIn": {
      "tokens": {
        "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
        "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
      },
      "user": {
        "id": "20202020-0c28-43d9-b8f7-2b12e84b4d5e",
        "email": "user@example.com",
        "firstName": "John",
        "lastName": "Doe"
      }
    }
  }
}
```

#### 2. API Key (Programmatic Access)

```http
Authorization: Bearer <api_key>
```

**Creating API Key:**
Navigate to Settings → Developers → API Keys → Create New Key

**Usage Example:**
```bash
curl -X POST https://api.twenty.com/graphql \
  -H "Authorization: Bearer sk_live_abc123..." \
  -H "Content-Type: application/json" \
  -d '{"query": "{ people { edges { node { id name { firstName lastName } } } } }"}'
```

#### 3. OAuth (Third-Party Integration)

```http
Authorization: Bearer <oauth_access_token>
```

**OAuth Flow:**
1. Redirect user to `/auth/google` or `/auth/microsoft`
2. User authenticates
3. Redirect back with authorization code
4. Exchange code for access token

### Token Refresh

```graphql
mutation RefreshToken {
  refreshToken(input: {
    refreshToken: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }) {
    tokens {
      accessToken
      refreshToken
    }
  }
}
```

## GraphQL API

### Base URL

- **Development:** `http://localhost:3000/graphql`
- **Production:** `https://your-domain.com/graphql`

### GraphQL Playground

Access the interactive GraphQL playground:
- **URL:** `http://localhost:3000/graphql`
- **Features:** Schema exploration, query builder, documentation

### Schema Introspection

```graphql
query IntrospectionQuery {
  __schema {
    types {
      name
      description
    }
  }
}
```

## Core Object Operations

### Person (Contact) Operations

#### Query: List People

```graphql
query GetPeople {
  people(
    first: 10
    after: null
    orderBy: { createdAt: DescNullsFirst }
    filter: { email: { ilike: "%@example.com" } }
  ) {
    edges {
      node {
        id
        name {
          firstName
          lastName
        }
        email
        phone
        city
        createdAt
        updatedAt
        company {
          id
          name
        }
        opportunities {
          edges {
            node {
              id
              name
              stage
            }
          }
        }
      }
      cursor
    }
    pageInfo {
      hasNextPage
      hasPreviousPage
      startCursor
      endCursor
    }
    totalCount
  }
}
```

**Response:**
```json
{
  "data": {
    "people": {
      "edges": [
        {
          "node": {
            "id": "20202020-0c28-43d9-b8f7-2b12e84b4d5e",
            "name": {
              "firstName": "John",
              "lastName": "Doe"
            },
            "email": "john.doe@example.com",
            "phone": "+1234567890",
            "city": "San Francisco",
            "createdAt": "2026-01-15T10:30:00.000Z",
            "updatedAt": "2026-02-01T15:45:00.000Z",
            "company": {
              "id": "30303030-1234-5678-9abc-def012345678",
              "name": "Example Corp"
            },
            "opportunities": {
              "edges": [
                {
                  "node": {
                    "id": "40404040-abcd-efgh-ijkl-mnopqrstuvwx",
                    "name": "Q1 Deal",
                    "stage": "PROPOSAL"
                  }
                }
              ]
            }
          },
          "cursor": "cursor_string_here"
        }
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPreviousPage": false,
        "startCursor": "first_cursor",
        "endCursor": "last_cursor"
      },
      "totalCount": 42
    }
  }
}
```

#### Mutation: Create Person

```graphql
mutation CreatePerson {
  createPerson(input: {
    name: {
      firstName: "Jane"
      lastName: "Smith"
    }
    email: "jane.smith@example.com"
    phone: "+1987654321"
    city: "New York"
    companyId: "30303030-1234-5678-9abc-def012345678"
  }) {
    id
    name {
      firstName
      lastName
    }
    email
    createdAt
  }
}
```

#### Mutation: Update Person

```graphql
mutation UpdatePerson {
  updatePerson(
    id: "20202020-0c28-43d9-b8f7-2b12e84b4d5e"
    input: {
      phone: "+1555555555"
      city: "Los Angeles"
    }
  ) {
    id
    phone
    city
    updatedAt
  }
}
```

#### Mutation: Delete Person

```graphql
mutation DeletePerson {
  deletePerson(id: "20202020-0c28-43d9-b8f7-2b12e84b4d5e") {
    id
  }
}
```

### Company Operations

#### Query: List Companies

```graphql
query GetCompanies {
  companies(
    first: 10
    filter: { name: { ilike: "%Tech%" } }
    orderBy: { createdAt: DescNullsFirst }
  ) {
    edges {
      node {
        id
        name
        domainName
        address
        employees
        idealCustomerProfile
        createdAt
        people {
          edges {
            node {
              id
              name {
                firstName
                lastName
              }
              email
            }
          }
        }
      }
    }
    pageInfo {
      hasNextPage
      endCursor
    }
  }
}
```

#### Mutation: Create Company

```graphql
mutation CreateCompany {
  createCompany(input: {
    name: "Tech Innovations Inc."
    domainName: "techinnovations.com"
    address: "123 Tech Street, SF"
    employees: 50
    idealCustomerProfile: true
  }) {
    id
    name
    domainName
    createdAt
  }
}
```

### Opportunity Operations

#### Query: List Opportunities

```graphql
query GetOpportunities {
  opportunities(
    first: 20
    filter: {
      stage: { in: ["PROPOSAL", "NEGOTIATION"] }
      amount: { gte: 10000 }
    }
    orderBy: { closeDate: AscNullsLast }
  ) {
    edges {
      node {
        id
        name
        amount {
          amountMicros
          currencyCode
        }
        stage
        probability
        closeDate
        createdAt
        pointOfContact {
          id
          name {
            firstName
            lastName
          }
        }
        company {
          id
          name
        }
      }
    }
  }
}
```

#### Mutation: Create Opportunity

```graphql
mutation CreateOpportunity {
  createOpportunity(input: {
    name: "Q2 Enterprise Deal"
    amount: {
      amountMicros: 50000000000  # $50,000 in micros
      currencyCode: "USD"
    }
    stage: "PROSPECTING"
    probability: 10
    closeDate: "2026-06-30"
    companyId: "30303030-1234-5678-9abc-def012345678"
    pointOfContactId: "20202020-0c28-43d9-b8f7-2b12e84b4d5e"
  }) {
    id
    name
    amount {
      amountMicros
      currencyCode
    }
    stage
    closeDate
  }
}
```

#### Mutation: Update Opportunity Stage

```graphql
mutation UpdateOpportunityStage {
  updateOpportunity(
    id: "40404040-abcd-efgh-ijkl-mnopqrstuvwx"
    input: {
      stage: "PROPOSAL"
      probability: 40
    }
  ) {
    id
    stage
    probability
    updatedAt
  }
}
```

### Task Operations

#### Query: List Tasks

```graphql
query GetTasks {
  tasks(
    first: 20
    filter: {
      status: { eq: "TODO" }
      dueAt: { lte: "2026-02-10T00:00:00.000Z" }
    }
    orderBy: { dueAt: AscNullsLast }
  ) {
    edges {
      node {
        id
        title
        body
        status
        dueAt
        assignee {
          id
          firstName
          lastName
        }
        createdAt
      }
    }
  }
}
```

#### Mutation: Create Task

```graphql
mutation CreateTask {
  createTask(input: {
    title: "Follow up with lead"
    body: "Send proposal document and schedule demo call"
    status: "TODO"
    dueAt: "2026-02-15T14:00:00.000Z"
    assigneeId: "50505050-user-uuid-here"
  }) {
    id
    title
    status
    dueAt
    createdAt
  }
}
```

#### Mutation: Complete Task

```graphql
mutation CompleteTask {
  updateTask(
    id: "60606060-task-uuid-here"
    input: {
      status: "DONE"
    }
  ) {
    id
    status
    updatedAt
  }
}
```

## Advanced Queries

### Filtering

```graphql
query FilteredPeople {
  people(
    filter: {
      AND: [
        { email: { ilike: "%@company.com" } }
        { city: { eq: "San Francisco" } }
        { createdAt: { gte: "2026-01-01T00:00:00.000Z" } }
      ]
    }
  ) {
    edges {
      node {
        id
        email
        city
      }
    }
  }
}
```

**Filter Operators:**
- `eq` - Equals
- `neq` - Not equals
- `gt` - Greater than
- `gte` - Greater than or equal
- `lt` - Less than
- `lte` - Less than or equal
- `in` - In array
- `ilike` - Case-insensitive LIKE
- `is` - Is null/not null
- `AND` - Logical AND
- `OR` - Logical OR

### Pagination

**Cursor-Based (Recommended):**
```graphql
query PaginatedPeople($cursor: String) {
  people(first: 10, after: $cursor) {
    edges {
      node { id name { firstName lastName } }
      cursor
    }
    pageInfo {
      hasNextPage
      endCursor
    }
  }
}
```

**Variables:**
```json
{
  "cursor": "cursor_from_previous_response"
}
```

### Sorting

```graphql
query SortedCompanies {
  companies(
    orderBy: [
      { createdAt: DescNullsFirst }
      { name: AscNullsLast }
    ]
  ) {
    edges {
      node {
        id
        name
        createdAt
      }
    }
  }
}
```

**Sort Orders:**
- `AscNullsFirst` - Ascending, nulls first
- `AscNullsLast` - Ascending, nulls last
- `DescNullsFirst` - Descending, nulls first
- `DescNullsLast` - Descending, nulls last

### Nested Relationships

```graphql
query CompanyWithRelations {
  company(id: "30303030-1234-5678-9abc-def012345678") {
    id
    name
    people {
      edges {
        node {
          id
          name { firstName lastName }
          opportunities {
            edges {
              node {
                id
                name
                stage
                amount { amountMicros currencyCode }
              }
            }
          }
        }
      }
    }
  }
}
```

## Workflow API

### Query: List Workflows

```graphql
query GetWorkflows {
  workflows(first: 20) {
    edges {
      node {
        id
        name
        statuses
        versions {
          edges {
            node {
              id
              trigger
              steps
              createdAt
            }
          }
        }
      }
    }
  }
}
```

### Mutation: Create Workflow

```graphql
mutation CreateWorkflow {
  createWorkflow(input: {
    name: "New Lead Notification"
    trigger: {
      type: "DATABASE_EVENT"
      settings: {
        eventName: "person.created"
      }
    }
    steps: [
      {
        id: "step1"
        type: "SEND_EMAIL"
        settings: {
          to: "sales@company.com"
          subject: "New Lead Created"
          body: "A new lead was created: {{person.email}}"
        }
      }
    ]
  }) {
    id
    name
    createdAt
  }
}
```

### Mutation: Activate Workflow

```graphql
mutation ActivateWorkflow {
  updateWorkflow(
    id: "70707070-workflow-uuid"
    input: {
      statuses: ["ACTIVE"]
    }
  ) {
    id
    statuses
  }
}
```

## Metadata API

### Query: Object Metadata

```graphql
query GetObjectMetadata {
  objects {
    edges {
      node {
        id
        nameSingular
        namePlural
        labelSingular
        labelPlural
        description
        icon
        isCustom
        isActive
        fields {
          edges {
            node {
              id
              name
              label
              type
              description
              isNullable
              defaultValue
            }
          }
        }
      }
    }
  }
}
```

### Mutation: Create Custom Object

```graphql
mutation CreateCustomObject {
  createOneObject(input: {
    nameSingular: "project"
    namePlural: "projects"
    labelSingular: "Project"
    labelPlural: "Projects"
    description: "Project tracking"
    icon: "IconBriefcase"
  }) {
    id
    nameSingular
    namePlural
  }
}
```

### Mutation: Create Custom Field

```graphql
mutation CreateCustomField {
  createOneField(input: {
    objectId: "object-uuid-here"
    name: "projectStatus"
    label: "Project Status"
    type: "SELECT"
    description: "Current project status"
    options: ["Planning", "In Progress", "Completed", "On Hold"]
    defaultValue: "Planning"
  }) {
    id
    name
    label
    type
  }
}
```

## GraphQL Subscriptions

### Real-Time Updates

```graphql
subscription OnPersonCreated {
  personCreated {
    id
    name {
      firstName
      lastName
    }
    email
    createdAt
  }
}
```

**Client Implementation (Apollo):**
```typescript
import { useSubscription } from '@apollo/client';

function PersonList() {
  const { data } = useSubscription(ON_PERSON_CREATED);

  useEffect(() => {
    if (data?.personCreated) {
      console.log('New person created:', data.personCreated);
    }
  }, [data]);
}
```

## REST API

### Base URL

- **Development:** `http://localhost:3000/rest`
- **Production:** `https://your-domain.com/rest`

### API Documentation Endpoint

```http
GET /rest/api/docs
```

Returns OpenAPI/Swagger documentation for REST endpoints.

### File Upload

```http
POST /rest/files
Content-Type: multipart/form-data
Authorization: Bearer <token>

file: <binary file data>
```

**Response:**
```json
{
  "id": "file-uuid",
  "url": "https://storage.example.com/files/file-uuid.pdf",
  "filename": "document.pdf",
  "mimeType": "application/pdf",
  "size": 245678
}
```

### File Download

```http
GET /rest/files/{fileId}
Authorization: Bearer <token>
```

Returns the file binary data with appropriate Content-Type header.

### Webhook Endpoints

```http
POST /rest/webhooks/{workflowId}
Content-Type: application/json

{
  "event": "custom_event",
  "data": {
    "key": "value"
  }
}
```

## Error Handling

### GraphQL Errors

```json
{
  "errors": [
    {
      "message": "Record not found",
      "extensions": {
        "code": "NOT_FOUND",
        "statusCode": 404
      },
      "path": ["person"],
      "locations": [{ "line": 2, "column": 3 }]
    }
  ],
  "data": null
}
```

### Common Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `UNAUTHENTICATED` | 401 | Missing or invalid authentication |
| `FORBIDDEN` | 403 | Insufficient permissions |
| `NOT_FOUND` | 404 | Resource not found |
| `BAD_USER_INPUT` | 400 | Invalid input data |
| `CONFLICT` | 409 | Resource conflict (e.g., duplicate) |
| `INTERNAL_SERVER_ERROR` | 500 | Server error |
| `RATE_LIMIT_EXCEEDED` | 429 | Too many requests |

### Error Response Format

```json
{
  "error": {
    "code": "FORBIDDEN",
    "message": "You do not have permission to access this resource",
    "details": {
      "resource": "company",
      "action": "delete",
      "reason": "missing_permission"
    }
  }
}
```

## Rate Limiting

### Limits

- **Authenticated Users:** 1000 requests per minute per workspace
- **API Keys:** 5000 requests per minute per key
- **Unauthenticated:** 100 requests per minute per IP

### Rate Limit Headers

```http
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 987
X-RateLimit-Reset: 1675123456
```

### Rate Limit Exceeded Response

```json
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Rate limit exceeded. Please try again later.",
    "retryAfter": 60
  }
}
```

## Best Practices

### 1. Use Field Selection

Only request fields you need to reduce payload size:

```graphql
# Good - specific fields
query GetPeople {
  people(first: 10) {
    edges {
      node {
        id
        name { firstName lastName }
        email
      }
    }
  }
}

# Bad - requesting everything
query GetPeople {
  people(first: 10) {
    edges {
      node {
        # ... all fields
      }
    }
  }
}
```

### 2. Use Pagination

Always paginate large result sets:

```graphql
query GetPeople($cursor: String) {
  people(first: 50, after: $cursor) {
    edges {
      node { id name { firstName } }
    }
    pageInfo {
      hasNextPage
      endCursor
    }
  }
}
```

### 3. Batch Mutations

Use batching for multiple operations:

```graphql
mutation BatchCreatePeople($people: [CreatePersonInput!]!) {
  createManyPeople(input: $people) {
    id
    name { firstName lastName }
  }
}
```

### 4. Handle Errors Gracefully

```typescript
try {
  const result = await client.mutate({
    mutation: CREATE_PERSON,
    variables: { input: personData }
  });
} catch (error) {
  if (error.graphQLErrors) {
    error.graphQLErrors.forEach(({ message, extensions }) => {
      console.error(`GraphQL Error: ${message}`, extensions);
    });
  }
  if (error.networkError) {
    console.error('Network Error:', error.networkError);
  }
}
```

### 5. Use Variables

Always use variables instead of inline values:

```graphql
# Good
query GetPerson($id: ID!) {
  person(id: $id) {
    id
    name { firstName lastName }
  }
}

# Bad
query GetPerson {
  person(id: "hardcoded-id-here") {
    id
    name { firstName lastName }
  }
}
```

## SDK Usage

### JavaScript/TypeScript SDK

```typescript
import { TwentySDK } from 'twenty-sdk';

const client = new TwentySDK({
  baseUrl: 'https://api.twenty.com',
  apiKey: 'sk_live_abc123...'
});

// Create person
const person = await client.people.create({
  name: { firstName: 'John', lastName: 'Doe' },
  email: 'john@example.com'
});

// List people
const people = await client.people.list({
  first: 10,
  filter: { email: { ilike: '%@example.com' } }
});

// Update person
await client.people.update('person-id', {
  phone: '+1234567890'
});

// Delete person
await client.people.delete('person-id');
```

## API Changelog

### Version 0.2.1 (Current)
- Added workflow API endpoints
- Enhanced filtering capabilities
- Improved error messages
- Added rate limiting headers

### Version 0.2.0
- Initial public API release
- Core CRUD operations for all standard objects
- Metadata API for custom objects
- WebSocket subscriptions

---

**See Also:**
- [Workflows & Examples](./06_Workflows_Examples.md) for usage scenarios
- [Setup Guide](./02_Setup_and_Runbook.md) for API configuration
