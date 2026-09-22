---
layout: default
permalink: /blog/scsm-workflow-date-time-token-error-in-the-authoring-console
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# SCSM Workflow Date Time Token Error in the Authoring Console
*Author: Brody Kilpatrick* | *Created: October 25, 2011*

The Authoring Tool workflow has an option of "relative" when using date fields. However, when you check the checkbox and enter a relative token such as [now] the authoring console doesn't like it and will not save. I haven't seen anyone else complain about this, so maybe it is just my environment. Either way, I had to find a workaround. It is pretty simple.  
  
First what might happen is you are creating an on update trigger. You select a date field and select the relate checkbox.  

[![](/assets/blog/RelativeCriteria.png)](/assets/blog/RelativeCriteria.png)

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
You might be able to click okay, but later down the road when you save you might get this token error.  

[![](/assets/blog/RelativeCriteria_Error.png)](/assets/blog/RelativeCriteria_Error.png)

  
  
  
  
  
  
  
  
  
  
  
  
  
If this is happening to you, specify the expression that you want, except do not use "relative." For now, insert a static date. Save the management pack and open the xml. Go find your expression. You can see mine below.  
> *&lt;Expression&gt;**&lt;SimpleExpression&gt;**&lt;ValueExpression&gt;**&lt;Property State="Post"&gt;$Context/Property[Type='CustomSystem\_WorkItem\_Library!System.WorkItem']/ScheduledStartDate$&lt;/Property&gt;**&lt;/ValueExpression&gt;**&lt;Operator&gt;LessEqual&lt;/Operator&gt;**&lt;ValueExpression&gt;**&lt;Value&gt;2011-09-25T05:00:00&lt;/Value&gt;**&lt;/ValueExpression&gt;**&lt;/SimpleExpression&gt;**&lt;/Expression&gt;*

  
Replace*&lt;Value&gt;2011-09-25T05:00:00&lt;/Value&gt;* with *&lt;Token&gt;[now]&lt;/Token&gt;*  

That's it.
