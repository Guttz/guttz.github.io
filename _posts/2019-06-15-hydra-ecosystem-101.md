---
layout: post
title:  "Hydra Ecosystem - 101"
date:   2019-07-08
desc: "This post will describe the first steps developed in the Hydra Smart Agent"
keywords: "Hydra,linked-data,hypermedia,webapis,websematics"
categories: [Gsoc19]
tags: [Hydra, GSoC'19, Hypermedia APIs, Linked Data, Hydra Ecosystem]
icon: icon-google
redirect_from:
  - /y8wx
  - /hydra-agent-foundations
---

Today we live in a connected world and distributed world, one that enables us to quickly build applications that can fetch Google Maps addresses from US, get the weather from India, check the currency exchange from anywhere in the world and much more. This enables us to build such complete and powerful applications and it  sounds awesome, right? Well, it's not so easy as it sounds... For these informations to be provided each of these services are provided by what we call APIs(Application Programming Interfaces), and well... interfaces come in all shapes, sizes and formats. 

So let's say, **you are creating an application** that would like to have the weather shown to the user. You browsed through the thousands different Weather APIs and you picked the famous free [OpenWeather](https://google.com/weather/SaoPaulo) API. What happens now is that you have

You scroll down through the list of possibilities.. You can get the weather by ZIP Code, Rectangle Geographic Zone, Latitude and Longitude, City Name and more. You decide you want to do it by city name. 


More than that, all of these interfaces are only **human-readable**, which means that for every piece of information that is necessary for my application, I have to follow a precise recipe given by the by each of these service providers, and all of them are totally different! Okay, but let's say you found a really easy API to help you get the weather, all you have to do is get https://google.com/weather/SaoPaulo and you have São Paulo's weather. You put this into your application that a bunch of users that use it everyday, it works perfectly for 3 years and then.. Google decides to change the way you get São Paulo's weather to https://google.com/weather2/SaoPaulo, all of a sudden you have to update your codebase all around with tousands of API calls or your application will break.



**Hello and welcome to my blog!** This post is part of a series of blog posts where I'll be documenting my Google Summer of Code 2019 experience. I'm working at the Hydra Ecosystem organization developing a set of tools with Python to build the future of descriptive Web APIs! Now, if you didn't understand any of this but you found the concept interesting, first head to [https://www.hydraecosystem.org/](https://www.hydraecosystem.org/) and read some introduction materials :) <br />
                              

### Hydra Ecosystem and GSoC'19

Now to the real deal. [Our community](https://github.com/HTTP-APIs) has currently different tools including a working server that can be built from a Hydra Doc, a.k.a[^2] [hydrus](https://github.com/HTTP-APIs/hydrus). And also, we have a PoC[^1]Python Agent to work with that server, [a.k.a Hydra Agent](https://github.com/HTTP-APIs/hydra-python-agent). Hydra Agent is a big and important step for the community because it leverages on everything Hydra consists of to enable not only a client, but a [*Smart Agent*](https://www.hydraecosystem.org/heracles_explained). I'm currently in charge of developing the Python Hydra Agent to take it from being a PoC to a Pre-Alpha release supporting all basic operations of a Hydra Agent.

### First weeks of GSoC'19

To take the next step into building a full-featured Smart Agent for our community we have to set a good and well-projected foundation so our code fits and is able to use all the advantages of Hydra. That said, in the first weeks of GSoC'19 we focused on a Design Document paired with various discussions between the members to settle into more rounded up architecture. Here are some reference links to take a look at the discussions and may be helpful for future readers too!

- [Design Doc Available at Google Docs with Discussion](https://docs.google.com/document/d/189TNgNHFG79U68fJtqz12lhACDvMqzYEUIShv9Bl2RI/edit?usp=sharing)
- Issues and Open Dicussions being developed: [#101](https://github.com/HTTP-APIs/hydra-python-agent/issues/101) and [#99](https://github.com/HTTP-APIs/hydra-python-agent/issues/99)
- Ref. base links and older issues: [Redis as Database](https://www.hydraecosystem.org/hydra-agent-redis-graph) - [Graph Structure Issue](https://github.com/HTTP-APIs/hydra-python-agent/issues/18) - [Redis Basic Implementation PR](https://github.com/HTTP-APIs/hydra-python-agent/pull/13) -  [Querying Mechanism Implementation](https://github.com/HTTP-APIs/hydra-python-agent/pull/23)

After all the discussion and conclusions **we settled onto a layered model** of abstraction that consists of three levels. The picture below has the different modules in the code and their functions.

<div style="height: 30px"></div>
<img src="/static/assets/img/blog/2019-06-15-hydra-smart-agent-foundations/structure.png" alt="agent-structure" style="display: block; margin-left: auto; margin-right: auto; width: 900px; max-width: 100%;">
<div style="height: 30px"></div>

Each layer and its function:

1. The first layer should contain and encapsulate the low-level interaction when making HTTP requests to the server and also interacting internally with Redis.

2. Now the second layer is an Agent interface which should enable users to be able to do all basic CRUD operations while being able to use and support **all Hydra features.**

3. Last we have a high-level layer which will be the enhancement of a querying format that should enable natural language-like querying for an even higher level of abstraction for the user. He should be able to ask for things like **"Show me temperature for city Berlin"**.

### Let's get to the coding

With that structure, the first version was developed at [PR #109](https://github.com/HTTP-APIs/hydra-python-agent/pull/109). Basically, we wanted to provide a **working grounded structure for the Agent**  module making use of graphutils operations and a lasting Session. So in the end the Agent is able to something like this:

<div style="height: 30px"></div>
<img src="/static/assets/img/blog/2019-06-15-hydra-smart-agent-foundations/current_agent.png" alt="agent-structure" style="display: block; margin-left: auto; margin-right: auto; width: 600px; max-width: 100%;">
<div style="height: 30px"></div>

The same is **valid for all CRUD operations**, right now the Agents supports:

- GET - used to READ resources or collections
- PUT - used to CREATE new resources in the Server
- POST - used to UPDATE resources in the Server
- DELETE - used to DELETE resources in the Server

To **READ** an existing resource you should:
```
agent.get("http://localhost:8080/serverapi/<CollectionType>/<Resource-ID>")
agent.get("http://localhost:8080/serverapi/<CollectionType>/")
```

To **CREATE** a new resource you should:
```
new_resource = {"@type": "Drone", "name": "Drone 1", "model": "Model S", ...}
agent.put("http://localhost:8080/serverapi/<CollectionType>/", new_resource)
```

To **UPDATE** a resource you should:
```
existing_resource["name"] = "Updated Name"
agent.post("http://localhost:8080/serverapi/<CollectionType>/<Resource-ID>", existing_resource)
```

To **DELETE** a resource you should:
```
agent.delete("http://localhost:8080/serverapi/<CollectionType>/<Resource-ID>")
```

So with this methods working, we are now able to do some interactions with a hydrus server in a straighfoward way. **A simple lifecycle of a resource**, for a collection of Drones, would be something like:

```
# Initializing Agent with hydrus server link
agent = Agent("http://localhost:8080/serverapi")

# Creating Resource on the Server
new_object = {"@type": "Drone", "DroneState": "Simplified state", "name": "Smart Drone", "model": "Hydra Drone", "MaxSpeed": "999", "Sensor": "Wind"}
put_response, new_resource_url = agent.put("http://localhost:8080/serverapi/DroneCollection/", new_object)

# Updating the resource
new_object["name"] = "Updated Name"
agent.post(new_resource_url, new_object)

# Retrieving updated resource
agent.get(new_resource_url)

# Deleting resource from server
agent.delete(new_resource_url)
```

**It's important to note,** that all these straighforward requests are now supported by a Caching Redis-layer. So everytime you query for a resource that the Agent already has it will be instatly fetched from Redis.

### That's not only it...

This was **only the first step of a series of enhancements** that will make sure that the Agent uses everything Hydra enables it to do. Ultimately, the Agent should have the core functionality as depicted in the GET below(code details were hidden and naming simplified to focus on the future core functionality).

<div style="height: 30px"></div>
<img src="/static/assets/img/blog/2019-06-15-hydra-smart-agent-foundations/future_agent.png" alt="agent-structure" style="display: block; margin-left: auto; margin-right: auto; width: 600px; max-width: 100%;">
<div style="height: 30px"></div>

When fully implemented, this will enable the user to make a GET request as *agent.get("Temperature", {City: "hyderabad"})*{: style="color: red"} and retrieve that information with a transparent interface. This is an extremely decoupled methodology which allows the API to adapt, change it's architecture and evolve and all of this shouldn't affect the Agent or how it operates. 

More than that, this same interface will be using all the advantages of a cache Redis Database as a valid Graph, which enables blazing fast querying and will be always updated within a network of Agents, allowing it to easily find out brand new resources. **Pretty interesting, right?** This is the goal of one of the Agent's next step, *a.k.a the Synchronization Mechanism*. But hang on there because that will be content for one of our next blog posts ;).

Thanks for reading. Hope to see you on the next one!

[^2]: A.K.A. - Also known as
[^1]: PoC - Proof of Concept

---
