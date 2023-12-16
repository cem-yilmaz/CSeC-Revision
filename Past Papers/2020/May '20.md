---
tags:
  - uni
  - sem1
---
# 1. Networking
## a) Link and network layer
### i) \[1 mark]
![[Pasted image 20231209140122.png]]
B)?

A) - *no*. because not all communication is encrypted
D) - *no*, because not all communication is signed
C) - *no*, it contains more information such as sender's IP
### ii) \[2 marks]
![[Pasted image 20231209140433.png]]
C) - **this is the TTL field**

A) - it doesn't know this
B) - not stored
D) - this is another layer's job
### iii) \[3 marks]
![[Pasted image 20231209141407.png]]
*had to look up [[Network Security#Address Resolution Protocol|ARP]] for this FFS*

Eve sees when *Alice* wants to send *Bob* a message, spoofs *Alice's* IP, and inserts her own MAC address into the message. *Bob's* ARP cache will update and replace the MAC address with *Eve's*. The same will happen when *Bob* tries to send a message to *Alice*, so her ARP cache gets poisoned.
## b) TCP uses a three way handshake to initialise connections
### i) \[3 marks]
![[Pasted image 20231209142503.png]]
My attempt, before looking up [[Network Security#TCP|TCP]] 

Sequence numbers are used to prevent packet forgery with respect to replay attacks.

The handshake goes as follows:
1. client sends server a `SYN` packet
2. server sends back a `SYN/ACK` packet
3. client sends back a `ACK` packet
and then communication can proceed

An adversary could intercept this `SYN/ACK` packet and tamper with it to make sure the client unknowingly sends back an `ACK` packet to them, and ruin the TCP trust.
The use of sequence numbers is so these packets cannot be forged, as they are made difficult to guess.

*Addition after lookup*

This attack can be done in inverse, with the client sending the `ACK` to forge being the legit client
### ii) \[2 marks]
![[Pasted image 20231209143510.png]]
This will use up the bandwidth as the server will have to process all of theses requests and try to send out `SYN/ACK` packets. This is a form of DoS attack as it uses up a ton of resource.

*After lookup*
[[Network Security#Availability|It was availability]]

### iii) \[3 marks]
![[Pasted image 20231209143922.png]]
Not fully hardened. It means that the server won't send out the `SYN/ACK` requests, as the sequence nums wont match, but it will still have to process each `SYN` request and this can still likely result in a DoS.

This does protect against forged packets however, such as attacks like [[Network Security#TCP Sequence Prediction|TCP sequence prediction]], when the aim is to establish a connection bypassing authentication.
## c) TLS and Tunnelling
### i)
![[Pasted image 20231209144307.png]]
Forward security is the process of making it so future breaks in security don't affect past messages.
For example, even if TLS messages are stored and eventually one is cracked, because of the message-by-message method of encryption, this info can't be used to retroactively decrypt the old communication
### ii) 
![[Pasted image 20231209144857.png]]
This is to ensure backwards compatibility. Sometimes there exist certain machines that cannot upgrade to more recent protocols, for implementation-based, or physical reasons. This ensures the internet can still be accessed by everybody, even very critical (but old) machines.

This does bring about some security issues. Newer versions are created to prevent exploits with the older, imperfect ones, and by providing the "option" to avoid these opens up these communications to these exploits. 

While not strictly related to TCP, the WannaCry attack on the NHS in 2017(or 2018??) draws parallels. This was because the NHS majorly used Windows XP - which was deprecated at that time - but due to the NHS's significance, Microsoft had to develop a patch because it was clearly too widespread despite deprecation.
# TOR
## a)
![[Pasted image 20231210125110.png]]
3? 

- Exit node
- entry
- relay

Since each TOR relay doesn't know any information other than where to send the next packet, the censor could not realistically determine the HTTP website you are sending the packet to at the end of the chain. Due to these 3 layers of obfuscation, there is no way for any eavesdropper to know that a packet leaving the exit node is for yours.

## b)
![[Pasted image 20231210132107.png]]
*not a scooby*

*3 should still be fine. The statement is importantly "where **your** traffic is being **routed to**". If we still have an entry, relay, and exit node:*
- entry - knows the IP address, and that's it. no information about destination
- relay - might know the exit node. but not the ip
- exit - knows the destination address - but not who requested to it
## c)
![[Pasted image 20231210133956.png]]
3 should still be fine. As mentioned above, each individual relay doesn't know both the address and destination. Since they're independent, this is a similar response to above's situation
## d)
![[Pasted image 20231210134058.png]]
We can't guarantee this anymore unfortunately. By controlling all of the entry and exit nodes, we can monitor all traffic entering and exiting the network. These packets can not only be tagged, but also timed to determine which packet leaving an exit node came from which entry node

If we know that there exists one uncompromised entry/exit, this is fine. But if we don't know this, the guarantee is gone.
## e)
![[Pasted image 20231210141341.png]]
