# Application Onboarding Templates

This repository contains major onboarding templates for application information collection, centralizing all integration requirements. These templates can be modified, updated, or used as a basis for creating new ones to support various applications.

## Templates

### 1. Active Directory - Onboarding - User Source (SOT)
This template includes information for:
- Joiner, Mover, and Leaver (JML) use cases
- Attribute mapping and transformation
- Schema and other application integration details specific to Active Directory

**Note:** This template can be extended to any Source of Truth (SOT) or HR source integration. Saviynt uses it for Workday and other user information sources.

### 2. AWS Onboarding - IGA and CPAM
This template provides comprehensive details for AWS integration, including:
- PAM-specific configurations for Windows, Unix, and AD integration
- Use of master access for onboarding these systems into Saviynt PAM

**Adaptability:** It can also be applied to Azure, on-premises, and other cloud integrations, particularly for Privileged Access Management (PAM) requirements.

### 3. AzureAD Non-SOT Onboarding
This template facilitates the collection of integration parameters for AzureAD. While this connector is plug-and-play, the template ensures:
- Efficient setup for applications
- Faster data collection with reduced risk of errors

### 4. AzureAD SOT Onboarding
An extension of Templates 1 and 3, this template covers AzureAD Source of Truth (SOT) integration, including:
- Joiner, Mover, and Leaver (JML) use cases

### 5. Lenel Onboarding
This template supports complex and uncommon integrations using the General Purpose connector. It includes:
- A detailed listing of all required APIs

### 6. Oracle EBS Onboarding
A complete IGA integration template that provides:
- Standard application integration options per connector documentation
- Customization options for specific customer environments
- Identity governance packages and access request options for the Saviynt UI
- Additional fields for information collection

### 7. WinPS Onboarding
This template is designed for WinPS-based application integrations, suitable for:
- Exchange mailboxes
- Other Windows PowerShell-based applications

### 8. Salesforce Onboarding
A Salesforce integration template covering all IGA use cases except Joiner, Mover, and Leaver (JML) processes.

### 9. ServiceNow SOT Onboarding
A Source of Truth (SOT) template aligned with ServiceNow user integration, covering:
- Ticketing integration
- Managed system integration
- ServiceNow app integration
- API-based connections

### 10. Workday - Fine-Grained
A Separation of Duties (SoD) integration template with a rule set for fine-grained access control.

---

These templates serve as a foundational resource for application onboarding, enabling efficient, streamlined integrations across various platforms.
