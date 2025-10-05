```markdown
# Dynatrace Setup Manual

This manual provides a step-by-step guide to configure Dynatrace with the necessary permissions, user groups, and deployment steps.

---

## Step 1: User Permissions Needed to Run the Setup

### Create a Dynatrace Policy with the Following Permissions

#### States
```plaintext
ALLOW state:app-states:delete, state:app-states:read, state:app-states:write, 
      state:user-app-states:read, state:user-app-states:write, 
      state:user-app-states:delete, state-management:user-app-states:delete, 
      state-management:user-app-states:delete-all, state-management:app-states:delete;
```

#### Documents
```plaintext
ALLOW document:documents:read, document:documents:write, document:documents:delete, 
      document:documents:admin, document:environment-shares:read, 
      document:environment-shares:write, document:environment-shares:claim, 
      document:environment-shares:delete, document:direct-shares:read, 
      document:direct-shares:write, document:direct-shares:delete, 
      document:trash.documents:read, document:trash.documents:restore, 
      document:trash.documents:delete;
```

#### Grail
```plaintext
ALLOW storage:bucket-definitions:read, storage:filter-segments:read, 
      storage:fieldset-definitions:read, storage:fieldset-definitions:write;
ALLOW storage:filter-segments:read, storage:filter-segments:write, 
      storage:filter-segments:share, storage:filter-segments:delete, 
      storage:filter-segments:admin;
```

#### Hub
```plaintext
ALLOW hub:catalog:read;
```

#### AppEngine
```plaintext
ALLOW app-engine:apps:run, app-engine:functions:run, app-engine:edge-connects:read, 
      app-engine:edge-connects:write, app-engine:edge-connects:delete, 
      app-engine:apps:install, app-engine:apps:delete;
```

#### AutomationEngine
```plaintext
ALLOW automation:workflows:read, automation:calendars:read, automation:rules:read, 
      automation:workflows:write, automation:workflows:run, automation:calendars:write, 
      automation:rules:write, automation:workflows:admin;
```

#### Davis
```plaintext
ALLOW davis:analyzers:read, davis:analyzers:execute;
```

#### IAM
```plaintext
ALLOW iam:service-users:use, oauth2:clients:manage;
```

#### Settings
```plaintext
ALLOW settings:objects:read, settings:schemas:read, app-settings:objects:read, 
      app-settings:objects:write, settings:objects:write, settings:objects:admin, 
      app-settings:objects:admin;
```

#### Deployment
```plaintext
ALLOW deployment:activegates.network-zones:write, deployment:activegates.groups:write, 
      deployment:oneagents.network-zones:write, deployment:oneagents.host-groups:write, 
      deployment:oneagents.host-tags:write, deployment:oneagents.host-properties:write;
```

#### Hyperscaler Authentication
```plaintext
ALLOW hyperscaler-authentication:aws:authenticate;
```

---

### Create a Dynatrace User Group

1. Add the following permissions:
   - View and manage account and billing information
   - View and manage users and groups
   - View account
2. Add the policy you created earlier to the user group.

---

## Step 2: Give Yourself the Permission in the Workflows App

---

## Step 3: Allow Outgoing Traffic

---

## Step 4: Install Apps from Dynatrace Hub

---

## Step 5: Import and Deploy Workflow

---

## Step 6: Create OAuth Token

Go to account management and create an OAuth token with the following permissions:

```plaintext
automation:workflows:read
automation:workflows:write
automation:workflows:run
automation:rules:read
automation:rules:write
automation:calendars:read
automation:calendars:write
automation:workflows:admin
account-idm-read, iam:users:read, iam:groups:read
account-idm-write
account-env-read
account-env-write
account-uac-read
account-uac-write
account-audit-logs-read
iam-policies-management, iam:policies:write, iam:policies:read, iam:bindings:write, 
iam:bindings:read, iam:effective-permissions:read, iam:service-users:use, 
iam:limits:read, iam:boundaries:read, iam:boundaries:write
Manage platform tokens
platform-token:tokens:manage
```

Save the **Client ID** and **Client Secret**.

---

## Step 7: Create Access Token

1. Go to **Deploy Agents** in the Dynatrace tenant.
2. Click on **Create Agent for Linux**.
3. Click on **Create Token** button.
4. Copy and save the created token.

---

## Step 8: Deploy CloudFormation Template

1. Go to the **AWS Management Console** and open **CloudFormation**.
2. Upload the CloudFormation template.
3. Fill in the parameters.
4. Run the CloudFormation YAML.

---
```
