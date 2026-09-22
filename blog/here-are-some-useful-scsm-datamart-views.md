---
layout: default
permalink: /blog/here-are-some-useful-scsm-datamart-views
---
# Here are some useful SCSM DataMart Views
*Author: Brody Kilpatrick* | *Created: August 29, 2011*

**Create Announcements view from The CMDB**  
  
  
USE [DWDataMart]  
GO  
/\*\*\*\*\*\* Object:  View [dbo].[Custom\_Announcements]    Script Date: 08/29/2011 13:16:37 \*\*\*\*\*\*/  
SET ANSI\_NULLS ON  
GO  
SET QUOTED\_IDENTIFIER ON  
GO  
Create View [dbo].[Custom\_Announcements]  
As  
Select BaseManagedEntityId,  
ExpirationDate\_DDB905FF\_A923\_F97D\_D00B\_6430C2CD5E95 as Expiration,  
P.DisplayName as Priority,  
Title\_A24FDDD9\_5BC0\_B3D0\_E014\_1F98C0B9143E Announcement,  
Left(Substring(Body\_3D226BC1\_0528\_14EA\_F3FB\_3823A132B4F8,Charindex('\ltrch',Body\_3D226BC1\_0528\_14EA\_F3FB\_3823A132B4F8)+7,250),Charindex('}',Substring(Body\_3D226BC1\_0528\_14EA\_F3FB\_3823A132B4F8,Charindex('\ltrch',Body\_3D226BC1\_0528\_14EA\_F3FB\_3823A132B4F8)+7,250))-1) Body  
 FROM ServiceManager.dbo.MTV\_System$Announcement$Item a  
 Join ServiceManager.dbo.DisplayStringView P on a.Priority\_6986BA50\_58CF\_ABCA\_FB58\_8FCAB694E6C9 = P.LTStringId and LanguageCode = 'ENU'  
GO  
  
  
**Review Activities**  
  
/\*\*\*\*\*\* Object:  View [dbo].[Custom\_ReviewActivityList]    Script Date: 08/29/2011 13:16:37 \*\*\*\*\*\*/  
SET ANSI\_NULLS ON  
GO  
SET QUOTED\_IDENTIFIER ON  
GO  
  
CREATE VIEW [dbo].[Custom\_ReviewActivityList]  
 AS  
  
  
Select  
RA.Id + Coalesce(Cast(R.ReviewerDimKey as varchar(255)),'') As ActivityandReviewer\_Key,  
RA.BaseManagedEntityId as ActivityGUID,  
RA.Id as ActivityId,  
Cast('https://servicemanagerdev/analyst/ReviewActivityDetails.aspx?AID=' + RA.Id AS varchar(255)) ViewActivity,  
RA.CreatedDate,  
RA.Title,  
DecString.DisplayName,  
R.DecisionDate,  
R.Comments as DecisionComments,  
U.FirstName as ReviewerFirstName, U.LastName as ReviewerLastName, U.DisplayName as ReviewerDisplayName, U.UserName as ReviewerUserName,  
WI.BaseManagedEntityId as ParentWorkItemGUId,  
WI.Id as ParentWorkItemId,  
WI.Title as ParentWorkitemTitle,  
Cast('https://servicemanagerdev/analyst/ChangeRequestDetails.aspx?CRID=' + WI.Id  AS varchar(255)) ViewParent  
FROM ActivityDimvw A  
Join ReviewActivityDimvw RA on A.BaseManagedEntityId = RA.BaseManagedEntityId  
Left Join ReviewActivityhasReviewerFactvw RARF on A.ActivityDimKey = RARF.ActivityDimKey  
LEFT Join ReviewerDimvw R on RARF.ReviewActivityHasReviewer\_ReviewerDimKey = R.ReviewerDimKey  
 LEFT Join ReviewerDecisionvw RD ON R.Decision\_ReviewerDecisionId = RD.ReviewerDecisionId  
 Left Join dbo.DisplayStringDimvw as DecString on RD.EnumTypeId = DecString.BaseManagedEntityId and DecString.LanguageCode = 'ENU'  
  
LEFT Join ReviewerIsUserFactvw RU on R.ReviewerDimKey = RU.ReviewerDimKey  
LEFT Join UserDimvw U on RU.ReviewerIsUser\_UserDimKey = U.UserDimKey  
  
