# Saviynt SAV Roles Overview

In Saviynt, **SAV roles** (Saviynt Access Viewer roles) are predefined access roles that determine the permissions and capabilities of users within the Saviynt platform. These roles govern what users can view, edit, or administer within the system, allowing organizations to implement precise access controls over identity and access management (IAM) tasks. SAV roles are essential for securing sensitive information, delegating administrative tasks, and ensuring that users have only the permissions necessary for their roles. Saviynt provides default SAV roles to cover a broad range of user personas, but organizations often require custom roles to address specific needs. This document provides an overview of how SAV roles work and examples of custom roles for common IT functions.

## How SAV Roles Work

### Role Definitions
SAV roles are preconfigured with specific permissions to support various functions within the Saviynt ecosystem, such as managing users, applications, policies, and reports. Each role has a set of permissions that define what users can see (view permissions) and do (action permissions) within Saviynt, including creating, modifying, or viewing reports, access requests, and user attributes. Roles are assigned based on organizational needs, allowing certain users to perform administrative functions while limiting access for others.

### Role Assignment
Roles are assigned based on the **principle of least privilege**, ensuring users receive only the access they need to perform their tasks. Users can be assigned one or multiple roles, combining permissions as needed. Assignments can be manual, automated, or rule-based, depending on an organization’s IAM strategy.

### Segregation of Duties (SoD)
Saviynt incorporates **SoD controls** to prevent conflicts of interest, particularly in sensitive functions. SoD policies prevent users with conflicting roles (e.g., access requestor and approver) from having both roles simultaneously, reducing fraud risk and ensuring regulatory compliance.

### Access and Permissions Management
SAV roles include granular permissions for module, data, and action levels, enabling fine-tuned control so users perform only permitted actions (e.g., read-only access). Permissions are reviewed and updated as roles evolve, aligning with new modules or features.

### Audit and Reporting
All changes related to SAV roles (e.g., assignments, permission changes) are logged in Saviynt’s audit trail, supporting compliance and traceability. Reports on role assignments, usage statistics, and SoD conflicts aid in audit and compliance functions.

## Benefits of Using SAV Roles
- **Enhanced Security**: Precise access control ensures users have only the permissions they need.
- **Compliance Support**: SoD and audit trails support regulatory compliance.
- **Efficiency**: Predefined roles streamline provisioning, role assignments, and access requests.
- **Scalability**: SAV roles provide scalable access control as organizations grow.

## Example SAV Roles

### 1. IT Administrator Role
- **Purpose**: For IT administrators managing users, access policies, and configurations.
- **Permissions**:
  - Full administrative access to user and identity management, provisioning, and de-provisioning.
  - Permissions to modify application configurations, roles, and workflows.
  - Ability to create and update access policies, including SoD rules.
- **Usage**: The IT administrator can manage and enforce access policies, handle access requests, and maintain configurations, ensuring alignment with security standards.

### 2. IT Auditor Role (View-Only)
- **Purpose**: For IT auditors needing read-only access to audit logs, reports, and compliance data for review.
- **Permissions**:
  - View-only access to audit logs and access request history, enabling tracking of access changes and assignments.
  - Read access to system configurations, user roles, and permission settings.
  - Ability to generate and export compliance reports without modification permissions.
- **Usage**: IT auditors use this view-only role to review system access patterns, verify role assignments, and support regulatory compliance without altering data.

## How These Roles Support IAM Objectives

- **Enhanced Security**: By defining specific permissions, the IT administrator can handle configurations while the IT auditor can review data independently, reducing risk and maintaining data integrity.
- **Segregation of Duties (SoD)**: These roles follow SoD principles, ensuring that auditors don’t have system change permissions, and administrators don’t have unchecked authority over logs.
- **Compliance Support**: View-only access for auditors allows for secure compliance processes without exposing critical settings to unauthorized changes.

Saviynt’s flexible SAV roles empower administrators, auditors, and other key stakeholders, supporting security and compliance while facilitating efficient and effective access management.
