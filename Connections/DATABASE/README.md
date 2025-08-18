# Saviynt DB Connector


The Saviynt DB connector facilitates the integration of databases into the Saviynt IGA platform by connecting to the source database, retrieving data, and ensuring that this data is properly mapped and synchronized with Saviynt’s identity governance workflows. It supports the management of users, accounts, and entitlements across databases.

### Key Features
- **Data Import**: The connector is used to import user information (e.g., usernames, employee IDs), account details, and entitlement data from a database into Saviynt. It supports pulling large datasets for accurate identity management.
- **Custom SQL Queries**: Saviynt allows you to write custom SQL queries to define the data that should be pulled from the database. This flexibility ensures that the data meets your organization’s specific requirements.
- **Mapping and Transformation**: The connector allows for the mapping of database fields (such as usernames, account names, and entitlement attributes) to Saviynt’s internal fields. You can configure data transformation to ensure proper data consistency and integration.
- **Scheduled Sync**: The DB connector supports scheduled synchronization, allowing for regular data imports from the database to keep the identity information in Saviynt up to date.
- **Support for Various Database Types**: The connector supports integration with various relational databases, including Oracle, SQL Server, MySQL, and others.

### Integration Components
- **Data Mapping**: A crucial feature of the DB connector is the ability to map database fields to Saviynt’s identity data fields. Custom field mappings ensure the correct transfer of attributes like username, email, manager, and entitlement type.
- **SQL Query Customization**: You can customize SQL queries to filter and retrieve specific user or entitlement data. This customization helps to ensure that the correct and relevant data is pulled into Saviynt.
- **User and Account Reconciliation**: The DB connector supports reconciliation, which ensures that users, accounts, and entitlements are kept in sync between the database and Saviynt.
- **Error Handling and Logging**: The connector provides detailed logs and error handling mechanisms, ensuring that any issues with data import or synchronization are easily identifiable.

### Use Cases
- **User Provisioning**: Automatically provision new users in Saviynt by importing user data from a database.
- **Account Synchronization**: Sync accounts across multiple systems by pulling data from relational databases to keep access data updated.
- **Entitlement Management**: Integrate entitlement data to manage roles, permissions, and security groups within Saviynt.
- **Data Governance**: Facilitate continuous data governance by ensuring accurate and timely data imports and synchronization.

### Setup and Configuration
- The connector setup involves providing database connection details, writing SQL queries to retrieve data, and mapping fields between the database and Saviynt.
- Admins can configure data pull schedules, error handling, and reconciliation settings to suit their operational needs.