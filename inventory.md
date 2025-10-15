# AWS Use Case Setup Inventory

This setup creates AWS resources and Dynatrace configurations to enable automated workflows for remediation,
chaos testing, and anomaly detection.

This is a list of assets that the setup creates:

## AWS Assets Created by the Setup

### AWS OIDC Connection for Workflows
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
- One EC2 instance to send API command and trigger Dynatrace workflow (will automatically be stopped after installation, can be terminated manually).  
- One EC2 instance to test **low disk space**. (Instance on which the chaos workflow will create the Low Disk Space problem to remediate )
  - Name Tag: Dynatrace-SSM-Action-Demo_{your Aws Accountid}
  - Problem Tag: problem: disk_{your Aws Accountid}
  - Security group does not have any inbound access to keep it 100% secure
  - Region: `us-east-1`  
- One EC2 instance to test **CPU saturation**. (Instance on which the chaos workflow will create the rocess CPU Saturation problem to remediate )
  - Name Tag: Dynatrace-SSM-Action-Demo_{your Aws Accountid}
  - Problem Tag: problem: cpu_{your Aws Accountid}
  - Security group does not have any inbound access to keep it 100% secure
  - Region: `us-east-1`  

---

### Temporary S3 Buckets
The Setup creates a Temporary S3 bucket to upload Cloudformation yaml files and pass them on to Cloudformation Create Stack.
The Bucket and content will be deleted after install.
- Bucket: `dynatraceinstall{execution id of th install workflow}`
- Permissions: No Public Access
- Region: `us-east-1`  
- Files:  
- `cloudformation_create_role_for_ssm_execution_ec2.yaml`  (Creates Instance Profile with AmazonSSMManagedInstanceCore attached for SSM Execution permissions on EC2 Instances)
- `create_remediation_demo_instances.yaml` (Creates the 2 Test Ec2 Instances for Low Diskspace and Process Cpu Saturation testing)

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

