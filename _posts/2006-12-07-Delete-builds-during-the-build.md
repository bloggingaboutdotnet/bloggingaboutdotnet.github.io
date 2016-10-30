---
layout: post
title: "Delete builds during the build"
date: 2006-12-07T12:06:00Z
modtime: 2006-12-07T12:06:00Z
pubdate: 2006-12-07T12:06:00Z
originalurl: "http://bloggingabout.net/blogs/rene/default.aspx/blogs/rene/archive/2006/12/07/Delete-builds-during-the-build.aspx"
---


<p>Now that is an intriguing title. But if you are using Team Foundation Server and especially Team Build you know where I'm talking about.</p><p>If you use the team build as the heart beat of your project and therefore build daily (or nightly if you're in the java camp) you end up with a large list of Builds. Most of them are probably failed (just kidding), some of them have a build quality of unexamined and most of them are so old you probalby never want to touch or install any of it.</p><p>So you want to get rid of the old builds. Luckily you can. Microsoft provided us with a nice command line utility called <a href="http://msdn2.microsoft.com/en-us/library/aa337656(VS.80).aspx" target="_blank">tfsbuild</a> which enable you to remove completed builds. Just provide a projectname and a buildnumber. Repeat 400 times until all unwanted builds are deleted.</p><p>There must be an easier way. I already found <a href="http://notgartner.wordpress.com/2006/05/22/release-build-clean-up-service-for-team-foundation-server/" target="_blank">this</a> solution but in our development shop I'm not the owner of the buildbox. Handing over the control of which builds to delete to an uncontrolled service didn't appealed to me much.</p><p>So reinvent the wheel to come up with a for me more suitable solution. Create a msbuild Task that deletes builds based on criteria that are provided in the build script. In the download you'll find the source (so you can see that there is no virus in it) of the build task that enables you to delete builds from the BuildStore. If you place the compiled assembly in the same source control folder where tfsbuild.proj resides you can start configuring.</p><p>In your tfsbuild.proj script do this:</p><p><span style="font-size: x-small; font-family: Courier New;">&lt;UsingTask AssemblyFile="DeleteBuildEasy.dll" TaskName="DeleteBuilds" /&gt;</span></p><p><span style="font-size: x-small; font-family: Courier New;">&lt;Target Name="AfterEndToEndIteration"&gt;
<br />
&lt;DeleteBuilds
<br /></span><span style="font-size: x-small; font-family: Courier New;">TeamServer="[your tfs server]" &lt;!-- tfs:8080 --&gt;
<br />
TeamProject="$(TeamProject)"
<br />
BuildType="$(BuildType)"
<br />
Status="Failed"
<br />
Quality="Unexamined"</span><span><br /><span style="font-size: x-small; font-family: Courier New;">Days="20" /&gt;
<br />
&lt;/Target&gt;</span></span></p><p>Check-in all your changes, kick-off your build and you'll see the builds disappear as snow for the sun. Having the Days as selection mechanism quarantees that you do not have to modify the script day after day. If you come up with a good set of thresholds you can leave it running without any aftercare.</p><p>Best of all is that not only you're builds from the BuildStore will disappaer but also the files on you're buildserver will be removed, so it really cleans up.</p><p>This attached source code shows the basic mechanism. I have a more advanced version available. That one will also be able to run as service (as mentioned earlier) and has a more sophisticated query mechanism. Just contact me if you're intereseted in that version.</p><p>[UPDATE]</p><p>When you use this task on a TFS2008 based server keep in mind that this task DOESN'T honor the KeepForever=true property. As a matter of fact it will happily remove the build.</p><p>Source code is <a href="/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/rene.src/DeleteBuildsEasy_2D00_Source.zip" title="source code">here</a></p>
