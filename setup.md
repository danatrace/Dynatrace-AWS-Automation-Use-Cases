
## Dynatrace AWS Automation Use Cases setup step by step guide

### **Step 1: Create user permissions needed to run the setup**
#### *Create a Dynatrace Policy:*
1. Go to "Dynatrace Account Management", click on "Identitiy & Access Management" and chose "Policy Management" from the Dropdown menu.
2. Click on the "+ Create policy" button
3. Give the policy a name
4. Copy and paste the following into the "Policy Statement" field:
```   
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
```

#### *Create a Dynatrace User Group and add policies and roles:*
 1. Go to "Dynatrace Account Management", click on "Identitiy & Access Management" and chose "Group Management" from the Dropdown menu
 2. Click on the "+ Create Group" button and give your group a name
 3. In the group click on the "+ permissions" button and add the policy that you have created in the previous step to the group
 4. In the group go to "Account management permissions" and add the following permissions:
     * View and manage account and billing information
     * View and manage users and groups
     * View account

#### *Add the group to the user that is going to be the Workflow Owner:*
 1. Go to Dynatrace Account Management, click on "Identitiy & Access Management" and chose "User Management" from the Dropdown menu  
 2. In the Search field, search for the user that you have chosen to become the Workflow Owner and click on the user
 3. Now Add the group that you have created in the previous step to the user  <br <br

    
---

## **Step 2: Give workflow owner the permissions in the Workflows App:**
 *In your Dynatrace Tenant:*
 1. Go to the workflows app
 2. Click on the "settings" icon in the upper right corner and chose "Authorization settings"
 3. In Authorizations check the checkboxes for:
       * app-engine:apps:run
       * app-engine:functions:run
       * app-settings:objects:read
       * select all
 4. Click on Save in the upper right corner, you will be presented with the "missing permissions" window, ignore and click on close <br <br

---

## **Step 3: Allow Outgoing Traffic for workflow tasks**
 *In your Dynatrace Tenant:*
 1. Go to "settings classic"
 2. Click on preferences and "Limit outbound connections" 
 3. In "Limit outbound connections" click on the "Add item" button
 4. Add the following allowed hosts:
    * "*.amazonaws.com"
    * "api.dynatrace.com"
    * "sso.dynatrace.com"
    * "{yourtenantid}.live.dynatrace.com"    
 4. If you are using a sprint tenat (your Dynatrace Tenant url has the word sprint in it) add the follwoing to allowed hosts 
    * "*.amazonaws.com"
    * "api-hardening.internal.dynatracelabs.com"
    * "sso-sprint.dynatracelabs.com"
    * "{yourtenantid}.sprint.dynatrace.com"
---

## *Step 4: Install Apps from Dynatrace Hub*
 *In your Dynatrace Tenant:*
 1. Search for the "Hub" app
 2. In the Hub App search for "AWS Connector"
 3. Install the "AWS Connector" App


---

## **Step 5: Import and Deploy Workflow**
 *In your Dynatrace Tenant:*
 1. Open the Workflows App
 2. Cilck on the "Upload" button in the upper right corner
 3. Upload the workflow ....yaml from this repository

---

## **Step 6: Create OAuth Token**
 1. Go to "Dynatrace Account Management", click on "Identitiy & Access Management" and chose "OAuth Clients" from the Dropdown menu.
 2. In OAuth Clients click on the "create client" button.
 3. In User email fill in the email of the Dynatrace user to become the workflow owner!
 4. Check the following permissions:
 ### All Account permissions:
 * View users and groups / account-idm-read, iam:users:read, iam:groups:read
 * Manage users and groups / account-idm-write
 * View environments / account-env-read
 * Manage environments / account-env-write
 * View usage and consumption account-uac-read
 * Manage usage and consumption account-uac-write
 * View account audit logs account-audit-logs-read
 * View and manage policies / iam-policies-management, iam:policies:write, iam:policies:read, iam:bindings:write, iam:bindings:read, iam:effective-permissions:read, iam:service-users:use, iam:limits:read, iam:boundaries:read, iam:boundaries:write
 * Manage platform tokens / platform-token:tokens:manage
 ### All Automation Service permissions:
 * View workflows / automation:workflows:read
 * Create and edit workflows / automation:workflows:write
 * Run workflows / automation:workflows:run
 * View rules / automation:rules:read
 * Create and edit rules / automation:rules:write
 * View calendars / automation:calendars:read
 * Create and edit calendars / automation:calendars:write
 * Admin permission to manage all workflows and executions / automation:workflows:admin
 * **Save the **Client ID** and **Client Secret**.**

---

## **Step 7: Create Access Token**
 *In your Dynatrace Tenant:*
 1. Go to **Deploy OneAgent** in the Dynatrace tenant.
 2. Click on **Create Agent for Linux**.
 3. Click on **Create Token** button.
 4. Copy and save the created token.

---

## **Step 8: Get Dynatrace Account id**
 1. Go to "Dynatrace Account Management", click on "Support Information" paste the content into a textfile.
 2. Copy the Account id (Value accountUuid in Json) and save it.

---

## **Step 9: Create the Cloudformation template**
 1. Go to the **AWS Management Console** and open **CloudFormation**.
 2. Upload the CloudFormation template ....yaml from this repository
 3. Fill in the parameters.
 4. Run the CloudFormation YAML.

---

## **Step 10: Wait until the Workflow has finished**
 *In your Dynatrace Tenant:* 
 1. Open the Workflows App
 2. Open the workflow "Install AWS Use Cases {version} (main)""
 3. Click on Executions and click on the latest execution from the dropdown
 4. This will take you to the monitor of the workflow
 5. Wait until all tasks have completed (10 - 15min) 
 6. Once the installation workflow has completeled the use cases have installed successfully

## **Step 11: Find the Dashboard:**
 *In your Dynatrace Tenant:* 
 1. Open the Dashboards app
 2. Search for the Dashboard "AWs Automation {your aws account id}"


## **Unistall**
*If you wish to uninstall everything that the Installation created 
The Cloudfromation template creates a deletion workflow for easy uninstall*
 *In your Dynatrace Tenant:* 
 1. Go to "settings classic"
 2. Click on preferences and "Limit outbound connections" 
 3. In "Limit outbound connections" click on the "Add item" button
 4. Add the following allowed hosts:


