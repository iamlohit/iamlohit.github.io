---
layout: pages
title:  "Modeling Network Transport - How Models can help manage & comprehend Network Complexity"
date:   2022-07-28 18:40:00 +0530
categories: network-fundamentals
---

Sticking with First Principles,

    Q. How can engineers engage with the complexity involved in Data Transport Systems?
    A. - 1. Understand the basic problems and their various possible solutions.
         2. Build Models that aid engineers in understanding of transport protocols by:
            - Classifying Protocols by: Purpose, Information in each Protocol and interaction between protocols.
            - Help chose which questions to ask to understand a given protcol, what problems all protocols need to solve.
            - How a protocol interacts with the network it runs over carrying app traffic.
            - How each protocol fits together to make a transport system.

        Q.What are models ?
        A. They are abstract representations of Problems & Solutions to show how things fit together.

            Q. How can transport problems be modeled in a way that:
                - Allows engineers to quickly and fully grasp the problems these systems need to solve.
                - The way multiple protocols can be put together to solve these problems.
            A. Two Ways:
                1. DOD/OSI/RINA Models of Protocol Stack.
                2. Nature:
                    - Connection Oriented:
                        - They set up end-to-end connection before sending first bit of data.
                        - If it breaks, session needs to be re-built.
                    - Connectionless:
                        - No Connection Set up.
                        - Combine data required to transmit the data (header) with the first bits of payload of app data.
                        - Heals quickly against device/link failures, since the path is not fixed, since there is no session.

