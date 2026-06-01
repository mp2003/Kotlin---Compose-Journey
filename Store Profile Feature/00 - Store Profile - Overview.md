###### Elevator Pitch
- Store Profile ek view-only feature hai jo ek real production module ke saare layers (domain, data, di, presentation) sabse simple flow me dikhata hai — load once, show once.

---

###### What
- My ==first feature== in the 1Pharmacy retailer app
- View-only screen -> koi edit/save nahi, bas data dikhana
- Real code: `features/android-retailer-store-profile-impl/`
- Package: `io.one.pharma.retailer.store.profile.impl`

---

###### Why this is a good first feature
- Simplest flow -> kam confusion, poora skeleton clear dikhe
- Har layer ka ek-ek file -> ek-ek note
- Same pattern baaki saare features bhi follow karte hain

---

###### The Flow (one line each)

```text
Drawer tap
   -> Routes.Graph pe navigate
   -> RootScreen (stateful)
   -> ViewModel.init -> UseCase -> Repository -> data source
   -> Result<StoreProfile>
   -> InitialState = Success
   -> Screen (pure) renders
```

---

###### Layers (kaise socho)
- domain -> "kya data, kya rules" (framework-free)
- data -> "data aata kahan se hai" (API/cache)
- mapper -> "ek shape ko doosri shape me badlo"
- di -> "kaun sa impl kis interface ko milega" (Hilt)
- presentation -> "screen pe kya dikhega aur user kya kar sakta hai"

---

###### Read in this order
1. [[01 - Domain Model]]
2. [[02 - Repository Contract]]
3. [[03 - UseCase Contract]]
4. [[04 - UseCase Impl]]
5. [[05 - Error Ids]]
6. [[06 - Repository Impl]]
7. [[07 - Mapper Transformer]]
8. [[08 - DI Module]]
9. [[09 - UiState]]
10. [[10 - Action]]
11. [[11 - Effect]]
12. [[12 - Navigation]]
13. [[13 - InitialState]]
14. [[14 - ViewModel]]
15. [[15 - RootScreen]]
16. [[16 - Pure Screen]]
17. [[17 - Routes]]
18. [[18 - Destinations]]
19. [[19 - NavGraph]]
20. [[20 - Build Gradle]]

---

###### Key Rule
- Ek feature = layers ka stack. Niche se upar likho: domain -> data -> di -> presentation.

---

###### Related
- [[00 - MVI - Overview]]
- [[00 - DI - Overview]]
- [[00 - Navigation - Overview]]
