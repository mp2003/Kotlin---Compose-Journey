---
up: "[[00 - Networking - Overview]]"
---

###### Elevator Pitch
- Phase 1 wires up Retrofit and OkHttp from scratch, makes a single GET call to fetch posts, and logs the result — no repository, no MVI, no Hilt yet.

---

###### Definition
- The minimal setup needed to make a network request in Android: an API interface, a data class, a Retrofit client, and one coroutine call

---

###### Real-World Analogy
- Retrofit -> a waiter who takes your order and brings the food back
- OkHttp -> the kitchen that actually does the cooking
- Interceptor -> a security guard who inspects every order going in and out
- Gson -> a translator who converts the kitchen's output into your language

---

###### What
- `PostDto` -> data class that mirrors the JSON response shape
- `PostsApiService` -> interface that declares what endpoints exist
- `NetworkClient` -> singleton that builds and holds the Retrofit instance
- `lifecycleScope.launch` -> runs the network call on the Activity's coroutine scope
- `@GET("posts")` -> tells Retrofit which URL to call

---

###### Why
- Phase 1 keeps it raw on purpose — no extra layers, so you can see exactly what Retrofit does
- Adding Hilt/repo/MVI on top of something you don't understand yet just hides the mechanics

---

###### Core Concepts
- `Retrofit` -> HTTP client that reads your interface and generates the real implementation
- `OkHttpClient` -> the layer that physically sends the request and reads the response
- `HttpLoggingInterceptor` -> prints every request URL, headers, and response body to Logcat
- `GsonConverterFactory` -> tells Retrofit to use Gson to parse JSON into your data class
- `@GET` -> annotation that marks a function as a GET request
- `suspend fun` -> the function can pause while waiting for the network — no thread blocking
- `data class` -> auto-matches JSON field names to Kotlin properties
- Link: [[00 - Networking - Overview]]

---

###### How it Works
- App starts -> `MainActivity.onCreate()` runs
- `lifecycleScope.launch` starts a coroutine on the main thread
- `NetworkClient.postsApiService.getPosts()` is called
- Retrofit sends the request to OkHttp
- OkHttp opens a connection to `jsonplaceholder.typicode.com`
- `HttpLoggingInterceptor` prints the full request + response to Logcat
- Gson parses the JSON array into `List<PostDto>`
- The coroutine resumes with the result
- `MainActivity` logs the count and first 3 titles

---

###### ASCII Flowchart

```text
[ MainActivity.onCreate() ]
        |
        v   lifecycleScope.launch { }
[ NetworkClient.postsApiService.getPosts() ]
        |
        v   Retrofit generates the real call
[ OkHttpClient sends HTTP GET /posts ]
        |
        v   HttpLoggingInterceptor prints to Logcat
[ jsonplaceholder.typicode.com responds with JSON ]
        |
        v   GsonConverterFactory parses JSON
[ List<PostDto> returned to MainActivity ]
        |
        v
[ Log.d prints count + first 3 titles ]
```

---

###### Syntax

- `@GET("posts")` -> marks this function as a GET request to `/posts`
- `suspend fun getPosts(): List<PostDto>` -> async function, returns parsed list
- `object NetworkClient` -> singleton — one instance, shared everywhere
- `HttpLoggingInterceptor().apply { level = Level.BODY }` -> log full request + response
- `OkHttpClient.Builder().addInterceptor(...).build()` -> add interceptor to the HTTP engine
- `Retrofit.Builder().baseUrl(...).client(...).addConverterFactory(...).build()` -> build Retrofit
- `retrofit.create(PostsApiService::class.java)` -> generate the real implementation of the interface
- `lifecycleScope.launch { }` -> start coroutine tied to Activity lifetime

```kotlin
// Minimal skeleton — Phase 1 setup

// 1. Data class — mirrors JSON shape exactly
data class PostDto(
    val userId: Int,
    val id: Int,
    val title: String,
    val body: String
)

// 2. API interface — one function per endpoint
interface PostsApiService {
    @GET("posts")                            // GET /posts
    suspend fun getPosts(): List<PostDto>    // returns parsed list
}

// 3. Retrofit client — singleton, built once
object NetworkClient {
    private val loggingInterceptor = HttpLoggingInterceptor().apply {
        level = HttpLoggingInterceptor.Level.BODY   // log full request + response
    }

    private val okHttpClient = OkHttpClient.Builder()
        .addInterceptor(loggingInterceptor)          // attach interceptor
        .build()

    private val retrofit = Retrofit.Builder()
        .baseUrl("https://jsonplaceholder.typicode.com/")
        .client(okHttpClient)                        // use our OkHttp setup
        .addConverterFactory(GsonConverterFactory.create())  // JSON -> Kotlin
        .build()

    val postsApiService: PostsApiService =
        retrofit.create(PostsApiService::class.java) // generate real impl
}

// 4. Call from Activity
lifecycleScope.launch {
    try {
        val posts = NetworkClient.postsApiService.getPosts()
        Log.d("PostsApp", "Fetched ${posts.size} posts")
    } catch (e: Exception) {
        Log.e("PostsApp", "Error", e)
    }
}
```

---

###### Example (Step-by-step)

**Step 1 — PostDto: match the JSON shape**

```kotlin
// Each field name must match the JSON key exactly
// Gson uses the property name to find the matching JSON field
data class PostDto(
    val userId: Int,    // "userId": 1
    val id: Int,        // "id": 1
    val title: String,  // "title": "..."
    val body: String    // "body": "..."
)
```

- If a field name doesn't match the JSON key, Gson sets it to null/0

