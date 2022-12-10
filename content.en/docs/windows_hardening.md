+++
title = "Windows - Hardening"
date = '2022-11-07'
description = 'Some thoughts, ideas, techniques, mandatory changes, etc.. for hardening various Windows environments.'
include_toc = 'true'
+++

# Windows - Hardening

## Essential Concepts to Understand

### Windows Services

Windows Services create and manage critical functions such as network connectivity, storage, memory, sound, user credentials, and data backup and runs automatically in the background. These services are managed by the Service Control Manager panel and divided into three categories, i.e. Local, Network & System. Many applications like browsers and anti-virus software can also run their services for a seamless user experience.

Type `services.msc` in the Run window to access Windows services.

### Windows Registry

The Windows registry is a unified container database that stores configurational settings, essential keys and shared preferences for Windows and third-party applications. Usually, on the installation of most applications, it uses a registry editor for storing various states of the application. For example, suppose an application (malicious or normal) wants to execute itself during the computer boot-up process; In that case, it will store its entry in the Run & Run Once key.

Usually, a malicious program makes undesired changes in the registry editor and tries to abuse its program or service as part of system routine activities. It is always recommended to protect the registry editor by limiting its access to unauthorised users.

Type `regedit` in the Run dialogue or taskbar search to access the registry editor.

### Windows Event Viewer

Event Viewer is an app that shows log details about all events occurring on your computer, including driver updates, hardware failures, changes in the operating system, invalid authentication attempts and application crash logs. Event Viewer receives notifications from different services and applications running on the computer and stores them in a centralised database. 
Hackers and malicious actors access Event Viewer to increase their attack surface and enhance the target system's profiling. Event categories are as below:

 - Application: Records events of system components.
 - System: Records events of already installed programs.
 - Security: Logs events related to security and authentication etc.

We can access Event Viewer by typing `eventvwr` in the Run window. The default location for storing events is `C:\WINDOWS\system32\config\folder`

### Windows Telemetry

Telemetry is a data collection system used by Microsoft to enhance the user experience by preemptively identifying security and functional issues in software. An application seamlessly shares data (crash logs, application-specific) with Microsoft to improve the user experience for future releases.  

Telemetry functionality is achieved by Universal Telemetry Client (UTC) services available in Windows and runs through `diagtrack.dll`. Contents acquired through telemetry service are stored encrypted in a local folder `%ProgramData%\Microsoft\Diagnosis` and sent to Microsoft after 15 minutes or so.

We can access `The DiagTrack` through the Services console in Windows 10.

## Identity and Access Management

### Standard and Admin Accounts

Identity and access management involves employing best practices to ensure that only authenticated and authorised users can access the system. There are two types of accounts in Windows, i.e. Admin and Standard Account. Per best practice, the Admin account should only be used to carry out tasks like software installation and accessing the registry editor, service panel, etc. Routine functions like access to regular applications, including Microsoft Office, browser, etc., can be allowed to standard accounts. Go to `Control Panel > User Accounts` to create standard or administrator accounts.

In either case, a user can authenticate themselves on the system through a password; however, Windows 10 has introduced a new feature called Windows Hello, which allows authenticating someone based on “something you have, something you know or something you are”. 
To access accounts and select the sign-in option, go to `Settings > Accounts > Sign-in` Options.

### User Account Control

User Account Control (UAC) is a feature that enforces enhanced access control and ensures that all services and applications execute in non-administrator accounts. It helps mitigate malware's impact and minimises privilege escalation by bypassing UAC. Actions requiring elevated privileges will automatically prompt for administrative user account credentials if the logged-in user does not already possess these.
For example, installing device drivers or allowing inbound connections through Windows Firewall requires more permissions than already available privileges for a standard user.

As a principle, always follow the Principle of Least Privilege, which states that (Per CISA) “a subject should be given only those privileges needed for it to complete its task. If a subject does not need an access right, the subject should not have that right”.
To access UAC, go to `Control Panel -> User Accounts` and click on `Change User Account Control Setting`.

### Local Policies and Group Policy Editor

Group Policy Editor is a built-in interactive tool by Microsoft that allows to configure and implement local and group policies. We mainly use this feature when part of a network; however, we can also use it for a workstation to limit the execution of vulnerable extensions, set password policies, and other administrative settings.
Note: The feature is not available in Windows Home but only in the Pro and Enterprise versions.

### Password Policies



One primary use of a local policy editor is to ensure complex and strong passwords for user accounts. For example, we can design password policies to maximise our security:

 - Passwords must contain both uppercase and lowercase characters.
 - Check passwords against leaked or already hacked databases or a dictionary of compromised passwords.
 - In case of 6 failed login attempts within 15 minutes, the account will remain locked for at least 1 hour.

