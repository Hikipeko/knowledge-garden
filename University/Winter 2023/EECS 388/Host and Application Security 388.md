## L14 Authentication and Passwords

**Authentication** is the protocol where one party proves the truth of some assertions about themselves to another party. It can be something your know (password), something you have (security token), or something you are (fingerprint).

#### Passwords

Passwords are a *weak* form of authentication. Never reuse + two-factor authentication + password manager + outsourcing sign-on.

##### Problems with Passwords

* People reuse passwords
* People forget their passwords
* Passwords breaches
* Phishing attacks

##### Defense

* Rate-limit login attempts
* Require solving a CAPTCHA (I'm not a robot)
* Store H(password) in database
* Use `<salt, H(salt||password)>` to protect against Rainbow Table attack
* Use hash function that is *slow* and *memory-hard*, e.g. `scrypt`.
* Store root password hash in a *secure enclave*.
* Use two-factor authentication

##### Two-Factor Approaches

* One-time passwords
* Phone calls
* Universal 2nd Factor (U2F): secure, but need a hardware device.
* Biometrics, e.g. fingerprints, face id.

**WebAuthn**

![[Pasted image 20230315191843.png|550]]



## L15 Malware

Malicious software is any software intentionally designed to secretly run on client devices, servers or networks and cause harm.

* **Spyware** is malware that provides remote access to data and sensors available from the device.
* **Adware** is unwanted software that display paid advertisements.
* **Cryptojacking** malware uses the user's device to mine for cryptocurrency.
* **Ransomware** encrypts files and demand payment.
* **DDoS** by infected clients.

##### Infection Methods

* **Software Exploits**: e.g. buffer overflow. Zerodium has a market for software exploits.
* **Trojan horse** appears to perform a desirable function but performs undisclosed malicious functions.
* **Hardware Trojans** is a malicious change to circuits or chips
* **Driven-by downloads**: exploit client vulns. to install malware. Exploit Kits automate the process of exploiting vulnerability.
* **Malicious network**: malicious code injected to unencrypted web pages.
* **Compromised server**: source code altered.
* **Social engineering**: trick users into install malwares. E.g. dropped USB drives.
* **Supply chain attacks**: malicious functions added to hardware / software.
* **Insider attacks**: attacker with local access runs it directly. 

##### Virus

A self-replicating malware which spreads by modifying other programs to include copies of itself. It usually infects a program by prepending its code to the executable.

* **Polymorphic viruses** generate a new key and re-encrypt their code.
* **Metamorphic viruses** rewrites themselves completely, having same functionality.

##### Worms

A worm spreads by exploiting network vulnerabilities to run copies itself. Can spread by [[Web and Network Security 388#Cross Site Scripting (XSS)]].

##### Botnets

Collections of compromised machines under the unified control of an attacker. Remotely commanded via *command and control (C&C) infrastructure*. Often have a financial motive: *malware-as-a-service*.

Example: Mirai botnet infected IoT devices by guessing passwords. It DDoS against a large DNS provider and caused a large Internet outrage.

**Rootkits** are malware components that uses stealth to maintain a persistent and undetected presence on a machine. It can even be a installed virtual machine.

##### Defense

* Secure programming
* Security testing
* Secure architectures
* Run-time detection
* Google safe browsing
* Antiviruses: signature detection & heuristic detection
* Intrusion-prevention systems




