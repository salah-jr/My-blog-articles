---
title: "Designing Web APIs: Chapter 2 - API Paradigms"
seoTitle: "Designing Web APIs: Chapter 2 - API Paradigms"
seoDescription: "Choosing the right API style is important for future success. There are multiple API styles such as REST, RPC, GraphQL, WebHooks, and WebSockets..."
datePublished: Fri Oct 11 2024 12:09:31 GMT+0000 (Coordinated Universal Time)
cuid: cm24oq6bw000509l38aghbhxs
slug: designing-web-apis-chapter-2-api-paradigms
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1728597100253/b4991895-0f9f-4510-b27f-983da5dcbeb5.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1728597132634/53dc1a28-3519-49b2-9d59-4213876e6ac3.jpeg

---

Choosing the right API style is important for future success. It sets how a service shares data with other applications. Some companies don't plan enough for future updates, making it hard to change the API once it's in use by other developers. To avoid problems, it's good to think about the right protocols and best practices from the start. There are multiple API styles such as REST, RPC, GraphQL, WebHooks, and WebSockets, each with its own benefits.

## Request-Response APIs

Request–Response APIs allow clients to interact with a service via HTTP based web server. APIs define a set of endpoints. The client sends a request to a specific endpoint, and the server responds, usually in JSON or XML format. Common approaches for request-response APIs are **REST**, **RPC**, and **GraphQL**.

### ✨Representational State Transfer (REST)

It’s the most popular choice for API development. REST APIs expose data as resources (customers, events or orders). A resource is an entity that can be addressed, named or handled on the web. It uses standard HTTP methods to represent Create, Read, Update and Delete (CRUD) operations against these resources.

In my experience as a backend engineer, I designed a REST API for an e-commerce platform, allowing clients to create, update, and retrieve products using HTTP methods like POST, PUT, and GET. Another example is a reservation system I built, where each booking was treated as a resource, enabling users to easily interact with reservations through the API.

**General rules REST APIs follow:**

* Resources are part of the URL like `/users`.
    
* For each resource generally, we have two URLs: one for the collection like `/users` , and one for a specific element like `/users/U123`.
    
* Nouns are used instead of verbs for resources. For example, instead of `/getUserInfo/U123` use `/users/U123`.
    
* HTTP methods like GET, POST, PUT, PATCH, and DELETE tell the server what action to take on a resource. Each method serves a different purpose:
    
    * **Create (POST):** Use `POST` to add new resources. For example, sending a `POST` request to `/products` create a new product in an online store.
        
    * **Read (GET):** Use `GET` to retrieve resources without changing them. For instance, a `GET` request to `/products/1` will fetch details of the product with ID 1. GET requests are read-only and can be cached.
        
    * **Update (PUT/PATCH):** Use `PUT` to completely replace a resource and `PATCH` for partial updates. For example, a `PUT` request `/products/1` could replace the entire product, while a `PATCH` request might only change its price.
        
    * **Delete (DELETE):** Use `DELETE` to remove resources. A `DELETE` request to `/products/1` would remove the product with ID 1.
        
* Standard HTTP response status codes are sent by the server to indicate whether a request was successful or if there was an error. Here's a quick overview:
    
    * **2XX (Success):** Codes in this range signify that the request was successful. For example, `200 OK` indicates that the request has succeeded.
        
    * **3XX (Redirection):** These codes indicate that a resource has moved. For instance, `301 Moved Permanently` means the requested resource has a new URL.
        
    * **4XX (Client Error):** Codes in this range point to client-side errors, such as a missing required parameter or too many requests. For example, `404 Not Found` means the requested resource does not exist.
        
    * **5XX (Server Error):** These codes indicate server-side errors, such as when the server encounters an unexpected condition. For example, `500 Internal Server Error` suggests a general problem on the server.
        
* REST APIs might return JSON or XML responses. That said, due to its simplicity and ease of use with JavaScript, JSON has become the standard for modern APIs.
    

