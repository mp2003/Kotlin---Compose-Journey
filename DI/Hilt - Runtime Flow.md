---
up: "[[Hilt - DI Library]]"
---

### [[Hilt - Architecture Layers]]
```

PRESENTATION   → HiltTaskScreen → TaskViewModel  
DOMAIN         → TaskRepository (interface)  
DATA           → TaskRepositoryImpl  
DI             → RepositoryModule

```

---
## Dependency Direction

- Presentation depends on Domain  
- Data depends on Domain  
- Domain depends on nothing  
  
```

Presentation → Domain ← Data

```

- Domain is the center
- No external dependencies
- This makes it testable and stable

---

## Runtime Flow (How Hilt Works)

1. App starts  
   - `MyApp` (`@HiltAndroidApp`) creates the dependency graph  

2. Activity launches  
   - `@AndroidEntryPoint` attaches Hilt container  

3. Compose runs  
   - `hiltViewModel()` is called  

4. Hilt receives request  
   - "Give me TaskViewModel"  

5. Hilt checks ViewModel  
   - Finds `@HiltViewModel`  
   - Sees `@Inject constructor(repository: TaskRepository)`  

6. Hilt resolves dependency  
   - Needs `TaskRepository`  
   - Looks into `RepositoryModule`  
   - Finds `@Binds → TaskRepositoryImpl`  

7. Hilt creates objects  
   - Builds `TaskRepositoryImpl()`  
   - Injects into `TaskViewModel`  

8. UI gets data  
   - ViewModel calls repo  
   - Repo returns data  
   - Compose renders UI  

---

## Why This Is Powerful

- Easy testing  
  - Replace with `FakeTaskRepository`  
  - No Hilt needed in tests  

- Easy replacement  
  - Swap implementation (API → DB)  
  - Only change module  

- No manual wiring  
  - No `TaskRepositoryImpl()` anywhere  
  - Hilt handles everything  

---

## Mental Model (Core of Hilt)

Hilt always answers 3 questions:

1. Who can build this?  
   → `@Inject constructor` or `@Provides`  

2. Which implementation to use?  
   → `@Binds`  

3. How long should it live?  
   → `@InstallIn` (scope)  

---

## Key Understanding

- You declare dependencies  
- Hilt builds and connects them  
- Classes stay clean and focused  

---

## One-Line Summary

- Hilt manages object creation, wiring, and lifecycle automatically.

[[DI - Overview]]