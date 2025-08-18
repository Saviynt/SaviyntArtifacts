# E-mail Template

**E-mail Template** is a standard object used to share the status of various operations with Users. These templates are commonly used for notifications, alerts, and other information/status sharing. This collection contains 10 email templates used for Access Request Approval and Rejection Workflows.

**Email Templates:**

* AccessRequest-ApprovalNotify
* AccessRequest-ApprovalNotify-Reminder
* AccessRequest-Approved
* AccessRequest-Discontinued
* AccessRequest-Expired
* AccessRequest-Rejected
* AccessRequest-Removal
* AccessRequest-Removal-Approved
* AccessRequest-Removal-Manager-Reject
* AccessRequest-Removal-Reminder

![Email Template List](./images/visual.png)


## Working Model

These templates can be used for various purposes. In this collection, they are used for two workflows to send notifications based on the status of an Access Request.


## Prerequisites

| Object | Purpose |
|---|---|
| N/A | N/A |


## Integration Steps

1. As an Admin, navigate to the **Transport** module in the Saviynt UI.
2. Click on **Import Package**.
3. Choose the package to import (select the ZIP file).
4. Enter a **Business Justification** for the import (e.g., "Importing Access Request Email Templates").
5. Click on "**View Summary**".
6. Review the summary and click on "**REQUEST**" to import the package.


## Troubleshooting

* **Workflow might not be added in Transport:** Verify that the imported workflows are added to the appropriate transport configuration within the Saviynt UI.
* **SMTP is not configured:** Ensure that your Saviynt environment has a properly configured SMTP server to send email notifications.
