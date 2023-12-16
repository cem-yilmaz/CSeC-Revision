---
tags:
  - uni
  - sem1
---
# 1. Firewalls
## a)
![[Pasted image 20231211144320.png]]
### i)
![[Pasted image 20231211144407.png]]
*had to use chatgpt to look up firewall portscan shit because i didn't do CW1*

She would see the ports `22`, `80`, and `443` - 3 ports appearing open.
Ports `22` and `80` are open to everyone, so this would be detected.
Port `443` would be allowed as *Eve* is part of the `192.168.1.(0/24)` subnet

Port `6666` will read the `DROP` instruction first and will appear closed.
### ii)
![[Pasted image 20231211145924.png]]
We would see `HTTPS` requests going to `443` and `SSL` going to port `80`?

*Wrong. `HTTP` is `80`, `HTTPS` is `443`*
### iii)
![[Pasted image 20231211150429.png]]
Packet filter? Mainly because it operates solely on a IP/port level so it has to be *transport layer*? And there's no extra context for a stateful/application layer?
### iv)
![[Pasted image 20231211150644.png]]
The issue is `443` only accepts requests from IPs in the `192.168.1.(0/24)` subnet, which means if Eve wanted to use a botnet, she would have to spoof these packets to valid. *Alice's* firewall here could likely know all the registered devices on this subnet and filter it out.

A DOS attack on port 80 might be better as it allows inputs from basically anywhere
## b)
![[Pasted image 20231211151029.png]]
### i)
![[Pasted image 20231211151040.png]]
Not *absolutely* certain. The packet sent won't be able to be linked to Bob, but if TOR encryption is broken *before* reaching the entry node, it could be read and linked to his IP. Additionally, even without breaking encryption, if the output from the exit node contains a username and password, this will render the previous effort useless - although it's not certain they could link that account to Bob.
### ii)
![[Pasted image 20231211151433.png]]
*Bob's* individual data, yes. They have no means to discover which sites Bob intends to visit. They only may be able to know *some* users are accessing sites *through the TOR network*, which is far from valuable

## c)
![[Pasted image 20231211151549.png]]
### i)
![[Pasted image 20231211151600.png]]
By choosing a more secure password, and enabling 2 factor authentication just in case this password was ever discovered
### ii)
![[Pasted image 20231211151642.png]]
*"Something you know"*?

**we didn't cover this in our course this year the paper's from 2019 ffs**
### iii)
![[Pasted image 20231211151726.png]]
The admin is on about two-factor authentication here. This involves another physical device - usually a mobile phone - having an app installed on it. The user successfully logs into the app and it's address is listed in the database. Whenever a successful login attempt occurs, the app is "pinged" and only once the user has authorised it on app does the login actually occur. Randomised tokens and such procedures must be taken to ensure this second authentication cannot be forged.
### iv)
![[Pasted image 20231211152409.png]]
Confidentiality - the personal customer data should have only been accessed by that customer, or the admin. Not Eve, like it was.
# 3. Web Security
## a)
![[Pasted image 20231211152504.png]]
### i)
![[Pasted image 20231211152514.png]]
This code is subject to `SQL` Injection. This is where, since the query is simply substituted with the provided `$UID$`, we can spoof this to be `SQL` code.
To execute this, we use two bits of syntax from `SQL`:
- Multiple different queries can be separated with `;` e.g. `SELECT ...; INSERT ...`
- We can comment out code with `---`
If we somehow submitted the `$UID$` to be `0; DROP-TABLE *`, this would execute the search and also delete all tables in the database
### ii) 
![[Pasted image 20231211152757.png]]
Blacklisting works by denying access to any resource originating from a (black)list. This means any known malicious sources cannot do any harm (or anything at all) to the server. The difficulties with blacklisting surround keeping it up to date from new threats, which are constantly developing. A blacklist is only effective while it is being monitored and updated - if left to rot, it will eventually cease to provide security.

### iii)
![[Pasted image 20231211153057.png]]
More than likely input validation. In a rough sense, this could be to strip any offending characters (like the `;` and `---` mentioned before) from the input, but since we're dealing with a user id, a more strict "has to be an integer between `x` and `y`" rule might be more applicable. We pass this input to a validator before even attempting to insert it into a query and running that query, to ensure dangerous queries are not even constructed.
## b)
![[Pasted image 20231211153257.png]]
No. `Secure` only ensures the cookie is not sent unencrypted. CSRF often relies on trust on websites of their users to not send malicious requests. The requests sent with CSRF to `https` sites are themselves `https` and such send the cookie - the issue comes with the content of the request itself.
## c)
![[Pasted image 20231211153419.png]]
No. If both the victim and malicious site are `HTTPS`, the attack continues as normal. The problem is with where the request comes from, not the type of encryption used
## d)
![[Pasted image 20231211153528.png]]
No. It defends against *most*, but some websites could be so poorly configured that the information from this cookie could be exploited through a specific link and still obtained, and an XSS attack could still continue.

*This would only prevent attacks that use scripting to read the cookie*.
## e)
![[Pasted image 20231211153723.png]]
Not really. CSRF depends on the request and uses the existing browser cookies to make these malicious, but accepted requests. Since this cookie already exists, whatever mechanism a `HTTP Post` application has to process the request will still process these malicious requests
## f)
![[Pasted image 20231211153857.png]]
Not all XSS attacks. This is because the script for an XSS attack can come from it's origin, in the form of a stored xss attack. This means that the cookie stealing can come from within, and the recipient domain can be configured in such a way that can happily interpret the cookie
## g)
![[Pasted image 20231211155531.png]]
*not a scooby. also hungry. chatgpt'ing this one, then going home*

only if part of same origin i think. otherwise, sop would stop them going across, regardless