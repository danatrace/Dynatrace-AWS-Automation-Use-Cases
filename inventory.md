# AWS Use Case Setup Inventory

https://img.shields.io/badge/AWS-Cloud-orange?logo=amazon-aws
https://img.shields.io/badge/Dynatrace-Automation-blue?logo=dynatrace
![tps://img.shields.io/badge/license-MIT-green
![Status](https://img.shields.io/badge/status-InProgresshis repository documents the setup of AWS use cases integrated with **Dynatrace Workflows** for automation and remediation.

---

## Table of Contents
- [Overview](#overview)
ites
- #aws-configuration
  - [OIDC Connection for Workflows](#oidc-connection-for-workflows)
tances
  - #temporary-s3-buckets
- #dynatrace-configuration
  - #policies
  - [Groups](#groups)
ce-user
  - #dashboard
  - [OIDC Connection
  - #subworkflows-original-names
  - [Workflows
  - [Chaos Workflows](#chaos-workflows)
ion
- #setup-instructions
- #references

---

## Overview
This setup creates AWS resources and Dynatrace configurations to enable automated workflows for remediation, chaos testing, and anomaly detection.

---

## Prerequisites
- **AWS Account** with admin permissions.
- **Dynatrace SaaS Tenant** with Workflow Automation enabled.
- Installed **AWS CLI** and **Dynatrace CLI**.
- IAM permissions for:
  - EC2
  - S3
  - Systems Manager
  - CloudFormation
  - IAM

---

## AWS Configuration

### OIDC Connection for Workflows
- **Connection Name:**  
  - `https://token.dynatrace.com`  
  - `https://hard.token.dynatracelabs.com`
- **References:**  
  - [Dynatrace AWS Workflows Setup](https://docs.dynatrace.com/docs/analyze-explore-automate/workflows  
  - [AWS IAM Identity Providers](https://us-east-1.console.aws.amazon1#/identity_providers
- **Role:** `dynatrace_oidc_conn_for_workflows`  
  **Permissions:**  

arn:aws:iam::aws:policy/AmazonEC2FullAccess
arn:aws:iam::aws:policy/AmazonS3FullAccess
arn:aws:iam::aws:policy/AmazonSSMFullAccess
arn:aws:iam::aws:policy/AWSCloudFormationFullAccess
arn:aws:iam::aws:policy/IAMFullAccess

- **IAM Policy:** `dynatrace_workflow_list_regions`  
Attached to allow listing AWS regions for dropdown menus in AWS Actions.

---

### Systems Manager
- **Custom Documents/Runbooks:**  
- `Dynwf-CreateCloudFormationStack`  
- `Dynwf-DeleteCloudFormationStack`
- **IAM Instance Profile:**  
- Role: `AmazonSSMManagedInstanceCore`

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

