---
layout: post
title:  "Hydra Smart Agent - All Set Up"
date:   2019-07-20
desc: "New features and enhancements for in the Hydra Smart Agent"
keywords: "Hydra,linked-data,hypermedia,webapis,websematics"
categories: [Gsoc19]
tags: [Hydra, GSoC'19, Hypermedia APIs, Linked Data, Hydra Ecosystem]
icon: icon-google
redirect_from:
  - /y8wa
  - /hydra-agent-all-set-up
---

Hello and welcome to my blog! This is the second part of the [blog post here](https://gustavomorais.me/gsoc19/2019/06/15/hydra-smart-agent-foundations.html) about the Hydra Smart Agent. If you have some interest but isn't yet quite familiar with Hydra, head to https://www.hydraecosystem.org/ and read some introductory materials to get familiar :) 

In our last blog post, we detailed the foundations of the Hydra Python Agent. The agent could already execute all CRUD operations through GET, PUT, POST and DELETE. However, this was still at a pretty basic level where it could only be made using specific URLs for the resources, not leveraging on the power of Hydra's Descriptive APIs to have the Agent set up as we wanted. More than that, we had some inconsistencies in the implementation we had to solve. Below there's a summary of what was made with the proper PR links:

### New Features
1. [Recursive Node Creation for Linked Data](https://github.com/HTTP-APIs/hydra-python-agent/pull/118)
2. [Querying by classes and properties for Redis cached data](https://github.com/HTTP-APIs/hydra-python-agent/pull/120)
3. [Server queries with classes and properties using IRI Template](https://github.com/HTTP-APIs/hydra-python-agent/pull/121) 

### Enhancements and Inconsistencies 
1. [Project Layout modification](https://github.com/HTTP-APIs/hydra-python-agent/pull/115)
2. [Duplicate Nodes when committing](https://github.com/HTTP-APIs/hydra-python-agent/pull/117)
3. [Updating Core Packages](https://github.com/HTTP-APIs/hydra-python-agent/pull/119)

### Ongoing implementation
- [Synchronization Mechanism for Server-Client Updates](https://github.com/HTTP-APIs/hydra-python-agent/pull/123)
                              
On this blog post we will focus mainly detailing the new features since they involve more complex architecture decision making.

---------------------------
# New features


### Recursive Node Creation for Linked Data
When creating Hydra APIs, it's possible to link data assigning URIs as a resource property as you see can see in the image below. In practical terms, you can have an object called Drone which has 10 properties and one of them is State, which is actually another object that may contain other 5 properties and so on. Considering that, we had an inconsistency with the Redis Caching implementation([ISSUE #113](https://github.com/HTTP-APIs/hydra-python-agent/issues/113)), when we had embedded URIs for other resources. Then we would only fetch the main object's properties and not the linked one.

<div style="height: 30px"></div>
<img src="/static/assets/img/blog/2019-07-20-hydra-smart-agent-in-action/embedded_resource.png" alt="agent-structure" style="display: block; margin-left: auto; margin-right: auto; width: 700px; max-width: 100%;">
<div style="height: 30px"></div>

With this implementation done([PR #118](https://github.com/HTTP-APIs/hydra-python-agent/pull/118)), the Agent now recursively searches for embedded URIs fetching them and storing in the Redis Cache layer, making the caching more effective since it will have already all the resource's properties. So for example, if you run ```Agent.get("Drone", {"model": "Smart 2000"})```, this will store in Redis all Drone properties, but also all State properties that are associated with that instance. More than that, if State had an URI in its properties it would also fetch that object and so on, recursively.

More about this on PR #118: https://github.com/HTTP-APIs/hydra-python-agent/pull/118

### Server queries with classes and properties using IRI Template
Our Hydra Server(hydrus) now supports searching the server by classes and properties using IRI Template([ISSUE #302](https://github.com/HTTP-APIs/hydrus/issues/302)). That means that now instead of always querying only for specific resources URLs, you can search simply doing something like ```http://localhost:8080/serverapi/DroneCollection?name=Drone 1```. This is also valid for the linked data, following the same previous example of a linked State object, you could do: ```http://localhost:8080/serverapi/DroneCollection?DroneState[Battery]=10```. The server response to queries like this is the collection with a filtered list of members as a property.

In that context, we had the goal of enabling the Agent to use this functionality([PR #121](https://github.com/HTTP-APIs/hydra-python-agent/pull/121)). Therefore, now instead of only enabling users to use the Agent with full resources URLs, it's also possible to create requests as ```agent.get("Drone", {"name": "Drone 1", "DroneState[Battery]": 10})```, which will create the proper request and process it searching the hydrus server. As a response, the Agent provides a list of matching resources with their "@id" and "@type":

```
[
        {
            "@id": "/serverapi/DroneCollection/76fb06cf-2ea5-481a-86e5-7e7e2d44c8e5",
            "@type": "Drone"
        },
        {
            "@id": "/serverapi/DroneCollection/75eea5ed-83d1-4dec-ad93-855d43afdb90",
            "@type": "Drone"
        },
        {
        ...
]
```

More about this on PR #121: https://github.com/HTTP-APIs/hydra-python-agent/pull/121

### Querying by classes and properties for Redis cached data
The above implementation enabled the Agent to query by classes and properties from the server, however, the Agent also has the capability of caching resources. Therefore, it's an interesting feature to enable the Agent to try to search for information first from its cached resources.

By default, the Agent will forward search queries to the server since Redis is used to cache requests and not copy the database. Which means, that the Agent doesn't have all the resources to search through them. However, usually the user uses a pagination limit to query for a maximum amount of nodes to be shown in a UI, for example.  Thinking of that, the most common search patterns or repeated ones can be fetched directly by Redis using the optional variable "**cached_limit**". Thus, with the implementation at [PR #120](https://github.com/HTTP-APIs/hydra-python-agent/pull/120) it's possible to execute queries like ```agent.get("Drone", {"name": "Drone 1"), cached_limit=5)``` and avoid a big amount of common server queries.

More about this on PR #120: https://github.com/HTTP-APIs/hydra-python-agent/pull/120

---------------------------
# Enhancements and Inconsistencies 

### Project Layout modification
  After some discussion [#PR 115](https://github.com/HTTP-APIs/hydra-python-agent/pull/115) changed a bit the folder structure in a logical way for the code to be more organized.

### Duplicate Nodes when committing
This was an old problem encountered in the Agent the produced the graph to create a new node for every previously created node every time a new resource is inserted. This took a while to debug and find the proper solution, which was using the correct Redis Graph function.

More about these on PR #117: https://github.com/HTTP-APIs/hydra-python-agent/pull/117

### Updating Core Packages
Since the Agent's implementation started, its core repositories received breaking updates that weren't yet implemented. These were focused in two areas, first the Redis Graph implementation: 

- Redis Graph 1.7 -> 2.0
- Redis 2.10.6 -> 3.2.1
- Redis Docker IMG 1.2.2 -> edge

Second Hydra Spec updates implemented in our Hydra Python Core repository:

- hydra-python-core@v0.1 -> master

Now the agent is up to date with both, and its inner functions were adopted to make use of the beneficial changes in the repositories.

More about these on PR #119: https://github.com/HTTP-APIs/hydra-python-agent/pull/119

## Ongoing Synchronization Mechanism Implementation
  The current big feature that will be released by the repository is the Synchronization Mechanism between Agent and hydrus Server. A lot was already done in that scope, the whole architecture is set and the Agent side is mostly finished. Now we're developing the server-side of the sync mechanism, what might lead to some adjustments in the Agent/hydrus. That said, we will leave that for the next blog post when everything is solid and working. Stay tuned!

More about the ongoing progress at PR #123: https://github.com/HTTP-APIs/hydra-python-agent/pull/123

---
Thanks for reading. Hope to see you on the next one!