**Step 2 — PostsApiService: declare the endpoint**

```kotlin
interface PostsApiService {
    @GET("posts")                          // appended to base URL: /posts
    suspend fun getPosts(): List<PostDto>  // Retrofit calls this for us
}
```

- Retrofit generates a real class from this interface at runtime

**Step 3 — NetworkClient: wire everything together**

```kotlin
object NetworkClient {
    // Level.BODY = log URL + headers + full response body
    private val loggingInterceptor = HttpLoggingInterceptor().apply {
        level = HttpLoggingInterceptor.Level.BODY
    }

    // OkHttp is the engine — interceptors attach here
    private val okHttpClient = OkHttpClient.Builder()
        .addInterceptor(loggingInterceptor)
        .build()

    // Retrofit sits on top of OkHttp
    private val retrofit = Retrofit.Builder()
        .baseUrl("https://jsonplaceholder.typicode.com/")
        .client(okHttpClient)
        .addConverterFactory(GsonConverterFactory.create())
        .build()

    // This is the object you call in your code
    val postsApiService: PostsApiService =
        retrofit.create(PostsApiService::class.java)
}
```

- `retrofit.create(...)` is where Retrofit generates the implementation of your interface

**Step 4 — MainActivity: make the call**

```kotlin
lifecycleScope.launch {                     // coroutine tied to Activity
    try {
        val posts = NetworkClient.postsApiService.getPosts()
        Log.d("PostsApp", "Fetched ${posts.size} posts")
        posts.take(3).forEach { post ->
            Log.d("PostsApp", "Post: ${post.id} - ${post.title}")
        }
    } catch (e: Exception) {
        Log.e("PostsApp", "Failed to fetch posts", e)
    }
}
```

- `try/catch` is the only error handling in Phase 1 — Phase 4 replaces this properly

---

###### What the Interceptor Logs (Logcat)

```text
--> GET https://jsonplaceholder.typicode.com/posts
--> END GET

<-- 200 OK https://jsonplaceholder.typicode.com/posts
Content-Type: application/json
[
  { "userId": 1, "id": 1, "title": "..." },
  ...
]
<-- END HTTP (body truncated)
```

- Every outgoing request shows `-->`, every response shows `<--`
- `Level.BODY` = most verbose — shows headers + full body
- `Level.HEADERS` = headers only
- `Level.BASIC` = just URL + response code
- `Level.NONE` = silent

---

###### Common Mistakes
- BAD: forgetting `INTERNET` permission in `AndroidManifest.xml` -> silent failure, no network
- BAD: calling `getPosts()` on the main thread without `lifecycleScope.launch` -> crash
- BAD: field names in `PostDto` that don't match JSON keys -> Gson gives you nulls/zeros
- BAD: using `http://` base URL without `android:usesCleartextTraffic="true"` -> blocked by Android
- BAD: forgetting `.build()` at the end of `OkHttpClient.Builder()` or `Retrofit.Builder()`

---

###### Weak Area Clarification
- Confusion: "Why do we need both Retrofit AND OkHttp?"
- Why: they do different jobs — Retrofit is the API layer, OkHttp is the transport layer
- Resolution: Retrofit reads your interface and builds the request; OkHttp physically sends it. Retrofit cannot work without OkHttp under the hood.

---

###### Common Follow-up Traps
- Q: Why is `getPosts()` a `suspend fun`?
  A: Network calls take time — `suspend` lets the coroutine pause and wait without blocking the thread
- Q: What does `retrofit.create(PostsApiService::class.java)` actually do?
  A: Retrofit generates a real class that implements your interface at runtime — you never write the implementation yourself
- Q: Why `object NetworkClient` instead of a regular class?
  A: `object` is a singleton — one Retrofit instance shared across the app. Creating Retrofit repeatedly is expensive.
- Q: What happens if the JSON has extra fields my data class doesn't have?
  A: Gson ignores them by default — only fields you declare get mapped

---

###### Keywords Used

| Keyword                          | What it does                                                                   |
| -------------------------------- | ------------------------------------------------------------------------------ |
| `data class`                     | auto-generates equals/hashCode/copy/toString; field names must match JSON keys |
| `interface`                      | declares the API contract — Retrofit generates the real implementation         |
| `object`                         | singleton — one instance for the whole app                                     |
| `suspend fun`                    | function that can pause while waiting for network without blocking the thread  |
| `@GET("posts")`                  | marks this function as an HTTP GET to the given path                           |
| `HttpLoggingInterceptor`         | prints every request and response to Logcat                                    |
| `.apply { }`                     | runs a block on the object and returns the same object                         |
| `OkHttpClient.Builder()`         | builder pattern — configure OkHttp step by step                                |
| `Retrofit.Builder()`             | builder pattern — configure Retrofit step by step                              |
| `GsonConverterFactory`           | tells Retrofit to use Gson to parse JSON into your data class                  |
| `retrofit.create(X::class.java)` | generates a real implementation of your API interface                          |
| `lifecycleScope.launch`          | starts a coroutine tied to the Activity's lifetime                             |
| `try / catch`                    | handles errors — catches any exception thrown by the network call              |
| `.take(3)`                       | returns only the first 3 items from a list                                     |
| `forEach`                        | loops over every item in a list                                                |

---

###### Memory Hook
- Retrofit = waiter, OkHttp = kitchen, Interceptor = security guard, Gson = translator

---

###### Key Rule
- Phase 1 is intentionally raw — no Hilt, no repo, no MVI. Understand the plumbing before you hide it.

---

###### Related
- [[00 - Networking - Overview]]
