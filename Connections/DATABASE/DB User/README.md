# Saviynt Database Connectors for user or HR Directory integration

Saviynt EIC offers the capability to integrate database applications with Saviynt through configurable database connectors. Saviynt provides OOB templates that can be customized to meet specific integration needs. This README describes how to configure the connectors for pulling users, accounts, and entitlements from a database.

## Prerequisites

Before you begin the integration process, ensure that:

- Network connectivity is established between Saviynt and the database.
- The network should be whitelisted, and the firewall should allow bi-directional traffic between Saviynt and the database.
  
Ensure you have the necessary credentials (username, password) and database details (hostname, schema) to establish the connection.

## Integration Steps

### 1. Database Connector for Users

This section explains how to configure a database connector to pull user data.

#### Steps to Set Up:

1. **Provide a Connection Name and Description.**

	![Workflow Diagram](./images/visual.png)

2. **Select Connection Type as DB:** The template for an Active Directory (AD) connection will load.
3. **Provide the URL:** Enter the hostname of the database, along with the schema.
4. **Provide the Username and Password** for the database.
5. **Specify the Database Driver Name**: For MySQL, use `com.mysql.jdbc.Driver`.

	![Workflow Diagram](./images/visual1.png)

6. **Configure User Import**: Example XML configuration for user import:

```xml
<dataMapping>
    <before-import></before-import>
    <sql-query description="This is the Source DB Query" uniquecolumnsascommaseparated="username">
        <![CDATA[
        select username, 
               (CASE WHEN statuskey IS NULL THEN '1' ELSE NULL END) as manager 
        from users 
        where statuskey = 1;
        ]]>
    </sql-query>
    <importsettings>
        <zeroDayProvisioning>false</zeroDayProvisioning>
        <userNotInFileAction>NOACTION</userNotInFileAction>
        <checkRules>true</checkRules>
        <buildUserMap>false</buildUserMap>
        <userReconcillationField>username</userReconcillationField>
    </importsettings>
    <mapper description="This is the mapping field for Saviynt Field name" dateformat="date">
        <mapfield saviyntproperty="username" sourceproperty="username" type="character"></mapfield>
        <mapfield saviyntproperty="manager" sourceproperty="manager" type="character"></mapfield>
    </mapper>
    <after-import description="SQL"></after-import>
</dataMapping>
