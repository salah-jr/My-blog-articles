---
title: "Designing Web APIs: Chapter 7 - Managing Change"
seoTitle: "Designing Web APIs: Chapter 7 - Managing Change"
seoDescription: "Manage API changes, maintain consistency, ensure backward compatibility for user satisfaction and system reliability"
datePublished: Fri Jan 17 2025 14:35:41 GMT+0000 (Coordinated Universal Time)
cuid: cm60v3mca000m09l72fdp1nec
slug: designing-web-apis-chapter-7-managing-change
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1737064162577/f227a075-7133-434c-8d1f-995411cf63fe.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1737064172723/8998a447-b35c-412f-af91-ea7a299ad260.jpeg
tags: apis, api-versioning

---

This chapter dives into the art of managing change in your API. It explores strategies to maintain consistency, ensure backward compatibility, and guide your API’s evolution smoothly while keeping your users happy and your system reliable

# **✨** Toward Consistency

Consistency is key to building trust and creating a great developer experience. A consistent API is easy to understand and use, making it more reliable for developers.

**Here’s what consistency means:**

* Developers can easily understand how to access data.
    
* Responses have clear, meaningful names and follow the same structure.
    
* The same request patterns work across different endpoints.
    
* Errors are predictable and provide helpful messages.
    

However, inconsistency can creep in, even for established platforms. Take Slack's API as an example. Over time, endpoints were added without central oversight, leading to mismatched patterns across similar methods.

Here’s a look at Slack's API inconsistency in 2017:

```json
// Join a channel
channels.join({
  channel: "channel-name"
})

// Invite a user to a channel
channels.invite({
  channel: "C12345",
  user: "U23456"
})
```

In one case, the method uses a channel name, while the other expects a channel ID. These inconsistencies can confuse developers and complicate integrations. By prioritizing consistency, you can avoid these pitfalls and create a smoother, more dependable experience.

Fortunately, there are tools and processes available to prevent such inconsistencies. In the following sections, we’ll explore some of these approaches and how they can help maintain consistency in your API design.

## Automated Testing

Automated testing helps maintain API consistency by ensuring that changes don’t introduce issues, especially backward incompatibilities. With Continuous Integration (CI), developers merge their changes into a shared repository multiple times a day, triggering an automated test suite that checks for regressions. This test suite validates input, expected behaviour, and data types in responses.

Having timely feedback from automated tests empowers developers to make the right decisions. It also ensures that code changes affecting the API are reviewed by the right people before merging. Even without CI, running continuous tests off the main code branch helps monitor your API’s health.

In short, automated testing is crucial for catching unwanted changes early and maintaining API consistency.

## API description languages

JSON is the most common format for API communication, offering flexibility and expressiveness. However, as JSON becomes more complex, it can’t be limited to the same type of systems found in most programming languages.

When providing a JSON-based web API, it’s crucial to carefully manage changes for both requests and responses to maintain consistency and clarity.

### **Describing and validating responses**

Validating API responses is an important step in ensuring consistency and avoiding unintended breaking changes. By using tools like JSON Schema, you can define the expected structure of your API responses and integrate automated testing to validate them. This approach helps maintain consistency across your API as it evolves over time.

With JSON Schema, you can define the expected structure of the API response, including the required fields and their types. By describing your responses in this structured way, you can use automated testing tools to ensure the responses remain consistent.

**Example: JSON Schema Validation**

In practice, when defining a response structure, you might specify that a response must include certain fields (e.g., `id`, `name`, `description`) and ensure these fields have the correct types.

***Example of a simple JSON Schema definition for a response payload:***

```json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "title": "repositories.fetch",
  "description": "Schema for repositories.fetch response payload",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "repositories"
  ],
  "properties": {
    "repositories": {
      "type": "array",
      "items": {
        "$ref": "../common_objects_schema.json#/repository"
      }
    }
  }
}
```

Here, the response is expected to include a `repositories` field that is an array of objects. Each object follows another defined schema (`repository`), ensuring consistency across multiple endpoints.

#### **Reusable Components: Keep It Consistent**

To ensure consistency, the objects described in the schema can be reused across different endpoints. For example, the `repository` object is defined once, and that definition is referenced in various other schema files. This approach avoids duplication and ensures that any change to the structure of a repository object is reflected across all relevant endpoints.

```json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "repository": {
    "type": "object",
    "additionalProperties": false,
    "required": [
      "id",
      "name",
      "description",
      "created"
    ],
    "properties": {
      "id": {
        "type": "integer"
      },
      "name": {
        "type": "string"
      },
      "description": {
        "type": "string"
      },
      "created": {
        "type": "integer"
      }
    }
  }
}
```

This reusable definition allows you to ensure consistency throughout your API. Every time you call for a `repository`, you can expect the same structure.

#### **Testing the Response**

Once you’ve defined your API response using JSON Schema, it’s time to validate it in your automated tests. Using a tool like RSpec in Ruby (or any other testing framework) helps you run tests to ensure that the API response adheres to the defined schema.

