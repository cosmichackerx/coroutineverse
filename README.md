# coroutineverse
**Complete Kotlin Coroutines Guide** covering **every topic** from basics to advanced!  
**Definitions** | **Mnemonics (English+Urdu)** | **Code Examples** | **Real-world use cases**

---
## 🧵 Kotlin Coroutines  
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 🔤 Definitions

- **Coroutine**: A lightweight thread-like construct used for asynchronous programming in Kotlin.
- **Lifecycle States**:
  - `NEW`: Coroutine is created but not started (`lazy` mode).
  - `ACTIVE`: Coroutine has started and is running.
  - `SUSPENDED`: Coroutine is paused temporarily (e.g., via `delay()`).
  - `CANCELLED`: Coroutine was stopped before completion.
  - `COMPLETED`: Coroutine finished its task successfully.

---

## 🧠 Mnemonics & Analogies (English + Urdu)

| State        | Mnemonic (English)                              | Analogy (Urdu)                                                                 |
|--------------|--------------------------------------------------|--------------------------------------------------------------------------------|
| `NEW`        | "Newborn but asleep"                            | **Naya bacha paida hua hai, lekin abhi so raha hai** – coroutine abhi start nahi hui |
| `ACTIVE`     | "Engine started, wheels turning"                | **Gaari chal padi hai** – coroutine ka kaam shuru ho gaya hai 🔥              |
| `SUSPENDED`  | "Paused like a movie break"                     | **Movie pause kar di hai** – `delay()` ne temporary roka hai                  |
| `CANCELLED`  | "Emergency brake pulled"                        | **Brake maar di gayi** – kaam adhura chhod diya gaya                          |
| `COMPLETED`  | "Reached destination"                           | **Manzil mil gayi** – coroutine ne apna kaam mukammal kar liya 🏁              |

---

## 💻 Code Examples

### ✅ Basic Coroutine with Lifecycle Logging

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    val job = launch(start = CoroutineStart.LAZY) {
        println("🔥 Coroutine ACTIVE")
        delay(1000)
        println("🏁 Coroutine COMPLETED")
    }

    println("State: NEW ➤ Coroutine created lazily")
    job.start()
    println("State: ACTIVE ➤ Coroutine start ho gayi hai")

    delay(500)
    println("State: SUSPENDED ➤ delay() ne coroutine ko temporarily roka hai")

    job.join()
    println("State: COMPLETED ➤ coroutine ka kaam khatam ho gaya")
}
```

### ❌ Cancelling a Coroutine

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    val job = launch {
        try {
            repeat(5) { i ->
                println("Working... $i")
                delay(500)
            }
        } catch (e: CancellationException) {
            println("State: CANCELLED ➤ Coroutine ko cancel kar diya gaya")
        } finally {
            println("🏁 Coroutine COMPLETED ➤ Cleanup done")
        }
    }

    delay(1000)
    job.cancel()
    job.join()
}
```

---
---
