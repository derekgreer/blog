---
id: 755
title: 'Dependency Management in .Net: Get'
date: 2011-09-21T02:45:11+00:00
author: derekgreer
layout: post
guid: http://www.aspiringcraftsman.com/2011/09/21/dependency-management-in-net-get/
permalink: /2011/09/21/dependency-management-in-net-get/
dsq_thread_id:
  - "644536288"
categories:
  - Uncategorized
tags:
  - NuGet
---
<font color="red"></p> 

<p>
  [Update: <noindex></noindex> This article refers to a tool which will no longer be maintained. Until such time as NuGet is updated to naively support these capabilities, consider using the plug-in described <a href="http://lostechies.com/derekgreer/2011/09/27/dependency-management-in-net-install2/">here</a>.]
</p>

<p>
  </font>
</p>

<p>
  In my <a href="http://lostechies.com/derekgreer/2011/09/20/dependency-management-in-net-using-nuget-without-visual-studio/">last article</a>, I demonstrated how my team is currently using NuGet.exe from our rake build to facilitate application-level, build-time retrieval of external dependencies.&nbsp; Since not everyone uses rake for their build process, I decided to create a simple tool that could be consumed by any build process.
</p>

<p>
  To see how the tool works, follow these steps:
</p>

<blockquote>
  <p>
    <strong>Step 1</strong>: From the command line, execute the following:
  </p>
  
  <pre>
$&gt; nuget install Get
</pre>
  
  <p>
  </p>
  
  <p>
    <strong>Step 2</strong>: Change directory to the Get tools folder.
  </p>
  
  <p>
  </p>
  
  <p>
    <strong>Step 3</strong>: Create a plain text file named dependencies.config and add the following package references:
  </p>
  
  <pre>
NHibernate   3.2.0.4000
Moq          4.0.10827
</pre>
  
  <p>
  </p>
  
  <p>
    <strong>Step 4</strong>: Execute the following command:
  </p>
  
  <p>
  </p>
  
  <pre>
$&gt; get dependencies.config
</pre>
</blockquote>

<p>
</p>

<p>
  The tool currently supports NuGet’s -Source, -ExcludeVersion, and -OutputDirectory switches.&nbsp; From here, you just need to have it download to a central lib folder and adjust your project references as necessary.&nbsp; Now stop checking in those assemblies! 🙂
</p>