Left Join ActivityAreavw AA  With (NOLOCK) on RA.Area\_ActivityAreaId= AA.ActivityAreaId  
Left Join dbo.DisplayStringDimvw as AAString on AA.EnumTypeId = AAString.BaseManagedEntityId and AAString.LanguageCode = 'ENU'  
  
Left Join ActivityPriorityvw AP  With (NOLOCK) on RA.Priority\_ActivityPriorityId = AP.ActivityPriorityId  
Left Join dbo.DisplayStringDimvw as APString on AP.EnumTypeId = APString.BaseManagedEntityId and APString.LanguageCode = 'ENU'  
  
Left Join ActivityStagevw AStage  With (NOLOCK) on RA.Stage\_ActivityStageId = AStage.ActivityStageId  
Left Join dbo.DisplayStringDimvw as AStageString on AStage.EnumTypeId = AStageString.BaseManagedEntityId and AStageString.LanguageCode = 'ENU'  
  
Left Join ActivityStatusvw AStatus  With (NOLOCK) on RA.Status\_ActivityStatusId = AStatus.ActivityStatusId  
Left Join dbo.DisplayStringDimvw as AStatusString on AStatus.EnumTypeId = AStatusString.BaseManagedEntityId and AStatusString.LanguageCode = 'ENU'  
  
Left Join WorkItemContainsActivityFactvw WICA on A.ActivityDimKey = WICA.WorkItemContainsActivity\_ActivityDimKey  
Left Join WorkItemDimvw WI on WICA.WorkItemDimKey = WI.WorkItemDimKey  
GO  
  
**Incidents**  
  
  
/\*\*\*\*\*\* Object:  View [dbo].[Custom\_IncidentList\_with\_HL]    Script Date: 08/29/2011 13:16:37 \*\*\*\*\*\*/  
SET ANSI\_NULLS ON  
GO  
SET QUOTED\_IDENTIFIER ON  
GO  
CREATE VIEW [dbo].[Custom\_IncidentList\_with\_HL]  
 AS  
  
Select   
I.BaseManagedEntityId as IncidentGUID,  
I.Id as IncidentId,  
'https://servicemanagerdev/enduser/RequestDetails.aspx?RequestsId=' + Convert(varchar(50),I.BaseManagedEntityId) + '&RequestType=incident'  
AS IncidentLink,  
I.CreatedDate,  
IncidentStatusString.DisplayName as 'Status',  
IncidentClassString.DisplayName  as 'Classification',  
I.Title, I.Description, Escalated,  
IncidentImpactString.DisplayName as Impact,  
IncidentUrgencyString.DisplayName as Urgency,  
Priority,  
IncidentResCatString.DisplayName as ResolutionCategory,  
ResolutionDescription,  
TargetResolutionTime,  
AffectedUser.FirstName as AffectedUserFirstName, AffectedUser.LastName as AffectedUserLastName, AffectedUser.DisplayName as AffectedUserDisplayName, AffectedUser.UserName as AffectedUserName,  
ContactMethod as AffectedUserContactMethod,  
AssignedUser.FirstName as AssignedUserFirstName, AssignedUser.LastName as AssignedUserLastName, AssignedUser.DisplayName as AssignedUserDisplayName, AssignedUser.UserName as AssignedUserName,  
CreatedByUser.FirstName as CreatedByUserFirstName, CreatedByUser.LastName as CreatedByUserLastName, CreatedByUser.DisplayName as CreatedByUserDisplayName, CreatedByUser.UserName as CreatedByUserName  
FROM dbo.IncidentDimvw I With (NOLOCK)  
Join WorkItemDimvw WI  With (NOLOCK) on I.BaseManagedEntityId = WI.BaseManagedEntityId  
  
LEFT Join IncidentStatusvw S  With (NOLOCK) on I.Status\_IncidentStatusId = s.IncidentStatusId  
LEFT Join dbo.DisplayStringDimvw as IncidentStatusString on S.EnumTypeId = IncidentStatusString.BaseManagedEntityId and IncidentStatusString.LanguageCode = 'ENU'  
  
LEFT Join IncidentClassificationvw C on I.Classification\_IncidentClassificationId = C.IncidentClassificationId  
LEFT Join dbo.DisplayStringDimvw as IncidentClassString on C.EnumTypeId = IncidentClassString.BaseManagedEntityId and IncidentClassString.LanguageCode = 'ENU'  
  
