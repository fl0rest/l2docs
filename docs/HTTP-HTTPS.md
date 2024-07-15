### HTTP

Hypertext transfer protocol is a [protocol](https://developer.mozilla.org/en-US/docs/Glossary/Protocol) used for transferring HTML documentations over the internet. It is a client-server protocol, which means requests are initiated by the recipient, usually the Web browser. Clients and servers communicate by exchanging individual messages. The messages sent by the client, usually a Web browser, are called _requests_ and the messages sent by the server as an answer are called _responses_.

To display a Web page, the browser sends an original request to fetch the HTML document that represents the webpage. A server will accept the request and access port 80 which is reserved for HTTP requests and serve the file to the user-agent. The Web browser then combines these resources to present the complete document, the Web page. Scripts executed by the browser can fetch more resources in later phases and the browser updates the Web page accordingly.

HTTP is a stateless protocol:

HTTP that is the actual transport protocol between the server and the client -- is "stateless" because it remembers nothing between invocations. **EVERY** resource that is accessed via HTTP is a single request with no threaded connection between them. If you load a web page with an HTML file that within it contains three `<img>` tags hitting the same server, there will be four TCP connections negotiated and opened, four data transfers, four connections closed. There is simply no state kept at the server **at the _protocol_ level** that will have the server know anything about you as you come in.

Almost anything you want to do other than viewing static web pages will involve sessions and states. When HTTP is used for its original purpose (sharing static information like scientific papers) the stateless protocol makes a lot of sense. When you start using it for things like web applications, online stores, etc. then statelessness starts to be a bother because these are inherently stateful activities. As a result people very rapidly came up with ways to slather state on top of the stateless protocol. These mechanisms have included things like cookies, like encoding state in the URLs and having the server dynamically fire up data based on those, like hidden state requests

### HTTPS - hypertext transfer protocol secure. 

The issue with HTTP is that it does not encrypt the packages when a client is communicating with the server. This means that anyone who has an access to that network can read the information that is being transmitted, such as passwords, credit card numbers etc. In order to protect users from man-in-the-middle attacks, we use HTTPS. 

HTTPS uses an TLS/SSL encryption in order to randomize the text being transferred and hide the contents of the communication. This type of security system uses two different keys to encrypt communications between two parties:

1. The private key - this key is controlled by the owner of a website and it’s kept, as the reader may have speculated, private. This key lives on a web server and is used to decrypt information encrypted by the public key.

2. The public key - this key is available to everyone who wants to interact with the server in a way that’s secure. Information that’s encrypted by the public key can only be decrypted by the private key.

HTTPS uses port 443

#### HTTP RESPONSE/STATUS CODES:

All HTTP response status codes are separated into five classes or categories. The first digit of the status code defines the class of response, while the last two digits do not have any classifying or categorization role. There are five classes defined by the standard:

- _1xx informational response_ – the request was received, continuing process
- _2xx successful_ – the request was successfully received, understood, and accepted
- _3xx redirection_ – further action needs to be taken in order to complete the request
- _4xx client error_ – the request contains bad syntax or cannot be fulfilled
- _5xx server error_ – the server failed to fulfill an apparently valid request

Common response codes:

- `200` - Ok
- `300` - Multiple choices
- `301` - Moved Permanently
- `302` - Found
- `400` - Bad request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `408` - Request Timeout
- `418` - I'm a teapot
- `429` - Too many requests
- `444` - No Response
- `500` - Internal Server Error
- `502` - Bad Gateway
- `503` - Service Unavailable
- `504` - Gateway Timeout
