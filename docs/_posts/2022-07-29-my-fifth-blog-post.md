---
layout: post
title:  "Higher Layer Transports - Internet Protocol"
date:   2022-07-29 12:29:00 +0530
categories: network-fundamentals IP
---

    To understand any data transport protocol, there are 4 crucial questions we must ask:
        1. How does it Marshal(Format) and Transport the data ?
        2. How does it provide Multiplexing ? (Ability to carry multiple streams of data on a single shared resource)
        3. How does it provide Error Control ? (Error Detection, Correction & Retransmission)
        4. How does it provide Flow Control ?

    Lets look at the Internet Protocol:
        - In middle 1970's in IEN Spec. by Jonathan B. Postel.
        - Originally called TCP (it combined the IP & TCP in one protocol)
        - Postel understood this is bad design (breaking the principle of layering).
        - IEN2 rewritten to seperate the two protocols.
        - Published as IP Protocol in Spetember 1981. (RFC 791 IPV4)
        - IPV4 address Space is 32-bits or 4.2 Billion IP's.
        - Since this was not enough, why ?
            - One device can use multiple IP's.
            - Aggregation wastes alot of IP's.
        - IPV6 address space has 128-bits or 3.4 x 10 to power of 38 IP's introduced in 1998.

    Q. So from the above 4 questions to ask from each protocol, how many does IP Answer in solving ?
        A. 1. Marshaling(Formatting) & Transporting Data
        2. Providing Multiplexing.
        3. & 4. are left to upper or lower layer protocols.

        Q. How does IP Marshal(Format) and Transport the data ?
        A. Purpose - IP Provides a base layer on which wide-array of Higher Protocols run. (Eg: TCP,UDP)

                Q. What are the problems to be solved by IP?
                A. 1. Run on alot of different Physical Media and Lower Layer Protocols (Ethernet), while,       presenting a consistent set of services to higher layer protocols(TCP,UDP)
                2. Adapt to wide variety of frame sizes provided by lower-layer.
                        - IP must fit-into the frame type of many different kinds of Physical layer protocols.

                Q. What is inter-layer discovery ?
                A. - Layer 2 of OSI Model or DataLink Layer (ETHERNET) uses MAC addresses.
                - Layer 3 of OSI Model or Network Layer (IP) uses IP addresses.
                - So inter-layer discovery is done in this case of mapping addreses between Layer 2 and Layer 3, Mac & IP, by ARP - Address Resolution Protocol.
                - This is where the notion of ARP being a Layer 2.5 protocol comes from.
                
                Q. Applications can send any size of data to network layer (IP), and 
                - IP needs to find out the Size of the Gates through which the packets it creates will pass - (Interface is the Gate and MTU is the size of the gate) through, 
                - from source to destinations, there might be many gates so it needs to find out the smallest gate size, 
                - so it can create packets that will fit the smallest gate along the path from source to destination, 
                - how does IP manage this ?
                A. IP Solves this problem with Fragmentation.
                    - Eg: Application sends 2000 octets to be sent by IP, IP Implementation will:
                        1. Determine the smallest MTU along the path. (Usually a configured or default value)
                        2. Break the information into multiple fragments based on the MTU. (Data+Header must fit the MTU)
                        3. Send the first fragment with an IPV6 optional header.
                        4. Set more Fragment bits set to 1. Signalling more fragements are on the way.
                        5. Set more Fragments bits to 0 on the last fragement, signalling this is the last fragment.
                
                Q. How bug a chunk of data can be given to IPV6 to handle?
                A. - Depends on the size of the OFFSET Field.
                   - Offset Field in IPV6 is 13 bits long.
                   - So, IPV6 can carry at most 2 to the power 14 octets of data in a single packet.
                   - Anything bigger than that will have to be sliced by higher-layer protocol.
                   - This is theoretical limit, actual size depends on the MTU.
                
                Q. How IP carries application data through a network that uses multiple link-types in the path ?
                A. - By rewriting the Lower Layer Headers ( Data Link Layer - Ethernet - Frame - Header ) where multiple media types might be connected. ( At each hop in the network )
                   - These devices were originally called GATEWAYs, now called ROUTERS, since they route packets based on the IP Header.
                
                Q. What are the different IPV6 Header Fields and what do they do ?
                A. 1. Version - Specifies IP version 4 or 6.
                   2. Traffic Class - Used for QOS during congestion. 6 bits TOS fields, 2 bits for congestion notification.
                   3. Flow Label - To tell the forwarding devices how to keep the packets withing a single flow/session on the same path in an ECMP.
                   4. Payload Length - In Octets - The amount of data being carried in the packet.
                   5. NEXT Header - Information about additional headers contained in the packet.
                   6. HOP Limit - Same as TTL in IPV4. Decremented by 1 by every router. To avoid endless looping of packets.
                   7. Options Header
                   8. Source Address
                   9. Destination Address
                   10. Data
                
                Q. How does IPV4 or IPV6 provide multiplexing ?
                A. Two ways:
                    1. Providing Large Address Space.
                    2. By providing a place in the IPV4/6 Header for Protocol Number, which allows multiple higher-layer protocols (TCP/UDP) to run over IP.

                    Q. How is addressing handled in IPV6 ?
                    A. - 128 Bits = 2 to power of 128 addresses - Enough IP's for every grain of dust on earth.
                       - Written as a series of Hexa Decimal numbers.
                       - Two points on Zero in IPV6:
                            1. Leading Zero's in each section are omitted.
                            2. A single long string of zero's can be replaced by a "::", only once in the address.

                        Q. What are the reserved addresses in IPV6 ?
                        A. - "::ffff/96" Reserved for IPV6 addresses "mapped-into" IPV6 address space.
                           - "fc00::/7" Reserved for Unique Local Addresses (ULA's)(Private Addressing - Non Routable on the internet).
                           - "fe80::/10" Reserved for Link Local Addresses.
                                - Automatically assigned on each interface.
                                - Only used over a single link.
                            - "::/0" Default Route.

                Q. 
                    
                