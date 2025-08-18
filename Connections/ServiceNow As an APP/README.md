# ServiceNow as a Managed Application with Saviynt EIC

Saviynt EIC provides the capability to integrate ServiceNow as a managed application for both reconciliation and provisioning (including de-provisioning). To set up this integration, follow these steps:

## Overview

Saviynt offers a connector module that can be configured to establish a connection with ServiceNow. Before initiating operations like reconciliation and provisioning, ensure network connectivity is established:
- The network must be whitelisted.
- Firewalls should allow traffic between Saviynt EIC and ServiceNow.
- Ensure the REST API credentials are valid.

## Integration Steps

### 1. Create a REST Connection
Begin by creating a REST connection, which will populate the template accordingly.

	![SNOW Diagram](./images/visual.png)

### 2. Provide a Connection JSON

#### Sample JSON
```json
{
  "authentications": {
    "acctAuth": {
      "authType": "Basic",
      "url": "",
      "httpMethod": "POST",
      "httpHeaders": {
        "content-Type": "application/json"
      },
      "properties": {
        "userName": "<user name>",
        "password": "<password>"
      },
      "httpContentType": "application/json",
      "expiryError": "ExpiredAuthenticationToken",
      "retryFailureStatusCode": [401],
      "authError": [
        "Internal server error"
      ],
      "timeOutError": "Read timed out",
      "errorPath": "message",
      "maxRefreshTryCount": 5,
      "tokenResponsePath": "access_token",
      "tokenType": "Basic",
      "accessToken": "Basic abcd"
    }
  }
}

####  Parameters

	![SNOW Diagram](./images/visual1.png)

- **authType**: Specifies Basic Authentication, which uses a `userName` and `password` for access.

- **url**: The endpoint for ServiceNow’s API where the REST request is directed. Replace `<domain name>` with your ServiceNow instance domain.

- **httpMethod**: Sets the HTTP method, here set to POST.

- **httpHeaders**: Defines the HTTP headers, with `Content-Type` set to `"application/json"` to ensure the request is JSON-formatted.

- **properties**: Contains the following credentials required for authentication:
  - **userName** and **password**: Credentials required for Basic Authentication with ServiceNow.

- **httpContentType**: Specifies that the content type for the request is `application/json`.

#### Error Handling

- **expiryError**: Specifies the error for an expired authentication token.

- **retryFailureStatusCode**: Lists status codes that trigger retry attempts, such as `401` (unauthorized).

- **authError**: Lists potential authentication errors, e.g., `"Internal server error"`.

- **timeOutError**: Defines the error message if the connection times out.

- **errorPath**: Points to the JSON path for the error message in the response, typically `"message"`.

- **maxRefreshTryCount**: Limits the number of retry attempts (e.g., `5`).

- **tokenResponsePath**: Specifies the path to the token in the response JSON, though typically more relevant for OAuth2 configurations. For Basic Authentication, it can remain unused.

- **tokenType** and **accessToken**: `tokenType` is set to `Basic`, with an example access token provided in `accessToken`.

#### Example JSON Configuration

Below is a sample JSON configuration with the described parameters:

```json
{
  "authentications": {
    "acctAuth": {
      "authType": "Basic",
      "url": "",
      "httpMethod": "POST",
      "httpHeaders": {
        "content-Type": "application/json"
      },
      "properties": {
        "userName": "<user name>",
        "password": "<password>"
      },
      "httpContentType": "application/json",
      "expiryError": "ExpiredAuthenticationToken",
      "retryFailureStatusCode": [401],
      "authError": [
        "Internal server error"
      ],
      "timeOutError": "Read timed out",
      "errorPath": "message",
      "maxRefreshTryCount": 5,
      "tokenResponsePath": "access_token",
      "tokenType": "Basic",
      "accessToken": "Basic abcd"
    }
  }
}

#### AccountEnTJson Configuration

This JSON configuration, `AccountEnTJson`, is designed for managing account and entitlement data from a ServiceNow instance. It handles data import for user accounts, groups, and roles, including entitlements and mappings. The configuration supports pagination and includes specific mapping for key fields and properties.

#### Structure

The configuration consists of the following main sections:
1. **accountParams**: Handles user account data retrieval and mapping.
2. **entitlementParams**: Manages entitlements (Groups and Roles) and their respective mappings.
3. **acctEntParams**: Defines relationships between accounts and entitlements, providing account-to-entitlement mappings.

