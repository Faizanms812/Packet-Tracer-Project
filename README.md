# Packet-Tracer-Project

Introduction
---------------------------------
In this project, I will be using the knowledge I have learned from my CCNA studies and applying it by designing my own network. I  began by creating multiple departments such as HR, IT, and Security and using Variable Length Subnetting (VLSM) to divide my address space efficiently and support each department's number of hosts. I also took into consideration for future expansion. I used VLSM instead of classful and FLSM subnetting because VLSM allows flexibility when assigning IP addresses to our networks. It minimizes address waste and assigns just enough available addresses for the hosts on the network. I will be explaining as many concepts of networking as I can using my own knowledge. I also plan on implementing various services such as DHCP, DNS, FTP, SNMP, SSH, ACLs, TFTP, NAT and more. I also plan on using dynamic routing protocols such as OSPF and SVIs to act as default gateways for HSRP and to provide IP addressing for my management VLANs. I will continue to update this project as I learn more about Cisco and Networking.

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

**Why add redundancy** - Redundancy is critical in any enterprise network. This is because if one service fails such as the default gateway to other subnets, this could disrupt services and resources being accessed by your network. Another example can be if a critical router or switch fails it's important to implement a plan for a backup device. This allows you to minimize downtime and keep operations running as normal. In my network, I have implemented redundancy by having multiple switches, routers and services. I also have HSRP to have a backup router for each network to prevent any unwanted network outages.

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

