---
layout: post
title: "BaseAddress "
date: 2006-10-07T13:48:00Z
modtime: 2006-10-07T13:48:00Z
pubdate: 2006-10-07T13:48:00Z
originalurl: "http://bloggingabout.net/blogs/rene/default.aspx/blogs/rene/archive/2006/10/07/BaseAddress-.aspx"
---


<p>It is time again for something completly off-topic.</p><p>In februari I read this <a href="http://msdn.microsoft.com/msdnmag/issues/06/02/CLRInsideOut/default.aspx" target="_blank" title="msdn">msdn</a> article regarding startup time of .Net applications. One of the problems is that every DLL you create in VS will have the same baseaddress causing the LoadImage api to relocate you're dll in memory and run a 'fixup' routine to change the memory address to match for the new location. You can read <a href="http://blogs.msdn.com/oldnewthing/archive/2004/12/17/323556.aspx" title="this blog">this blog</a> from Raymond Chen for some background.</p><p>Well that is fixed then, just add a baseaddress for each dll. On that small demo project that will work but for your multi-layer, multi assemblies projects this will be a cumbersome task. Reading through all the available information there is some extra info to take into account. First of all baseAdress start on 64K boundaries and should map into empty process memory space. Determing what empty process space is cannot be calculated during compile time. Only after all dlls are loaded in the process you can run <a href="http://www.sysinternals.com/Utilities/ListDlls.html">listdlls</a> to get a hint if rebasing was needed. Running listdlls -r [pid] will also show you that some dlls are loaded in your process which depend on the software configuration of the pc. For example the virusscanner software loads a dll in the process on my machine. This makes the problem worse. Not only is it not possible to calculate a rebase free memory space for compile time but also not for all runtime configurations.</p><p>Given this I decided to just provide a macro that sets the baseaddress for all your dll projects in your solution. I know this is far from a 100% solution but at least it should fix the rebases on our development box. The macro does not yet take into account definitely blocked addresses but on the next rainy weekend I might improve this macro.</p><p>Remember that the BaseAdress setting is bound to the build configuration. You have to run the macro at least for your Debug and Relase config.</p><p>In the attachment you find the zip file with the VS Macro.</p><div style="MARGIN-RIGHT: 0px" dir="ltr"><p style="MARGIN-RIGHT: 0px" dir="ltr"></p><pre xml:space="preserve">
<span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: black; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">Imports System<br />
Imports System.IO<br />
Imports EnvDTE<br />
Imports EnvDTE80<br />
Imports System.Diagnostics<br />
<br />
Public Module BaseAddress<br />
<br />
Sub SetBaseAddress()<br />
Const DllKind As String <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: #666666; FONT-FAMILY: Courier New; BACKGROUND-COLOR: #e4e4e4">"{FAE04EC0-301F-11D3-BF4B-00C04F79EFBC}"</span>
              <br />
Const ClassLibrary As String <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: #666666; FONT-FAMILY: Courier New; BACKGROUND-COLOR: #e4e4e4">"2"</span>
              <br />
Const RebaseTop As Long <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> &amp;H7F000000<br />
Const DefaultFileLength As Long <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> &amp;H1000<br />
Dim project As Project<br />
<br />
Dim startBase As Long <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> RebaseTop<br />
<br />
If DTE.ActiveSolutionProjects.Length <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> 0 Then<br />
Exit Sub<br />
End If<br />
<br />
For Each project In DTE.Solution.Projects<br />
<br />
' only dlls require a <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">base</span> adress<br />
If (project.Kind <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> DllKind) Then<br />
<br />
If (project.Properties.Item(<span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: #666666; FONT-FAMILY: Courier New; BACKGROUND-COLOR: #e4e4e4">"OutputType"</span>).Value <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> ClassLibrary) Then<br />
<br />
' <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">this</span> <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">is</span> the initial poor mans memory size calc<br />
' we just taken the filesize of the dll<br />
' so compile your solution before running <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">this</span> macro<br />
Dim CompleteOutputFile As String<br />
CompleteOutputFile <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> project.Properties.Item(<span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: #666666; FONT-FAMILY: Courier New; BACKGROUND-COLOR: #e4e4e4">"FullPath"</span>).Value <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">+</span> _<br />
project.ConfigurationManager.ActiveConfiguration.Properties.Item(<span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: #666666; FONT-FAMILY: Courier New; BACKGROUND-COLOR: #e4e4e4">"OutputPath"</span>).Value <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">+</span> _<br />
project.Properties.Item(<span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: #666666; FONT-FAMILY: Courier New; BACKGROUND-COLOR: #e4e4e4">"OutputFileName"</span>).Value<br />
Dim opfileLength As Long <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> DefaultFileLength<br />
If (File.Exists(CompleteOutputFile)) Then<br />
Dim fi As FileInfo <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> New FileInfo(CompleteOutputFile)<br />
opfileLength <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> fi.Length<br />
End If<br />
' <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">base</span> adress on 64k boundaries<br />
startBase <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> startBase <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">-</span> opfileLength<br />
startBase <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: red; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">=</span> startBase &gt;&gt; 16 &lt;&lt; 16 ' rotate right 16 times and and left 16 times<br />
Debug.Print(String.Format(<span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: #666666; FONT-FAMILY: Courier New; BACKGROUND-COLOR: #e4e4e4">"name:{0} address:{1:x}"</span>, project.Name, startBase))<br />
' store the calculated baseadress <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">in</span> the project BaseAddress<br />
' remember <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">this</span> <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">is</span> a Compile config setting. So rerun <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">this</span> <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">if</span> you compile <span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: blue; FONT-FAMILY: Courier New; BACKGROUND-COLOR: transparent">for</span> Release<br />
project.ConfigurationManager.ActiveConfiguration.Properties.Item(<span style="FONT-WEIGHT: normal; FONT-SIZE: 11px; COLOR: #666666; FONT-FAMILY: Courier New; BACKGROUND-COLOR: #e4e4e4">"BaseAddress"</span>).let_Value(startBase)<br />
End If<br />
End If<br />
Next<br />
End Sub<br />
End Module</span>
</pre></div>
