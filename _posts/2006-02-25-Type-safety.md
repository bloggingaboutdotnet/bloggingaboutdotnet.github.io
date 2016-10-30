---
layout: post
title: "Type safety"
date: 2006-02-25T13:24:00Z
modtime: 2006-02-25T13:24:00Z
pubdate: 2006-02-25T13:24:00Z
originalurl: "http://bloggingabout.net/blogs/rene/default.aspx/blogs/rene/archive/2006/02/25/11167.aspx"
---


<p>I had a look at the following code:</p><p><font color="#008080" size="2">cmbSomeComboBox.DataSource = MySource;
<br /><font size="2">cmbSomeComboBox.DisplayMember = <font color="#800000" size="2">"Description"</font><font size="2">;
<br /><font size="2">cmbSomeComboBox.ValueMember = <font color="#800000" size="2">"Code"</font><font size="2">;</font></font></font></font></font></p><p><font color="#008080" size="2"><font size="2"><font color="#000000">This assumes that MySource contains an list of objects MyObject with property "Description" and "Code".</font></font></font></p><p><font color="#000000">If for some reason you change the MyObject class (get rid of either Code or description) your code will still compile fine but it will throw a argumentException on runtime. It is nicer to have a compile time error..
<br />
The only solution I could come up with is this:</font></p><p><font color="#008080" size="2">Debug<font color="#000000" size="2">.Assert(</font><font color="#0000FF" size="2">new</font><font color="#008080" size="2">MyObject</font><font color="#000000" size="2">().Description.GetType() !=</font><font color="#0000FF" size="2">null</font><font color="#000000" size="2">,</font><font color="#800000" size="2">"Description property gone?"</font><font size="2"><font color="#000000">);</font></font></font></p><p><font size="2"><font color="#000000">This will give a compile time error...for now I didn't find a nicer way to come up with compile time binding without a lot of hassle.</font></font></p>
