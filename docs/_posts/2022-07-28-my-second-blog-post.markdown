---
layout: post
title:  "Data Transport - Problems & Solutions"
date:   2022-07-28 13:45:00 +0530
categories: network-fundamentals
---

Starting from First Principles,

    Q. What is the main job of networks?
    A. Data transport between applications on Nodes/hosts.

    Q. What problems does a language need to overcome to make
       the communication (me writing and the reader reading this) possible ?
    A.  1. The thoughts must be captured in a form that allows them to be         retrieved by the reciever.
        2. There must be some way of "managing errors in transmission or reception".
        3. There must be some way to "talk to 1 person", "a small group" or "all users" using a single shared medium, within a larger crowd.
        4. Finally, there must be some way to "control the flow of the conversation"

        In Short,

        - Marshalling the data.
        - Managing Errors
        - Multiplexing
        - Flow Control

        Q. What do we mean by Marshalling the data problem ?
        A. - Converting ideas into symbols and grammer that reciever will understand.
           - A language in network communications terms is called a Protocol.
           - A Protocol defines the metadata ( dictionary and grammar ) used to translate one kind of information into another.
            - Dictionary: What are the different fields a packet contains and What does it mean to me, the reader?
            - Grammar: How the packet is built, What values are contained in those fields in a packet and what do those values represent ?

            Q. What are the different ways to build this Dictionary(Fields in Packet) and Grammer(Values in those Feilds) ?
            A. Two possible ways:
                - Fixed Length Fields/Headers: ip address, mac address, mpls label
                - Type Length Value (Variable Length): TLVs contain Type Code, Length of the Data & Value(the data itself) and can be of various length.
            
            Q. What needs to be kept in mind while designing such a protocol ?
            A. - Resource Efficiency:
                - How much resources are required to encode/decode ?
                - How much metadata to include with the data itself for smooth operation without increasing too much overhead?
               - Flexibility:
                - How easy is it to change and implement by vendors ?
                - How does it maintain backwards compatibility with previous versions in lieu of such flexibility of changes ?

        Q. What do we mean by managing errors problem ?
        A. We need to ensure the idea/message is correctly transmitted from sender to reciever.
            - This can be further devided into two problems to be solved:
                1. Error Detection:
                    - Due to transmission media failures, memory corruption in switch, loss of power.
                    - Ways to Detect:
                        1. Parity Check - Can detect single bit flip.
                        2. CRC Check - Can detect multiple bit flips.
                2. Error Correction:
                    - Once detected, we have 3 possible options to fix or not to fix these errors:
                        1. Ignore it, let the higher layer protocol decide what to do about the missing information.
                        2. Transport system can signal the sender and let the sender decide what to do. (This involves protocol carrying some state information about the data sent)
                        3. Find where data is corrupted and attempt to recover it - known as FEC ( Forward Error Correction )
                            - This uses 2 Solutions:
                                1. Hamming Codes: Can detect more than 1 bit flips but not all. Used on upto 1Gbps links.
                                2. Reed-Solmon Codes: Can detect more flips than hamming codes. Used on 10Gbps links or above.
        
        Q. What do we mean by the Multiplexing problem ?
        A. - Allowing a common media to be used for conversations between many different pairs of senders and recievers.

            Q. So how do hosts communicate effectively over Computer Networks ?
            A. Multiplexing is achieved using addressing devices and applications:
                - Physical/Link-Level Layer - MAC addreses.
                - Host/Local Addressing Layer - IP addresses.
                - Process Level Addressing - Port Numbers.
                - Conversation Level Addresing - Session ID representing IP+Port pairs become unique conversations.

            Q. Types of communication situations/requirements ?
            A.  - 1 to 1 or Unicast. Only 2 speakers.
                - 1 to all or Broadcast. 1 Speaker multiple recievers.
                - 1 to many or Multicast. 1 Speaker and selective recievers.
                
                    Q. Problem Multicast must solve ?
                    A. 1. How do you discover discover which device needs a copy of the traffic ?
                       2. How to you determine which device in the network will replicate the packet ?
                       3. How will be a loop free path built for this multicast traffic ?
                    
                    Q. What if there are multiple speakers who can both deliver Multicast to a group of selective hosts ?
                    A. Two problems arise with differet solution needed for each:
                        1. Both speakers use different addressing 
                            - Use Load Balancer.
                            - Not the most optimal.
                            - Introduces complexity.
                        2. Both speakers use same addressing
                            - Use Anycast.
                            - Used by DNS, Facebook, Geolocation based Content Caching, CDNs like Netflix, AmazonPrimeVideo( Content Delivery Networks)
                    
            Q. What is the problem of Flow Control ?
            A. The ability to be able to say with certainity that the reciever is actually receving and processing the information before the sender transmits more.
                - Q. What if the 2 hosts are of different size, one speaks fast (10G Link) and other speaks slow(100Mbps Link)?
                A. If nothing is done, it would cause reciever buffer to overflow and packet being dropped.
                    - We need a feedback loop. The reciever being able to tell the sender to slow down.
                    - This means we need some sort of signaling, it can be of two types:
                        1. Implicit Signaling:
                            - More widely deployed.
                            - TCP-wait retransmit timer.
                            - IRTT + Random Delay
                            - Basically saying retransmit packet if Ack not recieved.
                            - Reciever sends nothing.
                        2. Explicit Signaling:
                            - Reciever explicitly tells the sender to slow down.
                            - In case of retransmission, reciever is able to tell the sender exactly what needs retransmission and what was recieved.
                
                - Q. How is Flow Control implemented ?
                A. Windowing + Implicit Signaling
                    - Most widely deployed Flow-Control mechanism.
                        1. Single Packet Window:
                            - Sender sends second packet only after last packet has been acknowledged.
                            - If not ack'ed and the retransmit timer expires resend the packet.
                            - Reciever does not send any signal/Ack for the corrupted/out-of-order/dropped packet.(Implicit nature)
                        2. Multiple Packet Window:
                            - A finite/Fixed window size.
                            - Window = No. of packets to send before pausing and waiting for acknowledgement before sending more.
                            - Following Types of Explcit Signaling are used:
                                1. Positive Ack: Only ack for recieved packets.
                                2. Negative Ack: Only ack for what is not recieved.
                                3. Selective Ack: Positive + Negative
                                4. Cumulative Ack: If Ack recieved for packet 10, assume 0-9 must have been recieved.
                        3. Sliding Window:
                            - Sender reciever agree on a stack/implementation defined window size and scale it to avoid buffer-overflows on the reciever side causing packets to be dripper.
                    
                    Q. What if two identical packets are recieved ?
                    A. No two packets are identical.
                        - Each packet has a Sequence No. field set by the sender.
                        - Reciever keeps record of what Seq. No it has seen so far in the current session.
                        - Reciever can determine if it sees a repeated Seq. no, in which case it can discard/ignore the packet.
                    
                        
            


    



    