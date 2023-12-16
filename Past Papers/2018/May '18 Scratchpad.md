---
tags:
  - uni
  - sem1
---
# 1. Network Security
## a)
![[Pasted image 20231212163431.png]]
### i)
![[Pasted image 20231212163859.png]]
She would only see two ports `22` and `80` as open. 

Her firewall is accepting traffic to port `22`, and so it will be open.
Her firewall is accepting traffic to port `80`, and so it will be open.
Her firewall is only accepting traffic to port `443` if it comes from the IP `192.168.1.102`; this is not Eve's, and as such it will not be open.
Despite the later `ACCEPT` instruction, the firewall will read the first `DROP` instruction for port `4444`, and as such it will not be open.
### ii)
![[Pasted image 20231212165301.png]]
We would likely see quite a bit of traffic entering ports `80` and probably `443`. This is because `80` is reserved for `HTTP` requests, and by accessing the Computer Security Class website we will be making `HTTP` requests to this server and will see traffic as a result.
`443` is for `HTTPS` traffic. This will only see traffic if two requirements are met, although I do deem this reasonable:
- If traffic comes from address `192.168.1.102` - although it would make sense if this were the Computer Security website, so it will probably be allowed
- If the traffic is through the encrypted `HTTPS` standard - although for a class website, I'd hope it would be, especially a class on Computer Security
So we'd see some traffic in `HTTP` (`80`), and if the two requirements met, some traffic in `HTTPS` (`443`)

### Marking
*2 marks. Need to mention [[Network Security#TCP Handshakes|TCP Handshake]]*
### iii)
![[Pasted image 20231212171412.png]]
Packet filter. This is because the firewall stores no states of active connections (stateful filter) nor uses application context to determine the request at the application layer. It simply checks packets on an individual basis, and assesses the address and port to allow or deny access
### iv)
Yes, as Alice has not set up any protection against messages into these ports. Eve could get a botnet to send many packets to port `80` of which the firewall does not check the origin of, and this would eat up at Alice's bandwidth (to send back the `SYN/ACK` packets. Additionally, it would fill up the state table of Alice's website's connections with these dummy connections, and would stop other users from connecting to her website, effectively taking it offline.
### v)
![[Pasted image 20231212175736.png]]
*Eve's* attack could be thwarted by understanding these requests are faulty and, by appending the firewall with a whitelist, which would mean *Eve* would waste bandwidth trying to make things harder for *Alice*. A simple DOS attack, or more likely a DDoS attack, would be better as this traffic would be much harder to defend against.
## b)
![[Pasted image 20231212180934.png]]
Yes. There are 3 things that signify the email's legitimacy.
1. It is from an address `@twitter.com`. While this can be spoofed, not only do most modern clients detect this but the other two reasons make up for this defect
2. It is signed by twitter.com. This means that the email has definitely come from Twitter.
3. The link is to `twitter.com`. It is not a spoof link. It is a real link to twitter.com
## c)
![[Pasted image 20231212182231.png]]
### i)
![[Pasted image 20231212182247.png]]
*not covered in the course*
### ii)
![[Pasted image 20231212182313.png]]
*not covered in course*, but it's probably an offline guessing attack. Since the password will be much more secure, it won't be in any manageably sized dictionary and make an attack so much harder, that it would be effectively protected against.
### iii)
![[Pasted image 20231212183311.png]]
2FA - using a seperate mobile device. Answered this in a previous past paper and I'm happy with my answer there.
# 3. Memory Management
![[Pasted image 20231212183603.png]]
## a)
![[Pasted image 20231212183616.png]]
This code is vulnerable because `gets` does *not* check the size of the buffer it is intending to load into. This means, if the input is longer than `[4]`, `gets` will start writing into the stack into other addresses. The stack frame look like
- the `%eip` is pointing to the call to the `gets` function
- the `%esp` is pointing to the base of the stack
- The `%ebp` is pointing to the return address of `vuln`
- We also have the 4 bytes allocated for `buffer`
- Aswell as the functions `foo` and `bar`'s addresses
## b)
![[Pasted image 20231212190008.png]]
We need to overwrite the address of `foo` to that of `bar`. If we know the address of `foo` is located `x` bytes away in memory, then we can provide a string that is `x+4` bytes long, appended with `bar`'s address (`0x00000462`). When `gets` reads this string, it will fill the `buffer` with the 4 bytes, and then keep overriding until it reaches the address of `foo`, and then fills this with `bar`'s address. When the if statement calls `foo`, the `eip` will jump to this address location, and will then jump to `bar`'s instead, transferring control of the program over to `bar`. 
## c)
Stack canaries.

We insert known (but randomly generated at runtime, to avoid replay attacks) values into the stack - between `buffer` and the addresses of `foo` & `bar`.

By checking these after `gets`, we can see if stack smashing has occured. If it has, we immediately terminate the program to prevent the payload from harming the computer

This thwarts this attack as, since we do not know the details of the canaries, we cannot maintain them when overflowing the stack. This means the program will terminate after `gets`, and has completely thwarted this attack
## d)
![[Pasted image 20231212203334.png]]
![[Pasted image 20231212203344.png]]
![[Pasted image 20231212203834.png]]
We supply malicious code as `input` to `vuln` which contains a `NOP` sled followed by arbitrary bytecode. 

We would need to know the location of the `%eip` so instead of it being loaded with the address of the `it` instruction, it points somewhere in our `NOP` sled. It will then slide down the sled and eventually go to our bad instructions, which it will then execute.
## e)
![[Pasted image 20231212213527.png]]
We should place the stack canary directly after the `buffer` to ensure an overflow does not occur. If it overflows to any capacity, the canary will be overwritten, and this will be detected. We should check this immediately after the `strcpy` command, to prevent any unwanted payloads from being "executed". This way, if we were to perform the attack, it would override the stack, and then it would immediately be detected as doing so - and program execution would stop.