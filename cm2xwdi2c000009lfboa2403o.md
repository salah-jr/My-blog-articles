---
title: "Designing Web APIs: Chapter 3 -  API Security"
seoTitle: "Designing Web APIs: Chapter 3 -  API Security"
seoDescription: "Explore essential best practices for securing APIs, including OAuth implementation, WebHook security, and strategies for safeguarding sensitive data."
datePublished: Thu Oct 31 2024 22:48:56 GMT+0000 (Coordinated Universal Time)
cuid: cm2xwdi2c000009lfboa2403o
slug: designing-web-apis-chapter-3-api-security
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1728690021055/20876009-bdf2-4002-8d0c-b44d8290f95c.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1728690048613/cbd996c1-7c7c-4145-8c12-0c4febf02f31.jpeg
tags: apis, programming-tips, api-security

---

API security is vital for web applications because new security risks appear all the time, and a breach can lead to serious data and financial losses. Engineers use methods like SSL, content checks, logs, and protections against attacks like CSRF and XSS. However, APIs shared with outside developers need extra protection beyond these standard practices. This chapter dives into the best practices companies use to keep their APIs secure. We’ll dive into security practices, including **Authentication and Authorization**, **OAuth**, and **WebHooks Security**, and wrap up with essential takeaways to strengthen your API security strategy.

# Authentication and Authorization

Authentication and authorization are foundational elements of API security:

* **Authentication** is about verifying identity, confirming who you are. Most web applications handle this by requiring you to log in with a username and password, which are checked against existing records to ensure the authenticity of your request.
    
* **Authorization** determines what actions you’re allowed to perform. For example, you might be permitted to view a page but restricted from editing it unless you’re an administrator. This is the core of authorization.
    

When designing an API, it’s essential to consider how developers will manage authentication and authorization securely. One of the earliest methods introduced for web access control was **Basic Authentication**. This technique is simple but comes with significant security drawbacks.

## ✨ **What is Basic Authentication?**

In Basic Authentication, clients send HTTP requests that include an `Authorization` header. This header consists of the word “Basic,” followed by a space and a base64-encoded string that combines the username and password (formatted as `username:password`). For example: `Authorization: Basic dXNlcjpwYXNzd29yZA==`

### Security Downsides of Basic Authentication

While Basic Authentication is simple, it has several significant security drawbacks:

1. **Credential Sharing**: Users often need to share their usernames and passwords with third-party applications, which can be risky.
    
2. **Storage Risks**: Applications must store these credentials in a readable or decryptable format. If exposed due to a bug or security breach, this can lead to serious data leaks, especially since many users reuse passwords across multiple services.
    
3. **Revocation Limitations**: Users cannot revoke access for a single application without changing their password, which affects all connected apps.
    
4. **Full Access**: Applications gain complete access to user accounts without the ability for users to limit access to specific resources.
    

Due to these vulnerabilities, platforms like Twitter discontinued support for Basic Authentication in 2010, opting for more secure authentication methods. As you design your API, consider implementing more robust authentication and authorization techniques to better protect user data and privacy.

## **✨ What is OAuth?**

To tackle the problems with Basic Authentication and other common methods, OAuth was introduced in 2007 as an open standard for secure access. The latest version, OAuth 2.0, has become the go-to protocol for authorization and is used by many well-known companies like Amazon, Google, GitHub, Stripe, and Slack.

### Benefits of OAuth

The primary advantage of OAuth is that it allows users to grant applications access without sharing their passwords. For instance, consider a scenario involving **Spotify**.

If Spotify wants to enable users to share their music playlists on **Twitter**, the process would work as follows:

1. When a user chooses to connect their Spotify account to Twitter, Spotify redirects the user to Twitter's authorization page.
    
2. The user logs in to Twitter and is presented with a prompt to authorize Spotify to access their account.
    
3. Upon granting permission, Twitter generates an access token that Spotify can use to post on the user’s behalf without needing the user’s Twitter password.
    

This way, users can safely authorize access while maintaining control over their credentials and privacy, significantly reducing security risks compared to Basic Authentication.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730400494277/cae08ebd-fa42-4fbd-92c4-43f62e856433.png align="center")

Another key benefit is that OAuth enables users to grant selective permissions. Each application may need different data, and OAuth allows users to control what resources they share. In the TripAdvisor example above, the app can read a user’s profile and friend list but cannot post on their behalf.

If a user decides to revoke TripAdvisor’s access to their Facebook data, they can easily do so through their Facebook settings, all without changing their password.

### How it works?

As an API provider, implementing OAuth allows you to give third-party applications secure access to your users' data without compromising their passwords. Here's a straightforward breakdown of how OAuth works, with examples relevant to your role as the API provider.

1. **Application Registration**:  
    Before third-party applications can use OAuth with your API, they must register with you as the API provider. During this registration, developers will provide a redirect URL (where users will go after authorization) and receive a client ID and client secret.
    
    * **Example**: Suppose you are providing an API for a travel application called *TravelBuddy*. When *TravelBuddy* registers with your API, you issue them a client ID (to identify the application) and a client secret (which they should keep confidential).
        
