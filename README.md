# Week 7
# Tauspace Developer Internship 

## Outcomes:
1. Understand API's
2. Build a simple Elixir REST API.
3. Document the API endpoints.

### Prerequisites

Your computers support Hyper-V.
This needs to first be enabled in the BIOS, then the feature activated in windows.

To enable Hyper-V using PowerShell:
```
Press the Win key, and type powershell. ...
In the PowerShell window, type the following shell command and hit Enter: Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All.
PowerShell will run the cmdlet and initiate the Hyper-V enabling process.
```
[Setup local virtual POSTGres environment in Hyper-V](https://us.ovhcloud.com/community/tutorials/how-to-install-pg-ubuntu/)

We will be using Postgres on ubuntu in your Hyper-V VM.

## APIs

### What is an API?

API stands for Application Programming Interface. It's a set of rules and protocols that allows different software applications to communicate with each other. Think of it as a menu in a restaurant. You, the customer, have a list of dishes you can order, prepared by the kitchen. The waiter (API) takes your request to the kitchen and brings back the dish you ordered. In this analogy:
- You are the user.
- The order represents the request.
- The kitchen is the system or application that processes the request.
- The dish that is served to you is the response.
- In a technical context, when you use an app on your phone to check the weather, the app sends a request to a server via an API. The server processes the request, retrieves the data, and sends it back to your app, which then displays the weather to you.

### Why are APIs important?

* Interoperability: APIs allow different software systems, which might have been developed independently and possibly in entirely different programming languages, to work together.
* Modularity: Systems can be built modularly, with different components interacting through well-defined APIs. This makes development, scaling, and troubleshooting easier.
* Security: Instead of giving direct access to databases or core systems, APIs allow controlled access, exposing only what's necessary.
* Efficiency: APIs can provide a standardized way to provide and receive data, which can lead to faster development and integration times.
* Economic Value: Companies often monetize their data and services through APIs. For instance, cloud providers offer a plethora of services accessed primarily through APIs.

### Different types of APIs:
* REST (Representational State Transfer): REST is an architectural style for distributed systems. RESTful APIs use HTTP requests to perform CRUD (Create, Read, Update, Delete) operations on resources, which are represented as URLs.

* GraphQL: Developed by Facebook, GraphQL is a query language for APIs. Instead of having multiple endpoints, GraphQL APIs typically have a single endpoint that can interpret and respond to complex queries, allowing clients to request only the data they need.

* SOAP (Simple Object Access Protocol): A protocol, primarily used for web services. It's known for its robustness and has built-in error handling. However, it's considered heavier than REST.

* WebSockets: Unlike the request-response model of REST and GraphQL, WebSockets allow a continuous two-way communication between the client and server. It's useful for applications that require real-time functionality.

* gRPC: Developed by Google, gRPC is a high-performance, open-source framework that uses HTTP/2 for transport and Protocol Buffers as the interface description language.

## OAS 3.0 Specification

* [Rest API and OpenAPI](https://www.youtube.com/watch?v=pRS9LRBgjYg&pp=ygUHb3BlbmFwaQ%3D%3D)
* [Better API Design with OpenAPI](https://www.youtube.com/watch?v=uBs6dfUgxcI&pp=ygUHb3BlbmFwaQ%3D%3D)
* [OpenAPI 3.0 Specification](https://spec.openapis.org/oas/latest.html)

1. Introduction - What is OAS?
- OAS (formerly known as Swagger) is a specification for building APIs. OAS defines a standard, language-agnostic interface to RESTful APIs.
Allows both humans and computers to understand the capabilities of a service without accessing its source code or seeing further documentation.

2. Major Components

- OpenAPI Object: The root document object of the OAS document.

- Info Object: Provides metadata about the API like title, version, and other details.

- Paths Object: Defines the available paths and operations for the API.

- Components Object: A set of reusable objects for different aspects of the OAS. Can include schemas (like JSON schemas for request and response bodies), parameters, responses, examples, request bodies, headers, security schemes, links, callbacks, and more.

[IANA Status Codes](https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml)
[OpenAPI Definitions Designer](https://openapidesigner.com/)



[Please read this page](https://blog.logrocket.com/build-rest-api-elixir-phoenix/)


## Course Material on Setting up an API

* [0. First Endpoint in Elixir](https://www.youtube.com/watch?v=UgQTcvdbccA)

* [1. Spin up a Postgres Docker Container locally](https://www.youtube.com/watch?v=LGY_eILc8Ks)

* [2. Using Phoenix Framework to Create an Elixir REST API Project](https://www.youtube.com/watch?v=s3WNCjN4Pes)

* [3. Using Phoenix Framework to Generate JSON Resources for Elixir REST API Project](https://www.youtube.com/watch?v=DRsKmU3Sytw)

* [4. Authentication using Guardian & Bcrypt for Elixir REST API Project](https://www.youtube.com/watch?v=4_CuT8oP5Ss)

* [5. Register New Account Endpoint for Elixir REST API Project](https://www.youtube.com/watch?v=FPEZSJTNbks)

* [6. Sign In Account Endpoint for Elixir REST API Project](https://www.youtube.com/watch?v=beW3yscX3ww)

* [7. Protected Endpoints for Elixir REST API Project](https://www.youtube.com/watch?v=58o66XhsWj8)

* [8. Account Session with Plug.Conn for Elixir REST API Project](https://www.youtube.com/watch?v=XVeCkV8KBuU)

* [9. Using Plug Actions as "middleware" for Elixir REST API Project](https://www.youtube.com/watch?v=1waMFH8I_cc)

* [10. Using Guardian.DB to track or revoke JWT's for Elixir REST API Project](https://www.youtube.com/watch?v=RTM_Mwbo3C8)

* [11. Using Guardian to refresh a JWT session for Elixir REST API Tutorial](https://www.youtube.com/watch?v=fIulOxYWUJY)

* [12. Set Token types & Expiration with Guardian for Elixir REST API Tutorial](https://www.youtube.com/watch?v=FTpRJ-YOKM8)

* [13. Using Ecto Preload for Elixir REST API Tutorial](https://www.youtube.com/watch?v=2J2ZlJO9kq4)

* [14. How to Refactor Our Plug Action for Reusability in our Elixir REST API Tutorial](https://www.youtube.com/watch?v=QPeq4IWIRPI)

* [15. Password Required to Update Account in our Elixir REST API Tutorial](https://www.youtube.com/watch?v=hmQ2K6dvqXk)

* [16. Documenting the API](www.google.co.za)


[PostGres Cheatsheet](https://postgrescheatsheet.com)

CICD
