# Workflows & Usage Examples

This document provides practical examples of common Twenty CRM workflows with step-by-step instructions and code samples.

## Workflow 1: Creating and Managing a Sales Lead

### Scenario
A sales representative receives an inquiry from a potential customer and needs to create a lead in the CRM.

### Step-by-Step Process

#### Step 1: Create Company Record

**UI Method:**
1. Navigate to Companies
2. Click "Add Company"
3. Fill in details:
   - Name: "Acme Corporation"
   - Domain: "acme.com"
   - Industry: "Technology"
   - Employees: 100

**API Method:**
```graphql
mutation CreateCompany {
  createCompany(input: {
    name: "Acme Corporation"
    domainName: "acme.com"
    employees: 100
    idealCustomerProfile: true
  }) {
    id
    name
    domainName
  }
}
```

**cURL Example:**
```bash
curl -X POST http://localhost:3000/graphql \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "mutation { createCompany(input: { name: \"Acme Corporation\", domainName: \"acme.com\", employees: 100 }) { id name } }"
  }'
```

**Response:**
```json
{
  "data": {
    "createCompany": {
      "id": "company-uuid-12345",
      "name": "Acme Corporation",
      "domainName": "acme.com"
    }
  }
}
```

#### Step 2: Create Person (Contact) Record

**UI Method:**
1. Navigate to People
2. Click "Add Person"
3. Fill in details:
   - First Name: "Sarah"
   - Last Name: "Johnson"
   - Email: "sarah.johnson@acme.com"
   - Phone: "+1-555-0123"
   - Company: Select "Acme Corporation"
   - Job Title: "VP of Sales"

**API Method:**
```graphql
mutation CreatePerson {
  createPerson(input: {
    name: {
      firstName: "Sarah"
      lastName: "Johnson"
    }
    email: "sarah.johnson@acme.com"
    phone: "+1-555-0123"
    jobTitle: "VP of Sales"
    companyId: "company-uuid-12345"
  }) {
    id
    name {
      firstName
      lastName
    }
    email
    company {
      id
      name
    }
  }
}
```

**Response:**
```json
{
  "data": {
    "createPerson": {
      "id": "person-uuid-67890",
      "name": {
        "firstName": "Sarah",
        "lastName": "Johnson"
      },
      "email": "sarah.johnson@acme.com",
      "company": {
        "id": "company-uuid-12345",
        "name": "Acme Corporation"
      }
    }
  }
}
```

#### Step 3: Create Opportunity

**UI Method:**
1. Navigate to Opportunities
2. Click "Add Opportunity"
3. Fill in details:
   - Name: "Q1 2026 Enterprise Deal"
   - Amount: $50,000
   - Stage: "Prospecting"
   - Probability: 10%
   - Close Date: March 31, 2026
   - Point of Contact: Select "Sarah Johnson"
   - Company: Select "Acme Corporation"

**API Method:**
```graphql
mutation CreateOpportunity {
  createOpportunity(input: {
    name: "Q1 2026 Enterprise Deal"
    amount: {
      amountMicros: 50000000000  # $50,000 in micros (multiply by 1M)
      currencyCode: "USD"
    }
    stage: "PROSPECTING"
    probability: 10
    closeDate: "2026-03-31"
    pointOfContactId: "person-uuid-67890"
    companyId: "company-uuid-12345"
  }) {
    id
    name
    amount {
      amountMicros
      currencyCode
    }
    stage
    probability
    closeDate
    pointOfContact {
      name {
        firstName
        lastName
      }
    }
    company {
      name
    }
  }
}
```

**Response:**
```json
{
  "data": {
    "createOpportunity": {
      "id": "opportunity-uuid-abcde",
      "name": "Q1 2026 Enterprise Deal",
      "amount": {
        "amountMicros": 50000000000,
        "currencyCode": "USD"
      },
      "stage": "PROSPECTING",
      "probability": 10,
      "closeDate": "2026-03-31",
      "pointOfContact": {
        "name": {
          "firstName": "Sarah",
          "lastName": "Johnson"
        }
      },
      "company": {
        "name": "Acme Corporation"
      }
    }
  }
}
```

#### Step 4: Create Follow-Up Task

**API Method:**
```graphql
mutation CreateTask {
  createTask(input: {
    title: "Send proposal to Sarah Johnson"
    body: "Follow up on our initial call. Send enterprise plan proposal and schedule demo for next week."
    status: "TODO"
    dueAt: "2026-02-10T14:00:00.000Z"
    assigneeId: "current-user-uuid"
  }) {
    id
    title
    status
    dueAt
  }
}
```

