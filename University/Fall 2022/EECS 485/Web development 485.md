## L2 Static Pages

WWW is a service provided by the Internet.

##### Markup languages

Markup language is used for data presentation. E.g. HTML (HyperText Markup Language), CSS, Markdown, TEX

**Hypertext**: text with embedded links to other documents.

##### Request response cycle

Static pages: no programming language on the server, same content for each request, only HTML/CSS. Static file server serves static pages.

See [[Application Layer 489#2.2.1 Overview of HTTP|HTTP]] from EECS 489.



## L3 Server-side Dynamic Pages

##### Server-side dynamic pages

Server response is the output of a function (usually an HTML file). Templates are a common way to generate server-side dynamic pages. Data are queried from database (CRUD). 

##### Dynamic page URL routing

URL routing is a table mapping URL to function-reference. Dynamic page allows multiple URLs being mapped to the same function.

```python
@app.route('/u/<username>')
def show_user(username):
	return "hello {}!".format(username)
```

URL slugs: usually last in URL, often an input to the server function.

**Python decorator**

In Python, functions are first class objects.

Use decorators to register functions



## L4 Sessions

##### Session

* Goal: maintain state with stateless HTTP
* A session is an interaction between the site and a user, explicitly opened and closed by the server.
* Application layer session is implemented by framework
* Explicitly opened and closed by the server
* Common practice: use small amount of data (username, session ID) as cookie and use session ID for database lookup

##### Cookies

* Implementation of session
* Small files on client machine, set/modify by the server.
* **Content**: name, value, domain, path, expiration,  SameSite
* Sent as part of HTTP headers
* Encrypted cookie transfer: use [[Web and Network Security 388#Cookie Scope|secure]], to only allow https cookie transmission

##### Third-party cookies

Cookie that has a different domain. Often used (by Google, Facebook, etc.) to track a user access different websites. [Check what you sent to trackers](https://panopticlick.eff.org/).



## L5 Encryption

Confidentiality, sender authenticity, message integrity, anonymity

##### Symmetric encryption

* Both side need the same key (how to distribute?) #link 475
* [[Security Fundamentals 388#AES|AES]]
* Fast to compute
* Provides confidentiality and message integrity

##### Asymmetric encryption

* AKA public key cryptography
* Each part has a private key and a public key

**Combination**

Use asymmetric encryption establish a session private key for symmetric encryption.

See [[Web and Network Security 388#L9 HTTPS and the Web PKI|PKI]] from EECS 388. Use [TLS/SSL](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/). Two common production web servers are Nginx and Apache.

##### Cryptographic hash functions

Never store passwords in plain text. To protect against rainbow table attack tools such as John the ripper, we should *salt* the password.

```python
salt = uuid.uuid4().hex
m = hashlib.new('sha512')
m.update((salt + password).encode('utf-8'))
password_hash = m.hexdigest()
```



## L6 Web Security

![[Pasted image 20230301155038.png|350]]

See [[Web and Network Security 388]].

#### Network attacks

* **Masquerading**: Pretend to be a website <- Public key
* **Replay**: Replay a message <- Use a unique sequence number (check number) for each message

#### Web attacks

* **Sybil**: uses a single node to operate many active fake identities. Solution: raise cost of creating a user account
* **De-anonymization**: recover identifiers from de-identified data



## L7 REST APIs

Representational State Transfer. REST is a set of architectural constraints. It is a collection of principles and best practices. When a client request (HTTP) is made via a RESTful API, it transfers the state of the resource (JSON data in HTTP response payload).

**JSON**: JavaScript Object Notation, a widely used data format in web.

**Tools**: `curl jq Httpie`

**Pagination**: list views should return a limited number of items

**Verbs**

* GET: return datum
* POST: create new datum
* PATCH: update part of a datum (e.g. change the picture in a post)
* PUT: replace the entire datum
* DELETE: delete datum

Response includes a copy of the entire modified object.

**Design principles**

* Uniform interface
	* Resource-based: each resource is identified by a URL
	* Manipulation of resource through representations: the state data is sufficient for client to make any modification
	* Self-descriptive messages: each message includes enough information to process the message
	* HATEOAS: Hypermedia as the engine of application state
* Client-server architecture
* Stateless: everything needed to handle the request is in the request itself
* Cacheable
* Layered



## L8 Client-side Dynamic Pages

##### JavaScript

* Interpreted, Python with C++ syntax
* node.js -> CLI for Google Chrome's V8 JS interpreter

##### DOM

* DOM is a data structure built from the HTML. In the DOM, everything is a node (element nodes & text node), they from a DOM tree.
* JavaScript runs in the client's web browser and modifies the DOM.
* The `document` API represents the web page as a Document Object Model (DOM)
* When the DOM changes, the rendered page changes

##### Event-driven programming

```javascript
<button onClick="hello()" type="button">
	Click Me!
</button>
<div id="JSEntry"</div>
<script>
function hello() {
	const n = document.getElementById('JSEntry');
	n.innerHTML = 'Hello World!';
}
<\script>
```

The flow of program is determined by events (e.g. onClick), which is useful for GUIs like web applications.

**Callback functions**

* A main loop listens for events and executes a callback function
* A callback function is a function waiting to be executed (e.g. `hello()`)
* In HTML, we register our function as an event handler.

**Execution model**

* **Event table**: A map from events to functions
* **Event queue**: Store messages to be handled
* **Stack**: Execute function calls
* **Heap**: Store objects, including functions

##### JS data model

Dynamically typed. Primitives and objects are JS's abstraction for data

**Prototypes**

Objects have properties (values associated with an object) and a prototype (the mechanism by which JS objects inherit features from one another). All objects inherits the properties and methods from their prototype (another object).

##### Pitfalls

 * Always use `===` and `!==`
 * Never simply assign values (e.g. `x = 5`), which creates a global variable
 * Never use `var`, use `let` or `const`
 * Never use `for-in` loop, use `forEach` or `map`



## L9 Client-side applications

##### JavaScript + REST APIs

* Link client-side dynamic pages to server-side dynamic pages via a REST API.
* JS makes a REST API request and then modifies the DOM using data from the response.

```javascript
function showUsers() {
    const users = /* ... */ ;
    users.forEach((user) => {
        const n = document.createElement('p');
        const s = '${user.username} has ${user.snippets.length} snippets';
        const t = document.createTextNode(s);
        n.appendChild(t);
        entry.appendChild(n);
    });
}
```

**Fetch API**

* Provides an interface for HTTP requests.
* Return a Promise
* Fulfilled when HTTP headers arrive

##### Closure

**Inner function**: we can create function inside another function at runtime.

* The inner function has access to outer function's variables (context) by storing a pointer to the outer function's frame.
* A closure is a function with data attached.
* Closure is important in web dev since callback functions are everywhere. They need to remember their context.

**Anonymous functions**

Arrow functions `(response) => {...}`

##### Frameworks and React

Problem: DOM acts like a giant global variable, and its operation is slow.

Solution: use a framework such as React, Vue instead of using a library such as jQuery



## L10 Asynchronous Programming

Problem: synchronous requests are slow

Solution: send both requests right away

**Promise**

* Promise represents a value that will be available in the future.
* Promise handler (provided by `.then()`) runs when Promise is fulfilled (callback).
* Promise handler's Input is the Promise's fulfilled value.

Problem: different response order -> different page display order

Solution: use placeholders, i.e. create placeholder `<div>` in the right order

**Promise chain**

```javascript
function show_posts() {
    const urls = [/* ... */];
    const entry = document.getElementById('JSEntry');
    urls.forEach((url) => {
        const div = document.createElement('div');
        entry.appendChild(div);
        fetch(url)
            .then(response => {
	            // also returns a Promise
            	return response.json();
            })
            .then(post => {
            	updateDOM(post, div);
            });
    }
}
```
