

# 00 Basic Info 



## 00.01 view

vw_function:  The latest data [Actually Today left outer Yesterday ] 

<img src="https://miro.medium.com/max/1400/1*UAPgZRnhFG29C0nDFi5D0A.png" alt="What is the difference between an inner and an outer join in SQL? | by Kate  Marie Lewis | Towards Data Science" style="zoom:33%;" />

```KQL
vwOpportunity(earlierThan:datetime=datetime(2099-12-31T00:00:00.0000000Z), snapshotIndex:int=0)	"{
let lastETLTime = toscalar(
Opportunity | where ETLDate <= earlierThan | distinct ETLDate | sort by ETLDate desc | extend rank = row_number(0) | summarize minif(ETLDate, rank<=snapshotIndex)
);
let lastExETLTime = toscalar(
OpportunityEx | where ETLDate <= earlierThan | distinct ETLDate | sort by ETLDate desc | extend rank = row_number(0) | summarize minif(ETLDate, rank<=snapshotIndex)
);
Opportunity
| where ETLDate == lastETLTime
| join kind=leftouter (
    OpportunityEx
    | where ETLDate == lastExETLTime
) on CRMOpportunityID
| project-away CRMOpportunityID1
}"
```

Functions that do not need vw but still the latest: 

| Calendar() | Person() | Geography | ProductWorkload() |
| ---------- | -------- | --------- | ----------------- |
|            |          |           |                   |

# 01 Opportunity





# 02 Engagement



# 03 Other Charts



## 03.01 HeadTrax

```
 cluster('1es.kusto.windows.net').database('HeadTrax').Person
    | where CurrentlyEmployed
    | project Alias=toupper(Alias), DisplayName
```

## 03.02 vwOpportunityPipeline()

**ID, Fiscal_Time, Product_category,[ Pipeline**: 预测收入]



| ColumnName          | ColumnType | Sample                            | Note           |
| ------------------- | ---------- | --------------------------------- | -------------- |
| CRMOpportunityID    | string     | 7-VP32SVFQM                       |                |
| ETLDate             | datetime   | \|2021-08-24 00:00:00.000000      |                |
| FiscalMonth         | datetime   | \|2020-03-01 00:00:00.000000      | 与Dueddate配套 |
| FiscalQuarter       | string     | FY20-Q3                           |                |
| FiscalYear          | string     | FY20                              |                |
| Pipeline            | decimal    | 2971                              | 预测收入       |
| ProductFamily       | string     | M365 Apps for business            |                |
| ProductQuantity     | real       | 31                                |                |
| QualifiedPipeline   | real       | 0                                 |                |
| RevSumCategory      | string     | M365 Apps for business            |                |
| RevSumDivision      | string     | O365 Standalone Office Commercial |                |
| SuperRevSumDivision | string     | O365 Core - Non M365              |                |













# Assignment 1



## T1-AVD Engagement

```Kql
# AVD Engagement
vwEngagementMilestone(azureSecOnly=false)
| join kind=inner vwEngagement() on CRMEngagementID
| where EngagementMilestoneWorkload == "Infra: Native Azure Virtual Desktop"
| where isnotempty(CRMEngagementID)
| where EngagementEstimatedCompletionDate >= datetime(2021-04-01) // temporarily using last FY Q4 since current FY Q1 hasn't ended yet
| distinct CRMEngagementID, TPID, EngagementStatus, EngagementEstimatedCompletionDate
| join kind=inner Calendar() on $left.EngagementEstimatedCompletionDate == $right.FiscalDate
| project CRMEngagementID, TPID, EngagementStatus,
    ClosedQuarter=iif(EngagementStatus == "Closed-Completed", FiscalQuarter, ""),
    ClosedMonth=iif(EngagementStatus == "Closed-Completed", startofmonth(EngagementEstimatedCompletionDate), datetime(null))
| join kind=inner vwEngagementTeam() on CRMEngagementID
| join kind=inner Person() on $left.EngagementTeamAlias == $right.Alias
| join kind=inner (
    cluster('1es.kusto.windows.net').database('HeadTrax').Person
    | where CurrentlyEmployed
    | project Alias=toupper(Alias), DisplayName
) on Alias
| project CRMEngagementID, TPID, EngagementStatus, ClosedQuarter, ClosedMonth,
    Alias=EngagementTeamAlias, DisplayName, StandardTitle=trim_end("\\d", StandardTitle), Qualifier1, Qualifier2
| extend
    QualifiedRole=case(
        StandardTitle == "Solution Area Specialists IC" and Qualifier1 == "N/A" and Qualifier2 in ("Modern Work", "MW-Surface", "Security"), "Yes",
        StandardTitle == "Technology Specialists IC" and Qualifier1 == "N/A" and Qualifier2 == "Cloud Endpoint", "Yes",
        "No"
    )
```

## T2-TP ACR FY21Q4

```Kql
let endOfQuarterMonth = toscalar(datetime(2021-06-01));
let endOfPrevQuarterMonth = datetime_add("month", -3, endOfQuarterMonth);
let quarter = toscalar(Calendar() | where FiscalDate == endOfQuarterMonth | project FiscalQuarter);
let tpids = materialize(vwEngagementMilestone(azureSecOnly=false)
| join kind=inner vwEngagement(azureSecOnly=false) on CRMEngagementID
| where EngagementMilestoneWorkload == "Infra: Native Azure Virtual Desktop"
| where isnotempty(CRMEngagementID)
| where EngagementEstimatedCompletionDate >= datetime(2021-04-01) // temporarily using last FY Q4 since current FY Q1 hasn't ended yet
| distinct TPID
| where TPID != 0); 
let currentACR = tpids
| join kind=inner vwACR() on TPID
| where ServiceLevel1 == "Compute"
| where ServiceInfluencer == "NATIVE WVD"
| where FiscalMonth == endOfQuarterMonth
| summarize ACR=sum(AzureConsumedRevenue) by TPID
| join kind=inner vwTPAccountAzDash() on TPID
| project TPID, TopParent, FiscalQuarter=quarter, EndOfQuarterMonth=endOfQuarterMonth, ACR;
let prevACR = tpids
| join kind=inner vwACR() on TPID
| where ServiceLevel1 == "Compute"
| where ServiceInfluencer == "NATIVE WVD"
| where FiscalMonth == endOfPrevQuarterMonth
| summarize ACR=sum(AzureConsumedRevenue) by TPID
| join kind=inner vwTPAccountAzDash() on TPID
| project TPID, TopParent, EndOfPreviousQuarterMonth=endOfPrevQuarterMonth, PreviousACR=ACR;
currentACR
| join kind=leftouter prevACR on TPID
| project TPID, TopParent, FiscalQuarter, EndOfQuarterMonth, ACR,
    EndOfPreviousQuarterMonth=endOfPrevQuarterMonth, PreviousACR=iif(isnull(PreviousACR), decimal(0), PreviousACR)
| extend
    ACRCap=case(
        ACR < 1000, 0,
        ACR >= 20000, 20000,
        tolong(ACR)
    )
| extend
    Gap=ACRCap - PreviousACR
| extend
    NetACR=iif(Gap < 0, decimal(0), Gap),
    ConvertionRatio=1.25
| extend
    ArtificialRev=NetACR * ConvertionRatio * 12
```



## T3-W365 Opportunity

```
vwFactOpportunityPipeline()
| where OpportunityCloseDate >= datetime(2021-07-01) or OpportunityDueDate >= datetime(2021-07-01)
| where RevSumCategory == "Cloud PC"
| distinct CRMOpportunityID, TPID, TopParent, MSXStatus, OpportunityDueDate, OpportunityCloseDate
| extend OpportunityCloseDate=case(
    MSXStatus == "Open", datetime(null),
    OpportunityCloseDate
)
| join kind=leftouter (
    Calendar()
    | distinct FiscalQuarter, FiscalDate
) on $left.OpportunityCloseDate == $right.FiscalDate
| join kind=inner vwOpportunityTeam() on $left.CRMOpportunityID == $right.OpportunityCRMID
| join kind=inner Person() on $left.OpportunityTeamAlias == $right.Alias
| join kind=inner (
    cluster('1es.kusto.windows.net').database('HeadTrax').Person
    | where CurrentlyEmployed
    | project Alias=toupper(Alias), DisplayName
) on Alias
| project CRMOpportunityID, TPID, TopParent, MSXStatus, OpportunityDueDate, OpportunityCloseDate, ClosedQuarter=FiscalQuarter, ClosedMonth=startofmonth(OpportunityCloseDate),
    DisplayName, OpportunityTeamAlias, StandardTitle=trim_end("\\d", StandardTitle), Qualifier1, Qualifier2
| extend
    QualifiedRole=case(
        StandardTitle in ("Solution Area Specialists IC", "Cloud Solution Architecture IC") and Qualifier1 == "N/A" and Qualifier2 == "Azure-Infrastructure", "Yes",
        StandardTitle == "Cloud Solution Architecture OPEX IC" and Qualifier2 == "Azure-Infrastructure", "Yes",
        "No"
    )
```





