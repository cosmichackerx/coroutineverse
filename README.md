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
// Importing all coroutine features
import kotlinx.coroutines.*

fun main() = runBlocking {
    // Creating a coroutine lazily (not started yet)
    val job = launch(start = CoroutineStart.LAZY) {
        println("🔥 Coroutine ACTIVE")
        delay(1000) // suspends coroutine for 1 second
        println("🏁 Coroutine COMPLETED")
    }

    println("State: NEW ➤ Coroutine created lazily (not started yet)")

    job.start() // starting the coroutine manually
    println("State: ACTIVE ➤ Coroutine started running")

    delay(500) // simulate mid-execution suspension
    println("State: SUSPENDED ➤ delay() temporarily paused coroutine")

    job.join() // wait until coroutine finishes execution
    println("State: COMPLETED ➤ Coroutine work finished successfully")
}
```

### ❌ Cancelling a Coroutine

```kotlin
// Import all coroutine utilities
import kotlinx.coroutines.*

fun main() = runBlocking {
    // Launch a coroutine in the current scope
    val job = launch {
        try {
            // Repeat 5 times with delay to simulate ongoing work
            repeat(5) { i ->
                println("Working... $i")
                delay(500) // suspends coroutine for 0.5 seconds
            }
        } 
        // This block catches cancellation
        catch (e: CancellationException) {
            println("State: CANCELLED ➤ Coroutine ko cancel kar diya gaya")
        } 
        // Always runs (like finally block in try-catch)
        finally {
            println("🏁 Coroutine COMPLETED ➤ Cleanup done")
        }
    }

    delay(1000) // Let the coroutine run for a while
    job.cancel() // Cancel the coroutine
    job.join() // Wait until coroutine fully terminates
}
```

---
---

## 🧭 `runBlocking` & `launch` in Kotlin  

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 🔤 Definitions (runBlocking)

- **`runBlocking {}`**  
  Blocks the current thread until all coroutines inside it complete. Commonly used in `main()` or tests. Think of it as the **shift manager** who waits until all tasks are done before closing the kitchen.

- **`launch {}`**  
  Starts a new coroutine without blocking the current thread. Think of it as assigning a task to a **kitchen worker** who works independently.

---

## 🧠 Mnemonics & Analogies (English + Urdu)

| Concept        | Mnemonic (English)                                  | Analogy (Urdu)                                                                 |
|----------------|------------------------------------------------------|--------------------------------------------------------------------------------|
| `runBlocking`  | "Main manager waits till all tasks are done"        | **Kitchen ka manager har kaam mukammal hone tak rukta hai**                   |
| `launch`       | "Assign task to worker, let them work independently" | **Kaam kisi worker ko de diya, ab wo apne hisaab se karega**                 |
| `delay()`      | "Simulates time taken for a task"                   | **Kaam mein lagne wala waqt dikhane ke liye delay use hota hai**             |

---

## 💻 Code Examples

### 🍽️ Kitchen Shift Simulation with `runBlocking` and `launch`

```kotlin
// Import coroutine library
import kotlinx.coroutines.*

fun main() {
    println("Program Start: Opening the kitchen for the shift.")

    // runBlocking = Acts like the whole kitchen shift manager.
    // It blocks the main thread until all assigned coroutine tasks inside it are done.
    runBlocking {
        println("  [runBlocking]: Kitchen shift starts. (Thread: ${Thread.currentThread().name})")

        // Task 1: Washing dishes (takes ~1 second)
        launch {
            println("    [Launch 1]: Started washing dishes...")
            delay(1000L) // Simulate a long-running task
            println("    [Launch 1]: ...Dishes are clean.")
        }

        // Task 2: Prepping vegetables (takes ~0.5 seconds)
        launch {
            println("    [Launch 2]: Started prepping vegetables...")
            delay(500L) // Simulate a shorter task
            println("    [Launch 2]: ...Vegetables are prepped.")
        }

        // This part runs right after launching both tasks.
        println("  [runBlocking]: Main coroutine has assigned all tasks and is now waiting...")
        // runBlocking will pause here until both 'launch' coroutines finish.
    }

    // This line runs only after runBlocking (and all its coroutines) complete.
    println("Program End: All kitchen tasks are done. Shift over. Locking the door.")
}
```

---
---
