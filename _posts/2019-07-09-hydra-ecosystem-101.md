---
layout: post
title:  "Next Generation Hypermedia APIs - Hydra Ecosystem"
date:   2019-07-08
desc: "Next Generation Hypermedia APIs and Hydra Ecosystem | Reading Time: 5 minutes"
keywords: "Hydra,linked-data,hypermedia,webapis,websematics"
categories: [Gsoc19]
tags: [Hydra, GSoC'19, Hypermedia APIs, Linked Data, Hydra Ecosystem]
icon: icon-google
redirect_from:
  - /y8wb
  - /hydra-agent-101
---

Today we live in a **connected and distributed world**, one that enables us to quickly build applications and solutions that **can fetch Google Maps addresses from US, get the weather from India, check the currency exchange from anywhere** in the world and much more. This enables us to build such complete and powerful applications and it sounds awesome, right? Well, it's not so easy as it sounds... For these information to be provided each of these services are provided by what we call APIs(Application Programming Interfaces), and well... **interfaces come in all shapes, sizes and formats**. 

This post is focused on describing in a simple language **what is the Hydra Ecosystem** and how it can make it easier to provide a new data-rich and linked World Wide Web. For that, **I'll guide you through an application implementation process** as the developer partner, so you can understand **how connecting services(APIs) are done today, and how they could be done tomorrow**. 

<div style="height: 30px"></div>
<img src="/static/assets/img/blog/2019-07-09-hydra-ecosystem-101/hydra_network.png" alt="hydra_ecosystem" style="display: block; margin-left: auto; margin-right: auto; width: 750px; max-width: 100%;">
<div style="height: 30px"></div>

-------------
## Descriptive Interfaces

So let's say, we are a team that has to create an application that would like to have **the weather shown to the user**. We browse through hundreds of different Weather Services(APIs) and after reading about a dozen of architectural styles and formats we finally pick one, the [OpenWeather](https://openweathermap.org/) API. Although we selected the API, there are still various different ways of querying for the information we need as you can glance below.

<div style="height: 30px"></div>
<img src="/static/assets/img/blog/2019-07-09-hydra-ecosystem-101/openweather_services.png" alt="agent-structure" style="display: block; margin-left: auto; margin-right: auto; width: 900px; max-width: 100%;">
<div style="height: 30px"></div>


We browse and read through the above list of possibilities. We can get the weather by ZIP Code, Rectangle Geographic Zone, Latitude and Longitude, City Name and more. We decide we want to do it the simplest way, the current weather **by city name**. 

<div style="height: 30px"></div>
<img src="/static/assets/img/blog/2019-07-09-hydra-ecosystem-101/city_service.png" alt="agent-structure" style="display: block; margin-left: auto; margin-right: auto; width: 650px; max-width: 100%;">
<div style="height: 30px"></div>

In the end we have to create a request like this to fetch the information we need:

```
http://api.openweathermap.org/data/2.5/weather?q=SaoPaulo
```

Do you realize how much information we had to manually process and investigate to finally be able to achieve our goal? What do you think would happen if **we automate all the process above** and create interfaces with a standard highly descriptive specification that allows machines to understand what each API offer and also how to interact with them. **That's exactly the first thing the Hydra Ecosystem provides.** 

You can create an interface description saying what your API should provide using Hydra Specification, seamlessly host it with our hydrus server and then query it using our Hydra Python Agent. In the end, what's possible is that with this is if you want to get the weather for a city all you have to do is:

```
agent.get("Weather", {city: "São Paulo"})
```

Do you want to get it by zipcode? 
```
agent.get("Weather", {zipcode: "80230-000"})
```
Want to combine both queries?
```
agent.get("Weather", {city: "São Paulo", zipcode: "80230-000"})
```

**You can simply semantically express what you need.** What happened here is that, all the human-interpreting and reading I had to manually do before to understand how the service interface(API) works, now is automated. The agent is a Smart Agent, therefore all I have to tell him is semantically what I want and **he will be able to construct syntactically the correct HTTP request** and fetch exactly what I need.

-------------

## Stable Interfaces
Okay, but let's say we followed OpenWeather's documentation and now we can get São Paulo's weather, everything works fine for a couple of months. However, now OpenWeather decides to update and change its interface format: 

<div style="height: 30px"></div>
<img src="/static/assets/img/blog/2019-07-09-hydra-ecosystem-101/api_stable.png" alt="agent-structure" style="display: block; margin-left: auto; margin-right: auto; width: 650px; max-width: 100%;">
<div style="height: 30px"></div>

The above modification seems pretty legit, right? It changes the version number from `2.5` to `3.0`, makes the parameter more clear from `q` to `city`. Well, I have to tell you that's a big headache for a software developer. We have dozens of this API call around our application fetching the weather and we now have to update and test all of them or our application will break. And yeah, this happened once but might actually happen multiple times depending on our application longevity.

By using Hydra, server-side changes to our interface can be decoupled from the user. We changed the API version and the internal naming for parameters, but our user request remains exactly the same:
```
agent.get("Weather", {city: "São Paulo"})
```
**Nothing changed. No breaking application.** Since our API is described with Hypermedia in a machine processable manner, the client is **an Agent that is capable of automatically understand he new interface too** and identify where and how to get the weather!

--------------
## Linked Data

Maybe simply looking for the **weather of a city isn't a hard task** and you wonder if Hydra can deal more complex data. The thing is, Hydra is built on top of Linked Data, which enables you to do things like:

```
agent.get("Weather", {country: "Brazil", has_forecast: "rain"})
```

In this example, forecast is not an information that is already contained in the Weather when we fetch it, however, **the Weather object has a link** to another existing information that are the Forecasts. Linked Data basically means that the data stored by our server not only has the information for each resource but also a link that describes how the data relates to other information also available on the web! In the end, this enables the Agent to query and filter resources not only by one property, but however we want **combining all the properties the resource supports and also the relationships/properties that the resource is linked to!**

--------------
## Scaling and Future

One last interesting feature that the Hydra Ecosystem aims to provide in the future is that all the above to be supported in a easily scalable parallel decoupled manner. That said, imagine we have created our small hydrus server to provide the Weather for a bunch of Smart Devices at our office.

Nonetheless, we decide that we want our office to be more technological and we also have a new hydrus server that will be able to provide Luminosity and Humidity for all meeting rooms. In that scenario, the Agent should be able to automatically detect the new server and already be able to know what kind of data it provides and make requests to it.

With that, if there are some of our Smart Devices that can make use of this information, they should automatically detect and start to use it. So if we have a Smart Window in our rooms, for example, it should automatically start to use not only the weather but also the luminosity and humidity to determine how it should behave along the day with different weather, luminosity and humidity.

--------------
## Want to know more?
**If you're not a developer** but a technology enthusiast make sure to [follow the Hydra Ecosystem on Twitter](https://twitter.com/HydraEcosystem) to stay tuned!

**If you're a developer** and found the above interesting, there's plenty more from what this came from. You can consider joining [our Open Source Community](https://github.com/HTTP-APIs/), read some introductory technical material [at our home page](https://www.hydraecosystem.org/) or reach out to us in [our Open Gitter chat!](https://gitter.im/HTTP-APIs/Lobby)  

**Links:**

- https://github.com/HTTP-APIs/
- https://www.hydraecosystem.org/
- https://gitter.im/HTTP-APIs/Lobby
- https://gustavomorais.me/gsoc19/
- https://twitter.com/HydraEcosystem

---

Thanks for reading all of this! Drop your like/comment below to support me.

[^1]: Hydra - Hypermedia Driven API

**About the Author** <br>
[Gustavo Morais - Software Developer](https://gustavomorais.me/) <br>
Website: https://gustavomorais.me/ <br>
Linkedin: https://www.linkedin.com/in/gustavo-morais/ <br>
Github: https://github.com/guttz/ <br>