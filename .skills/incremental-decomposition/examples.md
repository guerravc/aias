# Incremental Decomposition — Examples

Before/after examples demonstrating good and bad increment decomposition.

---

## Example 1: Horizontal Layer Split (Anti-Pattern)

**Requirement:** Add a candidate search feature with API integration, ViewModel, and UI.

**Bad — Horizontal layers:**

```
Increment 1: "Create data models"
  - Create CandidateSearchRequest model
  - Create CandidateSearchResponse model
  - Create CandidateSearchResult model

Increment 2: "Create service layer"
  - Create CandidateSearchService protocol
  - Create CandidateSearchServiceImpl
  - Add DI registration

Increment 3: "Create ViewModel"
  - Create CandidateSearchViewModel
  - Add search logic
  - Add state management

Increment 4: "Create UI"
  - Create CandidateSearchView
  - Wire up ViewModel
  - Add navigation
```

**Why it's bad:** Increment 1 delivers models nobody uses. Increment 2 delivers a service nobody calls. Only after Increment 4 does the feature work. If the developer stops after Increment 2, the codebase has dead code.

**Good — Vertical slices:**

```
Increment 1: "Add candidate search with basic results"
  Goal: User can type a keyword, see matching candidates in a list; verified by
  manual search returning results and SwiftUI preview rendering the list.
  - Create CandidateSearchService (model + protocol + implementation)
  - Create CandidateSearchViewModel with search logic
  - Create CandidateSearchView with text field and result list
  - Register in DI container and add navigation entry

Increment 2: "Add empty state and error handling"
  Goal: Search shows an empty state when no results match and an error state
  on network failure; verified by triggering both states manually.
  - Add empty state view to CandidateSearchView
  - Add error state handling in ViewModel
  - Add error UI (Alert or inline message)

Increment 3: "Add search debounce and loading indicator"
  Goal: Search waits 300ms after typing stops before calling API, with a
  loading spinner during the request; verified by observing debounce behavior
  and spinner in SwiftUI preview.
  - Add debounce logic to ViewModel
  - Add loading state and spinner to CandidateSearchView
```

**Why it's better:** Each increment delivers a working, demoable feature. Stopping after Increment 1 gives a functional (if basic) search.

---

## Example 2: Setup Increment (Anti-Pattern)

**Requirement:** Integrate analytics tracking into the app.

**Bad — Setup first:**

```
Increment 1: "Set up analytics infrastructure"
  - Add RDSAnalytics SPM dependency
  - Create AnalyticsService protocol
  - Create AnalyticsServiceImpl
  - Register in DI container
  - Configure API key in AppDelegate

Increment 2: "Track screen views"
  - Add screen view tracking to 5 main screens

Increment 3: "Track user actions"
  - Add action tracking to search, filter, and sort interactions
```

**Why it's bad:** Increment 1 adds a fully configured analytics system that tracks nothing. It builds, but there is no way to verify it works until Increment 2.

**Good — Infrastructure + first usage together:**

```
Increment 1: "Add analytics with screen view tracking for Home"
  Goal: HomeScreen tracks a screen_view event on appear; verified by
  checking analytics dashboard shows events from Home.
  - Add RDSAnalytics SPM dependency
  - Create AnalyticsService protocol + implementation
  - Register in DI, configure API key
  - Add screen view tracking to HomeScreen

Increment 2: "Extend screen view tracking to remaining screens"
  Goal: All 4 remaining main screens track screen_view; verified by
  navigating to each and confirming events in analytics dashboard.
  - Add tracking to CandidateList, PositionDetail, Settings, Profile

Increment 3: "Add user action tracking"
  Goal: Search, filter, and sort interactions generate action events;
  verified by performing each action and confirming events.
  - Add action tracking calls to search, filter, and sort handlers
```

**Why it's better:** Increment 1 is immediately verifiable — you can check the analytics dashboard. The infrastructure is present because it's needed, not as a standalone deliverable.

---

## Example 3: Multi-Goal Increment (Too Big)

**Requirement:** Refactor authentication flow and add biometric login.

**Bad — Two goals in one:**

```
Increment 1: "Refactor auth and add biometrics"
  - Refactor AuthService to use async/await instead of closures
  - Add BiometricAuthProvider
  - Update LoginViewModel to support both flows
  - Update LoginView with biometric option
  - Update DI registrations
  - Add unit tests for both flows
```

**Why it's bad:** The increment goal needs "and" — that's a signal it's two increments. If the refactor breaks something, it's mixed with new biometric code, making debugging harder.

**Good — Separated concerns:**

```
Increment 1: "Refactor AuthService to async/await"
  Goal: AuthService uses async/await internally with no behavior change;
  verified by all existing auth tests passing.
  - Refactor AuthService methods from closures to async/await
  - Update LoginViewModel to use new async API
  - Verify existing tests pass (refactor-safe)

Increment 2: "Add biometric login option"
  Goal: User can authenticate via Face ID/Touch ID from the login screen;
  verified by enabling biometrics in Settings, then logging in with Face ID.
  - Create BiometricAuthProvider
  - Add biometric option to LoginView
  - Integrate with LoginViewModel
  - Add unit tests for biometric flow
```

**Why it's better:** Increment 1 is a pure refactor (no behavior change, verified by existing tests). Increment 2 is a pure feature addition. Each has one clear goal.

---

## Example 4: Estimation Alignment (for Charter)

**Bad estimation:**

| Increment | Complexity | Size | Confidence | Rationale |
|---|---|---|---|---|
| Everything | High | XL | Low | "It's a big feature" |

**Good estimation:**

| Increment | Complexity | Size | Confidence | Rationale |
|---|---|---|---|---|
| Add candidate search with basic results | Med | M | High | Standard API + ViewModel + UI; well-understood pattern |
| Add empty state and error handling | Low | S | High | Pure UI states, no new dependencies |
| Add search debounce and loading indicator | Med | S | Med | Debounce timing needs tuning; may require Combine/async adjustments |

**Why it's better:** Each increment is estimated independently. Complexity and confidence are calibrated to the specific increment, not the whole feature. Low confidence on Increment 3 flags that it might need adjustment during execution.

---

## Example 5: Improvement Margin

**Bad — Optional items mixed into core increments:**

```
Increment 2: "Add empty state, error handling, and accessibility labels"
  - Add empty state view
  - Add error state handling
  - Add VoiceOver labels to all interactive elements
  - Add dynamic type support
  - Write snapshot tests for empty and error states
```

**Good — Core and optional separated:**

```
Increment 2: "Add empty state and error handling"
  Goal: Search shows empty state and error state; verified manually.
  - Add empty state view
  - Add error state handling in ViewModel
  - Add error UI

Improvement Margin:
  - (Optional) Add VoiceOver labels to search screen interactive elements
  - (Optional) Add dynamic type support to result cells
  - (Optional) Add snapshot tests for empty and error states
```

**Why it's better:** The core increment is smaller and has a clear "done." Optional items are tracked but don't inflate scope or block completion.
