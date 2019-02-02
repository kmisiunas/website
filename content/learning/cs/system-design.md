---
title: "System Design"
date: 2018-10-05
draft: false
typora-root-url: ../../../static
toc: true
---

These questions are designed to see how you would perform with real world programming challenges. If you were asked by your manager to design some system, what would you do?

## Tips:

 - **Communicate**: A key goal of system design questions is to evaluate your ability to communicate. Stay engaged with the interviewer. Ask them questions. Be open about the issues of your system.
 - **Go broad first**
 - **Use the whiteboard**: Using a whiteboard helps your interviewer follow your proposed design.
 - **Acknowledge interviewer concerns**
 - **Be careful about assumptions**: An incorrect assumption can dramatically change the problem. For example, if your system produces analytics / statistics for a dataset, it matters whether those analytics must be totally up to date.
 - **State your assumptions explicitly**
 - **Estimate when necessary:** In many cases, you might not have the data you need. For example, if you're designing a web crawler, you might need to estimate how much space it will take to store all the URLs. You can estimate this with other data you know.
 - **Drive:** you should be driving through the question. Ask questions. Be open about tradeoffs. Continue to go deeper. Continue to make improvements
 - Poking holes in your own solution is a good way to demonstrate analyzing and solving problems.

# Design process

 1. **Scope the Problem** 
     Ask questions. It is important to understand what is the scope of the problem and what are use cases. Map out the major features that are requred. 

 2. **Make Reasonable Assumptions**
     Some insight into product working helps coming up with reasonable assumptions about user behaviour, quantities, etc.

 3. **Draw the Major Components**
     Draw a diagram of the major components. You might have something like a frontend server (or set of servers) that pull data from the backend's data store. You might have another set of servers that crawl the internet for some data, and anotl")er set that process analytics. Draw a picture of what this system might look like.

 4. **Identify the Key Issues**
     For example, if you were designing TinyURL, one situation you might consider is that while some URLs will be infrequently accessed, others can suddenly peak. This might happen if a URL is posted on Reddit or another popular forum. You don't necessarily want to constantly hit the database.

 5. **Redesign for the Key Issues**



# Scalability

This is critical step for designing big system and thus careful consideration must be made. 

 1. **Ask Questions**
 2. **Make Believe**
      Pretend that the data can all fit on one machine and there are no memory limitations. How would you solve the problem? The answer to this question will provide the general outline for your solution.

 3. **Get Real**
     How much data can you fit on one machine, and what problems will occur when you split up the data?
     Common problems include figuring out how to logically divide the data up, and how one machine would identify where to look up a different piece of data.

 4. **Solve Problems**



# Key Concepts

### Horizontal vs. Vertical Scaling

 - Vertical scaling means increasing the resources of a specific node. For example, you might add additional memory to a server to improve its ability to handle load changes.
 - Horizontal scaling means increasing the number of nodes. For example, you might add additional servers, thus decreasing the load on any one server.

### Load Balancer

Allows a system to distribute the load evenly so that one server doesn't crash and take down the whole system

Routing 

### Database Denormalization and NoSQL

Join in SQL is slow. 

Denormalization means adding redundant information into a database to speed up reads. For example, imagine a database describing projects and tasks. You might need to get the project name and the task information. Rather than doing a **join operation** across these tables, you can store the project name within the task table (in addition to the project table).

A NoSQL database does not support joins and might structure data in a different way. It is designed to scale better.

**todo**: what is typical technologies: hadoop?



### Database Partitioning (Sharding)

Sharding means splitting the data across multiple machines while ensuring you have a way of figuring out which data is on which machine. Methods:

 - Vertical Partitioning 
 - Key-Based (or Hash-Based) Partitioning
 - Directory-Based Partitioning

### Caching

When an application requests a piece of information, it first tries the cache. If the cache does not contain the key, it will then look up the data in the data store.

**Almost any solution needs a cashing layer**

### Asynchronous Processing & Queues

If we were running a forum, one of these jobs might be to re-render a page that lists the most popular posts and the number of comments. That list might end
up being slightly out of date, but that's perhaps okay.

### Networking Metrics (Logging)

 - Bandwidth - the maximum amount of data that can be transferred in a unit of time.
 - Throughput - the actual amount of data that is transferred.
 - Latency - how long it takes data to go from one end to the other.

*Technology examples:* Kafka for real time logging

### MapReduce

 - Map takes in some data and emits a `<key, value>` pair.
 - Reduce takes a key and a set of associated values and "reduces" them in some way, emitting a new key and value. The results of this might be fed back into the Reduce program for more reducing.

### Service Orientated Architecture (SOA)

Provisioning is ...

Containers are used... Docker 


## Considerations

 - **Failures**: where might the system fail?
 - **Availability and Reliability**: time in % system is available and probability it is available for given unit of time. 
 - **Read-heavy vs. Write-heavy**: whether an application will do a lot of reads or a lot of writes impacts the design.
 - **Security**

---

# Examples

## Twitter 

[Following this demo](https://www.youtube.com/watch?v=KmAyPUv9gOY). Key features include: (1) Tweeting, (2.a) User timeline (2.b) Home timeline (3) Following. When tweet is posted, it goes through a load balancer, then to REDIS database that updates all the user timelines, who are following. This has two problems: when someone has a lot of followers, there are a lot of timelines to update, which is slow. Mixed approach, where popular figures are stored in SQL database and only couple of people are inserted at runtime.  Second issue, is inactive users that use up resources. To reduce their strain on the server, only use REDIS to update users active in the last 2 weeks. 

For followers, use a table for each user, where REDIS gets the timelines to update. In general it is called **Push on Change** and it updates all user info when a change is submitted. 

REDIS is in-memory database (key-value database) that can be distributed across many machines. This is similar to NoSQL. To find machines with relevant user data, we can look them up using a hash table where the key is username. 

Limitations of our approach. Space is not an issue since twitter limits the message length to 120 chars or so. But we are replicating a lot of data since we precompute home timelines for most users. This allows quick access of data. 

## Messaging service 

[Following this demo](https://www.youtube.com/watch?v=5m0L0k8ZtEs). Key features: (1) One-to-one messaging, (2) Sent / delivered / read notifications (3) Push notifications. For the messaging, we can imagine communication as follows

Message => L.B. => Server with a queue => client messaging app

The message gets deleted from the server once the client app received the message. This could be TCP protocol confirmation. Indicator 'Sent' is easy, same sever confirmation of received message. While other two messages can be implemented as a special type of message sent in a typical fashion as above. This would allow to reuse the same architecture. 

For push notifications, the timing does not have to be very precise. These messages could be sent by a separate server that receives a notification from the main server. This new server can be integrated with Google Push notification APIs for simplicity. 


# References 

1. "Cracking the coding interview" 6th edition, book. 
2. https://github.com/checkcheckzz/system-design-interview
3. https://www.hiredintech.com/system-design