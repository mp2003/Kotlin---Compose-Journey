---
up: "[[00 - Networking - Overview]]"
---

###### Elevator Pitch
- Phase 2 moves the network call into a repository so the UI stops knowing how Retrofit works.

---

###### Definition
- A repository is a thin data layer class that fetches data and hides the network details from the screen.

---

###### Real-World Analogy
- Repository -> the front desk that gets the request and brings back the answer
- ViewModel -> the person asking for the data
- UI -> the person showing the result

---

###### What
- `PostRepository` -> owns the post-fetching logic
- `NetworkClient` -> still builds and exposes Retrofit
- `MainActivity` or `PostViewModel` -> calls the repository instead of Retrofit directly

---

###### Why
- Keeps network code out of the UI layer
- Gives one home for data access
- Makes later MVI and error handling easier to wire

---

###### Core Concepts
- Repository -> one place for data access
- Separation of concerns -> UI renders, repository fetches
- Thin abstraction -> only the logic needed right now

---

###### How it Works
- The caller asks `PostRepository` for posts
- The repository calls `NetworkClient.postsApiService`
- Retrofit returns the parsed data
- The repository gives the result back to the caller

---

###### Syntax
- `class PostRepository` -> wraps data access for posts
- `suspend fun getPosts()` -> pauses while the network call runs
- `NetworkClient.postsApiService.getPosts()` -> executes the actual request

```kotlin
// Repository: one place for the post request
class PostRepository {
    // Suspend function because network work must happen asynchronously
    suspend fun getPosts(): ApiResult<List<PostDto>> {
        // Try the remote request and wrap the response
        return try {
            // Ask Retrofit for the posts list
            val posts = NetworkClient.postsApiService.getPosts()
            // Return the success path with data
            ApiResult.Success(posts)
        } catch (e: Exception) {
            // Return the error path with a readable message
            ApiResult.Error(e.message ?: "Something went wrong")
        }
    }
}
```

---

###### Common Mistakes
- BAD: calling Retrofit directly from the screen
- BAD: putting too much logic inside the repository
- BAD: turning the repository into a second ViewModel

---

###### Memory Hook
- Repository means "one door in, one place out".

---

###### Key Rule
- Keep the repository thin and focused on data access only.

---

###### Related
- [[00 - Networking - Overview]]
- [[03 - Phase 3 - MVI Integration]]

