# Snapshot and Patching Workflow using AWS Step Functions and CloudFormation

This repository contains a CloudFormation template for deploying an automated workflow that handles EC2 snapshot creation and patching using AWS Step Functions. The workflow ensures data integrity by introducing a dependency: patching will only proceed if snapshot creation succeeds.

## Features

- **Automated Workflow**: Orchestrates snapshot creation and patching using AWS Step Functions.
- **Error Handling**: Ensures patching does not proceed if snapshot creation fails.
- **Scheduled Execution**: Triggered by an EventBridge rule running every second Tuesday at midnight (aligning with Microsoft's Patch Tuesday schedule).
- **Scalability and Security**: Uses IAM roles for secure access to resources and integrates with Lambda and AWS Systems Manager.

## Components

- **CloudFormation Template**: Defines resources including IAM roles, Lambda functions, and Step Functions state machine.
- **Lambda Function**: Creates snapshots for EC2 instances based on tags.
- **Step Functions Workflow**: Manages task dependencies with clear error handling.
- **EventBridge Rule**: Schedules the workflow execution.

## Deployment Instructions

1. Clone the repository.
2. Deploy the CloudFormation template using the AWS Management Console or CLI.
3. Update parameters as needed:
   - `StateMachineName`: Name of the Step Functions state machine.
   - `ScheduleExpression`: Adjusted to "cron(0 0 ? * 3#2 *)" for execution on the second Tuesday of each month.

## Workflow Diagram

```text
SnapshotCreation -> Patching
  On Failure: Workflow stops with a failure state.
