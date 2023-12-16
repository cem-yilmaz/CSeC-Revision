---
tags:
  - uni
  - sem1
  - flashcards/CompSec/NetworkSecurity
---
# Security Definitions
Confidentiality is defined by giving access to information to ==authorised entities==
<!--SR:!2023-12-19,4,300-->

Integrity is defined as the ==trust== the data you want to access hasn't ==been tampered with==
<!--SR:!2023-12-16,2,220!2023-12-19,4,300-->

Availability is defined as, when you want to access data, ==the data and the system to access it are both available when you need it==
<!--SR:!2023-12-18,4,280-->

What is a threat model?::A description of the threat we aim to protect a system against.
<!--SR:!2023-12-19,4,293-->

Reliability/Resilience is the knowledge that if a system has an attack attempted, it **won't** ==become insecure== and **will** ==remain intact==
<!--SR:!2023-12-17,2,253!2023-12-19,4,300-->

What is the difference between *trust* and *trustworthy*?::*Trust* is an assumption, *trustworthy* is if you check this assumption and the results come back in favour of it.
<!--SR:!2023-12-19,4,300-->
# Packet Switching
What is a packet?::A collection of bits, made from breaking down messages or commands into smaller parts?
<!--SR:!2023-12-19,4,280-->

How do packets travel through a network, in relation to other packets?::Each packet is handled *independently* and on a *best efforts basis*; multiple packets of the same message/command may be transmitted in different ways (e.g. different routes)
<!--SR:!2023-12-19,4,300-->

How do LANs solve the problem of packet collisions?::By employing a *random-wait-strategy*, where each machine waits a *random* amount of time before re-attempting.
<!--SR:!2023-12-19,4,290-->

How do hubs solve the problem of packet collisions?::Hubs broadcast all frames sent to it to every connected device, so each device receives all frames of data even if not intended for them.
<!--SR:!2023-12-19,4,300-->

How do switches solve the problem of packet collisions?::Switches broadcast all frames sent to it to every connected device, but slowly learn the addresses of each machine over time. After this, it will send only the relevant frames to each machine, which reduces traffic.
<!--SR:!2023-12-19,4,280-->
# Layer Stack
Why do we use a stack of layers to process network communications?::This helps us distribute problems between layer implementations.
<!--SR:!2023-12-19,4,300-->

How does the layer stack know which layer a packet is from?::Packets have [[Internet Packet Encapsulation.png|headers]] that get pushed/popped when transferring layers, so the stack simply just has to read this header.
<!--SR:!2023-12-19,4,300-->
## Physical Layer
What does the physical layer send and receive?::The actual bits of communication.
<!--SR:!2023-12-19,4,300-->

How does the physical layer communicate?::Sends it through the physical medium (copper wires, fibre-optic cables, etc)
<!--SR:!2023-12-19,4,300-->
## Link Layer
What does the link layer send and receive?::Ordered records of bits (called *frames*)
<!--SR:!2023-12-19,4,280-->

What are the ordered records of bits the link layer sends called?::*Frames*
<!--SR:!2023-12-19,4,300-->

Aside from sending groups of bits, the link layer also ==detects errors== in these groups
<!--SR:!2023-12-19,4,275-->

