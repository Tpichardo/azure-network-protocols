<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>
<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
<br/>

In this tutorial we will use Wireshark to observe various network traffic between two Azure virtual machines, as well as, experiement with Network Security Groups.

<h2>What Is Wireshark?</h2>
Wireshark is a software tool used to capture and analyze data packets that are transmitted over a network.

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

1. Log in to the Azure portal.
2. Create a Resource Group to add the virtual machines to. This will help us keep our resources organized.
3. Create a Windows 10 virtual machine. Select <b>password</b> for authentication type for both VMs. Save these credentials, as we will use them for authenticating later.
4. Create a Linux (Ubuntu) virtual machine. Ensure that this VM is on the same virtual network as your Windows VM.

<h2>Download and Install Wireshark</h2>

1. Open the Windows App and click on the <b>+</b> at the top right of the window to add a PC.
   <img src="https://i.imgur.com/q2CIjtQ.png" height="80%" width="80%" alt="interface for adding a PC on the Windows App"/>
2. Paste your Windows VM's public IP address for <b>PC name</b>. Feel free to give your VM whatever friendly name you would like. I named mine windows-vm so that I can easily know what operating system is running on the PC. Then click <b>Add</b> to add the VM.
      <img src="https://i.imgur.com/cvBmV30.png" height="80%" width="80%" alt=""/>
3. Click on the ellipsis and select <b>connect</b> to connect to the Windows VM.
   <img src="https://i.imgur.com/OKSJhL1.png" height="80%" width="80%" alt=""/>
4. Add the username and password you created when you created your virtual machine in Azure to authenticate yourself.
    <img src="https://i.imgur.com/dkhuqJB.png" height="80%" width="80%" alt=""/>
5. Once connected, open the browser within your Windows VM to download and install [Wireshark](https://www.wireshark.org/). Choose the <b>Windows x64 Installer</b>.

<h2>Observe ICMP Traffic</h2>
Ping the Linux VM:

1. In Wireshark, select <b>ethernet</b>. Then select the blue shark fin at the top left corner of the window to begin viewing the network traffic.
    <img src="https://i.imgur.com/uuF3IKC.png" height="80%" width="80%" alt=""/>
    <img src="https://i.imgur.com/uOzA5mF.png" height="80%" width="80%" alt=""/> 
2. Type `icmp` on the bar at the top to filter for ICMP traffic only. ICMP, or Internet Control Message Protocol operates on layer 3 of the OSI model and is used to relay information about network issues. Ping is a tool that uses ICMP to test connectivity between two devices by sending an echo request and waiting for an echo reply. It's like one computer asks "Hey, are you there?" and the other responds with "Yes, I'm here."

    <img src="https://i.imgur.com/5VmhUAp.png" height="80%" width="80%" alt=""/>
4. Open Power Shell on the Windows VM to ping the linux VM. Let's ping the Linux VM's private IP address instead of the public IP address for improved security and efficiency: `ping 10.0.0.5`.
    <img src="https://i.imgur.com/LBHFZtz.png" height="80%" width="80%" alt=""/>
5. Now we can see the packets that were sent across the network when we pinged the Linux PC and idividually examine each one.

<h2>Experiment with Network Security Groups</h2>
Pertually ping the Linux VM, and use NSGs to deny ICMP traffic to the Linux VM:

1. Let's perpetually ping the Linux VM with `ping -t 10.0.0.5`. This will send a continuous ping to the Linux VM until we decide to stop it.
3. In your Azure portal, go to the Linux VM's network security group, and set a rule to deny ICMP traffic. Once this is done, our echo request will begin to time out as we will stop receiving echo replies from the Linux VM. You can also see this by observing the ICMP traffic on Wireshark.

   <img src="https://i.imgur.com/mOaDU5w.png" height="80%" width="80%" alt=""/>
   <img src="https://i.imgur.com/JfPk36p.png" height="80%" width="80%" alt=""/><br>
   
   Notice how there are no longer any replies from the Linux VM, only requests.
   <img src="https://i.imgur.com/gS5yukg.png" height="80%" width="80%" alt=""/>
5. To allow ICMP traffic, just delete the rule on the NSG and the Linux VM will eventually begin sending echo replies.
   <img src="https://i.imgur.com/on8Q7gP.png" height="80%" width="80%" alt=""/>
7. Stop the perpetual pings with <b>CTRL + C</b>

<h2>Observe SSH Traffic</h2>
Establish a secure remote connection to the Linux VM using the Windows VM to log in through SSH:

1. Filter for SSH traffic in Wireshark. SSH or Secure Shell is a network protocol that allows users to securely access a computer over an unsecured network
2. In PowerShell, type `ssh <username>@<private IP>` to connect. For example, I would type: `ssh labuser@10.0.0.5`. We can see that Wireshark immediately starts to display SSH traffic.
3. After authenticating with the credentials created for the Linux VM, you’ll have access to your VM as if it were physically in front of you. However, this access is limited to the command line. This means you can create and delete files, run programs, and manage the system, but only through plain text commands—no Graphical User Interface (GUI).
4. We can see that we are actually connected to our Linux VM because our prompt has changed. We can also verify our connection by typing `hostname` on the command prompt. This should return the name that you gave to your Linux VM when you created it on Azure.
5. To exit the SSH connection just type `exit`. Notice how our command prompt has changed, and typing `hostname` now returns the name of our Windows VM.

<h2>Observe DHCP Traffic</h2>
Release our Windows VM's IP address and request a new one from the DHCP server:

1. Filter for DHCP traffic on Wireshark. DHCP or Dynamic Host Configuration Protocol is used to automatically assign IP addresses to devices connected to the network.
2. Create a <b>.bat</b> file within the Windows VM to execute multiple commands in a sequence. The file will contain the following commands: `ipconfig /release` and `ipconfig / renew`. The first command will release our IP address, causing us to temporarily loose connection to our VM. The second command will then run, allowing our Windows VM to get an IP address and reestablish our connection to our Windows VM.
3. Observe the DHCP traffic on Wireshark.
   - We can see the <b>release</b> of our IP address occured.
   - Then the Windows VM sent a DHCP <b>Discover</b> Message looking for a DHCP server.
   - The DHCP Server then responded with a DHCP <b>Offer</b>, suggesting an available IP address.
   - The Windows VM sent a <b>Request</b>, requesting to use the offered IP address.
   - Finally the DHCP server sends an <b>Acknowledgment</b> message to confirm and finalize the IP address assignment.  
<h2>Observe DNS Traffic</h2>
Request the IP address for popular sites

1. Filter for DNS traffic on Wireshark. DNS or Domain Name Server is used to map human readable domain names to IP addresses.
2. Lets request Google's IP address! On Power Shell type `nslookup google.com`. This will return Google's IP address.
3. Use this IP address to access google via your browser. For security purposes, you won't be able to successfully do this for every website you try to access with the IP address.
4. Feel free to request the IP addredd for any other site you're interested in by following the command `nslookup <websitename>`

<h2>Observe RDP Traffic</h2>

1. Filter for RDP traffic on Wireshark with `tcp.port == 3389`. RDP or Remote Destop Protocol is used to remotely access a computer. Very similar to SSH, except this time we have a Graphical User Interface (GUI). We are currently using RDP to use our Windows VM.
2. Notice how the traffic on Wireshark is non-stop because we are currently using RDP.

<h2>Delete Resource Group</h2>
Delete your resource group to avoid accruing heavy charges

1. Go into the Azure portal and delete your resource groups. This will in turn delete both the Windows and Linux VM.
