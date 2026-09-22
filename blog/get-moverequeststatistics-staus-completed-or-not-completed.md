---
layout: default
permalink: /blog/get-moverequeststatistics-staus-completed-or-not-completed
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Get-MoveRequestStatistics staus completed or not completed
*Author: Brody Kilpatrick* | *Created: February 23, 2018*

Get-MoveRequestStatistics -MoveRequestQueue "mydbname" | ? {$\_.Status.Value -ne 'Completed'}  
  
In this case ever notice how "status" in the where clause does not work? Instead use $\_.Status.Value.
