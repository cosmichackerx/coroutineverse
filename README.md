# coroutineverse
**Complete Kotlin Coroutines Guide** covering **every topic** from basics to advanced!  
**Definitions** | **Mnemonics (English+Urdu)** | **Code Examples** | **Real-world use cases**

---

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## 🧵 Kotlin Coroutines  

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

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## 🧭 `runBlocking` & `launch` in Kotlin  

## 🔤 Definitions (runBlocking & launch)

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

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## 🧑‍💼 `launch` with `job.join()` in `runBlocking`  

## 🔤 Definitions (launch)

- **`launch {}`**  
  Starts a coroutine in a fire-and-forget style. It runs independently and doesn’t block the current coroutine.

- **`job.join()`**  
  Suspends the current coroutine until the launched coroutine (`job`) finishes. Think of it as the manager waiting for the worker to complete the task.

- **`runBlocking {}`**  
  Blocks the current thread until all child coroutines inside it finish. Commonly used in `main()` functions.

---

## 🧠 Mnemonics & Analogies (English + Urdu)

| Concept        | Mnemonic (English)                                      | Analogy (Urdu)                                                                 |
|----------------|----------------------------------------------------------|--------------------------------------------------------------------------------|
| `launch`       | "Send a worker to do a task, don’t wait"                | **Kaam kisi worker ko de diya, manager apna kaam karta raha**                |
| `job.join()`   | "Manager waits for the worker to finish"                | **Manager ruk gaya jab tak worker ka kaam mukammal nahi ho gaya**            |
| `runBlocking`  | "Main thread waits for all child tasks"                 | **Main thread tab tak rukta hai jab tak saare kaam mukammal na ho jayein**   |

---

## 💻 Code Examples

### 🧑‍🍳 Manager & Worker Analogy with `launch` and `job.join()`

```kotlin
// Import coroutine utilities
import kotlinx.coroutines.*

fun main() = runBlocking { // 'runBlocking' ensures main waits for all child coroutines to finish
    
    println("Manager (main coroutine): I need to send this report, but I have my own work to do.")
    println("Manager (main coroutine): Thread: ${Thread.currentThread().name}\n")

    // L = Launch a Worker
    // 'launch' = fire-and-forget coroutine
    // It starts a new coroutine but doesn’t block the current one.
    val job = launch { 
        println("  Worker (launch): Got the task! Starting to send the report...")
        println("  Worker (launch): Thread: ${Thread.currentThread().name}")
        delay(1000L) // Simulate a long-running background task
        println("  Worker (launch): ...Report has been sent!")
    }

    // The manager continues immediately without waiting for the worker to finish
    println("Manager (main coroutine): Great, I've 'launched' that worker. Now I'll do my own paperwork.")
    delay(300L) // Simulate a shorter task for the manager
    println("Manager (main coroutine): ...Finished my paperwork.\n")

    // Even if we don’t call job.join(),
    // 'runBlocking' automatically waits for all its children before exiting.
    println("Manager (main coroutine): Now I'll just wait for that worker to be done...")
    job.join() // Explicitly wait for the launched worker to complete
    println("Manager (main coroutine): OK, the worker is done. Time to go home.")
}
```

---
---

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## ⚡ `async` & `await` in Kotlin  

## 🔤 Definitions (async & await)

- **`async {}`**  
  Starts a coroutine that returns a result via a `Deferred<T>`. Multiple `async` blocks can run concurrently.

- **`await()`**  
  Suspends the current coroutine until the result of the `Deferred` is ready. It’s non-blocking and efficient.

- **Concurrency vs Parallelism**  
  `async` enables **concurrent** execution — tasks start together and run independently, saving time.

---

## 🧠 Mnemonics & Analogies (English + Urdu)

| Concept        | Mnemonic (English)                                      | Analogy (Urdu)                                                                 |
|----------------|----------------------------------------------------------|--------------------------------------------------------------------------------|
| `async`        | "Send multiple requests at once"                        | **Ek saath do kaam ke liye farmaish bhej di gayi**                            |
| `await()`      | "Wait for each answer when it’s ready"                  | **Har jawab ka intezar jab wo tayar ho jaye**                                 |
| `Deferred`     | "Promise of a future result"                            | **Wada kiya gaya hai ke result baad mein milega**                             |
| `measureTimeMillis` | "Track total time taken for tasks"                | **Pura waqt naapne ke liye stopwatch lagaya gaya hai**                        |

---

## 💻 Code Examples

### 👤 Building a User Profile Concurrently with `async` & `await`

```kotlin
// Import coroutine features and utility for measuring execution time
import kotlinx.coroutines.*
import kotlin.system.measureTimeMillis

// Simulates a slow network call to fetch a user's name
suspend fun fetchUsername(): String {
    println("  [Async 1]: Asking for username... (will take 1000ms)")
    delay(1000L) // Simulate network delay
    return "KotlinHero" // Return fetched username
}

// Simulates a slow network call to fetch a profile picture
suspend fun fetchProfilePictureUrl(): String {
    println("  [Async 2]: Asking for profile picture... (will take 1500ms)")
    delay(1500L) // Simulate network delay
    return "https://example.com/pic.png" // Return fetched picture URL
}

fun main() = runBlocking {
    println("Main: Need to build the user profile. I will ask for data.\n")

    val totalTime = measureTimeMillis {
        // 'async' = start concurrent tasks that return results
        // Both requests start *at the same time* (not one after another)
        val deferredUsername: Deferred<String> = async { fetchUsername() }
        val deferredPictureUrl: Deferred<String> = async { fetchProfilePictureUrl() }

        println("Main: I have 'asked' for both username and picture. Now I wait for the answers.")
        println("Main: ...waiting...\n")

        // 'await()' suspends until the result is ready (non-blocking)
        val username = deferredUsername.await()
        println("Main: Got the username! It is '$username'")

        val pictureUrl = deferredPictureUrl.await()
        println("Main: Got the picture URL! It is '$pictureUrl'\n")

        // Use both results together
        println("Main: ✅ Success! Displaying profile for '$username' with image at '$pictureUrl'")
    }

    // Measure total time — should be close to the longest single delay (≈1500ms)
    println("\nMain: Total time taken: $totalTime ms")
}
```

---
---

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## ⏳ `delay()` vs `Thread.sleep()` in Coroutines  

## 🔤 Definitions ( delay() vs Thread.sleep() )

- **`delay(timeMillis)`**  
  Suspends the coroutine without blocking the thread. Other coroutines can run during this time. Ideal for non-blocking asynchronous tasks.

- **`Thread.sleep(timeMillis)`**  
  Blocks the entire thread. No other coroutine on that thread can run until sleep ends. Not recommended inside coroutines.

- **Concurrency Impact**  
  `delay()` enables concurrent execution, while `Thread.sleep()` forces sequential blocking.

---

## 🧠 Mnemonics & Analogies (English + Urdu)

| Concept           | Mnemonic (English)                                      | Analogy (Urdu)                                                                 |
|-------------------|----------------------------------------------------------|--------------------------------------------------------------------------------|
| `delay()`         | "Pause the coroutine, but let others work"              | **Chef ne paani ubalne diya, aur saath saath sabzi kaat li**                 |
| `Thread.sleep()`  | "Freeze the thread, no one else can work"               | **Chef sirf pateeli ko ghoorta raha, sabzi kaatna bhi ruk gaya**             |
| `runBlocking`     | "Kitchen manager waits till all tasks finish"           | **Manager tab tak rukta hai jab tak sab kaam mukammal na ho jayein**         |

---

## 💻 Code Examples

### 🍳 Comparing Non-Blocking vs Blocking in the Kitchen

