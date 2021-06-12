# Intro

Welcome to the Pegasus API Documentation!

This documentation gives you access to the entire Pegasus stack, including live communication, device interaction and historical data. The documentation is designed to make it easy to interact with DCT's suite of [IoT devices](https://www.digitalcomtech.com/devices-2018/) that are currently supported on Pegasus, allowing you to deploy highly customizable solutions.

If you find yourself stuck or don't have a clear understanding of what's going on, please don't hesitate to post issues on our public repo and join the conversation in our [Gitter](https://gitter.im/dctdevelop/pegasus?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge). 

The documentation was generated using [Slate](https://github.com/tripit/slate).

## Background

Digital Communication Technologies [(DCT)](http://digitalcomtech.com) manufactures intelligent [devices](https://www.digitalcomtech.com/devices-2018/) which collect data from different sensors and [accessories](https://www.digitalcomtech.com/accessories) as well as GPS satellites simulatenously.

These intelligent devices send all their data in realtime to a server called: [Pegasus Gateway](http://digitalcomtech.com/products/application-enablement-platform/) or just Pegasus.  Pegasus works as a central server where all the data is collected, analyzed and then exposed via the API for easy viewing.

The API allows you to query any of the 300+ parameters the devices and their sensors expose. It is also a way for you to interact directly with the device, allowing you to make changes to the configuration, create rules, customizable actions depending on device states, and much more with easy to understand API commands.


## Pegasus Overview

Multiple Pegasus servers can exist, in fact there are multiple Pegasus servers deployed around the world, each server having a custom URL or subdomain which is controlled by a Pegasus owner via a licensed fee. 

![pegasusOverview image](slate/img/pegasusoverview.png "pegasusOverview image")

At the core of each Pegasus site, the API is the same.  The requests are built the same way, the only difference being the base URL.

## Base URL

> `https://sitedomain.com/api`

> [`https://pegasus1.pegasusgateway.com/api`](https://pegasus1.pegasusgateway.com/api)

The base URL is the domain where the Pegasus server is hosted, in our case it's used to identify where the requests will be built from.

<aside class="notice">If you are making an application that applies to more than one Pegasus site, it's recommended to leave the base URL configurable.</aside>


## API Reference

> `https://sitedomain.com/api-static/docs`

> [`https://pegasus1.pegasusgateway.com/api-static/docs`](https://pegasus1.pegasusgateway.com/api-static/docs)

The Pegasus API reference allows you to test all the methods directly with your site, it can be found under the path `/api-static/docs`

## Demo

> [API Reference Login](https://pegasus1.pegasusgateway.com/api-static/docs/#api-Authentication-Login)

> `user: developer@digitalcomtech.com`

> `pass: deV3lopErs`

Throughout the guide we'll work on a Demo server called: **Pegasus1**

The vehicles we'll work with are:

vehicle_id | name | imei | device | features |
---:|:-----|:----:|:-------|:---------|
2600 | PruIng1-TDX566 | 356612022409637 | Syrus 2 | Engine data
1956 | (1) TEST HALL | 357042062920955 | Syrus 3G BT | Photo data
691 | Stand 4 - Syrus Mobile | 357042063193172 | Syrus 3G BT | Device outputs (sensors / buzzers)
3168 | Syrus OBDII SO3G-3 | 357041069434986 | Syrus OBDII | Engine data
3005 | Titan 4G | 869912030003715 | Titan | GPS data & temperature

## Versioning

> [/api](https://pegasus1.pegasusgateway.com/api/)

```json
{
  "domain": "pegasus1.pegasusgateway.com",
  "name": "6.0.1-aws",
  "live-url": "https://aws-live-0.pegasusgateway.com",
  "pegasus_id": 1,
  "live_url": "https://aws-live-0.pegasusgateway.com",
  "tag": "6.0.1-aws",
  "date": "Dec 22 09:41:47 2020",
  "_deprecated": [
    "live-url"
  ]
}
```

To consult the version of your API use the `/api` endpoint.  

field | description
---:|:-----
date | Time of last update 
live-url | Socket communication endpoint
tag | Version tag
name | `major.minor.patch`

Breaking changes may occur on major and minor numbers. Patches, are not expected to affect compatibility.

For future version releases and changelog go to [Github - Releases](https://github.com/dctdevelop/pegasus/tree/master/releases).

Also [subscribe to our mailing list](http://developers.digitalcomtech.com/#mc_embed_signup_scroll) for updates.

<aside class="notice"> Whenever an update is made to the core API, you should use <b>Pegasus 1</b> to test the compatibility with your app. Pegasus 1 will always be updated with any critical changes prior to releasing to Pegasus servers.</aside>

## Resource Patterns
Resources URI patterns try to follow the general convention:

`/vehicles`
represents all the vehicles

`/vehicles/:id`
represents a particular vehicle ID

`/vehicles/:id/remote/state`
see the state of a particular vehicle ID

`/vehicles/:id/routes`
see the routes of a particular vehicle ID

etc.

## Pagination

> [`api/vehicles?set=2&page=2`](https://pegasus1.pegasusgateway.com/api/vehicles?set=2&page=2)

```json
{
  "set": 2,
  "total": 5,
  "data": [...],
  "page": 2,
  "pages": 3
}
```

Requests to a collection of resources (such as: [Users](/#users), [Vehicles](/#vehicles), [Devices](/#devices)) are automatically paginated and will return a set of 200 resources by default.
You can request up to 1000 entries per page (up to 500 Devices) by using the `set` parameter.

You will see the following additional parameters in the request.

parameter | description
----:| ----
set | Number of items in current page
page | current page
pages | total number of pages
total | total number of items