##### 1. Account Parameters (`accountParams`)

	![SNOW Diagram](./images/visual2.png)

This section retrieves user account data from ServiceNow and maps it to the required properties.

- **connection**: `userAuth` - Authentication method.
- **processingType**: Sequential and Iterative, handling each account sequentially.
- **call**:
  - **call1**:
    - **url**: Retrieves user details from `sys_user`.
    - **httpMethod**: `GET`
    - **httpHeaders**: Authorization header with an access token.
    - **colsToPropsMap**: Maps fields from the ServiceNow API response to the desired properties (e.g., `accountID`, `DISPLAYNAME`, `status`).
    - **pagination**: Configured to handle paginated responses using `nextUrlPath`.

##### 2. Entitlement Parameters (`entitlementParams`)

Manages entitlements in ServiceNow, specifically groups and roles.

- **processingType**: Sequential and Iterative for handling multiple entitlements.
- **entTypes**:
  - **Group**:
    - **call**:
      - **call1**: Retrieves groups from `sys_user_group`.
      - **call2**: Retrieves active groups with display values.
      - **call3**: Filters active groups with the ITIL role.
      - **call4**: Retrieves groups with the "business_stakeholder" role.
    - **colsToPropsMap**: Maps API fields to entitlement properties, with constants for `customproperty30` and `confidentiality`.
  - **Roles**:
    - **call1**: Fetches roles, excluding those with "PNOW" in the name.
    - **colsToPropsMap**: Maps API fields to role properties (e.g., `entitlementID`, `entitlement_Value`, `displayname`).

##### 3. Account-Entitlement Parameters (`acctEntParams`)

Defines the relationship between accounts and entitlements for Groups and Roles.

- **entTypes**:
  - **Group**:
    - **call1**: Fetches group membership details from `sys_user_grmember`.
    - **pagination**: Handles paginated responses using `nextUrlPath`.
  - **Roles**:
    - **call1**: Retrieves user-role mappings from `sys_user_has_role`.

##### Sample Configuration

Below is a sample JSON structure for `AccountEnTJson`.

```json
{
  "accountParams": { ... },
  "entitlementParams": { ... },
  "acctEntParams": { ... }
}

#####  Usage Instructions for AccountEnTJson Configuration

To use the `AccountEnTJson` configuration effectively, follow these steps:

1. **ServiceNow Instance URL**: 
   - Replace all instances of `Service now domain URL' with the URL of your specific ServiceNow instance.
  
2. **Authentication Token**:
   - Ensure that the `${access_token}` placeholder in `httpHeaders` is configured with a valid access token. This token is required for authorized API access.

3. **Field Mapping Customization**:
   - Review and modify the `colsToPropsMap` mappings in each section to align with your system's field and property requirements. This mapping ensures that data fields from the API responses are correctly assigned to your desired attributes.

This configuration facilitates streamlined management of user accounts and entitlements by handling custom field mappings, pagination, and API calls efficiently.


#####  Account Parameters (accountParams)
This section defines the parameters used for managing user accounts.

- **connection**: Specifies the connection type as `"userAuth"`, which includes authentication details.
- **processingType**: Set to `"SequentialAndIterative"`, meaning requests will be processed in sequence.
- **call1**:
  - Retrieves user accounts from the ServiceNow `sys_user` table.
  - The URL includes parameters to fetch fields like `user_name`, `sys_id`, `email`, `status`, etc.
  - **colsToPropsMap**: Maps ServiceNow response fields to EIC attributes.
  - **pagination**: Uses `nextUrlPath` to handle paginated responses from ServiceNow.

#####  Entitlement Parameters (entitlementParams)
This section defines how entitlements (Groups and Roles) are managed.

#####  Groups
- **call1 to call4**:
  - Fetches group data, including `name`, `sys_id`, and `manager`.
  - Some calls (e.g., `call3` and `call4`) apply filters such as `role.name=Itil` and `role.name=business_stakeholder`.
  - Each call uses `colsToPropsMap` to map data to EIC attributes.
- **entMappings**: Defines entitlement mappings, specifying which fields in ServiceNow (e.g., `parent`, `value`) map to entitlement fields in EIC.

