<span style="background-color: tomato;"HINT: THEME: Drake Juejin</span>

# 00 Basic Info 



## 00.00 Work Contents

Data Analyst, MW&S(Modern Work & Security)



1. Define functions to solve actual business problems(find out abnormal records)

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

Relationship between TP and the actual company is not clear yet, we're still exploring how to present it in our Kusto

## 00.02 Product 





O365_core/M365_core(similar product): 3 subproduct for personal, Enterprise[E5] and  Family[E3] : [Office 365 Login | Microsoft Office](https://www.office.com/)

AVD: Azure virtual Desktop

WVD: Windows Virtual Desktop

ACR: *Azure* Committed Revenue



Windows 365[Cloud PC]

### Azure Sentinel

Microsoft Azure Sentinel is a scalable, cloud-native, **security information event management (SIEM)** and **security orchestration automated response (SOAR)** solution.

**XDR**(extended detection and response) collects and automatically correlates data across multiple security layers – email, endpoint, server, cloud workload, and network. This allows for faster detection of threats and improved investigation and response times through security analysis,**Microsoft Defender contains [M365 Defender and Azure Defender]**

### OT/IT

IT: information technology cyber security

IT security: confidentiality-integrity-availability

OT: operational technology the hardware and software dedicated to detecting or causing changes in physical processes through direct monitoring and /or control of physical devices such as valves, pumps, etc.

OT Security: Safty-Availability-Integrity



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

标量运算

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



# 01 *Opportunity

vwopportuity: The properties of a opportunity ID.

vwFactOpportunityPipeline()

| Issue                                                        | Sample 1                                                     | Sample 2                                                     | Note                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ETLDate                                                      | 00:00.0                                                      | 00:00.0                                                      |                                                              |
| AreaShortName                                                | Germany                                                      | GCR                                                          |                                                              |
| Subsidiary                                                   | Germany                                                      | China                                                        |                                                              |
| TopParent                                                    | Unknown                                                      | Unknown                                                      |                                                              |
| <span style="background-color: tomato;">TPID</span>          | 0                                                            | 0                                                            | **缺失值：<br />TPID !=0**                                   |
| <span style="background-color: tomato;">CRMOpportunityID</span> | 7-UP3O4JIWH                                                  | 7-SUOMKBJYZ                                                  |                                                              |
| OpportunityName                                              | RCD - IB Wallner - Microsoft 365 â€“ Kontaktaufnahme - alboscai | Signed up For Office 365 Enterprise  E3 Trial                |                                                              |
| OpportunityCreatedDate                                       | 00:00.0                                                      | 00:00.0                                                      |                                                              |
| OpportunityDueDate                                           | 00:00.0                                                      | 00:00.0                                                      |                                                              |
| OpportunityCloseDate                                         | 00:00.0                                                      | 0001-01-01 00:00:00.0000000                                  |                                                              |
| MSXStatus                                                    | Lost                                                         | Open                                                         | **{lost/win/open}**                                          |
| CRMHyperlink                                                 | [Link 1](https://microsoftsales.crm.dynamics.com/main.aspx?appid=fe0c3504-3700-e911-a849-000d3a10b7cc&pagetype=entityrecord&etn=opportunity&id=5ebae664-c806-ea11-a811-000d3a8ccf2f) | [Link 2](https://microsoftsales.crm.dynamics.com/main.aspx?appid=fe0c3504-3700-e911-a849-000d3a10b7cc&pagetype=entityrecord&etn=opportunity&id=1461d329-4250-e911-a850-000d3a10b05d) |                                                              |
| SalesUnit                                                    | Not Mapped                                                   | Not Mapped                                                   |                                                              |
| DaysInSalesStage                                             | 649                                                          | 882                                                          | How many days has the opportunity been **in its current Sales Stage.** |
| SalesStage                                                   | Prove Value 60%                                              | Develop Strategy 20%                                         |                                                              |
| ForecastRecommendation                                       | Committed At Risk                                            | Uncommitted                                                  |                                                              |
| ForecastComments                                             | AB - 15/Oct - 15.Oct.2020     No answer  <br />   checked Tennant     no action |                                                              | Just something sellers put into CRM to help them track progress, it's not too important |
| SummarySegment                                               | Small, Medium & Corporate Commercial                         | Small, Medium & Corporate  Commercial                        |                                                              |
| Segment                                                      | Small, Medium & Corporate Commercial                         | Small, Medium & Corporate  Commercial                        |                                                              |
| SubSegment                                                   | SM&C Commercial - SMB                                        | SM&C Commercial - SMB                                        |                                                              |
| CRMAccount                                                   | IB Wallner                                                   | ç‘žå£«å†ä¿é™©è‚¡ä»½æœ‰é™å…¬å¸åŒ—äº¬åˆ†å…¬å¸                  | **Same as CRMAccount1**                                      |
| ETLDate1                                                     | 00:00.0                                                      | 00:00.0                                                      |                                                              |
| FieldPrimarySegment                                          | Commercial                                                   | Commercial                                                   |                                                              |
| FieldSuperSummarySegment                                     | SM&C                                                         | SM&C                                                         |                                                              |
| FieldSegment                                                 | SM&C SMB                                                     | SM&C SMB                                                     |                                                              |
| FieldSubSegment                                              | SM&C Commercial - SMB                                        | SM&C Commercial - SMB                                        |                                                              |
| OpportunityDesc                                              | OfficeProPLus. Windows, Server                               | 365                                                          |                                                              |
| CreatedBy                                                    | alboscai                                                     | a64d5bc2f0d241bcac649426<br />f3ac6c3fv-wenmya               |                                                              |
| CreatedByGroup                                               | SMC                                                          |                                                              |                                                              |
| CreatedByStandardTitleGroup                                  |                                                              | Other                                                        |                                                              |
| OpportunityOwner                                             | ALBOSCAI                                                     | ZHAYING                                                      | **缺失值： <br />OpportunityOwner == "UNKNOWN"**             |
| OpportunityOwnerStandardTitle                                | Digital Sales Representative                                 | Digital Sales Representative                                 |                                                              |
| OwnershipGroup                                               | SMC                                                          | SMC                                                          |                                                              |
| PendingCloseStatus                                           |                                                              |                                                              |                                                              |
| PrimaryPartnerName                                           | Infraserv GmbH & Co. Gendorf KG (Germany)                    | Microsoft Corporation                                        |                                                              |
| PrimaryCompetitor                                            |                                                              |                                                              |                                                              |
| CompeteThreatLevel                                           | UNKNOWN                                                      | UNKNOWN                                                      |                                                              |
| LicensingProgram                                             | Cloud Solution Provider                                      | Online                                                       |                                                              |
| LicensingSubType                                             | Cloud Solution Provider New                                  | Existing - New Contract                                      |                                                              |
| WorkloadType                                                 | UNKNOWN                                                      | UNKNOWN                                                      |                                                              |
| WorkloadCapability                                           | UNKNOWN                                                      | UNKNOWN                                                      |                                                              |
| WorkloadDetail                                               | UNKNOWN                                                      | UNKNOWN                                                      |                                                              |
| ClosePlan                                                    | UNKNOWN                                                      | UNKNOWN                                                      |                                                              |
| GBBAttachFlag                                                | No                                                           | No                                                           |                                                              |
| PrimarySolutionArea                                          | Modern Work                                                  | Modern Work                                                  |                                                              |
| OtherCompetitor                                              |                                                              |                                                              |                                                              |
| SalesPlayChecklist                                           | UNKNOWN                                                      | UNKNOWN                                                      |                                                              |
| DetailPricingLevel                                           | Cloud Solution Provider New                                  | MOSP Baseline Renewal Recurring                              |                                                              |
| SummaryPricingLevel                                          | Cloud Solution Provider                                      | Online Services Baseline                                     |                                                              |
| ReportingPricingLevel                                        | Cloud Solution Provider                                      | Online Services                                              |                                                              |
| ReportingSummaryPricingLevel                                 | Cloud Solution Provider                                      | Online Services                                              |                                                              |
| <span style="background-color: tomato;">BusinessType</span>  | New                                                          | New                                                          | **{New\|Unknown\|<br />Renewal\|Recurring}**                 |
| QualifiedPipelineFlag                                        | No                                                           | No                                                           |                                                              |
| PriceListType                                                | Product Family                                               | Product Family                                               |                                                              |
| OpportunityModifiedDate                                      | 00:00.0                                                      | 00:00.0                                                      |                                                              |
| ForecastCommentsModifiedBy                                   | alboscai                                                     |                                                              |                                                              |
| ForecastCommentsModifiedDate                                 | 00:00.0                                                      | 00:00.0                                                      |                                                              |
| CRMAccount1                                                  | IB Wallner                                                   | ç‘žå£«å†ä¿é™©è‚¡ä»½æœ<br />‰é™å…¬å¸åŒ—äº¬åˆ†å…¬å¸            | **Same as CRMAccount**                                       |



# 02 *Engagements

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



## HUMAN RELATED



#### H.01 HeadTrax

Detailed info about all employers (including current and expired).

```
 cluster('1es.kusto.windows.net').database('HeadTrax').Person
    | where CurrentlyEmployed
    | project Alias=toupper(Alias), DisplayName
```



|                              | Sample 1                                                     | Sample 2                                                     | Note                                 |
| ---------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------ |
| AadTenantId                  | 72f988bf-86f1-41af-91ab-2d7cd011db47                         | 72f988bf-86f1-41af-91ab-2d7cd011db47                         |                                      |
| AadObjectId                  | fb011a43-4c1e-4f37-91d5-97cc91c43646                         | fb00315e-3363-4d0f-97fd-c7f6df8fe972                         |                                      |
| AadAccountEnabled            | 1                                                            | 1                                                            |                                      |
| AadEtlIngestDateTime         | 04:01.0                                                      | 43:10.8                                                      |                                      |
| Alias                        | v-aparveen                                                   | johennes                                                     |                                      |
| BuildingAddressLine1         | South County Business Park                                   |                                                              |                                      |
| BuildingAddressLine2         | Leopardstown                                                 |                                                              |                                      |
| BuildingCampus               | Dublin                                                       |                                                              |                                      |
| BuildingCity                 | Dublin                                                       |                                                              |                                      |
| BuildingCountry              | IE                                                           |                                                              |                                      |
| BuildingLatitude             | 53.26904                                                     |                                                              |                                      |
| BuildingLongitude            | -6.19692                                                     |                                                              |                                      |
| BuildingName                 | NO WORKSPACE                                                 | DUBLIN-PLACE                                                 |                                      |
| BuildingPostalCode           | D18                                                          |                                                              |                                      |
| BuildingRegion               | MSUS                                                         | EMEA                                                         |                                      |
| BuildingSiteCode             | IEDUBTRI                                                     |                                                              |                                      |
| BuildingSort                 | NO WORKSPACE                                                 | DUBLIN-PLACE                                                 |                                      |
| BuildingStateProvince        | D                                                            |                                                              |                                      |
| BuildingTimeZone             | (GMT) Greenwich Mean  Time;Dublin,Edinburgh,London,Lisbon    |                                                              |                                      |
| BusinessPhone                | +353 (1) 7066287                                             |                                                              |                                      |
| CareerStageName              | IC4                                                          |                                                              |                                      |
| CityName                     | Dublin                                                       |                                                              |                                      |
| CompanyCode                  | 1010                                                         | 1062                                                         |                                      |
| CompanyStartDate             | 00:00.0                                                      |                                                              |                                      |
| CostCenterCode               | 10175446                                                     | 10124653                                                     |                                      |
| CSCategoryCode               | 34                                                           | 0                                                            |                                      |
| CUId                         | f05631cf-3701-7de4-8fbc-a433f3e21bae                         | 61d5cb70-5879-7278-9595-21deeb5ec148                         |                                      |
| Department                   | CI Venus AOC                                                 | GCS Corp IE                                                  |                                      |
| DisplayName                  | Amreen Parveen (Cognizant Worldwide Limited)                 | Joe Hennessy                                                 |                                      |
| EmailAddress                 | v-aparveen@microsoft.com                                     | Joe.Hennessy@microsoft.com                                   |                                      |
| EmployeeLevel                | 9                                                            | 9                                                            |                                      |
| FirstMsJvHireDate            | 00:00.0                                                      |                                                              |                                      |
| FirstRegularHireDate         | 00:00.0                                                      |                                                              |                                      |
| FullName                     | Parveen, Amreen                                              | Hennessy, Joseph James                                       |                                      |
| FunctionHierarchyExecCode    | XI29                                                         | H98W                                                         |                                      |
| GivenName                    | Amreen                                                       | Joseph                                                       |                                      |
| JobTitle                     |                                                              | ACCOUNT MGR                                                  |                                      |
| JobTitleName                 | Business Operations & Program Management                     | Advertising Client Success                                   |                                      |
| JobTitleSummary              | Business Programs & Operations                               | Customer Success                                             |                                      |
| LocationAreaCode             | IE                                                           |                                                              |                                      |
| MostRecentHireDate           | 00:00.0                                                      |                                                              |                                      |
| OfficeLocation               | No WorkSpace                                                 | DUBLIN-PLACE/Mobile                                          |                                      |
| OnPremisesSecurityIdentifier | S-1-5-21-2146773085-903363285-719344707-2691929              | S-1-5-21-1721254763-462695806-1538882281-4129313             |                                      |
| OrganizationAliasHierarchy   | /satyan/amyhood/mesmith/mamckinn/<br />delawton/sadesaus/v-ashukla/v-amaddala/v-aparveen | /satyan/rajeshj/mikhailp/rikvdk/lytad/<br />mrichard/helenat/chfields/johennes |                                      |
| OrganizationHierarchy        | /30958/190659/298458/763013/722852/<br />1353318/1445512/1445173/1459811 | /30958/22206/373539/111263/354456/<br />312553/137397/1376701/1393583 |                                      |
| PersonnelNumber              | 1459811                                                      | 1393583                                                      |                                      |
| PositionNumber               | 92043587                                                     | 91338046                                                     |                                      |
| PositionProjectCategory      |                                                              |                                                              |                                      |
| PositionProjectCode          | 0                                                            | 0                                                            |                                      |
| PositionProjectName          |                                                              |                                                              |                                      |
| ServiceAwardDate             | 00:00.0                                                      |                                                              |                                      |
| StaffingAgencyName           | Cognizant Worldwide Limited                                  |                                                              |                                      |
| StaffingResourceCategory     | Vendor                                                       | Regular                                                      |                                      |
| StaffingResourceGroup        | Outsourced Staff                                             | Regular/Expat/Comm Eligible                                  |                                      |
| StandardTitle                | XR - Business Operations & Program Mgmt                      | Advertising Client Success IC4                               | **Position Level should be removed** |
| Surname                      | Parveen                                                      | Hennessy                                                     |                                      |
| TerminationDate              |                                                              |                                                              |                                      |
| EtlFromDateTime              | 00:00.0                                                      | 00:00.0                                                      |                                      |
| EtlLastUpdateDate            | 04:01.0                                                      | 43:10.8                                                      |                                      |
| EtlCurrent                   | 1                                                            | 1                                                            |                                      |
| EtlToDateTime                | ########                                                     | ########                                                     |                                      |
| RemovedFromAad               | 0                                                            | 0                                                            |                                      |
| CurrentlyEmployed            | 1                                                            | 1                                                            | **should be currently employed**     |



#### H.02 vwAccountSellerAssignment()

many to many 

|                                                            | Sample 1                     | Sample 2                     | Note                                                         |
| ---------------------------------------------------------- | ---------------------------- | ---------------------------- | ------------------------------------------------------------ |
| ETLDate                                                    | 00:00.0                      | 00:00.0                      |                                                              |
| <span style="background-color: tomato;">TPID</span>        | 2537830                      | 3850555                      | **<Customers>** Top Parent ID:                               |
| AccountName                                                | NV-SMB K12                   | MARYLAND HEALTH ENTERPRISES  |                                                              |
| <span style="background-color: tomato;">EmailAlias</span>  | NAOSTEND                     | DESLETTE                     | **Microsoft Officers, also OpportunityOwner in <<u>Opportunity</u>>** |
| AssignmentStartDate                                        | 00:00.0                      | 00:00.0                      |                                                              |
| AssignmentEndDate                                          | 59:59.0                      | 59:59.0                      |                                                              |
| ReportingManagerAlias                                      | KAKOLDEN                     | KAKOLDEN                     |                                                              |
| AssignmentStatus                                           | ACTIVE                       | ACTIVE                       |                                                              |
| StandardTitle                                              | Digital Cloud Acquisition IC | Digital Cloud Acquisition IC |                                                              |
| <span style="background-color: #9FE2BF;">Qualifier1</span> | Demand Response              | Demand Response              | **同为Qualifier 字段，一般不用此表中的字段【更新过慢有信息差】<br />**优先选用Person() 中的Qualifier 字段 |
| <span style="background-color: #9FE2BF;">Qualifier2</span> | Cloud                        | Cloud                        |                                                              |
| Role                                                       | Other                        | Other                        |                                                              |
| MCSCoverageModel                                           | Unassigned                   | Unassigned                   |                                                              |
| OrgDesc                                                    | Small, Medium & Corporate    | Small, Medium & Corporate    |                                                              |
| VPN                                                        | CorpHQ-CHS-SMC-DCAI-23925    | CorpHQ-CHS-SMC-DCAI-24078    |                                                              |
| VPNType                                                    | Blueprint                    | Blueprint                    |                                                              |

#### H.03 Person()

|                                                            | Sample 1                       | Sample 2                                 | Note                                                   |
| ---------------------------------------------------------- | ------------------------------ | ---------------------------------------- | ------------------------------------------------------ |
| Alias                                                      | MARCELWE                       | V-ERBRI                                  |                                                        |
| FullName                                                   | Weissinger, Marcel             | Brimmer, Erin M                          |                                                        |
| Org                                                        | Microsoft Consulting           | M&O                                      |                                                        |
| OrgExecSummary                                             | Global Sales and Marketing Ops | Global Sales and Marketing Ops           |                                                        |
| <span style="background-color: #9FE2BF;">Qualifier1</span> | N/A                            | N/A                                      | **同字段出现时， 优先选用Person() 中的Qualifier 字段** |
| <span style="background-color: #9FE2BF;">Qualifier2</span> | N/A                            | N/A                                      |                                                        |
| StandardTitle                                              | Account Delivery Executive     | XR - Business Operations &  Program Mgmt |                                                        |
| ManagerEmail                                               | THOMASTH                       | KEPRANGH                                 |                                                        |
| Area                                                       | Germany                        | United States                            |                                                        |
| Region                                                     | Germany                        | United States                            |                                                        |
| XTUGroup                                                   | CCE                            |                                          |                                                        |
| ETLDate                                                    | 00:00.0                        | 00:00.0                                  |                                                        |

## OPPORTUNITY RELATED



#### O.02 vwOpportunityPipeline()

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

全表

| issue                         |                                                              |                                                              | note           |
| ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------- |
| ETLDate                       | 00:00.0                                                      | 00:00.0                                                      |                |
| SuperRevSumDivision           | Enterprise Mobility Core - M365                              | Office                                                       |                |
| RevSumDivision                | EMS Core - M365 Frontline                                    | Skype for Business                                           |                |
| RevSumCategory                | EMS - M365 F3                                                | SfB Standard CAL                                             |                |
| ProductFamily                 | CnE M365 F3                                                  | Office Live Comm Svr - CAL                                   |                |
| AreaShortName                 | LATAM                                                        | Germany                                                      |                |
| Subsidiary                    | Mexico                                                       | Germany                                                      |                |
| TopParent                     | ADMINISTRACION Y OPERACION PROFESIONAL S.A. de C.V. FLEXI    | Komm.ONE                                                     |                |
| TPID                          | 2137888                                                      | 1075493                                                      |                |
| CRMOpportunityID              | 7-SJ7UH3KV6                                                  | 7-SMLD2E72R                                                  |                |
| OpportunityName               | FY20 Q1 Ren-July 1th - EA Subscription - AdministraciÃ³n y OperaciÃ³n  Profesional S.A de C.V - 53452411 | FY20 TUP Yr 3 \| 4849565 \| exp:  11/30/2019 \| Partner: COMPAREX Deutschland AG |                |
| MSXStatus                     | Won                                                          | Lost                                                         |                |
| CRMHyperlink                  | https://microsoftsales.crm.dynamics.com/main.aspx?appid=fe0c3504-3700-e911-a849-000d3a10b7cc&pagetype=entityrecord&etn=opportunity&id=2a0270ec-bbd1-450d-8952-fe5c2728cb15 | https://microsoftsales.crm.dynamics.com/main.aspx?appid=fe0c3504-3700-e911-a849-000d3a10b7cc&pagetype=entityrecord&etn=opportunity&id=44a0f1bf-ef36-e911-a851-000d3a10b7cc |                |
| PrimaryPartnerName            | SoftwareONE AG (Romania)                                     | Comparex (Germany)                                           |                |
| SalesProgram                  |                                                              |                                                              |                |
| OpportunityTeam               | [           {            "Alias":  "GIMENDEZ",            "StandardTitle":  "UNKNOWN"           }          ] | [           {            "Alias":  "LBRAUN",            "StandardTitle":  "UNKNOWN"           }          ] |                |
| SalesUnit                     | Mexico.Corp                                                  | DE-Enterprise Public Sector                                  |                |
| SalesStage                    | Negotiate Terms 80%                                          | Prove Value 60%                                              |                |
| ForecastRecommendation        | Committed                                                    | Committed At Risk                                            |                |
| FiscalYear                    | FY20                                                         | FY20                                                         |                |
| FiscalQuarter                 | FY20-Q1                                                      | FY20-Q2                                                      |                |
| FiscalMonth                   | 00:00.0                                                      | 00:00.0                                                      | 与Dueddate配套 |
| QualifiedPipeline             | 0                                                            | 0                                                            |                |
| ProductQuantity               | 0                                                            | 0                                                            |                |
| OpportunityCreatedDate        | 00:00.0                                                      | 00:00.0                                                      |                |
| OpportunityDueDate            | 00:00.0                                                      | 00:00.0                                                      |                |
| OpportunityCloseDate          | 00:00.0                                                      | 00:00.0                                                      |                |
| DaysInSalesStage              | 784                                                          | 650                                                          |                |
| ForecastComments              | Agosto 2 - El cliente ya firmo su  contrato digi--           |                                                              |                |
| SummarySegment                | Small, Medium & Corporate Commercial                         | Enterprise Public Sector                                     |                |
| Segment                       | Small, Medium & Corporate Commercial                         | Major Public Sector                                          |                |
| SubSegment                    | SM&C Commercial - Corporate                                  | Major - Government Other                                     |                |
| FieldPrimarySegment           | Commercial                                                   | Commercial                                                   |                |
| FieldSuperSummarySegment      | SM&C                                                         | Enterprise                                                   |                |
| FieldSegment                  | SM&C Corporate                                               | Major Public Sector                                          |                |
| FieldSubSegment               | SM&C Commercial - Corporate                                  | Major - Government Other                                     |                |
| OpportunityDesc               | Please contact your Sales Excellence  Representative for any questions. And following is primary Licensing Partner:  PerifÃ©ricos ElectrÃ³nicos S.A. de C.V. |                                                              |                |
| CreatedBy                     | msxinteg                                                     | d2fd0be99c93415ba1d69425edce3d16v-akhraj                     |                |
| CreatedByGroup                |                                                              |                                                              |                |
| CreatedByStandardTitleGroup   | Other                                                        | Other                                                        |                |
| OpportunityOwner              | UNKNOWN                                                      | UNKNOWN                                                      |                |
| OpportunityOwnerStandardTitle | UNKNOWN                                                      | UNKNOWN                                                      |                |
| OwnershipGroup                | UNKNOWN                                                      | UNKNOWN                                                      |                |
| PendingCloseStatus            | Final Pricing Sent                                           |                                                              |                |
| PrimaryCompetitor             | Amazon Web Services                                          | None                                                         |                |
| CompeteThreatLevel            | Low/ None                                                    | Low/ None                                                    |                |
| LicensingProgram              | EA Subscription                                              | EA                                                           |                |
| LicensingSubType              | EA Subscription Renewal                                      | True Up                                                      |                |
| WorkloadType                  | UNKNOWN                                                      | UNKNOWN                                                      |                |
| WorkloadCapability            | UNKNOWN                                                      | UNKNOWN                                                      |                |
| WorkloadDetail                | UNKNOWN                                                      | UNKNOWN                                                      |                |
| ClosePlan                     | UNKNOWN                                                      | UNKNOWN                                                      |                |
| Pipeline                      | 19026                                                        | 36.8303                                                      | 预测收入       |
| GBBAttachFlag                 | No                                                           | No                                                           |                |
| PrimarySolutionArea           | Modern Work                                                  | Modern Work                                                  |                |
| OtherCompetitor               |                                                              |                                                              |                |
| SalesPlayChecklist            | UNKNOWN                                                      | UNKNOWN                                                      |                |
| DetailPricingLevel            | EA Subscription Renewal NEW                                  | EA SA True Up NEW                                            |                |
| SummaryPricingLevel           | EA Subscription Renew                                        | EA True Up                                                   |                |
| ReportingPricingLevel         | EA Subscription Renewal                                      | EA Add-On                                                    |                |
| ReportingSummaryPricingLevel  | EA Subscription Renewal                                      | EA Add-On                                                    |                |
| BusinessType                  | Renewal                                                      | New                                                          |                |
| QualifiedPipelineFlag         | No                                                           | No                                                           |                |
| PriceListType                 | Product Family                                               | Product Family                                               |                |
| OpportunityModifiedDate       | 00:00.0                                                      | 00:00.0                                                      |                |
| ForecastCommentsModifiedBy    |                                                              |                                                              |                |
| ForecastCommentsModifiedDate  | 00:00.0                                                      | 00:00.0                                                      |                |
| SecurityQualifiedPipeline     | 0                                                            |                                                              |                |
| CRMAccount                    | ADMINISTRACION Y OPERACION PROFESIONAL S.A. de C.V. FLEXI    | Komm.ONE                                                     |                |





## BILL RELATED



#### B.04 vwBilledRevenue()

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



#### B.05 vwBilledBudget()

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

## Time Related

### T.01 Calendar()

| FiscalYear | FiscalQuarter | FiscalMonth | FiscalWeek         | FiscalDate | ETLDate |
| ---------- | ------------- | ----------- | ------------------ | ---------- | ------- |
| FY18       | FY18-Q2       | 2017-10-01  | 2017-10-06（FRI.） | 2017-10-01 | 00:00.0 |
| FY18       | FY18-Q2       | 2017-10-01  | 2017-10-06         | 2017-10-02 | 00:00.0 |
| FY18       | FY18-Q2       | 2017-10-01  | 2017-10-06         | 2017-10-03 | 00:00.0 |
| FY18       | FY18-Q2       | 2017-10-01  | 2017-10-06         | 2017-10-04 | 00:00.0 |
| FY18       | FY18-Q2       | 2017-10-01  | 2017-10-06         | 2017-10-05 | 00:00.0 |
| FY18       | FY18-Q2       | 2017-10-01  | 2017-10-06         | 2017-10-06 | 00:00.0 |
| FY18       | FY18-Q2       | 2017-10-01  | 2017-10-13         | 2017-10-07 | 00:00.0 |
| FY18       | FY18-Q2       | 2017-10-01  | 2017-10-13         | 2017-10-08 | 00:00.0 |



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

| 联接风格                                                     | 输出记录                                                     | Result                                           |
| :----------------------------------------------------------- | :----------------------------------------------------------- | ------------------------------------------------ |
| `kind=leftanti`,                                             | 返回左侧中在**右侧没有匹配项**的所有记录,仅会返回左侧的列。  | **left**                                         |
| `kind=rightanti`, ``                                         | 返回右侧中在左侧没有匹配项的所有记录。仅会返回左侧的列。     | right                                            |
| `kind=leftsemi`                                              | 左半联接返回左侧中**在右侧具有匹配记录的所有记录**。 仅会返回左侧的列。 | **left**                                         |
| `kind=rightsemi`                                             | 返回右侧中在左侧具有匹配项的所有记录。                       | right                                            |
| `kind=innerunique`<br />或`kind` 未指定                      | **左侧中仅有一行与 `on` 键的每个值匹配**。 输出包含一行，用于此行与右侧行的每一个匹配项。*[默认联接在左侧对联接键执行重复数据删除后（删除重复数据时会保留第一个记录）执行内联]* | both<br />**[key\|value<br />\|key 1\|value 1]** |
| `kind=inner`                                                 | 输出中包含一行，该行对应于<u>左右匹配行</u>的**每种组合。**  | both                                             |
| `kind=leftouter`<br />（或 `kind=rightouter` <br />或 `kind=fullouter`） | 包含的**一行对应于左侧和右侧的每一行**，即使没有匹配项。 不匹配的输出单元格包含 null。<br />*[表 X 和 Y 的leftouter Join的结果始终包含左表 (X) 的所有记录，即使联接条件在右表 (Y) 中未找到任何匹配记录]* | both                                             |
| `fullouter`                                                  | 对于表中缺少匹配行的每个列，结果集都会有 `null` 值           |                                                  |
| `kind = cross`                                               | Kusto 本身不提供交叉联接风格。 无法用 `kind=cross` 来标记运算符。 | none                                             |

<img src="https://miro.medium.com/max/1400/1*UAPgZRnhFG29C0nDFi5D0A.png" alt="What is the difference between an inner and an outer join in SQL? | by Kate  Marie Lewis | Towards Data Science" style="zoom:33%;" />

## 77.02 Querys

[查询最佳做法 - Azure 数据资源管理器 | Microsoft Docs](https://docs.microsoft.com/zh-cn/azure/data-explorer/kusto/query/best-practices)

## 77.03 My Special Usage 

```R
# ETL date == today
| where ETLDate == startofday(now()) 


```



## 77.04 iff()

```
iff(`*predicate*`,` *ifTrue*`,` *ifFalse*`)
```

## 77.05 strcat_array

```
print str = strcat_array(dynamic([1, 2, 3]), "->")
>> 1->2->3
```





# Learning Material

## LM1

**<Azure Defender for IOT - Propensity Score Model Analysis, FY22 Propensity List>**

### data sample 

| ETLDate                         | 8/17/2021                           | 8/17/2021             | 8/17/2021                | 8/17/2021             |
| ------------------------------- | ----------------------------------- | --------------------- | ------------------------ | --------------------- |
| TPID                            | 940486                              | 1563603               | 521428                   | 6003430               |
| TopParent                       | PETROBRAS - PETRÃ“LEO BRASILEIRO SA | VALE SA               | SYDNEY WATER CORPORATION | DOWNER EDI LIMITED    |
| SummarySegment                  | Enterprise Commercial               | Enterprise Commercial | Enterprise Public Sector | Enterprise Commercial |
| Segment                         | Strategic Commercial                | Strategic Commercial  | Major Public Sector      | Major Commercial      |
| TimeZone                        | Americas                            | Americas              | Asia                     | Asia                  |
| Area                            | Latam                               | Latam                 | Australia                | Australia             |
| Industry                        | Oil & Gas                           | Mining                | Water & Sewage           | Real Estate           |
| Score                           | 5                                   | 6                     | 1                        | 6                     |
| IsTargetIndustry                | 0                                   | 0                     | 0                        | 1                     |
| HasSentinelACR                  | 0                                   | 1                     | 0                        | 0                     |
| IsAvgMonthlyAzureSecurityACRL50 | 1                                   | 1                     | 0                        | 1                     |
| HasOtherIoTACR                  | 1                                   | 1                     | 1                        | 1                     |
| HasE5BilledRevenue              | 1                                   | 1                     | 0                        | 1                     |
| HasMDEBilledRevenue             | 1                                   | 1                     | 0                        | 1                     |
| IsS100Account                   | 0                                   | 0                     | 0                        | 0                     |
| IsIoTDepthAccount               | 1                                   | 1                     | 0                        | 1                     |
| AvgSentinelACR                  | 0                                   | 48.64404211           | 0                        | 0                     |
| AvgAzureSecurityACR             | 107.2078189                         | 113.0411077           | 35.28801864              | 129.6997772           |
| AvgAzureTotalACR                | 420.8865483                         | 153.9425934           | 38.78522867              | 227.2815423           |
| AvgOtherIoTACR                  | 46.0071375                          | 16.97965909           | 10.4987                  | 241.6757118           |
| AvgE5BilledRevenue              | 33288.03298                         | 12293.07105           | 0                        | 426.2162806           |
| AvgMDEBilledRevenue             | 232169.8504                         | 46796.74073           | 0                        | 13.2126               |
| AvgMDEtUsageRate                | 137.3815386                         | 1.211798097           | 0                        | 0.724496806           |
| IsAD4IoTCustomer                | 0                                   | 0                     | 0                        | 0                     |



This is to share the propensity score analysis and FY22 propensity list for Azure Defender for IOT. In FY21, there are 6,461 Enterprise accounts from AzureDashboard cube that are part  of the 16 FY22 focused industries list: 6,025 major and 436 strategic accounts.  

- 5,470 have at least 1 Extensible Storage Engine (*ESE*) assigned 
- Regardless of Extensible Storage Engine (*ESE*) assignment 
  - Score >=4: 1,896 out of 6,461 , or 29.3% 
  - Score >=5: 820 out of 6,461, or 12.7%

![account_rankings.png](https://github.com/otainginga/Image/blob/main/iot_material/account_rankings.png?raw=true)

We can find detailed data at AD4IOT propensity list(based on FY21 Azure account) (total: 6,461 enterprise accounts filtered by 16 FY22 focused industries) Also sharing the propensity list using FY22 new enterprise segment list AD4IOT Propensity List-based on FY22 Account Segmentation (total: 11,011 enterprise accounts with all  industry)



### **Azure Defender for IOT - Propensity Score Model Analysis, FY22 Propensity List**

```python
The Azure Internet of Things (IoT) is a collection of Microsoft-managed cloud services that connect, monitor, and control billions of IoT assets

ACR - Azure Consumed Revenue
Sentinel ACR - 

MDE - Microsoft Defender for Endpoint

Azure Container Registry (ACR) 

YTD ACR flag

AD4IOT


```



- #### Summary 

- #### Current propensity score analysis details 

- #### High correlation from converted AD4IOT customer 

- #### New propensity score model exploration 

- #### New propensity score model proposal



**< Summary >**The goal is to analyze the Feb 2021 propensity list, conversion rate to identify higher correlation metrics from converted AD4IOT customers, explore the new propensity score model  and preparing a new FY22 propensity list for review. Summary of findings:



1.  High score shows higher conversion. 3.3% overall CR on Score >=5 in current propensity list. Total 36 accounts converted regardless of score. 
2.  The converted AD4IOT customers show higher correlation with **Sentinel ACR**, Other I**OT ACR** and **E5 revenue**, and the small attach rate shows big growth opportunity 
3.  Conversion rate using ACR in propensity score is higher than the current model using pipeline, and no significant difference on other ACR in AD4IOT customers and non-AD4IoT customers but Azure Security ACR.  
4.  50%+ accounts has less than $50 monthly Azure Security ACR, it’s the long tail distribution, and applying Azure Security ACR threshold (monthly >=$50) performs best 
5.  No significant conversion difference for different level MDE usage 
6. New Propensity score model 
   1. Remove D4IoT, E5, Azure Security Pipeline – use MDE billed Revenue, E5 Revenue, and Azure Security ACR flag instead 
   2.  Consider the 3 month avg Azure Security ACR threshold (>= $50)              Target Industry covered 68% of TPIDs (exc. N/A), suggest to add “Health Provider”, “Health Payor”, “Other government” 
   3. Propose the new score formula – sum from 0 to 8  
      -  Has Sentinel ACR  
      - Has Other IOT ACR 
      -  3 month avg Azure Security ACR >50  
      -  Has MDE Billed Revenue 
      - Has E5 Revenue  
      -  Is S100 Account 
      - Is IOT Depth Account 
      -  Is Target Focused Industry based on final list from Scott Lerch and CyberX team 
        - Automotive/ Health Provider /Consumer Goods/ Intelligence /Critical Infrastructure /Other Government/ Defense Pharma & Life Sciences /Defense Systems Integrators /Process Manufacturing & Agriculture /Discrete Manufacturing /Real Estat /Energy /Retailers /Health Payor /Travel, Tpt, Lgstcs & Hosp



**< Feb 2021 Propensity Score Analysis Details >**



The current propensity score is in range of 0 to 8, which involves the YTD ACR Flag,  Pipelines, Target Industry flag, Is S100 and Is IOT Depth account flags. Here are some  observation from the existing list. 1. There are 334(or 4.1%) from current 8.2K account list with propensity score  >=5. Total 11 accounts converted to AD4IOT customer, 3.3% overall CR.  ¡ 315 commercial customers and 11(or 3.5%) converted,  ¡ The other 19 public sector customers has 0 conversion. 2. 36 accounts converted from entire list regardless of score.  l 32 commercial, 21/32 (65%) are below score 5. All of them have Azure  Security and Azure ACR, 87.5% have sentinel ACR. 

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







## Solution [A1E]

Based on the feature, we decided to create 2 charts for each view and link them on <ID> in Power BI.

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



## Solution [A1O]





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



```R
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


# Message 2

how many of these are not matching the Account-Seller assignment.

As you already know, Opportunities and Engagements have Owners (which usually are sellers), and sellers have TP assignments. Recently we discovered that there are cases where a seller owns an Opportunity/Engagement but he/she is not assigned to the TP that these Opportunities/Engagements associated with. So we’d like to identify such cases…
              
              As I mentioned in my original email, you’ll need to leverage vwAccountSellerAssignment() to achieve what I asked for.

```

## 

|                     |        |                           |      |               |              |                      |                    |                   |
| ------------------- | ------ | ------------------------- | ---- | ------------- | ------------ | -------------------- | ------------------ | ----------------- |
| CRM  Opportunity ID | Status | Due Date                  | TPID | Business Type | Owner  Alias | Owner Standard Title | Owner  Qualifier 1 | Owner Qualifier 2 |
| CRM Engagement ID   | Status | Estimated Completion Date | TPID |               | Owner Alias  | Owner Standard Title | Owner Qualifier 1  | Owner Qualifier 2 |



## Solution [A2O]

```R
# original - with useless cases
# CRM Opportunity ID| 	Status|	Due Date|	TPID|	Business Type	|Owner Alias|	Owner Standard Title	|Owner Qualifier 1	| Owner Qualifier 2
vwOpportunity()
| where OpportunityCloseDate >= datetime(2021-07-01) or OpportunityDueDate >= datetime(2021-07-01)
| where TPID != 0
| distinct CRMOpportunityID,OpportunityOwner,OpportunityOwnerStandardTitle,TPID,MSXStatus,OpportunityDueDate,BusinessType
| join kind=inner (
    vwAccountSellerAssignment()
    | where TPID != 0
    | project EmailAlias,ATPID = TPID
)on $left.OpportunityOwner==$right.EmailAlias
| join kind=inner(
    Person()
    | distinct Alias,Qualifier1,Qualifier2
) on $left.OpportunityOwner == $right.Alias
| extend idflag = iff(ATPID ==TPID,1,0)
| summarize Totalflag =sum(idflag) by CRMOpportunityID,MSXStatus,OpportunityDueDate,TPID,BusinessType,OpportunityOwner,OpportunityOwnerStandardTitle,Qualifier1,Qualifier2
| where Totalflag ==0
| project CRMOpportunityID,MSXStatus,OpportunityDueDate,TPID,BusinessType,OpportunityOwner,OpportunityOwnerStandardTitle,Qualifier1,Qualifier2

```



```R
# Corrected by Raien
.create-or-alter function with (folder = "Adhoc/v-wenbosun", skipvalidation = "true") GetUnassignedAccoutOpp() {
    
database("MWSData").vwOpportunity()
| where TPID != 0
| where OpportunityOwner != "UNKNOWN"
| join kind=leftanti database("MWSData").vwAccountSellerAssignment() on $left.OpportunityOwner == $right.EmailAlias, TPID
| join kind=inner database("MWSData").Person() on $left.OpportunityOwner == $right.Alias
| project CRMOpportunityID, MSXStatus, OpportunityDueDate, TPID, BusinessType, OpportunityOwner, StandardTitle, Qualifier1, Qualifier2
    
    
}
```

## Solution [A2E]

```R
.create-or-alter function with (folder = "Adhoc/v-wenbosun", skipvalidation = "true") GetUnassignedAccoutEng() {
database("MWSData").vwEngagement()
| where TPID !=0
| where EngagementOwner != ""
| join kind=leftanti database("MWSData").vwAccountSellerAssignment() on $left.EngagementOwner == $right.EmailAlias,TPID
| join kind=inner database("MWSData").Person() on $left.EngagementOwner == $right.Alias
| project CRMEngagementID, EngagementStatus, EngagementEstimatedCompletionDate, TPID, EngagementOwner, StandardTitle, Qualifier1, Qualifier2
}
```





# Assignment 3

```R
Both in the Artificial Rev$ AVD and Artificial Win 365 ACR table include a column for Engagement/Opportunity owner.

 

Artificial Rev$ AVD table

Include a column with Engagement Workload.
Review 
NET ACR column calculation: End of Quarter Month ACR minus End of Previous Quarter Month ACR

End of Quarter Month ACR Cap: delete or repurpose to NET ACR Cap

NET ACR Cap Column:
    If last month of previous quarter ACR >$20K, then NET ACR Cap = 0 (Customer already invested in AVD)
    If Net ACR > $20K, then NET ACR Cap = $20K
    If Net ACR <$1K, then NET ACR Cap = 0
    Artificial Rev$ AVD column: =[Net ACR Cap]*[Conversion Ratio]*12, if < 0, then = 0
```









Hi! 

I have published the pbix to MW&S reporting Devenv, This is the progress and some questions:

Progress

1. add owners in Engagement/Opportunity
2. add  Engagement Workload.
3. update NET ACR





Engagement 

1. Is that necessary to make sure the data should all in the category of "Infra: AVD" when joining tables?
2. I kept the time on FY21-Q4, since the ACR are all zeros now. Do I need to update the table to FY21Q1?
3. One thing I can not fix is, there are still some blanks in Engagementworkload.
4. What's the meaning for the limitation EngagementEstimatedCompletionDate >= datetime(2021-04-01) 



Opportunity: 

There are still a lot of blanks inn opportunityowner and I do not know why.



## AVD Engagement

```python

vwEngagementMilestone(azureSecOnly=false)
| join kind=inner vwEngagement() on CRMEngagementID
| where EngagementMilestoneWorkload in ("Infra: Native Azure Virtual Desktop", "Infra: Citrix + Azure Virtual Desktop", "Infra: Horizon + Azure Virtual Desktop (VMware)")
| where isnotempty(CRMEngagementID)
| where EngagementEstimatedCompletionDate >= datetime(2021-04-01) 
// temporarily using last FY Q4 since current FY Q1 hasn't ended yet
| distinct CRMEngagementID, TPID, TopParent, EngagementStatus, EngagementEstimatedCompletionDate, EngagementOwner, EngagementMilestoneWorkload
| summarize EngagementWorkload=strcat_array(make_list(EngagementMilestoneWorkload), ", ") by CRMEngagementID, TPID, TopParent, EngagementStatus, EngagementEstimatedCompletionDate, EngagementOwner
| join kind=inner vwMAL() on TPID // exclude non-MAL accounts
| join kind=inner Calendar() on $left.EngagementEstimatedCompletionDate == $right.FiscalDate
| project CRMEngagementID, TPID, EngagementStatus, EngagementOwner=toupper(EngagementOwner), EngagementWorkload,
    ClosedQuarter=iif(EngagementStatus == "Closed-Completed", FiscalQuarter, ""),
    ClosedMonth=iif(EngagementStatus == "Closed-Completed", startofmonth(EngagementEstimatedCompletionDate), datetime(null))
| join kind=inner vwEngagementTeam() on CRMEngagementID
| join kind=inner Person() on $left.EngagementTeamAlias == $right.Alias
| join kind=inner (
    cluster('1es.kusto.windows.net').database('HeadTrax').Person
    | where CurrentlyEmployed
    | project Alias=toupper(Alias), DisplayName
) on Alias
| project CRMEngagementID, TPID, EngagementStatus, ClosedQuarter, ClosedMonth, EngagementOwner, EngagementWorkload,
    Alias=EngagementTeamAlias, DisplayName, StandardTitle=trim_end("\\d", StandardTitle), Qualifier1, Qualifier2
| extend
    QualifiedRole=case(
        StandardTitle in ("Solution Area Specialists IC", "Specialist") and Qualifier1 == "N/A" and Qualifier2 in ("Modern Work", "MW-Surface", "Security"), "Yes",
        StandardTitle in ("Technology Specialists IC", "Technical Sepcialist") and Qualifier1 == "N/A" and Qualifier2 == "Cloud Endpoint", "Yes",
        "No"
    )
```



notes:

```R
# for different ID with mulitiple EngagementMilestoneWorkload
| summarize EngagementWorkload=strcat_array(make_list(EngagementMilestoneWorkload), ", ") by CRMEngagementID, TPID, TopParent, EngagementStatus, EngagementEstimatedCompletionDate, EngagementOwner

# exclude non-MAL accounts
| join kind=inner vwMAL() on TPID 


```



## TP ACR FY21Q4

```python
let endOfQuarterMonth = toscalar(datetime(2021-06-01));
let endOfPrevQuarterMonth = datetime_add("month", -3, endOfQuarterMonth);
let quarter = toscalar(Calendar() | where FiscalDate == endOfQuarterMonth | project FiscalQuarter);
let tpids = materialize(vwEngagementMilestone(azureSecOnly=false)
| join kind=inner vwEngagement(azureSecOnly=false) on CRMEngagementID
| where EngagementMilestoneWorkload == "Infra: Native Azure Virtual Desktop"
| where isnotempty(CRMEngagementID)
| where EngagementEstimatedCompletionDate >= datetime(2021-04-01) // temporarily using last FY Q4 since current FY Q1 hasn't ended yet
| distinct TPID
| join kind=inner vwMAL() on TPID // exclude non-MAL accounts
| project TPID); 
let currentACR = tpids
| join kind=inner vwACR() on TPID
| where ServiceLevel1 == "Compute"
| where ServiceInfluencer in ("NATIVE WVD", "CITRIX.CLOUD.WVD", "VMWARE.HORIZON CLOUD.AVD")
| where FiscalMonth == endOfQuarterMonth
| summarize ACR=sum(AzureConsumedRevenue) by TPID
| project TPID, FiscalQuarter=quarter, EndOfQuarterMonth=endOfQuarterMonth, ACR;
let prevACR = tpids
| join kind=inner vwACR() on TPID
| where ServiceLevel1 == "Compute"
| where ServiceInfluencer in ("NATIVE WVD", "CITRIX.CLOUD.WVD", "VMWARE.HORIZON CLOUD.AVD")
| where FiscalMonth == endOfPrevQuarterMonth
| summarize ACR=sum(AzureConsumedRevenue) by TPID
| project TPID, EndOfPreviousQuarterMonth=endOfPrevQuarterMonth, PreviousACR=ACR;
currentACR
| join kind=leftouter prevACR on TPID
| join kind=inner vwMAL() on TPID
| project TPID, TopParent, SummarySegment, FiscalQuarter, EndOfQuarterMonth, ACR,
    EndOfPreviousQuarterMonth=endOfPrevQuarterMonth, PreviousACR=iif(isnull(PreviousACR), decimal(0), PreviousACR)
| extend
    NetACR=ACR - PreviousACR
| extend
    NetACRCap=case(
        PreviousACR > 20000, 0,
        NetACR > 20000, 20000,
        NetACR < 1000, 0,
        0
    )
| extend
    ConvertionRatio=1.20
| extend
    ArtificialRev=NetACRCap * ConvertionRatio * 12
```







## W365 Opportunity

```python
vwFactOpportunityPipeline()
| where OpportunityCloseDate >= datetime(2021-07-01) or OpportunityDueDate >= datetime(2021-07-01)
| where RevSumCategory == "Cloud PC"
| distinct CRMOpportunityID, TPID, TopParent, MSXStatus, OpportunityDueDate, OpportunityCloseDate, OpportunityOwner
| join kind=inner vwMAL() on TPID // exclude non-MAL accounts
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
    DisplayName, OpportunityTeamAlias, StandardTitle=trim_end("\\d", StandardTitle), Qualifier1, Qualifier2, OpportunityOwner
| extend
    QualifiedRole=case(
        StandardTitle in (
            "Solution Area Specialists IC",
            "Specialist",
            "Cloud Solution Architecture IC",
            "Cloud Solution Architect",
            "Cloud Solution Architecture OPEX IC",
            "Cloud Solution Architect (OPEX)") and Qualifier1 == "N/A" and Qualifier2 == "Azure-Infrastructure", "Yes",
        StandardTitle in (
            "Digital Solution Area Specialists IC",
            "Digital Specialist") and Qualifier1 == "Enterprise" and Qualifier2 == "Azure-Apps&Infra-CustAcq", "Yes",
        "No"
    )
```



## TP CloudPC Seats FY22

```python
vwFactOpportunityPipeline()
| where OpportunityCloseDate >= datetime(2021-07-01) or OpportunityDueDate >= datetime(2021-07-01)
| where RevSumCategory == "Cloud PC"
| distinct TPID, TopParent
| join kind=inner vwBilledRevenue(snapshotIndex=1) on TPID
| where RevSumCategory == "Cloud PC"
| where FiscalMonth between (datetime(2021-07-01) .. startofmonth(now()))
| summarize InitiatedUnits=sum(InitiatedUnits) by TPID, FiscalMonth, TransactionType
| join kind=inner (
    Calendar()
    | distinct FiscalQuarter, FiscalMonth
) on FiscalMonth
| join kind=inner vwMAL() on TPID
| project TPID, TopParent, SummarySegment, FiscalQuarter, FiscalMonth,TransactionType, InitiatedUnits,
    InitiatedUnitsCap=case(
        InitiatedUnits < 50, todecimal(0),
        InitiatedUnits >= 1500, todecimal(1500),
        InitiatedUnits
    ),
    RateCard=22
| extend
    ArtificialACRW365=InitiatedUnitsCap * RateCard
```















































































































