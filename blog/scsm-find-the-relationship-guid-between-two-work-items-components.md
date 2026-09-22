---
layout: default
permalink: /blog/scsm-find-the-relationship-guid-between-two-work-items-components
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# SCSM Find the Relationship GUID between two Work Items Components
*Author: Brody Kilpatrick* | *Created: February 2, 2012*

I was recently editing a notification mangement pack and needed to find the GUID for a relationship between two instance so I could send an email to the correct user.  
IN SCSM 2012 there is a cmdlet that can be used to find the name if a relationship called get-SCRelationship. However, it doesn't provide the GUID.  
  
If you want to view all relationship types along with there GUID and other details, use this simple database query. Open SQL Management Studio and connect to your Service Manager Database.  
  
  
  
*Select  \**  
*FROM ServiceManager.dbo.RelationshipTypeView*  
*where*  
*LanguageCode = 'ENU'*  
*and (*  
*Name like '%incident%' OR*  
*DisplayName like '%incident%' OR*  
*TargetName like '%incident%' OR*  
*SourceName like '%incident%'*  
*)*  
  
  
Replace "Incident" with different details if needed. For example "PrimaryOwner."
