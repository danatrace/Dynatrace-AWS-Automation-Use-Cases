# AWS Use Case Setup Inventory

This setup creates AWS resources and Dynatrace configurations to enable automated workflows for remediation,
chaos testing, and anomaly detection.

This is a list of assets that the setup creates:

## AWS Assets Created by the Setup

### OIDC Connection for Workflows
The setup creates an Open ID Connection needed for Dynatrace Workflow Actions to connect to AWS Accounts.

- **Name of OIDC Connection that will be created :**  
  - `https://token.dynatrace.com`  (If setup is done for a Dynatrace production tenant)
  - `https://hard.token.dynatracelabs.com` (If setup is done for a Dynatrace sprint tenant) (For Dynatrace Internal use
    
- **References:**  
  - [Dynatrace AWS Workflows Setup](https://docs.dynatrace.com/docs/analyze-explore-automate/workflows)
  - [AWS OpenID Connect (OIDC)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html)
  - [AWS IAM Identity Providers](https://us-east-1.console.aws.amazon1#/identity_providers)
    
- **Name of the role that will be created:** `dynatrace_oidc_conn_for_workflows`  
  **I am policies that will be attached to this role:**  
    - <p> arn:aws:iam::aws:policy/AmazonEC2FullAccess <br> (Needed to give Dynatrace Actions the permissions to create/delete/modify Ec2 Instances) </p>
    - <p> arn:aws:iam::aws:policy/AmazonS3FullAccess <br> (Needed to give Dynatrace Actions the permissions to create/delete/modify S3 Buckets and upload and delete files) </p>
    - <p> arn:aws:iam::aws:policy/AmazonSSMFullAccess <br> (Needed to give Dynatrace Actions the permissions to create/execute/delete SSM Documents/Runbooks)</p>
    - <p> arn:aws:iam::aws:policy/AWSCloudFormationFullAccess <br> (Needed to give Dynatrace Actions the permissions to create/execute/delete Cloudformation Stacks)</p>
    - <p> arn:aws:iam::aws:policy/IAMFullAccess <br> (Needed to give Dynatrace Actions the permissions to create/delete IAM Roles)</p>

- **Custom IAM Policy that will be created and attached to the role:** `dynatrace_workflow_list_regions`  
Attached to allow listing AWS regions for dropdown menus in AWS Actions.

---

### AWS Systems Manager
- **Custom Documents/Runbooks:**  
- `Dynwf-CreateCloudFormationStack`  <br> (Needed for Dynatrace Actions to perform Create Cloudformation Stack using SSM Document) </p>
- `Dynwf-DeleteCloudFormationStack` <br> (Needed for Dynatrace Actions to perform Delete Cloudformation Stack using SSM Document)
- **IAM Instance Profile:**  
- Role: `AmazonSSMManagedInstanceCore` (Instance Profile that will be attached to the test Ec2 Instances to allow SSM Agent access)
- **References:**  
  - [Info about AmazonSSMManagedInstanceCore Instance Profile](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AmazonSSMManagedInstanceCore.html)
  - [AWS Systems Manager Documents](https://docs.aws.amazon.com/systems-manager/latest/userguide/documents.html)
  - [Link to Systems Manger Documents Overview in your Management Console](https://us-east-1.console.aws.amazon.com/systems-manager/documents?region=us-east-1#)

---

### EC2 Instances
- One EC2 instance to send API command and trigger Dynatrace workflow (stopped after installation).  
- One EC2 instance to test **low disk space**.  
- One EC2 instance to test **CPU saturation**.

---

### Temporary S3 Buckets
- Bucket: `dynatraceinstall52e2cf3c2fa54905a211165e4aa331f5`  
- Region: `us-east-1`  
- Files:  
- `cloudformation_create_role_for_ssm_execution_ec2.yaml`  
- `create_remediation_demo_instances.yaml`

---

## Dynatrace Configuration

### Policies
 
workflows-read-spans
workflows-write-logs
workflows-read-events
workflows-run-general
workflows-read-general
workflows-read-metrics
workflows-service-user
workflows-write-events
workflows-read-entities
workflows-run-functions
workflows-write-general
workflows-write-metrics
workflows-read-bizevents
workflows-run-send-emails
workflows-smartscape-read
workflows-run-readsettings
workflows-read-allworkflowactions
workflows-read-aws-action-connections


### Groups
- `workflow-user`
- `workflow-builder`
- `workflow-admin`

### Service User
- `workflow-service-user`

### Dashboard
- Automation use cases

### OIDC Connection
- Configured for workflows

---

### Subworkflows (Original Names)
- `subworkflow - dynatrace ingest custom alert`
- `subworkflow - dynatrace get all problem data`
- `subworkflow - aws create cloudformation stack`
- `subworkflow - aws run command on ec2 instance`
- `subworkflow - aws get host data of ec2 instance`
- `subworkflow - aws wait for run command execution`
- `subworkflow - dynatrace ingest custom info event`
- `subworkflow - aws davis forecast for aws volume usage`
- `subworkflow - dynatrace limit parallel runs of workflow`
- `subworkflow - aws disable public access for security group`
- `subworkflow - aws wait for systems manager document execution`
- `subworkflow - dynatrace check if davis problem is solved or closed`
- `template - dynatrace remediate aws ec2 instance process or memory saturation`
- `create-policies`

---

### Workflows
- AWS Security Hub remediate public S3 Bucket  
- AWS Security Hub create alert for public S3 Buckets  
- AWS remediate public security groups  
- AWS Security create alert for public security groups  
- AWS EC2 remediate high CPU or memory consumption by processes  
- AWS EBS remediate low storage  
- Delete AWS Use Cases  

---

### Chaos Workflows
- Create Low Storage Chaos  
- AWS S3 Create Public S3 Bucket  
- Create big logfile and fill disk  
- AWS Create Public Security Group Chaos/Test  
- Create CPU stress on EC2 instance with Stress-ng  

---

### Anomaly Detection
- Low Storage  
- Process CPU saturation  

