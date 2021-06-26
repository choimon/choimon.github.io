---
title: 'REST vs HTTP'
last_modified_at: 2021-06-26T23:30
categories:
  - Software
tags:
  - rest 
  - http
toc: true
toc_sticky: true
---


# Summary 
Nowadays, many people seem to interchangeably use the words REST and HTTP. However, they are two different things. Through this post, I will introduce their differences. Before doing that, I will first explain what each term really means. 

## REST
Representational State Transfer or REST is not a standard or a specification.[^fn1] It is an architectural style or "concept"[fn2]for distributed hypermedia systems. It sets a series of constraints about how Server and Client should interact. 

### Resources and Representations 
RESTful systems' core concept involves resources. A resource is something that is uniquely identifiable in a system. It can be a web page, a video stream, or an image. It can even be an abstract concept like list of users in a database. Unique identification is the key constraint of REST resources. 

Resources may be available in multiple representations or formats, such as JSON and XML. You are using a "representation" of a resource to transfer resource state on the server into application state on the client. For example, in a client-server model, client requests an information of a user in JSON format. 

### Uniform Interface 
RESTful systems require clients to access all resources using the same set of standard operations. 


### Stateless 
All operations in a RESTful system should be stateless. Server should not store any state. Every request to the server should include all required data for the server to fulfill the request. 

This gives visibility, reliability, and scalability to systems. 




### RESTful API 

When a RESTful API is called, the server transfers to the client a representation of the requested resource's state. 

For example, when an application calls some REST API to fetch a user(resource) named "choi," the API will return the state of the user choi. This state may include full name, telephone number, and number of friends. 

The representation of the state can be in various formats, such as JSON, XML, and HTML.




## HTTP 
The HyperText Transfer Protocol or HTTP is a communication protocol with a given mechanism for server-client data transfer. It is a standard with well-defined constraints. It shows many features of a RESTful system and it is not a coincidence because HTTP 1.1 protocol was designed to follow the principles and constraints of REST. HTTP is commonly used in REST API. 

### URLs and Media types 
Resources are addressable using unique URLs, and clients can choose different representations for resources. Different representations are handled by headers and media types in HTTP. 



### HTTP Methods 
HTTP follows the principles of REST by providing the same set of methods for every resource. The common HTTP methods are POST, GET, PUT, and DELETE. 

### HTTP is Not Always RESTful

Modern web servers use cookies and sessions to store state. These cookies and session data are also used to represent the state on the server, which is not stateless. 

Using URLs can violate the uniform interface. 
`https://www.choi.com/api/v1/users?id=1&action=run`
The above URL is using the predefined HTTP method GET, but is using the query parameter `action=run` that is not available to all resources in the system.


## Difference 
As mentioned above, REST is an architectural style, while HTTP is a protocol. REST is not necessarily tied to HTTP[^fn2]. This architectural style may use HTTP, FTP, or other communication protocols (HTTP is widely used, though). 



# References
[^fn1]: [baeldung](https://www.baeldung.com/cs/rest-vs-http){:target="_blank"}
[^fn2]: [medium post](https://medium.com/the-sixt-india-blog/rest-stop-calling-your-http-apis-as-restful-apis-e8336e3e799b){:target="_blank"}



https://stackoverflow.com/questions/10418105/what-does-representational-state-mean-in-rest