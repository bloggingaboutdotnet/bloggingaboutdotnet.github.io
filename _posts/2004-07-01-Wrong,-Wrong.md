---
layout: post
title: "Wrong, Wrong"
date: 2004-07-01T17:42:00Z
modtime: 2004-07-01T17:42:00Z
pubdate: 2004-07-01T17:42:00Z
originalurl: "http://bloggingabout.net/blogs/rene/default.aspx/blogs/rene/archive/2004/07/01/1118.aspx"
---


<p style="MARGIN: 0cm 0cm 0pt">Blogging takes simply too long. It is difficult to write down an recollection of the past few hours and fit them in just two or three pieces of text. For this reason I ended up too late for the session on Threading Pleasures and Pitfalls. So I decided to join the session in the Auditorium with all the big names (vasters, lowy, chapel et al). But the after lunchdip kicked in and I ended up in the Forum. When I was seated I realized that some guy was telling me everything about MFC/C++ in Whidbey.</p><p style="MARGIN: 0cm 0cm 0pt"></p><p style="MARGIN: 0cm 0cm 0pt">Vague memories came floating by when MFC, messageloops and ATL where brought to my attention. The guy explained that the new runtimelibraries will not generate anymore bufferoverruns but that ypou as a developer are still able to create your own bufferoverruns,, stackoverflows and blue screen of death.</p><p style="MARGIN: 0cm 0cm 0pt"></p><p style="MARGIN: 0cm 0cm 0pt">In the compiler there is one new flag /CLR which you need to compile ypur MFC code to let it run in managed coe. And some new language features exits making it easier to integrate managed types. However simple database apps. ISAPI and parti;;y trusted apps are discouraged and need to be rewritten. Hmm that sounds wrong.</p><p style="MARGIN: 0cm 0cm 0pt"></p><p style="MARGIN: 0cm 0cm 0pt">Then if you want to use WinForms (managed Windows) you will be limited. There is no way a C++ programmer is going to interfere with handles and command loops etc. They will all be put behind a compiler (or macro) generated proxy class and the only thing they can do is call WinForms via that proxy. The poor C++/MFC guys are put behind bars.</p><p style="MARGIN: 0cm 0cm 0pt"></p><p style="MARGIN: 0cm 0cm 0pt">The worst thinmg ks still to come. The beta of vs2005 (out in 2 weeks) will lack any support for managed code. And the major advise was; stay put don't rewrite too much because we have no managed equivalent of MFC and the earliest you can expect anything like that will be with Avalon (that is in Longhorn). If you are a C++/MFC programmer you are on the wrong track.</p>