```kotlin
// Import coroutine utilities and time measurement function
import kotlinx.coroutines.*
import kotlin.system.measureTimeMillis

fun main() {
    println("--- Example 1: Using delay() (Non-Blocking) ---")
    println("Analogy: The Chef puts water on to boil, then chops veggies while waiting.\n")

    // Measure how long the non-blocking example takes
    val nonBlockingTime = measureTimeMillis {
        runBlocking {
            // Coroutine 1: Boil water
            launch {
                println("  [Boil Water]: Starting... (will 'delay' for 1000ms)")
                // D = Don't Block Delay
                // 'delay()' suspends this coroutine but frees the thread 
                // so other coroutines can run meanwhile.
                delay(1000L)
                println("  [Boil Water]: ...Water is boiling!")
            }

            // Coroutine 2: Chop vegetables
            launch {
                println("    [Chop Veggies]: Starting to chop veggies...")
                delay(250L) // First chopping phase
                println("    [Chop Veggies]: ...still chopping...")
                delay(250L) // Second chopping phase
                println("    [Chop Veggies]: ...Veggies are done.")
            }
        }
    }
    println("Total time for Example 1: $nonBlockingTime ms (✅ Both tasks ran concurrently)\n")


    println("--- Example 2: Using Thread.sleep() (Blocking) ---")
    println("Analogy: The Chef *stares* at the pot, blocking all other work.\n")

    // Measure how long the blocking example takes
    val blockingTime = measureTimeMillis {
        runBlocking {
            // Coroutine 1: Boil water (but using Thread.sleep)
            launch {
                println("  [Stare at Pot]: Starting... (will 'Thread.sleep' for 1000ms)")
                // This BLOCKS the entire thread.
                // While sleeping, no other coroutine on this thread can execute.
                Thread.sleep(1000L)
                println("  [Stare at Pot]: ...Water is boiling!")
            }

            // Coroutine 2: Chop vegetables
            launch {
                println("    [Chop Veggies]: Starting to chop veggies...")
                delay(250L)
                println("    [Chop Veggies]: ...still chopping...")
                delay(250L)
                println("    [Chop Veggies]: ...Veggies are done.")
            }
        }
    }
    println("Total time for Example 2: $blockingTime ms (❌ Tasks ran one after the other)")
}
```

---
---

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## 🤝 `join()` in Kotlin Coroutines  

## 🔤 Definitions ( join() )

- **`join()`**  
  Suspends the current coroutine until the specified coroutine (`Job`) completes. It’s used when you want to wait for a concurrent task to finish before proceeding.

- **`launch {}`**  
  Starts a coroutine that runs concurrently. It returns a `Job` which can be joined or cancelled.

- **Use Case**  
  Perfect for scenarios where one coroutine depends on another’s completion before continuing.

---

## 🧠 Mnemonics & Analogies (English + Urdu)

| Concept     | Mnemonic (English)                                  | Analogy (Urdu)                                                                 |
|-------------|------------------------------------------------------|--------------------------------------------------------------------------------|
| `launch`    | "Send a friend to do a task"                        | **Dost ko kaam de diya, wo apne hisaab se karega**                            |
| `join()`    | "Wait for your friend to finish before starting together" | **Dost ka kaam mukammal hone ka intezar, phir mil kar kaam shuru karna**     |
| `delay()`   | "Simulate time taken for a task"                    | **Kaam mein lagne wala waqt dikhane ke liye delay use hota hai**             |

---

## 💻 Code Examples

### 🎲 Game Night Analogy with `launch` and `join()`

```kotlin
// Import coroutine utilities
import kotlinx.coroutines.*

fun main() = runBlocking {
    println("You (Main): Let's get ready for game night!")

    // Start your friend's task in a separate coroutine
    // 'launch' starts a new coroutine that runs concurrently
    val friendJob = launch {
        println("  Friend (Job): Starting to set up the complex board game...")
        delay(1500L) // Simulate a long setup task
        println("  Friend (Job): ...Game is set up!")
    }

    // Meanwhile, you do your own shorter task
    println("You (Main): I'll go get the snacks. (My task takes 500ms)")
    delay(500L) // Simulate your snack-fetching time
    println("You (Main): ...Snacks are ready. Is my friend done?")
    println("You (Main): ---> Now I will 'join()' my friend and wait for them. <---\n")
    
    // J = Join when Done
    // 'join()' suspends the main coroutine until 'friendJob' completes
    friendJob.join() 

    // This runs only after the friend's coroutine finishes
    println("\nYou (Main): Great, my friend is done! Now we can start the game together.")
}
```

---
---

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 🛑 Coroutine Cancellation with `cancel()`  

## 🔤 Definitions ( cancel() )

- **`cancel()`**  
  Sends a cancellation signal to a coroutine. If the coroutine is suspended (e.g., at `delay()`), it throws a `CancellationException`.

- **`try-catch-finally` in coroutines**  
  Used to handle cancellation gracefully. `catch` handles the cancellation exception, and `finally` ensures cleanup (e.g., closing files, releasing resources).

- **Suspension Point**  
  A place like `delay()` where the coroutine checks for cancellation and can be interrupted.

