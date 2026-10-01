# _Design a URL shortener like Bitly_

The system should allow users to convert a long URL into a short URL, and when someone visits that short URL, they should be redirected to the original destination.

### _1. High-level design_

I would divide the system into three main components:
- Application server: Handles URL creation and redirection.
- Database: Stores the mapping between short codes and original URLs.
- Redis cache: Stores frequently accessed mappings to make redirects faster.

### _2. API design_

I would create two main endpoints.
- First, POST /api/shorten, which accepts a long URL and returns a short URL.
- Second, GET /:shortCode, which looks up the original URL and redirects the user.

### _3. Generating the short URL_

To generate a unique short code, I would generate a unique numeric ID and convert it into Base62, using uppercase letters, lowercase letters, and digits.

For example, the generated code might be aB91xZ.

This gives us relatively short URLs while supporting a large number of unique codes.

### _4. Database design_

I would create a table with the following fields:


| id |
|----|
| short_code |
| original_url |
| created_at |
| expires_at |

I would add a unique constraint on short_code to prevent duplicate mappings.

### _5. Redirection flow_

When a user visits a short URL, the server first checks Redis for the short code.
If the mapping exists in the cache, it retrieves the original URL directly.
If there is a cache miss, it fetches the mapping from the database and stores it in Redis for future requests.
Finally, the server returns a 302 redirect to the original URL.

### _6. Scalability_

If the application experiences high traffic, I would introduce a load balancer and multiple application servers.

I would use Redis to reduce database reads and consider database sharding as the number of URL mappings grows.

For analytics, I would use a message queue to process click events asynchronously, so analytics processing does not slow down redirects.

### _7. Edge cases_

I would also handle invalid URLs, nonexistent short codes, expired links, and duplicate codes.

Additionally, I would implement rate limiting to prevent abuse and validate URLs to protect users from malicious destinations.

Overall, I would start with a simple application server, a database, and Redis, and introduce additional infrastructure as the traffic and requirements grow."

### _Why would you use a 302 redirect instead of a 301 redirect?_

"A 301 indicates a permanent redirect and may be cached by browsers. A 302 indicates a temporary redirect and generally gives the service more control over subsequent requests. Since I may want to track clicks or change destinations, I would initially use 302, depending on the product requirements."
