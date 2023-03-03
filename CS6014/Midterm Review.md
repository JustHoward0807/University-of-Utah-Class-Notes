# The Basics
## What are the layers of the network stack and what are their purposes? 
### Physical layer

This layer is responsible for transmitting raw bits over a physical medium, such as copper wire or fiber optic cable. Its main purpose is to define the physical characteristics of the network, including voltage levels, cable types, and transmission rates.

### Link layer

This is the layer that communicates directly with the physical network hardware, such as Ethernet or Wi-Fi. It is responsible for getting data from the network layer onto the physical network and vice versa.

### Network layer

This layer is responsible for getting data from one device to another across a network. It uses IP (Internet Protocol) to identify devices and routers and determine the best path for data to take.

### Transport layer

This layer is responsible for ensuring that data is transferred between devices reliably and efficiently. Its main purpose is to ensure that data is delivered reliably and in the correct order, using techniques such as flow control and error recovery. The two most common protocols at this layer are **TCP** and **UDP**.

### Application layer

This layer is responsible for providing services to end-users, such as email, web browsing, and file transfer. Its main purpose is to enable communication between applications on different devices, using standard protocols such as HTTP, FTP, and SMTP.

## What are example protocols in each layer?

| Physical Layer | Link Layer | Network Layer | Transport Layer | Application Layer |
| -------------- | ---------- | ------------- | --------------- | ----------------- |
| Ethernet       | Ethernet   | IP            | TCP             | HTTP              |
| WIFI           | WIFI       | ICMP          | UDP             | FTP               |
| Bluetooth      | Bluetooth  |               |                 | SMTP              |
|                |            |               |                 | **DNS**           |
|                |            |               |                 | SSH               |

## What are the types of network delays and their causes?
### Physics/Propagation Delay
> This is the delay caused by physics. It is the time it takes the signal to physically travel between nodes.
1. Depends on distance travelled
2. speed of “link” (radio vs fiber optic vs electrical)   

### Processing Delay - Device(Switch, Router) decide what to do with packet, usually look at the header small bit

> This is the delay caused by a device (switch, router) performing processing to decide what to do with a packet.
1. Time, Devices, Spend, Examining, Packets.

### Transmission Delay - Time to convert bits to physical signal
> This is the delay caused by the packet leaving a device and entering the transmission medium
1. Time to convert bits to physics. Electrical cable, etc..
2. Depends on devices (transmission rate), Fiber | ethernet internet - 1gbps, Wifi - 54Mbps, Dial up - 56Kbps
3. **Depends on package size**

### Queueing Delay (Fixed Size) - Depend on how busy the network is
1. Queue fill up when incoming traffic rate > outgoing rate. If the size is full, the incoming packets dropped.
2. Unpredictable. (How busy the network is + What i’m doing + Anybody else)

==If traffic intensity > 1 for a period time, queue will fill, packets dropped.==
==If traffic intensity < 1, the queues are getting shorter.==

# Application Layer

## UDP vs TCP

### Similarity

