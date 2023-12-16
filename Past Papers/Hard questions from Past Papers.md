---
tags:
  - uni
  - sem1
---
# Network Security
[[Network Security]]

- Firewall - specifics of rules using `iptables`
- Known ports of protocols (`HTTP`, `SSL`, etc)
## Revision
Using [`nmap`](https://nmap.org/), we can see *open ports* - representing a point of contact between the *Internet* and the *application that is listening on **that particular** port*. It is advisable to only open ports for essential network services.
The simplest method of port scanning is known as a *TCP scan* in which the port scanner attempts to initiate a [[Network Security#TCP|TCP]] connection with each of the ports on a target machine.
## Questions
From [[Goodrich, Michael T_Tamassia, Roberto - Introduction to computer security (2014, Pearson Education Limited).pdf#page=327|Network Security II - Exercises]]
### R-8
*How can SSH be used to bypass firewall policy? What can a network administrator do to prevent this circumvention?*

Since `SSH` provides potentially `root` level access to the computer, we can use this to bypass a firewall policy.

Imagine a firewall that allowed `SSH` traffic in and out, but not `HTTP` requests. An adversary could use `SSH` to send commands to perform *local* `HTTP` requests, and then send this data back *through* `SSH`.

This could be prevented against by using an [[Network Security#Application Layer (Firewall)|application layer firewall]], which has more context on what data is being sent and received (although easier solution is to block `SSH` access with a whitelist)
### R-9
*Describe a firewall rule that can prevent IP spoofing on outgoing packets from its internal network.*
A rule that denies any packets leaving the network if they do not (appear to) come from an address listed on the network itself
# Cryptography
[[Cryptography]]
# Secure Communications
[[Secure Communications]]

- `SSL(/TLS)` - protections against weak encryption schemes
- TOR - deep analysis into how attacks could be conducted
- TOR - protection of anonymity
- nDC - proof of confidentiality (and how to break it)
- Mixnets - like everything lol

There is nothing in the book!! Uh oh!
# OS Security
[[OS Security]]

- Layout of memory during runtime
## Questions
### C-8
*Consider the following piece of C code:*
```c
int main(int argc, char *argv[])
{
	char continue = 0;
	char password[8];
	strcpy(password, argv[1]);
	if (strcmp(password, "CS166")==0) 
	{
		continue = 1;
	}
	if (continue)
	{
	*login();
	}
}
```
*In the above code, `*login()` is a pointer to the function `login`. (In `C`, one can declare pointers to functions which means that the call to the function is actually a memory address that indicates where the executable code of the function lies)*
#### 1)
*Is this code vulnerable to a **buffer-overflow attack** with reference to the variables `password[]` and `continue`? If yes, describe how an attacker can achieve this and give an ideal ordering of the memory cells (assume that the memory addresses increase from left to right) that correspond the variables `password[]` and `continue` of the code  so that this attack can be avoided.*

Yes. The value of `password` is assigned by the *unsafe* `strcpy` function, which does not check the length of the buffer it is writing into. 
`strcpy(password, argv[1])` will:
- start writing values into the beginning of the `password` buffer
- and not stop until it has exhausted the values in `argv[1]`
The danger in this is `password[8]` is only 8 bytes long, so if `argv[1]` is longer than this, `strcpy` will write data from `argv[1]` into the other regions of memory.
An attacker could exploit this by writing such a string that extends beyond the buffer and finally into the location in memory that `continue` is stored, and overwrite this with `1`. When the program reaches the `if (continue)` instruction, this check will pass, and it will execute `*login()` with our attack.
An ideal position for `continue` would be *before* the `password` buffer as this means any overflows would go left-to-right, and not affect the `continue` location in memory.

#### 2)
*To fix the problem, a security expert suggests to remove the variable `continue` and simply use the comparison for login. Does this fix the vulnerability? What kind of new buffer overflow attack can be achieved in a multiuser system where the `login()` function is shared by a lot of users and many users can try to log in at the same time?*

It fixes this specific vulnerability, yes. Removing the risk of having our `continue` check overwritten by a value overwrite due a buffer overflow would fix the vulnerability.
A new attack would be to try to overwrite the `return` address of the main function so it becomes that of the `login` function. This could likely be easily acquired from a program decompile since the program has this pointer referenced. No idea what it means by a multi-user system; this attack would work in a single-user or multi-user system.
#### 3)
*What is the existing vulnerability when `login()` is not a pointer to the function code but terminates with a `return()` command? Note that the function `strcpy` does not check an array’s length.*

We can overwrite this `return` address back to the `main` function to point to our own malicious code. This is because the `%eip` will point back to `main` to continue executing instructions, so if we can overwrite this - with the same methods as before, utilising the `strcpy` to push into further bits of memory, until we reach the return address and can overwrite this value with our own address further up in the stack. We can then keep writing, until we reach our own code; probably with the safety of a `NOP` sled beforehand to help us and not necessitate our exactness 
# Web Security
[[Web Security]]

- Passwords - rainbow tables
- CSRF - recap
- SQL - quick syntax recap
- SOP - what it does and doesn't allow between tabs

## Questions
### R-9
*Can an XSS attack coded in Javascript access your cookies? Why, or why not?*

It could only access the cookies of the site that the attack originates from. If an adversary wanted the cookies from `bank.com`, but could only get a script on `facebook.com` to try and steal all cookies, this attack would only steal the cookies for `facebook.com`. If the attacker could get `bank.com` to execute this script (send all cookies to attacker), then *yes*.
### R-16
*What are the main differences between [[Web Security#Cross-Site Scripting|XSS attacks]] and [[Web Security#Cross-Site Request Forgery|CSRF attacks]]?*

Succinctly, XSS exploits a user's trust in a website, and CSRF exploit's a website's trust in a user.

XSS attacks occur on a trusted site, using a malicious foreign script that the website calls (either through stored or reflective means). The SOP will consider this script as to come from the website and run it with that site's privileges.

CSRF attacks occur on an untrusted site. They will use a genuine script stored on the target site's server, and hope the user has a current login token that will authenticate this script running. The SOP won't affect this, as it is calling the script remotely, and the attacker has no way (on their own page, inherently) to know if it worked. 