#### Step 5: Log Activity

**API Method:**
```graphql
mutation CreateActivity {
  createActivity(input: {
    title: "Discovery Call with Sarah Johnson"
    body: "Discussed their current CRM pain points. Key requirements: custom workflows, API access, self-hosting. Strong interest in Q1 implementation."
    type: "MEETING"
    activityDate: "2026-02-03T10:00:00.000Z"
    personId: "person-uuid-67890"
    opportunityId: "opportunity-uuid-abcde"
  }) {
    id
    title
    type
    activityDate
  }
}
```

### Complete Workflow Code

**TypeScript/Node.js Example:**
```typescript
import { ApolloClient, InMemoryCache, gql } from '@apollo/client';

const client = new ApolloClient({
  uri: 'http://localhost:3000/graphql',
  cache: new InMemoryCache(),
  headers: {
    authorization: `Bearer ${process.env.TWENTY_API_TOKEN}`
  }
});

async function createLead() {
  // 1. Create company
  const companyResult = await client.mutate({
    mutation: gql`
      mutation CreateCompany($input: CreateCompanyInput!) {
        createCompany(input: $input) {
          id
          name
        }
      }
    `,
    variables: {
      input: {
        name: "Acme Corporation",
        domainName: "acme.com",
        employees: 100
      }
    }
  });
  const companyId = companyResult.data.createCompany.id;
  console.log('Company created:', companyId);

  // 2. Create person
  const personResult = await client.mutate({
    mutation: gql`
      mutation CreatePerson($input: CreatePersonInput!) {
        createPerson(input: $input) {
          id
          name { firstName lastName }
        }
      }
    `,
    variables: {
      input: {
        name: { firstName: "Sarah", lastName: "Johnson" },
        email: "sarah.johnson@acme.com",
        phone: "+1-555-0123",
        companyId
      }
    }
  });
  const personId = personResult.data.createPerson.id;
  console.log('Person created:', personId);

  // 3. Create opportunity
  const opportunityResult = await client.mutate({
    mutation: gql`
      mutation CreateOpportunity($input: CreateOpportunityInput!) {
        createOpportunity(input: $input) {
          id
          name
          stage
        }
      }
    `,
    variables: {
      input: {
        name: "Q1 2026 Enterprise Deal",
        amount: {
          amountMicros: 50000000000,
          currencyCode: "USD"
        },
        stage: "PROSPECTING",
        probability: 10,
        closeDate: "2026-03-31",
        pointOfContactId: personId,
        companyId
      }
    }
  });
  const opportunityId = opportunityResult.data.createOpportunity.id;
  console.log('Opportunity created:', opportunityId);

  // 4. Create task
  await client.mutate({
    mutation: gql`
      mutation CreateTask($input: CreateTaskInput!) {
        createTask(input: $input) {
          id
          title
        }
      }
    `,
    variables: {
      input: {
        title: "Send proposal to Sarah Johnson",
        body: "Follow up with enterprise plan details",
        status: "TODO",
        dueAt: "2026-02-10T14:00:00.000Z"
      }
    }
  });

  return { companyId, personId, opportunityId };
}

createLead()
  .then(result => console.log('Lead created successfully:', result))
  .catch(error => console.error('Error creating lead:', error));
```

## Workflow 2: Managing Sales Pipeline

### Scenario
Moving an opportunity through the sales pipeline from prospecting to closed-won.

### Pipeline Stages

1. **PROSPECTING** - Initial contact and qualification
2. **MEETING** - Demo/discovery call scheduled
3. **PROPOSAL** - Proposal sent to customer
4. **NEGOTIATION** - Discussing terms and pricing
5. **CLOSED_WON** - Deal closed successfully
6. **CLOSED_LOST** - Deal lost

### Moving Through Stages

#### Update Opportunity Stage

```graphql
mutation UpdateOpportunityStage {
  updateOpportunity(
    id: "opportunity-uuid-abcde"
    input: {
      stage: "MEETING"
      probability: 25
    }
  ) {
    id
    stage
    probability
    updatedAt
  }
}
```

#### Complete Pipeline Progression Script

