---
title: "Blog 1"
 
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Zupee implements Amazon Neptune to detect Wallet transaction anomalies in real time

*This is a guest post by Aman Kumar Bahl, leader of Data Engineering, and Apoorv Mathur, Lead Data Engineer, at Zupee, in partnership with AWS.*

Zupee is a leading skill-based gaming platform offering casual and board games and is one of the fastest growing real money gaming platforms in India. Users can play multiple skill-based games online and win prizes.

Zupee offers both *free-to-play* and *pay-to-play* game formats. For paid games, users have to deposit money in their app wallet. Zupee runs user incentive programs that help users to gain rewards, refer friends, or increase engagement. To maintain the integrity of incentive programs, they wanted to understand user behaviors on the platform and deter the users who display suspicious transaction patterns.

In this post, we show you how Zupee integrated Amazon Neptune Database to detect anomalies in real time for wallet transactions by creating a system for tracing the complex relationships between users, devices, and wallet transactions metadata.

Amazon Neptune is a fully managed graph database service built for the cloud that makes it easier to build and run graph applications that work with highly connected datasets. Neptune is optimized for storing billions of relationships and querying the graph with milliseconds latency, and supports the popular graph query languages Apache TinkerPop Gremlin, the W3C’s SPARQL, and openCypher. Neptune provides built-in security, continuous backups, serverless compute, and integrations with other AWS services. Neptune supports in-place upgrades of cluster and database instances. Amazon Neptune Analytics is a high-performance analytics engine designed to extract insights and detect patterns in vast amounts of graph data stored in Amazon Simple Storage Service (Amazon S3) or Amazon Neptune Database.

---

## Solution overview

Initially, Zupee developed its solution with a basic design, using a relational database to store data. This approach facilitated the identification of connected users based on various attributes, enabling the determination of whether a user’s transaction was legitimate or not. The system’s limitations became apparent when processing millions of transactions across an expanding user base with multiple correlation attributes, necessitating a more robust solution. The inherently interconnected nature of the data, where users could be linked through multiple attributes and complex relationships, made it increasingly difficult to efficiently query and analyze these connections using traditional relational database structures. The need to traverse and explore these intricate relationships in real time led Zupee to consider a graph-based approach, which is inherently designed to handle highly connected data and complex relationship queries more efficiently.

Graph databases excel at managing diverse entities with intricate, interconnected relationships. Unlike traditional relational databases, which require complex multi-level table joins for querying data across multiple connections, graph databases allow for efficient traversal of these relationships without predefined join operations. This capability enables the exploration of data through multiple hops and transformations with minimal restrictions. As a result, graph databases can execute complex queries across numerous interconnected data points and return results in milliseconds. This performance advantage is particularly valuable when dealing with highly connected data sets where the relationships between entities are numerous and the optimal query paths might not be known in advance. By representing data as a network of nodes and edges, graph databases provide a more intuitive and flexible approach to storing and querying interconnected information, making them ideal for scenarios where relationship analysis is crucial.

Graph databases excel at managing and accessing interconnected data through their specialized design. They store information as nodes (representing entities) and edges (representing relationships), enabling efficient exploration of the intricate web of connections.

---

### Building on Neptune

Zupee stores the required model features in Neptune and uses those features to create clusters of users and monitor wallet transactions. Zupee started by processing over 1 million wallet transactions per day in real time. For confidentiality reasons, we only include placeholder names in the attributes, but you can think of several attributes belonging to users doing transactions on the Zupee platform. The transactions that have common attributes and patterns similar to known suspicious transactions are flagged as anomalies. This forms the root of Zupee’s integrity system meant to protect the overall user base and incentive programs.
The following image depicts a graph data model used by Zupee in a Neptune Graph database. It models relationships between users, devices, transactions, and payment instruments.

![pic1](/images/zupee1.png)

The central User node:

Connects to **Device_Token** for device logins

Connects to **Transaction** for creating and transferring transactions

Connects to **Payment_Instrument** for payment methods

Connects to **User_Profile** for user profiles

This flexible graph structure enables efficient data management and querying across various entities and their interconnections within Zupee’s platform.

Zupee looked for a graph-based database and built a small proof of concept. The results were surprisingly good in terms of the information Zupee could derive from the graph model. Using the graph model, they immediately found user associations that they weren’t aware of.

The following diagram represents a graph database structure, where nodes (circles) correspond to data entities or vertices, and edges (lines) depict relationships or connections between those entities.

![pic2](/images/zupee2.png)

The red nodes symbolize individual data records or documents, while the blue nodes represent consolidated views or indices linking related data. The radial clustering patterns suggest a distribution of data organized around specific relationship types or domains. This visual representation aids in understanding the inherent connectivity and complex associations within a graph database, which excels at efficiently storing and querying highly interconnected data structures compared to traditional tabular databases.

Zupee used the Union Find algorithm to efficiently create distinct subgraphs (groups) of entities on their platform, based on the relationships or associations between them. This approach helped them handle pseudo associations and use a combination of data models for identifying them without having to model every single relation explicitly in a graph database such as Neptune.

Using this approach, Zupee was able to develop a service that was capable of grouping associations together and look for disjointed subgroups of entities, enabling them to discover complex and undiscovered associations.

The solution architecture—shown in the following figure—implements real-time fraudulent transaction detection.

![pic3](/images/zupee3.png)

User transactions are processed through an Amazon API Gateway, which then passes the transactions to AWS Lambda for processing. The transaction data, including attributes such as Device_Token and Payment_Instrument, is stored in Neptune. AWS Global Accelerator is used to optimize network performance, and Network Load Balancer distributes the workload across the pool of game servers, which render the game.

Neptune’s graph structure enables Zupee to:

Identify suspicious patterns by analyzing relationships between user profiles, payment instruments, and device tokens
Detect duplicate accounts by finding connected components in the user transaction graph
Discover shared payment instruments (UPI/VPA IDs) across multiple accounts
Calculate appropriate incentive values based on user authenticity
For pay-to-play game formats, when users add money to their wallet, the system queries Neptune to fetch relevant subgraph showing user associations. This helps determine if the user is legitimate or potentially using multiple accounts. Based on this analysis:

Legitimate users with no suspicious connections receive full incentive benefits
Accounts with detected anomalies receive adjusted or no incentives
Shared payment instruments across accounts trigger reduced incentive amounts
This systematic approach helps Zupee optimize costs by ensuring incentives are primarily given to genuine users while protecting against potential misuse through duplicate accounts or shared payment methods.

---

## Summary

By using Amazon Neptune, Zupee has accomplished a remarkable response time of less than 50 milliseconds in optimal conditions, facilitating real-time anomaly detection and enabling the team to swiftly implement corrective measures of incentive distribution to legitimate users. Currently, Zupee oversees an expansive connected data network encompassing over 5 million nodes and edges. The company has developed the capability to dynamically search for entities and identify connected nodes in real time.

---

### About the author

**Aman Kumar Bahl** is the leader of Data Engineering at Zupee, Aman is entrusted with steering the technological backbone that fuels the organisation’s data-driven ecosystem. He exhibits a holistic approach in crafting data intensive applications aimed at empowering business scalability.

**Apoorv Mathur** is a Lead Data Engineer at Zupee, responsible for the design and architecture of the organization’s data systems. He drives the adoption of new technologies, enabling real-time analytics and enhancing the data infrastructure.

**Ajeet Dubey** is a seasoned Solutions Architect at AWS, with over 13 years of expertise in designing and implementing resilient, scalable cloud solutions. His specialization lies in developing end-to-end cloud-focused machine learning and generative AI solutions.