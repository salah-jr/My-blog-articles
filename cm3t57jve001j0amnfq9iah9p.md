---
title: "Designing Web APIs: Chapter 5 - Design In Practice"
seoTitle: "Designing Web APIs: Chapter 5 - Design In Practice"
seoDescription: "Learn to design APIs with a focus on usability, aligning with business goals and user needs for a superior developer experience"
datePublished: Fri Nov 22 2024 19:37:06 GMT+0000 (Coordinated Universal Time)
cuid: cm3t57jve001j0amnfq9iah9p
slug: designing-web-apis-chapter-5-design-in-practice
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1732167753691/e2290721-6db0-4a3c-ab2f-dc9da666b3dc.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1732300170495/3ad6bffe-73f2-4385-b01a-e44b582f56eb.jpeg

---

Now that we’ve covered API paradigms, security, and best practices, it’s time to put these concepts into action. This chapter focuses on designing APIs through a hands-on approach using an imaginary example, emphasizing user-focused design and practical considerations. The goal is to help you create APIs that deliver great developer experiences and meet real-world needs by prioritizing usability and feedback-driven decisions.

# Scenario 1

To get us started with a practical example, here’s a simple, imaginary scenario that we will use for the first part of this chapter: You are the lead engineer in a fast-growing image archive startup called **“MyFiles”**. The company’s main product allows individual users and companies to privately archive data. Now that there is a steady stream of new users and lots of archival metadata, you and the team think there is a big potential in creating and publishing an API. As the lead engineer, you’ve been tasked with launching the new API within the next quarter.

## ✨Define Business Objectives

Before diving into development or writing your API specification, it’s essential to step back and ask two fundamental questions:

1. **What problem are we solving?**
    
2. **What impact do we aim to achieve?**
    

The answers should focus on both the users’ needs and the business goals.

For the first question, your response should clearly explain how the problem affects your customers and your business. Think of it as a guiding statement for your technical or product specifications.

The second question defines success for your API. This involves imagining the behaviors and outcomes you want to see from developers who interact with it.

By answering these questions early on, you ensure that your design decisions align with your goals and provide clarity to stakeholders.

### MyFiles API: Problem and Impact

Here’s how these answers might look for **MyFiles** API:

* **Problem:**  
    **MyFiles** customers need file metadata for their integrations but are stuck manually using CSV files. There’s no easy way for them to access this data directly through third-party tools.
    
* **Impact:**  
    An API will let developers create plugins to improve **MyFiles**, This means external developers can easily connect their own systems or applications with MyFiles, making the integration process smoother. As a result, Their customers will have a better experience, use MyFiles more often, and discover new ways to interact with the platform.
    

### Aligning Stakeholders