LEFT Join IncidentImpactvw Impact on I.Impact\_IncidentImpactId = Impact.IncidentImpactId  
LEFT Join dbo.DisplayStringDimvw as IncidentImpactString on Impact.EnumTypeId = IncidentImpactString.BaseManagedEntityId and IncidentImpactString.LanguageCode = 'ENU'  
  
LEFT Join IncidentUrgencyvw Urgency on I.Urgency\_IncidentUrgencyId = Urgency.IncidentUrgencyId  
LEFT Join dbo.DisplayStringDimvw as IncidentUrgencyString on Urgency.EnumTypeId = IncidentUrgencyString.BaseManagedEntityId and IncidentUrgencyString.LanguageCode = 'ENU'  
  
LEFT Join IncidentResolutionCategoryvw ResCat on I.ResolutionCategory\_IncidentResolutionCategoryId = ResCat.IncidentResolutionCategoryId  
LEFT Join dbo.DisplayStringDimvw as IncidentResCatString on ResCat.EnumTypeId = IncidentResCatString.BaseManagedEntityId and IncidentResCatString.LanguageCode = 'ENU'  
  
Left Join WorkItemAffectedUserFactvw WIAF on WI.WorkItemDimKey = WIAF.WorkItemDimKey and WIAF.DeletedDate IS NULL  
Left Join UserDimvw AffectedUser on WIAF.WorkItemAffectedUser\_UserDimKey = AffectedUser.UserDimKey  
  
Left Join WorkItemAssignedToUserFactvw WIAT on WI.WorkItemDimKey = WIAT.WorkItemDimKey and WIAT.DeletedDate IS NULL  
Left Join UserDimvw AssignedUser on WIAT.WorkItemAssignedToUser\_UserDimKey = AssignedUser.UserDimKey  
  
Left Join WorkItemCreatedByUserFactvw WICB on WI.WorkItemDimKey = WICB.WorkItemDimKey and WICB.DeletedDate IS NULL  
Left Join UserDimvw CreatedByUser on WICB.WorkItemCreatedByUser\_UserDimKey = CreatedByUser.UserDimKey  
  
Where 1=1  
   
and Status &lt;&gt; ('IncidentStatusEnum.Closed')  
GO  
  
**Another Incident List without a Hyperlink**  
  
/\*\*\*\*\*\* Object:  View [dbo].[Custom\_IncidentList]    Script Date: 08/29/2011 13:16:37 \*\*\*\*\*\*/  
SET ANSI\_NULLS ON  
GO  
SET QUOTED\_IDENTIFIER ON  
GO  
CREATE VIEW [dbo].[Custom\_IncidentList]  
 AS  
  
Select   
I.BaseManagedEntityId as IncidentGUID,  
I.Id as IncidentId,  
I.CreatedDate,  
IncidentStatusString.DisplayName as 'Status',  
IncidentClassString.DisplayName  as 'Classification',  
I.Title, I.Description, Escalated,  
IncidentImpactString.DisplayName as Impact,  
IncidentUrgencyString.DisplayName as Urgency,  
Priority,  
IncidentResCatString.DisplayName as ResolutionCategory,  
ResolutionDescription,  
TargetResolutionTime,  
AffectedUser.FirstName as AffectedUserFirstName, AffectedUser.LastName as AffectedUserLastName, AffectedUser.DisplayName as AffectedUserDisplayName, AffectedUser.UserName as AffectedUserName,  
ContactMethod as AffectedUserContactMethod,  
AssignedUser.FirstName as AssignedUserFirstName, AssignedUser.LastName as AssignedUserLastName, AssignedUser.DisplayName as AssignedUserDisplayName, AssignedUser.UserName as AssignedUserName,  
CreatedByUser.FirstName as CreatedByUserFirstName, CreatedByUser.LastName as CreatedByUserLastName, CreatedByUser.DisplayName as CreatedByUserDisplayName, CreatedByUser.UserName as CreatedByUserName  
FROM dbo.IncidentDimvw I With (NOLOCK)  
Join WorkItemDimvw WI  With (NOLOCK) on I.BaseManagedEntityId = WI.BaseManagedEntityId  
  
