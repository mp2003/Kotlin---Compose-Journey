---
up: "[[03 - Hilt - DI Library]]"
---
##### **1. Presentation Layer**

###### **What it contains**

- Compose UI (Screens)
- ViewModel

###### **Responsibility**

- Handle UI + state
- Call business logic
- Do NOT know how data is fetched

---

###### **Example**

```kotlin
// PRESENTATION LAYER — ViewModel.  
//  
// @HiltViewModel marks this VM so Hilt knows it must build it (not Android).  
// @Inject constructor(...) tells Hilt: "to build me, you need to resolve these params."  
//  
// Notice: the VM depends on the INTERFACE `TaskRepository`, not on `TaskRepositoryImpl`.  
// That's the whole point of DI — this class doesn't know or care which implementation  
// it gets. Hilt looks at RepositoryModule and supplies TaskRepositoryImpl.
  
@HiltViewModel  
class TaskViewModel @Inject constructor(  
    private val repository: TaskRepository  
) : ViewModel() {  
  
    fun getTasks(): List<String> {  
        return repository.getTasks()  
    }  
}  
  
// `hiltViewModel()` is the bridge from Compose → Hilt.  
// It asks the nearest @AndroidEntryPoint Activity (MainActivity) for a TaskViewModel.  
// The Activity holds Hilt's component manager → that gives Hilt the ViewModelComponent  
// → Hilt builds the VM and resolves its `repository` dependency from the graph.


```

###### **Key Points**

- Depends on **Domain layer**
- Uses **interfaces only**
- Never uses implementation (`TaskRepositoryImpl`) directly

---

#### **2. Domain Layer**
###### **What it contains**

- Interfaces (contracts)
- Business rules (optional)

###### **Example**
```kotlin 
package com.milind.composeplayground.domain  
  
// DOMAIN LAYER — pure Kotlin contract. No Android, no Hilt, no implementation.  
// The ViewModel depends on THIS interface, not on a concrete class.  
// Why: lets you swap the implementation (real API, fake for tests, cached, etc.)  
// without touching the ViewModel. Classic Dependency Inversion.  

interface TaskRepository {  
    fun getTasks(): List<String>  
}
```

###### **Responsibility**

- Define **what needs to be done**
- No Android, no Retrofit, no DB

###### **Key Points**

- Pure Kotlin
- No dependencies
- Most stable layer

---

#### **3. Data Layer**

###### **What it contains**

- Implementations of domain interfaces
- API / DB logic

###### Example

```kotlin
package com.milind.composeplayground.data  
  
import com.milind.composeplayground.domain.TaskRepository  
import javax.inject.Inject  
  
// DATA LAYER — concrete implementation of the domain contract.  
// In a real app this is where you'd call Retrofit / Room / DataStore.  
//  
// @Inject constructor() tells Hilt: "you are allowed to build me — and here's how."  
// Since the constructor takes no args, Hilt just calls `TaskRepositoryImpl()` whenever  
// something asks for one. If this class needed dependencies (e.g. an ApiService),  
// Hilt would resolve those recursively from the same graph.  

class TaskRepositoryImpl @Inject constructor() : TaskRepository {  
  
    override fun getTasks(): List<String> {  
        return listOf("Task 1", "Task 2", "Task 3")  
    }  
}
```

###### **Responsibility**

- Decide **how data is fetched**
- Connect to network / database

###### **Key Points**

- Depends on Domain
- Can change freely without affecting UI

---

## **4. DI Layer (Hilt Layer)**

###### **What it contains**

- Modules (`@Module`)
- Bindings (`@Binds`, `@Provides`)

###### Example 

```kotlin
package com.milind.composeplayground.di  
  
import com.milind.composeplayground.data.TaskRepositoryImpl  
import com.milind.composeplayground.domain.TaskRepository  
import dagger.Binds  
import dagger.Module  
import dagger.hilt.InstallIn  
import dagger.hilt.components.SingletonComponent  
  
// DI LAYER — wiring rules for Hilt's object graph.  
//  
// The ViewModel asks for `TaskRepository` (an interface). Hilt cannot construct  
// an interface directly — it needs to know WHICH implementation to provide.  
// This module is that answer.  
//  
// @Module          → "I contain DI rules."  
// @InstallIn(SingletonComponent::class)  
//                  → "Install these rules into the app-wide (singleton) graph,  
//                    so the binding lives as long as the Application."  
//                    Other scopes exist: ActivityComponent, ViewModelComponent, etc.  
@Module  
@InstallIn(SingletonComponent::class)  

abstract class RepositoryModule {  
  
    // @Binds is the lightweight way to say:  
    //   "When someone needs a TaskRepository, give them a TaskRepositoryImpl."    //    // The class is `abstract` and the method is `abstract` because @Binds doesn't    // run — Hilt reads it as metadata at compile time and generates the wiring code.    //    // Alternative: @Provides (a normal function that returns an instance). Use @Provides    // when you can't just hand over a constructor-injected class — e.g. building a    // Retrofit instance, or wrapping a third-party class you can't annotate.    @Binds  
    
    abstract fun bindTaskRepository(  
        impl: TaskRepositoryImpl  
    ): TaskRepository  
}
```

### **Responsibility**

- Tell Hilt:
    - Which implementation to use
    - How to create objects

### **Key Points**

- Connects everything together
- No business logic
  
  ---


#### Dependency Direction (Very Important)

```
Presentation → Domain ← Data
         ↑
         DI (wires everything)
```


##### **How to Think While Coding**

###### **When writing ViewModel**

- “I need TaskRepository”
- Not → “I will create TaskRepositoryImpl”

---

###### **When writing Repository**

- “I implement domain contract”

---

###### **When writing Module**

- “If someone asks for X → give Y”

---

###### **Real Example Flow**

1. UI asks for ViewModel
2. Hilt creates ViewModel
3. ViewModel needs Repository
4. Hilt finds binding
5. Provides implementation
6. Data flows back to UI

---

##### **Why this structure matters**

- You can change data source without touching UI
- You can test ViewModel easily
- You avoid tight coupling
- Code becomes scalable

---

# **One-line Summary**

- Presentation = uses
- Domain = defines
- Data = implements
- DI = connects

---

[[03 - Hilt - DI Library]]