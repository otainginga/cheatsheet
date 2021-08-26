

# 00 Basic Info 



## 00.00 Work Contents

Data Analyst, MW&S(Modern Work & Security)



## 00.01 view function

vw_function:  The latest data [Actually Today left outer Yesterday ] 



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

Extract data ffrom other database/cloud:

```
cluster("Cluster2").database("SomeDB").Table2
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

## 00.05 Data Type

Kusto 不支持用户定义的数据类型。所有非字符串数据类型都包括一个特殊的“null”值，它表示缺少数据或数据不匹配

| 类型      | bool | datetime | dynamic                             | guid | int  | long | real | string | timespan | decimal |
| --------- | ---- | -------- | ----------------------------------- | ---- | ---- | ---- | ---- | ------ | -------- | ------- |
| gettype() | int8 | datetime | array 或 dictionary，或者任何其他值 | guid | int  | long | real | string | timespan | decimal |



## 00.06 Special Function

### 00.06.01 Toscalar()

### 00.06.02 materialize()



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

| Issue                         | Sample 1                                                     | Sample 2                                                     | Note            |
| ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------- |
| ETLDate                       | 00:00.0                                                      | 00:00.0                                                      |                 |
| AreaShortName                 | Germany                                                      | GCR                                                          |                 |
| Subsidiary                    | Germany                                                      | China                                                        |                 |
| TopParent                     | Unknown                                                      | Unknown                                                      |                 |
| TPID                          | 0                                                            | 0                                                            |                 |
| CRMOpportunityID              | 7-UP3O4JIWH                                                  | 7-SUOMKBJYZ                                                  |                 |
| OpportunityName               | RCD - IB Wallner - Microsoft 365 â€“ Kontaktaufnahme - alboscai | Signed up For Office 365 Enterprise  E3 Trial                |                 |
| OpportunityCreatedDate        | 00:00.0                                                      | 00:00.0                                                      |                 |
| OpportunityDueDate            | 00:00.0                                                      | 00:00.0                                                      |                 |
| OpportunityCloseDate          | 00:00.0                                                      | 0001-01-01 00:00:00.0000000                                  |                 |
| MSXStatus                     | Lost                                                         | Open                                                         | {lost/win/open} |
| CRMHyperlink                  | https://microsoftsales.crm.dynamics.com/main.aspx?appid=fe0c3504-3700-e911-a849-000d3a10b7cc&pagetype=entityrecord&etn=opportunity&id=5ebae664-c806-ea11-a811-000d3a8ccf2f | https://microsoftsales.crm.dynamics.com/main.aspx?appid=fe0c3504-3700-e911-a849-000d3a10b7cc&pagetype=entityrecord&etn=opportunity&id=1461d329-4250-e911-a850-000d3a10b05d |                 |
| SalesUnit                     | Not Mapped                                                   | Not Mapped                                                   |                 |
| DaysInSalesStage              | 649                                                          | 882                                                          |                 |
| SalesStage                    | Prove Value 60%                                              | Develop Strategy 20%                                         |                 |
| ForecastRecommendation        | Committed At Risk                                            | Uncommitted                                                  |                 |
| ForecastComments              | AB - 15/Oct - 15.Oct.2020     No answer     checked Tennant     no action     AB - 17/Sep - 05. MAR     - NA          C - 8 JAN          PARTNERS:     ma-edv     Infraserv     (3rd customer partner)          14.11     - buying SQL server     - sending contact     - sending tech offers     - x9 ProPlus     - xCall Licenses     - told him about diff office plans |                                                              |                 |
| SummarySegment                | Small, Medium & Corporate Commercial                         | Small, Medium & Corporate  Commercial                        |                 |
| Segment                       | Small, Medium & Corporate Commercial                         | Small, Medium & Corporate  Commercial                        |                 |
| SubSegment                    | SM&C Commercial - SMB                                        | SM&C Commercial - SMB                                        |                 |
| CRMAccount                    | IB Wallner                                                   | ç‘žå£«å†ä¿é™©è‚¡ä»½æœ‰é™å…¬å¸åŒ—äº¬åˆ†å…¬å¸                  |                 |
| ETLDate1                      | 00:00.0                                                      | 00:00.0                                                      |                 |
| FieldPrimarySegment           | Commercial                                                   | Commercial                                                   |                 |
| FieldSuperSummarySegment      | SM&C                                                         | SM&C                                                         |                 |
| FieldSegment                  | SM&C SMB                                                     | SM&C SMB                                                     |                 |
| FieldSubSegment               | SM&C Commercial - SMB                                        | SM&C Commercial - SMB                                        |                 |
| OpportunityDesc               | OfficeProPLus. Windows, Server                               | 365                                                          |                 |
| CreatedBy                     | alboscai                                                     | a64d5bc2f0d241bcac649426f3ac6c3fv-wenmya                     |                 |
| CreatedByGroup                | SMC                                                          |                                                              |                 |
| CreatedByStandardTitleGroup   |                                                              | Other                                                        |                 |
| OpportunityOwner              | ALBOSCAI                                                     | ZHAYING                                                      |                 |
| OpportunityOwnerStandardTitle | Digital Sales Representative                                 | Digital Sales Representative                                 |                 |
| OwnershipGroup                | SMC                                                          | SMC                                                          |                 |
| PendingCloseStatus            |                                                              |                                                              |                 |
| PrimaryPartnerName            | Infraserv GmbH & Co. Gendorf KG (Germany)                    | Microsoft Corporation                                        |                 |
| PrimaryCompetitor             |                                                              |                                                              |                 |
| CompeteThreatLevel            | UNKNOWN                                                      | UNKNOWN                                                      |                 |
| LicensingProgram              | Cloud Solution Provider                                      | Online                                                       |                 |
| LicensingSubType              | Cloud Solution Provider New                                  | Existing - New Contract                                      |                 |
| WorkloadType                  | UNKNOWN                                                      | UNKNOWN                                                      |                 |
| WorkloadCapability            | UNKNOWN                                                      | UNKNOWN                                                      |                 |
| WorkloadDetail                | UNKNOWN                                                      | UNKNOWN                                                      |                 |
| ClosePlan                     | UNKNOWN                                                      | UNKNOWN                                                      |                 |
| GBBAttachFlag                 | No                                                           | No                                                           |                 |
| PrimarySolutionArea           | Modern Work                                                  | Modern Work                                                  |                 |
| OtherCompetitor               |                                                              |                                                              |                 |
| SalesPlayChecklist            | UNKNOWN                                                      | UNKNOWN                                                      |                 |
| DetailPricingLevel            | Cloud Solution Provider New                                  | MOSP Baseline Renewal Recurring                              |                 |
| SummaryPricingLevel           | Cloud Solution Provider                                      | Online Services Baseline                                     |                 |
| ReportingPricingLevel         | Cloud Solution Provider                                      | Online Services                                              |                 |
| ReportingSummaryPricingLevel  | Cloud Solution Provider                                      | Online Services                                              |                 |
| BusinessType                  | New                                                          | New                                                          |                 |
| QualifiedPipelineFlag         | No                                                           | No                                                           |                 |
| PriceListType                 | Product Family                                               | Product Family                                               |                 |
| OpportunityModifiedDate       | 00:00.0                                                      | 00:00.0                                                      |                 |
| ForecastCommentsModifiedBy    | alboscai                                                     |                                                              |                 |
| ForecastCommentsModifiedDate  | 00:00.0                                                      | 00:00.0                                                      |                 |
| CRMAccount1                   | IB Wallner                                                   | ç‘žå£«å†ä¿é™©è‚¡ä»½æœ‰é™å…¬å¸åŒ—äº¬åˆ†å…¬å¸                  |                 |



# 02 Engagements

This is a chart for products which has a property of  **subscription**, which means, if we have a stable relationship with a company (got subscription each month), we will call it a good engagement. 

| Issue                             | Sample 1                                                     | Sample 2                                                     | Note |
| --------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| AreaShortName                     | Canada                                                       | US                                                           |      |
| Subsidiary                        | Canada                                                       | United States                                                |      |
| TopParent                         | FARM CREDIT CANADA                                           | MONTGOMERY COLLABORATION                                     |      |
| TPID                              | 9030813                                                      | 9004507                                                      |      |
| CRMEngagementID                   | 7-UNMWBOO6D                                                  | 7-UXYVQ2RBQ                                                  |      |
| EngagementName                    | FCC: Azure Sentinel                                          | Azure Sentinel Project at Montgomery  Collaboration          |      |
| EngagementStatus                  | In Progress                                                  | Closed-Canceled                                              |      |
| EngagementHealth                  | Yellow                                                       | Green                                                        |      |
| EngagementStage                   | 2-VALIDATE                                                   | 3-COMMIT                                                     |      |
| EngagementEstimatedStartDate      | 00:00.0                                                      | 00:00.0                                                      |      |
| EngagementEstimatedCompletionDate | 00:00.0                                                      | 00:00.0                                                      |      |
| EngagementCreatedBy               | ankmishr                                                     | v-mdzia                                                      |      |
| EngagementCreatedByStandardTitle  | Specialist                                                   | Sales Manager                                                |      |
| EngagementOwner                   | mibonnea                                                     | mbakert                                                      |      |
| EngagementOwnerStandardTitle      | Specialist                                                   | Sales Manager                                                |      |
| EngagementOwnershipGroup          | STU                                                          | ATU                                                          |      |
| EngagementURL                     | https://microsoftsales.crm.dynamics.com/main.aspx?appid=fe0c3504-3700-e911-a849-000d3a10b7cc&pagetype=entityrecord&etn=msp_engagement&id=cfe4d8c8-3702-ea11-a863-000d3a10a7ae | https://microsoftsales.crm.dynamics.com/main.aspx?appid=fe0c3504-3700-e911-a849-000d3a10b7cc&pagetype=entityrecord&etn=msp_engagement&id=99f71f56-a322-ea11-a811-000d3a8cc796 |      |
| EngagementAgeGroup                | 120+                                                         | 30 to 60                                                     |      |
| EngagementPrimaryCompetitor       | AWS \| Other                                                 | None                                                         |      |
| CompetitorThreatLevel             | High                                                         | Low/ None                                                    |      |
| MSServicesInvolved                | Microsoft Support                                            |                                                              |      |
| SalesUnit                         | Canada.Corp                                                  | USA - Education                                              |      |
| GBBAttachFlag                     | No                                                           | No                                                           |      |
| ETLDate                           | 00:00.0                                                      | 00:00.0                                                      |      |
| EngagementCreatedDate             | 00:00.0                                                      | 00:00.0                                                      |      |
| EngagementPartnerName             | Microsoft Services                                           | UNKNOWN                                                      |      |
| EngagementPartnerAttachFlag       | Yes                                                          | No                                                           |      |
| EngagementSalesPlayChecklist      | Other                                                        | Other                                                        |      |
| EngagementStageStartedOn          | 00:00.0                                                      | 00:00.0                                                      |      |
| MCSAttachFlag                     | Yes                                                          | No                                                           |      |
| MCSEngagedFlag                    | No                                                           | No                                                           |      |

# 03 Other Functions



## 03.01 HeadTrax

```
 cluster('1es.kusto.windows.net').database('HeadTrax').Person
    | where CurrentlyEmployed
    | project Alias=toupper(Alias), DisplayName
```

## 03.02 vwOpportunityPipeline()

1. **ID, Fiscal_Time, Product_category,[ Pipeline**: 预测收入/未来收入]
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



# 04 Other charts

##### 04.01 Engagement Milestone

| Issue                                 |                                     | Sample 2                            | Note |
| ------------------------------------- | ----------------------------------- | ----------------------------------- | ---- |
| CRMEngagementID                       | 7-WBQKLZMZX                         | 7-W457C6KF5                         |      |
| CRMEngagementMilestoneID              | 7-WBQL7S2P7                         | 7-W457PDWJ3                         |      |
| EngagementMilestoneName               | Windows Virtual Desktop POC         | PoC                                 |      |
| EngagementMilestoneOwner              | JOHNWILS                            | CMANSKE                             |      |
| EngagementMilestoneOwnerStandardTitle | Specialist                          | Specialist                          |      |
| EngagementMilestoneCategory           | Pre-Commit: POC/Pilot/MVP           | Pre-Commit: POC/Pilot/MVP           |      |
| EngagementMilestoneWorkload           | Infra: Native Azure Virtual Desktop | Infra: Native Azure Virtual Desktop |      |
| EngagementMilestoneWorkloadType       | Azure                               | Azure                               |      |
| EngagementMilestoneEstimatedDate      | 00:00.0                             | 00:00.0                             |      |
| EngagementMilestoneStatus             | On Track                            | On Track                            |      |
| EngagementMilestoneStatusReasonField  | UNKNOWN                             | UNKNOWN                             |      |
| EngagementMilestoneNonRecurring       | No                                  | No                                  |      |
| EngagementMilestoneHelpNeeded         | UNKNOWN                             | UNKNOWN                             |      |
| ETLDate                               | 00:00.0                             | 00:00.0                             |      |
| EngagementMilestoneCreatedBy          | JOHNWILS                            | CMANSKE                             |      |
| EngagementMilestoneCreateDate         | 00:00.0                             | 00:00.0                             |      |
| EngagementMilestoneOwnershipGroup     | STU                                 | STU                                 |      |

## MAL

##### MAL.01 TPAccount & Dash

TPAccount: Important Customers

TPAccountDash: TPAccount & nonimportant customers

|                         | Sample 1                          | Sample 2                       | Note |
| ----------------------- | --------------------------------- | ------------------------------ | ---- |
| TPID                    | 642093                            | 642902                         |      |
| TopParent               | UT-STATE GOVERNMENT               | OFFICE OF PERSONNEL MANAGEMENT |      |
| TranslatedAccount       | UT-STATE GOVERNMENT               | OFFICE OF PERSONNEL MANAGEMENT |      |
| HQDSFlag                | UNKNOWN                           | UNKNOWN                        |      |
| AreaShortName           | US                                | US                             |      |
| Subsidiary              | United States                     | United States                  |      |
| Industry                | Critical Infrastructure           | Critical Infrastructure        |      |
| SummarySegment          | Enterprise Public Sector          | Enterprise Public Sector       |      |
| Segment                 | Major Public Sector               | Major Public Sector            |      |
| SubSegment              | Major - State & Local Governments | Major - Federal Government     |      |
| FieldSummarySegment     | Enterprise Public Sector          | Enterprise Public Sector       |      |
| FieldSegment            | Major Public Sector               | Major Public Sector            |      |
| FieldSubSegment         | Major - State & Local Governments | Major - Federal Government     |      |
| MALFlag                 | Yes                               | Yes                            |      |
| ETLDate                 | 00:00.0                           | 00:00.0                        |      |
| SalesTerritory          | SLG.004.WST.004                   | FED.076.FCG.302                |      |
| SalesUnit               | USA - SLG                         | USA - Federal                  |      |
| ATS                     | JOBEAIRD                          | UNDEFINED                      |      |
| ATU                     | SLG.004.WST                       | FED.076.FCG                    |      |
| ATUGroup                | USA - SLG - Enterprise West       | USA - FED - Civilian           |      |
| ATUManager              | MELISSAL                          | MEMCGRIF                       |      |
| AccountPositionCM01     | 1C                                | 1A                             |      |
| AccountPositionJuneCY01 | 1B                                | 1B                             |      |
| MALAM                   | DAVST                             | GRGEHRIN                       |      |
| VerticalCategory        | Government Administration         | Government Administration      |      |
| Vertical                | Public Administration             | Public Administration          |      |
| GlobalSector            | Government                        | Government                     |      |





# 77 Kusto Usage



Each filter prefixed by the pipe character `|` is an instance of an *operator*, with some parameters. The input to the operator is the table that is the result of the preceding pipeline. In most cases, any parameters are [scalar expressions](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/scalar-data-types/) over the columns of the input. In a few cases, the parameters are the names of input columns, and in a few cases, the parameter is a second table. The result of a query is always a table, even if it only has one column and one row.

`T` is used in query to denote the preceding pipeline or source table.

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

<img src="https://miro.medium.com/max/1400/1*UAPgZRnhFG29C0nDFi5D0A.png" alt="What is the difference between an inner and an outer join in SQL? | by Kate  Marie Lewis | Towards Data Science" style="zoom:33%;" />

## 77.02 Querys

[查询最佳做法 - Azure 数据资源管理器 | Microsoft Docs](https://docs.microsoft.com/zh-cn/azure/data-explorer/kusto/query/best-practices)







# Assignment 1

## Issue 1

### I1.01 Engagement





| Engament  ID                                                 | Status                          | Close Quarter                   | Close Month                     | Engagement team  Full name                                   | Engagement team  Full Alias     | Engagement team  Standard Title | Engagement team  qualifier 2    | Qualified Role                                               | TPID                                                         | Top Parent Name               | Fiscal Quarter                | End of Quarter  Month                            | End of Quarter  Month ACR                                    | End of Quarter  Month ACR Cap                                | End of Previous  Quarter Month                               | End of Previous  Quarter Month ACR                           | Net ACR                                                      | Convertion Ratio            | Artificial Rev$  AVD                                     |
| ------------------------------------------------------------ | ------------------------------- | ------------------------------- | ------------------------------- | ------------------------------------------------------------ | ------------------------------- | ------------------------------- | ------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ----------------------------- | ----------------------------- | ------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------- | -------------------------------------------------------- |
| 7-ABCDF                                                      | closed-completed                | FY-Q1                           | August                          | John Smith                                                   | JSM                             | Solution Area Specialists IC    | Modern Work                     | Yes                                                          | 10001                                                        | ABC                           | FY22-Q1                       | September                                        | $5,000                                                       | $5,000                                                       | June                                                         | $1,000                                                       | $4,000                                                       | 1.25                        | $                           60,000                       |
| 7-ABCDF                                                      | closed-completed                | FY-Q1                           | August                          | Adam Johnson                                                 | AJO                             | Solution Area Specialists IC    | Security                        | Yes                                                          | 10001                                                        | ABC                           | FY22-Q1                       | September                                        | $5,000                                                       | $5,000                                                       | June                                                         | $1,000                                                       | $4,000                                                       | 1.25                        | $                           60,000                       |
| 7-ABCDF                                                      | closed-completed                | FY-Q1                           | August                          | Mark Taylor                                                  | MTA                             | Solution Area Specialists IC    | Azure-Infrastructure            | No                                                           | 10001                                                        | ABC                           | FY22-Q1                       | September                                        | $5,000                                                       | $5,000                                                       | June                                                         | $1,000                                                       | $4,000                                                       | 1.25                        | $                           60,000                       |
|                                                              |                                 |                                 |                                 |                                                              |                                 |                                 |                                 |                                                              |                                                              |                               |                               | December                                         | $5,000                                                       | $5,000                                                       | September                                                    | $5,000                                                       | $0                                                           | 1.25                        | $                                -                       |
|                                                              |                                 |                                 |                                 |                                                              |                                 |                                 |                                 |                                                              |                                                              |                               |                               |                                                  |                                                              |                                                              |                                                              |                                                              |                                                              |                             |                                                          |
| **pull  from data (engagement)**                             | **pull from data (engagement)** | **pull from data (engagement)** | **pull from data (engagement)** | **pull from data (engagement)**                              | **pull from data (engagement)** | **pull from data (engagement)** | **pull from data (engagement)** | **Calculated**                                               | **pull from data (TPID ACR)**                                | **pull from data (TPID ACR)** | **pull from data (TPID ACR)** | **pull from data (TPID ACR)**                    | **pull from data (TPID ACR)**                                | **Calculated**                                               | **pull from data (TPID ACR)**                                | **pull from data (TPID ACR)**                                | **Calculated**                                               | **Fixed Value**             | **Calculated**                                           |
| Pull Engagements from workload Infra:  Windows Virtual Desktop (WVD) |                                 |                                 |                                 | Each one in the engagement team  shows up in a row. Other information repeats (ex engegement ID, Status, etc) |                                 |                                 |                                 | Only modern  work sellers are eligible to retire artifical Rev$  AVD. Outcome: Yes/No Flag | Customers information from the  TPID associated to the Engagement |                               |                               | Shows End of quarter month (Sep,  Dec, Mar, Jun) | Shows ACR from respective TPID  and Month. Consider      SL1 = Compute     Service Influencer = NATIVE WVD | if "end of quarter month  ACR" <1000, then ACR cap = 0     if "end of quarter month ACR" >=20,000, then ACR cap =  20,000     if "end of quarter month ACR" >= 1000 and <20000, then ACR  cap = "end of quarter month ACR" | Shows the end of previous  quarter month.     Ex: If End of quarter is FY22-September the end of previous quarter is  FY-21 July | Shows ACR from respective TPID  and Month of previous end of quarter. Consider      SL1 = Compute     Service Influencer = NATIVE WVD | Month ACR cap - End of previous  quarter Month ACR     (column Q minus column S). If NET ACR <0, then NET ACR=0 | Fixed value provided = 1.25 | =[Net ACR]*[Convertion  Ratio]*12,      if < 0, then = 0 |



|                                      | Role summary                 | Qualifier 1    | Qualifier 2 |
| ------------------------------------ | ---------------------------- | -------------- | ----------- |
| Retiring Windows 365 quota with  AVD | Solution Area Specialists IC | NA             | Modern Work |
| Solution  Area Specialists IC        | NA                           | Security       |             |
| Solution  Area Specialists IC        | NA                           | MW-Surface     |             |
| Technology  Specialists IC           | NA                           | Cloud Endpoint |             |





### I2.02 Opportunity

## 

| CRM  opportunity ID                              | Status                           | Close Quarter                    | Close Month                      | Opportunity team  Full name                                  | Opportunity team  Full Alias     | Opportunity  team Standard Title | Opportunity team  qualifier 2    | Qualified Role                                               | TPID                                                         | Top Parent Name                 | Fiscal Quarter                  | Fiscal Month                    | Iniated Units                                                | Iniated Units  Cap                                           | Rate Card                  | Artificial ACR  W365                                    |
| ------------------------------------------------ | -------------------------------- | -------------------------------- | -------------------------------- | ------------------------------------------------------------ | -------------------------------- | -------------------------------- | -------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------- | ------------------------------- | ------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------- | ------------------------------------------------------- |
| 7-ABCDF                                          | won                              | FY-Q1                            | August                           | John Smith                                                   | JSM                              | Solution Area Specialists IC     | Modern Work                      | No                                                           | 10001                                                        | ABC                             | FY22-Q1                         | August                          | 1,500                                                        | 1,500                                                        | $          24.00           | $           36,000                                      |
| 7-ABCDF                                          | won                              | FY-Q1                            | August                           | Adam Johnson                                                 | AJO                              | Solution Area Specialists IC     | Security                         | No                                                           | 10001                                                        | ABC                             | FY22-Q1                         | August                          | 1,500                                                        | 1,500                                                        | $          24.00           | $           36,000                                      |
| 7-ABCDF                                          | won                              | FY-Q1                            | August                           | Mark Taylor                                                  | MTA                              | Solution Area Specialists IC     | Azure-Infrastructure             | Yes                                                          | 10001                                                        | ABC                             | FY22-Q1                         | August                          | 1,500                                                        | 1,500                                                        | $          24.00           | $           36,000                                      |
|                                                  |                                  |                                  |                                  |                                                              |                                  |                                  |                                  |                                                              |                                                              |                                 |                                 |                                 |                                                              |                                                              |                            |                                                         |
|                                                  |                                  |                                  |                                  |                                                              |                                  |                                  |                                  |                                                              |                                                              |                                 |                                 |                                 |                                                              |                                                              |                            |                                                         |
|                                                  |                                  |                                  |                                  |                                                              |                                  |                                  |                                  |                                                              |                                                              |                                 |                                 |                                 |                                                              |                                                              |                            |                                                         |
| **pull  from data (opportunity)**                | **pull from data (opportunity)** | **pull from data (opportunity)** | **pull from data (opportunity)** | **pull from data (opportunity)**                             | **pull from data (opportunity)** | **pull from data (opportunity)** | **pull from data (opportunity)** | **Calculated**                                               | **pull from data (TPID Seats)**                              | **pull from data (TPID Seats)** | **pull from data (TPID Seats)** | **pull from data (TPID Seats)** | **pull from data (TPID Seats)**                              | **Calculated**                                               | **Fixed Value**            | **Calculated**                                          |
| Pull  opportunity with Rev Sum Category=Cloud PC |                                  |                                  |                                  | Each one in the opportunity team  shows up in a row. Other information repeats (ex opportunity ID, Status, etc) |                                  |                                  |                                  | Only AZURE  sellers are eligible to retire artifical W365 ACR.  Outcome: Yes/No Flag | Customers information from the  TPID associated to the Opportunity |                                 |                                 |                                 | Shows Initiate units (seats) for  the TPID associated to the opportunity     Rev Sum Category = Cloud PC | if "intiate units"  <50, then units cap = 0     if "intiate units" >=1500 , then units cap = 1500     if "intiate units" >= 50 and <1500, then units cap =  "intiate units" | Fixed value provided = $24 | =[initiated units] * [Rate  Card],      if <0, then = 0 |



|                                      | Role summary                 | Qualifier 1          | Qualifier 2          |
| ------------------------------------ | ---------------------------- | -------------------- | -------------------- |
| Retiring AVD quota with Windows  365 | Solution Area Specialists IC | NA                   | Azure-Infrastructure |
| Cloud  Solution Architecture IC      | NA                           | Azure-Infrastructure |                      |
| Cloud  Solution Architecture OPEX IC |                              | Azure-Infrastructure |                      |







## Solution [I1]

#### T1-AVD Engagement





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

#### T2-TP ACR FY21Q4

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



## Solution [I2]





#### T3-W365 Opportunity

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





#### T4-TP CloudPC Seats FY22

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



## Sample Solution title

| CRMEngagementID    | TPID | EngagementStatus | ClosedQuarter | ClosedMonth | Alias       | DisplayName | StandardTitle        | Qualifier1        | Qualifier2        | QualifiedRole |                           |
| ------------------ | ---- | ---------------- | ------------- | ----------- | ----------- | ----------- | -------------------- | ----------------- | ----------------- | ------------- | ------------------------- |
| CRM  Engagement ID | TPID | Status           |               |             | Owner Alias |             | Owner Standard Title | Owner Qualifier 1 | Owner Qualifier 2 |               | Estimated Completion Date |





| CRMOpportunityID    | TPID | TopParent | MSXStatus | OpportunityDueDate | OpportunityCloseDate | ClosedQuarter | ClosedMonth | DisplayName | OpportunityTeamAlias | StandardTitle        | Qualifier1        | Qualifier2        | QualifiedRole |          |
| ------------------- | ---- | --------- | --------- | ------------------ | -------------------- | ------------- | ----------- | ----------- | -------------------- | -------------------- | ----------------- | ----------------- | ------------- | -------- |
| CRM  Opportunity ID | TPID |           | Status    | Business Type      |                      |               |             |             | Owner Alias          | Owner Standard Title | Owner Qualifier 1 | Owner Qualifier 2 |               | Due Date |

# Assignment 2



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



## 

| CRM  Opportunity ID | Status | Due Date | TPID | Business Type | Owner  Alias | Owner Standard Title | Owner  Qualifier 1 | Owner Qualifier 2 |
| ------------------- | ------ | -------- | ---- | ------------- | ------------ | -------------------- | ------------------ | ----------------- |
|                     |        |          |      |               |              |                      |                    |                   |

