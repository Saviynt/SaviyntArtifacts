# Schema Based Entitlement Import

Saviynt Enterprise Identity Cloud (EIC) offers an out-of-the-box (OOB) solution to automate manual entitlement reconciliation through schema-based entitlement import.

Saviynt’s schema-based entitlement jobs allow you to automate the reconciliation of manual entitlements. These jobs use specified schema files to pull entitlement data into Saviynt.

## Prerequisites
For schema-based entitlement imports, both SAV and CSV files are required:
- **SAV files** should be placed in the `File` directory.
- **CSV files** should be placed in the `Datafiles` folder.
- Ensure that the CSV file name starts with the prefix specified in the `FileNameStartswith` property within the SAV file.
- The SAV file name should be suffixed with `ENTITLEMENT_VALUES`.

## Integration Steps

### 1. Create the SAV File
Define an SAV file and place it in the `sav` folder under the file directory. Below is an example SAV file configuration for importing entitlements:

```plaintext
FILE_NAME_STARTS_WITH=Entitlement_Values_Import
SKIP_NO_OF_LINES=1
FILE_IMPORT_DELIMETER=,
EMAILTEMPLATE=SchemaFailureNotification
CREATE_SECURITYSYSTEM_IF_NOT_EXIST=YES
CREATE_ENDPOINT_IF_NOT_EXIST_IN_SECURITYSYSTEM=YES
CREATE_ENTITLEMENTTYPE_IF_NOT_EXIST_IN_ENDPOINT=YES
ENTITLEMENT_VALUES_NOT_IN_FILE_ACTION=INACTIVE
securitysystems,endpoints,entitlementtype,entitlement_value

### 2. Explanation of SAV File Properties

- **FILE_NAME_STARTS_WITH**: Specifies the prefix for the corresponding CSV file name.
- **SKIP_NO_OF_LINES**: Defines how many lines should be skipped at the beginning of the CSV file (e.g., for column headers).
- **FILE_IMPORT_DELIMETER**: Specifies the character used to separate data items in the CSV file (e.g., comma `,`).
- **CREATE_SECURITYSYSTEM_IF_NOT_EXIST**: Set to `YES` to automatically create a new security system if it doesn't already exist.
- **CREATE_ENDPOINT_IF_NOT_EXIST_IN_SECURITYSYSTEM**: Set to `YES` to create an endpoint within the specified security system if it doesn't exist.
- **CREATE_ENTITLEMENTTYPE_IF_NOT_EXIST_IN_ENDPOINT**: Set to `YES` to create a new entitlement type within the endpoint if it doesn't exist.
- **ENTITLEMENT_VALUES_NOT_IN_FILE_ACTION**: Defines the action for entitlements present in EIC but missing from the CSV file:
  - **INACTIVE**: Marks the entitlement as inactive.
  - **NOACTION**: Retains the existing entitlements.

---

### 3. Create the CSV File

Define the CSV file according to the schema in the SAV file. Place the CSV file in the **Datafiles** directory.

### Sample CSV File

```plaintext
securitysystems,endpoints,entitlementtype,entitlement_value,customproperty1,status,soxcritical
Amigopod,Amigopod,Access,IT Users,IT Engineers only,1,HIGH

### 4. Schedule the Job in the EIC Control Panel

1. Navigate to the **Job Control Panel** in Saviynt EIC.
2. Create a new job of type **File-Based Entitlement Import (Schema Entitlement)**.
3. Specify the job type as **Import Entitlement_Values**.
4. Schedule the job according to your organization's requirements.

### 5. Review Job Results

- **Success**: After the job completes successfully, the CSV file will be moved to the **success** folder for archiving.
- **Failure**: If the job fails, the CSV file will be moved to the **fail** folder for troubleshooting.

### Troubleshooting

#### Common Issue: "File Not Found" Error
- **Resolution**: Ensure that the latest CSV file is present in the **Datafiles** folder and that the corresponding SAV file is correctly placed in the **File** .