*Example 2-1. HTTP Request to Retrieve a Charge from the Stripe API*

```http
GET /v1/charges/ch_CWyutlXs9pZyfD
HOST: api.stripe.com
Authorization: Bearer YNoJ1Yq64iCBhzfL9HN0000fzVrsEjtVL
```

*Example 2-2. HTTP Request to Create a Charge from the Stripe API*

```http
POST /v1/charges/ch_CWyutlXs9pZyfD
HOST: api.stripe.com
Content-Type: application/x-www-form-urlencoded
Authorization: Bearer YNoJ1Yq64iCBhzfL9HN0000fzVrsEjtVL

amount=2000&currency=usd
```

**Showing relationships**

When a resource is dependent on another resource, it's more effective to represent it as a sub-resource rather than a standalone resource in the URL. This approach clarifies the relationship between the resources for developers using the API.

*Example*: If you have a resource for **users** and each user has **orders**, you can represent an order as a sub-resource of a user. Instead of using a top-level URL like `/orders/123`, you could use `/users/456/orders/123`, where `456` is the user ID and `123` is the order ID. This structure clearly indicates that the order belongs to a specific user.

**Non-CRUD Operations**

REST APIs can handle non-CRUD actions using the following methods:

* **Field Rendering:** Include actions as fields in a resource. For example, GitHub’s API uses the "archived" parameter to archive a repository.
    
* **Sub-resource Action:** Treat actions like sub-resources. The GitHub API locks an issue with:  
    `PUT /repos/:owner/:repo/issues/:number/lock`.
    
* **Action Verb in URL:** For operations like search, use the action verb in the URL:  
    `GET /search/code?q=:query` retrieves matching files.
    

*Example 2-3. HTTP request to archive a GitHub repository*

```http
PATCH /repos/saurabhsahni/Hacks
HOST: api.github.com
Content-Type: application/json
Authorization: token OAUTH-TOKEN

{
  "archived": true
}
```

### ✨Remote Procedure Call (RPC)

RPC is a straightforward API paradigm focused on executing actions rather than managing resources, as seen in REST. In this approach, clients invoke methods on a server by specifying the method name and passing arguments, receiving responses in JSON or XML format.

**General rules RPC APIs follow:**

* The endpoint contains the name of the operation to be executed.
    
* API calls are made with the HTTP verb that is most appropriate: GET for read-only requests and POST for others.
    

RPC is effective for APIs that need to expose complex actions that cannot be easily represented with CRUD operations. It’s also well-suited for scenarios with complex resource structures or operations that affect multiple resource types.

*Example 2-4. HTTP request to Slack’s API*

```http
POST /api/conversations.archive
HOST slack.com
Content-Type: application/x-www-form-urlencoded
Authorization: Bearer xoxp-1650112-jgc2asDae
channel=C01234
```

Slack’s Conversations API allows actions like archiving, joining, kicking, leaving, and renaming. While there is a clear resource (the conversation), these actions don’t fit well into the REST pattern. Additionally, actions like posting a message (e.g., `chat.postMessage`), which have complex relationships with message resources, attachments, and visibility settings in the web client.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728588850022/0fdfd817-4b28-4bbe-9f6d-9320e27ebf64.png align="center")