LEFT Join IncidentStatusvw S  With (NOLOCK) on I.Status\_IncidentStatusId = s.IncidentStatusId  
LEFT Join dbo.DisplayStringDimvw as IncidentStatusString on S.EnumTypeId = IncidentStatusString.BaseManagedEntityId and IncidentStatusString.LanguageCode = 'ENU'  
  
LEFT Join IncidentClassificationvw C on I.Classification\_IncidentClassificationId = C.IncidentClassificationId  
LEFT Join dbo.DisplayStringDimvw as IncidentClassString on C.EnumTypeId = IncidentClassString.BaseManagedEntityId and IncidentClassString.LanguageCode = 'ENU'  
  
LEFT Join IncidentImpactvw Impact on I.Impact\_IncidentImpactId = Impact.IncidentImpactId  
LEFT Join dbo.DisplayStringDimvw as IncidentImpactString on Impact.EnumTypeId = IncidentImpactString.BaseManagedEntityId and IncidentImpactString.LanguageCode = 'ENU'  
  
LEFT Join IncidentUrgencyvw Urgency on I.Urgency\_IncidentUrgencyId = Urgency.IncidentUrgencyId  
LEFT Join dbo.DisplayStringDimvw as IncidentUrgencyString on Urgency.EnumTypeId = IncidentUrgencyString.BaseManagedEntityId and IncidentUrgencyString.LanguageCode = 'ENU'  
  
LEFT Join IncidentResolutionCategoryvw ResCat on I.ResolutionCategory\_IncidentResolutionCategoryId = ResCat.IncidentResolutionCategoryId  
LEFT Join dbo.DisplayStringDimvw as IncidentResCatString on ResCat.EnumTypeId = IncidentResCatString.BaseManagedEntityId and IncidentResCatString.LanguageCode = 'ENU'  
  
Left Join WorkItemAffectedUserFactvw WIAF on WI.WorkItemDimKey = WIAF.WorkItemDimKey and WIAF.DeletedDate IS NULL  
Left Join UserDimvw AffectedUser on WIAF.WorkItemAffectedUser\_UserDimKey = AffectedUser.UserDimKey  
  
Left Join WorkItemAssignedToUserFactvw WIAT on WI.WorkItemDimKey = WIAT.WorkItemDimKey and WIAT.DeletedDate IS NULL  
Left Join UserDimvw AssignedUser on WIAT.WorkItemAssignedToUser\_UserDimKey = AssignedUser.UserDimKey  
  
Left Join WorkItemCreatedByUserFactvw WICB on WI.WorkItemDimKey = WICB.WorkItemDimKey and WICB.DeletedDate IS NULL  
Left Join UserDimvw CreatedByUser on WICB.WorkItemCreatedByUser\_UserDimKey = CreatedByUser.UserDimKey  
  
Where 1=1  
--and I.Id = 'IR124'  
--and (  
   
-- AffectedUser.UserName = 'Brody\_Kilpatrick' OR  
-- Assigneduser.UserName = 'Brody\_Kilpatrick' OR  
-- CreatedByUser.UserName = 'Brody\_Kilpatrick'  
-- )  
   
and Status &lt;&gt; ('IncidentStatusEnum.Closed')  
GO  
  
**Change Requests**  
  
/\*\*\*\*\*\* Object:  View [dbo].[Custom\_ChangeList]    Script Date: 08/29/2011 13:16:37 \*\*\*\*\*\*/  
SET ANSI\_NULLS ON  
GO  
SET QUOTED\_IDENTIFIER ON  
GO  
CREATE VIEW [dbo].[Custom\_ChangeList]  
 AS  
 Select   
CR.BaseManagedEntityId ChangeGUID,  
CR.ID ChangeRequestId,  
'https://servicemanagerdev/enduser/RequestDetails.aspx?RequestsId=' + Convert(varchar(50),CR.BaseManagedEntityId) + '&RequestType=changeRequest'  
AS EndUserPortalLink,  
'https://servicemanagerdev/analyst/ChangeRequestDetails.aspx?CRID=' + CONVERT(varchar(50),CR.ID)  
AS AnalystPortalLink,  
CR.ActualStartDate,  
CR.ActualEndDate,  
CR.CreatedDate,  
CR.Description,  
CR.Notes,  
CR.PostImplementationReview,  
CR.Reason,  
CR.RequiredByDate,  
CR.Title,  
CAString.DisplayName Area,  
CCString.DisplayName Category,  
CIString.DisplayName Impact,  
CImpString.DisplayName ImplementationResults,  
CPString.DisplayName Priority,  
CRiskString.DisplayName Risk,  
CSString.DisplayName 'Status',  
  
