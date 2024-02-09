
<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>
<h1>Network Security Groups (NSGs) and Analysing Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)
- PowerShell (Command-Line Interface)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Step 1: Create Virtual Machines
- Step 2: Remote Desktop into VMs
- Step 3: Install & Run WireShark
- Step 4: Test Connectivity Between VMs
- Step 5: Alter Network Security Group Settings
- Step 6: SSH into VM
- Step 7: Observe DHCP Traffic

<h2>Actions and Observations</h2>

Step 1: Create two Vitrual Machines in Azure
<p>
- Log into the Microsoft Azure Portal with your Microsoft Account
  </p>
- Create "resource group"
  </p>
- Create two Virtual Machines (DC-1 and Client-1)group
  </p>
- When setting up the VMs remember the username and password that you use. You'll need this information later to remote desktop into the VMs
</p>
- I recommend using the same login information for both VMs
<p>
 </p>
<img src="https:///VL9V0o6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
</p>
<br />
</p>
<p>
Step 3: Remote Desktop into Client-1
  <p>
  - Download and Run WireShark. 
    <p>
  - Open WireShark
       <p>
  - Click "Ethernet 2"
          <p>
  - Select "shark symbol" in upper left hand corner to start network traffic monitoring  
             <p>
  - Filter out irrelevant network traffic via display filter bar (type "icmp" in filter bar to only see ping traffic). 
               <p>
      - traffic can also be filtered using port numbers using the following syntax: tcp.port==22
               </p>
</p>
<br />
<p>
<img src="https:///bidy0lW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  
  <- Absence of ICMP traffic because no ping command has been made/>
<img src="https://imgur.com/Yifwztp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
  - ICMP requests and replies being observed now after a perpertual ping of server (10.0.0.4) from Client-1
  <p>
  <img src="https://imgur.com/7IgDHCU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
</p>
<p></p>
</p>
<p>
Step 4: Use NSG to Deny ICMP Traffic for DC-1 in Azure Portal and Observe Traffic Behaviour in wireshark
  <p>
    - NSG is an equivalent of firewall. 
    <p>
    - Select "DC-1-NSG"
      </p>
    - "Inbound security rules" 
        </p>
    - Under settings click "+ add"
        </p>
    - In the new pop up window change "protocol" to "ICMP"
        </p>
    - Change "action" to "deny" 
        </p>
    - Change "Name" to "Deny_ICMP_Ping"
        </p>
    - Click "add" at bottom. 
        </p>
    - *Notice how the ping in command line (cmd) on Client-1 done on step#3 is immediately halted due to being blocked by DC-1's firewall thanks to the new inbound security rule made. 
        </p>
    - If you edit the rule to allow traffic, notice how the perpetual ping cmds successfully resume. 
        </p>
    - *Press Ctrl+C to stop PowerShell ping
      <p>
      </p>
<img src="https:///5y1gdyo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
    - no response on wireshark
  <p>
    <img src="https://imgur.com/eO1BTWk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
    <p>
    - request times out on command prompt
    <img src="https://imgur.com/E2O5PtC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  
</p>
<br />
</p>
<p>
Step 5: SSH into DC-1 from Client-1 via PowerShell
  <p>
    <p> 
      - Note* the VM (DC-1) I tried to control using SSH was running on windows and ssh requests were timing out. SSH works with linux. Please take note. I had to start the whole lab again and set my DC-1 on Ubuntu. 
    </p>
  - Re-start capture on wireshark to clear previous filters
    <p>
  - Type "SSH" into WireShark's filter bar
      <p>
  - Go to "PowerShell" and type "ssh labuser@(DC-1 private IP address)
        <p>
        </p>
  - Type "yes" to connection prompt
       <p>
  - Enter password on next cmd line (password will not show but enter it anyway and press enter key, it will register)
              <p>
  - You have now successfully remotely logged into VM2's command-line Interface (CLI). It should now read "labuser@VM2:"
                     <p>
  - You can type a linux cmd such as: "id", "pwd" to see the new network traffic between the VMs. 
                            <p>
  - Filter DHCP traffic
                              </p>
  - When done exploring, type "exit" into cmd line on PowerShell to end the connection. You can always log in back by enteri ng the password for DC-1. 
        <p>
 ** - SSH Traffic**
          <p>
    <img src="https://imgur.com/LTyQMND.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
</p>
  - **DHCP traffic**
</p>
 <img src="https://imgur.com/ljJW62h.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
</p>
<br />