#####  Roles
- **call1**:
  - Retrieves roles using the `sys_user_role` endpoint, filtering out specific names (e.g., `name NOT LIKE 'PNOW'`).
  - Maps fields such as `sys_id` to `entitlementID` and `name` to `entitlement_Value`.

#####  Account-Entitlement Parameters (acctEntParams)
This section defines the relationship between accounts and entitlements for both groups and roles.

#####  Groups
- **call1**:
  - Uses the `sys_user_grmember` endpoint to map user memberships in groups.
  - Retrieves mappings for `accountID` (user) and `entitlementID` (group).
  - Uses the same pagination logic as `accountParams` to handle paginated results.

#####  Roles
- **call1**:
  - Uses the `sys_user_has_role` endpoint to map user role assignments.
  - Aligns `accountID` and `entitlementID` with ServiceNow’s user and role values.

#####  Key Points in JSON Configuration

- **Error Handling**: Each section of the configuration includes error handling to manage various response issues and retry logic when necessary.
- **Headers**: The `"Authorization": "${access_token}"` header is included in all requests to pass an access token for authentication.
- **Pagination**: A custom `nextUrl` logic is implemented to handle paginated data from the ServiceNow API.

#####  Provisioning and Deprovisioning

This section outlines how user provisioning and deprovisioning are handled using the provided JSON configurations.

#####  Create Account JSON

```json
{
  "call": [
    {
      "name": "call1",
      "connection": "userAUTH",
      "url": "",
      "httpMethod": "POST",
      "httpParams": "{\"city\":\"${user.city}\",\"country\":\"${user.country}\",\"email\":\"${user.email}\",\"first_name\":\"${user.firstname}\",\"last_name\":\"${user.lastname}\",\"phone\":\"${user.phonenumber}\",\"employee_number\":\"${user.employeeid}\",\"user_name\":\"${user.username}\",\"u_fullname\":\"${user.displayname}\",\"u_employement_type\":\"${user.employeeclass}\",\"u_discovery_source\":\"Saviynt\"}",
      "httpHeaders": {
        "Authorization": "${access_token}"
      },
      "httpContentType": "application/json",
      "unsuccessResponses": {
        "error.message": "Operation Failed"
      }
    }
  ],
  "accountIdPath": "call1.message.result.sys_id",
  "responseColsToPropsMap": {}
}


##### Main Structure (Create Account - `call1`)

- **call1**:
  - **name**: Identifies this call as "call1".
  - **connection**: Uses "userAUTH" to establish the required authentication.
  - **url**: Points to the `sys_user` table endpoint on ServiceNow, which is used to create user accounts.
  - **httpMethod**: Set to "POST" to create new records in ServiceNow.
  - **httpParams**: A JSON object containing user attributes as parameters:
    - Each field (e.g., `city`, `country`, `email`) is mapped dynamically using `${user.<attribute>}` placeholders, pulling data from the user object.
    - Static Field: `"u_discovery_source"` is set to `"Saviynt"` to mark the data origin.
  - **httpHeaders**: Sets the `Authorization` header with `"${access_token}"`, which provides an access token for authenticated requests.
  - **httpContentType**: Defined as `"application/json"`, indicating JSON formatting for the request body.
  - **unsuccessResponses**: If the API returns an error, such as in the `error.message` field, this field logs it as `"Operation Failed"`.

##### Account ID Path (accountIdPath)

- Specifies where in the response the unique `sys_id` of the newly created user can be found, which is `"call1.message.result.sys_id"`.

##### Response Columns to Properties Map (responseColsToPropsMap)

- This field is left empty (`{}`), implying no specific mapping is required from response properties to EIC attributes.

---

##### Add Access JSON - Sample Add Access JSON

```json
{
  "call": [
    {
      "name": "Group",
      "connection": "userAuth",
      "url": "",
      "httpMethod": "POST",
      "httpParams": "{\"group\":\"${entitlementValue.entitlementID}\",\"user\":\"${account.accountID}\"}",
      "httpHeaders": {
        "Authorization": "${access_token}"
      },
      "httpContentType": "application/json"
    }
  ]
}

##### Main Structure (Create Account - `call1`)

