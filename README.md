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
</p>
<p>
In this lab, we will set up two virtual machines (VMs) within the same virtual network (VNET). One VM will function as a Domain Controller (DC), while the other will serve as a client machine. The DC's IP address will be configured as static since it provides Active Directory services to the client. The client machine will be joined to the domain, and its DNS settings will be manually configured to use the Domain Controller as its DNS server. 
</p>
<br />

<p>
<img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
DC-1 must be assigned a static private IP address. Client-1 will connect to DC-1, and to verify connectivity, we will attempt to ping DC-1 from Client-1. Initially, the ping will fail. To resolve this, we need to enable ICMPv4 through the firewall on DC-1. Once this is done, Client-1 will be able to ping DC-1 successfully.
</p>
<br />

<p>
<img src="https://i.imgur.com/HvZBWzc.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/1lrrGPw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
Next, we will log back into DC-1 to install Active Directory Users & Computers. We will promote the VM to a Domain Controller and set up a new forest named "mydomain.com." After completing this process, we will restart the VM and log back into DC-1 as the user "mydomain.com\labuser." If the steps were followed correctly, AD Users & Computers should run successfully, as demonstrated below.
</p>
<img src="https://i.imgur.com/cGjvRke.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
Now we can begin creating Organizational Units (OUs). First, create an OU named _EMPLOYEES, followed by another OU named _ADMINS. To do this, right-click on the domain area, select New → Organizational Unit, and complete the required field. Next, navigate inside the _ADMINS OU, right-click, select New → User, and enter the details for a new user named Jane Doe. Since Jane will be an administrator, her username should be Jane_admin. Finally, add Jane to the Domain Admins security group.
</p>
<img src="https://i.imgur.com/hL7g5Y5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kcgvzdE.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
From this point forward, Jane_admin will be used as the administrator account. Now, we will join Client-1 to the mydomain.com domain. First, update Client-1's DNS settings in the Azure portal, setting its DNS server to the private IP address of DC-1. Once the changes are applied, restart Client-1 from within the Azure portal. The image below confirms that Client-1 is now using DC-1 as its DNS server. 
</p>
<img src="https://i.imgur.com/jbrGTXW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<p>
We have to join Client-1 to the domain in order to do so navigate to your system settings and go to about. Off to the right select rename this pc (advanced). From there select to change the domain. Enter "mydomain.com" after that enter your credentials from mydomain.com\labuser. Your computer will restart and then client-1 will be a part of mydomain.com
</p>
<br />
<p>
  <p>
<img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To join Client-1 to the domain, navigate to System Settings and go to About. On the right side, select Rename this PC (Advanced). In the window that appears, choose the option to change the domain and enter "mydomain.com". Next, provide the credentials for mydomain.com\labuser. Once authenticated, the computer will restart, and Client-1 will be successfully joined to mydomain.com.
</p>
<br />

<p>
  <p>
<img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Finally, to verify that regular users can RDP into Client-1, we will use a script to generate thousands of users in the domain. The script will be executed in PowerShell, and once the users are created, we will select one and attempt to RDP into Client-1 to confirm successful login.
</p>
<br />
<img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<p>
  <p>
<img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
As shown, the PowerShell script successfully created a user with the username "bab.hubo". We were able to log in to Client-1 using this user's credentials, confirming that normal users can successfully access the system via RDP.
</p>
