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

Now to the real deal. [Our community](https://github.com/HTTP-APIs) has currently different tools including: A working server that can be built from a Hydra Doc, a.k.a[^2] [hydrus](https://github.com/HTTP-APIs/hydrus). And also, we have a PoC[^1]Python Agent to work with that server, [a.k.a Hydra Agent](https://github.com/HTTP-APIs/hydra-python-agent). Hydra Agent is a big and important step for the community because it leverages on everything Hydra is built to enable not only a client, but a [*Smart Agent*](https://www.hydraecosystem.org/heracles_explained). I'm currently in charge of developing the Python Hydra Agent to take it from being a PoC to a Pre-Alpha release supporting all basic operations of a Hydra Agent.

### First weeks of GSoC'19

To take the next step into building a full-featured Smart Agent for our community we have to set good and well projected foundation so our code fits and is able to use all the advantages of Hydra. That said, in the first weeks of GSoC'19 we focused on a Design Document paired with various discussions between the members to settle into more rounded up architecture. Here are some reference links to take a look at the discussions and may be helpful for future readers too!

- [Design Doc Available at Google Docs with Discussion](https://docs.google.com/document/d/189TNgNHFG79U68fJtqz12lhACDvMqzYEUIShv9Bl2RI/edit?usp=sharing)
- Issues and open dicussions being developed: [#101](https://github.com/HTTP-APIs/hydra-python-agent/issues/101) and [#99](https://github.com/HTTP-APIs/hydra-python-agent/issues/99)
- Ref base links and older issues: [Redis as Database](https://www.hydraecosystem.org/hydra-agent-redis-graph) - [Graph Structure Issue](https://github.com/HTTP-APIs/hydra-python-agent/issues/18) - [Redis Basic Implementation PR](https://github.com/HTTP-APIs/hydra-python-agent/pull/13) -  [Querying Mechanism Implementation](https://github.com/HTTP-APIs/hydra-python-agent/pull/23)


After all the discussion and conclusions we settled onto a layered model of abstraction that consists of three layers. The picture below has the different modules in the code and their functions.

<div style="height: 30px"></div>

<img width="1000px" src="/static/assets/img/blog/2019-06-15-hydra-smart-agent-foundations/structure.png" alt="agent-structure" style="display: block; margin-left: auto; margin-right: auto;">

<div style="height: 30px"></div>

### Each layer and its function

1. The first layer should contain and encapsulate the low-level interaction when making http requests to the server and also interacting internally with Redis

2. Now the second layer is an Agent interface with should enable users to be able to do all basic crud operations while being able to use and support all Hydra features.

3. Last we have a high-level layer which will be the development and enhacement of a querying format that should enable natural language-like querying for a even higher level of abstraction for the user. He should be able to ask for things like "Show me temperature for city Berlin"

With that structure the development was started and can be followed up at [PR #109](https://github.com/HTTP-APIs/hydra-python-agent/pull/109). Basically we want for now to provide a working basic structure for the Agent module making use of graphutils operations and a lasting Session. Ultimately we are now able to do something like:

Hey, thanks for reading. Hope to see you on the next blog post!

[^2]: A.K.A. - Also known as
[^1]: PoC - Prove of Concept

---


