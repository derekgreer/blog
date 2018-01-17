---
id: 910
title: Hosting a Git Repository in Windows
date: 2012-02-20T03:37:26+00:00
author: derekgreer
layout: post
guid: http://aspiringcraftsman.com/?p=910
permalink: /2012/02/20/hosting-a-git-repository-in-windows/
dsq_thread_id:
  - "628680663"
categories:
  - Uncategorized
tags:
  - Git
---
If <noindex></noindex> you’re working in a Windows-only environment and you’d like to host a git repository, this article will walk you through three different approaches: Shared File System Hosting, Git Protocol Hosting, and SSH Hosting.

## Setting Up Your Repository

Before choosing which hosting strategy you’d like to use, you’ll first need a git repository to share.&nbsp; The following steps will walk you through setting up an empty git repository.

**Step 1 &#8211; Install Git**

The most popular way to install git on Windows is by installing [msysGit](http://code.google.com/p/msysgit/).&nbsp; The msysGit distribution of git includes a minimalist set of GNU utilities along with the git binaries.&nbsp; Download the msysGit installer and execute it.&nbsp; When prompted to select components on the forth dialog, enable the Windows Explorer Integration components: ‘_Git Bash Here_’ and ‘_Git GUI Here_’.&nbsp; When prompted to configure the line-ending conversions, select ‘_checkout as-is, commit as-is_’ if your team will only ever be working on projects from a Windows machine.

For reference, here are the dialogs presented by the wizard:

[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="msysgit-install-1-3_thumb2" border="0" alt="msysgit-install-1-3_thumb2" src="http://aspiringcraftsman.com/wp-content/uploads/2012/02/msysgit-install-1-3_thumb2_thumb.png" width="535" height="480" />](http://aspiringcraftsman.com/wp-content/uploads/2012/02/msysgit-install-1-3_thumb2.png)

[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="msysgit-install-4-6_thumb2" border="0" alt="msysgit-install-4-6_thumb2" src="http://aspiringcraftsman.com/wp-content/uploads/2012/02/msysgit-install-4-6_thumb2_thumb.png" width="559" height="480" />](http://aspiringcraftsman.com/wp-content/uploads/2012/02/msysgit-install-4-6_thumb2.png)

[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="msysgit-install-7-9_thumb3" border="0" alt="msysgit-install-7-9_thumb3" src="http://aspiringcraftsman.com/wp-content/uploads/2012/02/msysgit-install-7-9_thumb3_thumb.png" width="555" height="480" />](http://aspiringcraftsman.com/wp-content/uploads/2012/02/msysgit-install-7-9_thumb3.png)

&nbsp;

**Step 2 &#8211; Create a Bare Repository**

With this step, we’ll be creating an empty repository for a sample project named ‘sample’.&nbsp; Since we’ll only be using this repository remotely, we’ll be initializing a folder as a ‘bare’ repository, which means that git will place all the files git needs to manage the repository directly within the initialized folder rather than in a ‘.git’ sub-folder.&nbsp; To create the repository, follow these steps:

  * Using Windows Explorer, create a folder somewhere on the server where you plan to store all of your repositories (e.g. C:\git\). 
      * Within the chosen repositories parent folder, create a sub-folder named ‘example.git’. 
          * From the folder pane of Windows Explorer, right click on the ‘example’ folder and select ‘Git Bash Here’.&nbsp; This will open up a bash shell. 
              * At the command prompt, issue the following command: </ul> 
            <pre class="prettyprint">$&gt; git init --bare</pre>
            
            <div style="border-bottom: black 1px solid; border-left: black 1px solid; background: #ccc; border-top: black 1px solid; border-right: black 1px solid">
              <p align="center">
                <strong>Note</strong>
              </p>
              
              <p>
                The ‘example’ repository can be created using your own credentials, but if you’d like to use the ‘Single User’ strategy presented in the SSH Hosting section, I’d recommend creating a ‘git’ Windows account and creating the repository with that user.
              </p>
            </div>
            
            &nbsp;
            
            You now have a git repository for the ‘example’ project and are ready to select a strategy for sharing this repository with your team.
            
            ## Shared File System Hosting
            
            Hosting a git repository via a shared file system is by far the easiest way to get started sharing a git repository.&nbsp; Simply share the main git folder where you created the ‘example.git’ repository and grant full control of the contents.&nbsp; Once you’ve set up the desired level of access, users can clone the shared repository using the following command:
            
            <pre class="prettyprint">$&gt; git clone \\[repository server name]\[share name]\example.git</pre>
            
            ## Git Protocol Hosting
            
            Git supplies its own ‘git’ protocol for simple hosting purposes.&nbsp; To host an existing repository using the git protocol, you can simply issue the following command from the hosting server:
            
            <pre class="prettyprint"><p>
  $&gt; git daemon --base-path=C:/git --export-all
</p></pre>
            
            This will host all repositories located in the folder C:/git as read-only.&nbsp; To allow push access, add &#8211;enable=receive-pack:
            
            <pre class="prettyprint">$&gt; git daemon --base-path=C:/git --export-all --enable=receive-pack</pre>
            
            This is fine for situations where you want to temporarily share a repository with a co-worker (e.g. when working on a feature branch together), but if you want to permanently share remote repositories then you’ll need to start the git daemon as a Windows service.
            
            Unfortunately, it doesn’t seem Microsoft makes installing an arbitrary command as a service very easy.&nbsp; Windows does provide the Service Control utility (e.g. sc.exe), but this only allows you to work with Windows service applications.&nbsp; To resolve this problem, a simple .Net Windows Service application can be created which performs a Process.Start() on a supplied service argument.&nbsp; 
            
            For the purposes of this guide, I’ve created just such a service which you can obtain from <https://github.com/derekgreer/serviceRunner>.&nbsp; Once you have the service compiled, create a batch file named ‘gitd.bat’ with the following contents:
            
            <pre class="prettyprint">"C:\Program Files (x86)\Git\bin\git.exe" daemon --reuseaddr --base-path=C:\git --export-all --verbose --enable=receive-pack</pre>
            
            You can then register the gitd.bat file as a service using the Service Control utility by issuing the following command (from an elevated prompt): 
            
            <pre class="prettyprint">sc.exe create "Git Daemon" binpath= "C:\Utils\ServiceRunner.exe C:\Utils\git-daemon.bat" start= auto</pre>
            
            To start the service, issue the following command:
            
            <pre class="prettyprint">sc.exe start "Git Daemon"</pre>
            
            After successfully starting the Git Daemon service, you should be able to issue the following command:
            
            <pre class="prettyprint">git clone git://localhost/example.git</pre>
            
            ### Registering Git Daemon With Cygwin
            
            As an alternative to using the Service Control utility, you can also install any application as a service using Cygwin’s cygrunsrv command.&nbsp; If you already have Cygwin installed or you’re planning on using Cygwin to host git via SSH then this is the option I’d recommend.&nbsp; After installing Cygwin with the cygrunsrv package, follow these steps to register the git daemon command as a service: 
            
            **Step 1:** Open a bash shell
            
            **Step 2:** In a directory where you want to store your daemon scripts (e.g. /cygdrive/c/Cygwin/usr/bin/), create a file named &#8220;gitd&#8221; with the following content: 
            
            <pre class="prettyprint">#!/bin/bash

c:/Program \Files/Git/git daemon --reuseaddr                 \
                                 --base-path=/cygdrive/c/git \
                                 --export-all                \
                                 --verbose                   \
                                 --enable=receive-pack
</pre>
            
            **Step 3:** Run the following cygrunsrv command to install the script as a service (Note: assumes Cygwin is installed at C:\Cygwin):
            
            <pre class="prettyprint">cygrunsrv   --install gitd                          \
            --path c:/cygwin/bin/bash.exe           \
            --args c:/cygwin/usr/bin/gitd           \
            --desc "Git Daemon"                     \
            --neverexits                            \
            --shutdown
</pre>
            
            **Step 4:** Run the following command to start the service: 
            
            <pre class="prettyprint">cygrunsrv --start gitd
</pre>
            
            ## SSH Hosting
            
            Our final approach to be discussed is the hosting of git repositories via [SSH](http://en.wikipedia.org/wiki/Secure_Shell).&nbsp; SSH (which stands for Secure Shell) is a protocol commonly used by Unix-like systems for transporting information securely between networked systems.&nbsp; To host git repositories via SSH, you’ll need to run an SSH server process on the machine hosting your git repositories.&nbsp; To start, install Cygwin with the openssh package selected.&nbsp; 
            
            Once you have Cygwin installed, follow these steps:
            
            **Step 1** &#8211; Open a bash shell
            
            **Step 2** &#8211; Run the following command to configure ssh:
            
            <pre class="prettyprint">ssh-host-config -y
</pre>
            
            For later versions of Windows, this script may require that a privileged user be created in order to run with elevated privileges.&nbsp; By default, this user will be named _cyg_server_.&nbsp; When prompted, enter a password for the cyg_server user which meets the password policies for your server.
            
            **Step 3** &#8211; Run the following command to start the sshd service:
            
            <pre class="prettyprint">net start sshd
</pre>
            
            You should see the following output:
            
            <pre class="prettyprint">The CYGWIN sshd service is starting.
The CYGWIN sshd service was started successfully.
</pre>
            
            The sshd service should now be up and running on your server.&nbsp; The next step is to choose a strategy for allowing users to connect via SSH.
            
            ### User Management Strategy
            
            There are two primary strategies for managing how users connect to the SSH server.&nbsp; The first is to configure each user individually.&nbsp; The second is to use a single user for your entire team.&nbsp; Let’s look at each.
            
            #### Individual User Management
            
            SSH allows users configured in the /etc/passwd file to connect via SSH.&nbsp; Cygwin automatically creates this file when it first installs.&nbsp; To add additional users (e.g. the user ‘bob’), you can append new records to the passwd file by issuing the following command from a bash shell:
            
            <pre class="prettyprint">mkpasswd -l | grep ‘^bob’ &gt;&gt; /etc/passwd
</pre>
            
            This command adds an entry for a user ‘bob’ by doing the following:
            
              * run the mkpasswd command for all local users 
                  * select only entries starting with the string ‘bob’ 
                      * append the entry to the /etc/passwd file </ul> 
                    To see the new entry, you can ‘cat’ the contents of the file to the screen by issuing the following command:
                    
                    <pre class="prettyprint">cat /etc/passwd
</pre>
                    
                    This should show entries that look like the following:
                    
                    <pre class="prettyprint">SYSTEM:*:18:544:,S-1-5-18::
LocalService:*:19:544:U-NT AUTHORITY\LocalService,S-1-5-19::
NetworkService:*:20:544:U-NT AUTHORITY\NetworkService,S-1-5-20::
Administrators:*:544:544:,S-1-5-32-544::
Administrator:unused:500:513:DevMachine\Administrator,S-1-5-21-2747539007-3005349326-118100678-500:/home/Administrator:/bin/bash
Guest:unused:501:513:DevMachine\Guest,S-1-5-21-2747539007-3005349326-118100678-501:/home/Guest:/bin/bash
bob:unused:1026:513:git,DevMachine\bob,S-1-5-21-2747539007-3005349326-118100678-1026:/home/git:/bin/bash
</pre>
                    
                    By adding the new entry to the /etc/password file, the user ‘bob’ will now have access to access git repositories via SSH.&nbsp; To clone a repository, Bob would simply need to issue the following command:
                    
                    <pre class="prettyprint">git clone ssh://DevMachine/git/example.git
</pre>
                    
                    At this point, Bob would have to enter his password as it is set on the server.&nbsp; I’ll show you how to avoid typing the password a little later.
                    
                    One downside of this setup, however, is that the user ‘bob’ will also have access to shell into the machine by entering the following command:
                    
                    <pre class="prettyprint">ssh DevMachine
</pre>
                    
                    If the user bob already has an account on the machine, this probably isn’t an issue.&nbsp; However, not all users in the /etc/password file are necessarily accounts on the local server.&nbsp; You can also add network users by issuing the following command:
                    
                    <pre class="prettyprint">mkpasswd -d [domain name] -u [user name] &gt;&gt; /etc/passwd
</pre>
                    
                    In this case, you may want to restrict what the user ‘bob’ can do to just issuing git commands.&nbsp; You can do this by changing bob’s default shell entry in the /etc/passwd file from /bin/bash to /usr/bin/git-shell.&nbsp; The git-shell is a special shell which restricts access to just a few git commands.&nbsp; Trying to ssh into a server where the shell is set to git-shell will print an error message.
                    
                    #### Single User
                    
                    Another strategy that is a bit easier to manage is to use a single user for all team members.&nbsp; At first, this might seem like you’re just throwing up your hands and opening up access to everyone, but this isn’t actually the case as we’ll see shortly.
                    
                    To use this strategy, follow these steps:
                    
                    **Step 1 &#8211; Global User Creation**
                    
                    First create a new user to be used for git access.&nbsp; I’ll call the user ‘git’ for our example.&nbsp; 
                    
                    **Step 2 &#8211; Configure SSH Access**
                    
                    Now, configure the ‘git’ user to have access to shell into the server:
                    
                    <pre class="prettyprint">mkpasswd -l | grep ‘^git’ &gt;&gt; /etc/passwd</pre>
                    
                    **Step 3 &#8211; Change Shell**
                    
                    Optional, but it’s a good idea to set the git user’s shell to the git-shell.
                    
                    You should now be able to use the git user to access any git repositories on the server.&nbsp; If you test that out at this point, you’ll be prompted for the ‘git’ user’s password.
                    
                    What we want to do at this point is configure users to have access to ssh as the ‘git’ account without having to use the password.&nbsp; To do this, we’ll need to set up an ssh public/private key pair for each user.&nbsp; Let’s use bob as our example.&nbsp; I’ll be using Cygwin’s openssh as our example, but the concepts are the same if using Putty as your SSH client.
                    
                    To setup bob’s ssh keys, he’ll need to run the following command:
                    
                    <pre class="prettyprint">ssh-keygen</pre>
                    
                    This will prompt the user bob for the location to store the ssh keys.&nbsp; Hitting enter will accept the defaults. The command will further prompt him for a passphrase.&nbsp; A passphrase is recommended because it keeps anyone who has access to bob’s machine from getting his private key and impersonating him, but for now let’s just have him hit enter when prompted.&nbsp; Entering an empty passphrase will cause ssh to bypass asking for anything.&nbsp; I’ll talk about ways of using a passphrase without having to enter it every time in a bit.
                    
                    Here’s the kind of output you’ll see:
                    
                    <pre class="prettyprint">Generating public/private rsa key pair.
Enter file in which to save the key (/home/bob/.ssh/id_rsa):
Created directory '/home/bob/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/bob/.ssh/id_rsa.
Your public key has been saved in /home/bob/.ssh/id_rsa.pub.
The key fingerprint is:
53:6c:19:ec:c7:96:c5:ee:1d:b0:93:84:b6:8c:a5:2d bob@bobsMachine
The key's randomart image is:
+--[ RSA 2048]----+
|         .. ..   |
|         ..* oo  |
|         .%.o++  |
|         E.+=+.. |
|        S .o ....|
|         .    . .|
|                 |
|                 |
|                 |
+-----------------+</pre>
                    
                    From here, Bob will have a newly created .ssh directory in his home directory:
                    
                    <pre class="prettyprint">.ssh $&gt; ls

id_rsa&nbsp; id_rsa.pub</pre>
                    
                    What we as the admin of the git server need from Bob is the contents of his id_rsa.pub file.&nbsp; That should look something like the following:
                    
                    <pre class="prettyprint">ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDRVsZqNvQNc9YBcLCrm2pZotUiFsyZsQtdhWdtVF3u3PHR1ZNcGvWqSSI+hrb7HP/mTFBzyciO1nWRfbERXjexLd5uBf+ou5ZHDs51JIGQs61Lb+Kq/Q8P2/77bqGIIF5cZPfewZM/wQYHiR/JhIWHCRRmVOwPgPkfI7cqKOpbFRqyRYuV0pglsQEYrjm4FCM2MJ4iWnLKdgqj6vCJbNT6ydx4LqqNH9fCcbOphueoETgiBeUQ9U64OsEhlek9trKAQ0pBSNkJzbslbqzLgcJIitX4OYTxau3l74W/kamWeLe5+6M2CUUO826R9j4XuGQ2qqo5A5GrdVSZffuqRtX1 bob@bobMachine</pre>
                    
                    Next, we want to add this key to a special file named authorized_users in the remote ‘git’ user’s .ssh folder.
                    
                    <pre class="prettyprint">[DevMachine]: .ssh &gt; $ cat authorized_keys</pre>
                    
                    <pre class="prettyprint">ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC36qnox4nlTInc1fyOlaUC3hJhEdVM4/qKeEKPBJ520sOzJG+cRvRGNSdbtLNKD9xZs0dpiql9Vtgy9Yc2XI+lWjBGUmPbqWUuP8IZdFGx3QwPSIx9YzakuUBqYE5+9JKcBuHIhIlilqCCzDtXop6Bi1lN0ffV5r6PyqyFIv0L7MJb8jDsHX7GRl4IGu8ScxfY4G0PS3ZrMGfQBr2fm8KFzg7XWVaP/HTT4XKcf5Jp6oHvLz8FvEfdZdajyFUXRzrE0Kt9KAbeIBJV8+usiTAVpsmMY1yfrsuBUdOlhpvL/pU2o5B6K8VlJeXSF4IYEgS+v6JBAlyaWQkXupQr+lIL bob@BobMachine</pre>
                    
                    That’s it.&nbsp; The user ‘bob’ will now be able to ssh without providing a password.
                    
                    #### SSH Without Passphrases
                    
                    Remember, it’s recommended that everyone use a passphrase.&nbsp; This prevents someone who’s gained access to your machine from tricking out the remote server to letting them have access as you.&nbsp; To use a passphrase, repeat the above steps with a a passphrase set.&nbsp; To keep us from having to always type in the passphrase, we can use ssh-agent (or the Pageant process for Putty users).&nbsp; The basic premise is that you type your passphrase once per session and the ssh-agent will keep track of the passphrase for you.&nbsp; 
                    
                    To use the ssh-agent to track your passphrase, add the following to your ~/.bash_profile:
                    
                    <pre class="prettyprint">SSHAGENT=/usr/bin/ssh-agent
SSHAGENTARGS="-s"
if [ -z "$SSH_AUTH_SOCK" -a -x "$SSHAGENT" ]; then
	eval `$SSHAGENT $SSHAGENTARGS`
	trap "kill $SSH_AGENT_PID" 0
fi</pre>
                    
                    Close your bash shell and reopen it.&nbsp; You should see an agent pid output when you open your shell again.
                    
                    <pre class="prettyprint">Agent pid 1480
[DevMachine]: &gt;
</pre>
                    
                    Now, issue the following command:
                    
                    <pre class="prettyprint">ssh-add ~/.ssh/id_rsa</pre>
                    
                    Type your passphrase when prompted.&nbsp; Thereafter, you will be able to ssh without providing a passphrase.
                    
                    That concludes the guide.&nbsp; Enjoy!
                    
                    <pre></pre>
                    
                    <pre></pre>
                    
                    <pre></pre>