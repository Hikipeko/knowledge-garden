## L7 The Web Platform

##### Web Components

1. HTML is the standard language for web documents
2. JavaScript can read and modify page content through the [[Web development 485#DOM|DOM]] interface
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

Think of how can all of these be done in a single network round-trip. After the shared secret is established, all data is encrypted. Then client generates random key to be used for later symmetric encryption, encrypt with server public key, and send to server. The server decrypts this random key, and the session is established.

##### Certificates

![[Pasted image 20230208185312.png|400]]

Certificate is a message asserting the server's identity, signed by a certificate authority (CA). The public keys for the root CAs are embedded in browsers. They are used to verify certificates. With the design, we separates the issuance of certificates and its use.

![[Pasted image 20230208185751.png|330]] ![[Pasted image 20230208185914.png|330]]

**Certificate chains**: CAs sometimes issue intermediate CA certificates, which lend permission to sign further certificates. This results in a certificate chain.



## L10 HTTPS Attacks and Defenses

#### Fooling Users

##### Homographs

Use visually similar domain name to trick users. IDN homograph attacks use international characters which have the same pixel appearance. 

Mitigation: Browsers use heuristics to block these sites.

##### Stripping

![[Pasted image 20230211191212.png]]

A MITM exploit the HTTP that the user is using, even if the server only accepts HTTPS.

Mitigation: **HTTP Strict Transport Security (HSTS)**, the server can use a header to instruct the browser to use only HTTPS. Also, it can add itself to HSTS preload list, which is shipped with browsers.

##### Phishing

A fake website that pretends to be some famous websites (amazon). Mitigation: Google Safe Browsing uses ML to detect these sites.

#### Problems with Site Design

**Mixed Content** can be modified or leak information, e.g. browser might allow images to be loaded over HTTP. Mitigation: use HTTPS for all resources.

**Cookies** will be sent unencrypted for an HTTP request. Mitigation: Set the security attribute on cookies.

**HTTPS reveals information**. Mitigation: use VPN or Tor for privacy.

#### CA Weaknesses: Fooling Validation

##### Fooling Validation

What if an attacker can falsely convince a CA that they control a domain? They can perform a MITM attack and pretend to be that domain and steal users' information!

1. Email validation. Mitigation: Sites must prohibit users creating these email addresses: administrator, admin, etc.
2. HTTP validation: Attack intercept CA's GET request. Mitigation: CAs can introduce multi-perspective validation, sending the GET request from servers all over the world.

##### Attacks on CAs

Occasionally CAs get hacked. Mitigation: CAs can revoke their certs by adding them to a **Certificate Revocation List (CRL)**

Now browsers require CAs to record every cert they issue in a public ledger, called a **Certificate Transparency (CT) log**. Server can monitor CT logs to detect untrusted certificates.

![[Pasted image 20230211194217.png|400]]

#### Bugs in TLS Implementations

OpenSSL Heartbleed. Mitigation: Use formal verification techniques and write TLS code in memory-safe languages.

#### TLS Protocol Vulnerabilities

The **Compression-related Attack** exploits that some browsers compress the HTTPS request. Mitigation: use TLS 1.3, and use tools such as **SSL Labs** to test your server.

#### Server Vulnerabilities

TLS servers must protect their private key.



## L11-12 Networking

See [[EECS 489]]

![[Pasted image 20230220181003.png]]

Lower layers provide services to layers above. Higher layers use services of layers below.

##### Attack Models

* Off-path attacker: can talk to hosts, but cannot see packets.
* On-path attacker: can see and add packets.
* In-path attacker: can see, add, and change packets.

##### Physical Layer

* Eavesdropping
* Jamming
* IMSI catchers
* Weak encryption

##### Link Layer

* Packets are in plaintext.
* Clients can maliciously change their MACs.

##### Network Layer

* Packets are in plaintext.
* IP headers are not authenticated.
* ARP spoofing: a malicious host in a LAN can pretend to have a MAC address.
* BGP hijacking

##### Transport Layer

* Segments are in plaintext.
* On-path attacks are trivial.

##### Application Layer

###### DNS

See [[Application Layer 489#2.4 DNS - The Internet's Directory Service|DNS]]. Attacks:

* **DNS hijacking**: attacker changes DHCP setting to point clients to attacker-controller DNS server
* **Host file hijacking**: edit OS hosts file
* **DNS monitoring & blocking**
* **Off-path DNS cache poisoning**: attacker sends malicious DNS query response to the local DNS resolver

Defense

* **DNSSEC** adds authentication and integrity
* **DNS-over-TLS** adds confidentiality (but not authentication)

##### DoS Attack

Overwhelm a host or network with traffic, such that it cannot process legitimate requests.

###### SYN flood attack 

The attacker sends SYN packets from many spoofed source IPs. By leaving the TCP connection incomplete, the server soon run out of resources.

Defense: use SYN cookies which encode the initial connection state in the SYN-ACK packet itself.

###### Amplification

The attacker sets the source IP address to victim's IP address, and make huge amount of large UDP requests. Botnets generates nearly 1Tbps of requests.

Defense:

* **Ingress filtering**: ISPs drop packets from client if source IP is outside assigned range.
* **CDN**: use large CDN to absorb DDoS attacks.

##### Defense

* End-to-end encryption
* Use secure protocols
* Passive network monitoring
* Active port scanning
* Firewalls
* VPN


