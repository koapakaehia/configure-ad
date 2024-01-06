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
<b>Deploy a Virtual Machine and utilize Windows Server 2022 to create a Domain Controller.</b>

- Create a Virtual Machine
  - Assign the name "DC-1" to the Virtual Machine during the creation process.
  - Utilize the Windows Server 2022 image for the operating system of the Virtual Machine.
  - Make a note that the Virtual Machine is associated with a previously created Resource Group.
  - Take note that the Virtual Machine is deployed in the specified region.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
- Generate a username and password for authentication.
- Select a size with 1 virtual CPU.
  - (1vCPU), which offers a larger virtual CPU for improved Virtual Machine performance.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Proceed to the next step by clicking on "Next: Disks."</b>
- Advance to the next section by selecting "Next: Networking."
- Make a note that the Virtual Network (Vnet) has been created for the specified configuration
  - Click on "Review+Create" to assess and finalize the configuration before initiating the creation process.
  - Click on "Create" once the validation has passed, initiating the actual creation process.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Configure the Domain Controller's NIC (Network Interface Card) to use a static private IP address.
  - Select or click on "DC-1" to access or view details and settings associated with the specified entity
  - Choose the "Networking" option to access and configure network-related settings for the selected entity, "DC-1."
  - After selecting "Networking," navigate to the specific "Network Interface: dc-1678" to configure and manage the network settings for the associated entity "DC-1."
  - Proceed to the "IP Configurations" section under "Network Interface: dc-1678" to manage and set the IP configurations for the specified Domain Controller "DC-1."
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- To create "Client-1," you would typically follow a similar process as creating "DC-1."
  - Choose the "Windows 10" image for the operating system when configuring the image settings for the creation of "Client-1."
  - Use the same Resource Group and Region for "Client-1" as previously specified for "DC-1."
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Proceed to create a username and password for authentication during the setup of "Client-1."
  - Choose the "Windows 10" image for the operating system when configuring the image settings for the creation of "Client-1."
  - Choose the size with 2 virtual CPUs (2vCPUs) when configuring the size settings for the creation of "Client-1."
  - Tick the licensing box to indicate compliance or acknowledgment of licensing requirements during the setup of "Client-1."
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Verify that "Client-1" is connected to the identical Virtual Network (Vnet) as the Domain Controller ("DC-1")
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>Verify that the client VM can establish a connection with the DC VM without encountering any problems.</b>

- Log in to the "Client-1" virtual machine using Remote Desktop.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>Verify that the client VM can establish a connection with the DC VM without encountering any problems.</b>

- Execute the command "ping -t" to continuously ping the private IP address of "DC-1" and assess the network connectivity.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Access the Domain Controller by logging in with the appropriate credentials.
  - Using the previously created username and password. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Enable ICMPv4 on local windows firewall
  - Perform a search for "firewall" to access and configure firewall settings on your system.
  - Launch the "Windows Defender Firewall with Advanced Security" to access and configure advanced firewall settings on your system.
  - Choose "Inbound Rules" to manage and configure rules for incoming network traffic in the Windows Defender Firewall.
  - Afterward, choose "Sort by Protocol" to organize and view inbound rules based on the protocol in the Windows Defender Firewall.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>Install Active Directory (AD) on Windows Server</b>

- Log in to "DC-1" using the appropriate credentials.
  - Log in to the "DC-1" server using the appropriate credentials.
  - Open "Server Manager" and select "Add Roles and Features."
  - In the "Add Roles and Features Wizard," choose "Active Directory Domain Services."
  - Then proceed to install by clicking "Install."
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Promote the server to operate as a Domain Controller.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Set up a new forest, and for the domain name, choose "mydomain.com" (this can be customized, but remember the selected name).
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Restart the system, and after the reboot, log back into "DC-1" user: (domain)\labuser"
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>Generate both an administrative and a regular user account.</b>

- Within Active Directory Users and Computers.
  - Create an Organizational Unit (OU) with the name '_EMPLOYEES'."
  - Create a new Organizational Unit (OU) and designate it as '_ADMINS'."
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Establish a new administrator user
  - Identify the individual as Jane Doe.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Create username & password:
  - Identify the individual as Jane Doe.
  - Assign the username 'jane_admin' and the password 'Password1'.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Right click user, properties, Member of
  - Add jane_admin to the “Domain Admins” Security Group
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>Join Client-1 into your domain.</b>
  
- Configure the DNS settings for Client-1 to the private IP address of the DC from the Azure Portal.
  - Navigate to Client-1
  - Access the Networking tab.
  - Proceed to the Network Interface named 'client-1577
  - and configure the DNS servers.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
- Initiate a restart of the Client-1 virtual machine through the Azure Portal.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
- Access Client-1 via Remote Desktop using the credentials of the original local administrator, 'labuser'.
- Integrate Client-1 into the domain, keeping in mind that the computer will restart.
  - Following the restart, right-click on the Start menu
  - Navigate to 'System,'
  - And proceed to 'Rename This PC'.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
- During the domain integration process, a permission request will prompt.
  - Log in using the credentials '(domain)\jane_admin,'
  - And expect the computer to restart afterward.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
- Access the Domain Controller on Client-1.
  - By logging in using the domain associated with the Domain Controller and the credentials of Client-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
- Confirm the presence of Client-1 in the Active Directory Users and Computers interface.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>Configure Remote Desktop access for non-administrative users on the client computer.</b>

- Sign in to Client-1 using the credentials '(domain)\jane_admin'.
  - Access the system properties on the client computer.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Navigate to the 'Remote Desktop' settings.
  - and allow remote desktop access to 'domain users'.
- Now users with non-administrative privileges can log in to Client-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>Generate additional user accounts and verify their login capabilities.</b>

- Sign in to DC-1 using the credentials of 'jane_admin.'
- Then launch PowerShell ISE as an 'Administrator.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Generate a new file and paste the script from the provided link
  - (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1).
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Run the script and monitor the creation of accounts as it progresses.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Access Active Directory Users and Computers (ADUC) and observe the newly created accounts in the designated Organizational Unit (OU).
  - Try logging into Client-1 using one of the accounts created by referencing the password provided in the script.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Confirm successful logins on the client computer.
- Congratulations, you've successfully completed the step-by-step guide outlining the implementation of on-premises Active Directory within Azure Virtual Machines.
</p>
<br />
