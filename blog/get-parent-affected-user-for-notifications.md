---
layout: default
permalink: /blog/get-parent-affected-user-for-notifications
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Get Parent Affected User for Notifications
*Author: Brody Kilpatrick* | *Created: February 13, 2014*

This is more of a note for myself. For an activity, this will get the affected user of the parent work item.  
  
$Context/Path[Relationship='CoreActivity!System.WorkItemContainsActivity' SeedRole='Target' TypeConstraint='WorkItem!System.WorkItem']/Path[Relationship='WorkItem!System.WorkItemAffectedUser' TypeConstraint='System!System.User']/Property[Type='System!System.User']/FirstName$  
  
Obviously it is only the first name and the references will need to be changed to match the reference alias in the MP.
