---
layout: default
permalink: /blog/orchestrator-misrepresented-and-misunderstood
---
[Home](https://lonestarcoder.github.io) | [Blog](https://lonestarcoder.github.io/blog/home) | [Articles](https://lonestarcoder.github.io/articles)
# Orchestrator - Misrepresented and Misunderstood
*Author: Brody Kilpatrick* | *Created: August 1, 2014*

**Purpose:**

1.       Talk about the misrepresentation of
Orchestrator.

2.       Point out some things about
Orchestrator that people don't quite understand.

3.       Convince people to give Orchestrator a
try.

**Summary:**

System Center Orchestrator is a
product in the System Center Suite. If you have heard of Orchestrator, you have
probably heard it along with other buzzwords like "automation" and
"cloud." This text is not about writing a cool runbook, automation,
pitching cloud services, or discussing how System Center can solve all of the
world's IT problems (it can though). I want to discuss a misconstrued view of
Orchestrator that I continue to see over and over, which I think prevents IT
Organizations from using it or trying it out.

I think that Orchestrator is both *misrepresented* and *misunderstood*.

**What was *my* original perception of Orchestrator?**

As an Operations Manager and Service
Manager guy, I knew that I could build automation
for nearly anything without ever relying on any products outside tho~~e~~se two systems.
I have been using Operations Manager to automate IT tasks, and perform cleanup
as a result of alerts for years.

Service Manager provides the tools to
take a ticket system and turn it into an automation machine using the authoring
console, Powershell, and the native flexibility of the system.

So when Microsoft began touting
Orchestrator, my first reaction was, “why in the world
do I need that?” The simple answer is, I don't…

As a matter of fact, I have never *had* to use Orchestrator for anything…ever.
I still haven't found a task, workflow, process, or anything else I couldn't
complete by using the other System Center products that were already
implemented, even when they were communicating outside the realm of Microsoft.
(Thanks PowerShell!)

So, in a sense, Orchestrator almost
seems like a novelty - an unnecessary System Center Product…**but it's not.**

**Why use a complex system when a simple
system is available?**

Let's take a look at Operations
Manager and Service Manager. We know they both can do just about anything that
we want in terms of process and automation. When you get down deep, both
products can get somewhat complicated. They both run complex operations on the
back-end, which allows it to use a simple front-end. I mean, have you looked at
the databases or stored procedures? My point is, there is a lot going on that
we don't see. Even without much of anything configured, the systems are
churning away.

The beauty in Orchestrator is its **back-end simplicity**.
What is Orchestrator doing? Nothing. A little maintenance here or there,
checking schedules, checking the clock, etc. What are the others doing?
Constantly performing complex operations even at base load. And what are they
doing when you begin automating your processes, performing remediation, or
waiting for a criteria match? Even more. So much in fact, you can easily drop
40 GB of ram or more with several separated disks on the SQL server to maintain
an acceptable level of performance while the systems are performing their
operations.

So why are so many people taking such
complex operations and stuffing them full of more complex operations creating a
memory and disk eater, when we have this nice, little calculator waiting for
its command? A nice little calculator that will do anything you tell it to do,
and communicate with any machine you want to communicate with. Yet a lot of
people still don't use it.

**I think the answer is simply
misrepresentation.**

Everyone talks about all the cool,
complex things Orchestrator can do, all the systems it can talk to, and all the
integration packs that are available. It actually sounds kind of scary. I mean,
who has time to do all that?

But, at its base, Orchestrator is not
much more than a scheduling engine - a simple calculator.

"Simple" is what
Orchestrator was meant to be all along. Take a complex operation and break it
down into simple steps.

**Do this…**

o    Find something you would like to do –
such as automation, remediation, communication between 2 systems. Whatever it
is, start out with a goal.

o    Document the exact technical process
that should happen *on **paper***. If you can't write
your process on paper, you can't write it with a computer.

o    Read through Kevin Holman's Orchestrator quick start guide.

§  <http://blogs.technet.com/b/kevinholman/archive/2013/10/18/orchestrator-2012-r2-quickstart-deployment-guide.aspx>

o    Take 15 minutesand install
Orchestrator 2012 R2.

o    Take another 10 minutes to download
and install the integration packs.

§  <http://www.microsoft.com/en-us/download/details.aspx?id=39622>

o    Create a Runbook

**Remember this…**

1.       You will stumble through your first
runbook, but keep at it, it will get easier.

2.       The more complex operations you remove
from other systems and enter into Orchestrator, the easier it will be to
maintain, document, and transfer knowledge.

3.       Don't make it complicated. Don't write
a giant PowerShell script and enter it into Orchestrator; this defeats the
purpose of simplicity.  Break out your steps into multiple activities.
