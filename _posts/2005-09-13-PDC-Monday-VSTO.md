---
layout: post
title: "PDC Monday: VSTO"
date: 2005-09-13T20:05:00Z
modtime: 2005-09-13T20:05:00Z
pubdate: 2005-09-13T20:05:00Z
originalurl: "http://bloggingabout.net/blogs/rene/default.aspx/blogs/rene/archive/2005/09/13/9350.aspx"
---


<p>PDC: VSTO 2005</p><p>I did a talk last thursday on VSTO and VS2005 and today I'm briefed by 4 microsoft employees responsible for the VSTO product. It is clear that MS move away from per prodcut programmability (like the VBA-model was) to a more overall common program model is a long and winding road.
<br />
You can already see some great enhancements, like a better integrated object model but you still see some rough edges. The handling for things like Host items and Smarttags are just diffeent in Word or Excel or simply not fully extended yet. It will take the release of Office12 to get this model further enhancent and integrated.</p><p>The smarttag development will be easier but only on the per document basis, not application wide.</p><p>For security sake the VSTO team did remove a lot of full-trust permissions and for real deployment you have to fiddle around with caspol a lot. The nice thing was that even Mischa (a developer from the VSTO team) didn't get it clear. he admitted the security model sometimes fires back on him also.</p><p>For data and databinding sceanrio's we get support in excel and word like we are used to in 'traditional' winforms development. We recognize DataSources, databinders and nice ListControl that does the tricks.</p><p>The last demo we got was about mobile webservices. It enbales you to get into your outlook data (or better said, the exchange server) on your mobile by using SMS.</p><p>During Q&amp;A a good question was asked: how does IBF and VSTO and PAI goes together. PIA and VSTO could (and should) be used together. The IBF is another team, the VSTO is communicating with them but integrated: not really.</p><p>Overall VSTO looks good and sure is usable and deployable now for enterprise apps.
<br /></p>
