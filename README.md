<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines. You will create a Domain Controller to be the networks DNS server and a client in which you can practice adding users and rules as you would in a real world environment.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Domain Controller in Azure
- Setup Client-1 in Azure
- Install Active Directory
- Create Domain Admin User Within the Domain
- Join Client-1 to Domain
- Setup Remote Desktop For Non-Administrative Users On Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<h3 align="center"> Setup Domain Controller in Azure </h3>

<p align="center">
Create Resource Group on Microsoft Azure:
</p>
<br/>
<p>
<img src="https://i.imgur.com/mMOCPkK.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center">
Create Virtual Network on Microsoft Azure:
</p>
<br/>
<p>
<img src="https://i.imgur.com/qtb5sdU.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center">
Create Virtual Machine on Microsoft Azure and name it "dc-1". Make sure to put it in the resource group and virtual network we just created. Image: Windows Server 2022. Size: anything with 2 virtual cpus. username: make it and save it in a txt file (notepad/textedit). password: make it and save it in a txt file:
</p>
<br/>
<p>
<img src="https://i.imgur.com/phMMme6.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>

<h3 align="center">Setup Client-1 in Azure </h3>
<p align="center">
Create second virtual machine. Subscription: Pay-as-you-go. Same resource group as the first virtual machine. Name: "client-1". Image: Windows 10 Pro, version 22H2. Size: anything at least 2 vcpus. Username and passwords: username same as first vm but add password and save to txt file. Go to networking tab > Virtual Network: the virtual network we created and put dc-1 vm in:
</p>
<br/>
<p>
<img src="https://i.imgur.com/KTmZMrC.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center">
Set Domain Controller's NIC Private IP address to be static:
</p>
<br/>
<p>
<img src="https://i.imgur.com/EWw7Ijg.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center">
Log into dc-1 VM and disable Windows Firewall. This tests connectivity. Disable under domain and private profile tabs. Under public profile > inbound connections: allow inbound connections.
</p>
<br/>
<p>
<img src="https://i.imgur.com/1re9WP7.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center">
Set Client-1 DNS settings to DC-1's private IP address
</p>
<br/>
<p>
<img src="https://i.imgur.com/3EY8Ir1.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center">
Restart Client-1 from Azure portal
</p>
<br/>
<p>
<img src="https://i.imgur.com/4tSKQkj.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center">
Ping DC-1's private IP address after logging in to Client-1
</p>
<br/>
<p>
<img src="https://i.imgur.com/WeLgmm8.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center">
Open PowerShell and run "ipconfig /all". Look for DNS Settings and it should have DC-1's Private IP address
</p>
<br/>
<p>
<img src="https://i.imgur.com/W7TdQ5c.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<h3 align="center">Install Active Directory</h3>

<p align="center">
Login to DC-1 and Install Active Directory Domain Services
</p>
<br/>
<p>
<img src="https://i.imgur.com/oKDiLAy.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center">
Promote as a domain controller and set up new forest as whatever you want (ex: mydomain.com). Restart and log back into DC-1 as user: mydomain.com\labuser
</p>
<br/>
<p>
<img src="https://i.imgur.com/ziFtjOA.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center">
Restart and log back into DC-1 as user: mydomain.com/labuser
</p>
<br/>
<p>
<img src="https://i.imgur.com/L86caXO.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<h3 align="center">Create Domain Admin User Within the Domain</h3>

<p align="center">
In Active Directory User and Computers (ADUC), create Organizational Unit (OU) called “_EMPLOYEES” and another OU called "_ADMINS"
</p>
<br/>
<p>
<img src="https://i.imgur.com/ZwVF5wA.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center">
Create new employee named "Jane Doe" with same password. Username "jane_admin" / whatever password you want. Add "jane_admin" to the "Domain Admins" Security Group.
</p>
<br/>
<p>
<img src="https://i.imgur.com/gWPeZ84.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center">
Add jane_admin to the “Domain Admins” Security Group. 
</p>
<br/>
<p>
<img src="https://i.imgur.com/DAq1mgj.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center">
Log out and close connection to DC-1. Log back in as "mydomain.com/jane_admin". Use jane_admin as your admin account from now on
</p>
<br/>
<p>
<img src="https://i.imgur.com/bHnETUb.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<h3 align="center">Join Client-1 to Your Domain</h3>

<p align="center">
Login to Client-1 as the original local admin (labuser) and join it to the domain (computer will restart)
</p>
<br/>
<p>
<img src="https://i.imgur.com/Pq4rnSO.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center">
Login to the Domain Controller and verify Client-1 shows up in ADUC. Then create a new OU named “_CLIENTS” and drag Client-1 into there
</p>
<br/>
<p>
<img src="https://i.imgur.com/iPU9uEL.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/WwFhfUb.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<h3 align="center">Setup Remote Desktop For Non-Administrative Users On Client-1</h3>

<p align="center">
Log into Client-1 as mydomain.com/jane_admin > open system properties > Remote Desktop > Allow "domain users" access to remote desktop.
</p>
<p align="center">
You can now log into Client-1 as a normal, non-administrative user now
Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)
</p>
<br/>
<p>
<img src="https://i.imgur.com/pvEz2Mi.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<h3 align="center">Create a bunch of additional users and attempt to log into client-1 with one of the users
</h3>

<p align="center">
Login to DC-1 as jane_admin and open PowerShell_ise as an administrator
</p>
<br/>
<p>
<img src="https://i.imgur.com/1MsJ0l1.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center"> Create a new File and paste the contents of the <a href="https://drive.google.com/uc export=download&id=1b3RBkXTLNGXbibeMuAynkfzdBC1NnqaD" target="_blank"> script </a> into it then run the script and observe the accounts being created.
</p>
<br/>
<p>
<img src="https://i.imgur.com/gSHTyVx.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/EDKrtiV.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center">
Open ADUC and observe the accounts in the appropriate OU (_EMPLOYEES)
</p>
<br/>
<p>
<img src="https://i.imgur.com/X5dqW6T.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>

<p align="center">
Attempt to log into Client-1 with one of the accounts (take note of the password in the script)
</p>
<br/>
<p>
<img src="https://i.imgur.com/zUqUZ1I.png" height="75%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br/>
<br/>


