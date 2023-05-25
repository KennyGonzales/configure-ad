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

<h2>High-Level Deployment and Configuration Steps</h2>

- Naming Domain Controller VM (Windows Server 2022), "DC-1"
- Setting Domain Controller's NIC Private IP address as "static"
- Labeling ICMPv4 (ping) as "allowed" on the Domain Controller
- Creating an Admin account & User account in Active Directory 
- Make "Client-1" login with one of the users 

<h2>Deployment and Configuration Steps</h2>



<p>
  
To begin, we need to create a resource group that will serve as a container for the virtual machines, namely the Domain Controller (DC-1) and the Client Virtual Machine (Client-1). The Domain Controller VM will be provisioned using a Windows Server 2022 system image, which is a serialized copy of the complete state of a computer system stored in a non-volatile form, such as a file.
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
The Client Virtual Machine (Client-1) has been deployed on Windows 10 and is now part of the same Resource Group and Virtual Network (Vnet) as the previously created DC-1 (Domain Controller).
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
The private IP address has been configured as static, which is essential for devices requiring uninterrupted accessibility.  
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
Write here  
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
Write here  
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
Write here  
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
Write here  
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />
