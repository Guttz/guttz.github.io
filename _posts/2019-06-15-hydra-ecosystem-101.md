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

So let's say, **you are creating an application** that would like to have the weather shown to the user. You browsed through the thousands different Weather APIs and you picked the famous free [OpenWeather](https://google.com/weather/SaoPaulo) API. You choose between all the different interfaces they provide

--insert open weather interfaces--

You scroll down through the list of possibilities.. You can get the weather by ZIP Code, Rectangle Geographic Zone, Latitude and Longitude, City Name and more. You decide you want to do it **by city name**. 

--insert image--

Do you see how much information you had to manually proccess and investigate to finally be able to achieve your goal? What do you think would happen if we automate all the process above and create APIs with a standard highly descriptive specification that allows machines to understand what each API offer and also how to interact with them. That's exactly the first thing the Hydra Ecosystem provides. 

You can create an API using Hydra Spec, seamlessly host it with the hydrus and  query it using our Hydra Python Agent. In the end, what's possible is that if you want to query an API for it's weather all you have to do is:

```
agent.get("temperature", {city: "São Paulo"})
```
What happened here is that all the human-interpreting I had to do before is automated. The agent is a Smart Agent, therefore all I have to tell him is what I want, he will be able to construct the correct HTTP request and fetch exactly what I need.

Okay, but let's say we followed OpenWeather documentation and now we can get São Paulo's weather by https://google.com/weather/SaoPaulo. You put this into your application that has a bunch of users everyday, it works perfectly for 3 years and then.. OpenWeather decides to change the way you get São Paulo's weather to https://google.com/city/SaoPaulo/weather and all of a sudden you have to update your codebase all around with tousands of API calls or your application will break. Now imagine this happening to all the APIs you use.. constantly throughout the years.. multiple times.

By using Hydra as a way of decoupling the information from the server structure, it enables for changes to be made server-side and nothing needs to be changed at the Agent. 

## Linked Data

Maybe simply looking for the weather of a city isn't a hard task and you wonder if Hydra can deal more complex data. The thing is, Hydra is built on top of Linked Data, which basically means that the Data stored by our hydrus server not only has the attributes for each resource but also relationships describing how the Data relates to each other! That means you can build precise and detailed queries and fetch your information.

```
agent.get("temperature", {country: "Brazil", has_forecast: "rain"})
```

So we get the resource we want not only by one property, buy however we want combining all the properties the resource supports and also the relationships/properties that the resource is related to!

## Scaling

One last interesting core feature that the Hydra Ecosystem aims to provide is that all the above is supported in a scalable parallel decoupled manner. That said, imagine you have created your small hydrus server to provide the temperature for a bunch of microcontrollers at your home. If you decide to deploy one more hydrus server in the same network, your microcontrollers should be able to automatically detect the new server and already be able to make requests to it. With that, if the first server shuts down or changes, the Agents are able to know that the new server is also able to provide temperature.

---