2. **User Authorization Request**:
    
    * When *TravelBuddy* wants to allow users to log in with their accounts, it displays a button labeled “Continue with Your Service.”
        
    * When a user clicks this button, they are redirected to your authorization page. During this redirect, the application sends:
        
        * **Client ID**: A unique identifier for *TravelBuddy*.
            
        * **Requested Permissions (Scope)**: For instance, access to the user’s profile and friend list.
            
        * **State Parameter**: A unique string to maintain the session state.
            
        * **Redirect URL**: Where you will send the user after they have authorized the request.
            
3. **API Provider Authorization**:
    
    * On your authorization page, users will see the permissions *TravelBuddy* is requesting.
        
    * **User Decision**:
        
        * If the user denies permission, they are redirected back to *TravelBuddy* with an `access_denied` error.
            
        * If the user approves, you redirect them back to *TravelBuddy* with an authorization code.
            
4. **Token Exchange**:
    
    * Now, *TravelBuddy* has an authorization code that can only be used once. They send this code, along with their client ID, client secret, and redirect URL, back to your API.
        
    * You verify these details and return an **access token**.
        
    * **Access Token Usage**: With this token, *TravelBuddy* can now securely access the user’s data (like their profile and friend list) from your API without needing their password.
        

By implementing OAuth, you enable third-party applications to securely access user data, enhancing user control over their credentials while maintaining robust security for your API.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730401744491/301f9b77-41fb-45e9-95f5-29a99bb82085.png align="center")

*💡****Note,*** *Follow the arrows from right to left, top to bottom.*

### Scopes

OAuth scopes help restrict an application’s access to user data. For example, if an application only needs to identify a user, it can request access to just the user’s profile information instead of all their data through specific OAuth scopes. During the authorization process, the API provider will show all requested scopes to the user, ensuring they understand what permissions they are granting to the application.

Defining scopes for an API presents an interesting challenge. Many APIs provide basic read, write, and combined read/write scopes. For instance, the Twitter API offers three permission levels:

* Read only
    
* Read and write
    
* Read, write, and access direct messages
    

Often, API providers don’t think much about scopes until their API becomes popular or gets misused. When that happens, adding new scopes can be complicated. It’s important to consider your goals and how your API will be used before deciding on the right scopes. Granular scopes ensure that applications only get the permissions they need, but too many can confuse users and developers.

**Here are some additional considerations for defining your scopes:**

* **Minimal Scope**: Provide a basic scope that only grants access to essential user information, such as name and profile picture. This is useful for sign-in flows and is offered by many API providers like Slack and Facebook.
    
* **Isolated Scopes for Sensitive Information**: Protect sensitive data with separate scopes. When an application requests these scopes, users should receive a clear warning about what they are sharing. For example, GitHub and Twitter have specific scopes to manage access to private repositories and direct messages, respectively.
    
* **Differentiate Scopes by Resource Type**: Large API providers often split scopes based on functionality. For instance, the Slack API has separate scopes for messages, channels, and users, while GitHub differentiates scopes for repositories, organizations, and notifications. This prevents applications from accessing resources they don’t need.
    

### Token and Scope Validation

After developers receive an access token, they can make API requests by including it in the HTTP Authorization header, as shown in Example 3-1.

*Example 3-1: Request to Slack API with Access Token*

```http
POST /api/chat.postMessage
HOST: slack.com
Content-Type: application/json
Authorization: Bearer xoxp-16501860-a24afg234

{
  "channel": "C0GEV71UG",
  "text": "This is a message text",
  "attachments": [{"text": "attachment text"}]
}
```

When an API request is received, the server needs to verify two things:

1. **Token Validity**: Ensure the access token is valid by checking it against the tokens stored in your database.
    
2. **Scope Authorization**: Confirm that the token has the necessary scope for the requested action.
    

If either check fails, the server should return an error. To aid developers, it’s helpful to return metadata about the required and granted scopes. Many APIs, including GitHub and Slack, provide two headers:

* **X-OAuth-Scopes**: Lists the authorized scopes for the token.
    
* **X-Accepted-OAuth-Scopes**: Lists the scopes required for the requested action.
    

*Example 3-2: OAuth Scope Headers in GitHub API Response*

```http
curl -H "Authorization: token OAUTH-TOKEN" https://api.github.com/users/saurabhsahni -I

HTTP/1.1 200 OK
X-OAuth-Scopes: repo, user
X-Accepted-OAuth-Scopes: user
```

If a token lacks the required scope, returning detailed error messages can simplify troubleshooting. For instance, the Slack API might return an error like this if the required scope is missing.

*Example 3-3: Response from Slack API for Missing Scope*

```http
{
  "ok": false,
  "error": "missing_scope",
  "needed": "chat:write:user",
  "provided": "identify,bot,users:read"
}
```

This response clearly indicates what scope was needed and what scopes were provided.

### Token Expiry and Refresh Tokens

