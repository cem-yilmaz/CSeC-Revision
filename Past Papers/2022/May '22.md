---
tags:
  - uni
  - sem1
---
[[May '22.pdf]]

# Secure Communications
## 1)
### a)
![[Pasted image 20231208111845.png]]
#### i) \[4 marks]
![[Pasted image 20231208111915.png]]
A observes 1, 1, 0
B observes 1, 1, 0
C observes 1, 1, 1
D observes 0, 0, 1

A $\oplus$ = $(1 \oplus 1) \oplus 0 = 0 \oplus 0 = 0$
B $\oplus$ = $(1 \oplus 1) \oplus 0 = 0 \oplus 0 = 0$
C $\oplus$ = $(1 \oplus 1) \oplus 1 = 0 \oplus 1 = 1$
D $\oplus$ = $(0 \oplus 0) \oplus 1 = 0 \oplus 1 = 1$

Completion indicates that a cryptographer paid (1)
Since C is the only one who announced the negation ($¬\oplus$), it was them!
### ii) \[6 marks]
![[Pasted image 20231208112602.png]]
No. This would allow for two colluding cryptographers to forge results in such a way that they could also pretend one of them paid.

*Get ChatGPT to explain a scenario where this works, I'd be fucked in a real exam*

---

To define 4DC as still "secure" is to preserve the anonymity of the sender. Since we originally have 3 secrets for each participant, in this one we have 2 for each participants. Also, if the colluding cryptographers are neighbours, they could deduce the broadcast.

To answer simply, no, it is not secure. There would exist secure scenarios - the majority of scenarios - but the fact it is not 100% possible is a major blow to security.

## b)
![[Pasted image 20231208113531.png]]
### i) \[3 marks]
![[Pasted image 20231208113542.png]]
All an adversary could see is that encrypted data - without the tagging available to associate it with a user - and what node it will travel to next. It cannot extrapolate, or for any other method, find out what path it will take to an end-point, as this information is not accessible even by the node it will go to. The route is only known by the user, and layers are taken off the message as it moves along to remove any traces of where it's been. An adversary *might* be able to know Tor traffic is taking place, but not it's source, destination, or contents - rendering censorship impossible.
### ii) \[6 marks]
![[Pasted image 20231208114308.png]]
Not securely. Complete observability means the adversary knows the endpoints of the network. An adversary can do multiple things to these endpoints to compromise Tor's anonymity or flat out stop linkage. They can perform a [[Network Security#Block (DOS)|DOS]] attack on all of the end-points to flat out prevent any linkage. Additionally, the adversary could DOS all but a few compromised end-points, and be able to control information going through this.
## c)
![[Pasted image 20231208121438.png]]
### i) \[3 marks] 
![[Pasted image 20231208121451.png]]
This would be an example of a smurf attack. By targeting the network broadcast addresses to return a huge amount of `echo` attacks, this can overload the victim's machine (if executed from their machine).
### ii) \[3 marks]
![[Pasted image 20231208121644.png]]
Like above, but we send a spoofed request for tons of pings with the "sender" address spoofed to that of the victim's computer. This enables the adversary to remotely send this smurf attack, as long as they have the victim's IP address.