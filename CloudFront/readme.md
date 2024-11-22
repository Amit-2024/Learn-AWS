# CloudFront
suppose a application is on the server in america, now to accesss it each time we need to req. american server and for all the people who need to access it will make a req. to american server which takes a bit more time
or delay but american can access it a bit faster.
Here comes the service cloudfront
which uses edge locations worldwide to cache content for faster accesss, closed to user location.

# Definition
Amazon CloudFront is a content delivery network service that caches and delivers content from edge locations worldwide, 
improving performance, scalability, and security. It integrates seamlessly with AWS services like S3, EC2, and Lambda@Edge.

# Key Components of CloudFront
#### a) Distributions
Represents a CloudFront deployment and configuration.

Types of Distributions:
> Web Distribution: For websites and APIs.
> RTMP Distribution (Deprecated): For media streaming. (No longer used)


#### b) Origins
The source of the content that CloudFront delivers (e.g., S3 bucket, ALB, custom server).

#### c) Cache Behavior
Defines how CloudFront should handle specific URL patterns:


Path Patterns: Match URLs to customize cache settings.
Cache TTLs (Time-to-Live): Controls how long objects are cached.
Query Strings: Controls caching based on query parameters.
Headers and Cookies: Customize caching based on request headers and cookies.
d) Edge Locations
Data centers where CloudFront caches content for low-latency delivery.
Requests are routed to the nearest edge location automatically.

# 4. Common Use Cases
### a) Static Website Hosting
Deliver static assets (HTML, CSS, JS, images) with low latency and high performance.
Integrates with S3 for scalable hosting.

### b) API Acceleration
Reduces latency for API responses by caching common queries at edge locations.

### c) Video Streaming
Supports progressive downloads and streaming with adaptive bitrate formats like HLS and DASH.
Delivers media files securely and efficiently.

### d) Secure Content Delivery
Distribute private content using signed URLs or signed cookies.
Prevent unauthorized access to sensitive files.

### e) Real-Time Applications
Improve performance for live events and gaming applications by caching frequently accessed data.