OAuth allows access tokens to expire after a few hours or days. This limits the risk if a token gets stolen. To help applications get new tokens without bothering the user, we use **refresh tokens**.

A refresh token is a special token that lets an application request a new access token when the old one expires. To get a new access token, the application must provide the client ID, client secret, and the refresh token. Many services like Google, Salesforce, and Stripe use refresh tokens.

Even if access tokens don’t expire, having refresh tokens is a good idea. If an access token is compromised, developers can easily replace it using the refresh token.

**Why Short-Lived Access Tokens are Better:**

* **Less Risk:** If a token is stolen, it only works until it expires.
    
* **Secure:** A refresh token is useless without the client secret, which is kept safe.
    
* **Compromise Detection:** If both the refresh token and client secret are stolen, it’s easier to notice misuse since refresh tokens are usually one-time use only and can only be used by one party at a time.
    

### Listing and Revoking Authorizations

Users often want to see which applications can access their data and might want to revoke access for certain ones. To help with this, most API providers offer a dedicated page where users can view and manage authorized applications. This page usually features a “Revoke Access” button next to each application.

In addition to a user interface for revoking access, it’s also helpful to provide APIs that allow developers to revoke access tokens and refresh tokens programmatically. This is particularly useful if a token is compromised or needs to be revoked for any reason.

### OAuth Best Practices

When building your authorization server with OAuth, consider the following best practices:

* **State Parameter Support**: Implement the state parameter to help prevent CSRF attacks. This string is generated by the developer and is used to verify the user’s session.
    
* **Short-Lived Authorization Codes**: Ensure authorization codes expire within a few minutes and can be used only once to prevent unauthorized token generation.
    
* **One-Time-Use Refresh Tokens**: For applications with sensitive data, restrict refresh tokens to one-time use to detect token compromises. Allow a small time window for retries if needed.
    
* **Ability to Reset Client Secret**: Provide developers a way to reset the client secret if it’s compromised, preventing attackers from renewing access tokens.
    
* **Dedicated OAuth Scopes**: Use specific OAuth scopes to protect sensitive information, ensuring users only grant necessary permissions.
    
* **HTTPS Endpoints**: Require HTTPS for API endpoints to safeguard access tokens from man-in-the-middle attacks.
    
* **Verify Redirect URL**: Ensure the redirect URL matches registered URLs to prevent exposing secrets to attackers.
    
* **Disallow Iframe Rendering**: Use the `X-Frame-Options` header to block rendering of authorization pages in iframes, preventing clickjacking attacks.
    
* **Keep Users Informed**: Notify users via email when a new authorization is granted, helping them detect unintended access.
    
* **Prohibit Misleading Application Names**: Prevent apps from using misleading names that could trick users into thinking they are official applications.
    

# WebHooks Security

As discussed in [Chapter 2](https://hashnode.com/post/cm24oq6bw000509l38aghbhxs), WebHooks are URLs where API providers send POST requests upon certain events. Securing WebHooks differs from securing web APIs because the URLs are publicly accessible, making it essential to verify the sender of the requests.

#### Verification Tokens

API providers issue unique verification tokens to applications. Each WebHook request includes this token, which the application checks against its stored value. If the tokens don’t match, the request is ignored. While simple, this method has limited security since tokens are sent in plain text.

#### Request Signing

To enhance security, API providers often sign WebHook payloads using HMAC with a shared secret. Applications verify the request by computing the same HMAC and comparing it to the signature in the request header. This method is used by providers like Stripe and GitHub.

#### Preventing Replay Attacks

To prevent replay attacks, include a timestamp in the WebHook payload. If the timestamp is too old, the request can be rejected.

#### Mutual Transport Layer Security (TLS)

Mutual TLS allows both server and client to authenticate each other. This adds an additional layer of security for WebHook connections, especially in business-to-business scenarios.

#### Thin Payloads

Sending limited information in the WebHook payload can improve security. Applications would then make an authenticated API request to retrieve full event details, ensuring sensitive data is not exposed through WebHooks.

### Best Practices for WebHook Security

* Avoid sending sensitive information in WebHooks; use authenticated API requests instead.
    
* Include a timestamp in signed WebHooks to mitigate replay attacks.
    
* Allow for the regeneration of shared secrets used for verification or signing.
    
* Provide SDKs and sample code to help developers verify WebHook authenticity.
    

## Conclusion

Security is hard, and securing APIs is even harder. Once applications use a specific security method, changing it can be tricky, especially if vulnerabilities arise. It's important to think carefully about security before launching your API.

While creating new security methods might be tempting, it can lead to big risks if not properly reviewed. Sticking to well-established security standards is a safer choice.

In Chapter 4, we'll discuss best practices for designing APIs that improve the developer experience.

## **References**

[**https://www.goodreads.com/book/show/38396693-designing-web-apis?ref=nav\_sb\_ss\_1\_18**](https://www.goodreads.com/book/show/38396693-designing-web-apis?ref=nav_sb_ss_1_18)

**Thank you for reading! I hope you found the insights helpful.**