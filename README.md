<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/jIPFIBU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b> Setup Resources in Azure </b> <br />
- Create the Domain Contrller VM (Windows Server 2022). <br />
- Set Domain Controller's NIC Private IP address to be static. <br />
- Create the Client VM (Windows 10). *Use the same Resource Group and Vnet created in the first step. <br />
- Make sure both VMs are in the same Vnet (take a look at topology in Network Watcher). <br />
<br />
To ensure connectivity, login to Client VM with Remote Desktop and ping Domain COntroller's private IP address with <i>ping -t </i>  <br />
Login to the Domain Controller and enable ICMPv4 on the local windows firewall. <br />
See that the ping succeeds in Client VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/oqS2wr6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>Install Active Directory</b> <br />
- Login to Domain Controller and install Active Directory Domain Services.<br />
- Promote as a Domain Controller and setup a new forest as "mydomain.com". <br />
- Restart and log back into Domain Controller VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/U0bCgff.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
<b>Create an Admin and Normal User Account in Active Directory</b> <br />
- In Active Directory Users and Computers, create an Organizational Unit called “_EMPLOYEES”. <br />
- Create a new Organizational Unit named “_ADMINS”. <br />
- Create a new employee named "John Doe" with the username of "john_admin". <br />
- Add john_admin to the "Domain Admins" Security Group. <br />
- Log out and log back in as "mydomain.com\john_doe".
</p>
<br />

<p>
<b> Join the Client to your domain (mydomain.com). </b> <br />
-  Azure Portal, set Client's DNS settings to Domain Controller's Private IP address, then restart Client from the Azure Portal. <br />
- Login into Client VM as the original local admin and join it to the domain. <br />
- Login to Domain Controller and verify that the client shows up in Active Directory users and Computers inside the "Computers" container on the root of the domain. <br />
-Create a new organizational unit "_Clients", then drag the Client into there.
</p>
<br />

<p>
<b>Setup Remote Desktop for non-administrative users on the Client</b> <br />
-	Log into Client as mydomain.com\john_admin and open system properties. <br />
-	Click “Remote Desktop”. <br />
-	Allow “domain users” access to remote desktop. <br />
- You can now log into Client as a normal, non-administrative user now.
</p>
<br />