How does the link layer communicate?::The link layer uses [[Network Security#MAC Addresses|MAC Addresses]] to communicate with the right computer
<!--SR:!2023-12-19,4,300-->
## Network Layer
What does the network layer (or *internet layer*) send and receive?::Packets of data appended with it's header providing the methods of sending/receiving this data.
<!--SR:!2023-12-19,4,280-->

How does the network layer communicate?::Through [[Network Security#IP Addresses|IP Addresses]].
<!--SR:!2023-12-19,4,300-->

Why would the network layer be unreliable to use on it's own?::If two applications on the same machine want to communicate, the returning packets will both have the same [[Network Security#IP Addresses|IP Address]] and the packets can easily get confused. This problem is solved in the higher level [[Network Security#Transport Layer|transport layer]] through the use of [[Network Security#Ports|ports]].
<!--SR:!2023-12-19,4,272-->
## Transport Layer
What does the transport layer send and receive?::Packets of data, appended with it's own header providing more detailed instructions on where to send/receive data.
<!--SR:!2023-12-19,4,280-->

How does the transport layer communicate?::[[Network Security#IP Addresses|IP]], like the [[Network Security#Network Layer|network layer]], but by also using [[Network Security#Ports|ports]] multiple applications on the same machine can communicate simultaneously, and the transport layer mandates the use of these.
<!--SR:!2023-12-16,2,220-->

What are the two protocols the transport layer can use?::[[Network Security#TCP|TCP]] - reliable, ordered, but slow - or [[Network Security#UDP|UDP]] - less reliable, not ordered at all, but lightning fast.
<!--SR:!2023-12-19,4,300-->
## Application Layer
What does the transport layer send and receive?::Data packets, in their simplest form
<!--SR:!2023-12-18,4,273-->

How does the application layer communicate?::Using protocols defined in the [[Network Security#Application Layer|application layer]] ([[Network Security#TCP|TCP]], [[Network Security#UDP|UDP]])
<!--SR:!2023-12-19,4,300-->

What types of application layer protocols would use [[Network Security#TCP|TCP]]?::Applications that prioritise *ordered*, *reliable* packets (`HTTP`, `SSL`, mail protocols like `SMTP`/`IMAP`)
<!--SR:!2023-12-19,4,300-->

What types of application layer protocols would use [[Network Security#UDP|UDP]]?::Applications that priorities speedy connections (`VoIP`, `DNS`)
<!--SR:!2023-12-19,4,293-->
# IP Addresses
Why do we use IP Addresses instead of, for example, [[Network Security#MAC Addresses|MAC addresses]]?::IP addresses are often required to communicate across networks, as each machine - independent of network - has a unique IP address.
<!--SR:!2023-12-17,2,260-->

What does an IP packet contain?
?
Source address
Destination address
Packet length (in `kb`)
Time-to-live (amount of bounces it can do around a network)
IP protocol version
Fragmentation version
[[Network Security#Transport Layer|Transport Layer]] protocol info (e.g [[Network Security#TCP|TCP]])
<!--SR:!2023-12-16,1,180-->

What layer does routing with IP addresses?::[[Network Security#Network Layer|The network layer]].
<!--SR:!2023-12-19,4,300-->
# MAC Addresses
What layer does routing with MAC addresses?::[[Network Security#Link Layer|The link layer]]
<!--SR:!2023-12-18,4,273-->

MAC Addresses are a 48-bit (or 6 *octet*) number. The first {3 octets} are {assigned by the IEEE}, and the {remaining 3 octets} are {up to the manufacturer to assign and avoid repeating}..
<!--SR:!2023-12-15,4,280!2023-12-15,4,280!2023-12-15,4,280!2023-12-15,4,280-->

We can "translate" IP addresses into MAC by using ==[[Network Security#Address Resolution Protocol|ARP]]==, although this is only available when on the ==local network== and not over long range communication.
<!--SR:!2023-12-17,2,260!2023-12-19,4,280-->

# Ports

We reserve ports ==`0` - `1023`== for known protocols, and the "user ports" to listen for connections are ports ==`1024` - `49151`==
<!--SR:!2023-12-19,4,298!2023-12-19,4,298-->

# TCP

What is the process of the three-way TCP handshake?
?
1. The client sends a `SYN` packet to request a connection.
2. The server responds by sending a `SYN/ACK` packet which acknowledges the connection
3. The client responds by sending an `ACK` back to the server and connection is established
<!--SR:!2023-12-19,4,280-->

How do servers configured for TCP maintain connections?::With a *state table* that stores the state of each current connection
<!--SR:!2023-12-19,4,293-->

## `SYN` Flooding
`SYN` Flooding is the process of sending thousands of `SYN` requests to a victim. What two aspects of TCP does this target?
?
- The state table - this will be filled with all these dummy connections that will never send an `ACK` back and prevent new connections from being formed
- The response of a `SYN/ACK` packet - this eats up the victim's bandwidth
<!--SR:!2023-12-19,4,300-->

## Forged TCP packets
What is the difference between a [[Network Security#`SYN` Flooding|SYN flood]] attack and a forged TCP packet attack?::In the forged packet attack, the TCP header is forged with a different source, which will get flooded with the corresponding `SYN/ACK` packets.
<!--SR:!2023-12-19,4,273-->

## Smurfing
What protocol (and what aspect of that protocol) does a smurf attack exploit?::The Internet Control Message Protocol (`ICMP`), particularly the feature where hosts respond to `echo` packets to let the sender know they are online.
<!--SR:!2023-12-17,2,253-->

Smurf attacks contain a ==forged IP address (of the victim)== in the header.
<!--SR:!2023-12-19,4,300-->

How do smurf attacks exploit (poorly configured) networks?::Some LANs are configured that when pinged with a broadcast address, all hosts on the LAN will reply to the ping's sender.
<!--SR:!2023-12-19,4,300-->

## TCP Sequence Prediction
In a sequence prediction attack, the attacker launches a [[Network Security#Block (DOS)|denial of service]] attack on the victim. Why is this?::The attacker is spoofing the victim's IP address, and so wants to stop the victim receiving the `SYN/ACK` packet, so they can send their own `ACK` first which establishes a forged `TCP` connection with the attacker.
<!--SR:!2023-12-19,4,280-->

How can *sequence numbers* solve a TCP Sequence Prediction attack?::By having each packet reference a previous *sequence number*, we make it harder to spoof packets (such as the final `ACK` in the attack) by having this number *difficult to obtain/guess*.
<!--SR:!2023-12-19,4,280-->
# Address Resolution Protocol
What weaknesses do [[Network Security#Creation (Spoofing)|spoofing]] attacks on ARP exploit?::ARP stores it's MAC<->IP results in a table, which is updated upon every address. ARP Cache Poisoning attacks spoof a fake MAC-IP pair which gets stored in this table, and all sent messages for that IP will go to the attacker's machine (unless the client somehow updates this)
<!--SR:!2023-12-19,4,300-->
# Domain Name System
What layer does DNS operate at?::[[Network Security#Application Layer|Application Layer]]
<!--SR:!2023-12-19,4,280-->

What are the three types of DNS records that are stored?
?
- Address (**A**) record: IP address associated with host name
- Mail exchange (**MX**): mail server of a domain
- Name server (**NS**) record: authoritative server for a domain
<!--SR:!2023-12-19,4,300-->

We use a "tree" structure to store DNS records, using different servers, because it ==reduces the load== and allows for fast ==resolving== techniques
<!--SR:!2023-12-18,4,280!2023-12-19,4,300-->

How does an *iterative name resolver* work?::If a record is not found on a name server, the client will be *referred* to another name server (through the use of a [[Network Security#^1ed140|NS record]]) and will keep making these requests until they receive the [[Network Security#IP Addresses|IP]].
<!--SR:!2023-12-19,4,300-->

How does a *recursive name resolver* work?::If a record is not found on a name server, it will make *it's own request* to a referred name server, and this "chain" will continue until an [[Network Security#IP Addresses|IP]] is found - this then gets passed back down the chain.
<!--SR:!2023-12-19,4,300-->

What vulnerabilities in a DNS cache does a (regular) poisoning attack exploit?::DNS uses [[Network Security#UDP|UDP]] and [[Network Security#Ports|port]] `53` (**known communication standard**) and answers are matched with an **unauthenticated** request identifier and is simply **incremented by 1** each time a request is made.
<!--SR:!2023-12-19,4,280-->

How does a *subdomain* DNS cache poisoning attack differ from a regular one?::The attacker causes the victim to send many DNS requests for a non-existent subdomain of the actual domain, and then the attacker sends back fake NS records which contain forged info for the *actual domain*, which the victim then caches.
<!--SR:!2023-12-19,4,300-->

How does DNSSEC aim to solve security issues?::Through [[Secure Communications#Digital Signatures|signing]] and [[Cryptography#Public-Key Cryptography|public key cryptography]].
<!--SR:!2023-12-19,4,293-->
# Firewalls

Firewalls work by applying a ==set of rules==, called ==policies==, to incoming traffic. It will use either a ==blocklist== or ==allowlist==.
<!--SR:!2023-12-19,4,300!2023-12-19,4,300!2023-12-19,4,300!2023-12-19,4,300-->

## Packet Filters
A packet filter firewall will ==apply policies on a packet-by-packet basis==.
<!--SR:!2023-12-19,4,300-->

How do packet filters remember malicious senders?::Trick question! They don't. They are *stateless* and only deal with one packet at a time.
<!--SR:!2023-12-19,4,293-->

What layer do packet filters operate at?::[[Network Security#Network Layer|The Network Layer]]
<!--SR:!2023-12-19,4,280-->
## Stateful Filters
How do stateful filters remember malicious senders?::They keep tables containing records about *every active connection* that is passing through it.
<!--SR:!2023-12-19,4,280-->

What layer do stateful filters operate at?::[[Network Security#Network Layer|The Network Layer]]
<!--SR:!2023-12-19,4,280-->

Thanks to their *state tables*, stateful firewalls can only allow inbound TCP packets in response to a connection ==initiated from within the internal network==.
## Application Layer firewalls
Why do we want a firewall that operates at the [[Network Security#Application Layer|application layer]]?::These firewalls can "understand" applications, and provide more fine tuned policies through this additional context.
<!--SR:!2023-12-19,4,300-->

What is the risk of application layer firewalls?::They are incredibly intrusive and rely in [[Network Security#Trust|trust]] with the admin who runs them
<!--SR:!2023-12-19,4,300-->
# Network Address Translation
NAT takes a ==public IP address (for internet traffic)== and maps it to a ==private IP address (for LAN traffic)==.
<!--SR:!2023-12-19,4,300!2023-12-17,3,240-->

NAT solves the exhaustion of `IPv4` addresses, but we would much rather use ==`IPv6`== instead.
<!--SR:!2023-12-19,4,300-->

Why must we ensure ports are different for each machine on a LAN?::NAT sends messages through ports to determine the machine that receives the message.
<!--SR:!2023-12-16,2,240-->

# Intrusion Detection Systems
## Rule-Based IDS
What is the basis for *rule-based* IDS?::They send an alert when a known attack pattern (a *signature*) is detected.
<!--SR:!2023-12-19,4,280-->

Rule-Based IDS has ==high== accuracy and ==low== false positives, but is set back by ==requiring the signature set to be constantly updated against new attacks==.
## Statistical IDS
How do statistical IDS work?::They build up a statistical model of "normal" behaviour and then alert anything outside of this model. This means they can detect previously unknown attacks.
<!--SR:!2023-12-19,4,300-->

Statistical has ==low== accuracy and ==high== false positives (compared to [[Network Security#Rule-Based IDS|rule-based IDS]])
<!--SR:!2023-12-19,4,300!2023-12-19,4,300-->