---
up: "[[00 - Networking - Overview]]"
---

###### Elevator Pitch
- Phase 5 shows how OkHttp interceptors can log requests and inject headers before the request reaches the server.

---

###### Definition
- An interceptor is middleware that can inspect or modify a request or response during the network flow.

---

###### Real-World Analogy
- Logging interceptor -> a camera recording what happens
- Auth header interceptor -> a security desk adding an access badge

---

###### What
- `HttpLoggingInterceptor` -> prints request and response details
- `authHeaderInterceptor` -> adds `Authorization: Bearer ...`
- `OkHttpClient.Builder().addInterceptor(...)` -> installs the interceptors

---

###### Why
- Helps you see exactly what the app sends and receives
- Centralizes auth header logic in one place
- Prepares you for token-based APIs later

---

###### Core Concepts
- Request pipeline -> the path a request takes before leaving the app
- Header injection -> automatically adding metadata to requests
- Token refresh -> replacing an expired token and retrying the request

---

###### How it Works
- The app creates a request
- The auth interceptor adds the header
- The logging interceptor prints the request
- OkHttp sends the request to the server
- The response comes back through the same pipeline

---

###### Syntax
- `HttpLoggingInterceptor` -> logs HTTP traffic
- `Interceptor { chain -> ... }` -> custom request or response hook
- `addInterceptor(...)` -> adds an interceptor to OkHttp

```kotlin
// OkHttp setup: logging and auth header interceptors
object NetworkClient {
    // Shared base URL for the sample API
    private const val BASE_URL = "https://jsonplaceholder.typicode.com/"

    // Logs HTTP request and response bodies
    private val loggingInterceptor = HttpLoggingInterceptor().apply {
        // BODY prints the full request and response
        level = HttpLoggingInterceptor.Level.BODY
    }

    // Adds an Authorization header to every outgoing request
    private val authHeaderInterceptor = Interceptor { chain ->
        // Start from the original request
        val request = chain.request().newBuilder()
            // Attach a header for learning purposes
            .addHeader("Authorization", "Bearer sample-token")
            .build()
        // Continue with the modified request
        chain.proceed(request)
    }
}
```

---

###### Common Mistakes
- BAD: adding logging in the ViewModel instead of the network layer
- BAD: hardcoding auth logic all over the app
- BAD: thinking token refresh is the same as header injection

---

###### Memory Hook
- Interceptors are the checkpoint before and after the request.

---

###### Key Rule
- Put cross-cutting network behavior in OkHttp, not in your UI or ViewModel.

---

###### Related
- [[00 - Networking - Overview]]
- [[04 - Phase 4 - Error Handling]]