```typescript
async function progressOpportunity(opportunityId: string) {
  const stages = [
    { stage: 'PROSPECTING', probability: 10 },
    { stage: 'MEETING', probability: 25 },
    { stage: 'PROPOSAL', probability: 50 },
    { stage: 'NEGOTIATION', probability: 75 },
    { stage: 'CLOSED_WON', probability: 100 }
  ];

  for (const { stage, probability } of stages) {
    console.log(`Moving to stage: ${stage}`);

    await client.mutate({
      mutation: gql`
        mutation UpdateOpportunity($id: ID!, $input: UpdateOpportunityInput!) {
          updateOpportunity(id: $id, input: $input) {
            id
            stage
            probability
            updatedAt
          }
        }
      `,
      variables: {
        id: opportunityId,
        input: { stage, probability }
      }
    });

    // Log activity for each stage change
    await client.mutate({
      mutation: gql`
        mutation CreateActivity($input: CreateActivityInput!) {
          createActivity(input: $input) {
            id
          }
        }
      `,
      variables: {
        input: {
          title: `Moved to ${stage}`,
          type: "NOTE",
          activityDate: new Date().toISOString(),
          opportunityId
        }
      }
    });

    // Wait before moving to next stage (for demo purposes)
    await new Promise(resolve => setTimeout(resolve, 1000));
  }

  console.log('Opportunity closed won!');
}
```

### Querying Pipeline Status

```graphql
query GetPipelineStatus {
  opportunities(
    filter: {
      stage: { in: ["PROSPECTING", "MEETING", "PROPOSAL", "NEGOTIATION"] }
    }
    orderBy: { closeDate: AscNullsLast }
  ) {
    edges {
      node {
        id
        name
        stage
        probability
        amount {
          amountMicros
          currencyCode
        }
        closeDate
        company {
          name
        }
        pointOfContact {
          name {
            firstName
            lastName
          }
        }
      }
    }
  }
}
```

### Pipeline Reporting

```typescript
async function generatePipelineReport() {
  const { data } = await client.query({
    query: gql`
      query GetAllOpportunities {
        opportunities(first: 1000) {
          edges {
            node {
              id
              stage
              amount {
                amountMicros
                currencyCode
              }
            }
          }
        }
      }
    `
  });

  const report = {
    byStage: {} as Record<string, { count: number; totalValue: number }>,
    totalValue: 0,
    totalCount: 0
  };

  data.opportunities.edges.forEach(({ node }: any) => {
    const stage = node.stage;
    const value = node.amount.amountMicros / 1000000; // Convert from micros

    if (!report.byStage[stage]) {
      report.byStage[stage] = { count: 0, totalValue: 0 };
    }

    report.byStage[stage].count++;
    report.byStage[stage].totalValue += value;
    report.totalValue += value;
    report.totalCount++;
  });

  console.log('Pipeline Report:', report);
  return report;
}
```

## Workflow 3: Email-Driven Contact Creation

### Scenario
Automatically create contacts and activities from incoming emails using Twenty's email integration.

### Setup Email Integration

#### Step 1: Connect Email Account

**UI Method:**
1. Navigate to Settings → Accounts
2. Click "Connect Email Account"
3. Choose provider (Gmail or Microsoft)
4. Authorize OAuth connection

**API Method:**
```graphql
mutation ConnectGmailAccount {
  createConnectedAccount(input: {
    provider: "GOOGLE"
    refreshToken: "oauth_refresh_token_here"
    accessToken: "oauth_access_token_here"
    accountOwnerId: "user-uuid"
  }) {
    id
    provider
    handle
  }
}
```

#### Step 2: Configure Email Sync

Email sync runs automatically in the background worker. No additional configuration needed.

### How Email Sync Works

1. **Worker Process** polls Gmail/Outlook API every 5 minutes
2. **Message Import** downloads new emails
3. **Contact Extraction** parses sender email address
4. **Company Matching** matches email domain to existing companies
5. **Person Creation** creates person record if not exists
6. **Activity Logging** creates activity record for email
7. **Thread Detection** groups related emails

### Manual Trigger Email Sync

```bash
# Via CLI command
npx nx run twenty-server:command messaging:message-list-fetch:cron
```

### Query Synced Messages

```graphql
query GetMessages {
  messages(
    first: 20
    orderBy: { receivedAt: DescNullsFirst }
  ) {
    edges {
      node {
        id
        subject
        receivedAt
        direction
        messageThread {
          id
          subject
        }
        messageParticipants {
          edges {
            node {
              role
              handle
              displayName
              person {
                id
                name {
                  firstName
                  lastName
                }
              }
            }
          }
        }
      }
    }
  }
}
```

