---
layout: post
title:  "Data Plane Summary"
date:   2022-07-28 11:30:00 +0530
categories: network-fundamentals
---

How to Approach a technology/protocol/product and how to understand them using first principles ?
What are the questions to ask ?

    1. What is the problem ?
    2. What are the possible solutions ?
    3. How has the solution been implemented ?


History of Networking

    1. Fundamentals: 
        What are networks desinged to do ? > 1 thing: Carry information from one attached system to another.
            Further Detailed Questions:
                1.1 Should networks be circuit switched or packet switched ?
                1.2 Should packet switched networks use fixed or variable size frames ?
                1.3 What is the best way to calculate shortest path through a network ?
                1.4 How should packet switch networks interact with QOS ?
                1.5 Should the control-plane be centralised or decentralised ?
                1.6 What is network complexity ? Can it be "solved" ?

            Considering Solutions:
                1.1 Should networks be circuit switched or packet switched ?
                Sol. Circuit Switching:
                        - Uses Time Division Multiplexing
                            Meaning, Devices are alotted time to talk on the network by the controller. They can only transmit when the circuit is given to them for a certain duration. 
                            It devides total available time in slots and give these slots to hosts to communicate.
                                Advantages:
                                    - No need for any header lookup, or do any complex switching.
                                    - Parties scheduled to talk are predecided, so is the bandwitdth.
                                Disadvantages:
                                    - Bandwith wasted since hosts might not want to transfer data exactly as their time-slot. No communication between host and switching system.
                                    - Complexicity increased since control-plane is centralised, with users and services, hence not scalable.
                     Packet Switching:
                        - Each device can independently take forwarding decision, hence decentralised.
                        - Based on reading the header information of the packet.
                        - Efficient use of bandwidth, hosts can choose when to communicate, network is available to all users all the time.
                            Advantages:
                                - No need for a centralised controller since each device forwards based on header info.
                                - Scalable since its decentralised nature and always on nature.
                            Disadvantages:
                                - Additional complexity required by each node.
                
                1.2 Should packet switched networks use fixed or variable size frames ?
                Sol. First question we should ask is Why does the frame size matter ?
                     -The speed at which you can switch packets depends on the time it takes to copy the packet from one memory location to another.
                     -Every fragment needs to be copied seperately between memory locations.
                     -If the all the frames are fixed size, what if the info you are sending is less than frame size ? You will need to fill the frame
                     with padding, which is wasted bandwidth.
                     -If all the frames are variable size, we can have smaller and bigger frames.
                     -Even variable lengths have an upper finite length.
                     -So we can remove this padding requirement by using variable length frames, and allow some fragmentation where the data is larger than
                     the frame size.
                     -It also depends on how much of the information from the header needs to be read and understood to forward the packets.
                     -If we want to reduce the computation tax and time required to switch a packet by reducing the amount of time required to process the header.
                     -If we have a smaller header, the switching process will be faster.
                     -This introduced the concept of MPLS, using Lables (12 bit) field in MPLS Header, rather than based on 48-Bit MAC destination feild or 32-Bit IP Header feild.

                1.3 What is the best way to calculate shortest path through a network ?
                Sol. Why should be calculate the shortest-path ?
                     What is the relationship between shortest path and loop free path ?
                     What are the algoriths used and the basic idea of the method behind finding loop free paths ?
                     How are these algorithms implemented in Routing Protocols and their classification and examples ?
                     Distance Vector, Link State & Path Vecotor Algoriths and Protocols based on them.
                
                1.4 How should packet switch networks interact with QOS ?
                Sol. What is the need for QOS ?
                        - Its required for Real Time traffic i.e. Voice, Video and Prioritizing critical traffic in congestion scenario.
                        - Real time traffic required Low Delay and Low Jitter.
                        - Do we need qos with 100Gb/400Gb links ? Yes, congestion can always occur and thats when the QOS Queing Mechanism 
                        will kick into action.
                     How is it Implemented ?
                        - Packets are marked based on certain criteria.
                        - Routers are configured to alter queing of traffic with respect to the markings on the packet.
                        - Only takes effect at time of congestion.
                    
                1.5 Should the control-plane be centralised or decentralised ?
                Sol. Traditionally:
                        - Circuit Switched Networks: Centralised Control Plane
                        - Packet Switches Networks: Distributed Control Plane
                     Recent Attempts to use Centralised Control-Plane with Packet switched networks:
                        1. ForCES - Retrired 2015.
                        2. Open Flow - Began 2006.
                            Idea: A standard API that allows applications to direcly install entried in FIB of forwarding device,
                                  rather than in RIB, without code modification to the Data-Plane/Forwardaing Devices (Since they can be multi-vendor)
                            Reality: - Picked up by Vendors as Features.
                                     - Many controllers created by Vendors for their own since products.
                                     - Original idea of Open-Model left to the community.
                
                1.6 What is network complexity ? Can it be "solved" ?
                Sol. Further Fundamental Questions to ask:
                        - Why so complex ?
                            - Complexity is nessecary to deal with Uncertainity involved in difficult to solve problems.
                            - In any complex system, there will exists sets of three-way Tradeoffs Model:
                                - State: The information carried by the system.
                                - Optimization: Making somthing faster/efficient.
                                - Surface: Interaction Surface. The points at which two systems interact.
                            - If you have not found the tradeoffs, you have not looked hard enough, is a good rule of thumb to follow
                            in all engineering work.

                        - What is policy ?
                            - Any configuration that increases the strech of the network.
                            - Strech is hard to actually measure in the network, since its per source/destination pair.
                            - Strech of a network is defined as the degree of difference between shortest path and the desired path of packet.
                            - Shortest path is not always the best path.

                            - Why do we need policy then ?
                                - To avoid choking the shortest path.
                                - To make efficient use of multi-paths.
                                - Eg: PBR, Traffic-Engineering

                




                

                


