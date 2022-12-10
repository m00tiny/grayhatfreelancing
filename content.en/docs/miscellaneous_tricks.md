+++
title = "Miscellaneous Tips and Tricks"
date = '2022-10-26'
description = 'Miscellaneous useful tips and techniques for using computer systems'
+++
# Miscellaneous & Tricks

All the tricks that couldn't be classified somewhere else.

## Send a message to another user

```powershell
# Windows
PS C:\> msg Swissky /SERVER:CRASHLAB "Stop rebooting the XXXX service !"
PS C:\> msg * /V /W /SERVER:CRASHLAB "Hello all !"

# Linux
$ wall "Stop messing with the XXX service !"
$ wall -n "System will go down for 2 hours maintenance at 13:00 PM"  # "-n" only for root
$ who
$ write root pts/2	# press Ctrl+D  after typing the message. 
```

## CrackMapExec Credential Database

```ps1
cmedb (default) > workspace create test
cmedb (test) > workspace default
cmedb (test) > proto smb
cmedb (test)(smb) > creds
cmedb (test)(smb) > export creds csv /tmp/creds
```

## Local Windows Enumeration

### netstat
 - -a # all active and listening connections
 - -b # find the binaries involved in the connections
 - -n # don't resolve IPs to hostnames
 - -o # display the process ID involved in the connections

```powershell
C:\>netstat -abno

Active Connections

  Proto  Local Address          Foreign Address        State           PID
  TCP    0.0.0.0:22             0.0.0.0:0              LISTENING       2016    [sshd.exe]
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING       924     RpcSs [svchost.exe]
  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING       4       Can not obtain ownership information
  TCP    0.0.0.0:3389           0.0.0.0:0              LISTENING       416     TermService [svchost.exe]
[...]
  TCP    10.20.30.130:22        10.20.30.1:39956       ESTABLISHED     2016    [sshd.exe]
  TCP    10.20.30.130:22        10.20.30.1:39964       ESTABLISHED     2016    [sshd.exe]
```