### Example: Automated Contact Creation Flow

```typescript
// This happens automatically in the worker
async function processIncomingEmail(emailData: any) {
  const senderEmail = emailData.from.address;
  const domain = senderEmail.split('@')[1];

  // 1. Check if person exists
  let person = await findPersonByEmail(senderEmail);

  if (!person) {
    // 2. Find or create company by domain
    let company = await findCompanyByDomain(domain);

    if (!company) {
      company = await client.mutate({
        mutation: CREATE_COMPANY,
        variables: {
          input: {
            name: domain,
            domainName: domain
          }
        }
      });
    }

    // 3. Create person
    person = await client.mutate({
      mutation: CREATE_PERSON,
      variables: {
        input: {
          email: senderEmail,
          name: {
            firstName: emailData.from.name.split(' ')[0],
            lastName: emailData.from.name.split(' ').slice(1).join(' ')
          },
          companyId: company.id
        }
      }
    });
  }

  // 4. Create activity for email
  await client.mutate({
    mutation: CREATE_ACTIVITY,
    variables: {
      input: {
        title: emailData.subject,
        body: emailData.textBody,
        type: "EMAIL",
        activityDate: emailData.date,
        personId: person.id
      }
    }
  });

  console.log('Email processed and contact created/updated');
}
```

## Workflow 4: Workflow Automation

### Scenario
Create an automated workflow that sends a welcome email when a new person is added to the CRM.

### Step 1: Create Workflow

```graphql
mutation CreateWelcomeWorkflow {
  createWorkflow(input: {
    name: "New Contact Welcome Email"
    trigger: {
      type: "DATABASE_EVENT"
      settings: {
        eventName: "person.created"
        objectName: "person"
      }
    }
    steps: [
      {
        id: "send_email_step"
        type: "SEND_EMAIL"
        name: "Send Welcome Email"
        settings: {
          to: "{{trigger.person.email}}"
          subject: "Welcome to Our CRM"
          body: "Hi {{trigger.person.name.firstName}}, thanks for connecting!"
          from: "team@company.com"
        }
      }
    ]
  }) {
    id
    name
  }
}
```

### Step 2: Activate Workflow

```graphql
mutation ActivateWorkflow {
  updateWorkflow(
    id: "workflow-uuid"
    input: {
      statuses: ["ACTIVE"]
    }
  ) {
    id
    statuses
  }
}
```

### Step 3: Test Workflow

Create a new person to trigger the workflow:

```graphql
mutation TestWorkflow {
  createPerson(input: {
    name: {
      firstName: "Test"
      lastName: "User"
    }
    email: "test@example.com"
  }) {
    id
  }
}
```

### Complex Workflow Example

Multi-step workflow with conditions:

```graphql
mutation CreateComplexWorkflow {
  createWorkflow(input: {
    name: "Opportunity Follow-Up Automation"
    trigger: {
      type: "DATABASE_EVENT"
      settings: {
        eventName: "opportunity.updated"
        conditions: {
          stage: { eq: "PROPOSAL" }
        }
      }
    }
    steps: [
      {
        id: "create_task"
        type: "CREATE_RECORD"
        settings: {
          objectName: "task"
          record: {
            title: "Follow up on proposal"
            dueAt: "{{date.add(trigger.opportunity.updatedAt, 2, 'days')}}"
            assigneeId: "{{trigger.opportunity.ownerId}}"
          }
        }
      },
      {
        id: "send_notification"
        type: "SEND_EMAIL"
        settings: {
          to: "{{trigger.opportunity.owner.email}}"
          subject: "Proposal sent - follow up needed"
          body: "Don't forget to follow up on {{trigger.opportunity.name}}"
        }
      }
    ]
  }) {
    id
  }
}
```

## Workflow 5: Custom Object Implementation

### Scenario
Create a custom "Project" object to track implementation projects for customers.

### Step 1: Create Custom Object

```graphql
mutation CreateProjectObject {
  createOneObject(input: {
    nameSingular: "project"
    namePlural: "projects"
    labelSingular: "Project"
    labelPlural: "Projects"
    description: "Customer implementation projects"
    icon: "IconBriefcase"
  }) {
    id
    nameSingular
  }
}
```

### Step 2: Add Custom Fields

