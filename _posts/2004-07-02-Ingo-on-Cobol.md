---
layout: post
title: "Ingo on Cobol"
date: 2004-07-02T20:26:00Z
modtime: 2004-07-02T20:26:00Z
pubdate: 2004-07-02T20:26:00Z
originalurl: "http://bloggingabout.net/blogs/rene/default.aspx/blogs/rene/archive/2004/07/02/1138.aspx"
---


<p>Ingo Rammer was the presenter of my last session on TechEd 2004. In his talk he showed some stunning ways (tecnically, but also architecutral correct) of loading assemblies. This open ups the oppertunity for extending your app with 'plugins'. There are already some examples of this kind of applications (FxCop for example). Ingo showed in some small demos how you can accomplish this. There are some security ramifications though: with reflection anybody can change anything eithin your app (yes that is anything also private fields)</p><p>The demo of the codecompiler was nice to see. I didn't know it was actually used during runtime on some operations (like XMLSerializer).</p><p>The use of de codedom was even more funny. The code dom gives you the oppertunity to express code in an document object model. What you actually express is MSIL. But from this dom you can generate any .Net code you want. So you can use that dom to generate VB.NET but also C#. And as Fujutsi is making an COBOL.NET compiler you can output the dom as ... COBOL.</p><p>And while Ingo stated that he didn't understand the COBOL code, I have to admit that from my brains memories where emerging... Ingo has asked if anyone is using COBOL.NET in an ASP.NET page to send him the code-behind file, please CC me.</p>
