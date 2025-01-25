<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>
<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
<br/>

In this tutorial we will use Wireshark to observe various network traffic between two Azure virtual machines, as well as, experiement with Network Security Groups.

<h2>What Is Wireshark?</h2>
Wireshark is a software tool used to capture and amalyze data packets that are transmitted over a network.

<h2>What Are Network Security Groups?</h2>
Network Security Groups are a set of defined rules that control the traffic to and from Azure resources. In our case, the resources are Azure virtual machines. So our network security group will use rules to determine if traffic should be allowed or denied, acting like a virtual firewall for our virtual machines.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- [Windows App](https://apps.apple.com/us/app/windows-app/id1295203466?mt=12) or Remote Desktop
- Wireshark (Note that this will be downloaded to the Windows VM, not your physical computer)
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)

<h2>Operating Systems Used</h2>

- Windows 10 (22H2)
- Ubuntu Serve 22.04

<h2>Create Virtual Machines</h2>
Create two virtual machines on the same virtual network. One will use a Windows 10 image and the other will use an Ubuntu Server 22.04 image. 

1. Log in to the Azure portal
2. Create a Resource Group to add the virtual machines to. This will help us keep our resources organized
3. Create a Windows 10 virtual machine. Note: Select "password" as authentication type for both VMs, we will use these credentials to authenticate ourselves later.
4. Create a Linux (Ubuntu) virtual machine. Ensure that this VM is on the same virtual network as your windows VM

<h2>Download and Install Wireshark</h2>

1. Open the Windows App and click on the <b>+</b> at the top right of the window to add a PC
  <img src="https://i.imgur.com/q2CIjtQ.png" height="80%" width="80%" alt="interface for adding a PC on the Windows App"/>
2. Paste your Windows VM's public IP address for "PC name". Feel free to give your VM whatever friendly name you would like. I named mine windows-vm so that I can easily know what OS that PC is running. Then click "Add" to add the VM
  <img src="https://i.imgur.com/cvBmV30.png" height="80%" width="80% alt=""/>
3. Click on the ellipsis and select <b>connect</b> to connect to the VM
   <img src="https://i.imgur.com/OKSJhL1.png" height="80%" width="80%  alt=""/>
4. Add the username and password you created when you created your virtual machine to authenticate yourself
  <img src="https://i.imgur.com/dkhuqJB.png" height="80%" width="80%  alt=""/>
5. Download and install [Wireshark](https://www.wireshark.org/) within your Windows 10 VM. Choose the Windows x64 Installer

<h2>Observe ICMP Traffic</h2>

1. In Wireshark, select <b>ethernet</b> and then select the shark fin on the top left corner of the window so begin viewing the network activity
2. 
