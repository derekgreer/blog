---
id: 298
title: Getting Started With Cygwin
date: 2010-04-08T14:58:01+00:00
author: derekgreer
layout: post
guid: http://www.aspiringcraftsman.com/?p=298
permalink: /2010/04/08/getting-started-with-cygwin/
dsq_thread_id:
  - "261600794"
categories:
  - Uncategorized
tags:
  - Cygwin
---
With <noindex></noindex> the increasing popularity of the <a id="b89_" title="Git" href="http://git-scm.com/" target="_blank">Git</a> version control system, many .Net developers are being introduced for the first time to Unix-like tools for Windows by way of two popular Git client platforms: <a id="nucy" title="msysgit" href="http://code.google.com/p/msysgit/" target="_blank">msysgit</a> and <a id="tw22" title="Cygwin" href="http://en.wikipedia.org/wiki/Cygwin" target="_blank">Cygwin</a>. The more substantial of the two, Cygwin is a Linux-like environment for Windows and provides a wide range of useful utilities. The following is a guide for helping newcomers quickly get up and going with the Cygwin environment.

## Installation

The first step is to obtain the installer from the <a id="izx." title="Cygwin home page" href="http://www.cygwin.com/" target="_blank">Cygwin home page</a>. Upon running the installer, you&#8217;ll be presented with a wizard which guides you through the installation process:

<img class="alignnone size-full wp-image-302" title="cygwin-step-1-3" src="/wp-content/uploads/2010/04/cygwin-step-1-3.png" alt="" width="593" height="485" srcset="http://aspiringcraftsman.com/wp-content/uploads/2010/04/cygwin-step-1-3.png 847w, http://aspiringcraftsman.com/wp-content/uploads/2010/04/cygwin-step-1-3-300x245.png 300w" sizes="(max-width: 593px) 100vw, 593px" />

&nbsp;

[<img class="alignnone size-full wp-image-303" title="cygwin-step-4-6" src="/wp-content/uploads/2010/04/cygwin-step-4-6.png" alt="" width="594" height="488" srcset="http://aspiringcraftsman.com/wp-content/uploads/2010/04/cygwin-step-4-6.png 848w, http://aspiringcraftsman.com/wp-content/uploads/2010/04/cygwin-step-4-6-300x246.png 300w" sizes="(max-width: 594px) 100vw, 594px" />](/wp-content/uploads/2010/04/cygwin-step-4-6.png)

Upon arriving at the &#8220;Select Packages&#8221; dialog, you&#8217;ll be presented with all the packages available from the selected mirror site(s):

<img class="alignnone size-full wp-image-300" title="cygwin-packages-1" src="/wp-content/uploads/2010/04/cygwin-packages-1.png" alt="" width="497" height="324" srcset="http://aspiringcraftsman.com/wp-content/uploads/2010/04/cygwin-packages-1.png 710w, http://aspiringcraftsman.com/wp-content/uploads/2010/04/cygwin-packages-1-300x195.png 300w" sizes="(max-width: 497px) 100vw, 497px" />

&nbsp;

The installer allows you to install selected packages, or install by category:

<img class="alignnone size-full wp-image-301" title="cygwin-packages-2" src="/wp-content/uploads/2010/04/cygwin-packages-2.png" alt="" width="610" height="213" srcset="http://aspiringcraftsman.com/wp-content/uploads/2010/04/cygwin-packages-2.png 678w, http://aspiringcraftsman.com/wp-content/uploads/2010/04/cygwin-packages-2-300x104.png 300w" sizes="(max-width: 610px) 100vw, 610px" />

&nbsp;

At the package level, the rotating arrow icon allows you to cycle through the choices of installing one of the package versions available, skipping the package, or in the event the package is already installed, keeping, reinstalling, uninstalling, or obtaining the source code for the package. At the category level, the rotating arrow icon allows you to cycle through the choices of Default, Install, Reinstall, or Uninstall.

It&#8217;s best to only select the packages you really think you&#8217;ll want to use or immediately explore. There&#8217;s quite a bit of applications available and choosing everything would result in quite a large installation time. The installer can always be run at a later time to pickup up additional packages you want to explore.

By default, Cygwin selects the Base category packages as well as a few other odds & ends. Among the packages skipped by default, you may consider also installing the following:

<div class="theme-table">
  <table style="height: 226px;" border="1" cellspacing="0" cellpadding="2" width="100%">
    <tr>
      <td width="205" valign="top">
        Category
      </td>
      
      <td width="244" valign="top">
        Package
      </td>
      
      <td width="318" valign="top">
        Description
      </td>
    </tr>
    
    <tr>
      <td width="206" valign="top">
        Admin
      </td>
      
      <td width="243" valign="top">
        cygrunsrv
      </td>
      
      <td width="318" valign="top">
        Utility for easily working with Windows services (adding/removing/starting/stopping).  This is beneficial if you&#8217;d like to run git daemon or sshd as a windows service for anonymous or authenticated access.
      </td>
    </tr>
    
    <tr>
      <td width="207" valign="top">
        Archive
      </td>
      
      <td width="243" valign="top">
        zip, unzip
      </td>
      
      <td width="317" valign="top">
        PKZip compatible zip capabilities.
      </td>
    </tr>
    
    <tr>
      <td width="208" valign="top">
        Editors
      </td>
      
      <td width="243" valign="top">
        vim
      </td>
      
      <td width="317" valign="top">
        An enhanced VI editor.
      </td>
    </tr>
    
    <tr>
      <td width="208" valign="top">
        Net
      </td>
      
      <td width="243" valign="top">
        openssh
      </td>
      
      <td width="317" valign="top">
        Secure shell client and server programs.
      </td>
    </tr>
    
    <tr>
      <td width="208" valign="top">
        Utils
      </td>
      
      <td width="243" valign="top">
        ncurses
      </td>
      
      <td width="317" valign="top">
        Terminal utilities (has a clear command for clearing the screen).
      </td>
    </tr>
    
    <tr>
      <td width="208" valign="top">
        Web
      </td>
      
      <td width="243" valign="top">
        curl
      </td>
      
      <td width="317" valign="top">
        Multi-protocol file transfer tool.  This tool is useful for scripting HTTP interaction.
      </td>
    </tr>
  </table>
</div>

&nbsp;

In addition, I personally always install the X11 category packages. While this adds a bit to the download size, it gives you the ability to run your preferred shell (e.g. bash) in an xterm as opposed to the standard terminal window. I&#8217;ll cover a bit of X11 installation and customization later in this guide.

When you&#8217;re done selecting your packages, click &#8220;Next&#8221; and the install will begin:

[<img class="alignnone size-full wp-image-304" title="cygwin-step-7" src="/wp-content/uploads/2010/04/cygwin-step-7.png" alt="" width="546" height="397" />](/wp-content/uploads/2010/04/cygwin-step-7.png)

&nbsp;

<img class="alignnone size-full wp-image-305" title="cygwin-step-8" src="/wp-content/uploads/2010/04/cygwin-step-8.png" alt="" width="545" height="398" />

&nbsp;

Once complete, click the Finish button and you&#8217;re done with the install.

## Bash Shell Customization

If you selected the &#8220;Add icon to Start Menu&#8221; option during the install then you should have a new shortcut entitled &#8220;Cygwin Bash Shell&#8221; which will start the bash shell as an interactive login shell (i.e. bash &#8211;login -i). If not, you can browse to cygwin.bat file located within the chosen installation folder.

When bash runs for the first time, it checks to see if the home directory (denoted in the /etc/passwd file) exists for your account. If not, it creates the directory and copies over some default config files for your shell:

<img class="alignnone size-full wp-image-299" title="bash-text" src="/wp-content/uploads/2010/04/bash-text.png" alt="" width="609" height="317" srcset="http://aspiringcraftsman.com/wp-content/uploads/2010/04/bash-text.png 677w, http://aspiringcraftsman.com/wp-content/uploads/2010/04/bash-text-300x155.png 300w" sizes="(max-width: 609px) 100vw, 609px" />

&nbsp;

The .bash\_profile is used for login shells while the .bashrc file is used for interactive, non-login shells. The default .bash\_profile configuration sources the .bashrc file if it exists, so both are sourced for interactive login shells. This effectively allows the .bashrc to serve as the core configuration for both login and non-login shells. The .inputrc file is used to set custom key mappings. Of the three, the .bashrc file will generally be the one you&#8217;ll deal with most often.

Go ahead and open up the .bashrc file. (Note: Microsoft Notepad does not display Unix-style line endings correctly. If using a Windows editor, use Wordpad.) After opening the file, you&#8217;ll notice that most of the configuration is commented out. The only active configuration are commands to unset the Windows TMP and TEMP variables. You can keep this if you like, but I prefer to keep things a bit more tidy and only have the configuration I actually use. If you want to view the default contents of this file, it can always be viewed from its original source in /etc/skel. The following is a more minimal configuration you might wish to start with:

<pre class="brush:bash">unset TMP
unset TEMP

PATH=.:~/bin:${PATH}
PATH=${PATH}:c:/Windows/Microsoft.Net/Framework/v3.5/
PATH=${PATH}:c:/Program  Files/Reflector/
PATH=${PATH}:c:/Program Files/Microsoft  SDKs/Windows/v6.1/Bin/

. ~/.alias
. ~/.functions
</pre>

In this configuration, several folders have been added to the PATH environment variable. The first places a ~/bin folder before the PATH. This ensures any custom scripts found in the users bin folder occur first in the path allowing commands to be overridden. The remaining three lines are example folders you might want to set if you are doing .Net development related tasks at the bash command line.

The remaining two lines assume the existence of two new files, .alias and .functions. I find storing aliases and functions separately to be a bit more tidy, as well as making it easier to share with others.

Now, let&#8217;s take a look at the .bash_profile configuration. By default, the only configuration that exists is the sourcing of the .bashrc file. Again, normally the .bashrc file is only sourced for non-login shells, so this ensures the .bashrc settings are picked up for login shells as well.

In this file, let&#8217;s replace the contents with the following:

<pre class="brush:bash">#  source the system wide bashrc if it exists
if [ -e /etc/bash.bashrc ]  ; then
source /etc/bash.bashrc
fi

# source the users  bashrc if it exists
if [ -e "${HOME}/.bashrc" ] ; then
source  "${HOME}/.bashrc"
fi

PS1='[h]: ${PWD##*/} &gt; '
set -o vi
export  DISPLAY=127.0.0.1:0.0
</pre>



In addition to the existing configuration, we&#8217;ve added three additional settings. The first customizes how the prompt appears at the command line by setting the PS1 variable. This prompt is a bit plain, but you can spruce it up with a bit of color by replacing it with the following:

PS1=&#8217;\[\033]0;\w\007\033\[32m\\]\[\h\]: \[\033[33m ${PWD##*/}\033[0m\] > &#8216;

Next, I&#8217;ve issued: &#8220;set -o vi&#8221;. This tells bash to use a vi-style command line editing interface which let&#8217;s you more efficiently issue and edit commands at the command line. You can delete this if you don&#8217;t ever plan on learning vi.

Next, I&#8217;ve set an X environment variable named &#8220;DISPLAY&#8221; to my localhost. This tells the xterm and other X applications where to attempt to display. You can delete this if you don&#8217;t plan on running an xterm, but leaving it certainly won&#8217;t hurt.

This should get you started. Your config files will likely evolve from here if you find yourself working at the command line often.

## Aliases

The next step you may want to take is to define some command aliases. The bash alias command allows you to set up aliases for verbose or otherwise undesirable commands. As indicated, I store all of my aliases in a .alias file in my home directory to segregate them away from the rest of my .bashrc configuration. Here is a subset of my current list of aliases you might find useful:

<pre class="brush:bash">alias programfiles="cd  /cygdrive/c/Program Files"
alias projects="cd /cygdrive/c/projects"
alias  spikes="cd /cygdrive/c/projects/spikes"
alias vs='cmd /c *.sln'
alias  vs9='/cygdrive/c/Program Files/Microsoft Visual Studio  9.0/Common7/IDE/devenv.exe *.sln&amp;'
alias  wordpad='/cygdrive/c//Program Files/Windows  NT/Accessories/wordpad.exe'
alias config='cd  /cygdrive/c/Windows/Microsoft.NET/Framework/v2.0.50727/CONFIG'
alias  mydocs='cd /cygdrive/c/Users/${USER}/Documents'
alias myhome='cd  /cygdrive/c/Documents and Settings/${USER}/'
alias  fusion='fuslogvw.exe'
</pre>

You&#8217;ll be surprised at how fast you&#8217;ll start zipping around the system once you&#8217;ve got some good navigation aliases in place.

## Functions

I don&#8217;t tend to write a lot of bash functions, but I&#8217;m including this section here for completeness. Like the aliases, I like to segregate any functions I do write away from my main config file to make things a bit cleaner. As an example of what you can do, here is the contents of an example .functions file which allows you to tweet from the command line:

<pre class="brush:bash">tweet()
{
read -s -p "Password:" password
curl -u  derekgreer:$password -d status="$1"  http://twitter.com/statuses/update.xml
}
</pre>

<div class="theme-note">
  Note: If you want to actually use this function then you&#8217;ll need to have downloaded the curl package and will need to modify the Twitter id.
</div>

## Basic Commands

If you&#8217;re completely new to the Unix shells, the following are some common commands to get you started with navigating around, creating folders, deleting files, etc.:

<div class="theme-table">
  <table style="height: 268px;" border="1" cellspacing="0" cellpadding="0" width="100%">
    <tr>
      <td width="312">
        <strong>Command</strong>
      </td>
      
      <td width="386">
        <strong>Description</strong>
      </td>
    </tr>
    
    <tr>
      <td width="312">
        cd
      </td>
      
      <td width="386">
        Change to home directory
      </td>
    </tr>
    
    <tr>
      <td width="312">
        cd [directory name]
      </td>
      
      <td width="386">
        Change to a specified directory
      </td>
    </tr>
    
    <tr>
      <td width="312">
        cd &#8211;
      </td>
      
      <td width="386">
        Change to the last directory
      </td>
    </tr>
    
    <tr>
      <td width="312">
        ls
      </td>
      
      <td width="386">
        List the contents of the current folder (like dir)
      </td>
    </tr>
    
    <tr>
      <td width="312">
        ls -la
      </td>
      
      <td width="386">
        List the contents of the current folder in long format including all hidden files
      </td>
    </tr>
    
    <tr>
      <td width="312">
        cat [filename]
      </td>
      
      <td width="386">
        Print the context of a file (like DOS type)
      </td>
    </tr>
    
    <tr>
      <td width="312">
        ![command prefix]
      </td>
      
      <td width="386">
        Run the last command starting with the specified prefix.
      </td>
    </tr>
    
    <tr>
      <td width="312">
        mkdir [directory name]
      </td>
      
      <td width="386">
        Make a new directory
      </td>
    </tr>
    
    <tr>
      <td width="312">
        mkdir -p [directory hierarchy]
      </td>
      
      <td width="386">
        Make a all directories listed (e.g. mkdir -p a/b/c/d)
      </td>
    </tr>
    
    <tr>
      <td width="312">
        rm [filename]
      </td>
      
      <td width="386">
        Remove a file
      </td>
    </tr>
    
    <tr>
      <td width="312">
        rm -rf [filename]
      </td>
      
      <td width="386">
        Remove recursively with force
      </td>
    </tr>
    
    <tr>
      <td width="312">
        find [start folder] -name [regex] -print
      </td>
      
      <td width="386">
        Starting at the start folder, find a file matching the given regular expression.
      </td>
    </tr>
    
    <tr>
      <td width="312">
        grep [regex] [filename]
      </td>
      
      <td width="386">
        Display all lines matching the regular expression from the given file.
      </td>
    </tr>
  </table>
</div>

## Scripts