Here’s a simple test using RSpec to check that the response from the `repositories.fetch` endpoint matches the defined JSON Schema:

```ruby
rubyCopyEditdescribe 'repositories.fetch' do
  it "can fetch all repositories successfully", acceptance: true do
    response = @client.repositories.fetch().response_body
    expect(response).to_match_json_schema('repositories.fetch')
  end
end
```

This test calls the API endpoint and checks that the response matches the schema for `repositories.fetch`. If the response doesn’t match, the test will fail, giving you an opportunity to fix the issue before it becomes a problem.

### **Describing and validating requests**

When describing and validating API requests, the goal is to provide clear guidelines to help developers make correct choices, even when you can't control how they use your API. Tools like JSON Schema and OpenAPI (formerly Swagger) can define and validate the structure of API requests, making it easier to ensure consistent formats and types.

By using these tools, you can generate documentation, perform error checking, and validate inbound requests to prevent poorly formatted data from being processed. Additionally, clear and updated documentation is crucial for guiding developers and managing API changes. This helps developers understand the current state of your API and its evolution.

# **✨** Backward Compatibility

Backward Compatibility refers to the ability of an API to support older versions or behaviours even as it evolves. This ensures that existing integrations continue to function without breaking, which is especially critical for APIs with external users or widespread adoption. Maintaining backward compatibility is a cornerstone of a reliable and user-friendly API, as it fosters trust and minimizes disruptions.

For example, in systems like GraphQL, where clients specify the fields they need, developers might have some insight into which fields are actively being used. However, in traditional REST APIs or when developers request all fields (as often happens even in GraphQL), it becomes challenging to determine which fields are truly necessary. This lack of visibility complicates deprecation and changes, as you can't be sure what might break for users.

In cases where backward compatibility is non-negotiable, such as Cloudinary's API for image URLs, the design must prioritize extensibility and future-proofing from the outset. Even so, the challenge often shifts from preventing breaks to effectively communicating changes and new features to users. For internal APIs, managing compatibility is slightly easier since you have direct control over dependencies, but for external APIs, careful planning and transparency are crucial.

# **✨ Planning for and Communicating Change**

This is crucial for evolving APIs. As your API grows, decisions made earlier will influence future changes. It's essential to determine how flexible your system should be with modifications and their impact on users. A clear communication plan is key to notifying users about changes, whether they're breaking or additive. Consistent updates through documentation and changelogs help maintain trust while allowing the API to evolve.

## Communication Plan

