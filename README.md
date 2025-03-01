<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Preparing Active Directory Infrastructure in Azure</h1>

This project involves setting up a cloud-based Windows domain environment in Microsoft Azure, simulating enterprise-level IT administration.

  1. Infrastructure Setup
      - Create an Azure Resource Group, Virtual Network, and Subnet to host the environment.
      - Deploy a Windows Server 2022 VM (DC-1) as the Domain Controller and configure its static private IP.
      - Deploy a Windows 10 VM (Client-1) and connect it to the same network as DC-1.

  2. Networking & Configuration
      - Assign DC-1’s private IP as Client-1’s DNS server to enable domain communication.
      - Test connectivity between Client-1 and DC-1 via ping and PowerShell commands.

This setup provides hands-on experience with Active Directory, DNS, networking, and cloud administration in an Azure environment..<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Domain Name System
- PowerShell


<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>


We will start by creating a Resource Group to organize and manage all related Azure resources within a single logical container.
![Screenshot 2025-02-27 144532](https://github.com/user-attachments/assets/d8b16f74-7eb1-4184-8924-da7ffe2da760)

Next, we will create a Virtual Network (VNet), keeping all settings at their default values and assigning it to the previously created Resource Group for proper resource organization and connectivity.
![Screenshot 2025-02-28 085018](https://github.com/user-attachments/assets/1075a833-1f64-4c9d-bbcf-f908970d1bcb)

We will deploy two Virtual Machines (VMs) in Azure:

  - A Windows Server 2022 Datacenter Azure Edition VM, named "DC", which will function as the Domain Controller.
  - A Windows 10 Pro VM, named "Client", which will serve as the domain-joined workstation.

Both VMs should be assigned to the Virtual Network created in the previous step to ensure proper connectivity between them. You can choose any login credentials, but for this lab, we will use labuser/Cyberlab123! as an example.
![Screenshot 2025-02-28 085633](https://github.com/user-attachments/assets/fff0806d-eb24-43f9-bdab-67be254a758c)

We need to set the Domain Controller's private IP address to static to ensure network stability and proper communication between domain-joined devices. By default, Azure assigns dynamic private IP addresses to virtual machines, meaning the address could change upon a restart. Since Active Directory and DNS rely on consistent IP addressing, setting a static private IP ensures that Client-1 can always resolve and communicate with the Domain Controller without connectivity issues.

To do this, we will configure DC's network interface card (NIC) settings in Azure and assign it a static private IP within the Virtual Network.
![Screenshot 2025-03-01 120801](https://github.com/user-attachments/assets/66cceafa-aca7-41f6-9fcc-3cd87b2f33a3)
![Screenshot 2025-03-01 120838](https://github.com/user-attachments/assets/01faad0b-c3bc-4150-9096-5533e36d80ad)

Next, we will connect to the Domain Controller (DC) using Remote Desktop (RDP). To do this, we will use DC’s public IP address along with the login credentials created earlier.
![Step3](https://github.com/user-attachments/assets/6a44cc6a-c1b0-479f-aee7-987665ce4707)

Once connected to DC via Remote Desktop, we will disable the Windows Firewall to allow network traffic between the Domain Controller and the Client workstation.

**Important Note:**
Disabling the firewall is not a best practice in real-world environments, as it removes critical security protections and exposes the system to potential threats. However, for the purpose of this lab environment, we are temporarily disabling it to ensure smooth communication between the two virtual machines, allowing us to test connectivity.
![Step5](https://github.com/user-attachments/assets/5b142d40-92cd-4092-a65a-c5416261234a)

We will disable the Domain Profile, Private Profile, and Public Profile within the Windows Firewall settings.
![Step6](https://github.com/user-attachments/assets/48aace3e-17a1-43c3-b02b-c63bdd2e3aea)

Once the firewall configuration is complete, we will return to Azure and navigate to the Client workstation’s Network Interface (NIC) settings. Here, we will manually set the Client’s DNS server to the Domain Controller’s private IP address.
Why is this important?

By default, Azure assigns its own DNS settings to virtual machines. However, since our Domain Controller is also acting as the DNS server, we need to point the Client’s DNS settings to DC’s private IP. This ensures that domain name resolution and Active Directory authentication work correctly when we join the Client to the domain.
![Step7](https://github.com/user-attachments/assets/118c6d05-568e-459d-afb5-4f5d5cc6d133)
Next, we will Remote Desktop into the Client workstation using its public IP address and the login credentials we created earlier, just as we did with the Domain Controller.

Once connected, we will:

  - Open PowerShell
  - Run the ping command to test connectivity with the Domain Controller’s private IP address

Why is this important?

This step verifies that network traffic between the Client and the Domain Controller is flowing correctly. If the ping succeeds, it confirms that the machines are properly connected within the Virtual Network and that the firewall settings are correctly set.
![Step8](https://github.com/user-attachments/assets/4139a882-ef5c-4801-b969-c0201e2f9274)
![Step9](https://github.com/user-attachments/assets/c59858de-1e61-4abc-9f88-7fa5e1e2f3c6)

After confirming connectivity with the ping command, we will also run:

  - ipconfig /all in PowerShell to display detailed network configuration.
  - Check the DNS server settings, ensuring that the Domain Controller’s private IP address is listed as the Client's DNS server.

Why is this important?

This confirms that Client-1 is correctly using DC-1 for DNS resolution, which is essential for domain authentication and resource access. If the DNS server is not set to DC-1’s private IP, the Client may face issues when joining the domain or resolving network names.
![Screenshot 2025-02-27 144403](https://github.com/user-attachments/assets/3fd1dffc-61e4-4aa9-aaa9-0405b2bdaed9)
