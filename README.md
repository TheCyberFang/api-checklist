# API Testing Checklist

## Authentication
- [ ] Don't use `Basic Auth`. Use standard authentication instead (e.g., [JWT](https://jwt.io/), [OAuth](https://oauth.net/)).
- [ ] Don't reinvent the wheel in `Authentication`, `token generation`, `password storage`. Use the standards.
- [ ] Use `Max Retry` and jail features in Login.
- [ ] Use encryption on all sensitive data.
- [ ] Use [Authorizaion Code grant](https://oauth.net/2/grant-types/authorization-code/) with [PKCE (Proof Key for Code Exchange)](https://oauth.net/2/pkce/) instead of the [Implicit Grant](https://oauth.net/2/grant-types/implicit/) for Single Page Applications (SPAs) 

### JWT (JSON Web Token)
- [ ] Use a random complicated key (`JWT Secret`) to make brute forcing the token very hard.
- [ ] Don't extract the algorithm from the header. Force the algorithm in the backend (`HS256` or `RS256`).
- [ ] Make token expiration (`TTL`, `RTTL`) as short as possible.
- [ ] Don't store sensitive data in the JWT payload, it can be decoded [easily](https://jwt.io/#debugger-io).
- [ ] Avoid storing too much data. JWT is usually shared in headers and they have a size limit.

### OAuth
- [ ] Always validate `redirect_uri` server-side to allow only whitelisted URLs.
- [ ] Always try to exchange for code and not tokens (don't allow `response_type=token`).
- [ ] Use `state` parameter with a random hash to prevent CSRF on the OAuth authentication process.
- [ ] Define the default scope, and validate scope parameters for each application.

## Access
- [ ] Limit requests (Throttling) to avoid DDoS / brute-force attacks.
- [ ] Use HTTPS on server side with TLS 1.2+ and secure ciphers to avoid MITM (Man in the Middle Attack).
- [ ] Use `HSTS` header with SSL to avoid SSL Strip attacks.
- [ ] Turn off directory listings.
- [ ] For private APIs, allow access only from whitelisted IPs/hosts.

## Input / Input Validation
- [ ] Use the proper HTTP method according to the operation: `GET (read)`, `POST (create)`, `PUT/PATCH (replace/update)`, and `DELETE (to delete a record)`, and respond with `405 Method Not Allowed` if the requested method isn't appropriate for the requested resource.
- [ ] Validate `content-type` on request Accept header (Content Negotiation) to allow only your supported format (e.g., `application/xml`, `application/json`, etc.) and respond with `406 Not Acceptable` response if not matched.
- [ ] Validate `content-type` of posted data as you accept (e.g., `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, etc.).
- [ ] Validate user input to avoid common vulnerabilities (e.g., `XSS`, `SQL-Injection`, `Remote Code Execution`, etc.).
- [ ] Don't use any sensitive data (`credentials`, `Passwords`, `security tokens`, or `API keys`) in the URL, but use standard Authorization header.
- [ ] Use only server-side encryption.
- [ ] Use an API Gateway service to enable caching, Rate Limit policies (e.g., `Quota`, `Spike Arrest`, or `Concurrent Rate Limit`) and deploy APIs resources dynamically.
- [ ] Validate all user-supplied input in the headers and in the body before they are processed
- [ ] Validate the type (e.g. integer, string, floating point number), size (e.g. minimum string lengths, minimum abd maximum values for numbers) and format the composition of expected inputs
- [ ] Validate the sizes of the JSON arrays and number of child elements in XML requests
- [ ] Use built-in libraries or annotations for input validation as much as possible, build custom validators if built-in functionality is inadequate 
- [ ] Validate the size of the request body and request headers (at preferably at the API gateway)
- [ ] Do not attempt to sanitise input (e.g. remove certain bad characters or strings) 
- [ ] Make sure that if input validation fail, the request is rejected with an appropriate error HTTP response

## Processing
- [ ] Check if all the endpoints are protected behind authentication to avoid broken authentication process.
- [ ] User own resource ID should be avoided. Use `/me/orders` instead of `/user/654321/orders`.
- [ ] Don't auto-increment IDs. Use `UUID` instead.
- [ ] If you are parsing XML data, make sure entity parsing is not enabled to avoid `XXE` (XML external entity attack).
- [ ] If you are parsing XML, YAML or any other language with anchors and refs, make sure entity expansion is not enabled to avoid `Billion Laughs/XML bomb` via exponential entity expansion attack.
- [ ] Use a CDN for file uploads.
- [ ] If you are dealing with huge amount of data, use Workers and Queues to process as much as possible in background and return response fast to avoid HTTP Blocking.
- [ ] Do not forget to turn the DEBUG mode OFF.
- [ ] Use non-executable stacks when available.

## Output
- [ ] Send `X-Content-Type-Options: nosniff` header.
- [ ] Send `X-Frame-Options: deny` header.
- [ ] Send `Content-Security-Policy: default-src 'none'` header.
- [ ] Remove fingerprinting headers - `X-Powered-By`, `Server`, `X-AspNet-Version`, etc.
- [ ] Force `content-type` for your response. If you return `application/json`, then your `content-type` response is `application/json`.
- [ ] Don't return sensitive data like `credentials`, `passwords`, or `security tokens`.
- [ ] Return the proper status code according to the operation completed. (e.g., `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, etc.).