AffectedUser.FirstName as AffectedUserFirstName, AffectedUser.LastName as AffectedUserLastName, AffectedUser.DisplayName as AffectedUserDisplayName, AffectedUser.UserName as AffectedUserName,  
ContactMethod as AffectedUserContactMethod,  
AssignedUser.FirstName as AssignedUserFirstName, AssignedUser.LastName as AssignedUserLastName, AssignedUser.DisplayName as AssignedUserDisplayName, AssignedUser.UserName as AssignedUserName,  
CreatedByUser.FirstName as CreatedByUserFirstName, CreatedByUser.LastName as CreatedByUserLastName, CreatedByUser.DisplayName as CreatedByUserDisplayName, CreatedByUser.UserName as CreatedByUserName  
  
  
FROM dbo.ChangeRequestDimvw CR  
Join dbo.WorkItemDimvw WI on CR.BaseManagedEntityId = WI.BaseManagedEntityId  
  
Left Join WorkItemAffectedUserFactvw WIAF on WI.WorkItemDimKey = WIAF.WorkItemDimKey and WIAF.DeletedDate IS NULL  
Left Join UserDimvw AffectedUser on WIAF.WorkItemAffectedUser\_UserDimKey = AffectedUser.UserDimKey  
  
Left Join WorkItemAssignedToUserFactvw WIAT on WI.WorkItemDimKey = WIAT.WorkItemDimKey and WIAT.DeletedDate IS NULL  
Left Join UserDimvw AssignedUser on WIAT.WorkItemAssignedToUser\_UserDimKey = AssignedUser.UserDimKey  
  
Left Join WorkItemCreatedByUserFactvw WICB on WI.WorkItemDimKey = WICB.WorkItemDimKey and WICB.DeletedDate IS NULL  
Left Join UserDimvw CreatedByUser on WICB.WorkItemCreatedByUser\_UserDimKey = CreatedByUser.UserDimKey  
  
  
Left Join ChangeAreavw CA  With (NOLOCK) on CR.Area\_ChangeAreaId = CA.ChangeAreaId  
Left Join dbo.DisplayStringDimvw as CAString on CA.EnumTypeId = CAString.BaseManagedEntityId and CAString.LanguageCode = 'ENU'  
  
Left Join ChangeCategoryvw CC  With (NOLOCK) on CR.Category\_ChangeCategoryId = CC.ChangeCategoryId  
Left Join dbo.DisplayStringDimvw as CCString on CC.EnumTypeId = CCString.BaseManagedEntityId and CCString.LanguageCode = 'ENU'  
  
Left Join ChangeImpactvw CI  With (NOLOCK) on CR.Impact\_ChangeImpactId = CI.ChangeImpactId  
Left Join dbo.DisplayStringDimvw as CIString on CI.EnumTypeId = CIString.BaseManagedEntityId and CIString.LanguageCode = 'ENU'  
  
Left Join ChangeImplementationResultsvw CImp  With (NOLOCK) on CR.ImplementationResults\_ChangeImplementationResultsId = CImp.ChangeImplementationResultsId  
Left Join dbo.DisplayStringDimvw as CImpString on CImp.EnumTypeId = CImpString.BaseManagedEntityId and CImpString.LanguageCode = 'ENU'  
  
Left Join ChangePriorityvw CP  With (NOLOCK) on CR.Priority\_ChangePriorityId = CP.ChangePriorityId  
Left Join dbo.DisplayStringDimvw as CPString on CP.EnumTypeId = CPString.BaseManagedEntityId and CPString.LanguageCode = 'ENU'  
  
Left Join ChangeRiskvw CRisk  With (NOLOCK) on CR.Risk\_ChangeRiskId = CRisk.ChangeRiskId  
Left Join dbo.DisplayStringDimvw as CRiskString on CRisk.EnumTypeId = CRiskString.BaseManagedEntityId and CRiskString.LanguageCode = 'ENU'  
  
