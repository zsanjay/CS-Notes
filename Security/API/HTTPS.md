https://www.cloudflare.com/learning/ssl/what-is-https/
## What is HTTPS?

Hypertext transfer protocol secure (HTTPS) is the secure version of [HTTP](https://www.cloudflare.com/learning/ddos/glossary/hypertext-transfer-protocol-http/), which is the primary protocol used to send data between a web browser and a website. HTTPS is encrypted in order to increase security of data transfer. This is particularly important when users transmit sensitive data, such as by logging into a bank account, email service, or health insurance provider.

Any website, especially those that require login credentials, should use HTTPS. In modern web browsers such as Chrome, websites that do not use HTTPS are marked differently than those that are. Look for a padlock in the URL bar to signify the webpage is secure. Web browsers take HTTPS seriously; [Google Chrome and other browsers flag all non-HTTPS websites as not secure.](https://www.cloudflare.com/learning/ssl/why-use-https/)

![Google Chrome Security Example](https://www.cloudflare.com/img/learning/security/glossary/what-is-https/not-secure.png)

## How does HTTPS work?

HTTPS uses an [encryption](https://www.cloudflare.com/learning/ssl/what-is-encryption/) protocol to encrypt communications. The protocol is called [Transport Layer Security (TLS)](https://www.cloudflare.com/learning/ssl/transport-layer-security-tls/), although formerly it was known as [Secure Sockets Layer (SSL)](https://www.cloudflare.com/learning/ssl/what-is-ssl/). This protocol secures communications by using what’s known as an [asymmetric public key infrastructure](https://www.cloudflare.com/learning/ssl/how-does-public-key-encryption-work/). This type of security system uses two different keys to encrypt communications between two parties:

1. The private key - this key is controlled by the owner of a website and it’s kept, as the reader may have speculated, private. This key lives on a web server and is used to decrypt information encrypted by the public key.
2. The public key - this key is available to everyone who wants to interact with the server in a way that’s secure. Information that’s encrypted by the public key can only be decrypted by the private key.

5 Ways to Maximize Security & Performance

[Get the eBook](https://www.cloudflare.com/lp/five-ways-maximize-performance/)

Everywhere security for every phase of the attack lifecycle

[Read the ebook](https://www.cloudflare.com/lp/everywhere-security-ebook/)

## Why is HTTPS important? What happens if a website doesn’t have HTTPS?

HTTPS prevents websites from having their information broadcast in a way that’s easily viewed by anyone snooping on the network. When information is sent over regular HTTP, the information is broken into packets of data that can be easily “sniffed” using free software. This makes communication over the an unsecure medium, such as public Wi-Fi, highly vulnerable to interception. In fact, all communications that occur over HTTP occur in plain text, making them highly accessible to anyone with the correct tools, and vulnerable to [on-path attacks](https://www.cloudflare.com/learning/security/threats/on-path-attack/).

With HTTPS, traffic is encrypted such that even if the packets are sniffed or otherwise intercepted, they will come across as nonsensical characters. Let’s look at an example:

#### Before encryption:

```
This is a string of text that is completely readable
```

#### After encryption:

```
ITM0IRyiEhVpa6VnKyExMiEgNveroyWBPlgGyfkflYjDaaFf/Kn3bo3OfghBPDWo6AfSHlNtL8N7ITEwIXc1gU5X73xMsJormzzXlwOyrCs+9XCPk63Y+z0=
```

In websites without HTTPS, it is possible for Internet service providers (ISPs) or other intermediaries to inject content into webpages without the approval of the website owner. This commonly takes the form of advertising, where an ISP looking to increase revenue injects paid advertising into the webpages of their customers. Unsurprisingly, when this occurs, the profits for the advertisements and the quality control of those advertisements are in no way shared with the website owner. HTTPS eliminates the ability of unmoderated third parties to inject advertising into web content.

![Third-party content injection](https://www.cloudflare.com/img/learning/security/glossary/what-is-https/third-party-content.png)

Image from [Ars Technica](https://arstechnica.com/information-technology/2015/08/atts-free-wi-fi-hotspot-injects-extra-ads-on-non-att-websites/)

For a full list of benefits HTTPS provides, see [Why use HTTPS?](https://www.cloudflare.com/learning/ssl/why-use-https/)

## What port does HTTPS use?

HTTPS uses port 443. This differentiates HTTPS from HTTP, which uses port 80.

(In networking, a port is a virtual software-based point where network connections start and end. All network-connected computers expose a number of ports to enable them to receive traffic. Each port is associated with a specific process or service, and different protocols use different ports.)

## How else is HTTPS different from HTTP?

Technically speaking, HTTPS is not a separate protocol from HTTP. It is simply using TLS/SSL encryption over the HTTP protocol. HTTPS occurs based upon the transmission of [TLS/SSL certificates](https://www.cloudflare.com/learning/ssl/what-is-an-ssl-certificate/), which verify that a particular provider is who they say they are.

When a user connects to a webpage, the webpage will send over its SSL certificate which contains the public key necessary to start the secure session. The two computers, the client and the server, then go through a process called an SSL/TLS handshake, which is a series of back-and-forth communications used to establish a secure connection. To take a deeper dive into encryption and the SSL/TLS handshake, [read about what happens in a TLS handshake](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/).

## How does a website start using HTTPS?

Many website hosting providers and other services will offer TLS/SSL certificates for a fee. These certificates will be often be shared amongst many customers. More expensive certificates are available which can be individually registered to particular web properties.

All websites using Cloudflare [receive HTTPS for free](https://www.cloudflare.com/application-services/products/ssl/) using a shared certificate (the technical term for this is a multi-domain SSL certificate). Setting up a free account will guarantee a web property receives continually updated HTTPS protection. You can also explore our paid plans for individual certificates and other features. In either case, a web property receives all the benefits of using HTTPS.


## How does HTTPS work?

[Video](https://www.youtube.com/watch?v=j9QmMEWmcfo)

Hypertext Transfer Protocol Secure (HTTPS) is an extension of the Hypertext Transfer Protocol (HTTP.) HTTPS transmits encrypted data using Transport Layer Security (TLS.) If the data is hijacked online, all the hijacker gets is binary code. 


![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F0e18db0d-f511-4f85-bb58-388fce70d42e_2631x2103.png)

How is the data encrypted and decrypted?

Step 1 - The client (browser) and the server establish a TCP connection.

Step 2 - The client sends a “client hello” to the server. The message contains a set of necessary encryption algorithms (cipher suites) and the latest TLS version it can support. The server responds with a “server hello” so the browser knows whether it can support the algorithms and TLS version.

The server then sends the SSL certificate to the client. The certificate contains the public key, hostname, expiry dates, etc. The client validates the certificate. 

Step 3 - After validating the SSL certificate, the client generates a session key and encrypts it using the public key. The server receives the encrypted session key and decrypts it with the private key. 

Step 4 - Now that both the client and the server hold the same session key (symmetric encryption), the encrypted data is transmitted in a secure bi-directional channel.

Why does HTTPS switch to symmetric encryption during data transmission? There are two main reasons:

1. Security: The asymmetric encryption goes only one way. This means that if the server tries to send the encrypted data back to the client, anyone can decrypt the data using the public key.

2. Server resources: The asymmetric encryption adds quite a lot of mathematical overhead. It is not suitable for data transmissions in long sessions.

Over to you: how much performance overhead does HTTPS add, compared to HTTP?