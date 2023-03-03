# Network Layers
* **Application Layer** -- 
		High level, application specific protocols like HTTP, DNS, SSH, etc.
* **Transport Layer** -- 
		TCP and UDP. Provides some guarantees on delivery and adds nice features for programmers.
* **Network Layer** -- 
		This layer is responsible for routing data between networks and across the internet. Its main purpose is to address packets and determine the most efficient path for data to travel between networks.
* **Link Layer** -- 
		How are packets sent to the next node in a network.
* **Physical Layer** -- 
		How do bits get turned into electrons or photons or radio waves.


# Delays 
## 1. Physics/Propagation Delay
1. Depends on distance travelled
2. speed of “link” (radio vs fiber optic vs electrical)   

Delay = $scale * distance / speed of light$.
Control delay for distance to put data center across country so clients can find closet server.
Shortest distances mean less delay, and **fiber optics/radio based media are going to have lower propagation delays than electrical signals.**
==Control scale - change the wire.==

## 2. Processing Delay - Device(Switch, Router) decide what to do with packet, usually look at the header small bit
> This often involves looking at some of the packet's header data (destination or other metadata) and perhaps a table lookup.
1. Time, Devices, Spend, Examining, Packets.

O(nanoseconds).
Constant speed. Get faster device.
## 3. Transmission Delay - Time to convert bits to physical signal
1. Time to convert bits to physics. Electrical cable, etc..
2. Depends on devices (transmission rate), Fiber | ethernet internet - 1gbps, Wifi - 54Mbps, Dial up - 56Kbps
3. Depends on package size

Delay = $PacketSize / DeviceRate$

## 4. Queueing Delay (Fixed Size) - Depend on how busy the network is
1. Queue fill up when incoming traffic rate > outgoing rate. If the size is full, the incoming packets dropped.
2. Unpredictable. (How busy the network is + What i’m doing + Anybody else)

Delay = $Time thepacketsSpendInQueue$    
Incoming data rate = $avgPacketSize * avgNumOfPackets / Seconds$
Traffic Intensity (Outgoing data rate) = $IncomingDataRate / Transmission Rate (Outgoing rate)$

==If traffic intensity > 1 for a period time, queue will fill, packets dropped.==
==If traffic intensity < 1, the queues are getting shorter.==

## Measuring Delay
We don't know which path the packets would take, and don't control more than 2-3 devices along the path.

**What can we measure?**
%%Give images%%

Using round trip time method, also called (RTT).
The time take us to send a packet to another host and receive a response from it.
RTT = $host2 Processing (around(0)) + 2 * (4 Delays For Each Hop)$
EX: 100, 120, 110, 140 ms. Variability due to Queue delay

propagation ~= estimate based on distance.
Transmission delay vary packet size

**Measure just the network part. → ICMP, known as Ping. Host1 send ping and Host2 send pong back.**

### Neat IP header trick

header has field “time to live”

Number of hops the packet can go

many devices send a response when TTL hits 0

Tools: Trace-route - send 3 packets with TTL of 1 toward host. Same thing to 2, 3, 4 until host2(stops)

“*” means Don’t want to or don't have to or avoid spammer, so not sending response bc TTL.

# Application layer
This layer is responsible for providing services to end-users, such as email, web browsing, and file transfer. Its main purpose is to enable communication between applications on different devices, using standard protocols such as HTTP, FTP, and SMTP.

Application layer designer need to decide which transport layer to use. (UDP, TCP) based on application needs:
* Reliable data transfer: All data sent needs to be received in order.
	 TCP does this by coordinating between sender/receiver and resending dropped packets. But this would causing delay because application might waiting on resent packets.
	 
	 **UDP don't guarantee**. It delivers packets to the application layer as soon as they arrive. So if choose to use UDP, application is responsible for dealing with lost or reordered packets.
	 
	 **Bit Errors:** Using **checkSum** to packets to detect if there are any bit flips. If sender want to know if data has received or corrupted. The receiver need to send **Acknowledgment(ACK)** or **Negative Acknowledgement(NAK)** back to the sender. If sender gets the NAK, sender needs to keep sending until it gets an ACK.
	 
	 ***What happens if the ACK/NAK gets corrupted? What should the sender do?***
	 In TCP, each packet is numbered. The ACK message contains the sequence number of the last received packet. If the senders gets an ACK that does not have the sequence number of the last sent packet, it resends it.
	 ![ACK](/CS6014/Network/Images/ACK.jpeg)
	 ACK can get lost. The sender shouldn't keep waiting forever. Need to use **Timeout**, if sender did not receive ACK before timeout then resend the segment.
	
	 Above solutions do not provide RDT, but requires at least 1 RTT per packet. Since we need to receive an ACK before we can send a new packet, we'll be spending a LOT of time waiting on ACKs!
	 Pipelining means having many un-ACKed packets in flight simultaneously. This will complicate the end points...
	 Method: 
	 ==Go Back N (GBN)
	 Selective Repeat==
	 ![Selective Repeat](/CS6014/Network/Images/Selective%20Repeat.png)