- **call1**:
  - **name**: Identifies this call as "call1".
  - **connection**: Uses `"userAUTH"` to establish the required authentication.
  - **url**: Points to the `sys_user` table endpoint on ServiceNow, used to create user accounts.
  - **httpMethod**: Set to `"POST"` to create new records in ServiceNow.
  - **httpParams**: A JSON object containing user attributes as parameters:
    - Each field (e.g., `"city"`, `"country"`, `"email"`) is dynamically mapped using `${user.<attribute>}` placeholders, pulling data from the user object.
    - Static Field: `"u_discovery_source"` is set to `"Saviynt"` to mark the data origin.
  - **httpHeaders**: Sets the `Authorization` header with `"${access_token}"`, which provides an access token for authenticated requests.
  - **httpContentType**: Defined as `"application/json"`, indicating the request body is formatted in JSON.
  - **unsuccessResponses**: If the API returns an error (e.g., in `error.message`), this field logs it as `"Operation Failed"`.

##### Account ID Path (accountIdPath)
- Specifies where in the response the unique `sys_id` of the newly created user can be found, which is `"call1.message.result.sys_id"`.

##### Response Columns to Properties Map (responseColsToPropsMap)
- This field is left empty (`{}`), implying no specific mapping is required from response properties to EIC attributes.

---

##### Add Access JSON - Sample Add Access JSON

```json
{
  "call": [
    {
      "name": "Group",
      "connection": "userAuth",
      "url": "",
      "httpMethod": "POST",
      "httpParams": "{\"group\":\"${entitlementValue.entitlementID}\",\"user\":\"${account.accountID}\"}",
      "httpHeaders": {
        "Authorization": "${access_token}"
      },
      "httpContentType": "application/json"
    }
  ]
}

##### Main Structure (Add Access - `Group` Call)

- **Group Call**:
  - **name**: Identifies this call as `"Group"`, which is the entitlement type.
  - **connection**: Uses `"userAuth"` to authenticate the request.
  - **url**: The endpoint URL points to the `sys_user_grmember` table in ServiceNow, which manages user-group memberships.
  - **httpMethod**: Set to `"POST"` to create a new membership record (i.e., add a user to a group).
  - **httpParams**: Defines the body of the request in JSON format:
    - `"group"`: Populated with `${entitlementValue.entitlementID}`, which corresponds to the ID of the group.
    - `"user"`: Populated with `${account.accountID}`, which represents the user's unique ID in ServiceNow.
  - **httpHeaders**: Sets the `Authorization` header using `"${access_token}"` for authentication.
  - **httpContentType**: Specifies `"application/json"`, indicating the request body is formatted in JSON.

---

##### Sample Remove Access JSON

```json
{
  "call": [
    {
      "name": "Group",
      "connection": "userAuth",
      "url": "",
      "httpMethod": "GET",
      "httpHeaders": {
        "Authorization": "${access_token}"
      },
      "httpContentType": "application/json",
      "successResponses": {
        "statusCode": [200]
      },
      "unsuccessResponses": {
        "statusCode": [403]
      }
    },
    {
      "name": "Group",
      "connection": "userAuth",
      "url": "",
      "httpMethod": "DELETE",
      "httpHeaders": {
        "Authorization": "${access_token}"
      },
      "httpContentType": "application/json",
      "successResponses": {
        "statusCode": [204]
      },
      "unsuccessResponses": {
        "statusCode": [403]
      }
    }
  ]
}



##### Retrieving the User-Group Membership Record

- The first call performs a `GET` request to check if a user is currently a member of the specified group by querying the `sys_user_grmember` table with the `user=${account.accountID}` filter.
- If successful (status code `200`), it retrieves all group memberships associated with the specified user.

##### Deleting the User-Group Membership Record

- The second call performs a `DELETE` request to remove the user from the target group.
- This call dynamically resolves the URL endpoint for the specific membership record (`sys_user_grmember`) to delete, using a Groovy script embedded in the URL:
  - The script iterates through the response of the first call (`response.Group1.message.result`), locating the `sys_id` of the membership record where the group matches `entitlementID`.
  - Once located, it returns the `sys_id` to complete the URL for deletion.
- If successful, the response status code is expected to be `204` (indicating successful deletion without returning content).

---

## Troubleshooting

### Token Expiry Issue

In most ServiceNow connections, the token expires and does not refresh automatically.

### Resolution

- Ensure that the error code related to token expiry is properly defined in the connection JSON so that the token can be refreshed when necessary.