We can access Password policies through the Local group policy editor.
`Security settings > Account Policies > Password policy`

### Setting a Lockout Policy

To protect your system password from being guessed by an attacker, we can set out a lockout policy so the account will automatically lock after certain invalid attempts. To set a lockout policy, go to `Local Security Policy > Windows Settings > Account Policies > Account Lockout Policy` and configure values to lock out hackers after three invalid attempts.

## Network Management

### Windows Defender Firewall

Windows Defender Firewall is a built-in application that protects computers from malicious attacks and blocks unauthorised traffic through inbound and outbound rules or filters. As an analogy, this is equivalent to “who is coming in and going out of your home”.
Malicious actors abuse Windows Firewall by bypassing existing rules. For example, if we have configured the firewall to allow incoming connections, hackers will try to manipulate the functionality by creating a remote connection to the victim's computer.
You can see more details about Windows Firewall Configuration here.
We can access Windows Defender Firewall by accessing WF.msc in the Run dialogue.

As mentioned in the Windows Fundamentals room, it has three main profiles Domain, Public and Private.  The Private profile must be activated with "Blocked Incoming Connections" while using the computer at home. 
View detailed settings for each profile by clicking on Windows Defender Firewall Properties.
Whenever possible, enable the Windows Defender Firewall default settings. For blocking all the incoming traffic, always configure the firewall with a 'default deny' rule before making an exception rule that allows more specific traffic.

### Disable unused Networking Devices

Network devices like routers, ethernet cards, WiFI adapters etc., enable data sharing between computers. If the device is improperly configured or not being used by the owner, it is recommended to disable the interface so that threat actors cannot access them and use them for data retrieval from the victim's computer.
To disable the unused Networking Devices, go to the Control panel > System and Security Setting > System > Device Manager and disable all the unused Networking devices.

### Disable SMB Protocol

SMB is a file-sharing protocol exploited by hackers in the wild. The protocol is primarily used for file sharing in a network; therefore, you must disable the protocol if your computer is not part of a network by issuing the following command in PowerShell. 

### Protecting Local Domain Name System (DNS) 

The domain name system (DNS) is a naming system that translates Fully Qualified Domain Names (FQDN) into IP addresses. If the attacker places himself in the middle, he may intercept and manipulate DNS requests and point them to attacker-controlled systems since DNS replies are neither authenticated nor encrypted.
The hosts file located in Windows acts like local DNS and is responsible for resolving hostnames to IP addresses. Malicious actors try to edit the file's content to reroute traffic to their command and control server.
The hosts file is located at C:\Windows\System32\Drivers\etc\hosts.

### Mitigating Address Resolution Protocol Attack  

The address resolution protocol resolves MAC addresses from given IP addresses saved in the workstations ARP cache. The ARP offers no authentication and accepts responses from any user in the network. An attacker can flood target systems with crafted ARP responses, which point to an attacker-controlled machine and put him in the middle of communication between the targeted hosts.


The table contains MAC addresses in the middle and IP addresses in the left.  If the table includes a MAC mapped to two IPs, you are probably susceptible to an ARP poisoning attack.
To clear the ARP cache and prevent the attack, issue the command arp -d.

### Preventing Remote Access to Machine

Remote access provides a way to connect to other computers/networks even located at a different geographical location for file sharing and remotely make changes to a workstation. Microsoft has developed a Remote Desktop Protocol (RDP) for connecting with other computers. Hackers have exploited the protocol in the past, like the famous Blue Keep vulnerability, to gain unauthorised access to the target system.
We must disable remote access (if not required) by going to settings > Remote Desktop. Do not attempt this in VM attached to this room.

## Application Management

### Trust Application Store

Microsoft Store offers a complete range of applications (games, utilities) and allows downloading non-malicious files through a single click. Malicious actors bind legitimate software with trojans and viruses and upload it on the internet to infect and access the victim's computer. Therefore, downloading applications from the Microsoft Store ensures that the downloaded software is not malicious. 
We can access Microsoft Application Store by typing ms-windows-store in the Run dialogue.

### Safe App Installation

Only allow installation of applications from the Microsoft Store on your computer.  
Go to Setting > Select Apps and Features and then select The Microsoft Store only. 

### Malware Removal through Windows Defender Anti-Virus
Windows Defender Anti Virus is a complete anti-malware program capable of identifying malicious programs and taking remedial measures like quarantine. The program used to have an entire Graphical User Interface; however, Windows 10 and newer versions manage the same through Windows Security Centre. Windows Defender primarily offers four main functionalities:

 - Real-time protection - Enables periodic scanning of the computer.
 - Browser integration - Enables safe browsing by scanning all downloaded files, etc.
 - Application Guard - Allows complete web session sandboxing to block malicious websites or sessions to make changes in the computer.
 - Controlled Folder Access - Protect memory areas and folders from unwanted applications.

