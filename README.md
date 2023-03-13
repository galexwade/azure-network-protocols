<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Actions and Observations</h2>
<!--- Step 1 -->
<p>
In this tutorial, we are going to learn about Network Security Groups and Inspecting Network Protocols. To begin, we will need to create two Azure Virtual Machines. One will be a Linux Machine and the other will be a Windows 10 Machine. Both will use two CPUs and they must be on the same Virtual Network in Azure.
</p>
<p>
Once the machines are created, we will need to download <a href="https://www.wireshark.org/download.html">Wireshark</a>. After installing, open up Wireshark, and filter for Internet Control Message Protocol (ICMP) traffic only. ICMP is a network layer that relays messages concerning network connection issues. Ping uses this protocol and tests connectivity between hosts. When we filter Wireshark to only capture ICMP packets and ping the private IP address of our Linux Machine, we can visually see the packets on Wireshark.
</p>
<p>
<img src="https://i.imgur.com/TLQaAt7.png" height="80%" width="80%" alt="Internet Control Message Protocol"/>
</p>
<p>
Now we can inspect each individual packet and see the data that is being sent in each ping shown below:
</p>
<p>
<img src="https://i.imgur.com/t5l8iiG.png" height="80%" width="80%" alt="Internet Control Message Protocol dropdown"/>
</p>
<br />

<!--- Step 2 -->
<p>
Next, we will ping the Linux Machine nonstop with the command "ping -t". This will ping the machine until we decide to stop it. While the Windows Machine is pinging the Linux Machine, we will go to the Linux Machine and block the inbound ICMP traffic on it's firewall. Once we do this, we will stop recieving echo replys from the Linux Machine. We will block ICMP by creating a new Network Security Group on the Linux Machine that will be set to block ICMP. We can allow the traffic by allowing ICMP on the Linux Network Security Group, on Azure.
</p>
<p>
<img src="https://i.imgur.com/bEXCsF0.png" height="80%" width="80%" alt="Administrator Comand Prompt"/>
</p>
<p>
<img src="https://i.imgur.com/aLKFhAy.png" height="80%" width="80%" alt="Inbound security rules"/>
</p>
<br />

<!--- Step 3 -->
<p>
Next, we will use our Window's Machine to SSH to the Linux Machine. SSH has no Graphical User Interface, it just gives the User access to the Machines Command Line Interface. We will set the Wireshark filter to capture SSH packets only. When we SSH into the Linux Machine with the command "ssh labuser@10.0.0.5", we can see that Wireshark starts to immediately capture SSH packets.
</p>
<p>
<img src="https://i.imgur.com/1lgITuo.png" height="80%" width="80%" alt="Powershell"/>
</p>
<br />

<!--- Step 4 -->
<p>
Next we will use Wireshark to filter for DHCP. Dynamic Host Configuration Protocol (DHCP) works on ports 67/68. It is used to assign IP addresses to Machines. We will request a new IP address with the command "ipconfig /renew". Once we enter this command, Wireshark will capture DHCP traffic:
</p>
<p>
<img src="https://i.imgur.com/G2jwWVG.png" height="80%" width="80%" alt="Wireshark and Powershell"/>
</p>
<br />

<!--- Step 5 -->
<p>
Now lets filter DNS traffic. When we set Wireshark to filter DNS traffic, we initiate DNS traffic by typing in the command "nslookup www.google.com" (any url can be used in this exact, we however are using Google). This command will ask our DNS server what is Google's IP address.
</p>
<p>
<img src="https://i.imgur.com/oFPsL6c.png" height="80%" width="80%" alt="Wireshark"/>
</p>
<br />

<!--- Step 6 -->
<p>
Now we will filter for Remote Desktop Protocol (RDP) traffic. When we enter "tcp.port == 3389", traffic is constant because we are using RDP to connect to our Azure Virutal Machines.
</p>
<p>
<img src="https://i.imgur.com/SeeoU4S.png" height="80%" width="80%" alt="Wireshark TCP"/>
</p>
<br />
