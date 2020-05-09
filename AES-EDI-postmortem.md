# Root Cause Analysis of the AES EDI (Jira Issue No. AESCIS-38263 #400)

## Date
2020-05-08

## Authors
*Riya Agrahari* - 
*SRE*

## Status
Resolved

## Summary
The customer did not received the data from AES EDI. 
The investigation showed that the file with the data was actually sent.
It was not processed due to an issue with the AES CIS service [Jira Issue No: AESCIS-38263].

## Impact
About 486,000 records were affected.
EDI to CIS monitoring service were affected as per the CIS side.

## Root Causes
Customer Information Service could not process the file due to lack of available resources.

## Trigger
Customer reported no records have been processed.

## Resolution
Restarting the *AES CIS* monitoring service allowed us to spot the missed records that were not discovered automatically 
and where they were stuck.
Re-running Garbage Collector freed enough resources to properly process all 486,000 records.

## Detection
Our customer create this Jira to alert us on this failure.


## Action Items
| Action Item | Type | Owner | Bug |
| ----------- | ---- | ----- | --- |
| Write monitoring policy to detect records missings | prevent | JRT | **Done** |
| Set monitoring on a third-party in order to have no Single-Point-of-Failure | prevent | JRT | **ToDo** |
| Create auto-scaling group of our workers to avoid this incident in the future | prevent | JRT | **On Going** |
| Monitor the data ingesters and processors (ETL) | prevent | JRT | (Jira Issue No: AESCIS-38263)**ToDo** |
| Validate adjacent architecture to verify correct monitoring and scale-out policies | prevent | JRT | **ToDo** |

## Lessons Learned
We need to validate that our monitoring architecture fits our SOA infrastructure.

## Timeline

2020-05-08 (*all times UTC*)

| Time  | Description |
| ----- | ----------- |
| 11:06 | Issue reported |
| 11:56 | SRE logged on console to validate architecture health |
| 11:56 | SRE logged on CIS platform to check monitoring agents |
| 11:56 | SRE restarted monitoring service |
| 11:56 | SRE validated files processing status |
| 11:56 | SRE discovered records missing|
| 12:05 | SRE re-sent records to process |
| 12:15 | SRE validated correct data processing of the records files |
| 13:00 | SRE validated correct completion of the data processing of all the 486,000 records files |
| 13:15 | SRE modified monitoring agent to correct view Service health |
| 13:25 | SRE elevated change to set AutoScaling on these resources |