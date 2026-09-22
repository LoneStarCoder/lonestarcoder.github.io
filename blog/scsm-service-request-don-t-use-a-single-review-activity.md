---
layout: default
permalink: /blog/scsm-service-request-don-t-use-a-single-review-activity
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# SCSM Service Request - Don't use a Single Review Activity
*Author: Brody Kilpatrick* | *Created: March 27, 2012*

In Service Manager, we are allowed to have activities in service requests, just like Changes and Incidents. However, don't enter a single activity which is also a review activity. It will not move to "In Progress."  
  
If you create a Service Request or Service Request Template, and you decide to use activities, make sure that if you are going to have an approval (Review Activity), that you also have some type of Manual Activity associated with it. Otherwise, the workflow will run successfully, but it will not change the status from Pending. Now you are stuck, and will have to use Powershell to clean it up.  
  
Don't do this:  
  

- New Service Request

- 1 Review Activity

Do this:

- New Service Request

- 1 Review Activity
- 1 Manual Activity