When creating a communication plan, it’s essential to ensure developers have an effective way to receive updates. While a basic solution like an [RSS](https://www.postaffiliatepro.com/affiliate-marketing-glossary/rich-site-summary-rss/) feed is a good start, over time, you'll need to establish more direct communication methods, especially for informing specific developers about changes that impact them.

For instance, if you've introduced a new API endpoint (like `repositories.fetchSingle`), you may need to reach out to developers who’ve used the `repositories.fetch` endpoint in the past year. This ensures they are informed about the new feature. If there are any major issues, like a 500-level response after a release, contacting affected developers directly would also be crucial.

You can use various communication channels and set appropriate timelines for different types of changes. For backwards-compatible changes, updates can be shared at any time, while backwards-incompatible changes require advance notice—typically 18 months before the release.

***Example categories of types of changes***

| **Type of Change** | **Backward-Compatible** | **Backwards-Incompatible** |
| --- | --- | --- |
| Examples | Added request/response fields, new methods | Removed fields, changed functionality, deleted endpoints |
| Channels | RSS feed, API docs | RSS feed, API docs, Email to developers, Blog post |
| Time to Notify | Anytime | 18 months |

In addition to these communication channels, you can also annotate response payloads or headers to include details about changes. For example, you might add metadata to the response of an endpoint like `repositories.fetch` to inform developers of upcoming changes.

***Adding response metadata to communicate with developers***

```json
// GET repositories.fetch()
{
  "repositories": [
    {
      "id": 12345
    },
    {
      "id": 23456
    }
  ],
  "response_metadata": {
    "response_change": {
      "date": "January 1, 2021",
      "severity": 1,
      "affected_object": "repository",
      "details": "Starting January 1, 2021, a new field `date` will be added to each repository object"
    }
  }
}
```

A good communication plan balances notifying developers of changes with the need to allow your API to evolve. As you make more changes, the complexity and volume of communication will increase, so it’s helpful to automate notifications where possible to reduce manual effort.

## Adding

Adding new fields or endpoints is typically straightforward and backward-compatible. For response fields, adding a new key-value pair won’t impact developers, especially if the fields are consistently set. Similarly, adding a "column" in query-based interfaces is easier than removing one.

However, consider these points:

* If a field wasn’t set previously, ensure developers aren't relying on its absence.
    
* Allow developers to opt into new fields if needed.
    
* Be cautious with adding too many request parameters, as it complicates API descriptions and testing.
    
* New endpoints should be consistent with existing ones, provide a smooth upgrade path, and avoid overcrowding the namespace.
    

## Removing

Deprecating or removing features requires careful planning and clear communication. To help developers transition, offer incentives or new features that resolve limitations of the deprecated endpoints.

* **Ease the transition**: Clearly communicate what’s being deprecated and why, and make sure to highlight the benefits of new features or solutions.
    
* **Offer adequate time**: Avoid short deprecation timelines that could damage trust with developers. For example, Salesforce supports each API version for at least three years and provides one year’s notice before deprecating it.
    
* **Mark deprecated fields**: For frameworks like GraphQL, mark deprecated fields but allow querying to prevent breaking existing clients.
    

> 3.1.2.2 Object Field deprecation
> 
> Fields in an object may be marked as deprecated as deemed necessary by the application. It is still legal to query for these fields (to ensure existing clients are not broken by the change), but the fields should be appropriately treated in documentation and tooling.
> 
> —From the GraphQL spec, working draft, October 2016

## Versioning

To manage changes in your API and ensure developers understand updates, versioning is essential.

### **Additive-change strategy**

It is a common approach, where all changes are backwards-compatible. Here’s what to avoid:

* Removing or renaming APIs or parameters
    
* Changing response field types
    
* Modifying API behavior
    
* Altering error codes or fault contracts
    

In this strategy, you can add new fields or endpoints, but any changes to existing fields require the user to opt-in via request parameters. For example, to control whether a user's friends list is included in the response, you can use a request parameter like `exclude_friends=1`, as shown in the example requests.

```json
// GET /users/1234?exclude_friends=1
{
    "id": 1234,
    "name":"Chen Hong",
    "username:" "chenhong",
    "date_joined": 1514773798
}

// GET /users/1234
{
    "id": 1234,
    "name":"Chen Hong",
    "username:""chenhong",
    "date_joined": 1514773798,
    "friends": [
        2341,
        3449,
        2352,
        2353,
        2358    
    ]
}
```

This strategy ensures your API remains compatible while allowing for new features or fields.

### Explicit-version strategy

The explicit versioning strategy is one of the essential aspects of API management. It allows developers to maintain backward compatibility while introducing new features or making changes.

**Versioning Strategies:**

1. **URI Versioning:**
    
    * **Example**: [`https://api.uber.com/v1.2/requests`](https://api.uber.com/v1.2/requests)
        
    * Version information is included in the URI path, making it easy to bind requests to versions using the base URI.
        
    * This is useful for GET requests, but it assumes permanence in resources (i.e., the endpoint should remain stable).
        
    * Redirection might be required if a version is deprecated or moved.
        
2. **HTTP Headers Versioning**:
    
    * **Example**: `Stripe-Version: 2018-02-28`
        
    * The version is specified through HTTP headers like `Stripe-Version` or `Accept`, making the API less visible but reducing URI bloat.
        
    * It can cause issues with caching and debugging in some cases but is often used in combination with other strategies.
        
3. **Request Parameters Versioning**:
    
    * **Example**: [`https://maps.googleapis.com/maps/api/js?v=3`](https://maps.googleapis.com/maps/api/js?v=3)
        
    * Versioning is handled through query parameters, often easy to implement but more difficult to manage with complex APIs or larger query parameter variability.
        

**Key Considerations for Versioning:**

* **Backward Compatibility**: Ensuring that older versions of the API continue to work without breaking existing integrations.
    
* **Codebase Management**: Deciding whether to fork the codebase, route requests to different controllers or use a transformation layer that converts older versions' data to the new schema.
    
* **Semantic Versioning (SemVer)**: Using MAJOR.MINOR.PATCH format helps to standardize versioning across APIs:
    
    * **MAJOR**: Breaks backward compatibility.
        
    * **MINOR**: Introduces new features in a backward-compatible way.
        
    * **PATCH**: Introduces backward-compatible bug fixes.
        

**Example of Major and Minor Changes:**

* **Major Changes**:
    
    * Changes in business logic that affect the output.
        
    * Removal or deprecation of an endpoint or feature.
        
* **Minor Changes**:
    
    * Adding new endpoints or request parameters.
        
    * Adding new response fields.
        

Whether you decide to version or not, managing change means finding a balance between maintaining backward compatibility and releasing changes with enough velocity for your developers to succeed on your platform. Don’t forget to get feedback, optimize for your developers, and make all changes in moderation.

## Conclusion

In this chapter, we explored various aspects of the ongoing process of developing and enhancing APIs through effective change management. Establishing a solid framework for handling continuous changes is crucial to fully realizing the potential of your API. By mastering change management, you can avoid being held back by past decisions and keep improving your API moving forward.

In **Chapter 8**, we will dive into the importance of building a thriving developer ecosystem. This will ensure that when you're ready to elevate your API, you'll have a strong base of developers eager to adopt and benefit from the new features and updates.

## **References**

[**Designing Web APIs: Building APIs That Developers Love**](https://www.goodreads.com/book/show/38396693-designing-web-apis?ref=nav_sb_ss_1_18)

**Thank you for reading, and I hope you found the insights helpful! 💖**