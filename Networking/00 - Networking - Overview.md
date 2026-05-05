###### Elevator Pitch
- Week 3 covers networking in Android — how your app talks to a server, fetches data, and handles errors using Retrofit, OkHttp, and MVI.

---

###### What
- Retrofit -> turns an interface into real HTTP calls
- OkHttp -> the engine that sends/receives bytes over the network
- Interceptors -> middleware that runs on every request/response
- Serialization -> converts JSON text into Kotlin objects
- ApiResult -> a sealed class that wraps success/error cleanly
- MVI -> connects the API response to the UI via state

---

###### Build Goal
- Posts Feed App — fetches posts from a REST API, displays them in a list, handles loading/error states

---

###### Learning Phases
- Phase 1 -> Basic GET call, raw Retrofit, log result
- Phase 2 -> Move call into Repository
- Phase 3 -> Connect to MVI (UiState, Intent, ViewModel)
- Phase 4 -> Error handling with ApiResult
- Phase 5 -> Interceptors (logging, auth header)

---

###### Project Structure (grows phase by phase)

```text
postsapp/
+-- data/
|     +-- remote/
|           +-- PostDto.kt          (JSON -> Kotlin)
|           +-- PostsApiService.kt  (API interface)
|           +-- NetworkClient.kt    (Retrofit setup)
+-- domain/
|     +-- model/
|           +-- Post.kt             (Phase 2+)
+-- data/repository/
|     +-- PostRepository.kt         (Phase 2+)
+-- presentation/post/
|     +-- PostsScreen.kt            (Phase 3+)
|     +-- PostsViewModel.kt         (Phase 3+)
|     +-- PostsUiState.kt           (Phase 3+)
|     +-- PostsIntent.kt            (Phase 3+)
+-- di/
      +-- NetworkModule.kt          (Phase 2+)
```

---

[[01 - Phase 1 - Basic GET Call]]
[[02 - Phase 2 - Repository]]
[[03 - Phase 3 - MVI Integration]]
[[04 - Phase 4 - Error Handling]]
[[05 - Phase 5 - Interceptors]]
