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



## L16-17 Control Hijacking

**Binary exploitation** is the process of subverting a compiled application such that it violates some trust boundary.

#### Buffer Overflow & Stack Shellcode

See [[Process 482#Address Space|memory organization]].

* eax: store return values
* eip: instruction pointer
* esp: stack pointer
* ebp: frame/base pointer

![[Pasted image 20230321124310.png|350]] ![[Pasted image 20230321125827.png|150]]

##### Function Call

1. Caller pashes arguments
2. `call = push eip; jump foo`
3. After jump, callee `push ebp; mov ebp, esp`

##### Function Return

1. `leave = move esp, ebp; pop ebp`
2. `ret = pop eip`

##### Blowing things up

1. How can we guess the position that store the return address?
2. How can we guess the position of the evil code?

##### Data Execution Prevention

Defense: A memory address cannot have write and execute access at the same time.

#### Data-only Attacks & Return-to-Attack

**Data-only attacks** modifies data in the stack and gain advantage.

**Return-to-attack** reuse code that already exists, e.g. in the library. Our goal is that after the vulnerable function returns, we want the stack to be like a target function is just called. Using this technique, we can invoke any function that exists in the binary, e.g. `execv`.

![[Pasted image 20230321131851.png]]

**Extraneous Function Removal**: Take out functions that can launch shells.

#### Return Oriented Programming (ROP)

**ROP gadget** is a small section of code end in `ret`. Attacker uses ROP gadgets to compose functions they need. This is Turing complete. ROP chains are addresses of ROP gadgets stacked together.

##### ASLR

Address space layout randomization moves around code, making it hard to predict references. However, if the attacker can locate a single pointer in `libc`, he can locate any other functions by relative position.

![[Pasted image 20230321133612.png|200]] ![[Pasted image 20230321133629.png|200]]

##### Stack Canaries

Store a secret canary value above the caller FP, and detect stack modification.

#### Buffer Over-read

First read the stack as well as the canary value, and then modifies the stack without changing the canary value. **Heartbleed** is an buffer over-read attack against OpenSSL in 2014.

##### Integer Overflow

```c
void foo(int *array, int len) {
	int *buf;
	// what if len is very large, say 1,073,742,024, len * 4 becomes 800
	buf = malloc(len * sizeof(int)); 
	if (!buf) return;
	int i;
	for (i=0; i<len; i++) {
		buf[i] = array[i];
	}
}
```

##### Signed/Unsigned Integers

```c
int sendField(int socket, char *field){
	int fieldLen = 0;
	// what if field_len is negative?
	read(socket, &fieldLen, 4);
	if (fieldLen > 10) return;
	write(socket, field, fieldLen); // size_t
	return fieldLen;
}
```

Defense: automated testing, taint analysis, fuzzer, etc.



## L18 Access Control and Isolation

**Principle of least privilege**: Every program and user is given the least amount of privilege to complete its job.

##### Security Model

System abstraction that enables us to discuss and formulate a policy. Consists of subject, objects, and operations.

##### Security Policy

Define an access matrix.

![[Pasted image 20230328213257.png|400]]

**Principle of complete mediation**: each access to every object must be checked by authority by a mediator.

**Caching checks**: time-of-check, time-of-use vulnerabilities.

##### Unix Security Model

**Users and Groups** User belongs to several groups. Provides role-based access control. Superuser (root) has authority to do anything.

**File Permissions** File permission bits specify what role can do what to the file.

**Process** Every process has an *Effective User ID (EUID)*.

##### Confused Deputies

Low-privilege process tricks high-privilege process into performing an action it cannot perform itself. E.g. [[Web and Network Security 388#Cross-Site Request Forgery (CSRF)]].

#### Process Isolation

Ensure misbehaving process cannot harm rest of system.

**Reference monitor**

* Mediate every requests from applications
* Must be **tamperproof**: cannot be killed
* Must be small enough to be validated. Part of trusted computing base.

**chroot**: `chroot /tmp/guest; su guest; ./myapp` Confine `myapp`'s file within `/tmp/guest`.

##### 1 System Call Interposition

![[Pasted image 20230329122324.png|550]]

UNIX `ptrace` wakes up when `pid` makes a system call. Monitor checks policy.

##### 2 Containers

Confinement at the level of OS. Creates multiple isolated userspace instances.

##### 3 Virtual Machines

Isolate OSs on a single machine. Malware cannot escape from the infected VM.

**Covert Channels**: unintended communication channel between isolated components.



## L19 Machine Learning Security

How to make a model trustworthy?

* Poisoning attack: Microsoft's Tay chatbot outputs racist tweets.
* Membership inference attack: the model might memorize some unusual datapoints

![[Pasted image 20230324125313.png]]



## L20 Privacy and Anonymity

Privacy is seclusion, limits, control, secrecy, and liberty.

**Data brokers** such as Acxiom and Oracle aggregate data sources to create detailed profiles about people, and sell these data.

**Location tracking** Dozens of companies use smartphone locations to advertise.

##### Third-Party Cookies

When a page loads resources from another site, the browser set a cookie and sends it to that third-party origin. E.g. Doubleclick sends the same cookie each time any site loads any ad from Doubleclick.com.

##### Browser Fingerprinting

Even without cookies, sites can use browser fingerprinting to track users by reading attributes about browser information. E.g. Canvas fingerprinting

##### Data Anonymization

* **k-anonymity**: Each record is indistinguishable from at least k - 1 other records. Really hard to achieve.
* **Differential Privacy**: Adding some random noise to the data.

**National Security Order** issued by FBI compel network operators to provide information.

##### Off-the-Record (OTR) Messaging

![[Pasted image 20230329134933.png|400]]

A protocol designed for confidentiality, authentication, forward secrecy, and deniability. **Deniability** means any passive observer could modify old ciphertext and produce forged but valid MACs for it. This allows the user to deny the fact that they are able to decrypt the ciphertext.

##### Anonymous Networking

VPN, Tor (entry node, middle node, and a exit node).



## L21 Digital Forensics

Digital forensics is used to investigate crimes and recover from attacks.

##### Identification

Identify specific objects that store important data for the case analysis.

##### Collection

Preserve evidence, establish chain of custody, ensure data stays intact and unaltered. Produce a forensic image of the data. Prioritize collection by volatility: RAM > disk > external media. A **forensic image** preserves all partitions and residual data. The image should be created using a *write-blocker* device to prevent writing to the disk. Analyst should record original hash to later confirm image wasn't changed.

###### Imaging RAM

**Cold-boot attack** reset and boot the system with special-purpose software designed to image RAM.

**Mobile devices** place device in a Faraday bag to shield RF signals.

##### Analysis

Examine the information stored on digital evidence and conduct an analysis of the incident. Digital forensics tools such as Autopsy.

[[File System 482]]

![[Pasted image 20230331140030.png]]

##### Reporting

Interpret findings, prepare and deliver an expert report and testimony. A well written report.

##### Hiding Data

* Encryption
* Obfuscation: transforms code or binaries to conceal their purpose while preserve their function
* Watermarking: add durable mark to a file that resists removal attempts
* Steganography: encode hidden data inside other data, e.g. hide message in least significant bits of a bitmap image

##### Full Disk Encryption

![[Pasted image 20230331191356.png|350]]

Protect data by encrypting every sector of a storage partition using a secret key. 

* Store the encrypted key $k$ with PIN $p$ by applying a key derivation function $E_{KDF(p)}(k)$.
* Can use a *trusted platform module (TPM)*, a tamper-resistant chip that measures boot-time code and only provide the key to OS.

**Securely erasing media** by encrypting data and discarding the key or physically destroy the media.
