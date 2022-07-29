---
layout: post
title:  "Higher Layer Transports - Internet Protocol"
date:   2022-07-29 12:29:00 +0530
categories: network-fundamentals TCP IP
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
            
            Q. Applications can send any size of data to network layer (IP), and IP needs to find out the Size of the Gates through which the packets it creates will pass (Interface is the Gate and MTU is the size of the gate) through, from source to destinations, there might be many gates so it needs to find out the smallest gate size, so it can create packets that will fit the smallest gate along the path from source to destination, how does IP manage this ?
            A. IP Solves this problem with Fragmentation.
                - Eg: Application sends 2000 octets to be sent by IP, IP Implementation will:
                    1. Determine the smallest MTU along the path. (Usually a configured or default value)
                    2. Break the information into multiple fragments based on the MTU. (Data+Header must fit the MTU)
                    3. Send the first fragment with an IPV6 optional header.
                    4. Set more Fragment bits set to 1. Signalling more fragements are on the way.
                    5. Setting it to 0 on the last fragment.
            
            Q. How
            

    

