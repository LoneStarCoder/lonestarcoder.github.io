---
layout: default
permalink: /blog/scsm-2012-self-service-portal-service-category-color-customization
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# SCSM 2012: Self Service Portal Service category color customization
*Author: Brody Kilpatrick* | *Created: September 3, 2013*

The is a great solution
and worthy of a repost.

<http://www.expiscornovus.com/2012/05/06/scsm2012-self-service-portal-service-category-color-customization/>

SCSM 2012: Self Service Portal Service category color
customization

[1 Reply](http://www.expiscornovus.com/2012/05/06/scsm2012-self-service-portal-service-category-color-customization/#comments "Comment on SCSM 2012: Self Service Portal Service category color customization")

Lately I’ve been
wandering a bit more on the [Technet Forums](http://social.technet.microsoft.com/Forums/en-us/categories/ "Technet Forums"), this
has been pretty useful. Friday I came across this [thread](http://social.technet.microsoft.com/Forums/en-US/portals/thread/fc2ca534-022c-46ab-8472-02edebc560cd "thread") from [Bart Timmermans](http://www.bart-timmermans.nl/ "Bart Timmermans") about customization in the Self
Service Portal of the product System Center Service Manager 2012. He asked if
it was possible to adjust the styling of Service category headers. Of course I
accepted the challenge.

*Analysis*

The Self Service Portal
is a solution on SharePoint 2010 which deploys some web parts. Like Travis
Wright described in his latest [Self Service Portal blogpost](http://blogs.technet.com/b/servicemanager/archive/2012/05/04/faq-why-is-my-self-service-portal-service-catalog-blank.aspx "Self Service Portal blogpost") those
web parts use Silverlight .xap files. After some .NET Reflector work on the
Portal.BasicResources DLL. I found that a lot of the color styling for the
portal is being done by using brushes.

*Brushes*

A brush is an [Silverlight object](http://msdn.microsoft.com/en-us/library/cc189003(v=vs.95).aspx "Silverlight brush object") which
can be used to paint for example solid colors or linear gradient. In that DLL I
found a .xaml file which defined some solidcolorbrushes and they had a key. In
Silverlight they use a 8 digit notation for the color, this is a [RGBA](http://en.wikipedia.org/wiki/RGBA_color_space "RGBA") value.

*Settings.xml*

The Self Service Portal
actually has a settings.xml file which can be used to define some basic
settings. I noticed it also had some setting keys for colors. This triggered me
to add a key for one of the brushes, ExpanderHeaderBgBrush. My attempt worked.
After adjusting the Settings.xml and clearing my browsers cache I saw a new
green color!

*Solution*

1. Go to
C:\inetpub\wwwroot\System Center Service Manager Portal\ContentHost\Clientbin
(or another location if your installation directory was different

2. Open the Settings.xml
file

3. Add a setting key for
ExpanderHeaderBgBrush with your desired RGBA color:

Happy customizing!
