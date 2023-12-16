---
tags:
  - uni
  - sem1
  - flashcards/CompSec/SecureCommunications
---
# Nonces

In secure communications, what is a *nonce*?::A randomly generated number used to *salt* messages.
<!--SR:!2023-12-19,4,290-->

What types of attacks does a nonce prevent?::Replay attacks, when an adversary can use previous similar messages to crack [[Cryptography#Cryptographic Hash Functions|hash functions]].
<!--SR:!2023-12-19,4,290-->
# Online TTP's

What is the goal of an online TTP?::To establish a shared secret key for communication, by only using the secret keys each individual party (and the TTP) knows
<!--SR:!2023-12-18,4,270-->
## Yahalom Protocol

In the first step of the protocol, what does *Alice* send to *Bob*?
?
- Her identifier ($A$)
- Her nonce ($N_{A}$)
<!--SR:!2023-12-19,4,290-->

In the second step of the protocol, what does *Bob* send to *the TTP*?
?
- His identifier ($B$)
- His nonce ($N_{B}$) generated upon receiving *Alice's* previous message
- *Alice's previous message* ($A, N_{A}$) encrypted under *Bob's* secret key $K_{BS}$
<!--SR:!2023-12-18,4,270-->

In the third step of the protocol, what does *the TTP* send to *Alice*?
?
- *Bob's* nonce ($N_{B}$)
- ($B, k_{AB,}, N_{A}$) encrypted under *Alice's* secret key $k_{AS}$
- ($A, B, k_{AB}, N_{B}$) encrypted under *Bob's* secret key $k_{BS}$ (to send to *Bob*)
<!--SR:!2023-12-18,4,250-->

In the fourth step of the protocol, what does *Alice* send to *Bob*?
?
- $(A, B, k_{AB}, N_{B})$ encrypted under *Bob's* secret key $k_{BS}$
- *Bob's* nonce ($N_{B}$) encrypted under the shared secret key $k_{AB}$
	- Bob can get this key from the other part of the message
<!--SR:!2023-12-19,4,270-->

What are the negatives of using a TTP?
?
We need to [[Network Security#Trust|trust]] the TTP to:
- generate secure, un-guessable keys
- keep these keys secure and only send them to their owners
<!--SR:!2023-12-19,4,290-->

# Public Key Encryption

Public key is different to *shared key encryption* as it uses both ==public and private keys==
<!--SR:!2023-12-19,4,290-->

How do we establish a shared key using *public key encryption*?::A shared key is generated between 2 parties from intense calculations involving their public and private keys
<!--SR:!2023-12-18,4,250-->

## Number Theory

What do we mean by $\phi(n)$?::The number of elements (in a set, usually $\mathbb(Z)$) that have no common factors with $n$.
<!--SR:!2023-12-18,4,270-->

$\mathbb(Z)_{n}$ is defined as ==${0, \dots, n-1}$== for $n \in \mathbb(N)$
<!--SR:!2023-12-17,2,250-->

We can write $a = b + k \cdot n$ as ==$a \equiv b(\text{mod } n)$==
<!--SR:!2023-12-19,4,290-->

$\mathbb(Z)^{*}_{n}$ is the set of every element $x$ in $\mathbb(Z)_{n}$ where ==the $gcd(x, n)$ is 1==
<!--SR:!2023-12-18,4,270-->

$|\mathbb{Z}^{*}_{n}|$ = ==$\phi(n)$==
<!--SR:!2023-12-18,4,270--> 

# Intractable Problems
What is an intractable problem?::Problems of which there is no known polynomial time algorithm to solve them
<!--SR:!2023-12-19,4,290-->

What are the four intractable problems covered in this course?
?
- [[Secure Communications#Factoring|Factoring]]
- [[Secure Communications#RSAP|RSAP]]
- [[Secure Communications#Discrete Log|Discrete Log]]
- [[Secure Communications#Diffie-Hellman Protocol|Diffie-Hellman]]
<!--SR:!2023-12-19,4,290-->

# Diffie-Hellman

What are the *private keys* used in DHP?::The private keys are just normal $a$ and $b$.
<!--SR:!2023-12-19,4,290-->

What are the *public keys* used in DHP?::The public keys used are $g^{a}$ and $g^{b}$, where $a$ and $b$ are the respective private keys.
<!--SR:!2023-12-19,4,290-->

What is the *shared private key* used in DHP?::We fix a (publicly accessible) large prime $p$, and the shared private key is $g^{ab}(\text{mod }p)$.
<!--SR:!2023-12-18,4,270-->

The beauty of DHP is that the shared private key is ==never sent over messages and is computed at runtime for each message.==
<!--SR:!2023-12-19,4,290-->

How can an attacker perform a MiTM attack on DHP?
?
If the attacker can intercept communications and make the other side believe their accomplice is using a *(forged) private key*, then there are *two shared public keys* in use. The attacker knows both of these, and can create them by knowing both *forged private keys* and decrypt incoming messages/forge outgoing ones.
<!--SR:!2023-12-19,4,290-->

# RSA Trapdoor permutation

Why do we need to add a further layer to "raw RSA"?::RSA is deterministic; it is not secure against [[Cryptography#*Chosen-plaintext attack*|chosen plaintext attacks]], and if we have an idea on what format the message is, we can tweak a few numbers to analyse the outputs and break encryption
<!--SR:!2023-12-17,2,250-->

# ElGamal
# Message Authentication Codes

What security principle to MACs aim to protect?::[[Network Security#Integrity|Integrity]]
<!--SR:!2023-12-18,4,250-->

A MAC is a pair of algorithms ==$S, V$ (sign, verify)==
<!--SR:!2023-12-19,4,290-->

As long as the key is not known to the attacker, messages using MACs are ==unforgeable==
<!--SR:!2023-12-19,4,270-->

ECBC-MAC is like CBC but the final message ==is encrypted using a different key==
<!--SR:!2023-12-19,4,290-->

# Digital Signatures

Signatures are ==publically verifiable== - in MACs, only *Bob* can verify *Alice's* signature
<!--SR:!2023-12-19,4,270-->

Signatures provide ==non-repudiation== - *Alice* can't later claim she didn't sign a document if the signature exists for it
<!--SR:!2023-12-19,4,270-->

## RSA signature schemes

What does it mean when we say RSA signatures do not provide *existential inforgeability*?
?
Suppose *Eve* has two valid signatures ${\sigma}_{1} = M^{d}_{1} \mod n$ and ${\sigma}_{2} = M^{d}_{2} \mod n$ from *Bob* on messages $M_{1}$ and $M_{2}$.
Eve can produce a new valid signature for *Bob* on the message $M_{1} \cdot M_{2}$ by multiplying the two signatures together.
<!--SR:!2023-12-19,4,290-->

How can we prevent *existential inforgeability* in RSA?::We [[Cryptography#Cryptographic Hash Functions|hash]] the messages *before* we sign them (otherwise the signatures could just be hashed together)
<!--SR:!2023-12-18,4,270-->

# Public Key Authenticity

What does a certificate contain?
?
- A *public key*
- A *subject*, identifying the key's owner
- A *signature* by a *certificate authority*
<!--SR:!2023-12-19,4,250-->

Certificate Authorities can ==outsource the signing of certification== to other parties
<!--SR:!2023-12-19,4,290-->

What can we do if a CA's private key has been compromised?
?
Either have a list of all the known revoked certificates (***Certificate Revocation List***) or have that authority's *status* changed to compromised and check this upon fetching certificates (***Online Certificate Status Protocol***)
<!--SR:!2023-12-18,4,270-->

What is the "chain of trust" when discussing CA's?::If a CA is found to have been compromised, any CA's they outsourced signing to are noted as bad status, and any beneath them, etc
<!--SR:!2023-12-19,4,290-->
# TLS

How does TLS ensure *confidentiality* in both setup and data transmission?
?
- **Setup** - [[Secure Communications#Public Key Encryption|Public key encryption]] (like RSA or DH)
- **Data transmission** - [[Cryptography#Symmetric Cryptography|Symmetric Encryption]] (like [[Cryptography#AES|AES]])
<!--SR:!2023-12-19,4,270-->

How does TLS ensure *integrity* in both setup and data transmission?
?
- **Setup** - [[Secure Communications#Public Key Encryption|Public key encryption]] (like RSA or DH)
- **Data transmission** - [[Cryptography#Cryptographic Hash Functions|Hash-based]] [[Secure Communications#Message Authentication Codes|MACs]] (like [[Secure Communications#HMAC|HMAC]] using `SHA-256`)
<!--SR:!2023-12-19,4,290-->

How does TLS ensure *authenticity* in setup?
?
- **Setup** - [[Secure Communications#Public Key Encryption|Public key encryption]] (like RSA or DH)
<!--SR:!2023-12-18,4,250-->

How does the TLS handshake go?
?
1. Client proposes cryptographic schemes it can do
2. Server sends back best one that it wants to do
3. Server sends certificate
4. Client verifies certificate
5. Client and server exchange keys
6. Client and server verify keys
7. Data transfer can now begin using a shared private key
<!--SR:!2023-12-19,4,290-->

How do we prevent the MiTM attack on DH in TLS?::By [[Secure Communications#Digital Signatures|signing]] the messages exchanged during [[Secure Communications#Basic Key Exchange|key exchange]]
<!--SR:!2023-12-19,4,290-->

How does the `LOGJAM` exploit work?::A MiTM attack, the attacker forges the *type of encryption used* to an older one that can be brute forced
<!--SR:!2023-12-19,4,290-->

# Anonymous Communication

## nDC

In $nDC$, how many coin flips does every cryptographer see?::$n-1$
<!--SR:!2023-12-19,4,290-->

If a cryptographer **did not** pay, what do they announce?::The *XOR* ($\oplus$) of the flips they saw
<!--SR:!2023-12-19,4,270-->

If a cryptographer **did** pay, what do they announce?::The negation of the *XOR* ($¬\oplus$) of the flips they saw
<!--SR:!2023-12-17,2,250-->

## Crowds

We route a request through a *crowd* of users to ==hide the original sender's identity, since we know the last crowd member will not be the sender==
<!--SR:!2023-12-19,4,270-->

## Chaum's Mix

In mix nets, who is the identity of the sender anonymous from?::The recipient, and anyone who intercepts the recipient's message. The mix server itself *does know* the identity.
<!--SR:!2023-12-19,4,270-->

How do mix nets avoid matching the timing of messages?::Through keeping a pool of messages, and sending them from this, hopefully in a non-linear form.
<!--SR:!2023-12-19,4,290-->

Mix nets are often comprised of ==multiple mix servers to further obscure messages==
<!--SR:!2023-12-19,4,290-->

## TOR

TOR is *source-routed*, meaning ==the user defines the path a packet takes==.
<!--SR:!2023-12-19,4,290-->

How does TOR allow users to get around censored websites in their region?
?
Through TOR's worldwide collection of relays, the user can route their packet so it ends up out of an uncensored one
<!--SR:!2023-12-19,4,290-->

How does TOR maintain sender anonymity?::Through *layers* of encryption, each relay node only knows the immediate *predecessor* and *successor*, so by routing through multiple nodes, the path cannot easily be traced (as only the sender knows this)
<!--SR:!2023-12-19,4,290-->

How can a [[Network Security#Block (DOS)|DOS]] be used to compromise TOR's security?::By compromising an entry and exit node, and [[Network Security#Block (DOS)|DOS'ing]] all others, we can track all traffic entering and exiting the network. Using tags/timing attacks, we can work out who's sending these messages and to where.
<!--SR:!2023-12-19,4,290-->

