---
layout: pages
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
            A. - Rerource Efficiency:
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
            A. One 
            


    



    