Left Join ChangeStatusvw CS  With (NOLOCK) on CR.Status\_ChangeStatusId = CS.ChangeStatusId  
Left Join dbo.DisplayStringDimvw as CSString on CS.EnumTypeId = CSString.BaseManagedEntityId and CSString.LanguageCode = 'ENU'  
GO  
  
**All Activity List**  
  
/\*\*\*\*\*\* Object:  View [dbo].[Custom\_ActivityList]    Script Date: 08/29/2011 13:16:37 \*\*\*\*\*\*/  
SET ANSI\_NULLS ON  
GO  
SET QUOTED\_IDENTIFIER ON  
GO  
CREATE VIEW [dbo].[Custom\_ActivityList]  
 AS  
 Select  
A.BaseManagedEntityId ActivityGUID,  
A.Id ActivityId,  
Case When ra.ActivityId IS NULL  
 Then 'https://servicemanagerdev/analyst/ManualActivityDetails.aspx?AID=' + CONVERT(varchar(50),A.Id)  
 Else 'https://servicemanagerdev/analyst/ReviewActivityDetails.aspx?AID=' + CONVERT(varchar(50),A.Id)   
End ActivityLink,  
A.Title,  
ActualStartDate,  
ActualEndDate,  
A.CreatedDate,  
A.Description,  
A.Notes,  
ScheduledStartDate,  
ScheduledEndDate,  
  
AssignedUser.FirstName as AssignedUserFirstName, AssignedUser.LastName as AssignedUserLastName, AssignedUser.DisplayName as AssignedUserDisplayName, AssignedUser.UserName as AssignedUserName,  
CreatedByUser.FirstName as CreatedByUserFirstName, CreatedByUser.LastName as CreatedByUserLastName, CreatedByUser.DisplayName as CreatedByUserDisplayName, CreatedByUser.UserName as CreatedByUserName,  
  
AAString.DisplayName Area,  
APString.DisplayName Priority,  
AStageString.DisplayName Stage,  
AStatusString.DisplayName 'Status'  
  
FROM dbo.ActivityDimvw A  
Join WorkItemDimvw WI on A.BaseManagedEntityId = WI.BaseManagedEntityId  
  
Left Join WorkItemAssignedToUserFactvw WIAT on WI.WorkItemDimKey = WIAT.WorkItemDimKey and WIAT.DeletedDate IS NULL  
Left Join UserDimvw AssignedUser on WIAT.WorkItemAssignedToUser\_UserDimKey = AssignedUser.UserDimKey  
  
Left Join WorkItemCreatedByUserFactvw WICB on WI.WorkItemDimKey = WICB.WorkItemDimKey and WICB.DeletedDate IS NULL  
Left Join UserDimvw CreatedByUser on WICB.WorkItemCreatedByUser\_UserDimKey = CreatedByUser.UserDimKey  
  
  
Left Join ActivityAreavw AA  With (NOLOCK) on A.Area\_ActivityAreaId= AA.ActivityAreaId  
Left Join dbo.DisplayStringDimvw as AAString on AA.EnumTypeId = AAString.BaseManagedEntityId and AAString.LanguageCode = 'ENU'  
  
Left Join ActivityPriorityvw AP  With (NOLOCK) on A.Priority\_ActivityPriorityId = AP.ActivityPriorityId  
Left Join dbo.DisplayStringDimvw as APString on AP.EnumTypeId = APString.BaseManagedEntityId and APString.LanguageCode = 'ENU'  
  
Left Join ActivityStagevw AStage  With (NOLOCK) on A.Stage\_ActivityStageId = AStage.ActivityStageId  
Left Join dbo.DisplayStringDimvw as AStageString on AStage.EnumTypeId = AStageString.BaseManagedEntityId and AStageString.LanguageCode = 'ENU'  
  
Left Join ActivityStatusvw AStatus  With (NOLOCK) on A.Status\_ActivityStatusId = AStatus.ActivityStatusId  
Left Join dbo.DisplayStringDimvw as AStatusString on AStatus.EnumTypeId = AStatusString.BaseManagedEntityId and AStatusString.LanguageCode = 'ENU'  
  
Left Join Custom\_ReviewActivityList ra on A.Id = ra.ActivityId  
GO
