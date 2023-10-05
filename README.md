# configure-ad
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

- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
-Setup Remote Desktop for non-administrative users on Client-1
-Create a bunch of additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/LxFnk53.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
Inside Azure portal create virtual machine for domain computer, name resouce group in exp. or at leisure, next name VM, select OS Windows server 2022 datacenter.
</p>
<br />

<p>
<img src="https://i.imgur.com/O2XbVdR.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/vDrcv1B.png" height="45%" width="45%" alt="Disk Sanitization Steps"/>
</p>
<p>
Below select Atleast 2vcpus for DC VM, use labuser for login or at leisure just remember info. select confirm licensing and review and create machine with noting what Vnet you will create
</p>
<br />

<p>
<img src="https://i.imgur.com/4cTyv85.png" height="45%" width="45%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/xkFvHWN.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
While DC-1 is creating go back to azure portal create another VM but name it Client-1 VM, and make sure its in same RG as DC-1 like second example above.
</p>
<br />

<p>
<img src="https://i.imgur.com/vPL7LE6.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next you will need to Set Domain Controller’s NIC Private IP address to be static, go thru azure portal and open DC-1 network interface, and go to IP config.
</p>
<br />

<p>
<img src="https://i.imgur.com/6stwlua.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/lbIu6h5.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
Click to open ipconfig1, then change from dynamic to static.
</p>
<br />

<p>
<img src="https://i.imgur.com/goGUGNv.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
Verify both VM's created are connected to same vnet, go to RG>DC>resourse visualizer.
</p>
<br />

<p>
<img src="https://i.imgur.com/WJbCNiQ.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/3h55aN0.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/YxI6Ysn.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

</p>
<p>
Next we can ensure Connectivity between the client 1 and Domain Controller
  -Login to Client-1 with Remote Desktop.
  
 -Open Command prompt.
  
  -ping DC-1’s private IP address with ping <ip address> -t (perpetual ping) observe as it times out due to firewall in DC-1 Enable ICMPv4.



</p>
<br />

<p>
<img src="https://i.imgur.com/SlnHfWD.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/ZRrvt5e.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/rkY9wcr.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
-Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall, search firewall advanced security.

-Filter in in windows firewall by protocol, then enable all of ICMPv4.

-After you can open VM client and observe as the ping now is enabled.
</p>
<br />

<p>
<img src="https://i.imgur.com/BXGp89y.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/p37Kwp3.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next to do is Install Active Directory, Login to DC-1 and install Active Directory Domain Services. Go to server manager click add roles and features.

In Wizard install AD Domain services.
</p>
<br />

<p>
<img src="https://i.imgur.com/rwvujqd.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/As4hSoz.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/mILTVbr.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once Installed in server manager click on flag in top right corner and click promote server.

Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)

then after restart DC-1 and login as mydomain.com\<login name>


</p>
<br />

<p>
<img src="https://i.imgur.com/Kmg2vat.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/5ZCnJal.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/8uAogow.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/GK5mgNh.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
-Next we will Create an Admin and Normal User Account in AD

-In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES” and also for _ADMINS

</p>
<br />

<p>
<img src="https://i.imgur.com/elT0TTd.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DtMViSV.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>  
</p>
<p>
-Back in ADUC create new employee “Jane Doe” with the username of “jane_admin”

</p>
<br />

<p>
<img src="https://i.imgur.com/pdDxiLY.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/nykNXZr.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
-Now add jane_admin to the “Domain Admins” Security Group


-then after relogin as mydomain.com\jane_admin which will be used form now on.

</p>
<br /><p>
<img src="https://i.imgur.com/dR5IBRO.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
-Next we Join Client-1 to your domain (mydomain.com), in Azure locate DC-1 Private IP address under DC-1>networking there you can find the IP copy and paste to client 1 DNS settings.
</p>
<br />

<p>
<img src="https://i.imgur.com/lLdkPP7.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/sEgyFE1.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
-In azure click client 1 VM > network > network interface >DNS servers and paste DC-1's IP as new DNS server for client-1 and save.

- then from the Azure Portal, restart Client-1


</p>
<br />

<p>
<img src="https://i.imgur.com/WJbCNiQ.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/PQLarHl.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/SgjetfE.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/JkabFSN.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
- Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
</p>
<br />

<p>
<img src="https://i.imgur.com/wKbgRlz.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/c7jT05v.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/npPSBzF.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

</p>
<p>
-Setup Remote Desktop for non-administrative users on Client-1, go to computer settings > About > Remote desktop >select DOMAIN users to allow access.
</p>
<br />

<p>
<img src="https://i.imgur.com/qbvvdMH.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
OPTIONAL:
 
-Create a bunch of additional users and attempt to log into client-1 with one of the users

-Login to DC-1 as jane_admin

-Open PowerShell_ise as an administrator

-
</p>
<br />

<p>
<img src="https://i.imgur.com/17Pyxda.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
-Create a new File and paste the contents of the script into it 

  -download random user code creator.[https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1](copy/paste into Powershell opened)

  -note when creating the users at the top of code you will see password for all users and number of user it will make.
</p>
<br />
