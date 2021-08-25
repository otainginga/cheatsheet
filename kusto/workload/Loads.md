

# 00 Basic Info 



## 00.01 view function

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



## 00.02 Product 

O365_core/M365_core(similar product): 3 subproduct for personal, Enterprise[E5] and  Family[E3] : [Office 365 Login | Microsoft Office](https://www.office.com/)

AVD: Azure virtual Desktop

WVD: Windows Virtual Desktop

ACR: *Azure* Committed Revenue



Windows 365[Cloud PC]

## 00.03 Fiscal Year

A New Fiscal Year starts from July, Fiscal year 2021starts from July 1st, 2021.

| Quarter | End Month |
| ------- | --------- |
| Q1      | Sep.      |
| Q2      | Dec.      |
| Q3      | Mar       |
| Q4      | Jun       |

## 00.04 File Management& Information

MWSData: by ETL tools, No manual changes.

MWSWorkData: Create your functions here.

```R
.create-or-alter function with (folder = "Adhoc/v-wenbosun", skipvalidation = "true") GetUnassignedAccoutOpp() {

PUT_YOUR_FUNCTION_HERE
    
}
```



###  OWNED FUNCTIONS[1]

```R
.create-or-alter function with (folder = "Adhoc/v-wenbosun", skipvalidation = "true") GetUnassignedAccoutOpp() {
    
database("MWSData").vwOpportunity()
| where TPID != 0
| where OpportunityOwner != "UNKNOWN"
| join kind=leftanti database("MWSData").vwAccountSellerAssignment() on $left.OpportunityOwner == $right.EmailAlias, TPID
| join kind=inner database("MWSData").Person() on $left.OpportunityOwner == $right.Alias
| project CRMOpportunityID, MSXStatus, OpportunityDueDate, TPID, BusinessType, OpportunityOwner, StandardTitle, Qualifier1, Qualifier2
    
}
```



# 005 Basic info for the table

```R
# show current database
print current_database()

# show the base folder
.show function() 
| project Folder

# show the base folder 2
.show() table() details
| project Folder

```



# 01 Opportunity

vwopportuity: The properties of a opportunity ID.

vwFactOpportunityPipeline()





# 02 Engagement



# 03 Other Charts



## 03.01 HeadTrax

```
 cluster('1es.kusto.windows.net').database('HeadTrax').Person
    | where CurrentlyEmployed
    | project Alias=toupper(Alias), DisplayName
```

## 03.02 vwOpportunityPipeline()

1. **ID, Fiscal_Time, Product_category,[ Pipeline**: 预测收入]
2. Different categories of the product: The product information, if it sells more than 1 product, it will bring out more than 1 records.

```
vwOpportunityPipeline()
| where CRMOpportunityID == "7-SNYJIIW2V"
```

<img src="https://github.com/otainginga/Image/blob/main/vwopportunitypipeline.png?raw=true" alt="vwopportunitypipeline.png" style="zoom:150%;" />

3. The quantity is called "SEATS"

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





## 03.04 vwBilledRevenue()

指真实收入

TPID is not primary key, there's no primary key in billed revenue

|                         | Sample 1                          | sample 2                                | Note               |
| :---------------------- | --------------------------------- | --------------------------------------- | ------------------ |
| CommercialWorkload      | M365 Suites E5                    | M365 Suites E5                          |                    |
| **SuperRevSumDivision** | **Windows E5 - M365**             | **O365 E5 - M365**                      |                    |
| **RevSumDivision**      | **Windows E5 - M365 Sec&Comp**    | **O365 F5 - M365 Sec&Comp**             |                    |
| **RevSumCategory**      | **M365 E5 Info Prot & Gov - Win** | **O365 F5 - M365 Sec&Comp - Sec  FUSL** |                    |
| **ProductFamily**       | **M365 E5 IP & Govern - Win**     | **M365 F5 Security - O365**             |                    |
| TPID                    | 641728                            | 648224                                  |                    |
| TransactionType         | N/A                               | Paid                                    |                    |
| **FiscalYear**          | **FY23**                          | **FY22**                                |                    |
| **FiscalQuarter**       | **FY23-Q4**                       | **FY22-Q2**                             |                    |
| **FiscalMonth**         | **00:00.0**                       | **00:00.0**                             |                    |
| InitiatedUnits          | 385160                            | 329060                                  | 一般不可做SUM 操作 |
| BilledRevenue           | 0                                 | 0                                       |                    |
| ETLDate                 | 00:00.0                           | 00:00.0                                 |                    |
| **Area**                | **United States**                 | **United States**                       |                    |
| **Subsidiary**          | **United States**                 | **United States**                       |                    |
| ActualLicenses          | 0                                 | 0                                       |                    |
| SecurityBilledRevenue   | 0                                 | 0                                       |                    |
| BusinessType            | Recurring                         | Renewal                                 |                    |
| BilledRevenueYoY        |                                   |                                         |                    |
| BilledRevenueYTD        | 400526.3                          | 0                                       |                    |
| BilledRevenueQTD        | 0                                 | 0                                       |                    |
| **SummarySegment**      | **Enterprise Commercial**         | **Enterprise Commercial**               |                    |
| **Segment**             | **Major Commercial**              | **Strategic Commercial**                |                    |
| **SubSegment**          | **Major - Commercial Other**      | **Strategic - Commercial Other**        |                    |



## 03.05 vwBilledBudget()

Target revenue: 某地区某月的希望销售额

billed budget is kinda like target that sales teams set, like goals. It just has several dimensions to break it down such as product, segment, area, etc

| Issue                    |                                          | Note     |
| ------------------------ | ---------------------------------------- | -------- |
| SuperRevSumDivision      | Enterprise Client                        |          |
| RevSumDivision           | Client Management                        |          |
| RevSumCategory           | Ops Mgr CML                              |          |
| FiscalYear               | FY19                                     |          |
| FiscalQuarter            | FY19-Q1                                  |          |
| FiscalMonth              | 00:00.0                                  |          |
| BilledBudget             |                                          |          |
| ETLDate                  | 00:00.0                                  |          |
| ProductFamily            | Sys Ctr Ops Mgr Clt Mgmt Lic             |          |
| Area                     | Australia                                |          |
| Subsidiary               | Australia                                |          |
| SummarySegment           | Small, Medium & Corporate Public  Sector | 客户属性 |
| Segment                  | Small, Medium & Corporate  Education     |          |
| SubSegment               | SM&C Education - SMB                     |          |
| SolutionArea             | Modern Work                              |          |
| CommercialWorkload       | MW On-Prem                               |          |
| BilledRevenue            | 0.1239                                   |          |
| ActualBilledRevenue      | 0.1239                                   |          |
| ScheduledBilledRevenue   |                                          |          |
| QualifiedPipeline        |                                          |          |
| CommittedAndRiskPipeline |                                          |          |
| RunRateProjections       |                                          |          |
| SecurityBilledBudget     |                                          |          |







# 77 Kusto Usage

## 77.01 Join

如果未指定 `kind`，则默认的联接风格为 `innerunique`

[join 运算符 - Azure 数据资源管理器 | Microsoft Docs](https://docs.microsoft.com/zh-cn/azure/data-explorer/kusto/query/joinoperator?pivots=azuredataexplorer)

| 联接风格                                                     | 输出记录                                                     |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `kind=leftanti`,                                             | 返回左侧中在**右侧没有匹配项**的所有记录,仅会返回左侧的列。  |
| `kind=rightanti`, ``                                         | 返回右侧中在左侧没有匹配项的所有记录。仅会返回左侧的列。     |
| `kind=leftantisemi`                                          | 左半联接返回左侧中在右侧具有匹配记录的所有记录。 仅会返回左侧的列。 |
| `kind=rightantisemi`                                         |                                                              |
| `kind` 未指定，`kind=innerunique`                            | **左侧中仅有一行与 `on` 键的每个值匹配**。 输出包含一行，用于此行与右侧行的每一个匹配项。*[默认联接在左侧对联接键执行重复数据删除后（删除重复数据时会保留第一个记录）执行内联]* |
| `kind=leftsemi`                                              | 返回**左侧中在右侧具有匹配项**的所有记录。                   |
| `kind=rightsemi`                                             | 返回右侧中在左侧具有匹配项的所有记录。                       |
| `kind=inner`                                                 | 输出中包含一行，该行对应于<u>左右匹配行</u>的**每种组合。**  |
| `kind=leftouter`（或 `kind=rightouter` 或 `kind=fullouter`） | 包含的**一行对应于左侧和右侧的每一行**，即使没有匹配项。 不匹配的输出单元格包含 null。*[表 X 和 Y 的leftouter Join的结果始终包含左表 (X) 的所有记录，即使联接条件在右表 (Y) 中未找到任何匹配记录]* |
| `fullouter`                                                  | 对于表中缺少匹配行的每个列，结果集都会有 `null` 值           |
| `kind = cross`                                               | Kusto 本身不提供交叉联接风格。 无法用 `kind=cross` 来标记运算符。 |

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