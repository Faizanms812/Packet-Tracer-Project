# Packet-Tracer-Project

Introduction
---------------------------------
In this project, I will be testing the knowledge I have learned from my CCNA studies using JeremysIT lab course on YouTube and Implementing it into a Campus Area Network Project. I first began by creating multiple departments such as HR, IT, and Security and using Variable Length Subnetting (VLSM) to divide my address space efficiently and still support each department's number of hosts. I also took into consideration for future expansion. I used VLSM instead of classful and FLSM subnetting is because VLSM allows flexibility when assigning IP addresses to our networks. It allows us to minimize address waste and assign just enough available addresses for the hosts on the network. I will be explaining as many concepts of networking as I can using my own knowledge. I also implemented various services such as DHCP, DNS, FTP, SNMP, SSH, and more. I also used dynamic routing protocols such as OSPF and SVI's for management VLANs. HSRP for redundancy and ACLs for security. I will continue to update this project as I learn more about Cisco and Networking.

**Current Progress**

Note: I will keep updating this picture below as I progress and learn more about networking and the CCNA. This is my current progress with this project.

![image](https://github.com/user-attachments/assets/5a424ef6-7094-46a9-8f95-503af833ea13)


Goals for this lab
----------------------------
Configure layer 2 segmentation using VLANs, Trunks, and Access ports
Configure layer 3 segmentation using VLSM
Configure HSRP using SVI's
Configuring management VLANs 
Configuring ACLs
Configuring SSH, FTP, NTP, DHCP, DNS
Configuring OSPF 
Configuring EtherChannel using PAGP
Successfully allow all inter-VLAN routing and let all devices communicate with each other 

![image](https://github.com/user-attachments/assets/cce3e3f9-3aa1-45ab-a0eb-06bf6df7fad0)

Layout and Design
---------------------------------
At this stage, I have implemented this design of the network. I have multiple buildings and departments and implemented redundancy, 3-tier architecture and a dedicated server network that will provide essential services such as NTP, DHCP, and DNS to the entire CAN network. I added redundancy to the network to ensure each department can access the required services and minimize downtime, which could impact the business greatly. I have multiple devices running critical services such as DHCP, NTP and DNS to make sure these services do not fail as they are critical to network operation.

**Questions and Answers**

**Why add redundancy** - Redundancy is critical in any enterprise networks. This is because if one service fails such as the default gateway to other subnets, this could disrupt services and resources being accessed by your network. Another example can be if a critical router or switch fails its important to implement a plan for a backup device. This allows you to minimize downtime and keep operation running as normal. In my network I have implemented redundancy by having multiple switches, routers and services. I also have HSRP to have a backup router for each network to prevent any unwanted network outages.

**Why have multiple devices running services such as NTP, DHCP, and DNS?** - As I mentioned earlier it all comes down to redundancy. Critical services such as DHCP is a must in many networks, so in my network I have added DHCP on the multilayer switch and also included a helper address acting as a backup. 

![image](https://github.com/user-attachments/assets/4f48c90c-2ae3-4efc-b7e1-95ffa173a4ab)

Service and Remote Access
---------------------------------
I also successfully implemented DHCP network services on each multilayer switch. This allows for extra redundancy for each subnet. I added a DHCP pool for each subnet with the appropriate DHCP exclusion range. DHCP allows devices to automatically obtain an IP address through a process known as DORA (Discover, Offer, Request, Acknowledge). This process begins with a device requesting an IP address by sending a DHCP discover packet with a MAC address of all F's and a source address of 0.0.0.0 since the device does not have an IP address. The DHCP server will respond with a unicast packet with an offer that will contain network configurations such as IP addressing, DNS, default gateway and subnet mask. The device will respond by sending a broadcast request message, asking for the DHCP server for this address. The DCHP server will acknowledge the request and the device has now successfully joined the network with new network configurations.

**Questions and Answers**

**Why use DHCP?** - DHCP saves valuable time for network administrators since they do not need to manually assign an IP address to each device joining the network. It also is very user-friendly for the clients joining the network as they do not need to enter any network configurations by themselves. DHCP is an automated process that takes care of this by dynamically assigning IP addresses on a lease and using certain timers such as the T1, and T2 timers to renew the lease when needed. I use DHCP in my network for each building to provide quality of life to my clients and to save valuable time for my network administrator.

**Why is the MAC address all F's?** - A MAC address that looks like this FFFF.FFFF.FFFF is commonly known as a broadcast MAC address. This MAC address is used to send an ethernet frame to everyone on the connected network. This MAC address is commonly used in protocols such as ARP and DHCP to discover packets to find devices.

**What is the default gateway?** - The default gateway is the IP address that directs traffic that end users are requesting that are not part of their local network. It is the default location that nodes such as computers and phones send their data to when the resources they are looking for are not in their local network. For instance, if you want to access this page you are reading, your device will have to send data to your default gateway since GitHub servers are not in your local network.

**What is a unicast packet?** - Unicast packets are one-to-one communication between a host and the destination. For instance, if I ping using the computer in the IT department to a host in the security department, this is a unicast message. 

**What is a broadcast packet?** - Broadcast packets are one-to-all communication between a host and all other hosts in a network. For example, an ARP request would be considered a broadcast packet.

**What is an IP address?** - An IP address is a unique identifier for nodes on a network. This allows nodes on a network to differentiate themselves from each other and allow for layer 3 communication.

![image](https://github.com/user-attachments/assets/2018d196-344d-4667-acce-18bf1d17e708)

![image](https://github.com/user-attachments/assets/f85be7c4-23fd-49a9-b41c-3d70c25611c0)

**Command to configure DHCP**

ip dhcp excluded-address {ip-low} {ip-high}

ip dhcp pool {Pool name}

network {ip} {subnetmask}

default-router {ip}

dns-server {ip}

lease {hh:mm:ss}

Configuring Switches and Trunking
---------------------------------
Each multilayer switch has multiple VLANs configured to allow for inter-VLAN routing with the usage of SVI interfaces and trunk ports. SVI provides virtual interfaces that can be used for remote management and as a default gateway. Trunk ports are interfaces that allow switches to carry multiple types of VLAN data using 802.1q tagging. I also have a management VLAN to allow the admin department to remotely access each managed switch for configuration and administrative purposes. I also added a Misc VLAN and changed the native VLAN to an Unused VLAN for security purposes.

In this picture, I am remotely connecting to DSW1 for building A using Admin PC. I used a secure remote access protocol known as SSH and used the appropriate commands to generate an SSH key. I also added local login on the VTY lines, ACL, and only allowed SSH as communication. 

**Questions and Answers**

**What is SSH?** - SSH stands for secure shell and it is a protocol that is used to securely connect to a remote device. SSH uses user authentication such as a username and password to validiate the client identity. The authentication method SSH uses is either password based or public key based authentication. SSH also uses data encryption making text unreadable when it is transferred over the network. SSH also performs checksum to ensure integrity of data has remained valid. It also can be used for file transfer protocols such as SCP and SFTP to securely transfer files between devices. Currently there are two versions of SSH, SSH version 1 and SSH verison 2. 

**Why I choose to use SSH rather than Telnet?** - I used SSH because it provides security, encryption and uses strong authentication methods. Telnet provides no encryption and all data that is sent is in plain-text including username and password.

**What is 802.1q tagging?** - 802.1q tagging is a standard developed by the IEEE that defines how ethernet frames will be tagged with VLAN information. 802.1q is a technology that allows the concept of trunking possible on switches and lets all VLANs share the same phyiscal link. An additional header called the VLAN tag is inserted into the Ethernet frame, acting as an identifier for that VLAN. The 802.1q tag is 4-byte in length with 4 different fields. The first field is the TPID (tag protocol identifier) which is a 2 byte field that has a value of 0x8100 to determine the VLAN tag. The 2nd field is the PCP (Priority code point) which is a 3 bit field use for quality of service. QOS (Quality of Service) allows you to priortize traffic from most important to least. The 3rd field is DEI (Drop eligible indicator) this is a 1 bit field used to determine if a frame should be dropped. The DEI is crucial if the network becomes congested. The last field is the VID (VLAN ID) which indicate what VLAN the frame belongs. This field can have VLAN ranges from 0 to 4096, however VLAN 1 and 4095 are reserved.

**What is the native VLAN?** - The native VLAN is by default VLAN 1 when using 802.1q technology. The native VLAN does not tag any traffic that is passing through on the trunk port. When a switch recieve traffic that is untagged on the trunk port it will associate it with the native VLAN. You can also follow the best practice of changing the native VLAN to an unused VLAN to prevent VLAN hopping attacks.

![image](https://github.com/user-attachments/assets/34519d4b-57f5-40b7-939c-a85fa66b8c18)

**Commands used to create trunk ports**

interface (int-id)

switchport mode trunk

switchport trunk allowed vlan add {vlan-id}

switchport trunk native vlan (vlan-id)

Note - Instead of "add" you can use keywords such as except, remove, none, all depending on your requirements.

**Command used to create access ports**

interface (int-id)

switchport mode access

switchport access vlan {vlan-id}

**Commands used to create SVI**

vlan {number} 

interface {vlan-id}

ip address {ip} {subnet mask}

no shutdown 

Note: Remember SVI's are shutdown by default when created. Additionally SVI's will NOT turn on if there is no connected trunk port or device to that VLAN ID.

Configuring EtherChannel
--------------------------------
For this step, I used EtherChannel using the protocol PAGP which is a Cisco proprietary protocol. EtherChannel allows multiple interfaces to act as a single logical interface and avoids spanning tree protocol from disabling redundant paths. This allows my logical interface to provide more bandwidth to allow for faster connection to the distribution and core layers of the network. For instance, if you have a five 1Gbps links and bundle all of them in an EtherChannel this will provide 5Gbps of bandwidth.

Types of EtherChannel

1. **LACP (Link Aggregation Control Protocol) - IEEE 802.3ad**

   This is an industry-standard protocol that can be used across all vendors and allows the aggregations of multiple links. There are two forms of LACP, active and passive.

     **Active:** This will tell the switch interface running LACP to actively form a EtherChannel
   
     **Passive:** This will tell the switch interface running LACP to not actively form a EtherChannel

2. **PAgP (Port Aggregation Protocol) - Cisco Proprietary**

   PAgP is a technology that is designed by Cisco and only works between Cisco devices. PAgP has two forms known as desirable and auto.

     **Desirable:** Actively creates an EtherChannel
     **Auto:** Only creates EtherChannel if the other side is willing to initiate.

3. **Static (Manual EtherChannel)**

   This type of EtherChannel is manually configured by the user and you will have to enable EtherChannel on both sides, there is no negotation process like there is in LACP and PAgP.

**Commands to show and configure EtherChannel**

  interface range {int-low}-{int-high}

  channel-group 1 mode {active, passive, desirable, auto, on}

  interface Port-channel 1

  switchport mode trunk

**Common show commands for EtherChannel**

  show etherchannel summary

  show etherchannel port-channel

  show interfaces port-channel 1

**Considerations when making a EtherChannel**

  1. Configuration must MATCH on both sides
     
     - Interfaces that are in an EtherChannel MUST be IDENTICAL on both sides. This include duplex, speed, mode, VLANs.

  3. Port Compatibility

     - You can not mix gigabit interfaces with fastethernet
    
  4. Number of links must remain the same

     - One end can't have less or more than the other end. The link numbers on both ends MUST MATCH

![image](https://github.com/user-attachments/assets/ed75a70f-826f-46d6-adba-163c47eea506)

Configuring HSRP
-------------------------------
HSRP stands for hot standby routing protocol and allows for a virtual default gateway to be used which provide redundnacy on the network. It allows routers to work together and appear as a single virtual router from the perspective of the hosts on the network. If one router fails the hosts on the network will not lose connection since another router can take its place. I load balanced between each vlan by having my active and standby router different per vlan. I updated my DHCP pool for each VLAN as well to ensure all devices default gateway points to the virtual IP address.

**Terminology for HSRP**

   1. Virtual IP: Routers that run HSRP create a virtual IP address (VIP) that is shared between the routers. Both routers must be in the same HSRP group and the virtual IP address will be used as the default gateway for each host.
      
   2. Virtual MAC address: When a virtual IP address is created, a virtual MAC address is also generated. The active router will respond to all ARP requests sent in the network.
      
   3. Active & Standby: In HSRP the routers that are sharing the virtual IP address will have two roles, Active or Standby. The active router will be forwarding and recieving the actual data sent from the hosts in the network. It will also respond to ARP requests.
      The standby router acts as the backup router and is waiting for the active router to fail to become the new router. The determination of active and backup router is determine by HSRP priority or the highest IP address if the prioritys are equal on both routers.

   4. Hello msg: Router in the same HSRP group will send hello messages to ensure that both routers acknowledge eachothers presence. Hello messages are also used to notify if the active router is down. If the standby router stop recieving hello messages from the active 
      router, the standby router will assume the active router has failed.

**How HSRP works**

   We now understand the terminology of HSRP so lets put this all together. First, the routers that are in the same HSRP group will use an election process to determine who is the active and standby router. Both routers will share a virtual IP and virtual MAC address.     From the perspective of the host machines on the local network believe that there is a single device acting as a default gateway. If the active router fails, the standby will take its place. The active router will forward all traffic and respond to all arp requests.

   **HSRP States**

   There are 6 steps in the HSRP process, initial, learn, listen, speak, standby, and active. Lets take a closer look at each of these steps.

   1. Initial: This is where the router is starting up.
   2. Learn: The router is currently not aware of any HSRP hello messages and does not know the VIP
   3. Listen: The router is aware of the VIP but has not determined the active or standby router
   4. Speak: The router will send hello messages and now begin the HSRP election process
   5. Standby: The router will become the standby router
      OR
   6. Active: This router is forwarding traffic and will respond to ARP
      
   **HSRP CONFIGURATION**

   interface {int-id}

   ip address {ip} {mask} - begin by assiging an address to the interface you are working on

   standby {group-number} ip {ip address} - This defines the HSRP group and creates the VIP

   standby {group-number} priority {number} - This changes the priority of the router which changes the speak state of HSRP where the election process begins. It's good to change the priority between VLANs to provide load balacning.

   standby {group-number} preempt - This enable preemption

   standby {group-number} version {number} - HSRP has two versions, 1 and 2.

   standby {group-number} timer {hello-interval} {hold-time} - Keep this at default but this command is optional

   **HSRP show commands**

   show standby

   show standby brief

   show standby timers

   **Whats the difference between HSRP version 1 and 2**

   
![image](https://github.com/user-attachments/assets/e0f94d88-d08b-4398-9b9b-cae1aae5eedf)

Configuring HSRP for Building A
------------------------------------
![image](https://github.com/user-attachments/assets/afa7fe7f-51e7-4fc3-9d2c-990d54887f03)


Successful ping between Building A and Building B
------------------------------------------
I configured OSPF (Open shorted path first), a dynamic routing protocol that is used to learn routes to different networks using Dijkstra's algorithm. OSPF has an AD value of 110. AD stands for administrative distance and allows the router to determine the most trust worthy route. A lower AD equals a more reliable route. OSPF metric to determine the best path to a node is cost. It uses a reference bandwidth which by default is 100 Mb to determine the best path to the destination. 
![Untitled](https://github.com/user-attachments/assets/ece9aafc-750d-4f8c-8775-22dea5cb9c8a)

Current routing table (Note Building C is sill not configured)
![image](https://github.com/user-attachments/assets/fb16bdf7-180e-4b48-b4eb-1486943d8a87)

Configuring NTP services
--------------------------------------------
NTP is essential to have on a network to allow devices to accurately synch there clocks. This is very important for logging and troubleshooting information. NTP stands for network time protocol and uses UDP port 123. 
![image](https://github.com/user-attachments/assets/22bc759c-6434-49af-8a82-af0157b5497a)

Configuring Syslog services
------------------------------------------
Successfully configured a dedicate syslog server to allow networking devices to store log information. Syslog is a protocol used for logging information from varying severity levels ranging from 0 to 7. It is used to troubleshoot and respond to issues.

![image](https://github.com/user-attachments/assets/8d6d6b32-4233-4fb5-9d4b-b2fbce318cb2)

Successful file transfer using FTP protocol
----------------------------------------
FTP stands for file transfer protocol, a protocol used to transfer files from one device to another. It uses authentication methods such as username and password but has no encryption ability. I created a username and password for my device in the IT network to remotely connect to the FTP server and download the files it needs.
![image](https://github.com/user-attachments/assets/b3dd433f-a35a-40b3-813a-2b1aaf201a3c)

Applying ACLs
------------------------

Applying ACLs to prevent unauthorized access to departments. In this image I applyed an ACL to the SVI on vlan 10 to prevent IT and HR from accessing each others networks. I used a more granular control to allow certain devices in IT department to access the HR department for management purposes. In the multilayer switch configuration I allowed he host 10.0.0.2 to access 10.0.1.192 network but denied the rest of the hosts in any other network. I applied the ACL inbound the VLAN 10 SVI.

![image](https://github.com/user-attachments/assets/7ac759f6-4087-4761-85d7-37e9c1d4a46b)

Configuring host files on DSW1
--------------------------------
Host files are a file on a device that store IP address to hostname mapping. They were used before DNS servers and were very common during the 90's.
![image](https://github.com/user-attachments/assets/0900ef2a-804d-4af8-922b-e45a14c2956b)

Configuring DNS services
---------------------------------
In this image I successfully configured DNS services in my server farm and I used commands such as domain-lookup and nameserver to allow the routers and switches to contact the DNS server to provide the proper IP address to hostname mappings if they do not contain the neccessary information in there host files.
![image](https://github.com/user-attachments/assets/17969917-bb53-4b91-a4ec-2934d042bbd6)

Current Progress
---------------------

Current Progress of my network, had to make many adjustments and changes. I got rid of the gym network to keep it more simple and changed the configuration of the server farm.

![image](https://github.com/user-attachments/assets/461e6867-421e-4171-bf6f-02e80714ee57)













