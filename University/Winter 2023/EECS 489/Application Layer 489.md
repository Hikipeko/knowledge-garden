## 2 Application Layer

The ultimate goal of computer network is to support Web applications such as search engines, video platforms and chatting apps.

### 2.1 Principles of Network Applications

If you want to implement a web app, the only layer you have to care about is the application layer.

#### 2.1.1 Network Application Architectures

##### Client-server architecture
 
Server services requests from many other hosts, called clients. The server has a fixed IP address.

E.g. Web, FTP, Telnet, e-mail, 

A data center houses a large number of servers.

##### P2P architecture

Minimal (or no) reliance on dedicated servers. It exploit direct communication between pairs of intermittently connected hosts, called peers. P2P has self-scalability.

P2P is commonly used in traffic-intensive applications such as Xunlei and BitTorrent.

#### 2.1.2 Processes Communicating

Processes on two different end systems communicate by exchanging messages across the computer network.

The peer downloading files labeled as **client**, the peer uploading the file labeled as **server**. Or during a communication session, the process initiating the communication is labeled as the client.

##### Socket

Socket is the interface (or API) between the processes and the computer network. It is the interface between the application layer and the transport layer within a host (usually implemented in OS).

![[Pasted image 20230109233703.png]]

How to distinguish between end devices? **IP address**

How to distinguish between applications on the same computer? **Port number**

E.g. Web server: 80, mail server: 25.

#### 2.1.3 Transport Services Available to Applications

1. Reliable Data Transfer
2. Throughputs
3. Timing (e.g. CS:GO)
4. Security

#### 2.1.4 Transport Services Provided by the Internet

