# Risk Based Approval with Escalation

## Purpose
This workflow is designed for use in the Saviynt Access Request System (ARS) to manage any requestable entitlement or access.

## Background
Organizations handle various access types, roles, and entitlements, each with different risk levels and required approvals. This workflow supports multiple approval levels:
1. **Single-Level Approval:** Example: Manager-only approval.
2. **Two-Level Approval:** Example: Manager and Entitlement Owner.
3. **Multi-Level Approval:** Example: Manager, Entitlement Owner (Rank 1), Rank 2 Owners, or Audit Control.

### Key Features:
- **Reminders and Escalations:** If approvers delay action, reminders are sent, or requests escalate to the next level. For high-risk entitlements without assigned rank owners, requests route to a default User Group, allowing them to assign an owner and approve on their behalf.
- **Rejection Notifications:** If a request at Level 2 or Level 3 is not approved within seven days, the requester is notified of rejection. If the request is with the Manager, it escalates to the Manager's manager.

This workflow streamlines access management, ensuring efficient and consistent access control.

## Requirements

To configure this workflow, the following components are needed:

1. **Email Templates:** Import the six `AccessRequest-*` templates (excluding `AccessRequest-Removal-*`) from this repository for notifications.
2. **User Groups:** Two User Groups handle approvals for high-risk entitlements missing rank owners. These groups also cover approvals when an owner is absent.
3. **Entitlement Risk Properties:** Upload a CSV file to set entitlement risk levels, which will enable appropriate approval workflows.
4. **Linked Artifacts:** This workflow addresses access request approvals. A separate workflow with email templates is available for access removals.

## Workflow Overview


![Workflow Diagram](./images/visual.png)

1. **Manager Approval Block:** Uses two email templates for approval and reminders. Approved requests move to Level 2 if required, while rejections send a denial notification. Escalations may route requests to a secondary manager, or, if no action is taken, follow rejection protocols.

2. **Risk-Based Conditions (IF-ELSE / Fallback Check Rank 1):** This block checks if:
   - Risk is medium and no Rank 1 Owner exists.
   - Risk is medium with an owner, routing requests to all owners for approval.

3. **Manager as Rank 1 Owner Check:** Skips Level 1 if the Manager is a Rank 1 Owner.

4. **High-Risk Rank 2 Owner Check:** For high-risk entitlements, requests move to Rank 2 Owners unless the Manager is among them. This is handled similarly to previous checks.

5. **Grant and Reject Access Blocks:** At the bottom of the workflow, these blocks notify requesters of the outcome.

## Configuration Instructions

1. **Adjust Timing:** Set approval timeframes according to organizational policies (currently 2 minutes for testing; production settings are usually 3–5 days). Some organizations send multiple reminders, but escalation to a secondary manager is often more effective.
2. **Customize Email Templates and Team Names:** Modify email content and team names as needed.
3. **Assign Users to User Groups:** Ensure the User Groups contain the correct users for approvals.
4. **Test Workflow:** Test all risk scenarios using the Entitlement Property CSV for setup.

## Common Enhancements

1. **Auto-Approval for Low-Risk Access:** Some organizations add auto-approval for low-risk requests (Risk Level 1) by reusing parts of this workflow.
2. **Additional Approval Levels or SoD Checks:** Some add more levels of approval or include Separation of Duties (SoD) checks before sending requests to Level 1.

This workflow has been successfully implemented for over 40 customers and is adaptable for future needs, allowing modifications to add levels or adjust approval criteria by updating entitlement properties.
