---
layout: post
title: "Is my type too safe?"
date: 2006-05-01T18:26:00Z
modtime: 2006-05-01T18:26:00Z
pubdate: 2006-05-01T18:26:00Z
originalurl: "http://bloggingabout.net/blogs/rene/default.aspx/blogs/rene/archive/2006/05/01/12107.aspx"
---


<p>Today I struggled with a minor problem which made me wonder: "Is this type too safe?".</p><p>So here is the code frament and the nice popup-box:</p><p><img src="/UserFiles/Rene Schrieken/Image/bug2.gif" alt="" height="342" width="813" /></p><p>The first inspection with the debugger showed that this.Value was really a DateTime, and value.Value was also really a DateTime. Why are the DateTime types complaining about an invalid Value.</p><p>As value is really a Nullable&lt;DateTime&gt; a small thought popped up that maybe a DateTime created from a Nullable type is something completely different. But value.Value looked ok: The debugger was also convinced that it was a DateTime.</p><p>So then what is this.Value doing in its setter? Ooh yeah, hold on, this class is inherited from DateTimePicker. The Value property is its strongly typed member that sets or get the date to be presented in the DateTimePicker control. It shouldn't be that it can't represent a valid DateTime?</p><p>It does. It has a business rule of its own and checks the DateTime in its setter code against DateTimePicker.MinDate and DateTimePicker.MaxDate. Trying to store a new DateTime() into the Value property of the DateTimePicker is not going to succeed because the default MinDate is 1-1-1753 (which is the start of the Gregorian calendar?) . Luckily MinDate and Maxdate are runtime setable so you can fiddle with that, I opted for some coding to bypass the Value setter of the base class and set the Checked to false when my incoming Date is below the base.MinDate.</p><p>(UPDATE: Some comments indicated that this architect made a mistake :-) )</p>
