---
layout: post
title: "CSV export "
date: 2009-09-06T19:12:00Z
modtime: 2009-09-06T19:12:00Z
pubdate: 2009-09-06T19:12:00Z
originalurl: "http://bloggingabout.net/blogs/rene/default.aspx/blogs/rene/archive/2009/09/06/csv-export.aspx"
---


<p>I had to export a bunch of data in a CSV file. Turned out that at the receiving end the Perl script didn't want to eat my export. Luckily there is a standard for CSV files here and as strong Microsoft follower I adhere to the standards. Read up on RFC 4180 <a href="http://www.rfc-editor.org/rfc/rfc4180.txt" target="_blank">here</a>.</p><p>To make life a little easier in getting all fields of my class String.Formatted the correct way I hammered down a CustomFormatter to format output according RFC4180. Your WriteLines can now look like this where the CsvFormatter takes care of adding double-quotes for the fields that matter:</p><p>&lt;snip&gt;
<br />
// boring code omitted
<br />
sww.WriteLine(
<br />
String.Format(
<br />
new CsvFormatter(),
<br />
@"{0},{1},{2},{3},{4},{5},{6},{7},{8},{9},{10},{11},{12}",
<br />
topic.id,
<br />
topic.title,
<br />
topic.description,
<br />
topic.createdBy,
<br />
topic.createdOn,
<br />
topic.lastModifiedBy,
<br />
topic.lastModifiedOn,
<br />
postitem.id,
<br />
postitem.title,
<br />
postitem.content,
<br />
postitem.createdByUserName,
<br />
postitem.createdDate,
<br />
postitem.replyToId)
<br />
);</p><p>&lt;/snip&gt;</p><p>The code for the CsvFormatter is here:</p><p>public class CsvFormatter:IFormatProvider, ICustomFormatter
<br />
{
<br />
#region IFormatProvider Members
<br />
public object GetFormat(Type formatType)
<br />
{
<br />
if (formatType == typeof(ICustomFormatter))
<br />
{
<br />
return this;
<br />
}
<br />
else
<br />
{
<br />
return null;
<br />
}
<br />
}</p><p>#endregion</p><p>#region ICustomFormatter Members</p><p>public string Format(string format, object arg, IFormatProvider formatProvider)
<br />
{
<br />
if (arg == null)
<br />
{
<br />
return "";
<br />
}
<br />
string result = "";
<br />
if (format == null)
<br />
{
<br />
result = arg.ToString();
<br />
}
<br />
else
<br />
{
<br />
result = String.Format(format, arg); // this part is never tested...
<br />
}
<br />
char[] escapeChars = new char[] {'"','\n','\r', ',' };
<br />
if (result.IndexOfAny(escapeChars) &gt; -1)
<br />
{
<br />
result = result.Replace("\"", "\"\"");
<br />
result = "\"" + result + "\"";
<br />
}
<br /><br />
return result;
<br />
}</p><p>#endregion
<br />
}</p><p>Obvious more ingenious ways are possible especially if you need more flexibility but this one did the trick for me (and Google didn't bring me a quick answer).</p>