💡*RPC-style APIs are not exclusive to HTTP. There are other high-performance protocols available for RPC-style APIs, including* [*Apache Thrift*](https://thrift.apache.org) *and* [*gRPC*](https://grpc.io)*.*

### ✨GraphQL

GraphQL is a query language for APIs that has become increasingly popular. Developed by Facebook in 2012 and released to the public in 2015, it’s now used by companies like GitHub, Yelp, and Pinterest. With GraphQL, clients can specify exactly what data they need, and the server provides a response based on that request.

*Example 2-5. GraphQL query*

```http
{
    user(login: "saurabhsahni") {
        id
        name
        company
        createdAt
   }
}
```

*Example 2-6. Response from GitHub GraphQL API*

```http
{
    "data": {
        "user": {
            "id": "MDQ6VXNlcjY1MDI5",
            "name": "Saurabh Sahni",
            "company": "Slack",
            "createdAt": "2009-03-19T21:00:06Z"
        }
    }
}
```

Unlike REST and RPC, GraphQL APIs only require a single URL endpoint. There's no need for different HTTP verbs to specify actions. Instead, in the JSON body, you declare whether you're making a query (to fetch data) or a mutation (to modify data), GraphQL supports both GET and POST requests.

*Example 2-7. GraphQL API call to GitHub*

```http
POST /graphql
HOST api.github.com
Content-Type: application/json
Authorization: bearer 2332dg1acf9f502737d5e
{
    "query": "query { viewer { login }}"
}
```

**GraphQL Key Advantages:**

* **Fewer Round Trips**  
    GraphQL lets clients fetch data across multiple resources in one request, Without GraphQL, this might require multiple HTTP calls to the server. This means mobile applications using GraphQL can be quick, especially on slow networks.
    
* **No Versioning Needed**  
    New fields can be added or deprecated without breaking existing queries, making it easier to manage API changes.
    
* **Smaller Payloads**  
    Clients request only the data they need, reducing payload size compared to REST and RPC APIs, which often send unused data.
    
* **Strongly Typed**  
    GraphQL ensures queries are syntactically correct during development, leading to fewer errors and higher-quality clients.
    
* **Built-in Introspection**  
    GraphQL is natively discoverable through tools like GraphiQL, an in-browser IDE for writing, testing, and exploring APIs.
    

While GraphQL has several benefits, it introduces challenges for the API provider. The server must handle complex query parsing and parameter validation, which complicates performance optimization. Internally, it's easier to anticipate use cases and address performance issues. However, with external developers, it becomes harder to anticipate use cases and optimize queries. Opening GraphQL to third parties can shift the burden of handling multiple requests (like in REST) to processing complex queries, leading to unpredictable performance and infrastructure impact.

**<mark>Comparison of request–response API paradigms</mark>**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728596889476/b2db94bb-1ead-4be1-80b5-76f126349298.png align="center")

## Event-Driven APIs

In traditional request-response APIs, data can quickly become outdated, especially for services with constantly changing information. Developers often use **polling** which is the process of checking for updates by repeatedly querying the API at set intervals.

* **Low-Frequency Polling**: If developers poll infrequently, they may miss important events, such as resource creation, updates, or deletions.
    
* **High-Frequency Polling**: Polling too often can waste resources, as most API calls may return no new data. For example, a study by Zapier found that only about 1.5% of their polling calls returned fresh data.
    

To address this issue and share real-time data about events, there are three common methods: **WebHooks**, **WebSockets**, and **HTTP Streaming**.

### **✨WebHooks**

It’s essentially a URL that listens for HTTP requests, typically using the POST method (though it can also accept GET, PUT, or DELETE). When an event occurs, an API provider that supports WebHooks sends a message to the configured URL added by the user or the developer.

This approach allows you to receive real-time updates instead of polling the API for changes. Many popular services, such as **Slack**, **Stripe**, **GitHub**, and **Zapier**, offer WebHook support to keep clients informed about relevant events as they happen.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728598649373/b8d9c8d7-3117-4dce-8fb5-a7cc21b9b987.png align="center")

*💡* ***For instance, based on my experience:***

* While working on a technical assessment platform, I needed to provide users with instant feedback on their results. Instead of manual checks or frequent **polling**, I configured a **WebHook** to notify my server whenever an assessment was completed. Once a user finished their test, the platform sent the results directly to my server through the WebHook URL I added, enabling real-time updates to the user interface.
    
* Similarly, while integrating with a payment gateway, I set up a WebHook to receive notifications about transaction success or failure when an order was completed. This allowed for immediate updates to the order status in my application, further enhancing user experience.
    

**While these advantages are significant, they also come with some challenges that deserve attention.**

