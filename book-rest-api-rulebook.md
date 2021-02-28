# REST API Design Rulebook

By: Mark Massé. [Purchase the book](https://www.oreilly.com/library/view/rest-api-design/9781449317904/)!

Date Started: Tuesday, January 12, 2021

Date Finished: Sunday, February 28, 2021

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

## Chapter 4: Metadata Design

- HTTP defines a set of standard headers, some of which provide information about a requested resource. Other headers indicate something about the representation carried by the message. Finally, a few headers serve as directives to control intermediary caches.
- Metadata design rules:
  - `Content-Type` must be used to specify the type of data found within a request or response message’s body.
  - `Content-Length` should be used to give the size of the entity-body in bytes.
  - `Last-Modified` should be used in responses to indicate the last time that something happened to alter the representational state of the resource. This header should always be supplied in response to *GET* requests.
  - `ETag` should be used in responses to identify a specific *version* of the representational state contained in the response’s entity. This header should always be supplied in response to *GET* requests.
  - Stores must support conditional PUT requests by relying on the client to include the `If-Unmodified-Since` and/or `If-Match` request headers to express their intent. The `If-Unmodified-Since` request header asks the API to proceed with the operation *if, and only if, the resource’s state representation hasn’t changed since the time indicated by the header’s supplied timestamp value*. The `If-Match` header’s value is an entity tag, which the client remembers from an earlier response’s `ETag` header value. The `If-Match` header *makes the request conditional, based upon an exact match of the header’s supplied entity tag value and the representational state’s current entity tag value, as stored or computed by the REST API*.
  - `Location` must be used to specify the URI of a newly created resource. In response to the successful creation of a resource within a collection or store, a REST API must include the `Location` header to designate the URI of the newly created resource.
  - `Cache-Control`, `Expires`, and `Date` response headers should be used to encourage caching. When serving a representation, include a `Cache-Control` header with a `max-age` value (in seconds) equal to the freshness lifetime. To support legacy HTTP 1.0 caches, a REST API should include an `Expires` header with the expiration date-time. *The value is a time at which the API generated the representation plus the freshness lifetime*. REST APIs should also include a `Date` header with a date-time of the time at which the API returned the response. *Including this header helps clients compute the freshness lifetime as the difference between the values of the Expires and Date headers*.
  - `Cache-Control`, `Expires`, and `Pragma` response headers may be used to discourage caching. If a REST API’s response must not cached, add `Cache-Control` headers with the value `no-cache` and `no-store`. In this case, also add the `Pragma: no-cache` and `Expires: 0` header values to interoperate with legacy HTTP 1.0 caches.
  - Caching should be encouraged as possible. Using a small value of `max-age` as opposed to adding `no-cache` directive helps clients fetch cached copies for at least a short while without significantly impacting freshness.
  - Expiration caching headers should be used with *200 (OK)* responses. Although `POST` is cacheable, most caches treat this method as non-cacheable.
  - Expiration caching headers may optionally be used with 3xx and 4xx responses, known as **negative caching**, this helps reduce the amount of redirecting and error-triggering load on a REST API.
  - Custom HTTP headers must not be used to change the behavior of HTTP methods, if the information that are conveyed through a custom HTTP header is important for the correct interpretation of the request or response, *include that information in the body of the request or response or the URI used for the request*.
- Media types have the following syntax: `type "/" subtype *( ";" parameter )`.
  - The type value may be one of: application, audio, image, message, model, multipart, text, or video. A typical REST API will most often work with media types that fall under the `application` type.
  - In a hierarchical fashion, the media type’s subtype value is sub- ordinate to its type.
  - The parameters may follow the type/subtype in the form of `attribute=value` pairs that are separated by a leading semi-colon (;) character. A media type’s specification may designate parameters as either required or optional. Parameter names are *case-insensitive*. Parameter values are normally *case-sensitive* and may be enclosed in double quote (“ ”) characters. *When more than one parameter is specified, their ordering is insignificant*.
- Some commonly used registered media types are listed below:
  - `text/plain`: A plain text format with no specific content structure or markup.
  - `text/html`: Content that is formatted using the HyperText Markup Language (HTML).
  - `image/jpeg`: An image compression method that was standardized by the Joint Photographic Experts Group (JPEG).
  - `application/xml`: Content that is structured using the Extensible Markup Language (XML).
  - `application/atom+xml`: Content that uses the Atom Syndication Format (Atom), which is an XML-based format that structures data into lists known as feeds.
  - `application/javascript`: Source code written in the JavaScript programming language.
  - `application/json`: The JavaScript Object Notation (JSON) text-based format that is often used by programs to exchange structured data.
- Media type design rules:
  - Application-specific media types should be used.
  - Media type negotiation should be supported when multiple representations are available by submitting an `Accept` header with the desired media type.
  - Media type selection using a query parameter may be supported.
- The URI of a resource type’s current schema version always identifies the concept of the most recent version. A schema document URI that ends with a number permanently identifies a specific version of the schema. Therefore the latest version of a schema is always modeled by two separate resources which conceptually overlap while the numbered version is also the current one. This overlap results in the two distinct resources, with two separate URIs, consistently having the same state representation.

## Chapter 5: Representation Design

- XML, like HTML, organizes a document’s information by nesting angle-bracketed tag pairs. Well-formed XML must have tag pairs that match perfectly. This *“buddy system”* of tag pairs is XML’s way of holding a document’s structure together.
- JSON uses curly brackets to hierarchically structure a document’s information. Most programmers are accustomed to this style of scope expression, which makes the JSON format feel natural to folks that are oriented to think in terms of object-based structures.
- Message body rules:
  - JSON should be supported for resource representation if there is not already a standard format for a given resource type.
  - JSON must be well-formed:
    - JSON supports number values directly, so they do not need to be treated as strings.
    - JSON does not support date-time values, so they are typically formatted as strings.
    - JSON names should use mixed lower case and should avoid special characters whenever possible.
  - XML and other formats may optionally be used for resource representation.
  - Additional envelopes must not be created, in other words, the body should contain a representation of the resource state, without any additional, transport-oriented wrappers
- *A REST API response message’s body includes links to indicate the associations and actions that are available for a given resource, in a given state*. Included along with other fields of a resource’s state representation, links convey the relationships between resources and offer clients a menu of resource-related actions, which are context-sensitive.
- Hypermedia representation rules:
  - A consistent form should be used to represent links.
  - A consistent form should be used to represent link relations. Every link has a `rel` value to identify a document that describes the link’s relation. A link’s `rel` value describes the relationship from the current resource to the resource specified by the link’s `href` attribute.
  - A consistent form should be used to advertise links and to enable this, representations should include a structure, named `links`, to *contain all of the links that are available in the resource’s current state*. The links structure is a predictable place for clients to easily look up known links, by their simple relation names, as well as discover new links.
  
  ```json
  {
    "firstName" : "Osvaldo", 
    "lastName" : "Alonso", 
    "links" : {
      "self" : {
        "href" : "http://api.soccer.restapi.org/players/2113",
        "rel": "http://api.relations.wrml.org/common/self",
      },
      "parent": {
        "href" : "http://api.soccer.restapi.org/players",
        "rel": "http://api.relations.wrml.org/common/parent",
      },
      "addToFavorites": {
        "href" : "http://api.soccer.restapi.org/users/42/favorites/{name}",
        "rel": "http://api.relations.wrml.org/common/addToFavorites",
      }
    }
  }
  ```

  - A `self` link should be included in response message body representations. The `self` link relation signifies that the href value identifies a resource equivalent to the containing resource.
  - Minimize the number of advertised *entry point* API URIs. The `docroot`’s representation should provide links to make every other re- source programmatically available.
  - Links should be used to advertise a resource’s available actions in a state-sensitive manner. REST’s HATEOAS constraint specifies that an API must answer all client requests with resource representations that contain state-sensitive links.
  
  ```json
  {
    "links" : {
      "self" : {
        "href" : "http://api.editor.restapi.org/docs/48679",
        "rel" : "http://api.relations.wrml.org/common/self"
      },
      "cut" : {
        "href" : "http://api.editor.restapi.org/docs/48679/edit/cut",
        "rel" : "http://api.relations.wrml.org/editor/edit/cut"
      },
      "copy" : {
        "href" : "http://api.editor.restapi.org/docs/48679/edit/copy",
        "rel" : "http://api.relations.wrml.org/editor/edit/copy"
      }
    }
  }
  ```

  - A consistent form should be used to represent media type formats.
  - A consistent form should be used to represent media type schemas.
  - A store’s insert link may be used to add a new resource, with a URI specified by the client. To assist clients, a store’s representational form should provide a URI template in the link’s href value.
- Error representation rules:
  - A consistent form should be used to represent errors.
  
  ```json
  {
    "id" : "Text",
    "description" : "Text"
  }
  ```

  - A consistent form should be used to represent error responses.
  - Consistent error types should be used for common error conditions. Generic error types may be leveraged by a variety of APIs. These error types should be defined once and then shared across all APIs via a service hosting the error schema documents.

## Chapter 6: Client Concerns

- Versioning design rules:
  - New URIs should be used to introduce new concepts. *A URI identifies a resource, independent of the version of its representational form and state*. REST APIs should maintain a consistent mapping of its URIs to its conceptually constant resources. A REST API should introduce a new URI only if it intends to expose a new concept.
  - Schemas should be used to manage representational form versions. *Clients use media type negotiation to bind to the representational forms that best suit their needs*.
  - Entity tags should be used to manage representational state versions. T*he entity tag values associated with each individual resource are a REST API’s most fine- grained versioning system*.
- Security design rules:
  - OAuth may be used to protect resources. *OAuth is an HTTP-based authorization protocol that enables the protection of resources*. The OAuth protocol’s flow is summarized in the steps below:
    - A client obtains the artifacts needed to interact with a REST API’s protected resources. Note that with respect to the character of these artifacts and how they are obtained, there are some significant differences between versions of the OAuth protocol specification.
    - Using the artifacts that it obtained in Step 1, the client requests an interaction with a REST API’s protected resource.
    - The REST API,or an intermediary acting on its behalf, validates the client request’s OAuth-based authorization information. Note that there are some significant differences in the validation process as detailed by the OAuth 1.0 and 2.0 specifications.
    - If the validation check succeeds, the REST API allows the client’s interaction with the protected resource to proceed.
  - API management solutions may be used to protect resources. An API reverse proxy is a relatively new type of network-based intermediary that may be used to secure a REST API’s resources.
- *A REST API can show respect for its clients by offering them a measure of control over the composition of its response representations*.Response representation composition rules:
  - *The query component of a URI should be used to support partial responses*. There may be times when a REST API offers a resource state model that includes a bit more data than the client wishes to receive. *In order to save on bandwidth, and possibly accelerate the overall interaction, a REST API’s client can use the query component to trim response data with the fields parameter*. The `fields` query parameter allows clients to request only the resource state information that it deems relevant for its particular use case.
  - The query component of a URI should be used to embed linked resources. *REST API’s should allow individual client requests to control which linked resources should remain “normal” and which ones should become “embedded”*. This request-time composition approach allows a REST API to present a consistent, fine-grained resource model while empowering its clients to create facades that better match their individual use cases. *Clients use the `embed` query parameter to identify the link relations that they wish to have included, as `fields`, directly in the response’s representation*.
- JSONP should be supported to provide multi-origin read access from JavaScript. *REST APIs enable JSONP client requests by supporting an optional call back query parameter. If the parameter is present in a request, the API should wrap its normal JSON response body’s data in a JavaScript function call with the callback query parameter’s value as the function’s name*.
- CORS should be supported to provide multi-origin read/write access from JavaScript. For request methods other than: `GET`, `HEAD`, and `POST`; CORS defines a preflight request interaction. *The preflight request occurs “behind-the-scenes” between a CORS compliant browser and server, in advance of the JavaScript client’s actual request to access a cross-origin resource*. REST APIs may use the CORS-proposed Access-Control-Allow-Origin HTTP header to list the set of origins that are permitted cross-origin access to its resources. Most modern browsers support CORS by sending special HTTP request headers such as Origin and Access-Control-Request-Method. *The Origin header value identifies the requesting JavaScript client’s scheme/host/port source location. The Access-Control-Request-Method header value is sent in the CORS preflight request to indicate which HTTP method will be used in the client’s actual request*.

## Chapter 7: Final Thoughts

- The REST-fulness of APIs continues to be debated by those that create and consume them. In the absence of standards, REST API designers are free to innovate and explore new concepts, which is a good thing. However, when REST API designs eventually converge on a set of common patterns that address each one of the cross-cutting concerns, developers will benefit from the uniformity.
- Coding a REST API typically means programming an interface that exposes a backend system’s resources to Web-aware clients.
- The web resource server engine’s configuration data consists of a small set of core constructs, which are summarized below:
  - **API template**: A named REST API containing a list of resource templates, a list of schemas, and a list of “global” API-level state facts.
  - **Resource template**: A resource template is a path segment within a REST API’s hierarchical resource model. It has an associated URI template and set of possible schemas that client’s may bind to, at request time, by using media type negotiation.
  - **Schema**: Schemas are like classes or tables: they are a web application’s structured types. They allow forms (instances consisting of fields and links) to be molded in their image and used to carry the state of a resource.
  - **Format**: Formats, like HTML, XML, and JSON, are often used on their own to declare the type associated with the content of a message’s body.
  - **Link relation**: A link relation is a concept borrowed from HTML that adds semantics to links.