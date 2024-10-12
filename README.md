# Packet-Tracer-Project

Introduction
---------------------------------
In this project, I will be testing the knowledge I have learned from my CCNA studies using JeremysIT lab course on YouTube and Implementing it into a Campus Area Network Project. I first began by creating multiple departments such as HR, IT, and Security and using Variable Length Subnetting (VLSM) to divide my address space efficiently and still support each department's number of hosts. I also took into consideration for future expansion. I used VLSM instead of classful and FLSM subnetting is because VLSM allows flexibility when assigning IP addresses to our networks. It allows us to minimize address waste and assign just enough available addresses for the hosts on the network.

Goals for this lab
----------------------------


![image](https://github.com/user-attachments/assets/cce3e3f9-3aa1-45ab-a0eb-06bf6df7fad0)

Layout and Design
---------------------------------
At this stage, I have implemented this design of the network. I have multiple buildings and departments and also implemented redundancy, 3 tier architecture and a dedicated network for servers that will provide essential services such as NTP, DHCP, and DNS to the entire CAN network. I added redundancy to the network to ensure each department can access the services they require and minimize any downtime as this could impact the business greatly. I have multiple devices running critical services such as DHCP, NTP and DNS to make sure these services do not fail as they are critical to network operation.

![image](https://github.com/user-attachments/assets/4f48c90c-2ae3-4efc-b7e1-95ffa173a4ab)

Service and Remote Access
---------------------------------
I also successfully implemented DHCP network services on each multilayer switch. This allows for extra redundancy for each subnet. I added a DHCP pool for each subnet with the appropriate DHCP exclusion range. DHCP allows devices to automatically obtain an IP address through a process known as DORA (Discover, Offer, Request, Acknowledge). This process begins with a device requesting an IP address by sending a DHCP discover packet with a MAC address of al F's and a source address of 0.0.0.0 since the device does not have an IP address. The DHCP server will respond with a unicast packet with an offer that will contain network configurations such as IP addressing, DNS, default gateway and subnet mask. The device will respond by sending a broadcast request message, asking for the DHCP server for this address. The DCHP server will acknowledge the request and the device has now successfully joined the network with new network configurations.


![image](https://github.com/user-attachments/assets/2018d196-344d-4667-acce-18bf1d17e708)
![image](https://github.com/user-attachments/assets/f85be7c4-23fd-49a9-b41c-3d70c25611c0)

Configuring Switches and Trunking
---------------------------------
Each multilayer switch has multiple VLANs configured to allow for inter-vlan routing with the usage of SVI interfaces and trunk ports. SVI provides virtual interfaces that can be used for remote management and as a default gateway. Trunk ports are interfaces that allow switches to carry multiple types of VLAN data using 802.1q tagging. I also have a management VLAN to allow for the admin department to remotely access each managed switch for configuration and administrative purposes. I also added a Misc VLAN and changed the native vlan to an Unused vlan for security purposes.

In this picture, I am remotely connecting to DSW1 for building A using Admin PC. I used a secure remote access protocol known as SSH and used the appropriate commands to generate an SSH key. I also added local login on the VTY lines, ACL, and only allowed SSH as communication. 
![image](https://github.com/user-attachments/assets/34519d4b-57f5-40b7-939c-a85fa66b8c18)

Configuring Etherchannel
--------------------------------
For this step, I used EtherChannel using the protocol PAGP which is a Cisco proprietary protocol. EtherChannel allows multiple interfaces to act as a single logical interface and avoids spanning tree protocol from disabling redundant paths. This allows my logical interface to provide more bandwidth to allow for faster connection to the distribution and core layers of the network. This also avoids any oversubscription issues.
![image](https://github.com/user-attachments/assets/ed75a70f-826f-46d6-adba-163c47eea506)

Configuring HSRP
-------------------------------
HSRP stands for hot standby routing protocol and allows for a virtual default gateway to be used which provide redundnacy on the network. I load balanced between each vlan by having my active and standby router different per vlan. I updated my DHCP pool for each VLAN as well to ensure all devices default gateway points to the virtual IP address.

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

How Rapid Spanning Tree Protocol works
------------------------------------
In this diagram, there are 4 switches, some of the links are orange, whereas others are green. RSTP (Rapid Spanning Tree Protocol) has 3 different
![image](https://github.com/user-attachments/assets/bf3b1889-60ba-4d36-a221-a05d15ce9ecc)










