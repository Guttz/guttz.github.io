---
layout: post
title:  "Google Summer of Code 19' Summary - Hydra Ecosystem"
date:   2019-07-24
desc: "Summary of my journey and contributions in GSoC 19' at Hydra Ecosystem | Reading Time: 4 minutes"
keywords: "Hydra,linked-data,hypermedia,webapis,websematics"
categories: [Gsoc19]
tags: [Hydra, GSoC'19, Hypermedia APIs, Linked Data, Hydra Ecosystem]
icon: icon-google
redirect_from:
  - /y8wc
  - /hydra-eco-gsoc19
---

Hello reader, I'm Gustavo Morais and this post aims to summarize my journey throughout Google Summer of Code 19'. It has been three months of intensive learning and a remarkable experience as a developer which was divided into three coding phases. I'll divide each phase citing its goal, small description and core PR links attached to each. In the end, I'll add some diverse auxiliary merged contributions.

[Our community](https://github.com/HTTP-APIs) has different tools including a working server that can be built from a Hydra Doc, a.k.a[^2] [hydrus](https://github.com/HTTP-APIs/hydrus). And also, we have a [^1]Python Agent to work with that server, [a.k.a Hydra Agent](https://github.com/HTTP-APIs/hydra-python-agent). I and my GSoC college Vishal Desai had a series of improvements to make, in which Vishal was leading hydrus' changes and I was responsible for the Agent side.

-----------
## [Phase 1] - Smart Agent Foundations


The first period of GSoC was market by intensive study and discussions. I was in charge of developing the Python Hydra Agent to take it from being an only-GET PoC Command Line tool to a Pre-Alpha release supporting all basic operations of the Hydra Agent. For that, I created a document in which I detailed and discussed with the mentors the architecture and then proceed to the coding and lastly documenting it. References for this phase are as below:
  
#### Goal:

- Implemented basic working Agent able to execute all CRUD operations through GET, PUT, POST and DELETE with URLs only.

#### Pull Request:

1. [[PR 109] Agent's first implementation 46 commits](https://github.com/HTTP-APIs/hydra-python-agent/pull/109) 

#### Extra reference links

- [Design Doc Available at Google Docs with Discussion](https://docs.google.com/document/d/189TNgNHFG79U68fJtqz12lhACDvMqzYEUIShv9Bl2RI/edit?usp=sharing)
- Ref. base links and older issues: [Redis as Database](https://www.hydraecosystem.org/hydra-agent-redis-graph) - [Graph Structure Issue](https://github.com/HTTP-APIs/hydra-python-agent/issues/18) - [Redis Basic Implementation PR](https://github.com/HTTP-APIs/hydra-python-agent/pull/13) -  [Querying Mechanism Implementation](https://github.com/HTTP-APIs/hydra-python-agent/pull/23)

<br>

__Detailed blog post__ : https://gustavomorais.me/y8wx

-----------
## [Phase 2] - Agent All Set Up

The second phase of GSoC 19' consisted of several different new features and enhancements developed separately to bring the Agent to its completeness. These included being able to use Hydra's descriptive concept to search resources by types and properties while updating some core packages, fixing bugs and making our caching mechanism with Redis more stable and complete.

#### Goal:
- Implement hydrus and Redis searching by type/properties, bug fixes, and core packages updating.

#### Pull Requests:

- New Features
1. [[PR 118] Recursive Node Creation for Linked Data](https://github.com/HTTP-APIs/hydra-python-agent/pull/118)
2. [[PR 120] Querying by classes and properties for Redis cached data](https://github.com/HTTP-APIs/hydra-python-agent/pull/120)
3. [[PR 121] Server queries with classes and properties using IRI Template](https://github.com/HTTP-APIs/hydra-python-agent/pull/121) 

- Enhancements and Inconsistencies 
1. [[PR 115] Project Layout modification](https://github.com/HTTP-APIs/hydra-python-agent/pull/115)
2. [[PR 117] Duplicate Nodes when committing](https://github.com/HTTP-APIs/hydra-python-agent/pull/117)
3. [[PR 119] Updating Core Packages](https://github.com/HTTP-APIs/hydra-python-agent/pull/119)

<br>

**Detailed blog post** : https://gustavomorais.me/y8wa

-----------
## [Phase 3] Power up and Graph it up
As the last phase started, we had covered a considerable amount of the core goals we had and now was time to close things out with a some last additions. We wanted our Agent to be even more powerful and then build a GUI(Graphical User Interface) so users can easily try the Agent and easily try deployed hydrus servers with a generic interface that is built according to the API Hydra Documentation.

#### Goal:
- Build a GUI for the Agent, deliver a Synchronization Mechanism between Client and Server and wrap up bug fixes and documentation.

### Pull Requests:
1. [[PR 123] Synchronization Mechanism](https://github.com/HTTP-APIs/hydra-python-agent/pull/123)
2. [[PR 125] Agent GUI 1/2](https://github.com/HTTP-APIs/hydra-python-agent/pull/125)
3. [[PR 128] Agent Documentation Update](https://github.com/HTTP-APIs/hydra-python-agent/pull/128)
4. [[PR 1] Agent GUI 2/2](https://github.com/HTTP-APIs/hydra-python-agent-gui/pull/1)
5. [[PR 4] Agent GUI Documentation](https://github.com/HTTP-APIs/hydra-python-agent-gui/pull/4)

-----------
## Auxiliary Contributions
There were a couple of additional contributions made along the way and during the Community Bonding that will be mentioned for consistency but were mainly auxiliary: 

### Blogging
An introductory article on the Hydra Ecosystem: https://gustavomorais.me/hydra-agent-101

### hydrus

1. [[PR 387] Last remnants of hydraspec](https://github.com/HTTP-APIs/hydrus/pull/387) 
2. [[PR 388] General config fixes on hydrus cli](https://github.com/HTTP-APIs/hydrus/pull/388) 
3. [[PR 389] Setting up migrations with Alembic](https://github.com/HTTP-APIs/hydrus/pull/389) 
4. [[PR 390] Avoid common installation errors](https://github.com/HTTP-APIs/hydrus/pull/390) 
5. [[PR 400] Adding DELETE operation to drone collection](https://github.com/HTTP-APIs/hydrus/pull/400)

### Python Agent
1. [[PR 106] Adding github template files](https://github.com/HTTP-APIs/hydra-python-agent/pull/106) 
2. [[PR 108] Rebasing outdated develop to master](https://github.com/HTTP-APIs/hydra-python-agent/pull/108) 

### http-apis-github.io
1. [[PR 37] Useful info, layout, fixes](https://github.com/HTTP-APIs/http-apis.github.io/pull/37) 
2. [[PR 38] Deployment instructions, doc readability, warnings](https://github.com/HTTP-APIs/http-apis.github.io/pull/38) 

## Acknowledgments
The GSoC 19' journey now comes to an end. I have to say it was personally a really important development phase for me and an experience that I'll remember forever. I feel now that I've progressed as a Software Developer and also that we were able to progress and take Hydra Ecosystem development a little bit further with a group of people spread in the world. 

And of course, all of that would not be possible without the support, time and will of my mentors [Akshay](https://github.com/xadahiya), [Chris](https://github.com/chrizandr) and [Lorenzo](https://github.com/Mec-iS). Whom have been, not only this summer but for years, cultivating the Hydra Ecosystem and giving their time and effort to develop this concept and provide the opportunity for other people to join and learn. My honest thanks for the knowledge and trust you've shared with me, it's something that I'll take with me in my career to become a better professional. I also have to give a huge shout out to my GSoC partner [Vishal Desai](https://github.com/vddesai1871) that helped me in most of my journey doing a meticulous work in hydrus and helping me with questions and concepts. He also has a great view of the world from which I've learned.

[^1]: Hydra CG - https://www.hydra-cg.com/
[^2]: Hydra Python Agent - https://github.com/HTTP-APIs/hydra-python-agent

---

### About the Author <br>
[Gustavo Morais - Software Developer](https://gustavomorais.me/) <br>
*Google Summer of Code Participant - Hydra Ecosystem* <br> <br>
Website: https://gustavomorais.me/ <br>
Linkedin: https://www.linkedin.com/in/gustavo-morais/ <br>
Github: https://github.com/guttz/ <br>

---
