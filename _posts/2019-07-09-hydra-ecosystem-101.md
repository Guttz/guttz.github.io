---
layout: post
title:  "Next Generation Hypermedia APIs - Hydra Ecosystem"
date:   2019-07-08
desc: "Quick read on what's the Hydra Ecosystem and Hypermedia Descriptive APIs | Reading Time: 5 minutes"
keywords: "Hydra,linked-data,hypermedia,webapis,websematics"
categories: [Gsoc19]
tags: [Hydra, GSoC'19, Hypermedia APIs, Linked Data, Hydra Ecosystem]
icon: icon-google
redirect_from:
  - /y8wx
  - /hydra-agent-foundations
---

Today we live in a **connected and distributed world**, one that enables us to quickly build applications and solutions that **can fetch Google Maps addresses from US, get the weather from India, check the currency exchange from anywhere** in the world and much more. This enables us to build such complete and powerful applications and it sounds awesome, right? Well, it's not so easy as it sounds... For these information to be provided each of these services are provided by what we call APIs(Application Programming Interfaces), and well... **interfaces come in all shapes, sizes and formats**. 

This post is focused on describing in a simple language what's the Hydra Ecosystem and how it can make easier to provide a new Linked and Rich World Wide Web. For that, **I'll guide you through an application implementation process** as the developer partner, so you can understand **how connecting services(APIs) are done today, and how they could be done tomorrow**. 

-------------
## Descriptive Interfaces

So let's say, we are a team that has to create an application that would like to have **the weather shown to the user**. We browse through hundreds of different Weather Services(APIs) and after reading about a dozen of architectural styles and formats we finally pick one, the [OpenWeather](https://openweathermap.org/) API. Although we selected the API, itself has various different ways of providing the information we need.

<div style="height: 30px"></div>
<img src="/static/assets/img/blog/2019-07-09-hydra-ecosystem-101/openweather_services.png" alt="agent-structure" style="display: block; margin-left: auto; margin-right: auto; width: 900px; max-width: 100%;">
<div style="height: 30px"></div>


We browse and read through the list of possibilities. We can get the weather by ZIP Code, Rectangle Geographic Zone, Latitude and Longitude, City Name and more. We decide we want to do it the simplest way, **by city name**. 

<div style="height: 30px"></div>
<img src="/static/assets/img/blog/2019-07-09-hydra-ecosystem-101/city_service.png" alt="agent-structure" style="display: block; margin-left: auto; margin-right: auto; width: 650px; max-width: 100%;">
<div style="height: 30px"></div>

In the end we have to create a request like this to fetch the information we need:

```
http://api.openweathermap.org/data/2.5/weather?q=SaoPaulo
```

Do you realize how much information we had to manually process and investigate to finally be able to achieve our goal? What do you think would happen if we automate all the process above and create interfaces with a standard highly descriptive specification that allows machines to understand what each API offer and also how to interact with them. That's exactly the first thing the Hydra Ecosystem provides. 

You can create an interface description saying what your API should provide using Hydra Specification, seamlessly host it with our hydrus server and query it using our Hydra Python Agent. In the end, what's possible is that with this is if you want to get the weather for a city all you have to do is:

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
Okay, but let's say we followed OpenWeather's documentation and now we can get São Paulo's weather, everything works fine for a couple of months. However, now OpenWeather decided to change its interface: 


Okay, but let's say we followed OpenWeather's documentation and now we can get São Paulo's weather by https://google.com/weather/SaoPaulo. You put this into your application that has a bunch of users everyday, it works perfectly for 3 years and then.. OpenWeather decides to change the way you get São Paulo's weather to https://google.com/city/SaoPaulo/weather and all of a sudden you have to update your codebase all around with tousands of API calls or your application will break. Now imagine this happening to all the APIs you use.. constantly throughout the years.. multiple times.

By using Hydra as a way of decoupling the information from the server structure, it enables for changes to be made server-side and nothing needs to be changed at the Agent. 

## Linked Data

Maybe simply looking for the weather of a city isn't a hard task and you wonder if Hydra can deal more complex data. The thing is, Hydra is built on top of Linked Data, which basically means that the Data stored by our hydrus server not only has the attributes for each resource but also relationships describing how the Data relates to each other! That means you can build precise and detailed queries and fetch your information.

```
agent.get("temperature", {country: "Brazil", has_forecast: "rain"})
```

So we can get the resource we want not only by one property, buy however we want combining all the properties the resource supports and also the relationships/properties that the resource is related to!

## Scaling

One last interesting core feature that the Hydra Ecosystem aims to provide in the future is that all the above is supported in a scalable parallel decoupled manner. That said, imagine you have created your small hydrus server to provide the temperature for a bunch of microcontrollers at your home. If you decide to deploy one more hydrus server in the same network, your microcontrollers should be able to automatically detect the new server and already be able to make requests to it. With that, if the first server shuts down or changes, the Agents are able to know that the new server is also able to provide temperature.

If you've find the above interesting and you're a developer, consider joining our Open Source Community! If you're just an technology enthusiast make sure to follow us on Twitter to stay tuned. 
---