-   Both TCP and UDP send packets of data over the internet.
-   Both protocols use IP (Internet Protocol) to route the packets to their destination.
-   Both protocols use port numbers to identify the application or service that should receive the data.(Application layer is about process-to-process delivery, port # are used to identify processes )

### Difference

-   TCP establishes a connection between two devices before data transfer begins, while UDP does not.
-   **TCP ensures reliable data transfer, while UDP does not guarantee delivery or order of packets.**
-   TCP has a higher overhead due to its connection-oriented nature, while UDP has a lower overhead.
-   TCP is commonly used for applications such as email, file transfer, and web browsing, while UDP is used for applications such as video streaming, online gaming, and DNS.

## RDT 3.0 details

RDT 3.0 is a protocol that provides reliable data transfer between a sender and a receiver over an unreliable channel, such as the Internet. It guarantees that all data packets are delivered to the receiver in the correct order and without any errors.

The sender and receiver both use a finite state machine that consists of three states: waiting, ready, and busy.

### Checksum

Used by the sender or receiver to detect bits flipped during a packet’s transmission, also used in UDP

### ACK # & SEQ #

-   Sender sends a packet with a SEQ number to the receiver.
    
-   Receiver receives the packet and sends back an ACK packet with the next expected SEQ number.
    
-   If the sender receives an ACK with the expected SEQ number, it sends the next packet with an incremented SEQ number.
    
-   If the sender does not receive an ACK after a timeout, it retransmits the packet with the same SEQ number.
    
-   If the receiver receives a packet with a SEQ number *(less/greater)* than the expected SEQ number, it discards the packet and sends back an ACK with the same expected SEQ number.

# Transport Layer
## What features are implemented at the transport layer?

-   Reliable data transfer: making sure that all data is delivered accurately and completely, without errors or loss
-   Flow control: controlling the amount of data sent by the sender so that the receiver can keep up and not get overwhelmed
-   Congestion control: preventing the network from becoming too congested by slowing down the sender if necessary
-   Multiplexing and demultiplexing: allowing multiple applications to use the same network connection by identifying which data belongs to which application
-  Transport layer provides process to process communication via port numbers.

## How can we achieve reliable data transfer (a bit of a misnomer) over an unreliable network?

-   Sequence numbers and acknowledgements: each packet is assigned a unique sequence number and the receiver sends an acknowledgement to the sender to confirm.
    
-   Retransmission of lost packets: if an acknowledgement is not received within a certain period of time, the sender will retransmit the packet.
    
-   Window-based flow control: the sender monitors the amount of unacknowledged data in transit and adjusts the rate of transmission to avoid overwhelming the receiver.

## What features does TCP provide beyond reliable data transfer?

-   Connection-oriented communication: a TCP connection is established between two endpoints before data is transmitted.
    
-   Full-duplex communication: data can be transmitted in both directions simultaneously.
    
-   Congestion control: TCP uses algorithms to detect and avoid network congestion, and adjusts the transmission rate accordingly.
    
-   Stream-oriented communication: TCP provides a continuous stream of bytes between endpoints, with no boundaries between packets or segments. This allows for more efficient use of network resources.

# Network Layer
## How do routers know where to forward packets?
Routers use the destination IP address in a packet's header to look up the best route in their routing table. It contains information about network topology and possible paths to different destinations. The router then forwards the packet to the next hop until it reaches its final destination.

## What is an autonomous system?
An autonomous system is like a big group of computers and other devices that are all connected to each other to form a really big network.
These ASs are usually owned and managed by big companies or organizations, like Google or universities. They use autonomous systems to help their computers talk to each other and share information more easily and quickly.

## What algorithms might we use to route within or between ASs
Dijkstra's algorithm is a faster algorithm that works for networks with non-negative edge weights.
Bellman-Ford's algorithm is slower but can handle networks with negative edge weights.

## How are IP addresses assigned to make life easier on routers?
The address divided into classes, and each class is further subdivided into smaller networks using a subnet mask. Routers use the network ID and subnet mask to determine how to forward packets, which allows them to make forwarding decisions.

## What is NAT, why do we need it, and how does it work?
NAT is used to allow multiple devices on a private network to share a single public IP address when connecting to the internet. It works by assigning private IP addresses to each device on the network and using a router to forward traffic to the right device.

# Link Layer
## What's the role of the link layer?
The link layer is responsible for communicating between devices that are directly connected to each other. It makes sure that data is sent and received correctly and can handle any errors or lost data that may occur.

## What is ARP and why do we need it?
ARP is used to find the physical hardware address of a device on the same local network. It works by sending requests to other devices on the same local network to ask for their MAC address. Once the MAC address is known, data can be sent directly to the destination device using that MAC address.

## How can we avoid collisions when communicating over a broadcast medium?
One way to avoid collisions when communicating over a broadcast medium is to use a protocol that implements collision detection and avoidance, such as CSMA. With CSMA/CD, devices listen to the medium before transmitting data, and if they detect that the medium is busy, they wait until it becomes free before transmitting. If two devices try to transmit data at the same time, a collision occurs, and the devices back off and try again after a random delay.

## What is a switch, and how are they different from routers?
Switch using a technique called "switching". They look at the MAC address of each device connected to them and use that information to send data only to the device that it's intended for.
Switching operate at the link layer and router works on network layer.

# Crypto
## What are the properties we want for secure communication?
1. Confidentiality *(Encryption)*: Communication should not be readable to anyone except the intended recipient.
2. Integrity *(Auth code)*: Message should not be changed or altered during transmission.
3. Authentication *(certificate)*: Make sure the person I'm sending the message with are the one I intended to chat with.
4. Non-repudiation *(Signature)*: Means the sender cannot deny having sent the message.

## What is a block cypher? What are their inputs/outputs? What are the primitives used to build one? What are examples of block cyphers?
A Block Cipher is a encryption algorithm that takes fixed-size block of PT and transform into same size block of CT.
Block cipher use **substitution** and **permutation** to build the algorithm.
* Substitution replace the letter or symbols with other letter and symbols.
* Permutation means shifting or changing the order of the letters or symbols.

## What is a stream cypher? How do they work? What are common examples of stream cyphers?
Stream Cipher is a encryption algorithm that encrypt PT one bit or one byte at a time, producing a stream of CT that is the same length as PT.
Stream Cipher take PT and combine (xor) with pseudorandom key stream to produce CT.
1. RC4 broken but widely used in the past. It generates key stream by keep swapping elements and xor with the PT.
2. ChaCha is another one. It also generate pseudorandom key stream but by applying set of key and a IV, then xor with the PT.

## What is a cryptographic hash function? How are they different from hash functions used for hash tables?
It take any input size and produce a fixed-size output. The output is unique, so small change might cause an avalanche effect.
They are different from hash function used for hash tables because they have additional security properties such as being one-way and being collision-resistant.
**One-way**: Impossible to know the original data from hash value.
**Collision-resistant**: Difficult to find two different inputs that produce same hash value.

## What is Diffie Hellman key exchange, and how does it work?
DH key exchange is for two parties to agree on a shared secret key over an insecure communication channel. The shared secret key can then be used for secure communication between two parties.

The protocol works as follows:

1.  Alice and Bob agree on a large prime number p and a base g, where g is a primitive root modulo p.
    
2.  Alice chooses a secret integer a, and Bob chooses a secret integer b.
    
3.  Alice computes g^a mod p, which she sends to Bob.
    
4.  Bob computes g^b mod p, which he sends to Alice.
    
5.  Alice computes (g^b mod p)^a mod p, which is the shared secret key.
    
6.  Bob computes (g^a mod p)^b mod p, which is also the shared secret key.

## What is RSA? What can you do with an RSA public and/or private key?
RSA is a fundamental tool in modern cryptography and is widely used in applications such as secure email, online banking, and e-commerce.
With an RSA public key, you can encrypt messages for the corresponding private key holder, and with an RSA private key, you can decrypt messages encrypted with the corresponding public key. This allows for secure communication between two parties, even over an insecure communication channel

In addition to encryption and decryption, RSA can also be used for digital signatures, which are used to verify the authenticity of a message or document.

## How are DH and RSA related/different?
While both Diffie-Hellman and RSA are used for secure communication, they are based on different mathematical calculation and are used for different purposes. Diffie-Hellman is used for key exchange, while RSA is used for encryption and digital signatures.

DH security of the algorithm is based on the difficulty of computing discrete logarithms in finite fields.
RSA key generated based on the mathematical properties of large prime numbers.

## What are block cypher modes of operation and why do we need them?
Block Cipher modes of operation provide additional security and help to protect the confidentiality of messages that are longer than the block size of the block cipher.

-   Electronic Codebook (ECB): This mode simply applies the block cipher to each block of the message separately. This mode is not very secure, because identical blocks in the message will be encrypted to the same CT.
    
-   Cipher Block Chaining (CBC): This mode XORs each block of the message with the previous block's CT before encrypting it. This makes it harder for an attacker to guess the content of the message. *CBC mode also requires an IV to start the encryption process.*
    
-   Counter (CTR): This mode uses a counter to generate a unique encryption key for each block of the message. The counter value is combined with a **nonce** and the key to generate a stream of pseudorandom bits and xor with the PT to produce the CT.
    
* Galois Counter Mode (GCM): GCM (Galois/Counter Mode) is a mode that combines both encryption and authentication. GCM uses CTR mode to encrypt the message and a special authentication tag to authenticate the message.

## What is a digital certificate? What are they used for?
It use to verify the identity of a person or organization. It contains informations such as the name, public key and the digital signature of a trusted third party.
It secure connections over the internet. When connecting to the website, the browser will checks the digital certificate to make sure it is authentic. This prevent attacker from intercepting information.

## What are some simple authentication protocols and how can they be compromised?
Use Shared Secret Based Protocol. Both the user and server have a shared secret and the server check if it matches.
One example of this shared secret based protocol is **"Challenge-response authentication protocol"**. The server sends a challenge to user which is random value or mathematical problem that the user must solve. The user respond with the answer and the shared secret, if it matches with the server, then granted access.

> Alice says "I'm alice" Bob says "here's a random number, R" Alice replies with f(Kab, R) Bob computes f(Kab, R) and verifies that Alice sent the right thing
> R is known as a "challenge" or a "nonce" and should be chosen randomly. f could be "Encrypt" (reversible, and bob's f would be "Decrypt") or something like HMAC (irreversible).
> Here are some issues with this protocol:
>  
> Alice doesn't know that she's actually talking to Bob. Trudy could pretend to be Bob, send R to Alice, then ignore Alice's reponse and impersonate Bob when talking to Alice.
> 
> If Trudy intercepts this protocol, she can do offline guesses for K (since she knows R and f(Kab, R). Offline guessing attacks are much easier to mount than online attacks.
> 
> If Bob ever gets compromised, the thief can pretend to be Alice later in the future.
> 
> After Alice authenticates, Trudy could cut Alice's connection and pretend to be her. This is called "session hijacking"