---
layout: doc
h3InToc: true
contributedBy: Patrick Coble, Martin Zugec
description: Copy & paste description from TOC here
---
# Citrix Virtual Apps and Desktops VDA Hardening Guide

When deploying any operating system, the settings are always targeted for the most compatible settings to ensure the device works along with it is the most backwards compatibility. The Windows Operating system is no different, even the latest Windows releases have settings to allow it to work with releases from 20+ years ago along with very few restrictions of what can be done within the operating system. Windows does have its built in Firewall, Antivirus and Update settings that will allow some protections, but a user can launch anything they have access to and by default they have access to most everything besides what is protected by local administrative access.

In the following sections we will cover the most recommended areas from how to planning to get started, how to configure some recommended policies, control privileged access and even configure some security based windows features. Most of these sections will be broken into these three sections minimum, recommended and high security. The minimum recommendations are just that a starting point at default or just beyond default settings, this will provide some protections, but should provide the most application compatibility and usability also. The recommended settings will start to secure the system further that will be able to start preventing some common attack methods while allowing the most common application compatibility and usability requirements too. The high security recommendations will provide the most secure deployment options with the most restrictive usability and the most targeted application compatibility also.

All these settings should be deployed in a test scenario and validated by your IT team and then scheduled and promoted to your test users before finally being promoted into production. With each level of recommendations, the risk of causing a useability or application compatibility issue will increase and should require further testing and tuning.

TODO Disclaimer from legal

## Planning

Planning is the one of the most crucial steps before starting to harden your VDAs operating system. These items below will apply to all three levels of recommendations (Minimum, Recommended and High Security) as planning is foundational to any successful and secure deployment.

### What will be published?

There are three main options in Citrix to deliver resources with Published Applications (Single Application), Published Desktops (Virtual Desktop) and Remote PC Access (Secure Connection to an existing VDA). With each of these publishing methods we recommend applying the same policies to each of these systems since they will be accessed remotely.

#### Minimum, Recommended and High Security

We recommend creating a list of all resources that will need to be published and therefore installed into the VDA to create a usable system for your users. This will help you also collect information below that will be helpful for further hardening of the system too.

Example:

-  Published Application EMR
-  Published Desktop EMR + Microsoft Office
-  Remote PC Access Accounting Application

### Which Operating System Version

Each operating system will have generic security recommendations, but they will also have specific recommendations based on specific features that are only in those specific versions.

#### Minimum, Recommended and High Security

We recommend creating a list of each operating system and build number for each published resource. Typically, there will be some overlap as the same VDA image may be used for multiple use cases along with even multiple publishing methods too. This will help you also collect information below that will be helpful for further hardening of the system too.

Example:

-  Published Application Windows Server 2019, Build 1809 (EMR Image)
-  Published Desktop Windows Server 2019, Build 1809 (EMR Image)
-  Remote PC Access Windows 10, Build 1909 (Finance Desktops)

### Software Requirements

The version of the software deployed in any operating system will also affect the recommend deployment and security settings that should be deployed.

#### Minimum, Recommended and High Security

Is the software supported by the vendor? This will dictate if there is support from the vendor if there is an issue along with if updates and security updates are being released. There are some instances where legacy software has to be used and the risk of using it has already been accepted by the business.

Is the software supported on your targeted operating system? This is another level of support that will also dictate what version of operating system you must use along with if you will be running a supported operating system too. Having an unsupported operating system is one of the riskiest items a deployment can have. Without having support from the operating system vendor, you may not have access to support if there is an issue, but most importantly you may not have access to security patches which can leave the system vulnerable to attacks. This could allow an attacker to more easily compromise the system and without support of the operating system the vendor will not be accountable. This can also impact the effectiveness of Cyber Security Insurance and other legal implications if an attack happens due to this weakness.

Example:

-  EMR Supported on Windows Server 2019 1809 to Windows 10 1909 or newer with known issues with 20H2 at this time. Requires Office 2016 or Newer. Requires Internet Explorer.
-  Office Supported on Windows Server 2019 1809 to Windows 10 1909 or newer with no known issues with the latest version of Windows 10 or Windows Server

### User Requirements

These will encompass what the user needs outside of the application and within the actual Citrix remote session. Many recommended settings could impact the usability of the system and the applications that are required within the session. This level of planning will help with the sections below as it will determine what settings can be configured based on the possible impact to the user workflow. We want to ensure that these items are documented so when there is an audit, or a new deployment is needed the user requirements are documented.

#### Minimum, Recommended and High Security

What does the user need to do within this application that must be allowed within the operating system? This will include core windows systems\applications like Windows Explorer to include mapped drives, control panel applets, start menu and desktop shortcuts and many others.

Does the application require administrative rights? There are some applications that require access to core operating system files or registry items. We recommend auditing these requirements from the vendor or from debugging using tools like Microsoft Process Monitor [https://docs.microsoft.com/en-us/sysinternals/downloads/procmon](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) to see the programs, .DLLs and registry items that are being accessed and what is getting "Access Denied" so that permissions can be adjusted. It is recommended to either use the same delivery group entitlement group or create AD groups to give access to these specific items.

Does the application open other applications? Many applications will require other applications to be launched for the workflow of the application to be useable. Some will require core operating system items and other will just open other installed applications. Documenting these dependencies will also help understand the requirements and possible support implications of those application relationships.

What session channels will they need access to? The most common requirements are having access to Print, Copy\Paste and access to other devices within the session. Each one of these session items have corresponding policies within Citrix and also some OS policies too.

Example:

-  Published Application-EMR Image Application Requirements: Microsoft Excel, Internet Explorer) OS Requirements File Explorer for File Shares
-  Published Desktop-EMR Image Same as above
-  Remote PC Access Application Requirements: Microsoft Office, Web Browser (No Specific Required) OS Requirements File Explorer for File Shares

### Compliance Requirements

Depending on what compliance bodies your business or application can drastically change the recommended settings as many are required by certain compliance bodies for specific operating systems and applications. Many compliance bodies are focused on change control procedures, logging, incident response and other IT business operations, but many also will require very specific settings to be deployed and for those same settings to be auditable by configured policies and possible Red Team testing of those controls.

