---
layout: post
title: "DPC Horror"
date: 2004-07-04T17:40:00Z
modtime: 2004-07-04T17:40:00Z
pubdate: 2004-07-04T17:40:00Z
originalurl: "http://bloggingabout.net/blogs/rene/default.aspx/blogs/rene/archive/2004/07/04/1143.aspx"
---


<p>Did you ever wonder what Deferred Procedure Calls where?</p><p>Me neither. But the horror full thing was that in PerfMon I saw that DPCsQueued/Sec where rocketing causing mouse and keyboard events to get lost. I ran every spyware tool I could find and McAfee has seen al my files by now. I've reinstalled every patch from WindowsUpdate and downloaded all tools from <a href="http://www.sysinternals.com" target="_blank" title="Sysinternals">Sysinternals</a>. ProcExplorer just showed DPC's but I had no idea which part of my system was generating them. I just want to understand what is going on.</p><p>This article on <a href="http://msdn.microsoft.com/library/default.asp?url=/library/en-us/kmarch/hh/kmarch/Intrupts_92bb7010-416b-46b7-804a-489578227b70.xml.asp" target="_blank" title="DPC Objects">MSDN</a> directed me to the solution. As I understand now DPCs are called by device driver (C++) hackers which enables them to quickly handle an interrupt but postpone the more heavy duty processing to a later moment. I think this makes that our PC's keep on working while we type, mouse move, network, print etc.</p><p>Ok, so some Hardware is probably sending interrupts and its driver is putting a lot of DPCs in the queue to get handled. But which part of my system is doing that. Nothing is broken....except for my battery. Can a battery generate interrupts? I find it hard to believe. Until I remove the battery from my laptop. The DPCs disappear! I must have overlooked something: put back the faulty battery, there are my DPCs again and my sticky mouse and my erratic keyboard. Those guys from HP must be kidding me. Remove the battery et voila laptop works just fine.</p><p>Is there an DPC debugger around somewhere?</p>
