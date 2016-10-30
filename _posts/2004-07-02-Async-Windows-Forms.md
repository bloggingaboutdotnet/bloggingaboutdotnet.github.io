---
layout: post
title: "Async Windows Forms"
date: 2004-07-02T14:16:00Z
modtime: 2004-07-02T14:16:00Z
pubdate: 2004-07-02T14:16:00Z
originalurl: "http://bloggingabout.net/blogs/rene/default.aspx/blogs/rene/archive/2004/07/02/1134.aspx"
---


<p>This was a great technical session by Juval Lowy on threads in WinForms. Great explanation on how the managed threads are coupled to those darn messageloop from win 3.1. Juval showed us the after some time rather laughable complexity you need to odo over and over again if you want a wokerthread to update ui-controls on the ui-thread. In Clr2.0 there will be no breaking changes regarding this topic (so everything what works now in 1.1) except for debugging. If you are debugging your app and you actually access an ui-control from an other thread than the ui-thread you will get an exception. During run-time it might work (if it worked in 1.1 it will in 2.0, if it broke in 1.1 it will in 2.0).</p><p>In 2.0 the WinForms team created an new control: BackGroundWorker which hides all thread jumping functionality needed. The good news is: Juval created BackGroundWorker for 1.1! So this 2.0 class is now available for all of us. Great! Juval also showed some other clever ways of hiding the complexity of thread jumping by subclassing controls which can also become very usefull.</p><p>Tnx Juval!</p><p><a href="http://www.idesign.net/idesign/DesktopDefault.aspx">http://www.idesign.net/idesign/DesktopDefault.aspx</a> (see downloads)</p>
