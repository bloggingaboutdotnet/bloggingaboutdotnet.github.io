---
layout: post
title: "XML in Indigo, forget that, XML in WCF"
date: 2005-09-16T18:51:00Z
modtime: 2005-09-16T18:51:00Z
pubdate: 2005-09-16T18:51:00Z
originalurl: "http://bloggingabout.net/blogs/rene/default.aspx/blogs/rene/archive/2005/09/16/9455.aspx"
---


<p>On the end of a PDC day you have to pick your session carefully to prevent falling a sleep. As Don Box pounded on me on the previous session to go to see Doug Purdy I decided to follow his advice. Nice to see that besides Don himself als Steve Swartz came to Doug his presentation.</p><p>Doug comes across loud and clear. And as a Lead Program Manager for Indi..WCF he has a lot of knowledge on the XML messaging bits and bytes. As a dare devil he used Windows Vista and a late build of VS2005 to do his demo's. And suprisingly they worked... except for F5. After the build was finished and the debugger should start it showed a msgbox with 'unknown handle'. Doug called to Redmond but a developer told him 'dunno'. Doug's response:'If you dunno how will our customers now'.</p><p>Now onto the XML in In..WCF. The WCF team did rebuild the XML stack primarily focussing on XML needed for messaging. The reason they had to do that was one of performance. The System.XML is great for standarized, general purpose XML but is overloaded for just messaging.My own experience is that XML serializing just creates a lot of bytes on the wire. And it still does until you use the new WCF concept of an XMLDictionary. The message size got reduced dramatically. Only requirement: both ends need to talk WCF. As Doug stated: If we talk Microsoft, we are pretty quick, if we talk with some one else, where not as quick.</p><p>The different ways of leveraging the infrastructure by adding custom readers, writers and validators makes the possible ways of exchanging messages very flexible.</p>
