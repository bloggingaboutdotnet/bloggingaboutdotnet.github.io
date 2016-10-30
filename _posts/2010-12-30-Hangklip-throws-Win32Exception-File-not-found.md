---
layout: post
title: "Hangklip throws Win32Exception: File not found"
date: 2010-12-30T15:45:00Z
modtime: 2010-12-30T15:45:00Z
pubdate: 2010-12-30T15:45:00Z
originalurl: "http://bloggingabout.net/blogs/rene/default.aspx/blogs/rene/archive/2010/12/30/hangklip-throws-win32exception-file-not-found.aspx"
---


<p>At least one commenter wondered if this still is a developers blog or just a personal whereabouts chitchat. It is both. And although this blog is going to present some code at last I wondered for a moment what would happen if I obfuscated all the logical names in the code to famous tourists attraction like <a href="http://en.wikipedia.org/wiki/Cape_Town" target="_blank">Cape Town</a>, <a href="http://en.wikipedia.org/wiki/Tafelberg,_South_Africa" target="_blank">Tafelberg</a>, <a href="http://en.wikipedia.org/wiki/Cape_Agulhas" target="_blank">Cape Alguhas</a> and the majestic <a href="http://en.wikipedia.org/wiki/False_Bay" target="_blank">Hangklip</a>. Maybe three blogs of chitchat is enough for most readers, pity.</p><p>On a small project last week I encountered an issue when I needed to launch an windows application. In my mind the solution was pretty simple, instantiate the <a href="http://msdn.microsoft.com/en-us/library/system.diagnostics.process.aspx" target="_blank">Process</a> class, supply it with a <a href="http://msdn.microsoft.com/en-us/library/bfbyhds5.aspx" target="_blank">ProcessStartInfo</a>, call Start(); and you're in business.</p><p>The first test run however ended in an Win32 Exception: File not Found. I'm still partly human so mistakes come natural. The command prompt was quickly started and the dir "c:\Windows\system32\filename.exe" showed immediately….that the file did exist, it even had a size, and when I ran the file it actually showed what I expected.</p><p>Puzzled I reverted back to the code, verifying that I was indeed providing the correct path and that ProcessStartInfo had the correct settings and arguments. Google pointed out that there are issues when you want to run a 64bits exe from a 32bits process. Could that be the case: I was on Vista x64, building a .Net 4.0 commandline app, targeting x86 from within Visual Studio 2010. The executable I wanted to launch was the Backup and Restore app, sdclt.exe. On disk it resided in the Environment.SystemDirectory ("c:\windows\system32"). The reason it refused to start/to be found lay in the Windows on Windows functionality. WOW is a concept that I first encountered in the form of <a href="http://en.wikipedia.org/wiki/Win32s" target="_blank">win32s</a> (yes, I'm that old). It automagically let you run apps compiled for an other number of bits on the same box. One of the features of WOW is to redirect file access in system32 to SysWow64, without the user ever knowing it. Great, and now disable that.</p><p>You could recompile to x64 and you're done. If that is not possible you might find this little class help full. It tries to determine as good as it can if the executable you want to launch is a 64bits PE file. I use the <a href="http://www.pinvoke.net/default.aspx/kernel32/GetBinaryType.html" target="_blank">GetBinaryType</a> api for that. Once that is determined a call to <a href="http://www.pinvoke.net/default.aspx/kernel32/Wow64DisableWow64FsRedirection.html" target="_blank">Wow64DisableWow64FsRedirection</a> (the microsoft employee responsible for coming up with decent short names for API's was an holiday) disables redirection and therefor a call to Process.Start() will succeed.</p><p>For convenience (or over engineered) I extended the Process class and abstracted the disabling and enabling in a helper class that implements IDisposable. Leveraging this class becomes as easy as this:</p><pre xml:space="preserve" class="csharpcode">
ProcessStartInfo restoreBackupProcess = <span class="kwrd">new</span> ProcessStartInfo(
                Path.Combine(Environment.SystemDirectory, <span class="str">"sdclt.exe"</span>), 
                <span class="str">@"/RESTOREPAGE"</span>);

ProcessEx pe = <span class="kwrd">new</span> ProcessEx();
pe.StartInfo = restoreBackupProcess;
pe.Start();
pe.WaitForExit();
</pre><p>The Start(); method will always succeed either if the windows executable is 64bits or 32bits.</p><p>The new Start() method looks like this:</p><pre xml:space="preserve" class="csharpcode">
    <span class="rem">/// &lt;summary&gt;</span>
    <span class="rem">/// ProcessEx extends Process to Start 64bits system exe's from a 32 bits process</span>
    <span class="rem">/// &lt;/summary&gt;</span>
    <span class="kwrd">public</span> <span class="kwrd">class</span> ProcessEx:Process
    {
        <span class="rem">/// &lt;summary&gt;</span>
        <span class="rem">/// Overriden Start method</span>
        <span class="rem">/// &lt;/summary&gt;</span>
        <span class="kwrd">new</span> <span class="kwrd">public</span> <span class="kwrd">void</span> Start()
        {
            <span class="rem">// we can only start if we have a StartInfo</span>
            <span class="kwrd">if</span> (<span class="kwrd">this</span>.StartInfo != <span class="kwrd">null</span>)
            {
                <span class="rem">// Disable WOW64 redirection if needed</span>
                <span class="kwrd">using</span> (<span class="kwrd">new</span> DisableWOW64(<span class="kwrd">base</span>.StartInfo.FileName))
                {
                    <span class="kwrd">base</span>.Start(); <span class="rem">// real start</span>
                }
            }
        }
       ....
     }
</pre><p>Have a look at the <a href="/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/rene.src/4380.ProcessEx.zip" target="_blank">full source code</a> for the DisableWOW64 implementation.</p>
