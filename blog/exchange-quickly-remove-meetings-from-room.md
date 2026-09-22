---
layout: default
permalink: /blog/exchange-quickly-remove-meetings-from-room
---
# Exchange - Quickly Remove Meetings From Room
*Author: Brody Kilpatrick* | *Created: June 25, 2019*

![](/assets/blog/pexels-photo-1893424.jpeg)

Photo by rawpixel.com on [Pexels.com](https://www.pexels.com/photo/macbook-pro-turned-on-displaying-schedule-on-table-1893424/)

This is a quick one-liner in Exchange Shell. It will remove any meeting created by a specific person. Usually, this is required when a person leaves the company and had setup a recurring meeting.

NOTE: If the user still exists, it would be best cancel the meeting from the user's mailbox and send out the cancellation notice. However, if the user has been deleted, or the cancellation method is not possible, this is a simple alternative.

```powershell
Get-Mailbox "MyMeetingRoom" | Search-Mailbox -SearchQuery 'Kind:Meetings AND From:email@domain.com' -DeleteContent –Force
```
