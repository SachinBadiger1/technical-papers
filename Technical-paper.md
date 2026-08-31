# Caching

## Introduction

In computing, caching is a technique of storing frequently accessed data in a high-speed storage layer so that it can be retrieved more quickly when requested again. The cached data is generally temporary and is kept separately from the primary storage location, such as a database or disk. Instead of repeatedly fetching or calculating the same data, an application can reuse the cached result, which helps reduce processing time, improve response speed, and decrease the load on the primary data source.

## How Caching Works

A typical caching process works as follows:

1. The application receives a request for some data.
2. The application checks whether the required data exists in the cache.
3. If the data exists, it is returned directly from the cache. This is called a cache hit.
4. If the data does not exist, it is retrieved from the primary database or another data source. This is called a cache miss.
5. The application stores the retrieved data in the cache.
6. Future requests for the same data can be served from the cache.

Consider an e-commerce application where thousands of users frequently request information about the same product. Querying the database for every request can increase database load and response time. By storing frequently accessed product information in a cache, most requests can be served without accessing the database.

## Caching Strategies

A caching strategy determines when data should be stored in the cache, when cached data should be used, and when fresh data should be retrieved from the network. The right strategy depends on the application's requirements, especially performance and data freshness.

### Cache First

  The application checks the cache before accessing the network. If the data is available, it is returned immediately. Otherwise, it is fetched from the network and can be stored in the cache. This strategy is useful for static resources such as images, CSS, and JavaScript files.

### Cache First with Refresh

  Also called stale-while-revalidate, this strategy returns cached data immediately while requesting an updated version from the network in the background. The new response is then used to refresh the cache. It provides fast responses while keeping cached data reasonably fresh.
  
### Network First

  The application tries the network first to obtain the latest data. If the network request fails, it uses the cached version if available. This strategy is useful for frequently changing data such as messages, news, and account information.

Choosing the right caching strategy depends on how often the data changes and how important freshness is. Different resources in the same application can use different caching strategies.

## Cache Expiration and TTL

Cache expiration determines how long data remains in the cache before it is removed. This is commonly controlled using Time to Live (TTL), which defines the maximum time a cached item can remain valid. When the TTL expires, the application can fetch the latest data from the primary data source.

The appropriate TTL depends on how frequently the data changes and how important data freshness is. Static data that rarely changes can have a longer TTL, while frequently changing data should use a shorter TTL to reduce the risk of serving outdated information.

A small random variation, known as TTL jitter, can also be added to expiration times. This prevents multiple cached items from expiring at the same time and causing a sudden increase in database requests. Proper cache expiration and TTL settings help balance performance, database load, and data freshness.


## Cache Invalidation

Cache invalidation is the process of removing or updating cached data when it is no longer valid or has become outdated. It helps ensure that the cache contains relevant information and stays consistent with the primary data source.

For example, when a user's profile information is updated in the database, the corresponding cached data should also be updated or removed. Otherwise, the application may continue to show outdated information. Cache invalidation becomes more challenging in distributed systems because the same data may exist in multiple caches or system layers. Proper invalidation helps maintain data consistency and prevents applications from serving incorrect or stale information.

## Types of Caches

Caching can exist at different levels of a system.

### Browser Cache

Web browsers can store resources such as HTML, CSS, JavaScript, and images locally. When the same resources are requested again, the browser may reuse the cached versions instead of downloading them again.

### Application Cache

An application can maintain frequently accessed objects in memory. This provides very fast access but may not work well when an application has multiple server instances because each instance may have its own cache.

### Distributed Cache

A distributed cache such as Redis can be shared by multiple application servers. This is useful for distributed systems where several application instances need access to the same cached data.

### CDN Cache

Content Delivery Networks cache static and sometimes dynamic content at locations closer to users. This reduces network latency and decreases the amount of traffic reaching the origin server.

## Conclusion

Caching is an important technique for improving the performance and scalability of modern software applications. By storing frequently accessed data in a faster storage layer, applications can reduce database load and respond to users more quickly. Common strategies such as cache-aside, read-through, write-through, and write-back provide different ways of managing cached data. However, caching also introduces challenges such as stale data, invalidation, memory usage, and consistency. A good caching design therefore considers the application's read and write patterns, freshness requirements, cache size, TTL, and failure scenarios. When implemented correctly, caching can significantly improve application performance while reducing the load on expensive backend resources.

## References
* [AWS - Caching]([https://aws.amazon.com/caching/))
* [MDN Web Docs - Caching Stratergies]([https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching#caching_strategies))
* [AWS - Cache Validity]([https://docs.aws.amazon.com/whitepapers/latest/database-caching-strategies-using-redis/cache-validity.html))
* [Redis Documentation - Cache-invalidatio]([https://redis.io/docs/latest/develop/use-cases/cache-aside/](https://redis.io/glossary/cache-invalidation/))
* [MDN Web Docs - Types of caches ]([https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Cache-Control](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching#types_of_caches))
