Samba/Troubleshooting
=====================

Contents
--------

-   1 Troubleshooting
    -   1.1 Windows 7 connectivity problems - mount error(12): cannot
        allocate memory
    -   1.2 Trouble accessing a password-protected share from Windows
    -   1.3 Getting a dialog box up takes a long time
    -   1.4 Changes in Samba version 3.4.0
    -   1.5 Error: Value too large for defined data type
    -   1.6 Error: Failed to retrieve printer list:
        NT_STATUS_UNSUCCESSFUL
    -   1.7 Sharing a folder fails
    -   1.8 "Browsing" network fails with "Failed to retrieve share list
        from server"

Troubleshooting
---------------

> Windows 7 connectivity problems - mount error(12): cannot allocate memory

A known Windows 7 bug that causes "mount error(12): cannot allocate
memory" on an otherwise perfect cifs share on the Linux end can be fixed
by setting a few registry keys on the Windows box as follows:

-   HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory
    Management\LargeSystemCache (set to 1)
-   HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters\Size
    (set to 3)

  
 Alternatively, in Command Prompt (make sure it is executed in Admin
Mode):

     reg add "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /v "LargeSystemCache" /t REG_DWORD /d 1 /f
     reg add "HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" /v "Size" /t REG_DWORD /d 3 /f

Restart the Windows machine for the settings to take effect.

Note: Googling will reveal another tweak recommending users to add a key
modifying the "IRPStackSize" size. This is incorrect for fixing this
issue under Windows 7. Do not attempt it.

Link to original article.

> Trouble accessing a password-protected share from Windows

For trouble accessing a password protected share from Windows, try
adding this to /etc/samba/smb.conf:[1]

Note that this needs to be added to the local smb.conf, not to the
server's smb.conf

    [global]
    # lanman fix
    client lanman auth = yes
    client ntlmv2 auth = no

> Getting a dialog box up takes a long time

I had a problem that it took ~30 seconds to get a password dialog box up
when trying to connect from both Windows XP/Windows 7. Analyzing the
error.log on the server I saw:

    [2009/11/11 06:20:12,  0] printing/print_cups.c:cups_connect(103)
    Unable to connect to CUPS server localhost:631 - Interrupted system call

This keeps samba from asking cups and also from complaining about
/etc/printcap missing:

    printing = bsd
    printcap name = /dev/null

> Changes in Samba version 3.4.0

Major enhancements in Samba 3.4.0 include:

The default passdb backend has been changed to 'tdbsam'! That breaks
existing setups using the 'smbpasswd' backend without explicit
declaration!

To stick to the 'smbpasswd' backend try changing this in
/etc/samba/smb.conf:

    passdb backend = smbpasswd

or convert the smbpasswd entries using:

    sudo pdbedit -i smbpasswd -e tdbsam

> Error: Value too large for defined data type

Some applications might encounter this error whith every attempt to open
a file mounted in smbfs/cifs:

     Value too large for defined data type

The solution[2] is to add this options to the smbfs/cifs mount options
(in /etc/fstab for example):

     ,nounix,noserverino

It works on Arch Linux up-to-date (2009-12-02)

  

> Error: Failed to retrieve printer list: NT_STATUS_UNSUCCESSFUL

If you are a home user and using samba purely for file sharing from a
server or NAS you’re probably not interested in sharing printers through
it. If so, you can prevent this error from occurring by adding the
following lines to your /etc/samba/smb.conf:

    load printers = No
    printing = bsd
    printcap name = /dev/null
    disable spoolss = Yes

Restart samba:

    systemctl restart smbd

…check your logs:

    cat /var/log/samba/smbd.log

and the error should now no longer be appearing.

  

> Sharing a folder fails

If sharing a folder from Dolphin (file manager) and everything seems ok
at first, but after restarting Dolphin (file manager) the share icon is
gone from the shared folder, and also some output like this in terminal
(Konsole) output:

    ‘net usershare’ returned error 255: net usershare: usershares are currently disabled

Do the following:

Open /etc/samba/smb.conf as root and edit the section [global]:

      [global]
      .....
      usershare allow guests = Yes
      usershare max shares = 100
      usershare owner only = False
      .....

close the file and do the following afterwards:

    # mkdir /var/lib/samba/usershares
    # chgrp users /var/lib/samba/usershares/
    # chmod 1775 /var/lib/samba/usershares/
    # chmod +t /var/lib/samba/usershares/

Restart the samba daemon

> "Browsing" network fails with "Failed to retrieve share list from server"

And you are using a firewall (iptables) because you don't trust your
local (school, university, hotel) local network. This may be due to the
following: When the smbclient is browsing the local network it sends out
a broadcast request on udp port 137. The servers on the network then
reply to your client but as the source address of this reply is
different from the destination address iptables saw when sending the
request for the listing out, iptables will not recognize the reply as
being "ESTABLISHED" or "RELATED", and hence the packet is dropped. A
possible solution is to add:

    iptables -t raw -A OUTPUT -p udp -m udp --dport 137 -j CT --helper netbios-ns

to your iptables setup.

Retrieved from
"https://wiki.archlinux.org/index.php?title=Samba/Troubleshooting&oldid=303593"

Category:

-   Networking

-   This page was last modified on 8 March 2014, at 12:33.
-   Content is available under GNU Free Documentation License 1.3 or
    later unless otherwise noted.
-   Privacy policy
-   About ArchWiki
-   Disclaimers
