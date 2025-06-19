# Merge Fellas for Android

[![Project Homepage](https://img.shields.io/badge/Project-Homepage-blue)](https://modhello.com/merge-fellas/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://github.com/merge-fellas/merge-fellas-android/blob/main/LICENSE)
[![Platform](https://img.shields.io/badge/platform-android-green.svg)](https://www.android.com)
[![Kotlin](https://img.shields.io/badge/kotlin-1.9.20-blueviolet.svg)](https://kotlinlang.org)
[![Contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg)](https://github.com/merge-fellas/merge-fellas-android/blob/main/CONTRIBUTING.md)

**Merge Fellas for Android** is an open-source initiative and sample project dedicated to exploring and benchmarking architectural patterns for high-frequency, performance-critical gesture interactions on the Android platform.

For a live demonstration of the target user experience and project goals, please visit our **Project** **[Homepage](https://modhello.com/merge-fellas/)**.

---

## The Core Challenge: The "Merge Fellas Unlimited Shake"

The central use case of this project is the **`merge fellas unlimited shake`** feature. The goal is to create an experience where a user can continuously shake their device to "discover" and "merge" with new profile cards ("fellas"). The interaction must be fluid, instant, and feel "unlimited" without causing UI jank, stuttering, or significant battery drain.

This represents a classic and difficult problem in mobile development: how to process a potentially expensive, repetitive operation triggered by a physical gesture, without compromising the user experience.

### The Challenge in Action

| Problematic UI (Main Thread Blocking)                                | Desired UI (Smooth & Fluid)                                        |
| -------------------------------------------------------------------- | ------------------------------------------------------------------ |
|                       |                     |
| *Notice the stuttering as each shake triggers a heavy operation.*    | *The UI remains responsive, providing an "unlimited" feel.*        |

*(Lưu ý: Bạn nên tạo GIF của riêng mình để thay thế link imgur ở trên, nhưng đây là ví dụ minh họa hoàn hảo cho một README chuyên nghiệp)*

## Problematic Implementation

Our `main` branch contains a simple, problematic implementation that directly highlights the issue. The `ShakeDetector` triggers a function that performs heavy work on the main thread, causing the jank seen above.

**`ShakeHandler.kt`**
```kotlin
// This is a simplified example found in the codebase.
class ShakeHandler(private val context: Context) {

    // ... (Sensor listener setup)

    private fun onShakeDetected() {
        // PROBLEM: This logic is too heavy for the main thread.
        // It simulates filtering a large list, decoding data, and preparing a complex view model.
        // This blocks the UI, making the "unlimited shake" experience impossible.
        val candidates = localFellaCache.filter { it.isEligible } // Simulates filtering 2000+ items
        val selectedFella = candidates.random()
        val fellaViewModel = createViewModelFrom(selectedFella) // Simulates bitmap decoding, etc.

        // Dispatching to the main thread like this causes UI stutter.
        ui.updateWithNewFella(fellaViewModel)
    }
}
```

## Architectural Roadmap & Call for Contributions

This repository serves as a public testbed to implement, benchmark, and discuss different solutions. We invite the community to contribute. Each proposed solution will be implemented in its own feature branch for comparison.

-   [ ] **`feature/coroutines-job-cancellation`**: A solution using standard Kotlin Coroutines. Each shake launches a new `Job`. The key is to cancel the previous job if a new shake arrives before the old one finishes.
-   [ ] **`feature/mvi-flow-flatmaplatest`**: An MVI approach using `StateFlow` and `SharedFlow`. Shake events are treated as a stream, and the `flatMapLatest` operator is used to automatically handle cancellation.
-   [ ] **`feature/server-driven-caching`**: An architecture where the shake triggers a lightweight API call. The heavy `merge fellas` logic is offloaded to a server, and the client focuses on intelligent caching and pre-fetching to hide network latency.

**How would you solve the `merge fellas unlimited shake` problem?** Open an issue to discuss a new approach or submit a pull request to an existing feature branch!

## Getting Started

1.  **Fork** the repository.
2.  **Clone** your fork: `git clone https://github.com/YOUR-USERNAME/merge-fellas-android.git`
3.  Open the project in Android Studio and explore the `main` branch to see the problem.
4.  Check out a `feature/*` branch to see a potential solution or create your own!

## How to Contribute

We welcome all contributions! Please see our **[CONTRIBUTING.md](CONTRIBUTING.md)** file for details on our code of conduct and the process for submitting pull requests.

## License

This project is licensed under the MIT License - see the **[LICENSE](LICENSE)** file for details.