* Delay concerns: data needs to be received within a given time.
* Data rate concerns: data needs to be transmitted at a particular rate.

If care about more reliable and latency, then use TCP.
If care about more Latency and data rate, then use UDP.

**Port numbers below 1024 are typically privileged so server processes listening on them usually run with elevated privileges.
Unprivileged clients usually get assigned an arbitrary, high numbered port.**

Host: IP Address
Process: Port Number
**Clients must specify the IP + Port of the server they want to connect.
Servers must specify the port they want to listen on.**
Different applications have different requirements for data transfer. These include:

*   **Reliable Data Transfer** -- all bytes sent over the connection should be received in order without loss
*   **Thoughput** -- the connection needs to maintain a minimum amount of data send/received per second
*   **Timing** -- the application may/may not be tolerant to network delays

## Example of application protocols:
* HTTP
	* Test based one-shot request/response model.
	* Using TCP bc reliable is more important than lowering latency.
	*  HTTP proxy server. It acts as both a client and server. It listens for client requests, and either forwards the request to the requested server, or returns results from its own cache.
	* A content distribution network is a similar, but more transparent concept.
	
* Mail
	 The Simple Mail Transfer Protocol is a protocol for sending mail messages. A client will send an email to their mail server, which will connect to the recipient's mail server and send the message to that server.
	 ==Note, the recipient usually has to ask their mail server for messages (they're not sent to the user's machine automatically).==
	 A mail server will listen for SMTP connections and either store the messages it receives in a user's mailbox (if they are the user's mail server), or forward them to another mail server closer to the user's mailbox.
	 SMTP is a text based protocol that uses TCP (reliable transfer is important!). Unlike HTTP, data is generally sent from the client to the server.
* Bittorrent
	 Bittorrent is a P2P protocol for sharing files. A user has a list of peers with whom they'd like to communicate. The users request file chunks from peers, and listen for requests for chunks from peers. The bittorrent protocol describes how to pick peers to ask, and who to send data to. In this protocol all the clients are essentially both clients and servers.
* **Domain Name System (DNS)**
	* DNS uses UDP. 
	* DNS is a binary protocol for quick machine processing and reducing packet size.
	* DNS also called "resolver" and clients send "queries" to the resolver.
	 DNS cache query result with a time limit (TTL). It will respond to the query if DNS have the answer in the cache. If not in cache, DNS perform "recursive query" by asking another resolver for the answer. Else, DNS perform "iterative" query to find the answers.
	 For example: 
	 If try to connect hostname (google.com). The DNS Server will find out the IP Address in order to start sending the packets.
	 DNS maps hostnames (shell.cs.utah.edu) to ip addresses (23.34.45.56). This is an important task as IP addresses can change frequently and are difficult for humans to remember. The DNS system is built on a hierarchical network of servers.

# Transport Layer
**TCP is a full-duplex (client and server can send at any time), point-to-point (communication between 2 processes, no "broadcast"), connection-oriented (stateful) protocol.**
Transport layer provides process to process communication via port numbers.
It also requires a handshake to connect. UDP can provide higher throughput and lower startup delay at the cost of data reliability.

TCP header (is the first part of the data field in the encapsulating IP packet) has bookkeeping information for the TCP connection including:
* Source and destination port numbers (16 bits each)
* Sequence number and ACK numbers
* Flags (SYN, ACK, RST, FIN)
* "Receive Window"

***Establish connection***
TCP begins with **"Three way handshake"**:
1. **Step 1 (SYN):** In the first step, the client wants to establish a connection with a server, so it sends a segment with SYN(Synchronize Sequence Number) which informs the server that the client is likely to start communication and with what sequence number it starts segments with.
2. **Step 2 (SYN + ACK):** Server responds to the client request with SYN-ACK signal bits set. Acknowledgement(ACK) signifies the response of the segment it received and SYN signifies with what sequence number it is likely to start the segments with.
3. **Step 3 (ACK):** In the final part client acknowledges the response of the server and they both establish a reliable connection with which they will start the actual data transfer.

