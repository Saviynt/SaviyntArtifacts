# Saviynt Performance Analytics

This document contains performance-related analytics for monitoring and evaluating key components within the Saviynt platform. Each entry includes a unique analytic name, category, and the SQL query used to derive insights from the platform's database.

---

## 1. User Import Success Rate

**Category:** Admin\
**Query:**

```sql
SELECT DISTINCT
  JOBNAME AS JOB_NAME,
  IFNULL(TRIGGERNAME, 'User Upload From UI') AS TRIGGER_NAME,
  IFNULL(EXTERNALCONNECTION, '') AS JOB_TRIGGER,
  CASE SAVRESPONSE WHEN 'Success' THEN 'Success' ELSE 'Failed' END AS STATUS_OF_JOB,
  JOBSTARTDATE AS STARTDATE_OF_JOB,
  JOBENDDATE AS ENDDATE_OF_JOB
FROM ecmimportjob
WHERE JOBNAME IN ('UserImportJob', 'UserImportFullJob', 'SchemaUserJob')
  AND JOBSTARTDATE >= (UTC_DATE() - INTERVAL 30 DAY)
```

## 2. Rule Run Evaluation Success Rate

**Category:** Admin\
**Query:**

```sql
SELECT DISTINCT
  JOBNAME AS JOB_NAME,
  SAVRESPONSE AS STATUS_OF_JOB,
  JOBSTARTDATE AS STARTDATE_OF_JOB,
  JOBENDDATE AS ENDDATE_OF_JOB
FROM ecmimportjob
WHERE JOBNAME LIKE 'RuleRunJob'
  AND JOBSTARTDATE >= (UTC_DATE() - INTERVAL 30 DAY)
```

## 3. SOD Evaluation Job Success Rate

**Category:** SOD\
**Query:**

```sql
SELECT DISTINCT JOBNAME AS JOB_NAME, TRIGGERNAME AS TRIGGER_NAME, EXTERNALCONNECTION AS CONNECTION_NAME, SAVRESPONSE AS STATUS_OF_JOB, JOBSTARTDATE AS STARTDATE_OF_JOB, JOBENDDATE AS ENDDATE_OF_JOB
FROM ecmimportjob
WHERE JOBNAME LIKE 'RiskSODEvaluationJob'
  AND JOBSTARTDATE >= (UTC_DATE() - INTERVAL 30 DAY)
```

## 4. Provisioning - Top 5 Applications by Tasks

**Category:** Admin\
**Query:**

```sql
SELECT a.TASKKEY, a.TASKTYPE, e.endpointname AS APPLICATION
FROM arstasks a
JOIN endpoints e ON a.endpoint=e.endpointkey AND e.displayname IS NOT NULL
JOIN (
  SELECT COUNT(*) AS totalCount, a.endpoint AS EP
  FROM arstasks a
  GROUP BY a.endpoint
  ORDER BY totalCount DESC
  LIMIT 5
) AS TEMP ON TEMP.EP=a.endpoint
```

## 5. Provisioning - Pending Tasks

**Category:** Admin\
**Query:**

```sql
SELECT 'Pending' AS STATUS_OF_TASK,
  CASE
    WHEN (UTC_TIMESTAMP() - INTERVAL 12 HOUR) >= TASKDATE THEN 'More than 12 Hours'
    WHEN (UTC_TIMESTAMP() - INTERVAL 8 HOUR) >= TASKDATE THEN 'More than 8 Hours'
    WHEN (UTC_TIMESTAMP() - INTERVAL 4 HOUR) >= TASKDATE THEN 'More than 4 Hours'
    ELSE 'Less than 4 hours'
  END AS TASK_PENDING_FOR,
  endpoints.DISPLAYNAME AS APPLICATION
FROM arstasks
JOIN endpoints ON arstasks.endpoint=endpoints.endpointkey
WHERE arstasks.STATUS=1
  AND arstasks.TASKTYPE IN (1,2,3,4,6,8,12,14,31)
```

## ... More analytics available in source

Let me know if you'd like the rest formatted as well or if you want them grouped into a table-based layout. ✅

