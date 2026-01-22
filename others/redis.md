1. it has a server
2. .env-->start/env.ts -->it check if the .env has every details before running
3. Improved Caching with Redis
4. npm i @adonisjs/redis
5. congigure this in file--->node ace configure @adonisjs/redis
6. adonisrc.ts file
7. config/redis.ts
8. https://github.com/microsoftarchive/redis/releases
9. redis is a linus orianted
10. redis-server --version
Redis server v=3.0.504

redis-cli ping
PONG

---

## 1️⃣ What is Redis?

![Image](https://backendless.com/wp-content/uploads/2022/12/How-Redis-typically-works.png)

![Image](https://substackcdn.com/image/fetch/%24s_%21XX4R%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8533bf77-bf19-4feb-b744-c4c29bd38592_1616x892.jpeg)

![Image](https://cdn.sanity.io/images/sy1jschh/production/3500a21ecb118b03a8d17970ab221d37df7f03d1-1024x314.webp?auto=format\&fit=clip\&q=80\&w=3840)

**Redis** is an **in-memory key–value data store**.

* Stores data in **RAM**, not on disk
* Extremely **fast** (much faster than DB queries)
* Used for:

  * Caching
  * Sessions
  * Rate limiting
  * Queues
  * Temporary data

### Simple idea:

Instead of hitting the **database every time**, you:

1. Store data in Redis
2. Read from Redis when possible
3. Fall back to DB only if needed

---

## 2️⃣ Why do we use Redis in AdonisJS?

In backend apps like **AdonisJS**, Redis is used to:

✅ Reduce database load
✅ Improve API response speed
✅ Cache frequently used data
✅ Share state between requests
✅ Handle sessions & background jobs

### Example without Redis:

```
Request → Database → Response (slow)
```

### Example with Redis:

```
Request → Redis (fast) → Response
        ↓
      Database (only if cache miss)
```

---

## 3️⃣ What this file does (High-level)

This file:

✔ Creates a **CacheService class**
✔ Wraps Redis logic into **clean reusable methods**
✔ Converts JS objects → JSON → Redis
✔ Converts Redis JSON → JS objects
✔ Exports a **singleton cache instance**

So anywhere in your Adonis app, you can simply do:

```ts
import cache from '#services/cache_service'
```

---

## 4️⃣ Line-by-line explanation

---

### 🔹 Import Redis service

```ts
import redis from '@adonisjs/redis/services/main'
```

* This imports the **configured Redis client** from AdonisJS
* Adonis already knows:

  * Redis host
  * Port
  * Password (from `.env`)

You don’t manually connect — Adonis handles it.

---

### 🔹 CacheService class

```ts
class CacheService {
```

A class that **encapsulates all Redis operations**.

This follows:

* **Service pattern**
* **Single Responsibility Principle**

---

### 🔹 Check if key(s) exist

```ts
async has(...keys: string[]) {
  return redis.exists(keys)
}
```

* `exists` checks if keys exist in Redis
* `...keys` → rest operator (multiple keys)

🔸 Example:

```ts
await cache.has('user:1', 'user:2')
```

🔸 Redis returns:

* `0` → none exist
* `1+` → number of keys found

---

### 🔹 Get value from Redis

```ts
async get(key: string) {
  const value = await redis.get(key)
  return value && JSON.parse(value)
}
```

What happens here:

1. Fetch value from Redis (string)
2. Convert JSON string → JS object
3. Return `null` if key doesn’t exist

🔸 Example:

```ts
const user = await cache.get('user:1')
```

---

### 🔹 Set value in Redis

```ts
async set(key: string, value: any) {
  return redis.set(key, JSON.stringify(value))
}
```

* Redis stores **strings only**
* So:

  * JS object → JSON string
  * Save in Redis

🔸 Example:

```ts
await cache.set('user:1', { name: 'Aswin', role: 'admin' })
```

---

### 🔹 Delete key(s)

```ts
async delete(...keys: string[]) {
  return redis.del(keys)
}
```

* Deletes one or multiple keys

🔸 Example:

```ts
await cache.delete('user:1', 'user:2')
```

---

### 🔹 Clear entire Redis DB

```ts
async flushDb() {
  return redis.flushdb()
}
```

⚠️ **Dangerous in production**

* Deletes **all keys** in the current Redis database
* Useful in:

  * Testing
  * Development
  * Reset cache

---

### 🔹 Create and export instance

```ts
const cache = new CacheService()
export default cache
```

* Creates a **single shared instance**
* Avoids multiple Redis connections
* This is a **singleton pattern**

---

## 5️⃣ Real-world usage example

### Cache DB query result

```ts
const cacheKey = 'users:list'

const cachedUsers = await cache.get(cacheKey)
if (cachedUsers) {
  return cachedUsers
}

const users = await User.all()
await cache.set(cacheKey, users)
return users
```

### Result:

* First request → DB
* Next requests → Redis (⚡ very fast)

---

## 6️⃣ Why this design is GOOD

✔ Clean abstraction
✔ Easy to reuse
✔ Easy to replace Redis later
✔ Centralized cache logic
✔ No Redis code scattered everywhere

---

## 7️⃣ One improvement suggestion (optional)

Add **TTL (expiry)**:

```ts
async set(key: string, value: any, ttlSeconds = 60) {
  return redis.set(key, JSON.stringify(value), 'EX', ttlSeconds)
}
```

This prevents stale cache.

---

## 8️⃣ Interview-ready explanation (short)

> “Redis is an in-memory key-value store used for caching to improve performance.
> In this file, we created a CacheService in AdonisJS that wraps Redis operations like get, set, delete, and exists.
> Data is stored as JSON strings and parsed back into objects, allowing fast access without hitting the database every time.”

---