## Fast Retransmit
The sender expects 1 ACK per segment, with ACK number of $segmentNumber + segmentLength + 1$. If it gets more than one of these, it means either packet loss, or packet reordering. Once it gets 3 duplicates, rather than wait for a timeout, the sender will *fast retransmit* and immediately resend the packet starting with that sequence number. If only one packet in a stream is lost, this will fill in the gap, and the next ACK will have an ACK number for the end of the stream, and most of the packets will not have to be retransmitted.

## Delayed ACK
For lots of protocols, a host will respond quickly after receiving a message. If the host ACKs the packet immediately, then the'll send a data segment right away (with the same ACK number), so the empty ACK segment wasn't really necessary.

To deal with this TCP recommends using a delayed ACK. A receiver can react differently to a segment based on what's happening:

-   Receive an in order packet, no pending ACKs -- wait up to 500ms to ACK
-   Receive an in order packet with a pending ACK -- ACK now with the ACK number for the most recent     packet.
-   Out of order segment with high sequence number (gap created) -- immediately send duplicate ACK for the last in-order segment
-   Receive segment filling in the beginning of the gap -- ACK it immediately

This complements the sending side fast retransmit scheme to fill in gaps. It also reduces the number of messages sent for quick responses from the receiver.

## Timeouts
TCP estimates the RTT for packets using a moving average. Each time a node gets an ACK, it updates the estimated RTT as $estimate = ( 1 - alpha)_estimate + alpha_packetRTT$ using an alpha of 1/8. This uses the most up to date data, but smooths it out.

TCP endpoints also track the variance of this estimate using a similar formula: $deviationEstimate = (1 - beta) * deviationEstimate + beta * abs(packetRTT - estimate)$. The beta value used is 1/4.

The timeout interval is the $estimatedRTT + 4*deviationEstimate$. This gives a reasonable buffer time after the expected RTT while being in the right order of magnitude.

## ==Flow control==
In order to make sure the sender doesn't overwhelm the receiver, the sender must implement flow control and slow the rate of transmission if the receiver is struggling to keep up.

​Sender reduce it sending rate, so the receiver can keep up. It makes sure that the receiver will not be overwhelmed with data.

* It will not send any more data as the receive buffer indicates the capacity.
* The receiver will not be able to process any more data.
* The sender can never have more data in flight than the last rwnd value it has received.

## ==Congestion control==

is a mechanism that limits the flow of packets at each node of the network.

The way to know if needed to slow down the transmission is **when losing packets** (means congestion).

If received the acknowledged packets mean the network is good, so can speed up the transmission.

There are three phases that TCP uses **"end-to-end"** for congestion control: slow start, congestion avoidance, and fast recovery: **(Described by 4 words (AIMD): additive increase, multiplicative decrease.)**

1. Slow start – Basically, the sender sends the packet and exponential increases the number of packets until it reaches a threshold. If the connection gets 3 duplicate ACKs, it switches to “fast recovery” state. And if the cwnd becomes greater than ssthresh, then the connection switches to "congestion avoidance” mode.

2. Congestion Avoidance – Sender increases its transmission rate more slowly. The purpose of doing that is to add 1MSS into every RTT so that ACK will result in a slow, linear increase.  
If there is timeout, switch to slow start mode.

If there are many duplicates, switch to fast recovery mode.

3. Fast recovery: Sender found lost packets and resends them w/o waiting for a timeout. However, if there are triple duplicates ACKs, then the sender is still getting packets, but just lost one. In this case, we do not need to dramatically reduce our sending rate. To avoid delays, the sender waits before resending packets. Then, it temporarily reduces the transmission rate to avoid further congestion.

## Quick UDP Internet Connections (QUIC)
> Since TCP is implemented in middle-box hardware/firmware + OS kernels, so Google want to update will be costly.
* It also provided reliable data transfer and some flow/congestion control features like TCP.
* Multiple "stream" are supported with one QUIC connection, and dropped packets only delay the stream that they're part of.
* The initial QUIC handshake is like the TCP + TLS handshakes, but when reconnecting to a server you "know" clients can send data in the first packet.
* Since its built on UDP, the implementation of QUIC can be in user mode code, not the kernel.

QUIC includes a client ID in its header so that traffic can be correctly identified even if IP address or port change.

QUIC tries to reduce handshake delays. The first time a client connects to a server, it does a 3-way handshake to establish a connection + encryption agreement. For subsequent connections, it can send an encrypted request with no handshake.

# Network Layer

# Link Layer

# Physical Layer