Once you start getting comfortable with the available commands, you may wish to start writing your own scripts to help with various everyday tasks. While covering the basics of bash shell scripting is beyond the intended scope of this article, you can obtain an archive of some scripts I&#8217;ve written for my own purposes from [here](http://www.aspiringcraftsman.com/downloads/bashscripts.zip "Bash Scripts"){#cf28}. Here&#8217;s a list of the contained scripts along with a brief description:

<div class="theme-table">
  <table style="height: 162px;" border="1" cellspacing="0" cellpadding="0" width="100%">
    <tr>
      <td width="315">
        <strong>Script</strong>
      </td>
      
      <td width="383">
        <strong>Description</strong>
      </td>
    </tr>
    
    <tr>
      <td width="315">
        clean
      </td>
      
      <td width="383">
        Cleans up common temp/generated Visual Studio files.
      </td>
    </tr>
    
    <tr>
      <td width="315">
        detachfromcvs
      </td>
      
      <td width="383">
        Removes CVS folders.
      </td>
    </tr>
    
    <tr>
      <td width="315">
        detachfromtfs
      </td>
      
      <td width="383">
        Removes TFS bindings from source files.
      </td>
    </tr>
    
    <tr>
      <td width="315">
        diff
      </td>
      
      <td width="383">
        Overrides the diff command to call WinMerge
      </td>
    </tr>
    
    <tr>
      <td width="315">
        git-diff-wrapper.sh
      </td>
      
      <td width="383">
        Diff wrapper for use with Git.
      </td>
    </tr>
    
    <tr>
      <td width="315">
        replaceinfile
      </td>
      
      <td width="383">
        Replaces string patterns in a file.
      </td>
    </tr>
    
    <tr>
      <td width="315">
        replaceinfiles
      </td>
      
      <td width="383">
        Wrapper for calling replaceinfile for files matching a given pattern.
      </td>
    </tr>
    
    <tr>
      <td width="315">
        rgrep
      </td>
      
      <td width="383">
        Recursive grep (Also greps from .Net assemblies)
      </td>
    </tr>
    
    <tr>
      <td width="315">
        unix2dosall
      </td>
      
      <td width="477">
        Recursive unix2dos
      </td>
    </tr>
  </table>
</div>

## X11 &#8211; Using an XTerm

The final portion of this guide is intended for those who would like to explore the X Windows system provided by Cygwin. One of the benefits of running an XServer on Windows is the ability to use an XTerm over the standard terminal provided by DOS. XTerm provides more flexibility over the fonts, colors used, and buffers used, provides dynamic resizing, and provides much more intuitive cut-n-paste capabilities (highlight = cut, middle mouse button = paste). It also displays buffered text which comes in handy when you want to use the tail -f command to follow a log file and queue up a bunch of spaces to visually separate new entries.

Assuming you&#8217;ve already installed the X11 packages, locate the Cygwin-X folder in your start menu and execute the &#8220;XWin Server&#8221; shortcut. By default, the XServer starts a plain white terminal:

<img class="alignnone size-full wp-image-306" title="white-xterm" src="/wp-content/uploads/2010/04/white-xterm.png" alt="" width="351" height="265" />

&nbsp;

Since this isn&#8217;t likely the style of terminal you&#8217;ll want to work with, you can prevent XServer from starting this default xterm by creating an empty .startxwinrc file in your home directory. You can do this by using the touch command:

touch ~/.startxwinrc

Next, you can modify the XTerm shortcut that was created in your start menu&#8217;s Cygwin-X folder to start a new xterm with no menu, a custom font, colors, a scrollbar, etc. using the following Target value:

C:Cygwinbinrun.exe -p /usr/X11R6/bin xterm -display 127.0.0.1:0.0 -ls -vb -fn 10&#215;20 +tb -bg gray14 -fg ivory -sb -rightbar -sl 400

This command will launch an xterm similar to the following:

<img class="alignnone size-full wp-image-307" title="xterm" src="/wp-content/uploads/2010/04/xterm.png" alt="" width="607" height="351" />

&nbsp;

From here, you may consider adding the XWin Server shortcut to your startup.

That concludes my guide. Enjoy!