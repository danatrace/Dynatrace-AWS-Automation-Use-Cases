# AWS Use Case Setup Inventory

This setup creates AWS resources and Dynatrace configurations to enable automated workflows for remediation,
chaos testing, and anomaly detection.

This is a list of assets that the setup creates:

## AWS Assets Created by the Setup

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

## Dynatrace Assets created

### Workflow Policies
These Policies are created so they can be attached to the Groups and Service users and create an Enterprise grade permissions system for Dynatrace Workflows.
The Polices are split up to allow for easy adding and removing of specific permissions in the groups!

- <p> workflows-admin <br> (Grants permission to become workflow admin which gives read, write and execute access to all workflows in the tenant ) </p>
- <p> workflows-read-logs <br> (Grants permission to read logs from workflow tasks) </p>
- <p> workflows-read-spans <br> (Grants permission to read records from the spans-table from workflow tasks) </p>
- <p> workflows-write-logs <br> (Grants permission to write logs from workflow tasks) </p>
- <p> workflows-read-events <br> (Grants permission to read records from the events-table. Needed for Event Triggers, DQL and Javascript Tasks that read events!) </p>
- <p> workflows-run-general <br> (Grants permission to run workflows) </p>
- <p> workflows-read-general <br> (Grants permission to the workflows app, Read permissions for workflows of which the user has ownership (through user or group) or workflows that are shared with them! Read Access to General workflow Actions Execute DQL Query, Http Request and Run Javascript.  <br> Also Allow access to read Calendars, Rules and access to Automation and System Events. Allows read access to storage buckets (events, logs and metrics need separate permissions)) </p>
- <p> workflows-read-metrics <br> (Grants permission to read metrics (timeseries) from workflow tasks) </p>
- <p> workflows-service-user <br> (Gants permission to set service user as actor in workflows) </p>
- <p> workflows-write-events <br> (Grants permission to write/ingest events with workflow tasks) </p>
- <p> workflows-read-entities <br> (Grants permission to read records from entities from workfow tasks) </p>
- <p> workflows-run-functions <br> (Grants permissions to run javascript tasks in workflows) </p>
- <p> workflows-write-general <br> (Grants permission to save workflows, calendars and rules) </p>
- <p> workflows-write-metrics <br> (Grants permission to write metrics from workflow tasks) </p>
- <p> workflows-read-bizevents <br> (Grants permission to read records from the bizevents-table from workflow tasks) </p>
- <p> workflows-run-send-emails <br> (Grants permission to send emails from tasks) </p>
- <p> workflows-smartscape-read <br> (Grants permission to query smarscape from workflow tasks) </p>
- <p> workflows-run-readsettings <br> (Grants access to settings from workflow tasks) </p>
- <p> workflows-read-allworkflowactions <br> (Grants permission to additional workflow actions like aws, github etc.) </p>
- <p> workflows-read-aws-action-connections <br> (Grants permission to set AWS Connections in Workflow Actions) </p>


### Groups
- <p> workflow-user <br> (The Purpose of the Workflow user is to run existing workflows that were created by the Workflow Builder. The perfect role for someone that consumes automation, but does not build automations.) </p>
   - Can access to the workflows app
   - Can see workflows, workflow executions and can see,run,cancel and restart workflows that are
     - public (shared with all users)
     - are shared within the usergroup the user belongs to
   - Can not see or run private workflows
   - Can not write (create or change) workflows

- <p> workflow-builder <br> (The Purpose of the Workflow builder is to create, modify and run workflows. They have the expertise to create automation services that can be triggered automatically and manually by the Workflow-User) </p>
   - Has the same access as the Workflow User
   - Can write/create new workflows and modify existing ones
   - Can share workflows with a usergroup
   - Can share workflows with all users (set public)
   - Can set a service user as Actor

- <p>  workflow-admin <br> (The Purpose of the Workflow Admin is to make sure workflows are built properly and that unused workflows dont create unnecesary cost) </p>
   - Has the same access as the Workflow User and Workflow Builder
   - Has Access to all Workflows!




### Service User
- <p> workflow-service-user <br> (The Purpose of the workflow-service-user is to restrict dynatrace permissions to workflows executions, by setting the service user as actor of workflows. For example if you want to give a user access to bizevents in workflows but not across the entire dynatrace tenant
  setting the service user as actor and allowing the user to execute workflows with that actor, restricts the given permissions to workflow executions only! ) </p>
- **References:**  
  - [Dynatrace Service User](https://docs.dynatrace.com/docs/manage/identity-access-management/user-and-group-management/access-service-users)

### Dashboard
The Setup creates a Dashboards that is useful to Observe and test the AWS use cases
- Name: Automation use cases

### OIDC Connection
The Setup also creates the AWS OIDC connection settings in Dynatrace
- Name: The Name that was set in the Cloudformation Stack creation (ideally the Acccount id)

---

### Subworkflows (Automation Templates Used by Install, Remediation and Chaos Workflows)
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

### Davis Anomaly Detections
- Low Storage  
- Process CPU saturation  