## T4-TP CloudPC Seats FY22

```
vwFactOpportunityPipeline()
| where OpportunityCloseDate >= datetime(2021-07-01) or OpportunityDueDate >= datetime(2021-07-01)
| where RevSumCategory == "Cloud PC"
| distinct TPID, TopParent
| join kind=inner vwBilledRevenue() on TPID
| where RevSumCategory == "Cloud PC"
| where FiscalMonth between (datetime(2021-07-01) .. startofmonth(now()))
| summarize InitiatedUnits=sum(InitiatedUnits) by TPID, TopParent, FiscalMonth,TransactionType
| join kind=inner (
    Calendar()
    | distinct FiscalQuarter, FiscalMonth
) on FiscalMonth
| project TPID, TopParent, FiscalQuarter, FiscalMonth,TransactionType, InitiatedUnits,
    InitiatedUnitsCap=case(
        InitiatedUnits < 50, todecimal(0),
        InitiatedUnits >= 1500, todecimal(1500),
        InitiatedUnits
    ),
    RateCard=24
| extend
    ArtificialACRW365=InitiatedUnits * RateCard
```





```
Hi Wenbo,

I have an item that I’d like you to work on. Here’s some context and requirements:

As you already know, Opportunities and Engagements have Owners (which usually are sellers), and sellers have TP assignments. Recently we discovered that there are cases where a seller owns an Opportunity/Engagement but he/she is not assigned to the TP that these Opportunities/Engagements associated with. So we’d like to identify such cases and report below data:

For opportunities:
CRM Opportunity ID	Status	Due Date	TPID	Business Type	Owner Alias	Owner Standard Title	Owner Qualifier 1	Owner Qualifier 2
								

For engagements:
CRM Engagement ID	Status	Estimated Completion Date	TPID	Owner Alias	Owner Standard Title	Owner Qualifier 1	Owner Qualifier 2
							

The tables/functions you will be leveraging are:
vwOpportunity()
vwEngagement()
vwAccountSellerAssignment()
Person()

Please write queries first and I’ll help review them. Once we finalize these queries we’ll create functions in MWSWorkArea database to make it easy to use.

Feel free to let me know if you have any questions.

Thanks,
-Raien

```



## Table title

| CRMEngagementID    | TPID | EngagementStatus | ClosedQuarter | ClosedMonth | Alias       | DisplayName | StandardTitle        | Qualifier1        | Qualifier2        | QualifiedRole |                           |
| ------------------ | ---- | ---------------- | ------------- | ----------- | ----------- | ----------- | -------------------- | ----------------- | ----------------- | ------------- | ------------------------- |
| CRM  Engagement ID | TPID | Status           |               |             | Owner Alias |             | Owner Standard Title | Owner Qualifier 1 | Owner Qualifier 2 |               | Estimated Completion Date |





| CRMOpportunityID    | TPID | TopParent | MSXStatus | OpportunityDueDate | OpportunityCloseDate | ClosedQuarter | ClosedMonth | DisplayName | OpportunityTeamAlias | StandardTitle        | Qualifier1        | Qualifier2        | QualifiedRole |          |
| ------------------- | ---- | --------- | --------- | ------------------ | -------------------- | ------------- | ----------- | ----------- | -------------------- | -------------------- | ----------------- | ----------------- | ------------- | -------- |
| CRM  Opportunity ID | TPID |           | Status    | Business Type      |                      |               |             |             | Owner Alias          | Owner Standard Title | Owner Qualifier 1 | Owner Qualifier 2 |               | Due Date |