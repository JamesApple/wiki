# The tech kind, not the crappy social kind.

* [Over engineered home network](https://ben.balter.com/2021/09/01/how-i-re-over-engineered-my-home-network/)

# Notes

IP:
  * Connectionless Protocol
  * Data Unit: packet
  * Addressing: Allows addressing for the hosts on a network to identify and locate other hosts
  * Routing: Provides mechanisms for routing these packets between different hosts


IPV4
First stable version
4 sections of 8 bits totalling 32
4 Billion total IP's
  Dotted decimal notation: 192.168.100.20 (0.0.0.0 -> 255.255.255.255)

IPV5
First streaming protocol. Not really used

IPV6
Newest version
128 bit address
8 sections made of 16 bits. 128 total
Simplified packet header
IPSEC mandatory
Represented by eight groups of 4 hexadecimal digits seperated by colons
  2001: 0db8:0a0b:12f0:0000:0000:0000:0001
Leading zeros may be removed when displaying the address (Zero suppression) as well as collapsing sections of only zeroes
  2001:db8:a0b:12f0::::1

IPv4 mapped IPV6 addresses
To allow IPV6 apps to communicate with IPV4 apps the IP is transformed as as
  192.168.100.20 -> ::FFFF:192.168.100.20

# Reserved IP Addresses
Private Ranges:
IPV4
  10.0.0.0 -> 10.255.255.255
  172.16.0.0 -> 172.31.255.255
  192.168.0.0 -> 192.168.255.255
IPV6
  fd00::/8

* These help to delay IPV4 address exhaustion
* Cannot be routed or used on the internet
* Used in LAN (Local Area Network)
* Allows for additional subnets within the ranges (?)

# Loopback IP Addresses
IP ranges
IPV4
  127.0.0.0 -> 127.255.255.255
IPV6
  ::1/128

  * Reserved for a hosts self address or localhost address
  * Does not utilize the NIC (Network Interface Card)
  * Uses a virtual network interface within the OS

# Link-local addresses
IPV4
  168.254.0.0 -> 168.254.255.255
IPV6
  fe80::/10

```sh
ping 127.0.0.1
ping6 ::1
```

  Used in absence of DHCP  or static addresses (?)
  Can only communicate within its own network interface
  Routers will not forward packets from a loopback address
  users ARP (Address Resolution Protocol) to ensure the desired IP is not in use
  IPV6 requires a link-local address even if a routable address has been assigned
#
# Unspecified Address
IPV4
  0.0.0.0
IPV6
  ::


Packet:
| Header | Data |

# IPV4 Address Header Format

Version: IP Prototcol version (4)
IHL: Internet Header Length (Length of header)
DSCP: Differentiated Services Codepoint (Classifies type of traffic ?)(QOS?)
ECN: E Congestion Notification
Total Length: Length of header and data in bytes
Identification: Unique identifier for packets that are fragmented
Flags: Controls packet fragmentation
Fragment Offset: Offset fragment relative to packet?
TTL: Number of routers a packet passes through (hops). Each hop decreases the count by 1. If the TTL is 0 the packet is discarded
Protocol: Data sections protocol (TCP:6,UDP:17)
Header Checksum: Checksum value of the header. Used to check for headers
Source Address:
Destination Address:
Options: Additional options such as timestamp. IHL must be > 5

IPV6 header
Version: IP Protocol Version ((6))
Traffic Class: First 6 bits(Classify packets for QOS) Last bits used for ECN (Explicit Congestion Control?)
Flow Label: Indicates which packets belong to which communication and ensures ordering
Payload: Size of Data 
Next Header: Type of extension header (TCP, UDP) If no extension header, the type of data
Hop limit: TTL, same style
Source Address:
Destination Address:

# CIDR:: Classless Inter-Domain Routing
EXAMPLES USE /24

CIDR Block: /0 -> /32
The number of bytes that are "locked" at the start of an IP to determine a block

CIDR Notation: 192.168.100.20/24
__192.168.100__.0-255
|Network   |Host|

Using /25 would allow one more bit than above, doubling the possible number of hosts

__8bit.16bit.24bit__.34bit

Network Mask 255.255.255.0
Uses binary & operation to determine block
11111111.11111111.11111111.00000000

IPV6 cidr
| Network     |snet| Host             |
2001:0db8:0a0b:12f0:0000:0000:0000:0001

2001:0db8:0a0b:12f0::1/48

# Address Resolution Protocol: OSI Layer 2
Find the hardware address of a device from its IP address

https://www.cellstream.com/reference-reading/tipsandtricks/219-what-is-the-arp-command-and-how-can-i-use-it

MAC Address
48 bits, groups of two hex digits in 6 sections
02:2f:df:d2:e6:8a

ARP Request Message sent by a host to obtain physical address of host it wants to comminucate with

ARP Response
Contains physical address of device

Proxy ARP
When one device response to a request on behalf of another device that is not on the network

`arp -a`

ARP Cache
Table of entries on a device that contains the physical MAC addresses and their corresponding IP Addresses.

| Ethernet hosts must convert a 32-bit IP address into a 48-bit Ethernet address

The first 24 bits is designated to a manufacturer (OSI)

Network Broadcast address: 255.255.255.255
ARP Request is broadcasted, 

ARP Packet Format
Hardware Type HTYPE: PRotocol type for network link (Ethernet: 1)
Protocol Type PTYPE: Network / transport layer protocol ( IPV4: 2048 )
Hardware Address Length HLEN: MAC is 6 bytes
Protocol Address Length PLEN: IPV4 is 4 octets
Operation: Senders operation 1 for request, 2 for reply
Source Hardware Address SHA: Hardware address of the sender
Source Protocol Address SPA: Protocol(IP?) address of the sender
Target Hardware Address THA: 0
Target Protocol Address TPA: Protocol IP address of the target

# NAT: Network Address Translation
Multiple devices sharing the same front facing IP address

Maps a single public(routable) IP address to one or more private(unroutable) IP addresses

Static NAT
* NAT is assigned a pool of public IP's
* Privates are assigned 1-1 to the public IP's

Dynamic NAT
* NAT is assigned a pool of public IP's
* IP's are used when needed by a host then returned back to the pool

Port Address Translation
* Single public IP is assigned to a network
* NAT records private IP addresses as well as ports
* Most common kind of NAT
* Helps conserve IP count

SNAT Source NAT
* Allows hosts on a private network to initiate connections to external hosts

DNAT Destination NAT
* Allows external hosts to connect to a host inside a network

Router
Communicates with a private network and an external network such as the internet.
Has a public IP address and private IP Address

Internet -[Public IP]-> Router -[Default Gateway]-> Any Host

# Types of Networks
PAN Personal Area Network: One or more devices connected through a single device
LAN Local Area Network: Multiple devices connected at a single site
MAN Metropolitan Area Network: Spread around a small area. May be a business or campus
WAN Wide Area Network: LAN's connect to a WAN via a router which permits the use of private IP addresses.

# OSI Model

7. Application. Deals with software apps. SMTP, HTTP, FTP
6. Presentation. Ensure the transfered data is in the correct state for the application. Typically compression
5. Session. Usually only implemented for RPC's which are normally implemented on l7
4. Transport. (TCP, UDP) Messages are broken into smaller pieces and then reassembled at the target
3. Network. (IP) Sends messages to a target destination.
2. Data Link. Node to node transfer by a link. Usually uses ethernet software with source and destination MAC addresses
1. Physical. Voltages, cables, wireless frequencies.

# TCP/IP Model

Layer 1: Network Access/Link
  Combination of OSI l1, l2
  Set of standards for communicating between nodes and physical transport
Layer 2: Internet
  OSI l3. IP
Layer 3: Transport
  OSI l4. TCP, UDP
Layer 4: Application
  OSI l5, l6, l7

Unicast
  Single source to single destination
  Supports TCP
  Servers can still have multiple clients

Broadcast
  One-to-all transmission that is connected.
  Used by ARP(Removed in IPV6)
  Always uses 255.255.255.255
  Network switches facilitate this communication type
  Routers drop broadcasts
  Removed in IPV6 in favour of multicast

Multicast Transmission
  One-to-many or many-to-many transmission model
  Requires membership in a multicast group
  Implements IGMP Internet Group Management protocol
  Data is replicated through network devices
  Primarily uses UDP
  Has an IP range of 224.0.0.0 to 239.255.255.255


Many networks use a combination of static and dynamic routing
# Static Routing
* Completely Manually configured
* Reduces resource utilization
* Provides greater security and control
Default Route: When no other route matches this route is used
Stub network: A network that is unaware of other networks and in which all non-local traffic is directed to the default path

3 Networks
192.168.1.0/24 -> Net A
192.168.2.0/24 -> Intermediary
192.168.3.0/24 -> Net B

( 192.168.1.2/24 ) \
( 192.168.1.3/24 )   ->  Switch -> (192.168.1.1/24 Router A 192.168.2.1/24) ->
( 192.168.1.4/24 ) /

CONT                                            / (192.168.3.2/24)
( 192.168.2.1/24 Router B 192.168.3.1/24 ) ----> (192.168.3.3/24)
                                                \ (192.168.3.4/24)

# Dynamic Routing
* Shares information with connected routers
* Selects best route for traffic
* Detects route changes
* Discovers remote networks
* Less administrative overhead
* Less secure
* Increases CPU and Memory usage on routers
* Increases bandwidth usage on the network

List routing tables
`cat /etc/iproute2/rt_tables`
`ip route show table [TABLE NUMBER FROM ABOVE | 255]`

`broadcast 127.0.0.0 dev lo proto kernel scope link src 127.0.0.1 ...`
`[Route type] [Destination Address] [lo(loopback)|eth0(ethernet)] [scope|proto] [link|src] [Source Address]`

Routing table: 
255 Local
254 Main Routing
```
default via 172.17.0.1 dev eth0
172.17.0.0/16 dev eth0 proto kernel scope link src 172.17.0.2
```
253 Default routing table
Removed in kernel 3.6

# Dynamic Routing
## Interior Gateway Protocol (IGP)
Transmit information between network devices (routers) in an autonomous system such as
Responsible for keeping track of the routes used on the network
Responsible for updating routers of changes

## Distance-vector routing protocols
Used to advertise routing tables to all other routers they are immediately connected to. This is recursive. RA -> RB -> RC
Keeps track of the distance (number of hops ((Belman-Ford algorithm)))  between routers

### RIPv1 Routing Information Protocol V1
Broadcasts every 30 seconds to connected routers and measures the distance in hops to a maximum of 15
Data contains full routing table
Uses UDP on 520
Uses classical routing

### RIPv2 Routing Information Protocol V2
* Supports CIDR
* VLSM
* Authentication
* Triggered Updates
* Multicast instead of broadcast
* More efficient
* Still every 30s

### IGRP Interior Gateway Routing Protocol
* More advanced than RIP
* Includes bandwidth and delay in its hop measurement
* Limited to 100 hops
* Every 90s
* Triggered by network changes
* Route posiining?
* Split Horizon?
* Classical networking

### EIGRP Enhanced Interior Gateway Routing Protocol
* CIDR support 
* Authentication
* Classless
* Multicast
* Establishes routes via hello router message
* 224.0.0.10 over RTP with acks and provide sequencing
* Sends changes when topology changes
* Expects response from 5 - 90 seconds
* If a neighbouring router cannot respond in 3x the send period it will remove that router from the network
* DUAL Diffusing Update Algorithm Removes need for periodic updates


## Link-State Routing Protocols

Routers that use this establish connections with their neighbours then send updates to the entire network about these changes. Each router stores a complete map of the network. Updates are only sent when there are changes

More efficient

### IS-IS

* Uses the djikstra algorithm to determine the shortest paths
* Not developed for IP networks and remains neutral about schemes
* Sends hello messages through multicast
* Implements "Areas" Level 1(Cannot talk to routers in level 2) To talk between routers must be assigned to be in both levels

### OSPF

* Open standard
* Djikstras SPF algorithm
* Routing information is shared using LSA's (Link State Advertisements)
* CIDR/VLSM
* Authentication
* Areas. Large number of areas. Area 0 is the backbone and all others are bordering.

## Exterior Gateway Protocol

Bridge gaps between autonomous systems

### Path Vector routing protocols

Does not determine whether or not a path is loop-free based on cost or distance but rather the path itself?

#### Border Gateway Protocol (BGP)
* Main internet routing protocol
* Requires connections between neighbours (peers)
* Must be manually configured to connect peers
* No automatic discovery
* Uses TCP over port 179
* Keepalive messages every 60 seconds between peers
* Peers updated across each other
* BGP enabled routers share their database with peers on boot
* Can update and receive updates from their peers
* RIB Routing Information Base is passed around which is used to determine the route that uses the least number of autonomous systems to pass through


# Route selection criteria
1. Prefix Length
  The more locked bits, the higher it will rank  192.168.1.1/16 > 192.168.1.1/8
2. Administrative Distance
  Arbitrary distance designated by the router to each other router depending on its protocol.
3. Metric Value
  Depends on the IGP
  RIP: Hop distance
  EIGRP: Bandwidth, delay

# IP Commands
`iproute2`
Used to monitor and manage networking on a linux host

`ip addr` (replaces `ifconfig`)
Address management

`ip link` (replaces ifconfig)
Network device configuration

`ip neigh` ARP/Neighbour tables management. Replaces `arp`

`ip rule` Routing policy database management

`ip route` Routing table mgmt Replaces (`route`)

# Common Specifications
## Types
* unicast  # Real path to a destination
* unreachable|blackhole|prohibit # Destination is unreachable. Discard or discard queitly
* local # Assigned to the host with loopback
* broadcast
* multicast # Not present in routing tables

## Dev

## Protocol
* kernel # Installed by config
* boot # Installed during boot
* static # Added by admin

## Scope
* global # Gateway unicast
* link # Direct unicast / broadcast
* host # Local

# Route testing
`ping` Sends an ICMP exho request to get an echo response
`traceroute` Track routes packets take as they travel to a destination

# ICMP
Internet control message protocol. Used to report failures
Sent whenever there is an issue with delivery
|ip header| ICMP header| ip header | first 8 bytes of data

## Header
Type: 3(destination unreachable)
Code: 1(port unreachable)
Checksum: Prevent corruption
Additional Information Based on Type:

ICMP Control Messages
* 0 Echo Reply
* 3 Destination Unreachable
* 8 Echo Request
* 5 Redirect
* 9 Router advertisement
* 10 Router solicitation
* 11 Time Exceeded

EXAMPLE

# Static IP forwarding/routing on a Linux host
Two networks: 192.168.0.0/24 and 10.0.0.0/24
Host A
  Eth0: 192.168.0.20/24
Host B
  Eth0: 192.168.0.30/24
  Eth1: 10.0.0.30/24
Host C
  Eth0: 10.0.0.20/24

Host A. Add static route
`ip route add 10.0.0.0/24 via 192.168.0.30 dev eth0`
`echo " 10.0.0.0/24 via 192.168.0.30 dev eth0" > /etc/sysconfig/network-scripts/route-eth0`

Host B. Enable port forwarding to allow eth0 to pass traffic through eth1 and visa-versa
`sysctl net.ipv4.ip_forward`
`systctl -w net.ipv4.ip_forward=1`
`echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf`

Host C. Invert Host A steps, pointing at Host B