---

## 🧠 Mnemonics & Analogies (English + Urdu)

| Concept           | Mnemonic (English)                                      | Analogy (Urdu)                                                                 |
|-------------------|----------------------------------------------------------|--------------------------------------------------------------------------------|
| `cancel()`        | "Cut off the task midway"                               | **Manager ne kaam beech mein rok diya**                                       |
| `delay()`         | "Pause point where cancellation is checked"             | **Kaam rukne ki jagah jahan worker check karta hai ke kaam cancel hua ya nahi** |
| `finally` block   | "Always clean up after stopping"                        | **Kaam chhodne ke baad desk saaf karna zaroori hai**                          |

---

## 💻 Code Examples

### 📄 Report Compilation with Cancellation Logic

```kotlin
// Import coroutine utilities
import kotlinx.coroutines.*

fun main() = runBlocking {
    println("Manager (Main): I need this 100-section report compiled. Start working.")

    // Start the worker coroutine that will do the long task
    val workerJob = launch {
        try {
            // The worker will compile 100 sections
            repeat(100) { i ->
                println("  Worker (Job): Compiling report, section ${i + 1}...")
                
                // 'delay()' is a suspension point.
                // The coroutine checks for cancellation here — 
                // if cancelled, it throws a CancellationException.
                delay(500L)
            }
        } catch (e: CancellationException) {
            println("  Worker (Job): Ah! The manager told me to stop. ${e.message}")
        } finally {
            // This block always runs (even after cancellation)
            // Used for cleanup work like closing files or releasing resources.
            println("  Worker (Job): Stopping work and cleaning up my desk.")
        }
    }

    // Let the worker run for 1.3 seconds before interrupting
    println("\nManager (Main): (Letting worker run for 1.3 seconds...)\n")
    delay(1300L)

    // C = Cut Off Coroutine
    // Cancel the worker — sends a cancellation signal
    println("Manager (Main): That's enough. STOP THE TASK!")
    workerJob.cancel() // Cancel signal sent

    // Wait for the worker to finish its 'finally' block
    workerJob.join()

    println("Manager (Main): Good, the worker has stopped.")
}
```

---
---

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📊 Coroutine State Checks: `isActive`, `isCompleted`, `isCancelled`  

## 🔤 Definitions (isActive , isCompleted, isCancelled)

- **`isActive`**  
  Returns `true` if the coroutine is currently running and hasn’t completed or been cancelled.

- **`isCompleted`**  
  Returns `true` if the coroutine has finished successfully or with an exception.

- **`isCancelled`**  
  Returns `true` if the coroutine was cancelled before completion.

- **`yield()`**  
  Gives other coroutines a chance to run. Useful for cooperative multitasking.

---

## 🧠 Mnemonics & Analogies (English + Urdu)

| Property         | Mnemonic (English)                                      | Analogy (Urdu)                                                                 |
|------------------|----------------------------------------------------------|--------------------------------------------------------------------------------|
| `isActive`       | "Is the worker still busy?"                             | **Kya worker abhi bhi kaam mein masroof hai?**                                |
| `isCompleted`    | "Has the worker finished the task?"                     | **Kya worker ne kaam mukammal kar liya hai?**                                 |
| `isCancelled`    | "Was the task stopped midway?"                          | **Kya kaam beech mein rok diya gaya tha?**                                    |
| `yield()`        | "Let others take a turn"                                | **Thodi der ke liye doosre ko kaam karne do**                                |

---

## 💻 Code Examples

### 🧪 Monitoring Coroutine Lifecycle States

```kotlin
import kotlinx.coroutines.*  // Import coroutine library

fun main() = runBlocking {
    println("Manager: I'm launching a worker for a 2-second task.")

    // Launch a coroutine as a Job
    val workerJob = launch {
        println("  Worker (Job): Starting my task...")
        delay(2000L) // Simulate a 2-second task
        println("  Worker (Job): ...Task complete.")
    }

    yield() // Let coroutine start before checking status

    // ✅ Check 1 — right after start: should be active
    println("Manager (at 0ms): Is the worker active? ${workerJob.isActive}")

    println("Manager: I'll do other work for 1 second...")
    delay(1000L) // Simulate manager’s work

    // ✅ Check 2 — after 1 second: still active
    println("Manager (at 1000ms): Still active? ${workerJob.isActive}")

    // Wait for the coroutine to complete
    println("Manager: Waiting for the worker to finish...")
    if (workerJob.isActive) println("Job is still active before join()")

    workerJob.join() // Suspend until worker finishes

    // ✅ Check 3 — after join(): check job states
    when {
        workerJob.isActive -> println("Job is unexpectedly still active.")
        workerJob.isCompleted -> println("Job has completed successfully.")
        workerJob.isCancelled -> println("Job was cancelled.")
        else -> println("Job state unknown.")
    }

    // Final confirmation
    println("Manager (at 2000ms): Final IsActive = ${workerJob.isActive}")
}
```

