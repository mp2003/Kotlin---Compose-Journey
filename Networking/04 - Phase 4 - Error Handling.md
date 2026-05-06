---
up: "[[00 - Networking - Overview]]"
---

###### Elevator Pitch
- Phase 4 introduces `ApiResult` so success and failure are handled in one clear contract instead of raw exceptions.

---

###### Definition
- `ApiResult` is a sealed wrapper that represents either a successful result or a failure message.

---

###### Real-World Analogy
- Success -> the package arrived
- Error -> the package did not arrive and the reason is attached

---

###### What
- `ApiResult.Success` -> carries parsed data
- `ApiResult.Error` -> carries an error message
- `PostRepository` -> returns `ApiResult`
- `PostViewModel` -> maps `ApiResult` to `PostUiState`

---

###### Why
- Keeps error handling in one shape
- Prevents the ViewModel from guessing whether a call worked
- Makes UI state mapping cleaner

---

###### Core Concepts
- Sealed class -> only the known result types exist
- Result wrapper -> one type for success and failure
- Mapping -> converting repository output into screen state

---

###### How it Works
- Repository tries the network call
- Success returns `ApiResult.Success(posts)`
- Failure returns `ApiResult.Error(message)`
- ViewModel turns that into Loading, Success, or Error UI state

---

###### Syntax
- `sealed class ApiResult<out T>` -> shared wrapper for data results
- `data class Success<T>(val data: T)` -> success path with data
- `data class Error(val message: String)` -> failure path with message

```kotlin
// Result wrapper: the repository returns one of these two paths
sealed class ApiResult<out T> {
    // Success path that carries the data
    data class Success<T>(val data: T) : ApiResult<T>()

    // Failure path that carries a message
    data class Error(val message: String) : ApiResult<Nothing>()
}
```

---

###### Common Mistakes
- BAD: returning raw data and exceptions from the same function
- BAD: putting UI text like `Error:` inside the repository
- BAD: ignoring the error message and only logging it

---

###### Memory Hook
- ApiResult means "one return type, two outcomes".

---

###### Key Rule
- Let the repository report the outcome, and let the ViewModel decide the UI state.

---

###### Related
- [[00 - Networking - Overview]]
- [[03 - Phase 3 - MVI Integration]]
- [[05 - Phase 5 - Interceptors]]