Defining the problem and impact doesn’t just clarify the purpose of your API; it aligns expectations across your team and stakeholders. Mismatched goals can lead to design inconsistencies or conflicts, so early alignment is crucial. These statements also provide a foundation to navigate debates, such as whether to use [REST](https://blog.mohammedsalah.online/designing-web-apis-chapter-2-api-paradigms#heading-representational-state-transfer-rest) or [RPC](https://blog.mohammedsalah.online/designing-web-apis-chapter-2-api-paradigms#heading-remote-procedure-call-rpc), by guiding decisions in user needs.

### Designing for Today, Preparing for Tomorrow

While designing for the present is key, remember that APIs evolve as products grow. A clear problem and impact statement helps guide you through changes. For existing products, this is even more crucial due to backward compatibility and dependencies.

Before designing new APIs, consider:

* What existing APIs and patterns can you leverage?
    
* Which features are popular in your current APIs, and do you have metrics?
    
* What are the weaknesses in your current APIs, and how can you improve them?
    
* Which teams will be affected, and how can you involve them early?
    

Investing upfront ensures your API meets user and business needs and streamlines development.

## **✨**Outline Key User Stories

Once you’ve defined the problem and impact, it’s important to outline the use cases your API will address. In Agile, these are similar to “user stories,” where the user is the developer who will use your API. Here, you’ll focus on the user type and the actions the user can perform.

**Template for Defining Key User Stories:**

* *As a \[user type\], I want \[action\] so that \[outcome\].*
    

**Example User Stories for the MyFiles API:**

* As a developer, I want to request a list of files so that I can see what a user has uploaded.
    
* As a developer, I want to request details for a single file so that I can get information about a file that a user has uploaded.
    
* As a developer, I want to upload files on behalf of a user so that users don’t need to leave my app to add a file to **MyFiles**.
    
* As a developer, I want to edit files on behalf of a user so that users don’t need to leave my app to update a file in **MyFiles**.
    

In the next section, we’ll see how these user stories translate into technology architecture decisions.

## **✨**Select Technology Architecture

Selecting the right technology architecture is crucial for both the product and developer experience. Developers expect seamless interactions, just like consumers do with products. Picking the right API paradigm and authentication system is key.

Start by choosing the API paradigm that fits best. Use what you learned in [Chapter 2](https://blog.mohammedsalah.online/designing-web-apis-chapter-2-api-paradigms) to evaluate the pros and cons of each pattern. We’ve filled out an example for the **MyFiles** API in *the following table.*

| **Paradigm** | **Pros** | **Cons** | **Selected?** |
| --- | --- | --- | --- |
| **REST** | MyFiles is resource-oriented with simple CRUD operations. | REST is a long-term commitment to a resource model. If more actions are needed, it may not fit. | ✓ |
| **RPC** | Expandable to actions beyond CRUD. | Currently, no operations beyond CRUD are required. No need to support extra actions. | ✘ |
| **GraphQL** | Flexible for developers, keeps payloads small. | Overly complicated to implement; no client presentation needed at this time. | ✘ |

For the **MyFiles** API, the best option is REST, as it aligns with the product's resource-oriented nature. Developers will need early access to the MyFiles account to understand it fully. In [**Scenario 2**](https://blog.mohammedsalah.online/designing-web-apis-chapter-5-design-in-practice#heading-scenario-2), we’ll explore how WebHooks can help avoid constant polling or open connections.

Next, we choose [OAuth](https://blog.mohammedsalah.online/designing-web-apis-chapter-3-api-security#heading-what-is-oauth) for authentication, offering more security than Basic Authentication. OAuth with short-lived tokens and refresh tokens ensures secure access to private files. Then, we define OAuth [scopes](https://blog.mohammedsalah.online/designing-web-apis-chapter-3-api-security#heading-scopes) based on the resources and operations we want to provide through the API.

The scope options are:

* General scope for all file operations
    
* Split scope into reading and writing files
    
* Granular scopes for specific CRUD operations
    

Here’s the breakdown:

| Resource or object | Operation | Scope |
| --- | --- | --- |
| Files and metadata | Create | write |
| Files and metadata | Read | read |
| Files and metadata | Update | write |

## **✨**Write an API Specification

An API specification (Also called a *spec*) ensures clarity, collaboration, and alignment. It helps finalize the design, facilitates stakeholder feedback, and serves as a contract for parallel development.

**Best Practices**:

* Use collaborative tools with version control and comments for tracking updates.
    
* Keep it structured and accessible for easier review and feedback.
    

A clear spec simplifies development and improves outcomes.

### The Specification Template

```http
Title: Proposal For MyFiles API Spec

Authors:
Brenda Jin, Saurabh Sahni, Amir Shevat

Problem:
Customers rely on file metadata for integrations with essential services. 
Currently, this requires dealing with CSV downloads and uploads, 
as there’s no programmatic access to repository metadata for third-party tools.

Solution:
Develop a REST API to enable developers to programmatically access MyFiles data.

Implementation:
API Type: REST, 
Chosen Because:
- Matches MyFiles' resource-oriented file structure.
- Aligns well with CRUD operations.
- Event transport isn’t needed at this stage.

Authentication: OAuth 2.0 with refresh tokens and token expiry.

Future-Considerations:
- WebHooks Planned for phase 2 to provide real-time updates for events 
  like file uploads and changes without polling.

- DELETE Operation: Excluded for the initial launch due to its risk and
  limited necessity.
```

The spec should include detailed information such as the developer’s workflow, authentication details, and data transmission protocols. Visual diagrams and tables can simplify complex concepts. For REST or RPC APIs, outlining URIs, inputs, outputs, errors, and scopes is helpful. Below is an example for the **MyFiles** API:

### Describing each API URI in the MyFiles API technical specification

| URI | Inputs | Outputs | Scope |
| --- | --- | --- | --- |
| **GET** /files | `include_deleted` (bool, default: false), `limit` (int, default: 100, max: 1000), `cursor` (string, default:null) ,`last_updated_after` (timestamp, default: null) | **Success (200):**`[ { "id":"123", "name": "file1", "size": 12345, "is_deleted": false, "permalink": "http://..." } ]`**Error (400):**`{ "error":"invalid_parameter", "message": "Limit exceeds 1000." }` | read |
| **GET** /files/:id | None | **Success (200):**`{ "id": "123", "name": "file1", "size": 12345, "is_deleted": false, "permalink": "http://..." }`**Error (404):**`{ "error":"not_found", "message": "File not found." }` | read |
| **PATCH** /files/:id | `name` (string, required) `notes` (string, optional) | **Success (202):**`{ "id": "123", "name": "updated_name", "last_modified": "2024-11-20" }`**Error (400):**`{"error": "missing_parameter", "message": "Name is required." }` | write |
| **POST** /files | `name` (string, required) `notes` (string, optional) | **Success (201):** `{ "id": "124", "name":"file2", "date_added": "2024-11-22", "size": 0, "is_deleted": false, "permalink": "http://..." }`**Error (400):**`{ "error":"invalid_parameter", "message": "Name is required." }` | write |

### MyFiles API: HTTP Status Codes and Error Responses

| **Status Code** | **Description** | **Error Response** |
| --- | --- | --- |
| **200 OK** | Request succeeded. | Outputs as described in API specification table. |
| **201 Created** | File created successfully. | Outputs as described in API specification table. |
| **202 Accepted** | File updated successfully. | Outputs as described in API specification table. |
| **400 Bad Request** | Missing or invalid parameters. | `{ "error": "missing_parameter", "message": "Missing parameters: <parameter>" }` `{ "error": "file_size_too_large", "message": "File exceeds size limit <file_size_limit>." }` |
| **401 Unauthorized** | No valid access token. | `{ "error": "unauthorized", "message": "Invalid token provided." }` |
| **403 Forbidden** | Access denied. | `{ "error": "forbidden", "message": "No permission to access file <id>." }` |
| **404 Not Found** | File not found. | `{ "error": "file_not_found", "message": "File <id> not found." }` |
| **429 Too Many Requests** | Rate limit exceeded. | `{ "error": "too_many_requests", "message": "Too many requests. Try again in <time> minutes." }` |
| **500 Server Error** | Internal server error. | General server issue. |

**💡Notes:**

* **Type Handling:** Specify nullable, optional, and required fields in API inputs and outputs.
    
* **Scaling and Performance:** Include strategies for scaling requests and performance optimization.
    
* **Security:** Use OAuth 2.0 for token-based access, with refresh tokens and expiry.
    
* **Additional Information:** File uploads, logging, and performance considerations should be detailed if applicable.
    

At the end of the spec, include an **Open Questions** section to track unresolved issues or future improvements.

# Scenario 2

You’ve built and released the **MyFiles** REST API, and it’s been a huge success over the past few months. You’ve been staying in touch with developers to get their feedback, and you’ve heard that they want the ability to receive updates when files have changed. With the original REST design, this means that they must make multiple requests on every notable file and check for any kind of difference.

This polling behavior is taking a toll on your infrastructure, and it’s not user-friendly for developers. As a result, your team has decided to build something to address this problem.

## ✨Define Business Objectives

**Problem**  
Currently, developers using our REST API must poll frequently, up to once per minute per file, to track changes. This is inefficient and places unnecessary strain on both the infrastructure and developers.

**Impact**  
By introducing webhooks, developers will be notified of changes to files in real-time, eliminating the need for constant polling and improving both performance and user experience.

## ✨Outline Key User Stories

As a developer, I want to receive an update when a le has been added, changed, or removed so that I don’t have to continuously poll the REST API.

## ✨Select Technology Architecture

In Scenario 1, we opted for a REST API for our technical architecture. In this case, we're exploring different event-driven API options. The following table outlines the advantages and disadvantages of three popular patterns for the MyFiles API: [WebHooks](https://blog.mohammedsalah.online/designing-web-apis-chapter-2-api-paradigms#heading-webhooks), [WebSockets](https://blog.mohammedsalah.online/designing-web-apis-chapter-2-api-paradigms#heading-websockets), and [HTTP Streaming](https://blog.mohammedsalah.online/designing-web-apis-chapter-2-api-paradigms#heading-http-streaming).

| **Paradigm** | **Pros** | **Cons** | **Selected?** |
| --- | --- | --- | --- |
| **WebHooks** | MyFiles developers may want to receive events when files are added, removed, or changed. | If files change too frequently, we may need complex infrastructure to prevent DDoS events due to excessive updates. | ✓ |
| **WebSockets** | Can be used by internal clients for UI display. | We don't want developers creating UI clients for MyFiles, and there’s no use case for long-lived connections. | ✘ |
| **HTTP Streaming** | Good for pushing data frequently. | With the low frequency of file changes, HTTP Streaming isn’t necessary. | ✘ |

After evaluating the pros and cons of different event-driven API styles, we've chosen to proceed with WebHooks. One important consideration for this design is how developers will configure their preferences to specify the events and files they want to track.

## ✨Write an API Specification

```http
Title: Proposal: MyFiles API WebHooks Spec

Authors: Brenda Jin, Saurabh Sahni, Amir Shevat

Problem:
Our REST API enables developers to access third-party integrations programmatically.
However, the only way developers can track file changes is by constantly polling
our API, up to once per minute per file.

Solution:
Build an event-driven WebHooks API that allows developers to receive notifications
for file-related events such as add, update, and change events.

Implementation:
Developers will provide an endpoint they control to receive POST requests with
JSON bodies for each event.

Authentication:
The API will use OAuth 2.0 authentication, similar to the MyFiles REST API. 
Any developer with the read OAuth scope will be able to receive WebHook requests.

Other-Considerations:
While WebHooks were chosen, we also evaluated other event-driven API designs, 
such as WebSockets and long-lived HTTP streaming connections.
```

### Describing event objects for the MyFiles API technical specification

| Event | Payload | OAuth Scope |
| --- | --- | --- |
| **file\_added** | `{ "id": $id, "resource_type": "file", "event_type": "added", "name": string, "date_added": $timestamp, "last_updated": $timestamp, "size": int, "permalink": $uri, "notes": array <file_notes>, "uri": $uri }` | read |
| **file\_changed** | `{ "id": $id, "resource_type": "file", "event_type": "changed", "name": string, "date_added": $timestamp, "last_updated": $timestamp, "size": int, "permalink": $uri, "notes": array <file_notes>, "uri": $uri }` | read |
| **file\_removed** | `{ "id": $id, "resource_type": "file", "event_type": "removed", "name": string, "date_added": $timestamp, "last_updated": $timestamp, "size": null, "permalink": null, "notes": null, "uri": null }` | read |

**Notes**

* Event names are kept simple to avoid overloading the payload with too much data. Developers can use the `payload.uri` field to request further details if needed.
    
* The granularity of the events and the type of changes (e.g., file size, name change) should be carefully considered based on developer needs.
    

## **Validate Your Decisions**

Once the API specification is written, it's essential to gather feedback from stakeholders. Testing your design early and often ensures that you’re on the right track and helps identify potential issues before implementation begins. In this chapter, we've provided two example specifications to illustrate a range of possible approaches. However, in a real-world scenario, there would be a feedback loop after each specification is written.

Don’t wait until your API is fully built to seek feedback. Collecting input early can save time and prevent costly rewrites later on.

### ✨Reviewing the specification with stakeholders

Gathering feedback early and often is key to refining your API design. Share the spec with stakeholders to get their input on the problem, solution, and details. Engage with developers who may build integrations and internal stakeholders for feedback. This helps identify usability issues before coding begins.

The goal is not just approval but constructive criticism to improve the design. Be open to disagreement and focus on refining the solution. Ask specific questions like, “What issues did you encounter with the WebHooks API?” instead of general ones like “Did you like it?”

By understanding others' concerns, you can make more informed, intentional decisions and ensure the API meets all needs effectively.

### ✨Mocking data for interactive user testing

To gather valuable feedback, use mock data to simulate your API responses. Create an interface where developers can interact with fixed data in the proposed format. This mock application can be integrated into your existing web app, providing a more interactive testing environment for stakeholders.

Mocking tools also allow developers to start working on their integrations in parallel with your API development, ensuring smoother implementation later.

### ✨Beta Testers

For public-facing APIs, external stakeholders are just as important as internal ones. After initial decisions and API development, consider launching a beta testing program. This allows developer partners early access to the API and provides valuable feedback before the public release. Beta testing offers real-world insights, helping improve the API design based on actual user needs.

## Conclusion

We believe the MyFiles API is off to a great start, what do you think? This process has provided a flexible template for designing APIs. As you customize the design for your own API, aim to keep it efficient and fast while ensuring you gather valuable feedback.

Throughout this book, we emphasize the importance of considering the human factor in computing. Many poor design decisions occur when API providers overlook the needs of their users and developers.

Adapt the provided template to suit your organization, but always keep the user in mind!

In [**Chapter 6**](https://blog.mohammedsalah.online/designing-web-apis-chapter-6-scaling-apis), we’ll dive into API scalability, pagination, rate limiting, and developing SDKs, providing you with the tools to optimize and enhance your API design.

## **References**

[**https://www.goodreads.com/book/show/38396693-designing-web-apis?ref=nav\_sb\_ss\_1\_18**](https://www.goodreads.com/book/show/38396693-designing-web-apis?ref=nav_sb_ss_1_18)

**Thank you for reading! I hope you found the insights helpful.**