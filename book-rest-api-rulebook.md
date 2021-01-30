# REST API Design Rulebook

By: Mark Massé. [Purchase the book](https://www.oreilly.com/library/view/rest-api-design/9781449317904/)!

Date Started: Tuesday, January 12, 2021

Date Finished: ongoing

## Chapter 1: Introduction

- A **REST Application Programming Interface (REST API)** is a type of web server that enables a client, either user-operated or automated, to access resources that model a system’s data and functions.
- The Web’s scalability was governed by a set of key constraints. The constraints, which Fielding grouped into six categories and collectively referred to as the Web’s architectural style, are:
  1. **Client-server**: The Web is a client-server based system, in which clients and servers have distinct parts to play.
  2. **Uniform interface**
  3. **Layered system**: A network-based intermediary will intercept client-server communication for a specific purpose. Network-based intermediaries are commonly used for enforcement of security, response caching, and load balancing.
  4. **Cache**: The cache constraints instruct a web server to declare the cacheability of each response’s data.
  5. **Stateless**: The stateless constraint dictates that a web server is not required to memorize the state of its client applications.
  6. **Code-on-demand**: The code-on-demand constraint enables web servers to temporarily transfer executable programs, such as scripts or plug-ins, to clients.
- Web components inter-operate consistently within the uniform interface’s four constraints, which Fielding identified as:
  1. **Identification of resources**: Each distinct Web-based concept is known as *a resource and may be addressed by a unique identifier*, such as a URI.
  2. **Manipulation of resources through representations**: Clients manipulate representations of resources. *The same exact resource can be represented to different clients in different ways*.
  3. **Self-descriptive messages**: A resource’s desired state can be represented within a client’s request message. A resource’s current state may be represented within the response message that comes back from a server.
  4. **Hypermedia as the engine of application state (HATEOAS)**: A resource’s state representation includes links to related resources.
- Having a REST API makes a web service “RESTful” A REST API consists of an assembly of interlinked resources. This set of resources is known as the REST API’s resource model. So REST API is a web service interface that conforms to the Web’s architectural style.
- We must continue to seek out answers to a slew of questions, such as:
  - When should URI path segments be named with plural nouns?
  - Which request method should be used to update resource state?
  - How do I map non-CRUD operations to my URIs?
  - What is the appropriate HTTP response status code for a given scenario?
  - How can I manage the versions of a resource’s state representations?
  - How should I structure a hyperlink in JSON?

## Chapter 2: Identifier Design with URIs

> The only thing you can use an identifier for is to refer to an object. When you are not dereferencing, you should not look at the contents of the URI string to gain other information.

- Every character within a URI counts toward a resource’s unique identity. *Two different URIs map to two different resources*. If the URIs differ, then so do the re- sources, and vice versa. Therefore, a REST API must generate and communicate clean URIs and should be intolerant of any client’s attempts to identify a resource imprecisely.
- URI format: `URI = scheme "://" authority "/" path [ "?" query ] [ "#" fragment ]`.
- URI format rules:
  - **Forward slash separator (/) must be used to indicate a hierarchical relationship between resources**.
    - e.g. [http://api.restapi.org/shapes/polygons/quadrilaterals/squares]
  - **A trailing forward slash (/) should not be included in URIs, as the last character within a URI's path**.
    - e.g. [http://api.restapi.org/shapes] not [http://api.restapi.org/shapes/]
  - **Hyphens (-) should be used to improve the readability of URIs. Anywhere we would use a space or hyphen in English, we should use a hyphen in a URI**.
    - e.g. [http://api.restapi.org/blogs/mark-masse/entries]
  - **Underscores (_) should not be used in URIs**.
    - e.g. [http://api.restapi.org/blogs/mark_masse/entries]
  - **Lowercase letters should be preferred in URI paths. [RFC 3986](https://www.rfc-editor.org/rfc/rfc3986.txt) defines URIs as case-sensitive except for the scheme and host components**.
    - e.g. [http://api.restapi.org/my-folder/my-doc] not [http://api.restapi.org/My-Folder/My-Doc]
  - **File extensions should not be included in URIs, they should rely on the media type, as communicated through the *Content-Type* header, to determine how to process the body’s content**.
    - e.g. [http://api.collage.restapi.org/students/3248234/transcripts/2005/fall] not [http://api.collage.restapi.org/students/3248234/transcripts/2005/fall.json]
- URI authority design rules:
  - **Consistent subdomain names should be used for your APIs. The full domain name of an API should add a subdomain named *api***.
    - e.g. [http://api.soccer.restapi.org]
  - **Consistent subdomain names should be used for your client developer portal. If an API provides a developer portal, by convention it should have a subdomain labeled *developer***
    - e.g. [http://developer.soccer.restapi.org]
- A REST API is composed of four distinct resource archetypes: *document*, *collection*, *store*, and *controller*.
  - **A document resource is a singular concept that is akin to an object instance or database record**. A document’s state representation typically includes both fields with values and links to other related resources. With its fundamental field and link-based structure, the document type is the conceptual base archetype of the other resource archetypes. e.g. [http://api.soccer.restapi.org/leagues/seattle].
  - **A collection resource is a server-managed directory of resources**. Clients may propose new resources to be added to a collection. However, it is up to the collection to choose to create a new resource, or not. e.g. [http://api.soccer.restapi.org/leagues].
  - **A store is a client-managed resource repository**. A store resource lets an API client put resources in, get them back out, and decide when to delete them. On their own, stores do not create new resources; therefore a store never generates new URIs. e.g. [http://api.restapi.org/users/1234/favorites/alonso].
  - **A controller resource models a procedural concept**. Controller resources are like exe- cutable functions, with parameters and return values; inputs and outputs. A REST API relies on controller resources to perform application-specific actions that cannot be logically mapped to one of the standard methods (create, retrieve, update, and delete, also known as CRUD). e.g. [http://api.restapi.org/alerts/245743/resend].
- URI path design rules:
  - *A singular noun should be used for document names*. e.g. [http://api.soccer.restapi.org/leagues/seattle].
  - *A plural noun should be used for collection names*. e.g. [http://api.soccer.restapi.org/leagues/seattle/teams/].
  - *A plural noun should be used for store names*. e.g. [http://api.music.restapi.org/artists/mikemassedotcom/playlists].
  - *A verb or verb phrase should be used for controller names*. e.g. [http://api.college.restapi.org/students/morgan/register].
  - *Variable path segments may be substituted with identity-based values*. e.g. [http://api.soccer.restapi.org/leagues/{leagueId}].
  - *CRUD function names should not be used in URIs*.
- URI query design rules:
  - The query component of a URI may be used to filter collections or stores.
  - The query component of a URI should be used to paginate collection or store results.

- The entirety of a resource’s URI should be treated opaquely by basic network-based intermediaries such as HTTP caches. Caches must not vary their behavior based on the presence or absence of a query in a given URI. Specifically, response messages must not be excluded from caches based solely upon the presence of a query in the requested URI.

## Chapter 3: Identifier Design with URIs

- Each HTTP method has specific, well-defined semantics within the context of a REST API’s resource model. The purpose of **GET** is to retrieve a representation of a resource’s state. **HEAD** is used to retrieve the metadata associated with the resource’s state. PUT should be used to add a new resource to a store or update a resource. **DELETE** removes a resource from its parent. **POST** should be used to create a new resource within a collection and execute controllers.
- `Request-Line = Method <SP> Request-URI <SP> HTTP-Version <CRLF>`.
- Request method rules:
  - **GET** and **POST** must not be used to tunnel other request methods. Tunneling refers to any abuse of HTTP that masks or misrepresents a message’s intent and undermines the protocol’s transparency.
  - **GET** must be used to retrieve a representation of a resource.
  - **HEAD** should be used to retrieve response headers. In other words, *HEAD* returns the same response as *GET*, except that the API returns an empty body.
  - **PUT** must be used to both insert and update a stored resource. The *PUT* request message must include a representation of a resource that the client wants to store. However, the body of the request may or may not be exactly the same as a client would receive from a subsequent *GET* request.
  - **PUT** must be used to update mutable resources.
  - **POST** must be used to create a new resource in a collection.
  - **POST** must be used to execute controllers.
  - **DELETE** must be used to remove a resource from its parent which is often a collection or store. Once a DELETE request has been processed for a given resource, the resource can no longer be found by clients. Therefore, any future attempt to retrieve the resource’s state representation, using either GET or HEAD, must result in a *404 (Not Found)* status returned by the API.
  - **OPTIONS** should be used to retrieve metadata that describes a resource’s available interactions that includes an *Allow* header value.
- `Status-Line = HTTP-Version <SP> Status-Code <SP> Reason-Phrase <CRLF>`.
- Response status codes rules:
  - 1xx: Informational: Communicates transfer protocol-level information.
  - 2xx: Success: Indicates that the client’s request was accepted successfully.
    - **200 (OK)** should be used to indicate nonspecific success and should include a response body.
    - **200 (OK)** must not be used to communicate errors in the response body.
    - **201 (Created)** must be used to indicate successful resource creation.
    - **202 (Accepted)** must be used to indicate successful start of an asynchronous action.
    - **204 (No Content)** should be used when the response body is intentionally empty and is usually sent out in response to a *PUT*, *POST*, or *DELETE* request, when the REST API declines to send back any status message or representation in the response message’s body.
  - 3xx: Redirection: Indicates that the client must take some additional action in order to complete their request.
    - **301 (Moved Permanently)** should be used to relocate resources.
    - **302 (Found)** should not be used.
    - **303 (See Other)** should be used to refer the client to a different URL. It allows a REST API to send a reference to a resource without forcing the client to download its state. Instead, the client may send a GET request to the value of the Location header.
    - **304 (Not Modified)** should be used to preserve bandwidth
    - **307 (Temporary Redirect)** should be used to tell clients to resubmit the request to another URI. It indicates that the REST API is not going to process the client’s request. Instead, the client should resubmit the request to the URI specified by the response message’s Location header.
  - 4xx: Client Error: This category of error status codes points the finger at clients.
    - **400 (Bad Request)** may be used to indicate nonspecific failure.
    - **401 (Unauthorized)** must be used when there is a problem with the client’s credentials.
    - **403 (Forbidden)** should be used to forbid access regardless of authorization state.
    - **404 (Not Found)** must be used when a client’s URI cannot be mapped to a resource.
    - **405 (Method Not Allowed)** must be used when the HTTP method is not supported.
    - **406 (Not Acceptable)** must be used when the requested media type cannot be served.
    - **409 (Conflict)** should be used to indicate a violation of resource state.
    - **412 (Precondition Failed)** should be used to support conditional operations
    - **415 (Unsupported Media Type)** must be used when the media type of a request’s payload cannot be processed.
  - 5xx: Server Error: The server takes responsibility for these error status codes.
    - **500 (Internal Server Error)** should be used to indicate API malfunction.