---
layout: post
title:  "Hydra Smart Agent - Foundations"
date:   2019-06-15
desc: "This post will describe the first steps developed in the Hydra Smart Agent"
keywords: "Hydra,linked-data,hypermedia,webapis,websematics"
categories: [Gsoc19]
tags: [Hydra, GSoC'19, Hypermedia APIs, Linked Data, Hydra Ecosystem]
icon: icon-google
---

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

With that structure, the development was started and can be followed up at [PR #109](https://github.com/HTTP-APIs/hydra-python-agent/pull/109). Basically, we want for now to provide a working grounded structure for the Agent module making use of graphutils operations and a lasting Session. In the current state, the Agent is able to do something like this:

<div style="height: 30px"></div>
<img src="/static/assets/img/blog/2019-06-15-hydra-smart-agent-foundations/current_agent.png" alt="agent-structure" style="display: block; margin-left: auto; margin-right: auto; width: 600px; max-width: 100%;">
<div style="height: 30px"></div>

**But that's not only it**, this was only the first step of a series of enhancements that will make sure that the Agent uses everything Hydra enables it to do. Ultimately, the Agent should have the core functionality as depicted in the GET below(code details were hidden and naming simplified to focus on the future core functionality).

<div style="height: 30px"></div>
<img src="/static/assets/img/blog/2019-06-15-hydra-smart-agent-foundations/future_agent.png" alt="agent-structure" style="display: block; margin-left: auto; margin-right: auto; width: 600px; max-width: 100%;">
<div style="height: 30px"></div>

When fully implemented, this will enable the user to make a GET request as *agent.get("Temperature", {City: "hyderabad"})*{: style="color: red"} and retrieve that information with a transparent interface. This is an extremely decoupled methodology which allows the API to adapt, change it's architecture and evolve and all of this shouldn't affect the Agent or how it operates. 

More than that, this same interface will be using all the advantages of a cache Redis Database as a valid Graph, which enables blazing fast querying and will be always updated within a network of Agents, allowing it to easily find out brand new resources. **Pretty interesting, right?** This is the goal of one of the Agent's next step, *a.k.a the Synchronization Mechanism*. But hang on there because that will be content for one of our next blog posts ;).

Thanks for reading. Hope to see you on the next one!

[^2]: A.K.A. - Also known as
[^1]: PoC - Proof of Concept

---