```graphql
mutation AddProjectFields {
  field1: createOneField(input: {
    objectId: "project-object-uuid"
    name: "projectStatus"
    label: "Status"
    type: "SELECT"
    options: ["Planning", "In Progress", "Testing", "Completed", "On Hold"]
    defaultValue: "Planning"
  }) {
    id
  }

  field2: createOneField(input: {
    objectId: "project-object-uuid"
    name: "startDate"
    label: "Start Date"
    type: "DATE"
  }) {
    id
  }

  field3: createOneField(input: {
    objectId: "project-object-uuid"
    name: "endDate"
    label: "End Date"
    type: "DATE"
  }) {
    id
  }

  field4: createOneField(input: {
    objectId: "project-object-uuid"
    name: "budget"
    label: "Budget"
    type: "CURRENCY"
  }) {
    id
  }
}
```

### Step 3: Create Relationship to Opportunity

```graphql
mutation LinkProjectToOpportunity {
  createOneRelation(input: {
    fromObjectId: "project-object-uuid"
    toObjectId: "opportunity-object-uuid"
    relationType: "MANY_TO_ONE"
    fromFieldName: "opportunity"
    toFieldName: "projects"
  }) {
    id
  }
}
```

### Step 4: Use Custom Object

```graphql
mutation CreateProject {
  createProject(input: {
    projectStatus: "Planning"
    startDate: "2026-03-01"
    endDate: "2026-06-30"
    budget: {
      amountMicros: 75000000000
      currencyCode: "USD"
    }
    opportunityId: "opportunity-uuid"
  }) {
    id
    projectStatus
    startDate
    endDate
  }
}
```

## Additional Code Examples

### Bulk Import Contacts from CSV

```typescript
import * as fs from 'fs';
import * as csv from 'csv-parser';

async function bulkImportContacts(filePath: string) {
  const contacts: any[] = [];

  // Read CSV file
  fs.createReadStream(filePath)
    .pipe(csv())
    .on('data', (row) => contacts.push(row))
    .on('end', async () => {
      console.log(`Importing ${contacts.length} contacts...`);

      for (const contact of contacts) {
        try {
          await client.mutate({
            mutation: gql`
              mutation CreatePerson($input: CreatePersonInput!) {
                createPerson(input: $input) {
                  id
                }
              }
            `,
            variables: {
              input: {
                name: {
                  firstName: contact.firstName,
                  lastName: contact.lastName
                },
                email: contact.email,
                phone: contact.phone,
                companyId: await findOrCreateCompany(contact.company)
              }
            }
          });
          console.log(`✓ Imported: ${contact.email}`);
        } catch (error) {
          console.error(`✗ Failed: ${contact.email}`, error);
        }
      }

      console.log('Import complete!');
    });
}
```

### Export Opportunities to CSV

```typescript
import { createObjectCsvWriter } from 'csv-writer';

async function exportOpportunities() {
  const { data } = await client.query({
    query: gql`
      query GetAllOpportunities {
        opportunities(first: 10000) {
          edges {
            node {
              id
              name
              stage
              probability
              amount { amountMicros currencyCode }
              closeDate
              company { name }
              pointOfContact {
                name { firstName lastName }
                email
              }
            }
          }
        }
      }
    `
  });

  const csvWriter = createObjectCsvWriter({
    path: 'opportunities_export.csv',
    header: [
      { id: 'name', title: 'Opportunity Name' },
      { id: 'stage', title: 'Stage' },
      { id: 'amount', title: 'Amount' },
      { id: 'closeDate', title: 'Close Date' },
      { id: 'company', title: 'Company' },
      { id: 'contact', title: 'Contact' },
      { id: 'email', title: 'Email' }
    ]
  });

  const records = data.opportunities.edges.map(({ node }: any) => ({
    name: node.name,
    stage: node.stage,
    amount: `${node.amount.currencyCode} ${node.amount.amountMicros / 1000000}`,
    closeDate: node.closeDate,
    company: node.company?.name || '',
    contact: `${node.pointOfContact?.name.firstName} ${node.pointOfContact?.name.lastName}`,
    email: node.pointOfContact?.email || ''
  }));

  await csvWriter.writeRecords(records);
  console.log('Export complete: opportunities_export.csv');
}
```

---

**See Also:**
- [API Documentation](./05_API_Documentation.md) for complete API reference
- [Setup Guide](./02_Setup_and_Runbook.md) for environment configuration
