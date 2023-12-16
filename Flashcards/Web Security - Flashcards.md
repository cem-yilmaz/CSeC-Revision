---
tags:
  - uni
  - sem1
  - flashcards/CompSec/WebSecurity
---
# `HTTP`

What are the two types of `HTTP` requests?:: `GET` and `POST`
<!--SR:!2023-12-19,4,290-->

How do we handle dynamic content in `HTML`?::Through the use of scripting languages such as `JavaScript`
<!--SR:!2023-12-19,4,290-->

How do we embed a webpage (or elements from it) inside another?::Using a *frame* (`<iframe src="url"></iframe>`)
<!--SR:!2023-12-19,4,290-->

How do we maintain *state* in `HTTP`?::`HTTP` isn't inherently stateful, so we pass a *token* back and forth between the client and server. This token is either a [[Web Security#Hidden Fields|hidden field]] or a [[Web Security#Cookies|cookie]].
<!--SR:!2023-12-18,4,270-->

A hidden field is a ==hidden form containing a session ID in all the HTML pages sent to the client==
<!--SR:!2023-12-19,4,290-->

# Cookies

A cookie is ==small piece of information that a server sends to a browser and is then stored inside the browser==.
<!--SR:!2023-12-19,4,290-->

What attributes does a cookie have?
?
- `name=value`
- `expires`: (when to be deleted)
- `domain`: (where to send)
- `path`: (where to send)
- `Secure`: (only over `SSL`)
- `HttpOnly`: (only over `HTTP`)
<!--SR:!2023-12-18,4,230-->

# Same Origin Policy

The *Same Origin Policy* (SOP) restricts ==how scripts/resources from one *origin* can interact with a resource from another==
<!--SR:!2023-12-18,4,270-->

When dealing with `JavaScript`, SOP allows it ==to call functions, but not inspect the source of a foreign page==
<!--SR:!2023-12-19,4,290-->

When dealing with *frames*, SOP allows ==them to be loaded, but not inspect nor modify the content of the foreign page==
<!--SR:!2023-12-18,4,250-->

# Cookie Policy

A cookie with a `domain` and `path` will be sent to all URLs of the form ==`http://*.domain/path*`==
<!--SR:!2023-12-19,4,290-->

If a cookie is `HTTPonly` ==scripting languages cannot access nor manipulate the cookie

If a cookie is `Secure` ==it will only be sent over encrypted `HTTPS` communications, not `HTTP`==
<!--SR:!2023-12-19,4,290-->

# Cross-Site Scripting

How does XSS work?
?
The malicious site (e.g. `evil.com`) provides a malicious script, and an attacker tricks a vulnerable server (e.g. `bank.com` ) to send the script to the user's browser. The browser now believes the script originates from `bank.com` and will run it, with `bank.com`'s access privileges.
<!--SR:!2023-12-19,4,290-->

What are the two types of XSS attacks?
?
- **Stored XSS attacks**
	- The script is _permanently stored on the target servers_ and accessed, such as in a database, message forum, visitor log, comment field, etc
- **Reflected XSS attacks**
	- The more common type of XSS, in a reflected attack the script is _reflected_ off the web server. This could be through an error message, or more commonly through a [phishing email](app://obsidian.md/OS%20Security#Phishing) - which directs to `evil.com`, and then redirects to `bank.com` with the attack request
	- The key to a good reflected XSS attack is finding a server that will _echo_ user input back in the `HTML` response.
	- We could replace `$term` with a [script](app://obsidian.md/index.html#JavaScript) that e.g. sends the `bank.com` cookies to `evil.com`, which is fine according to the browser, because it's all done through `bank.com`.
	- Imagine we visited `https://bank.com/search.php?term=...`
```html
<html>
	<title>
		Search results 
	</title>
	<body> 
		Results for: $term
		...
	</body>
</html>
```
<!--SR:!2023-12-19,4,290-->

How do we prevent XSS?
?
- Validating input
- Providing a whitelist of scripts that can appear on the page
- Enabling a cookie's `HTTPOnly` attribute (not conclusive!!)
<!--SR:!2023-12-18,3,230-->

# Cross-Site Request Forgery

How does CSRF work?
?
Upon visiting a malicious site, if a user *already* has a valid session token, a CSRF attack will send a request (via a hidden form & script) to that site pretending to be the real user (due to scripts). The cookie will be *sent alongside* the `GET` request from this form.
<!--SR:!2023-12-19,4,290-->

How can we protect against CSRF?
?
- Check the referrer field - this tells you where a request came from. This can be used for tracking, and as such some users can disable these
- CSRF tokens - have a token on every (valid) site which is hard to forge/steal and reject any requests without this token
- `SameSite` attribute - prevents cookies being sent in cross-site requests. Unfortunately, not suitable for all cookies that are at risk
<!--SR:!2023-12-18,4,250-->