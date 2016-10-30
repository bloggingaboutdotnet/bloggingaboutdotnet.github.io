---
layout: post
title: "HttpModule Code"
date: 2004-02-18T07:34:00Z
modtime: 2004-02-18T07:34:00Z
pubdate: 2004-02-18T07:34:00Z
originalurl: "http://bloggingabout.net/blogs/rene/default.aspx/blogs/rene/archive/2004/02/18/388.aspx"
---


<p>!HttpModule Code</p><p>Hier is hopelijk dan de code. Compileren en de dll in de bin van de website plaatsen.</p><p>Install door in de web.config van de te reducen website de httpmodule toe te voegen.
<br /><code>&lt;httpModules&gt;
<br />
&lt;add name="Reduce" type="Reduce.Reduce,Reduce" /&gt;
<br />
&lt;/httpModules&gt;
<br /></code></p><p><code><br />
using System;
<br />
using System.IO;
<br />
using System.Web;</code></p><p>namespace Reduce
<br />
{
<br />
/// <summary><br />
/// Reduce module cleans Http response stream from meaningless chars.
<br />
///</summary><br />
public class Reduce:IHttpModule
<br />
{
<br />
private HttpApplication mappCurrent; // The application</p><p>/// <summary><br />
/// Instantiates Reduce
<br />
///</summary><br />
public Reduce()
<br />
{
<br />
// no init
<br />
}</p><p>/// <summary><br />
/// Called when the client request start
<br />
///</summary><br />
/// <remarks><br />
/// Hier kunnen nog andere events gekoppeld worden..
<br />
///</remarks><br />
///
<br />
The application public void Init(HttpApplication pappCtx)
<br />
{
<br />
pappCtx.PreSendRequestContent += new EventHandler(PreSendRequestContent);
<br />
pappCtx.PreRequestHandlerExecute += new EventHandler(PreRequestHandlerExecute);
<br />
mappCurrent = pappCtx;</p><p>}</p><p>/// <summary><br />
/// Called for cleaning up
<br />
///</summary><br />
public void Dispose()
<br />
{
<br />
mappCurrent=null;
<br />
}</p><p>/// <summary><br />
/// Event called when request is send to client
<br />
///</summary><br />
///
<br />
Caller ///
<br />
Empty argument private void PreSendRequestContent(object sender, EventArgs e)
<br />
{
<br />
mappCurrent.Response.Write("<!-- testing -->");
<br />
}</p><p>/// <summary><br />
/// Event called when Page is handeld
<br />
///</summary><br />
///
<br />
Caller ///
<br />
Empty argument private void PreRequestHandlerExecute(object sender, EventArgs e)
<br />
{
<br />
mappCurrent.Response.Filter = new HttpStreamFilter(mappCurrent.Response.Filter);
<br />
mappCurrent.Response.Write("<!-- testing -->");
<br />
}
<br />
}</p><p>/// <summary><br />
/// De
<br />
///</summary><br />
public class HttpStreamFilter : Stream
<br />
{
<br />
private Stream mseaSink;
<br />
private long mlngPosition;
<br />
private Token menmToken;</p><p>private enum Token
<br />
{
<br />
None,
<br />
Tag,
<br />
Comment,
<br />
Script,
<br />
Space
<br />
}</p><p>private enum Space
<br />
{
<br />
None,
<br />
CR,
<br />
LF,
<br />
Tab,
<br />
Space,
<br />
Nbsp
<br />
}</p><p>public HttpStreamFilter(Stream pseaSink)
<br />
{
<br />
menmToken = Token.None;
<br />
mseaSink = pseaSink;
<br />
}</p><p>// The following members of Stream must be overriden.
<br />
public override bool CanRead
<br />
{
<br />
get { return true; }
<br />
}</p><p>public override bool CanSeek
<br />
{
<br />
get { return true; }
<br />
}</p><p>public override bool CanWrite
<br />
{
<br />
get { return true; }
<br />
}</p><p>public override long Length
<br />
{
<br />
get { return 0; }
<br />
}</p><p>public override long Position
<br />
{
<br />
get { return mlngPosition; }
<br />
set { mlngPosition = value; }
<br />
}</p><p>public override long Seek(long plngOffset, System.IO.SeekOrigin pseoDirection)
<br />
{
<br />
return mseaSink.Seek(plngOffset, pseoDirection);
<br />
}</p><p>public override void SetLength(long plngLength)
<br />
{
<br />
mseaSink.SetLength(plngLength);
<br />
}</p><p>public override void Close()
<br />
{
<br />
mseaSink.Close();
<br />
}</p><p>public override void Flush()
<br />
{
<br />
mseaSink.Flush();
<br />
}</p><p>public override int Read(byte[] buffer, int offset, int count)
<br />
{
<br />
return mseaSink.Read(buffer, offset, count);
<br />
}</p><p>// The Write method actually does the filtering.
<br />
public override void Write(byte[] parrBuffer, int pintOffset, int pintCount)
<br />
{</p><p>// de bedoeling is om alle onnodige space en onnodige comments te filteren.
<br />
// probleem is dat dit een stream is, maw er komen array of byte binnen
<br />
// maar dat hoeft niet de hele stream te zijn...
<br />
// We moeten dus op basis van de vorige bytes en de nieuwe byte beslissen
<br />
// wat er moet gebeuren:
<br />
// Verwijder zonder check:
<br />
// Cr (behalve in een script Comment), Lf, Tab : done
<br />
// space: verwijderen als het vorige char een space , Cr,Lf of Tab was: done
<br />
// space na een &lt; verwijderen : done
<br />
// space voor een &gt; verwijderen
<br />
// Comments buiten een script block verwijderen
<br />
// dubble constructies verwijderen
<br />
// alleen voor HTML graag...
<br />
byte[] larrData = new byte[pintCount];</p><p>int lintDataPos = 0;
<br />
// copy only chars not being space, tab, cr, lf
<br />
for (int i = 0; i &lt; pintCount; i++)
<br />
{
<br />
char c = (char) parrBuffer[pintOffset+i];</p><p>switch(c)
<br />
{
<br />
case '\t':
<br />
case '\r':
<br />
case '\n':
<br />
if (menmToken==Token.Tag)
<br />
{
<br />
menmToken = Token.Space;
<br />
}
<br />
break;
<br />
case ' ':
<br />
if (menmToken == Token.Space)
<br />
{
<br />
//vorige teken was ook een space
<br />
}
<br />
else
<br />
{
<br />
if (menmToken ==Token.Tag)
<br />
{
<br />
menmToken = Token.Space;
<br />
}
<br />
else
<br />
{
<br />
menmToken = Token.Space;
<br />
larrData[lintDataPos] = parrBuffer[pintOffset+i];
<br />
++lintDataPos;
<br />
}
<br />
}
<br />
break;
<br />
case '&lt;':
<br />
menmToken = Token.Tag;
<br />
larrData[lintDataPos] = parrBuffer[pintOffset+i];
<br />
++lintDataPos;
<br />
break;
<br />
default:
<br />
menmToken = Token.None;
<br />
larrData[lintDataPos] = parrBuffer[pintOffset+i];
<br />
++lintDataPos;
<br />
break;
<br />
}
<br />
}
<br />
// write out only valid bytes in larrData (hopefully less than pintCount)
<br />
mseaSink.Write(larrData, 0, lintDataPos);
<br />
}
<br />
}
<br />
}</p>
