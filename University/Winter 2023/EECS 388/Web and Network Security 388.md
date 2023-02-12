## L7 The Web Platform

##### Web Components

1. HTML is the standard language for web documents
2. JavaScript can read and modify page content through the DOM interface
3. [[Application Layer 489#URL|URL]] from EECS 489
4. [[Application Layer 489#2.2.3 HTTP Message Format|HTTP]] from EECS 489
5. [[Application Layer 489#2.2.4 User-Server Interaction: Cookies|Cookies]] from EECS 489
6. Browser

The browser have to isolate sites from each other, and protect users from malicious sites.

##### Same-Origin Policy

Each **origin** (scheme, hostname, and port) has local client-side resources that are protected. A document can include **frames**, which display another HTML pages. However, it cannot access data from other HTML pages if the origins are different.

##### Cookie Scope

A site can set a cookie for its own domain or any parent domain, as long as the parent domain is not a **public domain**. Note that cookies also specify a path on the site.

When using cookie, remember to add "Secure" attribute to prevent it from being sent through http.

Server can set **HttpOnly** attribute to prevent cookie from being accessed by DOM (e.g. Google Analytics scripts).

###### Cookie-Based Authentication

Upon successful login, server sets a cookie with an **authentication token**, and store this token, username, and expiry time in the DB.



## L8 Web Attacks and Defenses

##### Cross-Site Request Forgery (CSRF)

A site contains an attribute or script causing an http request to another website (e.g. send money to Mallory's bank account). It exploits the browser & cookie infrastructure.

**Defense**

* Referer validation: let the http requests contain URL of page making the request
* Secret token validation: embed a secret value (a session-dependent token) in each request
* SameSite cookies: prevents browser from sending cookie in cross-site requests

#### Injection Attacks

Exploits vulnerability that mistake untrusted data for code.

##### SQL Injection (SQLi)

Name your child `Robert'); DROP TABLE Students;--'.

**Defense**

* Parameterized (prepared) SQL statements which separates code from input data
* ORM (Object relational mapper), a language-specific methods for accessing data using native code

##### Cross Site Scripting (XSS)

**Reflected XSS**

Suppose some part of the website just echoes the input, the attacker can create a link which echoes a malicious script, which is then sent to the user's browser.

**Stored XSS**

The attacker send a post containing a malicious script.

**Defense**

* Input validation: checks the input against a rigorous specification of what should be allowed.
* Output escaping: encode all special characters in output.
* Content security policy (CSP): let the web page to specify what scripts are allowed to execute.



## L9 HTTPS and the Web PKI

#### TLS (Transport Layer Security)

TLS is a cryptographic protocol above TCP to provide a secure (confidentiality, integrity, and authenticity of server). HTTP + TLS = HTTPS.

Thread model: the client and the servers are secure, while the network might be malicious.

##### TLS Protocol Handshake

![[Pasted image 20230208184859.png]]

Think of how can all of these be done in a single network round-trip. After the shared secret is established, all data is encrypted.

##### Certificates

![[Pasted image 20230208185312.png|400]]

Certificate is a message asserting the server's identity, signed by a certificate authority (CA). The public keys for the root CAs are embedded in browsers. They are used to verify certificates. With the design, we separates the issuance of certificates and its use.

![[Pasted image 20230208185751.png|330]] ![[Pasted image 20230208185914.png|330]]

**Certificate chains**: CAs sometimes issue intermediate CA certificates, which lend permission to sign further certificates. This results in a certificate chain.