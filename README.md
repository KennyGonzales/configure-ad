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
  
To begin, we need to create a resource group that will serve as a container for the virtual machines, namely the Domain Controller (`DC-1`) and the Client Virtual Machine (`Client-1`). The Domain Controller VM will be provisioned using a **Windows Server 2022** system image, which is a serialized copy of the complete state of a computer system stored in a non-volatile form, such as a file.
  
<img src="https://i.imgur.com/yrZz8I3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
The Client Virtual Machine (`Client-1`) has been deployed on Windows 10 and is now part of the same Resource Group and Virtual Network (Vnet) as the previously created `DC-1` (Domain Controller).
  
<img src="https://i.imgur.com/Pp5DQdu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
The private IP address has been configured as **_static_**, which is essential for devices requiring uninterrupted accessibility.  
  
<img src="https://i.imgur.com/2UO3Wc1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
Verify the connection between the client device and domain controller by accessing `Client-1` using **Remote Desktop Connection (RDP)** and initiating a perpetual ping to `DC-1's` private IP address using the **ping -t** command. To allow ICMPv4 (ping) communication, the Windows Firewall on the Domain Controller (`DC-1`) was configured to permit **ICMP Echo Request (ICMPv4-In)** through **Core Networking Diagnostics**. Once you log back into `Client-1`, ensure that the ping operation is successful.
  
>**Note**: To ensure connectivity, activate the two ICMPv4-In rules **while initiating perpetual ping (ping -t)** to DC-1.
  
<img src="https://i.imgur.com/7P9UgKE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
</p>
<p>
</p>
<br />

<p>
  
The image below demonstrates that the **ICMP** rule has been permitted on the Windows Firewall to _allow_ **inbound traffic**. 
  
>**Note**: To access into the Windows Firewall, type **wf.msc** _or_ **firewall** into the Windows searchbar.  
  
<img src="https://i.imgur.com/K7n2bSF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
After accessing the "add roles and features" option within `DC-1`, we enabled Active Directory Domain Services and promoted `DC-1` as a Domain Controller for a newly created forest named mydomain.com. Remote Desktop was restarted and logged back into `DC-1` using the user credentials **"mydomain.com\labuser"**.  
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
</p>
<p>
</p>
<br />

<p>
  
While remaining connected to `DC-1` through Remote Desktop Connection, we proceed to set up **organizational units** for admins and employees in Active Directory (AD). The newly created accounts can now be found in their respective organizational units within Active Directory. To create the required folders for your AD, simply **right-click** on your domain name in Active Directory, navigate to the **"new"** option, select **"Organizational Unit"**, and proceed to create folders for employees, admins, and security groups.  
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
Let's create a new Organizational Unit (OU) called `_ADMINS`. We will proceed to create a new employee named "Karen Karen" with the username "**karen_admin"** and set the same password. Once the admin account is created, we will add **"karen_admin**" to the _domain admins_ security group.
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
To proceed, please log out and close the current session for the user **mydomain.com\labuser** on `DC-1`. Then, log in again using the credentials **mydomain.com\karen_admin**.
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
Now, let's move on to joining `Client-1` to the domain (**mydomain.com**). To do this, we need to change the _DNS_ configuration on `Client-1`. We will replace the current _DNS_ settings with the private IP address of `DC-1`. This modification is necessary to ensure a proper connection between `Client-1` and the domain. To make the change, locate the **Network Interface Card (NIC)** settings on `Client-1` and update the _DNS configuration_ with the _private IP address_ of `DC-1`. 
  
Then, select **DNS servers**.  
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
</p>
<p>
</p>
<br />

<p>
  
To proceed, select the "**Custom"** radio button in the DNS server configuration. This will allow you to enter the private IP address of `DC-1`. 
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
After successfully updating the DNS server with the private IP address of `DC-1`, we can proceed with adding `Client-1` to the domain without any errors. You will receive a notification confirming the successful addition of the client to the domain. To accomplish this, navigate to **System** → **Rename This PC** → **enter the domain name** → and click "OK". Finally, select **Apply** to save the changes. Keep in mind that a system restart will be necessary to apply the updates.  
  
A message will display that Client has been added to the domain.  
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
</p>
<p>
</p>
<br />

<p>
  
To create the users for our `_EMPLOYEES` organizational unit in the domain controller (`DC-1`), we will utilize _PowerShell_ISE_ with **administrative privileges**. Start by creating a new file and entering the provided pre-configured script. Running the script will generate the random employees as specified.  
  
<details>  
  <summary> <h6>Pre-Configured Powershell Script</h6> </summary>
  
  
```powershell
# ----- Edit these Variables for your own Use Case ----- #
$PASSWORD_FOR_USERS   = "Password1"
$NUMBER_OF_ACCOUNTS_TO_CREATE = 10000
# ------------------------------------------------------ #

Function generate-random-name() {
    $consonants = @('b','c','d','f','g','h','j','k','l','m','n','p','q','r','s','t','v','w','x','z')
    $vowels = @('a','e','i','o','u','y')
    $nameLength = Get-Random -Minimum 3 -Maximum 7
    $count = 0
    $name = ""

    while ($count -lt $nameLength) {
        if ($($count % 2) -eq 0) {
            $name += $consonants[$(Get-Random -Minimum 0 -Maximum $($consonants.Count - 1))]
        }
        else {
            $name += $vowels[$(Get-Random -Minimum 0 -Maximum $($vowels.Count - 1))]
        }
        $count++
    }

    return $name

}

$count = 1
while ($count -lt $NUMBER_OF_ACCOUNTS_TO_CREATE) {
    $fisrtName = generate-random-name
    $lastName = generate-random-name
    $username = $fisrtName + '.' + $lastName
    $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $firstName `
               -Surname $lastName `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_EMPLOYEES,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
    $count++
}
```
  
</details>
  
By executing this Powershell script, 1000 random users will be generated.
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
</p>
<p>
</p>
<br />

<p>
  
The users being created from the Powershell script will now be within the **Active Directory** on the Domain Controller.  
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<p>
  
You can login on `Client-1` with any user that has been created.   
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />
