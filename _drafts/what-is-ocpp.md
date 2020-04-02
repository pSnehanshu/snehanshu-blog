---
layout: post
title:  "What is Open Charge Point Protocol?"
categories: [electric-vehicles, ocpp]
---
![OCPP illustration](/assets/post-images/ocpp/ocpp.svg)

The [Electric Vehicle]({% post_url 2018-06-14-electric-vehicles-in-india-and-why-they-arent-already-here %}), or EV for short, are the new kind of vehicles that will slowly phase out the tradtional pertrol and diesel based vehicles. The pros are immense - environment friendly, low maintanance cost, low sound pollution etc. The only major con is the availability of public charging points. Although this is improving with every passing day, there is still a long time before an average buyer will become confident enough to choose EV over a traditional vehicle.

Traditional petrol pumps are very simple, there are human operated machines that fill the tank of the vehicles, other than showing the amount of fuel filled and the cost, it doesn't has any functionality. But EV charging points were designed to be smart from the beginning. Features such as card based authentication, remote control by charge point operator, remote configuration, smart grid based charging are very common.

With this feature set, there comes a problem, the charge points are tightly coupled with the backends (dashboards that operators use to remotely control the charge points) due to proprietary protocols. This means, if an operator buys a dashboard, then he can install charge points of only those brands which the backend supports. This is a problem. To solve this issue, there needs to be a standard protocol, which has to be open sourced, and must be followed by backend vendors and charge point vendors to enable interoperability. That's OCPP.

OCPP or [Open Charge Point Protocol](https://www.openchargealliance.org/protocols/ocpp-16/) is a protocol that was designed by [ElaadNL](https://www.elaad.nl/) as a standard for communication between a charging point and the backend software.

## Versions of OCPP

OCPP has version [1.5](https://www.openchargealliance.org/protocols/ocpp-15/), [1.6](https://www.openchargealliance.org/protocols/ocpp-16/) and [2.0.1](https://www.openchargealliance.org/protocols/ocpp-201/) till now.

OCPP 1.5 is officially based on [SOAP](https://en.wikipedia.org/wiki/SOAP), but there are also unofficial implementations based on JSON-over-Websocket. OCPP 1.6 officially supports both SOAP and JSON-over-Websocket. OCPP 2.0.1 officially supports only JSON-over-Websocket.

To avoid confusion, the version numbers are always suffixed with an "s" or "j", which respectively mean "SOAP" and "JSON-over-Websocket". e.g. ocpp1.5s, ocpp1.6j, ocpp2.0.1j etc.

Personally I'm not a big fan of SOAP, I will write down some disadvantages of SOAP from the top of my head.

* SOAP uses [XML](https://en.wikipedia.org/wiki/XML) to represent data, which is very bulky, not good for charge points which may have unreliable internet connectivity.

* SOAP requires both the backend and the charge point to act as servers to facilitate two-way communication. This is also bad because it eliminates the possibility of operating several charge points behind the same router, because to become a server, each charge points will require unique IP addresses, which is costly.

* SOAP is an old and outdated technology invented at Microsoft. Modern developers don't bother learning it (including me ;-) ). If your solution is based on SOAP, you may have a hard time finding good developers.

Therefore I am not going to discuss how the SOAP versions work, because neither do I know. JSON-over-websocker versions overcome all of the above shortcomings.

* As the name suggests, it uses [JSON](https://www.json.org/json-en.html) to represent data, which is way much leaner than XML.

* It relies on [websocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API)
for two-way communication, which requires only one entity to act as a server, which in OCPP's
case is the backend.

* JSON-over-Websocket is a very modern technology and most modern apps already use it. You will find plenty of good developers.

Creators of OCPP also realized their mistake, and therefore the latest version only supports JSON-over-Websocket.

## Let's talk about websocket

Websocket is an advanced protocol that works on top of [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP). Traditional HTTP is client-driven, that means it is the client who initiates a request and the server only responds. The server cannot send any data to the client without the client requesting it. It made sense in the early days of the internet when websites were mostly static, but think about modern apps, like [facebook messeger](https://www.messenger.com/). Whenever someone sends a message, the server needs to notify the client of the receiver in order to display the new message on the screen without reloading the page. Before the advent of websocket, developers used a little hack called [long-polling](https://www.pubnub.com/blog/http-long-polling/) to achieve the same functionality. Basically, the client will keep requesting the server for any updates at a fixed interval of time. If the server has updates, it will immediately send those updates as response to those requests. If the client received any updates, then it can act accordingly, i.e. show the message on the screen. But the disadvantage is that it is not that realtime, and if you reduce the interval, then there's a danger of overloading the server with too many useless requests.

Therefore websocket was invented. Here, the client still needs to initiate the connection and do the handshake, but once that's done, a two-way tunnel is created in which data can flow in both directions independently. The client can send messages to the server and the server too can send messages to the client.

Coupled with JSON, it becomes the perfect solution for OCPP, where the charge points act as clients and initiate the connection, and the backend acts as the server. No need to worry now, put as many as chargers you need behind that router, websocket got you covered!

---

That's all for this post. I will write about the technical details on how OCPP with JSON-over-Websockets functions, and I will also post some code examples about how to implement OCPP using Node.js in the next post. So stay tuned!