* **Failures and Retries**
    
    To ensure successful WebHook delivery, it's crucial to implement a retry system. For instance, Slack retries failed deliveries three times: immediately, one minute later, and five minutes later. If errors persist for 95% of requests, Slack stops sending events to that WebHook and notifies the developer.
    
* **Security**
    
    WebHook security is still evolving. Developers must verify WebHooks, which can lead to using unverified ones. Many API providers follow common security patterns for WebHooks, discussed in Chapter 3.
    
* **Firewalls**
    
    Applications behind firewalls can access APIs via HTTP but may struggle to receive inbound traffic, making WebHook use difficult.
    
* **Noise**
    
    Each WebHook call usually represents a single event, but when thousands occur in a short time, it can create noise.
    

### **✨WebSockets**

WebSocket is a protocol that creates a two-way communication channel over a single TCP connection, commonly used between web clients (like browsers) and servers, but also for server-to-server communication. WebSockets are great for fast, live-streaming data and long-lived connections.

Supported by all major browsers, WebSockets are often used for real-time applications. For instance, Slack uses WebSockets to stream events such as new messages or reactions to clients. It even offers a Real-Time Messaging API for developers to receive events and send messages in real-time. Similarly, Trello and Blockchain use WebSockets to update clients with real-time changes and notifications.

WebSockets allow both the server and client to send and receive data at the same time, making them great for real-time communication. They work over ports **80 or 443**, which helps them bypass firewalls, making them ideal for enterprise environments. For example, many developers prefer Slack’s WebSocket API over WebHooks because they can securely receive events without exposing an HTTP endpoint to the internet.

While WebSockets are excellent for live data and long-lasting connections, they have some downsides. In areas with spotty connections or on mobile, clients need to reconnect if the connection drops. They can also be hard to scale. If an app is used by 10,000 Slack teams, it requires 10,000 active connections, which can be challenging to manage.

In one of my projects, I used WebSockets to provide real-time updates for a live dashboard. The project involved tracking user activities and showing changes instantly on the interface without requiring page reloads or manual refreshing. By establishing a WebSocket connection between the server and client, I was able to push updates on the fly, ensuring that users saw live data streams with minimal delay.

This approach helped streamline the user experience by keeping the connection open for continuous communication, particularly useful in scenarios where frequent data updates were needed, such as tracking real-time metrics or notifications.

### **✨HTTP Streaming**

With HTTP request-response APIs, clients send a request, and the server responds with a fixed-length message. But by using HTTP Streaming, the server can keep the connection open and continue sending data in real-time.

To enable streaming, one approach is to use the `Transfer-Encoding: chunked` header, which tells the client to expect chunks of data. Another approach is server-sent events (SSE), which works well with browsers via the `EventSource` API.

Twitter uses HTTP Streaming in its API to push new tweets without clients needing to constantly poll for updates. This saves resources for both Twitter and developers.

However, HTTP Streaming has challenges like buffering, where data might not be displayed until a certain amount is received. Also, if clients need to frequently change event types, reconnecting may be necessary, which can be inconvenient.

**<mark>Comparison of event-driven API paradigms</mark>**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728648025123/5958f83a-c3db-47ac-920c-890a6cdba81c.jpeg align="center")

## Conclusion

There is no single API paradigm that fits all situations. Each type has its strengths for specific use cases, and sometimes, you may need to support more than one. For instance, Slack combines RPC-style APIs, WebSockets, and WebHooks. The key is understanding which approach best serves your users, aligns with your business objectives, and works within your system's limitations.

In [Chapter](https://hashnode.com/preview/66b52e7464b7931f978d1ac6) 3, we'll explore API security, including authentication and authorization techniques, with a focus on OAuth—a widely used protocol for secure, standard authorization.

## **References**

[**Designing Web APIs: Building APIs That Developers Love**](https://www.goodreads.com/book/show/38396693-designing-web-apis?ref=nav_sb_ss_1_18)

**Thank you for taking the time to read, and I hope you found the insights valuable!**