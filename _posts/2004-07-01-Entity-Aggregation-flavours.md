---
layout: post
title: "Entity Aggregation flavours"
date: 2004-07-01T20:47:00Z
modtime: 2004-07-01T20:47:00Z
pubdate: 2004-07-01T20:47:00Z
originalurl: "http://bloggingabout.net/blogs/rene/default.aspx/blogs/rene/archive/2004/07/01/1121.aspx"
---


<p>This post is about the session I attended which was hosted by Maarten Mullender. As I wrote earlier, Maarten has a presentation skill in Dutch which makes it rather unpleasant to listen to him for more than 30 minutes. Luckily this talk was in english and Maarten did a great job in explaining his dreams to us. Entity aggregation is needed if in your SOA you come up with a cannonical respresentation of an lets say Customer that differs from the underlying business entities holding the actual Customer (ie SAP, CRM and Outlook)</p><p>Entity aggregation is responsible for mapping the customer representations of the different under lying business entities to the cannonical form. It is also rsponsible for any update, insert and delete. Maarten thinks that you should prevent too much CRUD like operations on a service. For example in Outlook calendar when you add an appoint on the same time an other appoint is already scheduled you get two appointmens on the same time, the existing appointment is not edited. Try to design/(do some BPR) to get this type of fucntionality.</p><p>Entity aggregation services come in straight pass trough flavour which take care of the crud operation immediately and an caching flavour where the crud ops are cached in the service and reads can then be served from the cache and updates can be stored and deliverd to the back-end systems at a later moment.</p><p>Maarten has done an article on keymapping and I will certainly read it.</p>
