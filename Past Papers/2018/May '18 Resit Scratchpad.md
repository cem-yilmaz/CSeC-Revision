---
tags:
  - uni
  - sem1
---
# 2. TLS
![[Pasted image 20231213111137.png]]
## a)
![[Pasted image 20231213111205.png]]
Since the admin controls the gateway, all communications will pass through it.

1. `client-hello`: The Admin will take note of who is connecting to what server so they know to log these `HTTPS` communications. 
2. Certificate - the certificate verification (during the handshake) is *one-sided* - the server has no way of knowing that the client has successfully verified *it's* certificate. The server will send back a certificate and while the admin *can* verify this, it's not necessary. The admin then sends the client the `A` certificate pretending it came from the server, which it will verify as each computer has the verification key.
3. Key-exchange (client send) - the client now sends over the encrypted random number with the server's $pk$, and the admin just forwards this on. They do not need to take note of it.
4. Key-exchange (server send) - this is the main step in the MiTM attack. The server will send back the session key it will use to encrypt and decrypt the messages. The admin **stops** this reaching the client, and generates their own key to send and pretend instead
5. For all further communication, the client sends a message encrypted with the admin's key. As it enters the gateway, they can *decrypt* it, log the contents, then *encrypt* the plaintext with the real *server's* key so the server can get it correctly
This includes the `finish` message, and all other messages in the session.
## b)
![[Pasted image 20231213113012.png]]
No, since all traffic passes through this gateway whether forged or not. The server will receive messages encrypted with the shared session key, which it can only assume is safely secure between itself and the client
## c)
![[Pasted image 20231213113659.png]]
Only if they're able to work out that the certificate used in the communication isn't from `acmebook`. Ultimately, the CA will not sign this certificate in `acmebook`'s name - so if the employee can see this cert is not from `acmebook` as they're connecting, they can detect this attack
## d)
![[Pasted image 20231213114140.png]]
The attacker could act between the Gateway and the server using the same steps

*shortened for time now, will be realised in exam*
- Attacker sends their cert to admin spoofing server like how admin does to client
- During key exchange, attacker sends their own key and stores admin's given key
- Attacker stores server key, sends spoofed key
- From now on, while the admin can still read this, so can the attacker by doing the same "decrypt with spoofed key, encrypt with real key" stuff as earlier, and can log the plaintext too

Since the gateway doesn't check certs, it won't know it's receiving tampered messages.
## e)
![[Pasted image 20231213114518.png]]
The attacker can utilise the `LOGJAM` attack to downgrade the security of connection.

The attacker acts as a MiTM, and during the `client-hello`, tampers with the message to downgrade the supported encryption to `SSL2`. The server, since it supports this, will use `SSL2`. The attacker can then log and relay all communication between client and server, while using `ROBOT` to attack and gain the session key for that session. it can then decrypt all messages as `SSL2` does not have *forward secrecy*.
## f)
![[Pasted image 20231213115255.png]]
*not a clue*

The best defense would be to rate limit connections using `SSL2`. This would make a `ROBOT` attack much harder to execute, as the iterative queries would be rejected. Additionally, if we must use `SSL2`, we should try implementing *forward secrecy* through random salts added to messages, to make simple decryption using the exploited session key much harder
# 3. CSRF attacks and defenses
## a)
![[Pasted image 20231213115924.png]]
This is when a website blindly trusts a user to make their requests. The user accesses the legit target site and generates a valid session cookie. They then access the evil site, which has an (often hidden) script that will make a (maliciously intended, but genuine in nature) request to the legitimate site. When executed, since the session cookie is valid, the site will take it and process this request
## b)
![[Pasted image 20231213120619.png]]
No. `CSRF` relies on session cookies already existing, not stealing them. The attacker has no way of knowing what happened to an attack (outside of the real world impacts of that request, which might be sparse). `HTTPS` just encrypts this traffic, but the malicious CSRF attack still carries on.
## c)
![[Pasted image 20231213120753.png]]
Not solely. They only make it so a CSRF can happen when a current session is also occurring; this lowers the chances of it happening, but do not make it zero.
## d)
![[Pasted image 20231213120830.png]]
The target site could have a *whitelist* of all trusted sites it accepts requests from. For example, to use a bank's payment system on your site, you may have to contact the bank who audit the site and deem it genuine, adding you to the whitelist. Then, when they receive a request, they check the `Referer` against this whitelist - if it's on it, process the request.

This means even if the scenario described earlier still occurs, the bank will not see the evil site on the page, and can safely deny the request, assuming a CSRF is taking place.
## e)
![[Pasted image 20231213122504.png]]
This prevents `CSRF` attacks by making the request itself much harder to forge. Even if we have the cookie, we would need the token too. If we don't, the request is denied automatically. This only works if the CSRF token cannot be obtained via means such as replay attacks - if this token can somehow be obtained and appended to requests by the attacker, this defence fails
## f)
![[Pasted image 20231213122513.png]]
Not forever. A fixed random string can be easy to guess or brute force. As emtnioned earlier iof we hguess the string then we forge these requests all the same 
## g)
![[Pasted image 20231213123111.png]]
Yes. While a `CSRFToken` is present, it is not chjeclef. csrf can still use an existing cookie and make the request