# Access Removal Workflow

## Purpose
This workflow is designed for use within the Saviynt Access Request System (ARS) to facilitate access removal based on manager approval.

## Background
This workflow supports two main scenarios:
1. Access removal initiated by the user for unused access.
2. Access removal requested by the manager.

## Pre-requisites
To ensure proper functionality, this workflow requires specific components:

1. **Email Templates for Notifications**: The workflow relies on templates available in this repository. Import all four templates with the prefix `AccessRequest-Removal-*` before uploading the workflow.

## Workflow Structure

1. **Manager Approval Block**: Includes a block for manager approval to confirm access removal.
2. **Decision Blocks**: Located at the bottom of the workflow image, these blocks handle approval outcomes:
   - **Allow Access Removal**: Proceeds with access removal.
   - **Reject Access Removal**: Retains access and sends a notification to the requester.

![Workflow Diagram](./images/visual.png)

## Configuration Instructions

1. **Adjust Timing**: Set time intervals according to your organization's policy. Some parts of the workflow are configured with a 2-minute duration for testing purposes, but in production, customers usually set this to 3-5 days. While some customers opt for two reminder notifications to managers, experience shows multiple approvals may not be as effective. Escalation to a secondary manager is generally more impactful.
2. **Email Templates and Team Names**: Customize the email templates as needed, including updates to your team name.

## Standard Enhancements
None
