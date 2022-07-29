---
layout: post
title:  "Lower Layer Transports"
date:   2022-07-29 11:08:00 +0530
categories: network-fundamentals ethernet
---

Q. What do we mean by lower layers ?
A. The Physical and Data Link Layer in any of the previously discussed network protocol stacking models (DOD/OSI/RINA).

Let's learn a bit about the different mediums and data transport problems and solutions for those mediums.
    Available Mediums:
        - Physical(Wires)
        - Optical(Fiber)
        - Air(Wireless)
    
    Available Signals:
        - Electrical for Wired & Wireless
        - Light/Photons for Optical.

    - Physical Medium (Wires)
        - ETHERNET
            - Invented in 1970's by Bob Metcalf.
            - Initial "How's" of any protocol that interacts with Physical Medium is going to be
            in area of Multiplexing.

        Q. What is the Multiplexing problem ?
        A. On a shared medium, the entire shared medium is a single electrical circuit or wire.
            - Imagine its a single wire.
            - Any current you induce on the wire, will be detectable at all points on the wire.
            - So a signal sent by one host is received by all hosts.
            - What if all hosts starting speaking at once, can that be handled ?
            - There is no way to restrict listeners.
            - A network HUB is at the physical layer, its just a wire extender.
            - Meaning all recieved packets are sent to all hosts.
            - How to allow multiple hosts to talk to each other on a single medium.
            
            Solution 1: CSMA/CD - Carrier Sense Multi-Access/Collision Detection
                            - Host listens on the wire to hear any existing transmission. (Carrier sense portion)
                            - On hearing there is no one talking, host begins to send on the wire.
                            - Collisions can happen: Due to propogation delay.
                            - Host listens to its own signal on the wire at the time of sending.
                            - If interference is found by sender, Collision is detected.
                            - Once collision is detected, sender sends JAM signal on the wire, so all hosts
                            recieve it and stop sending.
                            - When recievers get the JAM Signal, each host will start a random back-off timer, and wait this time before sending again.
                            - Meaning, only a single user can send at any time. Loss of bandwidth.
                            - All recievers will recieve the traffic/signal on the wire.
                            - This problem is solved by Switches, discussed shortly.

            Q. If a medium is a single wire, and all the hosts can hear the sender, then how does a host
            identify which signal is for them, so that they can process it by copying the signal from wire-to-memory ?
            A. What we are really asking is, how do i address a packet to a particular host, so every reader can tell wether its a message for them or not. So this is a problem of uniquely identifying speakers on a shared medium. The solution really is addressing.
                - Media Access Control (MAC) Addressing
                    - Each physical interface(connection point) is atleast 1 MAC Address.
                    - Each ethernet Frame contains source/destination Mac address.
                    - Formatting is done so that the destination MAC is seen first by the reader, so they can determine if the packet is for them, before reading any data that they later might have to discard anyway.

            Q. Types of communication situations/requirements ?
            A.  - 1 to 1 or Unicast. Only 2 speakers. 
                    - Can work technically without any addressing. But this would never scale beyond 2 users without MAC Addressing.
                
                Q. How are these achieved ?
                - 1 to all or Broadcast. 1 Speaker multiple recievers.
                - 1 to many or Multicast. 1 Speaker and selective recievers.
                A. Let's talk about Ethernet Chipsets, Broadcast, Multicast and Promiscuos Mode.
                    
                    - Basic Ethernet operation requires chipsets to not only be able to recieve traffic for its own MAC address, but also for other MAC Addresses. (Broadcasts & Multicasts)
                    - You can program the chipset to listen to these programmed/configured/hardcoded addresses to listen to.
                    - If these set of addresses are larger that what can be programmed on the chipset, the Chipset(NIC) can be set to Promiscuos mode i.e. that is to say, to copy all signals recieved on the wire-to-memory.
                    - Since now all the frames are copied-to-memory, the decision as to which frame is to be kept and which to be discarded is off-loaded to the General Purpose CPU.
                        - This is an INTERUPT for the CPU.
                        - This is not good for efficiency.
                        - Also, these packets will wait in Memory(inducing delay) and consume the memory until they are not processed by the CPU.
                    
                    With that said,
                    Q. Does it ever make sense then, to ever enable Promiscous Mode on any chipset and if yes then why ?
                    A. 1. Routers: - They must recieve all packets to check if they can be forwarded.
                       2. Monitoring: - To capture packets on the wire.

                    Q. How can we keep the MAC addresses unique ?
                    A. MAC-48 OR EUI-64
                            - The first 3 octets are the OUI - Organizationally Unique Identifier / Vendor.
                            - From the 3rd octet, last 2 BITs are kept reserved for:
                                    - Eg: 7 6 5 4 3 2 | 1 0
                                    - At postion of 1:
                                                - 0 means original MAC from Manufacturer.
                                                - 1 means modified/locally assgined address.
                                    - At postion of 0:
                                                - 0 means single unitcast interface.
                                                - 1 means multicast-group of recievers
                            - So MAC address has 46 BITS for addressing (2 bits used as above) for a total of 70 Million addreses.
                            - Since that might not be sufficent given the growth of IOT (5 million new devices born per DAY).
                            - Hence MAC was extended to 64-bits to EUI-64.
                            - This can support 2 raise to 62 or over a Gazillion unique addresses.

                Q. How can we solve the problem of only a single user being able to transmit at a given time ?
                A. Let's look at Switched Network Ethernet Operation in comaprison to the Single Wire or HUB Network Operation:
                    1. HUBs:
                        - Shared Medium = Single Wire = Single Circuit
                        - All hosts connected to HUB are essentially connnected to a single wire/circuit.
                        - Same pair of wires used for sending and recieving, so can only send or recieve at a given time so hald duplex operation.
                    
                    2. Switches:
                        - Each connection from host to switch is a sperate Circuit. Seperate wire.
                        - This allows Hosts to be able to independently send traffic regardless if other users are also sending traffic since each port/connection is a new/independent circuit or wire.
                        - Will still need CSMA/CD if using Single pair so can only Send or recieve at a given moment in time. Still half duplex.
                        - If Two pairs of wire are used, 1 for RX/TX each, so hosts can send and recieve at the same time, meaning Full Duplex Operation, hence no need for CSMA/CD.

                Q. How are errors in trasmission handled ?
                A. CSMA/CD was desinged to Detect Collisions.
                    - However, this is not the only type of errors encountered while transporting data through a wire.
                    - Eg:  Magnetic Interference, Signal Reflection.

                    Basically, how do we ensure data is arriving in the same state it was sent ?
                    A. - For slower networks (upto 100mbps) - 32-bit CRC
                       - For higher speed networks (upto 1Gbps) - 8B10B Encoding (Clocking and Bit error correction)
                       - For even higher speeds (Upto 10Gbps) - ReedSolomon or 16B8B Encoding.
                        - Implemented in Hardware.
            
            Q. How is the Flow Control problem observed and solved at Physical Layer or Layer 1?
            A. In Full Duplex Switch Operation:
                    - There is no shared medium and two pairs are used so no need for CSMA/CD.
                    - Host can send data on the wire as soon as the wire permits.
                    - This can cause a situation where,
                        - Host recieves more data that it can process.
                    A. To resolve this, PAUSE Frames were developed for Ethernet.
                        - Switch can send(since the reciever sends pause frames) Pause Frames so sender knows to stop sending for a given length of time before resuming.

            
            Q. What is the maximum data you can send on the wire ?
            A. The following factors determine how much actual data from the application can be put on the wire:
                1. The total available Theoretical Bandwidth.
                2. Flow Control & Channel Efficiency
                3. Overhead of transport protocol.

               - The first 2, is how we calculate throughput.
               - All 3 together is what determines the GoodPut or the actual maximum bandwith available for data traffic. 


                
            