![image](https://github.com/user-attachments/assets/34519d4b-57f5-40b7-939c-a85fa66b8c18)


**Questions and Answers**

**What is SSH?** - SSH stands for secure shell and it is a protocol that is used to securely connect to a remote device. SSH uses user authentication such as a username and password to validiate the client identity. The authentication method SSH uses is either password based or public key based authentication. SSH also uses data encryption making text unreadable when it is transferred over the network. SSH also performs checksum to ensure integrity of data has remained valid. It also can be used for file transfer protocols such as SCP and SFTP to securely transfer files between devices. Currently there are two versions of SSH, SSH version 1 and SSH verison 2. 

**Why I choose to use SSH rather than Telnet?** - I used SSH because it provides security, encryption and uses strong authentication methods. Telnet provides no encryption and all data that is sent is in plain-text including username and password.

**What is 802.1q tagging?** - 802.1q tagging is a standard developed by the IEEE that defines how ethernet frames will be tagged with VLAN information. 802.1q is a technology that allows the concept of trunking possible on switches and lets all VLANs share the same phyiscal link. An additional header called the VLAN tag is inserted into the Ethernet frame, acting as an identifier for that VLAN. The 802.1q tag is 4-byte in length with 4 different fields. The first field is the TPID (tag protocol identifier) which is a 2 byte field that has a value of 0x8100 to determine the VLAN tag. The 2nd field is the PCP (Priority code point) which is a 3 bit field use for quality of service. QOS (Quality of Service) allows you to priortize traffic from most important to least. The 3rd field is DEI (Drop eligible indicator) this is a 1 bit field used to determine if a frame should be dropped. The DEI is crucial if the network becomes congested. The last field is the VID (VLAN ID) which indicate what VLAN the frame belongs. This field can have VLAN ranges from 0 to 4096, however VLAN 1 and 4095 are reserved.

**What is the native VLAN?** - The native VLAN is by default VLAN 1 when using 802.1q technology. The native VLAN does not tag any traffic that is passing through on the trunk port. When a switch recieve traffic that is untagged on the trunk port it will associate it with the native VLAN. You can also follow the best practice of changing the native VLAN to an unused VLAN to prevent VLAN hopping attacks.

This is my trunk configuration for ASW2 in Sales

![image](https://github.com/user-attachments/assets/0cc2e429-081a-4723-8b27-2285f2f35eea)

Access port information for ASW2 on sales, currently DTP is on and I will later turn it off for security purposes.

![image](https://github.com/user-attachments/assets/353f73e3-3392-4da7-8270-2cb54acae20a)

**What is DTP?** - DTP is a Cisco protocol known as Dynamic trunking protocol and allows switches to dynamically negotiate if a link should be a trunk or not.

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

SVIs for DSW1 for building A

![image](https://github.com/user-attachments/assets/19511227-b71c-4bf3-bd09-f35791c51571)

![image](https://github.com/user-attachments/assets/e0434029-be77-458c-a3fd-6221747da2f2)

SVIs for DSW2 for building B

![image](https://github.com/user-attachments/assets/0811340d-2b03-401e-8e22-ff3505090502)

![image](https://github.com/user-attachments/assets/a4a1fada-b6ba-4905-b97c-ead0d9a60963)

SVIs for DSW1 for building C

![image](https://github.com/user-attachments/assets/c4a3185f-828a-4709-8d1a-13bca182e694)

![image](https://github.com/user-attachments/assets/f7241f28-8ade-4453-85a2-58d9da7f565e)


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

What is HSRP?
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

   HSRP V1: Version 1 of HSRP supports only 255 groups and uses mutlicast address 224.0.0.2

   HSRP V2: Version 2 supports 4096 groups and uses multicast address 224.0.0.102.
   
![image](https://github.com/user-attachments/assets/e0f94d88-d08b-4398-9b9b-cae1aae5eedf)

Configuring HSRP for Building A
------------------------------------

![image](https://github.com/user-attachments/assets/afa7fe7f-51e7-4fc3-9d2c-990d54887f03)

HSRP for building B
------------------

![image](https://github.com/user-attachments/assets/26e8e4d6-a338-44d5-960a-0afa4dd5805b)

HSRP for building C
------------------------

![image](https://github.com/user-attachments/assets/164d8507-6ae0-472f-8c27-4f4c617fef03)


Successful ping between Building A and Building B
------------------------------------------

![image](https://github.com/user-attachments/assets/e8e9e47c-2587-484b-9854-001bdfab2bc9)

I configured OSPF (Open shorted path first), a dynamic routing protocol that is used to learn routes to different networks using Dijkstra's algorithm. OSPF is an interior gateway protocol (IGP) that operates in a single autonomous system (AS). AD stands for administrative distance and allows the router to determine the most trust worthy route. A lower AD equals a more reliable route. OSPF metric to determine the best path to a node is cost. It uses a reference bandwidth which by default is 100 Mb to determine the best path to the destination. OSPF uses hello packets which are multicast address 224.0.0.5 and 224.0.0.6 to communicate and establish neighborship with other OSPF enabled routers.

**OSPF Concepts**

   1. Link-state Protocol: OSPF routers will exchange information with all other routes and create a complete network map known as the link-state database (LSDB). Routers can use this map to determine the shortest and cheapest path to the destination.

   2. Cost: OSPF uses costs to determine the best path to the destination. By default OSPF has a reference bandwidth of 100Mbs and should be changed to a different bandwidth when configuring OSPF. The lower the cost the better the path.

   3. Convergence: When changes occur such as a network link going down, OSPF can quickly change the network map in the LSDB and send updates to other routers that are affected.

   4. Areas: OSPF also can segment networks into smaller areas which can reduce the size of routing tables and minimze network traffic

**How OSPF creates neighborship**

   OSPF has multiple states it goes through to create neighborship and exchange routing table information

   1. DOWN - The initial state of OSPF where the router has not attempted to contact its neighbors, nor has it recieved any hello packets.
      
   2. Init - The init state is when a OSPF enabled router recieves a Hello packet from a neighboring router. However two-way communication is still not established
      
   3. 2-way - Both routers have recieved hello packets from eachother and there own RID (Router-id) is in the hello messages. This state OSPF adjacencies are fully formed. Additionally DR and BDR routers are selected at this state.

   4. ExStart - This state determines which router will initate the sending of DBD (Database description) packets which contain a list of LSA in a routers LSDB. The router with the higher router ID (RID) will become the Master and the lower RID router will be the Slave.

   5. Exchange - This state the routers begin the actual exchange of DBD packets. Router use the information inside of the DBD packet and compare it to there own LSDB and determine which LSA they need to request from the other router.

   6. Loading - The routers exchange specific LSAs and request any missing or changed LSA from there neighboring router using LSR (link state request packets). LSA packets are then sent using LSU (link state update) packets. This process will keep going until both    
      routers have an updated network map.

   7. Full - This stage establishes full adjaceny between OSPF routers. Once both routers have an updated network map, they will transition into the full state and exchange OSPF hello messages to maintain neighborship. This stage is all about maintaining the    
      relationship with OSPF routers.

**The difference between neighborship and adjacencies**

   1. Neighbors - These OSPF routers establish neighborship but do not establish and share LSAs.
      
   2. Adjacencies: Router are fully synchronized and can exchange LSAs. Only DR and BDR can perform full adjacenies. DROther to DROther can not, they will only perform neighborship.

**OSPF commands**

   router ospf {process-id}

   network {ip} {wildcard} area {area-id}

   router-id {ip}

   ip ospf {process-id} area {area-id} - enable OSPF directly on an interface

   passive-interface {interface-id} - disable OSPF from sending Hello messages on certain interfaces

   ip ospf cost {value} - be in interface config mode

   ip ospf hello-interval {seconds}

   ip ospf dead-interval {seconds}

   auto-cost reference-bandwidth {Mbps}

**OSPF show commands**

   show ip ospf neighbor - Shows OSPF neighbors and their states (DR, BDR, DRother)

   show ip route ospf - Router learned by OSPF

   show ip ospf - Details about OSPF like RID, process id, area, LSA

   show ip ospf database - All the LSA in the router

   show ip ospf summary - OSPF information in summarized format
      
Current routing table for CoreR1 (Note Building C is sill not configured)

![image](https://github.com/user-attachments/assets/b72e05f0-6ba7-46ff-8842-0529b345cce9)

![image](https://github.com/user-attachments/assets/fb16bdf7-180e-4b48-b4eb-1486943d8a87)

OSPF database for CoreR1

![image](https://github.com/user-attachments/assets/f2acbe29-5d31-43f8-ad9f-971f2e02eca9)

OSPF neighbors for CoreR1

![image](https://github.com/user-attachments/assets/d719d932-a644-48af-b724-fab9d62e6ef3)

Configuring NTP services
--------------------------------------------
NTP is essential to have on a network to allow devices to accurately synchronize there clocks. This is very important for logging, security certificates, and troubleshooting information. NTP stands for network time protocol and uses UDP port 123. 

**Concepts of NTP**

   1. Time Synchronization: NTP sync networked devices to the UTC timezone
      
   2. Stratum: NTP uses a hierarchy of systems to provide time sources known as stratum levels. Devices with lower stratum always have a more reliable and accurate time. Atomic clocks are stratum 0, which is the most reliable time source. Stratum 1 are devices that get       there time from stratum 0. Stratum 2 get there time from stratum 1 and so on. After stratum 15 the time source becomes too unreliable, so 15 is the limit. Additionally, stratum 1 servers are called primary servers and below that are secondary servers.
      
   3. Security: You can also create NTP security keys to ensure your time source is coming from a trusted device.

**How NTP works?**

   NTP uses a client-server model where clients requests services from an NTP server that adjust their local clock accordingly.

Commands to configure NTP on network devices

   ntp server {ntp-server-ip} [prefer]

   show ntp associations

   ntp master {stratum} - Create your own NTP server

   ntp authenticate

   ntp authentication-key {key-num} md5 {password}

   ntp trusted-key {key-num}

   ntp server {ntp-serv-ip} key {key-num}

NTP server that CoreR1 is using to get its time

![image](https://github.com/user-attachments/assets/84ada2cb-5571-4bdc-9dd1-ff22c87a38b7)

![image](https://github.com/user-attachments/assets/22bc759c-6434-49af-8a82-af0157b5497a)

Configuring Syslog services
------------------------------------------
Successfully configured a dedicate syslog server to allow networking devices to store log information. Syslog is a protocol used for logging information from varying severity levels ranging from 0 to 7. Syslog allows network administrator to collect, store and analyze log data to track events and issues.

Components of Syslog:
   1. Syslog client: Network deivces such as routers, switches generate log messages and sends them to the syslog server
      
   2. Syslog server: A syslog server is a dedicate device that will recieve and store all log information. In my network I have a dedicated syslog server that will recieve all the events such as an interface turning off, or an OSPF neighbor reaching its dead timer.

   Severity Levels
   0. Emergency - System is unusable
   1. Alert - Immediate action is required
   2. Critical - Critical condition
   3. Error - Error condition
   4. Warning - Warning condition
   5. Notice - Normal
   6. Informational - informational message
   7. Debug - Debug-level message

**How Syslog works**

   A system issue can arise on a device such as an interface going down or ospf lost neighborship. This will create a syslog message and include information such as the event, facility and severity level. This message can either be stored in the internal buffer of a    
   device or sent to a syslog server using UDP port 514. The syslog server will store this information.

**Syslog commands**

   logging {syslog-ip} - log syslog data to an external server

   logging console {level} - log syslog console events and specify serverity level

   logging buffered {size} {level} - log syslog messages in network devices buffer

   logging monitor {level} - this can send syslog messages to client using SSH or Telnet, by default SSH and telnet do not show syslog message on the VTY lines.

   service timestamps log datetime msec - include timestamps in syslog messages

   show logging - show syslog data

![image](https://github.com/user-attachments/assets/8d6d6b32-4233-4fb5-9d4b-b2fbce318cb2)

Successful file transfer using FTP protocol
----------------------------------------
FTP stands for file transfer protocol, a protocol used to transfer files from one device to another. It uses authentication methods such as username and password but has no encryption ability. I created a username and password for my device in the IT network to remotely connect to the FTP server and download the files it needs.

**Key concepts**
   1. File transfer: FTP allows you to recieve and download files between client and server
      
   2. Authentication: FTP uses username and password to validate the person logging in
      
   3. Directory management: FTP uses commands to provide directory navigation, list files and create directories.
      
   4. FTP uses two channels for communication, port 21 for sending commands and port 20 to transfer files

**Considerations when using FTP**
   FTP by itself is insecure since it does not provide any encryption mechanisms. All data send via FTP will be sent in plain-text including username and password. You can use other protocols like SCP or SFTP to encrypt your data.
   
![image](https://github.com/user-attachments/assets/b3dd433f-a35a-40b3-813a-2b1aaf201a3c)

Applying ACLs
------------------------
ACLs are a security feature in networking that filter traffic based on a set rules. This allows you to control the flow of communication between devices and deny or permit the traffic you desire. You can apply ACLs to network devices such as routers, switches and firewalls to determine if traffic should be permitted or denied based on criterias such as source and destination IP addresses, port numbers and protocols. ACLs provide a granular control over your network and can be applied to the interfaces of your device either inbound or outbound.

ACLs Explained:
   The job of ACLs is to filter traffic based on the predefined rule you apply. You can block specific IP addresses, hosts, protocols to deny access to parts of the network. ACL use ACE (Access control entries) which are rules that determine what traffic is    
   permitted or denied. ACLs will check the list of ACEs from top to bottom making the order that you create and sequence ACEs VERY important. ACLs also have an implict deny if none of the rules in the ACL list match.

Type of ACLs:
   Standard ACLs: This type of ACLs filter traffic only based off of source IP addresses
   Extended ACLs: This type of ACL filter traffic on more criteria such as source/destination IP, Layer 4 port addresses, Protocols such as TCP, UDP, ICMP etc.

Numbers vs Named ACLs:
   Numbered ACLs: This type of ACL uses numbers to identify itself.
   Named ACLs: This type of ACL uses a name rather than a number, but this type of ACL provides better management, resequencing, and flexibility. Its much easer to manage named ACLs than numbered ACLs.

Inbound vs Outbound
   When applying an ACL the direction they are applied is very important. Inbound ACL will check traffic coming into the interface. Outbound ACL will check traffic going out of the interface.

ACLs configuration

Standard ACL
   access-list {ACL-Num} {permit | demy} {source} {wild-mask}

Extended ACL
   access-list {ACL-Num} {permit | deny} {protocol} {source} {wildcard-mask} {destination} {wildcard-mask} {eq port}

Applying ACL to an interface
   interface {int-id}
   ip access-group {acl-num | acl-name} {in | out}

   
In this image I applied an ACL to the SVI on vlan 10 to prevent IT and HR from accessing each others networks. I used a more granular control to allow certain devices in IT department to access the HR department for management purposes. In the multilayer switch configuration I allowed he host 10.0.0.2 to access 10.0.1.192 network but denied the rest of the hosts in any other network. I applied the ACL inbound the VLAN 10 SVI.

![image](https://github.com/user-attachments/assets/7ac759f6-4087-4761-85d7-37e9c1d4a46b)

Configuring host files on DSW1
--------------------------------
Host files are a file on a device that store IP address to hostname mapping. They were used before DNS servers and were very common during the 90's.
![image](https://github.com/user-attachments/assets/0900ef2a-804d-4af8-922b-e45a14c2956b)

Explaining DNS
---------------------------
What is DNS? - DNS is a hierarchal naming system that translates human readable domain names to IP addresses. DNS is the phonebook of the internet and allows users to visit websites by using a human friendly name rather than a long IP address.

DNS Hierarchy - DNS is represented in a hierachal strutructure with each hierarchy being represented by a dot. For instance, take a domain name like www.google.com, .com is the TLD (Top level Domain), Google is the second level domain and www is a subdomain that part of google.com.

DNS records:
   A Record (Address Record) - Maps hostnames to IPv4
   AAAA Record - Maps hostnames to IPv6 addresses
   CNAME Record (Canonical Name Record) - Maps an alias to a domain name. 
   NS record (Name server record) - the authoritative name server for a domain
   SOA record (Start of Authority record) - Hold all administrative information of a domain
   TXT record - used for SPF and DKIM, uses text based software
   MX record - Specifies what mail server is responsible for sending and recieving mail in a domain.
   PTR record - IPv4 to hostname (Reverse process)

How does the DNS query work?
   Lets say you are accessing a website such as youtube.com and you enter this in your browser. DNS will being its query process and find the IP address for youtube.com

   1. Local DNS: The first step your computer takes is to check its local DNS cache and the host files. Your computer may have stored a DNS query already for youtube.com or has the IP address for the domain name in its host file.
   2. ISP: The second step in DNS will be talking to your ISP and seeing if there DNS server has the hostname to IP address mapping for the domain you are requesting.
   3. Root: If your ISP resolver does not have the answers, it will forward the DNS query to the Root DNS server which is responsible for TLD domains like .com, .uk, .jp, .ca. There are 13 root server clusters around the globe but in reality this consists of thousands 
      of DNS root servers.
   4. TLD: Afterwards the ISP resolver will keep querying DNS server and now will look at the TLD DNS server, which will provide a response of the authoritative DNS server for youtube.com.
   5. Finale: The authoritative DNS server will now provide the IP address you are looking for and this request will be sent from your ISP resolver to your client.

Note: This process is known as a recursive lookup since your ISP resolver is querying DNS servers on your behalf. This saves processing power especially on lower end systems. There is also iterative lookup where the client will query DNS servers by itself.

   Types of DNS query
      Forward DNS: Resolved hostname to IP
      Reverse DNS (rDNS): Resolves IP to hostname

Configuring DNS services
---------------------------------
In this image I successfully configured DNS services in my server farm and I used commands such as domain-lookup and nameserver to allow the routers and switches to contact the DNS server to provide the proper IP address to hostname mappings if they do not contain the neccessary information in there host files.
![image](https://github.com/user-attachments/assets/17969917-bb53-4b91-a4ec-2934d042bbd6)

Current Progress
---------------------

Current Progress of my network, had to make many adjustments and changes. I got rid of the gym network to keep it more simple and changed the configuration of the server farm.

![image](https://github.com/user-attachments/assets/461e6867-421e-4171-bf6f-02e80714ee57)


Next Steps
----------------------
Configuring SNMP manager 
Configuring NAT and creating an external network
Configuring Port security
Configuring DAI
Configuring a wireless network and a guest network