---
---

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 🧭 Coroutine Dispatchers in Kotlin  

## 🔤 Definitions (Coroutine Dispatchers)

- **`Dispatchers.IO`**  
  Optimized for I/O-bound tasks like reading files, accessing databases, or making network calls.

- **`Dispatchers.Default`**  
  Designed for CPU-intensive tasks like sorting, calculations, or data processing.

- **`Dispatchers.Unconfined`**  
  Starts in the current thread but may resume on a different thread after suspension. Useful for lightweight tasks or debugging.

- **`runBlocking`**  
  Blocks the current thread until all child coroutines complete. Often used in `main()` functions.

---

## 🧠 Mnemonics & Analogies (English + Urdu)

| Dispatcher         | Mnemonic (English)                                      | Analogy (Urdu)                                                                 |
|--------------------|----------------------------------------------------------|--------------------------------------------------------------------------------|
| `Dispatchers.IO`   | "Warehouse for slow I/O tasks"                          | **Warehouse jahan file read/write aur network ka kaam hota hai**              |
| `Dispatchers.Default` | "Office for fast brain work"                        | **Office jahan calculations aur data processing hoti hai**                    |
| `Dispatchers.Unconfined` | "Freelancer who works anywhere"                 | **Freelancer jo pehle ek jagah kaam shuru karta hai, phir kahin aur chala jata hai** |
| `runBlocking`      | "Main desk that waits for all workers"                 | **Main desk jahan manager sab kaam mukammal hone ka intezar karta hai**       |

---

## 💻 Code Examples

### 🧵 Dispatching Tasks to Different Workstations

```kotlin
import kotlinx.coroutines.*  // Import coroutine support

fun main() = runBlocking {
    // 🏢 This 'runBlocking' coroutine is our "Main Desk"
    // It runs on the main thread and waits for all child coroutines to complete.
    println("Main Desk ('runBlocking'): Running on thread ➤ ${Thread.currentThread().name}\n")

    // ───────────────────────────────
    // 1️⃣ Dispatchers.IO  → "Warehouse" for I/O heavy tasks (e.g., reading files, network calls)
    // ───────────────────────────────
    launch(Dispatchers.IO) {
        println("  [IO Workstation]: Task started ➤ Thread: ${Thread.currentThread().name}")
        delay(500L) // Simulate I/O operation (suspends coroutine)
        println("  [IO Workstation]: ...Task finished ✅")
    }

    // ───────────────────────────────
    // 2️⃣ Dispatchers.Default  → "Office" for CPU-heavy tasks (e.g., computations)
    // ───────────────────────────────
    launch(Dispatchers.Default) {
        println("  [Default Office]: Task started ➤ Thread: ${Thread.currentThread().name}")
        var count = 0
        repeat(100_000) { count += 1 } // Simulate CPU work
        println("  [Default Office]: ...Task finished ✅ (Counted to $count)")
    }

    // ───────────────────────────────
    // 3️⃣ Dispatchers.Unconfined  → "Freelancer"
    // Starts in the current thread but may resume on a different one after suspension.
    // ───────────────────────────────
    launch(Dispatchers.Unconfined) {
        println("  [Unconfined Task]: Started ➤ Thread: ${Thread.currentThread().name}")
        delay(100L) // Suspension point (context may change)
        println("  [Unconfined Task]: Resumed ➤ Thread: ${Thread.currentThread().name}")
    }

    // ───────────────────────────────
    // The main desk will wait for all launched tasks to finish before ending.
    // ───────────────────────────────
    println("\nMain Desk ('runBlocking'): All tasks dispatched. Waiting for them to complete...")
}
```

---
---
