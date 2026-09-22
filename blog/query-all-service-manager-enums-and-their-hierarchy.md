---
layout: default
permalink: /blog/query-all-service-manager-enums-and-their-hierarchy
---
# Query ALL Service Manager ENUMS and their Hierarchy
*Author: Brody Kilpatrick* | *Created: April 22, 2014*

I find myself listing out all of the enumerations for lists in Service Manager quite a bit. Rather than spending time doing this over and over, I wrote a query that retrieves all of the enumeration items from Service Manager. I tried to keep it simple so anyone could adjust to his or her needs. It does not require the DW, as I am pulling directly from the ServiceManager database.  

>   SELECT [EnumType].[EnumTypeId] AS Id,  
>       [EnumType].[ManagementPackId] AS ManagementPackId,  
>       ep.EnumTypeName,  
>       [EnumType].[EnumTypeName] AS Name,  
>       [EnumType].[EnumTypeAccessibility] AS Accessibility,  
>       [EnumType].[ParentEnumTypeId] AS ParentId,  
>       DisplayName  
>   into #eview  
>   FROM dbo.EnumType  
>   LEFT Join dbo.EnumType ep on EnumType.EnumTypeId = ep.EnumTypeId and ep.ParentEnumTypeId IS NULL  
>   LEFT OUTER JOIN DisplayStringView DS1 ON DS1.LTStringId = dbo.[EnumType].[EnumTypeId] AND DS1.LanguageCode = 'ENU'  
>    
>   INNER JOIN dbo.ManagementPack  
>    ON dbo.ManagementPack.ManagementPackId = [EnumType].ManagementPackId AND dbo.ManagementPack.ContentReadable = 1;  
>      
>   with tree as (  
>   SELECT ManagementPackid, Id, name,  
>   cast(DisplayName as varchar(max)) as Hierarchy,  
>   DisplayName,  
>   ParentId  
>   FROM #eview  
>   Where ParentId IS NULL and displayName IS NOT NULL  
>   UNION ALL  
>   SELECT c.ManagementPackId, c.Id, c.name,  
>   p.hierarchy + ', ' + cast(c.DisplayName as varchar(max)),  
>   c.DisplayName, c.ParentId  
>   FROM #eview c  
>   join tree p on p.Id = c.parentID  
>   WHERE c.displayName IS NOT NULL  
>   )  
> select ManagementPackid, parentid, Name, Hierarchy, DisplayName  
> from tree  
> order by 3  
> drop table #eview
