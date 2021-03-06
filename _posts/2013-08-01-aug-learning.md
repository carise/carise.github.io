---
layout: post
title: "What I Learned in August"
---

h1. {{ page.title }}

h2. 2013 August 6

Setting up X and remote X (via Cygwin)

1. Use @yum groupinstall Desktop@
2. Edit @/etc/inittab@ for runlevel 3
3. Edit @/etc/gdm/custom.conf@ 
  [xdmcp]
  Enable=true

  [security]
  DisallowTCP=false
  AllowRemoteRoot=true
4. Get Cygwin, install X
5. Allow the following on iptables
  UDP 177 (xdmcp)
  TCP 6000-6005 (X11)
  TCP 7100 (xfs)
6. From Cygwin (not Cygwin X),
  @xwin :0.0 -query [MACHINE HOSTNAME]@


h2. 2013 August 26 

When installing Oracle 11g2 on Centos, the easiest thing to do is get the preinstall rpm. What are you waiting for? Go get it now!

And another thing. @iptables@ everything... I was wondering why my clock wasn't synced correctly and it turns out that ntpd was getting blocked.

If the Oracle runInstaller script won't run and neither will xclock, then do @xhost + [hostname]@ to enable access to the host (my local machine).

h2. 2013 August 27

CentOS 6 Oracle setup: 

Environment:
@ORACLE_BASE=/opt/oracle@
@ORACLE_HOME=/opt/oracle/product/11.2.0/dbhome_1@

This makes life much easier:
http://www.oracle.com/technetwork/articles/servers-storage-admin/ginnydbinstallonlinux-488779.html

Getting an error about display not > 256 colors?

Open terminal, and as the logged-in user type 

@xhost + ip-t3400-1542.ironplanet.local@

to allow usage of X applications

refer to http://www.unix.com/red-hat/116444-error-cant-open-display-0-0-a.html

Then @su oracle@ and run the installer


To avoid the issues with the db not being found due to oc4j Configuration issue/EM Configuration issue, install the db for "Server", not "Desktop"

Also change /etc/hosts and add as the first line:

@127.0.0.1 ip-t3400-1542.ironplanet.local@

See the Correct Answer: https://forums.oracle.com/thread/1003910
Maybe bug 8638267

From the 11gR2 README file (under Open Bugs):
/*

Bug 8638267
If you select the "Desktop Class" style database configuration in the Installer, and after completing the installation you attempt to create a database using DBCA or any DBControl setup using Enterprise Manager Configuration Assistant (EMCA), then you must set the ORACLE_HOSTNAME environment variable to 'localhost'. If you do not set ORACLE_HOSTNAME, then DBCA fails while configuring Enterprise Manager with the following error:
Listener is not up or database service is not registered with it. Start the Listener and register database service and run EM Configuration Assistant again.

Workaround: Set the ORACLE_HOSTNAME environment variable to 'localhost' and retry creating the database.
*/

http://download.oracle.com/docs/cd/E11882_01/readmes.112/e11015/toc.htm
