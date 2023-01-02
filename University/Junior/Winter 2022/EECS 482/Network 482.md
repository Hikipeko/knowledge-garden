![[Pasted image 20221224133059.png]]

#### Network Interface Card 网卡

A computer hardware components that connects a computer to a computer network.

Assigned a unique MAC (media access control) address.

#### Socket

Virtual network interface card, named by port number.

##### UDP

IP + sockets


##### TCP

IP + sockets + reliable, ordered streams

#link computer networks

**How to know \# of bytes to receive?**

1. Convention (e.g. specified by protocol)
2. Specified in header
3. End-of-message delimiter
4. Sender closes connection

#### Client-Server

A model in the web world. The client make requests, the server provides some centralized service.

**Thread Pool**

Lower overhead of creating threads.

```c++
int server() {
	while (true) {
		accept request;
		assign a thread for the request;
		sleep(0.1);
	}
}
```

#### Remote Procedure Call (RPC)

Hide complexity of message-based communication from developers. Make remote communication as if you are calling function in your local machine.

![[Pasted image 20221224142518.png]]

**Client Stub**

1. Construct message with function name and parameters
2. Send request to server
3. Receives response from server
4. Returns response as result of function call to client

**Server Stub**

1. Receives request
2. Invoke function call
3. Construct response with return value
4. Sends response to client stub

Pass a pointer? Different address space! So just pass the whole object.

Note the endianness, since network transmission uses big-endian.