#### Common Compliance Bodies

-  NIST (National Institute of Standards and Technology)
-  CIS Controls (Center for Internet Security Controls)
-  ISO (International Organization for Standardization)
-  HIPAA (Health Insurance Portability and Accountability Act) / HITECH Omnibus Rule
-  PCI-DSS (The Payment Card Industry Data Security Standard)
-  GDPR (General Data Protection Regulation)
-  CCPA (California Consumer Privacy Act)
-  AICPA (American Institute of Certified Public Accountants)
-  SOX (Sarbanes-Oxley Act)
-  COBIT (Control Objectives for Information and Related Technologies)
-  GLBA (Gramm-Leach-Bliley Act)
-  FISMA (Federal Information Security Modernization Act of 2014)
-  FedRAMP (The Federal Risk and Authorization Management Program)
-  FERPA (The Family Educational Rights and Privacy Act of 1974)
-  ITAR (International Traffic in Arms Regulations)
-  COPPA (Children's Online Privacy Protection Rule)
-  NERC CIP Standards (NERC Critical Infrastructure Protection Standards

#### Minimum, Recommended and High Security

We recommend following the guidelines from each compliance body at a minimum, but if possible, depending on those requirements we recommend evaluating other common frameworks from Microsoft, NIST and even third parties like CIS and HyTrust for very specific recommendations for Domains, Desktops, Servers and more. These frameworks have many options to make the deployment much more secure and reduce your attack surface along with helping accelerate your audits and reduce your findings.

## Reducing Attack Surface

One of the first steps to reducing the attack surface is to remove unnecessary software and services to help reduce the attack surface. The easiest way to accomplish this is a twofold approach.

Optimization is great for User and Resource performance but it also a very important to security as the less running software the more secure the system will be.

Reference Citrix Optimizer and write some on the type of Software (UWP), Scheduled Tasks and some of the other things that can be directly tied to security performance too.

### Minimum, Recommended and High Security

We first want to ensure that only required software is installed. Each piece of software installed on the system can create possible vulnerabilities that could be exploited.

We want to also ensure that the version of software is the latest version if possible and is supported by that vendor. Most pieces of software will have security vulnerabilities discovered and eventually remediated with a patch and/or software revision.

We then want to ensure that any necessary services are disabled from the operating system by using an OS optimizer. Most optimizations will help with the user density by disabling and configuring items that are not needed in a VDI deployment, but there are also security benefits by removing and configuring these same components. The highest percentage of density benefits comes from removing Windows Programs (UWP) and disabling scheduled tasks in Windows 10 operating systems. We recommend using Citrix Optimizer as it has been tuned to provide the most benefits with the least impact to the user's workflow in our experience. You can download this software here [https://support.citrix.com/article/CTX224676](https://support.citrix.com/article/CTX224676).

## Windows Policies

This is the core to any securing any Windows Operating System. The default policies for the operating system are focused on compatibility and useability first and security settings must be added to the configuration. This section will focus on Windows over Linux VDAs as Windows is still the majority of Citrix Virtual Apps and Desktops deployments. There are thousands of Group Policy Settings and working on them in sections based on your users' requirements is the best approach. Below are some of the types of Applications and areas you should focus on, but we also recommend making decisions on each of these settings and validating it will meet your users' requirements. The sections below will just point to the areas of the Group Policy that should be evaluated with some recommendations and not each specific policy. Having a standard for all Desktops and Servers is recommended so that all your systems can have a similar effective policy, there will be deviations that can be architected by other policies or device placement in specific OUs. Administrative deviations are possible by using an AD group with "Deny Policy" advanced permissions, so these settings don't apply to admins.

### Minimum

#### File Explorer

We recommend evaluating the File Explorer policies to see if hiding some of the menus like Favorites, Network Locations and other file locations that may not be necessary for the targeted job profile. There are items like Videos, Pictures and Music that are typically not needed by a corporate operating system.

#### Start Menu Layout

We should strive to ensure that users have access to only the shortcuts they need based on their job profile. This means we should ensure that shortcuts for necessary programs and admin only items are removed and/or hidden from their Start Menu view. We recommend either copying the whole start menu to a secure directory on the system that could still be used by Admins or use FSLogix to hide all these shortcuts for users only so Admins will have a full Start Menu.

#### Desktop Settings

Allow Edit\Creation of Shortcuts, we want to ensure that the users have the shortcuts on the desktops that they need, but we also don't want users to be able to add or create items to unintended locations. By controlling the adding and editing of these items on the Desktop we can prevent that from happening. This can have an impact on some users if they use their Desktop to store all their documents or other items. The Documents folder is the most appropriate place to store documents for the user and using Folder Redirection or a Profile Solutions from Citrix, Microsoft FSLogix or others to help retain items in a seamless way for the users. This can also be accomplished by ensuring the NTFS permissions of the shortcut don't allow editing by the user but still allow them to add items but not edit them.

#### Prevent Access to Registry Editing GPO or hide the shortcut

This will remove access to the registry editor which most job profiles do not require. This can also be accomplished by using Microsoft FSLogix from hiding and preventing access to regedit.exe as a shortcut and within the file system.

#### Auditing Settings - Local Log Retention

We recommend designing your VDA deployment to be able to host some local log retention for the Application, System and Security logs regardless of the provisioning method. Having some local log retention for troubleshooting is very valuable on each VDA, but it can be critical if you do not have a Windows Event Log forwarding or a SIEM agent installed on them. Without having logs during a security incident could prevent you from conducting an investigation and could also have legal implications depending on your compliance body or cyber insurance. With the rapid increase is storage technologies the storage implications of having local log retention are greatly diminished. We recommend using 200mb to 1gb for each log type to have a couple hours to a couple days of log retention locally based on the system usage.

### Recommended

#### System – Hide Specified Drives

We recommend hiding all drives within File Explorer. The default permissions of a Windows system can allow users to access and write items to many locations in the file system. Most deployments can have the drives hidden and still their mapped drives to access the files they need.

#### Start Menu - Remove Run from Start Menu

This will remove command execution possibilities for File Explorer, Internet Explorer, Task Manager and removes it from the Start Menu. This setting should be tested with your user's workflows, but most job profiles don't need access to command execution as they just use installed applications and shortcuts to navigate and launch them.

#### System - Prevent Access to Command Prompt GPO or hide the shortcut

This will remove access to the Command Prompt in the User context from the Start Menu and within the file system also. This will also prevent someone from launching a .cmd or .bat file and can prevent some login scripts from running. We recommend using User Environment Management solution to run your user personalization scripts if possible, this setting should still be tested with them as it may still cause issues depending on that system. This can also be accomplished by using Microsoft FSLogix from hiding and preventing access to cmd.exe as a shortcut and within the file system.

#### Hide Specified Control Panel Items or Prohibit access to Control Panel and PC Settings

This will remove access to specific Control Panel Applets or can hide all of them based on your needs. In many cases there may not be a need for a user to have access to the Control Panel at all or just to very specific applets to help configure or troubleshoot applications especially if there is device mapping involved.

#### Browser Settings – Internet\Intranet Access Required

We recommend evaluating that if Internet and Intranet Access is required for the Published Application or Desktop. There are many deployments that do not require access to the web locally or globally and we recommend blocking this by using Proxy and/or your Content Control solution for those users and/or those Machine Catalogs depending on which options is configured. There are simple options if no internet\intranet access is needed then a simple proxy configuration on the browser to the local loopback IP address will prevent access without any other systems. Since most attacks happen from email and malicious website it is worth investigating your user's requirements to help eliminate and or lower the risk for some or all of your users

#### Browser Settings – Content Control

We recommend ensuring there is some form of Content Control with some basic Allow, Deny Lists and/or DNS protection for known Malicious IPs. There are many options for this option and your abilities will be controlled by what Content Control solution you have, what firewall you own and what licensed features available along with what DNS protection you have available to your deployment too. Each of these options depending on the vendor and the location of the VDAs can make this configuration easier or more difficult. We also recommend evaluating the security settings so that these settings cannot be bypassed by users.

#### Browser Settings – Cookie Settings

We recommend evaluating your cookie requirements for your business web applications and adjusting your browsers settings to those cookie requirements. Cookies are useful tokens that are used by websites for authentication, but they are also used for tracking and can be used maliciously to. By setting a cookie processing standard it could create errors or nonfunctioning websites that nonbusiness applications. We believe the IT team and business should create a policy of what the nonbusiness web applications standards are as by doing that will increase the risk of error when accessing non-business applications as they could be using third-party cookies. We also recommend evaluating the security settings so that these settings cannot be bypassed by users.

#### Browser Settings - TLS Settings

We recommend evaluating the lowest encryption standard required for business web applications and adjusting your browsers settings to that TLS\SSL version. There are newer TLS\SSL versions that are coming out almost yearly and each one provides more cryptographic protection. By setting a TLS\SSL version standard it could create errors or nonfunctioning websites that nonbusiness applications. We believe the IT team and business should create a policy of what the nonbusiness web applications standards are as by doing that will increase the risk of error when accessing non-business applications as they could be using lower TLS\SSL versions. We also recommend evaluating the security settings so that these settings cannot be bypassed by users.

#### Browser Settings - Security Settings

We recommend evaluating your browser security settings as it relates to making changes to settings. When configuring any browser setting most browsers will have policies to prevent changing of those policies along with other settings. If web security browsing standards are created, you will not want users to be able to deviate these policies other than very specific user groups.

#### Supporting Applications Update and Security Settings

There are many supporting application's that may require special settings when it comes to policies and may require loading ADMX files to control them. These application's settings can drastically affect the security of your deployment along with their overall maintenance too.

#### Adobe and Java - Update Settings

One of the first settings most deployments need to do is disable automatic updates notifications and set the update process based on your patching tempo. Keeping these products up to date usually will directly relate to its security risk.

#### Adobe and Java – Security Settings

Each of these products have security settings that control their operations, and these should be evaluated based on your user's requirements and their workflow. There are policy guides from each of these vendors that can be helpful with the ADMX files to determine what to configure. These settings will need to be validated with your users that they do not impact the user's workflow.

#### Microsoft Office – Macro Settings

This is one of the most common exploit entry points in many attacks. These policies can control many levels of what Macros can be executed. There are workarounds and new exploits found at least yearly, but our recommendation is to disable Macros for all users and only enable them to just those specific users as the risk of possible compromise is too much.

### High Security

System - Restrict Users to the explicitly permitted list of snap-ins'

This setting will allow you to specify the permitted MMC snap-ins or if that policy is left blank then no MMCs can be loaded. As a user there are many sensitive MMCs that can expose information about the system and give access to unintended items.

#### System – Restrict Access to Specified Drives

Hiding the specified drives is a good first step but if possible, you should be able to restrict access to specified system drives to further secure your deployment. The default permissions of a Windows system can allow users to access and write items to many locations in the file system. This is a more restrictive system setting that depending on the application and workflow they could be impacted and will require more testing.

#### File Explorer Security Settings – Menu Bars

These settings can allow you to manipulate the settings withing File and Internet Explorer and other areas of the operating system to remove and hide options within these areas. These items will require testing with your users as this will limit their ability to do certain items within the OS.

#### File Explorer Security Settings – Help Menu

This is a common jailbreak point in many applications. There group policy will allow you to enter a list of comma deiminated executables that anything launched from help will be prevented from running. We recommend entering at least these common administrative executables to prevent their execution. If you have other browsers installed on the system, we recommend adding those executable names also. Most applications can have their help launches restricted with this policy without issue because they are not typically needed for the workflow

-  exe,cmd.exe,regedit.exe,mmc.exe,powershell.exe,PowerShell_ISE.exe

#### Windows Logging – Windows Event Log Forwarding

Local log retention is a good starting point but depending on the amount of space allocated and the number of events that may not provide you enough time or events for a proper incident response. Having good event logs can be very helpful for troubleshooting and especially for security incident response too. Windows Event Log Forwarding is a built-in feature that just requires storage and the servers and configuration list within these guides from Microsoft. From there with WEF you may also be able to setup an alerting from other providers to so you can be alerted when certain events happen. Many clients will have an existing Security information and event management which may use WEF to feed events or will have their client on to gather the logs from each system. The benefit of a SIEM system is the alerting that can be setup. Many of these systems will have some built in dashboards and alerts for known bad event IDs.

#### Windows Logging – PowerShell Logging and Transcription

PowerShell has become the most common Windows scripting languages used and by that same popularity for developers and admins it is also a very common attack method too. Knowing what PowerShell commands were ran. Depending on your SIEM there may be built-in alerts looking for long commands more than 30 characters. We recommend setting up a similar alert from your system if it isn't built in for your SIEM. This number of characters to trigger this alert may have to be adjusted or suppressed for some of your recurring scripts that run. Just like with any alerts you want get them tuned so that hopefully only actionable alerts are sent out.

#### Windows Logging – Advanced Logging Settings

Even with event log forwarding enabled or a SIEM client installed without these advanced logging settings properly configured the events are not logged. We recommend looking at Microsoft recommendations here: [https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations) Many of these settings should be paid special attention to for Active Directory as many cyber-attacks will be focused there and logging and detection is key.

#### Microsoft Office – Default File Open and Save Location

These settings will control what directory Microsoft Office Documents will open files from and save files to. This also works well with your document retention solution like file redirection or your profile solution. If you are using Hide or Restrict Specified Drives and you have not configured this to the designated area when a file is saved it can navigate into the OS drive outside of areas that you have hidden. This access from these actions could allow a user or attacker to access unintended areas of the OS through the file system. We also recommend looking at your other applications to ensure they do not have a default open or save location outside of the user's profile or your specified location for document retention.

#### Microsoft Office – Other Security Settings

We recommend evaluating the many other security settings within the Office group policy settings for your deployment.

## Session Policies

We recommend creating a session requirements matrix for each use case to control what session policies will need to be configured per job profile. We recommend creating a Zero Trust Policy for all session policies by using the "Security and Control" Policy Template to start any Citrix Virtual Apps and Desktop deployment to ensure that all session channels are blocked by default. If any session policies need to be adjusted, we recommend creating a policy for each policy deviation and using Active Directory groups to nest your delivery group entitlement groups in to make a seamless single group entitlement for the users.

This is the link of the default policies for reference and a list of the policies we focus on when it comes to the recommended Security Sections

[https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/policies/policies-default-settings.html](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/policies/policies-default-settings.html)

### Default Policy

| Policy Name                | Default Setting |
| -------------------------- | --------------- |
| Clipboard Redirection      | Allowed         |
| Clipboard Formats          | No Restriction  |
| Audio Out                  | Allowed         |
| Microphone                 | Allowed         |
| Auto Connect Client Drives | Allowed         |
| Client Drive Redirection   | Allowed         |
| Client Fixed Drives        | Allowed         |
| Client Floppy Drives       | Allowed         |
| Client Network Drives      | Allowed         |
| Client Optical Drives      | Allowed         |
| Client Removable Drives    | Allowed         |
| Auto Connect LPT           | Prohibited      |
| Auto Connect COM           | Prohibited      |
| Client Printing            | Allowed         |
| Client TWAIN Devices       | Allowed         |
| Client USB Devices         | Prohibited      |

We recommend using this detailed session policy questionnaire to determine which job profiles may require which virtual channels. The most common requirements are having access to Print, Copy\Paste and access to other devices within the session. Each one of these session items have corresponding policies within Citrix and also some OS policies too.

### High Level Session Policy Question

What do you need outside of just your keyboard and mouse in Application X/Desktop Y?

This question should start your users thinking about their workflow beyond asking questions for each virtual channel below.

### Clipboard

Do you need to copy and paste things in and or out of the Session? If the clipboard is needed, do you need just text or other format types and then which directionality is required in and out of the session or just within the session only.

Just out of the Session?

### Drive Mappings

Do you need to Copy/Move anything from and drives on your computer in and or out of the Session? Local C Drive, Network Drives Mapped to the Computer, Removable Media, Optical or Floppy

### USB Devices

Do you have to use any USB devices with your session? If a USB device is needed, we recommend collecting the VID and PID for those devices instead of just enabling all USB device types.

### Printing

Do you need to Print? Most customers need it but, there are instances where it should be disabled for Contractors\Third Parties or just different business units.

### Audio

Do you require audio out and or a microphone in? The audio virtual channel is only usually sensitive when it is in the medical dictation role or when there are sensitive meeting recordings that could have SEC or other compliance implications.

### Misc. Ports

Do you require COM or LPT Ports? These are becoming less prevalent but in certain industries it is still a requirement.

Example:

-  Published Application-EMR Image Session Requirements: Copy\Paste within the Session Only, Client Printing (Default Printer Set no need for the Control Panel)
-  Published Desktop-EMR Image Session Requirements: Copy\Paste into the Session Only, Client Printing (Default Printer Set no need for the Control Panel)
-  Remote PC Access Session Requirements: Copy\Paste into the Session, Client Printing (Default Printer Set no need for the Control Panel)

## Operations

Missing operating system patches are one of the most prolific finding in security audits. There are typically two reasons why patches are not done regularly, first there isn't a set schedule and time set aside each month for them and that there have been application compatibility issues in the past with patches.

### Minimum

#### OS and Application Patching Schedule

Based on the Windows release cycle we can expect to update the operating system at least every month and at around 2 times a year a critical patch must be applied along with the OS will now need to be upgraded every 30-60 months depending on the version chosen. This means we should plan for at least 12 updates a year and have a process to deploy at least one other patch per day. With these anticipated updates for the operating system, it can be very helpful to provide application owners access to that same time window for testing and promotion of these changes. Working with your IT team to create a process and time will increase the security of your deployment.

### Recommended

#### Purpose Built Updatable Image Design

We recommend creating a process for implementing and promoting image updates that can lower the risk and impact if issues arise. Depending on your Machine Catalog, Delivery Group and hosting design will determine what will be possible. We recommend having a Test Machine Catalog and Delivery Group that initial updates are applied to and then promoted to a Quality Assurance (QA) Catalog and Group for Application Owner testing and validation. Then if you are able to have a Pre-Production Machine Catalog in a Production Delivery Group to reduce the impact in the same Production delivery group. Using more than one Machine Catalog in the same Delivery Group will allow you to have a percentage of your VDAs on the new version and the others will be on the existing version. This may not be possible depending on the application update requirements but shouldn't be impacted by OS updates. There have been issues with OS updates that are not found until they are rolled out and have more than one user or they are load based issues so if you can afford to have the extra capacity it will help lower your risk while increasing your security.

Example:

-  Published Application-EMR Image (90 Users, 30 Users Per Server, 4x VDAs, 1x Delivery Group with 3x Prod Machine Catalog, 1x PreProd Machine Catalog and one Test Delivery Group (Master Image) and one QA Delivery Group with a single VDA)
-  Published Desktop-EMR Image (90 Users, 30 Users Per Server, 4x VDAs, 1x Delivery Group with 3x Prod Machine Catalog, 1x PreProd Machine Catalog and one Test Delivery Group (Master Image) and one QA Delivery Group with a single VDA)
-  Remote PC Access SCCM Patches Systems 7 Days after Patch Tuesday over the weekend with follow up on Monday and Tuesday.

## Application Control

Having an Application Control solution is imperative in the current cyber threat climate. Ensuring that not just any application can be launched is a cornerstone to OS security. A user being able to execute anything they can access is very risky and we recommend deploying an application control solution. There are many options when it comes to an Application Control solution that are included with Windows or are paid addons. When we publish a specific application or a desktop to access a specific application or applications most users only need to run that specific program or programs. There are some applications that will launch other supporting applications and must be included in an application control solution. In the planning phase you should have a list of primary executables and supporting executables to start this testing a solution listed below.

### Microsoft Windows AppLocker

Policy based with Allow Lists and Block Lists. You will use Group Policy to create and edit these policies and apply them to OUs and/or filter based on AD groups to meet your application control needs.

### Windows Defender Application Control

This Requires Windows Defender to Run Script runs on the system to mark trusted and installed files and only those files will be allowed to run

### WEM

Uses the native Microsoft Windows AppLocker system but allows you to configure these settings in the same context

### Third Party

#### Antivirus

Most Antivirus solutions have the ability to control applications launches. This can provide the benefit depending on who is managing the system that the same console can be used to manage these policies also.

#### PolicyPak, Ivanti and Others

These solutions can also be used as an Application Control solution and they also provide other benefits too.

### Minimum

#### Choosing a Solution

The first step is to choose which Application Control solution will work best for your team. There are many differences between each of these solutions and based on your team's expertise, how your privileged account management delegation is setup, the number of applications to define along with what solutions you already own.

#### Implement in Audit Only Mode

If your solution supports an audit only mode, we recommend deploying it to existing VDAs to understand what will need to be in the allow list. In many cases may catch other executables that were not noted in the planning phase. This may require Windows Event Log Forwarding depending on the solution selected. With the list of executables that could be discovered this may also change which solution you may want to deploy based on the numbers of applications to define or the number of groups they must each be delegated to.

### Recommended

#### Enable Solution – Block Admin Applications

We recommend enabling the block ability from your Application Control Solution for just administrative programs.

#### Recommend Admin Applications to Block

Most deployments don't require users access to the PowerShell command line or the editor. Currently there isn't a single GPO that will prohibit access to PowerShell that is equivalent to the command prompt. If there are other programs in use we recommend disabling those too.

PowerShell.exe

PowerShell_ISE.exe

### High Security

#### Allow List Only

After testing and validation we recommend setting your solution to just allow execution of the defined applications only in the user context. This can be a very long process depending the number of applications, the amount of delegation deviations and which solution was selected. We recommend working with the simplest images first if possible, with the least number of installed applications. It is not recommended to attempt to restrict all application launches on all images at once. We recommend using the same groups use for Delivery Group entitlements to help filter the policies to control which applications can be launched on the same image by multiple groups and/or multiple images.

Example:

-  Published Application EMR (Only, Filtered by Delivery Group)
-  Published Desktop EMR + Microsoft Office (Filtered by Delivery Group)
-  Remote PC Access Accounting Application (Only, Filtered by Delivery Group)

## Endpoint Protection

Endpoint protection is paramount on any operating system. With the amount of malware consistently growing daily which puts any system without any endpoint protection from any vendor at an increased risk level. There are many vendors in this space and now with the creation of the Endpoint detection and response systems there are even more choices with more traditional systems and more EDR based systems.

### Minimum

Deploy a solution on all VDAs along with any Citrix Infrastructure Servers and to all other systems if possible. It is also recommended to ensure it is the latest client paired for your operating system build. Ensure that the exclusions and best practices are applied from this article below. [https://docs.citrix.com/en-us/tech-zone/build/tech-papers/antivirus-best-practices.html](https://docs.citrix.com/en-us/tech-zone/build/tech-papers/antivirus-best-practices.html)

High Security

We recommend having a EDR based solution deployed if possible, to gain some of the features that most EDRs have. Most EDR solutions will look for known malicious files and processes but may also look for unusual data movement on the network and locally over even USB.

Example:

-  Published Application-EMR Image Microsoft Defender Antivirus
-  Published Desktop-EMR Image Microsoft Defender Antivirus
-  Remote PC Access Microsoft Defender Antivirus

## Logging

Without good proper logging you will not be able to provide a proper incident response. This problem is magnified in a non-persistent deployment as the system as the logs in many cases will be lost during a reboot if not retained on persistent storage and/or forwarded to a SIEM. There must be a tier approach when it comes to logging, We must not also forget about the other systems the items listed below. This is a typical logging priority list recommend ensuring you have log retention and forwarding set up for along with alerts for key events. Many of these systems will rely on Windows Event logs and others will require Syslog so you will need a solution for each of these event types. Logging can also be very helpful for troubleshooting and event correlation when there is a problem or outage.

1.  Networking
    1.  Firewall
    1.  Switches
    1.  Wireless
    1.  Content Control\Proxy
    1.  Load Balancers\Gateways
    1.  VPN Servers
1.  Domain Controllers
1.  File Servers
1.  DB Servers
1.  Web Servers
1.  Backup Servers
1.  Hypervisor Systems
1.  VDI Systems (Brokers)
1.  Other Application Servers
1.  Key Employee Computers
    1.  C-Suite
    1.  Finance
    1.  HR
    1.  IT
    1.  Privileged Account Workstations
1.  Managers and Team Leads
1.  All Other Systems

### BASIC AD SECURITY RELATED EVENT LOG EVENTS TO MONITOR & ALERT

Below in this table are some known Windows Event IDs that are typically associated with alerts as they could indicate compromise of systems via the normal attack paths most choose.

Reference

[https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor)

| Event ID                         | Description                                            | Impact                                                                                                                                                                                                                                                                                       |
| -------------------------------- | ------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1102/517                         | Event log cleared                                      | Attackers may clear Windows event logs to cover their tracks. This also relates to windows event log forwarding too.                                                                                                                                                                         |
| 4610, 4611, 4614, 4622, 4697     | Local Security Authority modification                  | Attackers may modify LSA for escalation/persistence in many common attack methods.                                                                                                                                                                                                           |
| 4648                             | Explicit credential logon                              | Typically, when a logged-on user provides different credentials to access a resource. Requires filtering of "normal".                                                                                                                                                                        |
| 4661                             | A handle to an object was requested                    | SAM/DSA Access. Requires filtering of "normal" to not overload the number of events logged.                                                                                                                                                                                                  |
| 4672                             | Special privileges assigned to new logon               | Monitor when someone with admin rights logs on. Is this an account that should have admin rights?  Knowing when someone or something is getting new privileges should be a cause of concern.                                                                                                 |
| 4723                             | Account password change attempted                      | Who or what attempted to change the password of this user?                                                                                                                                                                                                                                   |
| 4964                             | Custom Special Group logon tracking                    | Track admin &amp; "users of interest" logons. This relates to privileged account management for service accounts and normal privileged accounts to know when they are getting used.                                                                                                          |
| 7045, 4697                       | New service was installed                              | Attackers often install a new service for persistence and if someone is installing something as a service you should know.                                                                                                                                                                   |
| 4698, 4699, 4702                 | Scheduled task creation/modification                   | Attackers often create/modify scheduled tasks for persistence. Pull all events in Microsoft-Windows-TaskScheduler/Operational                                                                                                                                                                |
| 4719, 612                        | A system audit policy was changed                      | Attackers may modify the system's audit policy.                                                                                                                                                                                                                                              |
| 4732                             | A member was added to a (security-enabled) local group | Attackers may create a new local account &amp; add it to the local Administrators group. Restricted groups come into play here to ensure that their elevation will not last beyond a GPO refresh.                                                                                            |
| 4720                             | A (local) user account was created                     | Attackers may create a new local account for persistence.                                                                                                                                                                                                                                    |
| 3065, 3066                       | LSASS Auditing – Code Integrity Checking               | This monitors LSA drivers &amp; plugins. Test extensively before deploying!                                                                                                                                                                                                                  |
| 3033, 3063                       | LSA Protection – Failed to Load Drivers and Plugins    | This monitors the LSA drivers &amp; plugins and blocks any that are not properly signed.                                                                                                                                                                                                     |
| 4798                             | A user's group membership was enumerated               | This can potentially detect recon activity of local group membership enumeration which may lead to privilege escalation typical with Bloodhound. You may need to filter out normal activity.                                                                                                 |
| 4768, 4771                       | Account Logon / Kerberos Authentication Service        | Depending on your applications deployed this may be chatty or completely quiet until an attack or specific event triggers it. This is recommended because many attack methods use Kerberos as the authentication vehicle.                                                                    |
| 4769                             | Account Logon / Kerberos Service Ticket Operations     | Depending on your applications deployed this may be chatty or completely quiet until an attack or specific event triggers it. This is recommended because many attack methods use Kerberos as the authentication vehicle.                                                                    |
| 4741, 4742                       | Account Management / Computer Account Management       | Knowing when something is created or modified on a Domain Computer account it needs to be logged to be able to find out if it is a problem.                                                                                                                                                  |
| 4728, 4732, 4756                 | Account Management / Security Group Management         | Knowing when something is created or modified on a Domain Group it needs to be logged to be able to find out if it is a problem.                                                                                                                                                             |
| 4720, 22, 23, 38, 65, 66, 80, 94 | Account Management / User Account Management           | Knowing when something is created or modified on a Domain user it needs to be logged to be able to find out if it is a problem.                                                                                                                                                              |
| 4692                             | Detailed Tracking / DPAPI Activity                     | This is to track the export of the DPAPI backup key used for AD backup and restores. This is needed to take the AD database offline to crack the passwords along with other password related AD attacks also.                                                                                |
| 4688                             | Detailed Tracking / Process Creation                   | Tracking what is running can be very noisy, but once filtered it can be the best way to track what is going on specific computers. This may be harder to do on workstations but on servers it should be much less noise and when something is logged you most likely need to investigate it. |
| 4634                             | Collect events for account logoff                      | This can be noisy but is worth knowing when accounts logoff especially on servers and even on workstations based on your threat landscape.                                                                                                                                                   |
| 4624, 4625, 4648                 | Collect events for account logon                       | This can be noisy but is worth knowing when accounts logon especially on servers and even on workstations based on your threat landscape.                                                                                                                                                    |
| 4964                             | Collect events for special group attributed at logon   | Knowing when special accounts are logging in is a key indicator of these accounts being misused.                                                                                                                                                                                             |
| 4713, 4716, 4739                 | Collect events related to trust modifications          | No trust should be setup or modifying an existing trust from a one-way to a two-way one without you knowing                                                                                                                                                                                  |

If you also are using a Citrix ADC for load balancing, web application firewall, Citrix Gateway or other services we recommend visiting this reference Tech Zone Citrix ADC Security Guide along with the Syslog configuration guide

Citrix Tech Zone Link

TBD-One Published

Citrix ADC SYSLOG Guide

[https://docs.citrix.com/en-us/citrix-adc/13/system/audit-logging/configuring-audit-logging.html](https://docs.citrix.com/en-us/citrix-adc/13/system/audit-logging/configuring-audit-logging.html)

## Privilege Delegation

We recommend defining your IT roles and permissions for your VDI deployment along with your overall privileged accounts. The goal with any privilege delegation is to ensure administrators have the appropriate permissions needed to fulfill their assigned job roles. Depending on the products deployed you may have more or less groups based on your needs. These are just examples of some of the common delegation points. The recommended items below are done outside of the VDA OS hardening tasks itself. Without some of these core principals deployed your company will be at and increased risk.

### Minimum

#### Separate Administrative Accounts

We recommend ensuring that any user with more than Domain User rights to have a separate account. Too many attacks have started and or escalated due to administrators using their account to check email or browse the web. We recommend putting these in a protected OU with delegated permissions to prevent editing these users accounts other than a few individuals in your domain in a custom delegation group. It is also recommended to have a naming standard with a common suffix of prefix to help with auditing. The most common is using prefixes like "adm-", "admin-", "sa-" and "p".

##### Microsoft Privileged Access Accounts Overview

[https://docs.microsoft.com/en-us/security/compass/privileged-access-accounts](https://docs.microsoft.com/en-us/security/compass/privileged-access-accounts)

#### Service Account Naming Standard

Naming Standards should be used for all service accounts to separate them from normal accounts when doing account reviews. These accounts should also be in a separate OU to limit access to edit these accounts. The most common is using prefixes like "svc-", "service-", "s-" and "s". It is also recommended that within the account description or on a shared document a list of what each service does for easy reference for password resets.

#### Use Custom Groups for each Administrative Delegation

We recommend using custom AD groups to delegate all administrative roles within your deployment. Default Groups like Domain Admins should not be used to give access to remote into systems, manage privileged systems, manage accounts in AD, manage Citrix and other systems. Using groups with a naming standard will also help organize these groups for easy auditing and application on those systems, a common prefix is most common with other naming standards for each system group also. When common names are used to describe these roles, it will allow you to search for all groups for the Service Desk, VDI, AD and other systems. It can also be helpful for specific permissions like "Read Only" and "Full Control" to search and apply these roles on multiple systems. There may need to be multiple groups within each system that must be shared for each major role delegation. The complexity of delegation will increase the number of systems and permissions needed but this will also increase the security and flexibility of your deployment.

##### Example Citrix Custom Groups and Delegations

###### ADM-VDI-Full-Admins

These users will typically be local admins on all the Citrix Infrastructures Server Roles and VDAs, have full control of the profile share and be entitled to the Administrator roles within each Citrix Component. They will also have at least VM Administrator rights on the hypervisor hosting there in scope VMs.

###### ADM-VDI-Image-Admins

May only be local admins on the master image and may just be the patching that is responsible for updates of the image. If this is for an application team then we recommend a custom group per team that needs to maintain the applications per image. They may also have at least VM User rights on the hypervisor hosting these image VMs

###### ADM-VDI-ServiceDesk

Will have the roles of Help Desk or Service Desk in the Citrix Studio and Director. This will allow them to manage sessions and troubleshoot within Director and view configuration settings within Citrix Studio. For most deployments the Service Desk may not need access to any other Citrix Server role components like StoreFront Server, License Server, SQL, FAS Servers and others. Read only rights to things like WEM and Provisioning Server could be helpful for troubleshooting or spotting issues. The amount of privileged you will give you service desk will be based only our policies and their expertise. There may also be multiple levels of sub permissions also for each of these functions.

###### ADM-VDI-ReadOnly-Director

Allows this user the rights to just view items within Citrix Director. This can be helpful for an Application Owner to see the usage of their system along with the leadership team to track usage of the system.

###### ADM-VDI-ReadOnly-Studio

This may be needed just for Configuration Validations from other teams or Application Owners.

### Recommended

#### Migrate away from the AD Default Privileged Groups

Using the default groups for administrative elevation gives more permission than most need. It is very common that people and service accounts are just added to Domain Admins because it works and is easy. Many of these default groups have inherited and explicit rights on all machines and throughout the domain structure also. We recommend auditing the default privileged groups to identify all users, contractors and service accounts that may reside in them. The administrator account should not be removed from any default groups as this can cause issues and this should also be used as a highly sensitive privileged account and its password should be changed often along with it stored in secure manner.

#### Continual Account Review Audits

We recommend setting a schedule to regular account reviews with your Active Employee list along with your AD account listings to ensure only Active employees have accounts. We also recommend ensuring checking your third-parties accounts also based on active agreements with these vendors. Service accounts should also be audited regularly to ensure they are still needed, and they have just the required permissions for those roles.

#### Define Account Policy Requirements

We recommend setting an account policy standard in accordance with your compliance bodies. These settings are critical to the account security of the domain and should be as secure as possible along with a balance of usability. There are many standards for Password Age, Password Complexity, Length Requirements, Password History, Lockout Thresholds and Duration, Kerberos Ticket Settings, Logon Restrictions and many more items. These policies are normally defined at the root of the domain for all users and accounts and there may be other policies for privileged accounts and key employees to ensure they have a more secure standard.

##### Account Policy Policy Links

[https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/account-policies](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/account-policies)

##### Domain Password Requirements

[https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)

##### Office 365 Password Policies (MFA Required)

[https://docs.microsoft.com/en-us/microsoft-365/admin/misc/password-policy-recommendations?view=o365-worldwide](https://docs.microsoft.com/en-us/microsoft-365/admin/misc/password-policy-recommendations?view=o365-worldwide)

#### Define User Rights Assignments

We recommend auditing your Default Domain Policies User Rights assignment. There are many settings that at default settings that are built for backwards compatibility and ease of use without any custom settings deployed. Just like the Windows policies it is important to know the oldest systems to ensure that these systems will still function. These options should also be tied to your custom privileged groups to ensure only specified users can join computers to the domain, access the system remotely, log into specified systems and more. It can be helpful to make AD groups per user right assignment so that they can be nested into privilege group to allow specific permissions to specific users. User Rights Assignment decisions should be made for the root of your domain along with the location of your desktops and servers.

##### Microsoft User Rights Assignments Overview and Details

[https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/user-rights-assignment](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/user-rights-assignment)

#### Define Security Options

We recommend reviewing your Default Domain Policy to audit these Security Options. These options are central to the security of any domain. These settings control the behavior of the local machine with settings like renaming the guest and administrator accounts, permissions within the system from print driver install to SMB versions and many other settings. Each of these settings has a recommended setting beyond default but they will also be based on your oldest operating system you must support.

Security Options Overview

[https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/security-options](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/security-options)

### High Security

#### Use Privileged Workstations

All administrative work should be done from dedicated machines and not from each administrator's workstation. This will allow more thorough logging from these systems which will allow better visibility to the changes made in your deployment. There are third-party Privileged Account Management systems that will help build this system out and audit and control access to these machines to with even screen recording and log correlation depending on the vendor chosen. We recommend if you cannot afford a PAM solution to work in phases to deploy your own system. We recommend deploying a highly available set of servers/desktops that only defined administrators can remote into, these should be on a dedicated VLAN with ingress and egress control available. If available, we highly recommend requiring multifactor authentication to log into these systems. Once these systems are deployed you will want to install all the administrative tools that are needed for the targeted administrators. We want to then ensure these systems have logs retained locally and forwarded to your SIEM to have log retention and visibility so alerts can eventually be setup. Then the final steps will be implementing access control lists to limit source administrator from sensitive systems from this VLAN and also another highly controlled VLAN that machines could be put on in an emergency virtually or physically in the event of a failure of these systems. The primary and secondary privileged account workstation (PAW) VLANs should be audited and alerts should be sent if any devices are added to all responsible team members. With this final phase we can only administer these systems from these PAWs so that normal users will not be able to even get to the management UIs for these systems on their typical client networks or from a VDA.

##### Microsoft Overview of Privileged Account Workstations

[https://docs.microsoft.com/en-us/security/compass/concept-azure-managed-workstation](https://docs.microsoft.com/en-us/security/compass/concept-azure-managed-workstation)

#### Obfuscation

When your deployment is attack and there is an initial foothold there are benefits of using obfuscation to slow the attacker down so that you are logging and alerting systems can hopefully catch them before the next pivot. Having a password manager or another system is highly recommended before starting any obfuscation to be able to track the association. This obfuscation can start with privileged accounts that are not the same name of the user in AD. Privileged account obfuscation can be using the same unique last name in most cases or other unique name combinations so they can still be audited. Service accounts can also be obfuscated by using different prefixes or using names of people also, just remember you want to be able to audit these accounts. Sensitive server names can also be obfuscated with test and development prefixes or names that do not correlate to the role. The most powerful obfuscation is user obfuscation where their first and last name are not used for the account name, which normally is a combination of characters and numbers. User obfuscation is the most disruptive, but it can also be rolled out to just new users and slowly phased into and this is usually one of the last steps of a security remediation plan.

##### Local Groups

[https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/appendix-h--securing-local-administrator-accounts-and-groups](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/appendix-h--securing-local-administrator-accounts-and-groups)

##### Privileged Account Groups

[https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/manage/component-updates/appendix-i--creating-management-accounts-for-protected-accounts-and-groups-in-active-directory](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/manage/component-updates/appendix-i--creating-management-accounts-for-protected-accounts-and-groups-in-active-directory)

## Windows Features

There are many security features that are built into Windows that we recommend evaluating for deployment. This list will only be able to highlight a fraction of the features, we will try to showcase the ones that provide the biggest security impact to Citrix Virtual Apps and Desktops deployments.

### Local Administrator Password Solution (LAPS)

In most deployments the administrator password is the same for all desktops and servers because it may have been defined in the "Default Domain Policy" only. In many cases there may not be a local admin password standard as the desktops or servers built by different people or images use a different password and there isn't a set standard. With LAPs deployed each machine under that policy will have a unique password that is then stored within active directory and protected by an ACL. Having dedicated privileged accounts with proper delegation between roles is key when deploying this solution to ensure only authorized users are able to view and use the password. This can be rolled out in phases based on your OU structure to ensure everything is working as expected. This is can reduce the risk of Pass-the-Hash (PtH) credential replay attacks.

#### How to Change a Local Administrator Password with Group Policy

[https://social.technet.microsoft.com/wiki/contents/articles/4683.how-to-change-a-local-administrator-password-with-group-policy.aspx](https://social.technet.microsoft.com/wiki/contents/articles/4683.how-to-change-a-local-administrator-password-with-group-policy.aspx)

#### LAPs Tool Download

[https://www.microsoft.com/en-us/download/details.aspx?id=46899](https://www.microsoft.com/en-us/download/details.aspx?id=46899)

### Windows Event Log Forwarding

We recommend if you do not have a SIEM to configure Windows Event Log Forwarding as this solution is free with a license of Windows. The main requirement will be disk space and some Windows Event Collector servers depending on the number of events per second. We recommend enabling this on all Domain Controllers first as this information will be instrumental in any incident response investigations and also for troubleshooting. Tuning can be required based on the number of WEC servers for availability and stability and the number of events and disk space and I/O. These systems will only collect logs that are specified by Group Policies for Audit Policies and Advanced Audit Policies

#### WEF Setup Guide

[https://docs.microsoft.com/en-us/windows/security/threat-protection/use-windows-event-forwarding-to-assist-in-intrusion-detection](https://docs.microsoft.com/en-us/windows/security/threat-protection/use-windows-event-forwarding-to-assist-in-intrusion-detection)

#### Basic Security Audit Policies

[https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/basic-security-audit-policies](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/basic-security-audit-policies)

#### Advanced Audit Policies

[https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/advanced-security-audit-policy-settings](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/advanced-security-audit-policy-settings)

### Managed Service Accounts

This is designed to provide applications like SQL and Exchange to have an automatic password management. It will also simplify Service Principal Names management for these accounts which can help mitigate Kerberoast attacks.

[Managed Service Account Step-by-Step Guide](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd548356(v=ws.10)?redirectedfrom=MSDN)

[Top 10](https://docs.microsoft.com/en-us/windows/security/threat-protection/overview-of-threat-mitigations-in-windows-10)