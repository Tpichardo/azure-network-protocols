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
- Remote Desktop (if using Mac, download Windows App, previously known as Remote Desktop)
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
3. Create a Windows 10 virtual machine
4. Create a Linux (Ubuntu) virtual machine. Ensure that this VM is on the same virtual network as your windows VM


