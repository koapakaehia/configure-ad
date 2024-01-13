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
- Mac OS (User)

<h2>High-Level Deployment and Configuration Steps</h2>

- Establish a Virtual Machine as a Domain Controller by deploying Windows Server 2022 on the designated server.
- Make sure there is a smooth connection between the client and the Domain Controller.
- Install Active Directory (AD) on the designated server.
- Generate both an administrative and a regular user account within the Active Directory.
- Connect the client machine to your domain.
- Configure Remote Desktop settings on the client computer, specifically tailored for non-administrative users.
- Generate additional user accounts within the Active Directory and ensure that all created users can successfully log in as intended.

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>SETUP RESOURCES IN AZURE.</b>

1. Create Domain Controller VM (DC-1):
  - Open the Azure Portal (portal.azure.com).
  - Click on "Virtual Machines" and then click "Add."
  - Choose the "Windows Server 2022" image.
  - Fill in the necessary details, such as VM name ("DC-1"), username, and password.
  - Select the appropriate Resource Group or create a new one.
  - Configure networking settings, ensuring that you note the Resource Group and Virtual Network created.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
2. Set DC-1 NIC Private IP Address:

  - Navigate to the "Networking" tab of DC-1 in the Azure Portal.
  - Under "Settings," select the network interface.
  - Set the NIC's private IP address to be static.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
3. Create Client VM (Client-1):

  - Click on "Virtual Machines" and then click "Add" again.
  - Choose the "Windows 10" image.
  - Configure VM details, setting the VM name as "Client-1," and ensuring 2 vCPUs.
  - Use the same Resource Group and Virtual Network as DC-1.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
4. Ensure Connectivity:

  - Verify both VMs are in the same Virtual Network using Azure Network Watcher.
  - Remote Desktop into Client-1 and open a command prompt.
  - Ping DC-1's private IP address (ping -t <ip address>) to test connectivity.
  - On DC-1, open Windows Firewall settings and allow ICMPv4.
  - Confirm successful ping from Client-1.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>INSTALL ACTIVE DIRECTORY.</b>

1. Install Active Directory on DC-1:

  - Log in to DC-1 using the provided credentials.
  - Open PowerShell and run the following command
  - After installation, promote DC-1 to a domain controller using the Active Directory Domain Services Configuration Wizard.
  - Specify a new forest name (e.g., "mydomain.com").
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
2. Create Admin and Normal User in AD:

  - Open "Active Directory Users and Computers" on DC-1.
  - Create an Organizational Unit (OU) named "_EMPLOYEES."
  - Create another OU named "_ADMINS."
  - Inside "_EMPLOYEES," create a new user "Jane Doe" with the username "jane_admin."
  - Add "jane_admin" to the "Domain Admins" Security Group.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
3. Switch to Admin Account:

  - Log out or close the Remote Desktop connection to DC-1.
  - Log back in as "mydomain.com\jane_admin."
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>JOIN CLIENT-1 TO DOMAIN.</b>
1. Join Client-1 to Domain:

   - In the Azure Portal, navigate to "Virtual Machines" and select Client-1.
  - Configure VM details, ensuring 2 vCPUs.
  - Under "Settings," go to "Networking," and set DNS settings to DC's private IP.
  - Restart Client-1 from the Azure Portal.
  - Log in to Client-1 as the original local admin (labuser) and join it to the domain.
  - On DC-1, verify that Client-1 appears in ADUC (Active Directory Users and Computers) under the "Computers" container.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
2. Organize Clients in OU:

  - (Optional) In ADUC, create a new OU named "_CLIENTS."
  - Move Client-1 into the "_CLIENTS" OU for organizational purposes.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>SETUP REMOTE DESKTOP.</b>
1. Setup Remote Desktop:

  - Log into Client-1 as "mydomain.com\jane_admin."
  - Right-click on "This PC," select "Properties," and click "Remote settings."
  - In the "Remote" tab, click "Allow remote connections to this computer."
  - Click "Select Users," add "DOMAIN\domain users," and grant remote access.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
2. Verify Remote Desktop Access:

  1. Log into Client-1 as a normal, non-admin user.
  2. Open Remote Desktop Connection, enter Client-1's name, and log in as a regular user.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>CREATE ADDITIONAL USERS.</b>

1. Create Additional Users:

  - Log into DC-1 as jane_admin.
  - Open PowerShell ISE as an administrator.
  - Copy the script from GitHub.
- Run the script to create additional user accounts.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
2. Verify Accounts:

  - Open ADUC on DC-1 and observe the newly created accounts in the appropriate OU.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>FINISH.</b>
1. Finish:

  - Verify the functionality and access for the created users on Client-1.
  - Ensure successful logins for both administrative and non-administrative users.
  - Confirm that organizational units in ADUC reflect the desired structure.
</p>
<br />
