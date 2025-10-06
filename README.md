
## Dynatrace Automation Automation Use Case setup step by step guide

## Step 1: Create user permissions needed to run the setup

### Create a Dynatrace Policy and copy and paste the following permissions into the Policy

    //States
    ALLOW state:app-states:delete, state:app-states:read, state:app-states:write, state:user-app-states:read, state:user-app-states:write, state:user-app-states:delete, state-management:user-app-states:delete, state-management:user-app-states:delete-all, state-management:app-states:delete;
    
    //Documents
    ALLOW document:documents:read, document:documents:write, document:documents:delete, document:documents:admin, document:environment-shares:read, document:environment-shares:write, document:environment-shares:claim, document:environment-shares:delete, document:direct-shares:read, document:direct-shares:write, document:direct-shares:delete, document:trash.documents:read, document:trash.documents:restore, document:trash.documents:delete;
       
    //Grail
    ALLOW storage:bucket-definitions:read, storage:filter-segments:read, storage:fieldset-definitions:read, storage:fieldset-definitions:write;
    
    ALLOW storage:filter-segments:read, storage:filter-segments:write, storage:filter-segments:share, storage:filter-segments:delete, storage:filter-segments:admin;
    
    //Hub
    ALLOW hub:catalog:read;
      
    //AppEngine
    ALLOW app-engine:apps:run, app-engine:functions:run, app-engine:edge-connects:read, app-engine:edge-connects:write, app-engine:edge-connects:delete, app-engine:apps:install, app-engine:apps:delete;
    
    //AutomationEngine
    ALLOW automation:workflows:read, automation:calendars:read, automation:rules:read, automation:workflows:write, automation:workflows:run, automation:calendars:write, automation:rules:write, automation:workflows:admin;
        
    //Davis
    ALLOW davis:analyzers:read, davis:analyzers:execute;
        
    //IAM
    ALLOW iam:service-users:use, oauth2:clients:manage;
       
    //Settings
    ALLOW settings:objects:read, settings:schemas:read, app-settings:objects:read, app-settings:objects:write, settings:objects:write, settings:objects:admin, app-settings:objects:admin;
    
    //Deployment
    ALLOW deployment:activegates.network-zones:write, deployment:activegates.groups:write, deployment:oneagents.network-zones:write, deployment:oneagents.host-groups:write, deployment:oneagents.host-tags:write, deployment:oneagents.host-properties:write;

    // Hyperscaler Authentication
    ALLOW hyperscaler-authentication:aws:authenticate;

 




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
