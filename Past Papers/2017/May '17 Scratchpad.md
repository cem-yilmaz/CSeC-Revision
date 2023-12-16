---
tags:
  - uni
  - sem1
---
# 1. Passwords
![[Pasted image 20231213140821.png]]
## a)
![[Pasted image 20231213141036.png]]
This would be when they'd have to check all passwords before getting the desired one

The num of possible values for each of the 8 characters = 26 (lowercase a-z) + 26 (uppercase a-z) + 10 (digits 0-9, inclusive) = $26+26+10=62$

Num of possible passwords = $62^{8}$

With $k$ hashes per/second, that's $\frac{k}{3600}$ per hour

Therefore, = $\frac{62^{8}}{\frac{k}{3600}} = \frac{62^{8}\times3600}{k}$
### Marking
*1 or 2. I almost got there, but it's $k\times3600$ an hour*
## b)
Same as above, but on average

Eh I think it's $\frac{62^{8}\times1800}{k}$ because if we assume 

### Attempt 2
worst case it $\frac{62^{8}}{k\times3600}$ and that's the time for all of them
### Marking
*apparently I was right with the guess it'd be halfway down? I'd probably get maybe 2 or 3 marks again*
## c)
![[Pasted image 20231214091029.png]]
256 per hash.

It would take $\frac{256 \times 62^{8}}{8}$ bytes, or $32 \times 62^{8}$ bytes.
## d)
![[Pasted image 20231214091207.png]]
![[Pasted image 20231214091222.png]]
The chain length $\ell$ 

Rainbow tables only need the first and last password stored. The size of the table would be the $\text{size of storing the first and last password}\times\text{amount of chains}$

Representing $\text{amount of chains}$ as $x$, $x = \frac{\mathcal{P}}{\ell}$  

Taking the previous numbers in the question,
- The size of a password is *8 bytes*
- The size of storing a hash, using `SHA-256`, is *256 bytes*

Therefore, $\text{size of storing the first and last password}$ = 264 bytes

Total size of table = $\frac{264\times\mathcal{P}}{\ell}$
### Marking
*genuinely no idea if I'm correct, but I'd got with 5 marks due to proof shown*
## e)
![[Pasted image 20231214100834.png]]
It would jarble up the outputs from the reduction functions. Due to the nature of `OR`, the results are much less likely to be non-colliding. This means our chains would have to be much more carefully constructed - which in rare cases takes a lot of time to construct, or more likely just results in more chains, and massively increases the size of the table. unfortunately, if the secret is compromised, this does not mean we can't use the results from the hashes, as they're likely still valid to submit to the server

### Marking
*I'd have to guess maybe 4 or 5. Genuinely painful to jhave to just "assume" I am "right" on these "questions"*
## f)
It is best to salt our hashes on a password-by-password basis. One example would be the user's ID. We || this with the password, and since the username is provided, this can be looked up and computed upon login.

This provides much stronger protection than the existing solution with just one secret. This is still just one "set" of hashed passwords to compute. With my approach, an attacker would need to compute this set of passwords for *every* user they intend to hack, and this is in the case that they even know the user ID! We could further obfuscate this hash with session id's or more stored user data and this would add more permutations onto the hashes that would eventually be too much for many adversaries to deal with.


# 2. Analyse a warning
*some of this topic is not taught anymore. I will skip questions and bits that literally aren't relevant to me*
![[Pasted image 20231214101941.png]]
## a)
![[Pasted image 20231214102010.png]]
Confidentiality. The server does not support strong encryption algorithms. Strong algorithms are defined as those which have no known or easy methods to break. Therefore if we viewed the page we would be communicating over a weak algorithm, which could be easily broken. This breaks the confidentiality property as we cannot trust that our encrypted communication will be only read by sender and recipient.
## b)
*Not applicable*
## c)
![[Pasted image 20231214103743.png]]
It wants the user to contact the website owners to let them know of their poor cryptographic protocols supported, and that many browsers will refuse to connect to these due to their unsafe nature. It aims to get the owners to upgrade their encryption.
## d)
*Not applicable*
## e)
![[Pasted image 20231214104337.png]]
### i)
Maybe? Depends on if her VPN successfully encrypts the traffic and the server can understand it - and on what traffic is sent. There's a slim chance it'd remove her identity from the (easily cracked) health data, but it's unlikely she'd remain completely anonymous tho due to the nature of the records.
### ii)
No. All this does is prevent the local computer from storing data. All of these communications are still going out with risks to confidentiality
### iii)
No. The server is the weak point here, not the client hardware.
# 3. Web Security
![[Pasted image 20231214105526.png]]
## a)
The script only checks if the account with that username is valid. Eve could call the script `deleteaccount.php` on the `delete account` link and change the given username that gets sent to Alice's username. The script will then check to see if Alice's account is real (which it will be) and then it will be deleted. Eve does not need to interact with Alice or her browser as their is no confirmation that this request is made from the user of which the account deletion is targeted at
## b)
![[Pasted image 20231214105845.png]]
CSRF happens when a website trusts it's user. A user successfully logs into a trusted (target) site and to remain logged in through many `HTTP` requests, gets a token (in this case, the `login_cookie`). However, if not successfully defended against, this cookie can be used to make (valid, but) malicious requests.

If the user were to go onto an evil site *while remaining logged in with this cookie still active*, then the client could make an evil request and due to this cookie being active, it will be sent with it.

CSRF tokens are a method of preventing these evil requests being created. They are embedded into *trusted, genuine* pages as hidden forms, and the server checks if a request contains one of these before approving. They are designed to be difficult to forge or obtain by an adversary, and as such these (valid) requests would be difficult to create.
## c)
![[Pasted image 20231214111012.png]]
The site only checks if a valid session token exists. All Eve would need Alice to do is to enter a page that would run a script to delete her account *while she has a valid login cookie from `AcmeBook`*. This page runs the script with Alice's name as username, and since:
- her account is valid
- her login cookie exists
it will run the delete account
## d)
![[Pasted image 20231214111546.png]]
XSS attacks are when the attacker runs a script from a malicious site on a target site. There are two types of attacks:
- Stored - the script is stored somewhere on the target servers. This means it can simply be invoked
- Reflected - the attack utilises the fact that the target server "reflects" some of the user input back at the user. This means the attacker can call this page with a carefully crafted input to execute the script
## e)
![[Pasted image 20231214113031.png]]
*had to look the fact that the browser thinks the script comes from `Acmebook`*

Eve could get Alice to visit her site which invokes this script `deleteAccount.php`. The browser then thinks this script comes from `AcmeBook` and runs it with their privileges. If Eve can trick Alice into logging in, she can steal the cookie to run alongside this script, from the evil site.

We would prevent this by checking the `Referrer` field of the cookie. `AcmeBook` should have a whitelist of sites allowed to invoke it's outside scripts. It should then check the `Referrer` field of the cookie to see if the request originates from a genuine site, and if not deny the request.