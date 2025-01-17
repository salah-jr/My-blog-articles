---
title: "Designing Web APIs: Chapter 1 - What's an API?"
seoTitle: "Understanding APIs: An Introduction"
seoDescription: "Discover what APIs are, their importance, typical users, and how businesses use them to innovate and grow in modern software development"
datePublished: Sat Oct 05 2024 20:42:08 GMT+0000 (Coordinated Universal Time)
cuid: cm1wmeapc000t09li8mix15bs
slug: designing-web-apis-chapter-1-whats-an-api
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1728589997684/d5bd0b90-cc30-4782-bd58-a80c49489616.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1728590031889/974c1b67-7e48-4e88-beaa-9dabe49a2086.jpeg
tags: apis, restful-apis, api-best-practices, api-for-developers

---

### **Overview**

Let’s get back to the chapter title “What’s an API?” As we know, it’s an application programming interface. However, APIs are so much more than their name suggests, and to understand what they really are, we must focus on the keyword *interface*.

This chapter explains APIs — What they are, why they matter, and who uses them. It discusses their role in modern software development, highlights the traits of effective APIs, and emphasizes their importance for business success and user satisfaction.

APIs serve as the interface between software programs, humans, and the internet. They reveal insights about the underlying program, including its business model and features. While APIs connect different programs, they are primarily designed for developers.

For instance, APIs allow weather data from sources like the National Weather Service to be shared with various apps. They also handle payment processing, enabling businesses to collect money easily without needing to manage the financial technology themselves. APIs are essential for the success of major internet companies like Amazon, Stripe, Facebook, and Google.

### **Why Do We Need APIs?**

APIs were created to share data with providers who can solve specific problems, so others don’t have to solve them from scratch. For example, you can embed a map without rebuilding Google Maps, let users sign in without recreating Facebook Login, or create a chatbot without building a messaging system. In these cases, APIs allow you to add features using data or services from specialized platforms. This helps businesses build unique products faster, by leveraging existing technologies and tapping into other ecosystems instead of reinventing everything.

### **Who Are Our Users?**

When building any product, it is a good idea to focus on the customer first. Understanding who your developers are, their needs, and why they are using your API helps ensure you're building something useful and aligned with their expectations. Focusing on developers prevents creating APIs that are difficult to use or irrelevant to their needs.

Since redesigning an API is costly and complex, it’s crucial to define and validate your API before implementation. Changing an API after it's in use can be expensive and time-consuming.

The author provides examples of different developer needs:

* Lisa, a web developer, needs an easy way for artists to upload and display photos.
    
* Ben, a backend developer, requires a way to store receipts from an expense system.
    
* Jane, a front-end developer, wants to integrate real-time customer support chat into her website.
    

### **The Business Case for APIs**

APIs play a crucial role in today’s tech market, often driving innovation and business growth. They can generate profit through various models (like subscriptions or usage fees) or support a company's broader strategy by enabling third-party integrations and new distribution channels. However, APIs must align with the core business, as seen with companies like GitHub and Stripe, otherwise, they risk conflicting with revenue models, like Twitter’s API affecting ad revenue.

**Let’s take a more detailed look at the following ways that some companies have structured their API development:**

* APIs for internal developers first, external developers second
    
* APIs for external developers first, internal developers second.
    
* APIs as the product.
    

**1- APIs for Internal Developers First, External Developers Second:**

Some companies build their APIs for internal developers first and then release them to external developers. There could be a number of motivations for this. One reason might be that the company sees potential value in adding an external API. This could create a developer ecosystem, drive new demand for the company’s product, or enable other companies to build products that the company itself does not want to build.

To take a look at a specific instance, let’s explore how Slack’s API started— Slack’s API began as an internal tool for its messaging platform but later evolved into a Developer Platform, allowing third-party developers to create integrations because the APIs were already tested and well used by internal developers. This move helped expand Slack’s ecosystem, though it also introduced challenges in balancing the needs of internal developers with those of external developers who required API stability.

**2- APIs for External Developers First, Internal Developers Second:**

Some companies create APIs for external stakeholders first and then release them to internal stakeholders.

GitHub initially developed its API for external developers to access their data, which led to the creation of businesses offering developer tools. Over time, GitHub expanded the API to serve both individual users and teams, helping them build workflow tools and integrations. This external-first approach allowed GitHub to customize the API for external needs before eventually serving internal developers.

**3- APIs as the Product:**

For some companies, the API is the product. Such is the case with Stripe and Twilio. Stripe provides APIs for payment processing on the internet. Twilio provides APIs for communication via SMS, voice, and messaging. In the case of these two companies, building an API is 100% aligned with a single-product audience. Their entire business focuses on creating seamless API experiences, making it the most straightforward approach for aligning product and customer needs.

### **Conclusion**

APIs are an essential part of today's tech products, and there are many ways to build a business around them. In [Chapter 2](https://blog.mohammedsalah.online/designing-web-apis-chapter-2-api-paradigms), we'll look at different API paradigms and cover key ideas for creating them.

### References

[**Designing Web APIs: Building APIs That Developers Love**](https://www.goodreads.com/book/show/38396693-designing-web-apis?ref=nav_sb_ss_1_18)