See [[Transport Layer 489#3.1.2 Overview of the Transport Layer in the Internet]]

#### 2.1.5 Application-Layer Protocols

Defines how an application's processes passes messages to each other.

1. Type of messages (request, response)
2. Syntax of message. (how fields are delineated)
3. Semantics of the fields.
4. Rules for determining ways of response.

Some public application-layer protocols are specified in **RFC**, such as HTTP. HTTP as a protocol is a part of the Web application.

#### 2.1.6 Network Applications

Web, e-mail, DNS, video streaming, P2P, etc.



### 2.2 The Web and HTTP

The World Wide Web is an application enabling users to obtain documents from Web servers over the [[Introduction 489#1.1 What is the Internet?|Internet]].

#### 2.2.1 Overview of HTTP

The Hypertext Transfer Protocol is the Web's application-layer protocol.

HTTP is implemented in both the client program (e.g. browsers) and the server program (e.g. nginx), talking to each other by exchanging HTTP messages. HTTP define the structure of these messages.

A **Web page** = HTML source files + reference objects (images, videos).

An **object** is a file addressable by a single URL.

##### URL

Uniform resource locator = hostname + path name (scheme://host:port/path?query#fragment), a string specifying a unique resource on the Web.

HTTP is a stateless protocol, which means each request is executed independently, without knowing anything about previous requests. HTTP is based on TCP (socket).

#### 2.2.2 Non-Persistent and Persistent Connections

##### Non-Persistent connection

1. HTTP client initiates a TCP connection to the server by creating a socket.
2. The client sends HTTP request message of  a path.
3. Server receives the request via socket, retrieves the object, encapsulated the object in an HTTP response message, and send message via socket.
4. Server tells TCP to close the connection.
5. Client receives the response message, TCP connection terminates.

##### RTT

Round trip time, the time it takes for a small packet to travel from client to server and then back to the client. TCP connection is a "three-way handshake" protocol, while the third part is combined with the HTTP request message.

![[Pasted image 20230110191527.png]]

##### Persistent Connection

With HTTP 1.1 persistent connections, the server leaves the TCP connection open after sending a response. A common choice in modern web servers. **HTTP/2** allows requests and replies to interleave in the same connection. #confused

#### 2.2.3 HTTP Message Format

Uses US-ASCII encoding, so humans can read it.

##### Request

```http
GET /somedir/page.html HTTP/1.1
Host: www.someschool.edu
Connection: close
User-agent: Mozilla/5.0
Accept-language: fr
```

![[Pasted image 20230110192933.png]]

* **GET**: request an object
* **POST**: used when you post reviews or images
* **DELETE**: delete an object on a Web server.
* **HEAD**: GET without request, used mostly for debugging
* **PUT**: create file

##### Response

```http
HTTP/1.1 200 OK
Connection: close
Date: Tue, 18 Aug 2015 15:44:04 GMT
Server: Apache/2.2.3 (CentOS)
Last-Modified: Tue, 18 Aug 2015 15:11:03 GMT
Content-Length: 6821
Content-Type: text/html

(data data data data data ...)
```

![[Pasted image 20230110200922.png]]

##### Status codes

1. 2xx Success
2. 3xx Redirection
3. 4xx Client errors, e.g. 404 Not Found
4. 5xx Server errors

#### 2.2.4 User-Server Interaction: Cookies

HTTP uses cookies to allow sites to keep track of users. Cookie technology has four components:

1. A cookie header line in the HTTP response message
2. A cookie header line in the HTTP request message
3. A cookie file kept and managed by the client
4. A back-end database at the Web site

![[Pasted image 20230110202607.png]]

#### 2.2.5 Web Caching

Web cache, aka **proxy server**: a network entity that satisfies HTTP requests on the behalf of an origin Web server. It caches recently requested objects in its storage. It is typically installed by an ISP.

1. Reduce response time
2. Reduce traffic on an institution's access link to the Internet

##### CDN

Content distribution networks are entities implementing web caching. A CDN company installs geographically distributed caches, and thus localizing the traffic.

##### Conditional GET

Problem: the copy on the cache may be stale

Include a If-Modified-Since header line in the GET request. The server sends the file if the file has been modified, and 304 Not Modified otherwise.



### 2.3 Electronic Mail in the Internet

User agents, mail server, Simple Mail Transfer Protocol (SMTP). Uses TCP service.

#### 2.3.1 SMTP

![[Pasted image 20230112140920.png]]

Has a long history, and thus has come archaic properties. E.g. it used ascii encoding, so multimedia data has to be encoded to ascii.

The client SMTP has TCP establish a connection to port 25 at the server SMTP. Used to transfer file from a mail server to another (step 4). Note that SMTP uses persistent connection.

```smtp
S: 220 hamburger.edu
C: HELO crepes.fr
S: 250 Hello crepes.fr, pleased to meet you
C: MAIL FROM: <alice@crepes.fr>
S: 250 alice@crepes.fr ... Sender ok
C: RCPT TO: <bob@hamburger.edu>
S: 250 bob@hamburger.edu ... Recipient ok
C: DATA
S: 354 Enter mail, end with ”.” on a line by itself
C: Do you like ketchup?
C: How about pickles?
C: .
S: 250 Message accepted for delivery
C: QUIT
S: 221 hamburger.edu closing connection
```

You can try this with **TelNet**.

#### 2.3.2 Comparison with HTTP

HTTP is mainly a pull protocol, while SMTP is a push protocol. It is a TCP connection initiated by the machine that wants to send the file.

SMTP requires each message to be in 7-bit ASCII format while HTTP does not.

#### 2.3.3 Mail Message Formats

```
From: alice@crepes.fr
To: bob@hamburger.edu
Subject: Searching for the meaning of life.

Content
```

#### 2.3.4 Mail Access Protocols

![[Pasted image 20230112203902.png]]

##### POP3

Why don't we use SMTP for Bob's agent? Because Bob needs a pop protocol. Simple, limited function.

1. Client open TCP connection on port 110.
2. Client sends username and a password (authenticate).
3. User agent retrieves message (transaction).

##### IMAP

Allows user to access mails from any computer, maintains a folder hierarchy on a remote server. Used by Gmail.

**Web-Based E-Mail**

User agent is an ordinary Web browser, and the user communicates with its mailbox via HTTP.



### 2.4 DNS - The Internet's Directory Service

**Hostname** is just a name that can be used to identify a host, e.g. google.com.=

**IP address** is hierarchical from left to right.

#### 2.4.1 Services Provided by DNS

Domain name system is

1. A ==distributed database== implemented in a hierarchy of DNS serves.
2. An application-layer ==protocol== that allows host to query the distributed database.

DNS runs over UDP and uses port 53. It is commonly employed by application-layer protocols such as HTTP.

1. Client passes the hostname to the client side of DNS application.
2. DNS client sends a query to DNS server, and receives the IP address.
3. The browser starts a HTTP connection using this IP address.

##### Other services

1. Host aliasing
2. Mail sever aliasing
3. Load distribution (busy site can have multiple IP address, DNS rotation distributes the traffic among the replicated servers).

#### 2.4.2 Overview of How DNS Works

![[Pasted image 20230112211133.png]]

**Root DNS servers** provide the IP addresses of the TLD servers.

**Top-level domain servers (TLD)** (com, org, net, edu, gov, cn, uk) provide the IP addresses for authoritative DNS servers.

**Authoritative DNS servers**. An organization either implements its own authoritative DNS server or pay to have records stored in DNS server.

**Local DNS server** is implemented by ISP. Its IP address is stored in devices accessing the Internet through this ISP.

![[Pasted image 20230112211934.png]]

##### DNS Caching

A DNS server will cache the mapping in its local memory after receiving the query result. Thus, root servers are accessed for a tiny portion of DNS queries.

#### 2.4.3 DNS Records and Messages

The DNS servers together implement the DNS database, which stores **resources records (RRs)**, including the RRs that provide hostname-to-IP mappings. #link database

RR is a four-tuple containing (Name, Value, Type, TTL)

TTL: time to live, determines when to remove TT from cache.

| Type                   | Name                 | Value                                |
| ---------------------- | -------------------- | ------------------------------------ |
| A (Address)            | hostname             | IP address                           |
| NS (name server)       | domain (foo.com)     | hostname of authoritative DNS server |
| CNAME (canonical name) | alias hostname       | canonical hostname                   |
| MX (mail exchange)     | alias hostname(mail) | canonical hostname                   |

If DNS server authoritative for hostname, return Type A record for the hostname.

If not, return Type NS + type A which provides IP of DNS server.

##### DNS Messages

Two type: query & reply, having the same format.

![[Pasted image 20230113133826.png]]

##### Inserting records into the DNS Database

Suppose you want to register a domain name for your website, you have to:

1. Register the domain name
2. The registrar make sure that a Type NS (hostname, DNS server name) and a Type A (DNS server name, DNS server address) record are entered into the TLD com servers. These two RRs are the authoritative DNS servers.

Thus in step 5, the TLD servers will return these two records. The client can then access your DNS server and get IP address of your website.



### 2.5 Peer-to-Peer File Distribution

Our goal is to distribute a large file from a single server to many hosts (peers). In P2P, a when a peer receives some file data, it uses its upload capacity to redistribute the file to other peers. Useful when the server upload bandwidth is small.

$$
DP2P \geq \max\{F_{us}, F_{dmin}, NF_{u + Nui}\}
$$

##### BitTorrent

A popular P2P protocol for file distribution.

The collection of all peers participating in the distribution of a particular file is called a torrent. Each user downloads chunks (typically 256 KB data) while uploading chunks.

A tracker tracks all the users in a torrent. When a new user joins the torrent, it establishes TCP connection with a list of neighboring peers (sent by the tracker).

Which chunk to request first? **Rarest first**: the chunks that have the fewest copy among all the neighbors.

To which neighbor should she sent her chunks? Send data to the neighbors that are currently supplying her data at highest rate (unchoked) and a randomly chosen peer (optimistically unchoked).



### 2.6 Video Streaming and Content Distribution Networks

By 2022, 82% of the Internet traffic is videos, and it's quite expensive!

#### 2.6.1 Internet Video

Consume huge amount of traffic and storage (10 Mbps for 4K).

Use compression to create multiple versions of the same video.

#### 2.6.2 HTTP Streaming and DASH

Video is stored at an HTTP server as an ordinary file with a specific URL.

The streaming video application periodically grabs video frames from the client application buffer, decompress and play the frame.

Problem: all clients receive the same encoding of the video despite their bandwidth difference.

##### DASH

Dynamic Adaptive Streaming over HTTP. The video is encoded into several different versions with different bit rate, each with a different URL. The server first send a **manifest file** with a list of URLs, one for each version of the video. The client dynamically requests video segments depending on the available bandwidth. 

#### 2.6.3 Content Distribution Networks

Challenge: distribute massive amount of video contents to users around the world.

A CDN manages distributed servers to store copies of videos and attempts to direct each user request to the best CDN location.

* **Private CDN** is owned by the content provider, e.g. Google's CDN distributes YouTube videos.
* **Third-Party CDN** distributes content on behalf of multiple content providers.

Two server placement philosophies.

* **Enter Deep**: deploying server clusters in access ISPs all over the world. Highly distributed, challenging to manage.
* **Bring Home**: build large clusters at a smaller number (e.g. 10) of sites and connect them to IXPs. Each CDN stores the videos that are frequently requested by the corresponding region.

##### CDN Operation

![[Pasted image 20230113154024.png]]

1. The user sends a DNS query for video.netcinema.com
2. NetCinema's DNS server observes the string "video" and "hand over" the query to KingCDN and returns a hostname in KingCDN's domain.
3. The user send DNS query for a host in KingCDN and receives its IP address.
4. The user establish a TCP connection with the server at that IP address and issues an HTTP GET request for the video.

##### Cluster Selection Strategies

How do we select the CDN server?

1. Geographically closest. Problem: some users are configured to use remotely located LDNSs.
2. Periodic real-time measurements of delay between their clusters and clients.

*How do clusters know which videos are on which cluster?*

#### 2.6.4 Case Studies: Netflix, YouTube, Kankan

##### Netflix

Amazon cloud + own private CDN infrastructure.

Amazon cloud do:

1. Content ingestion: upload new movies to hosts in the Amazon cloud
2. Content processing: create different formats for each movie
3. Uploading versions to its CDN

When a user requests a video, the Netflix software determines a CDN to serve the user, and sends the IP address of that server.

Netflix uses *push cashing*, which pushes content into CDN during off-peak hours.

##### YouTube

* Google uses its private CDNs
* Uses pull caching cause the content is dynamic
* Uses HTTP streaming without adaptive streaming



### 2.7 Socket Programming

Two types of network applications:

1. Open: whose operation is specified in a protocol standard, such as HTTP
2. Proprietary: not openly published in an RFC

#### 2.7.1 Socket Programming with UDP

**UDPClient.py**

```python
from socket import *
serverName = '35.3.90.87'
serverPort = 12000
clientSocket = socket(AF_INET, SOCK_DGRAM) # IPv4, UPD
message = input("Input lowercase sentence:")
clientSocket.sendto(message.encode(), (serverName, serverPort))
modifiedMsg, serverAddress = clientSocket.recvfrom(2048)
print(modifiedMsg.decode())
clientSocket.close() # close the socket
```

**UDPServer.py**

```python
from socket import *
serverPort = 12000
serverSocket = socket(AF_INET, SOCK_DGRAM)
serverSocket.bind(('', serverPort))
print("The server is ready to receive")
while True:
    msg, clientAddress = serverSocket.recvfrom(2048)
    modifiedMsg = msg.decode().upper()
    serverSocket.sendto(modifiedMsg.encode(), clientAddress)
```

#### 2.7.2 Socket Programming with TCP

TCP is connection-oriented, a connection must be established before any communication. After the connection, both side can do both sending and receiving through the socket.

![[Pasted image 20230113161139.png]]

**TCPClient.py**

```python
from socket import *
serverName = '127.0.0.1'
serverPort = 12000
clientSocket = socket(AF_INET, SOCK_STREAM)
clientSocket.connect((serverName, serverPort)) # try to establish connection
sentence = input('Input sentence:')
clientSocket.send(sentence.encode())
modifiedSen = clientSocket.recv(1024)
print('From Server: ', modifiedSen.decode())
clientSocket.close() # close the socket, and hence the connection
```

**TCPServer.py**

```python
from socket import *
serverPort = 12000
serverSocket = socket(AF_INET, SOCK_STREAM)
serverSocket.bind(('', serverPort))
serverSocket.listen(1) # maximun number of queued connections
print('The server is ready to receive')
while True:
    connectionSocket, addr = serverSocket.accept() # create a new socket for this client. We are letting the OS to decide the port humber
    sentence  = connectionSocket.recv(1024).decode()
    capSen = sentence.upper()
    connectionSocket.send(capSen.encode())
    connectionSocket.close() # close the connection socket
```



### Problems

##### dig

A flexible tool for interrogating DNS name servers. It performs DNS lookups and displays the answers. Can use it to simulate how DNS works. `dig +norecurse @a.root-servers.net any gaia.cs.umass.edu`

It can also be used to know if a website is recently queried before by `dig bilibili.com` and observe the query time.