### Microsoft Office Hardening

Microsoft Office Suite is one of the most widely used application suites in all sectors, including financial, telecom, education, etc. Malicious actors abuse its functionality through macros, Flash applets, object linking etc., to achieve Remote Code Execution. 
Hardening of Microsoft Office may vary from person to person as legitimate functionality of Microsoft Office is exploited to gain access. For example, disabling macros in a University may be helpful as no one uses it; however, banks cannot disable macros as they heavily rely on complex invoices and formulas through macros. 
The attached VM contains a batch file based on best practices and Microsoft Attack Surface Reduction Rules for hardening Microsoft Office. To execute the script, right-click on the file office.bat on Desktop and Run as Administrator.

### AppLocker

AppLocker is a recently introduced feature that allows users to block specific executables, scripts, and installers from execution through a set of rules. We can easily configure them on a single PC or network through a GUI by the following method:

### Browser (MS Edge)

Microsoft Edge is a built-in browser available on Windows machines based on Chromium, inline with Google Chrome and Brave. The browser often acts as an entry point to a system for further pivoting and lateral movement. It is therefore of utmost importance to block and mitigate critical attacks carried out through a browser that include ransomware, ads, unsigned application downloads and trojans. 

### Protecting the Browser through Microsoft Smart Screen

Microsoft SmartScreen helps to protect you from phishing/malware sites and software when using Microsoft Edge. It helps to make informed decisions for downloads and lets you browse safely in Microsoft Edge by:
Displaying an alert if you are visiting any suspicious web pages.
Vetting downloads by checking their hash, signature etc against a malicious software database.

Protecting against phishing and malicious sites by checking visited websites against a threat intelligence database.To turn on the Smart Screen, go to Settings > Windows Security > App and Browser Control > Reputation-based Protection. Scroll down and turn on the SmartScreen option.

Open Microsoft Edge, go to Settings and then click “Privacy, Search and Services” - Set "Tracking prevention" to Strict to avoid tracking through ads, cookies etc.

## Storage Management

### Data Encryption through BitLocker

Encryption of the computer is one of the most vital things to which we usually pay little attention. The worst nightmare is that someone gets unfettered access to your devices' data. Encryption ensures that you or someone you share the recovery key with can access the stored content.
Microsoft, for its business edition of Windows, utilises the encryption tools by BitLocker. Let us have a quick look at how one can ensure to protect the data through BitLocker encryption features available on the Home Editions of Windows 10. You have already read about it here (Task 8).
Go to Start > Control Panel > System and Security > BitLocker Drive Encryption. You can easily see if the option to BitLocker Drive Encryption is enabled or not. 

A trusted Platform Module chip TPM is one of the basic requirements to support BitLocker device encryption. Keeping the BitLocker recovery key in a secure place (preferably not on the same computer) is imperative. You can read more about BitLocker Recovery here.


### Windows Sandbox

To run applications safely, we can use a temporary, isolated, lightweight desktop environment called Windows Sandbox. We can install software inside this safe environment, and this software will not be a part of our host machine, it will remain sandboxed. Once the Windows Sandbox is closed, everything, including files, software, and states will be deleted. We would require Virtualisation enabled on our OS to run this feature. We cannot try this in the attached VM but the steps for enabling the Sandbox feature are as below:
Click Start > Search for 'Windows Features' and turn it on > Select Sandbox > Click OK to restart 

If you want to close the Sandbox, click the close button, and it will disappear. Opening suspicious files in a Windows Sandbox before blindly executing them in your base OS is recommended.

### Windows Secure Boot

Secure boot – an advanced security standard - checks that your system is running on trusted hardware and firmware before booting, which ensures that your system boots up safely while preventing unauthorised software access from taking control of your PC, like malware.
You are already in a secure boot environment if you run a modern PC with Unified Extensible Firmware Interface UEFI (the best replacement for BIOS) or Windows 10. You can check the status of the secure boot by following:

The incredible thing is that you do not need to enable or install it as it works silently in the background. Windows allows you to disable these features, which is not recommended.  You can enable Secure boot from BIOS settings (if disabled).

### Enable File Backups

The last option, but certainly not the least important one to prevent losing irreplaceable and critical files is to enable file backups. Despite all the above techniques, if you somehow lose essential data/files, you can recover the loss by restoring it, if you have a file backup option. Creating file backups is the best option to avoid disasters like malware attacks or hardware failure. You can enable the file backup option through  Settings > Update and Security > Backup:- 

Therefore, the most convenient option is enabling it from the 'File History' option - a built-in functionality of Windows 10 and 11.
