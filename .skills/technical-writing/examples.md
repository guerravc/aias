# Technical Writing — Examples

Before/after examples for each writing pattern.

---

## 1. Problem Framing

**Bad:**
> There is a problem with the filters. They don't work well and users are complaining.

**Good:**
> The position list filter in MobileMax ignores the "Remote only" option, returning all positions regardless of work mode. This affects recruiters who rely on filters to shortlist candidates, forcing them to scroll through 200+ unfiltered results manually. Currently the filter sends `workMode=all` to the API instead of `workMode=remote`; the expected behavior is to pass the selected filter value and display only matching positions.

**Why it's better:** Names the specific component, quantifies the impact, states current vs desired behavior, and avoids proposing a solution.

---

## 2. Acceptance Criteria

**Bad:**
> - The login should work correctly
> - Edge cases should be handled
> - Error messages should appear when needed

**Good:**
> - Given a valid email and password, When the user taps "Sign In", Then the app navigates to the Home screen within 2 seconds.
> - Given an unregistered email, When the user taps "Sign In", Then the app displays "No account found for this email" below the email field.
> - Given a valid email and wrong password, When the user taps "Sign In" 5 times consecutively, Then the app displays "Account temporarily locked. Try again in 15 minutes" and disables the Sign In button.

**Why it's better:** Each criterion is independently testable with a clear precondition, action, and observable outcome. Failure paths are named explicitly.

---

## 3. Root Cause Description

**Bad:**
> The app crashes sometimes when loading the candidate list. It seems to be a backend issue or maybe a parsing problem.

**Good:**
> The crash log shows a fatal nil unwrap at `CandidateListViewModel.swift:87` when `response.candidates` is `nil` (evidence). The API returns `"candidates": null` instead of an empty array when the position has no applications — the ViewModel force-unwraps this value without a nil check (mechanism). Users see a crash immediately after opening any position with zero candidates, which affects approximately 30% of active positions (consequence).

**Why it's better:** Links specific evidence (crash log + line number) to a named mechanism (nil response + force unwrap) and quantifies the user-visible consequence.

---

## 4. Risk Articulation

**Bad:**
> There are some risks with this migration. Things could go wrong and we should be careful.

**Good:**
> | Risk | Likelihood | Impact | Mitigation |
> |---|---|---|---|
> | Migration fails mid-batch, leaving candidate records in inconsistent state | Medium | High | Run migration in a transaction with rollback; test on staging with production-size dataset first |
> | API response time exceeds 3s during migration due to lock contention | Low | Medium | Schedule migration during off-peak hours (Saturday 02:00 UTC); monitor response times during execution |
> | Legacy clients send requests in old format after migration | High | Low | Keep backward-compatible endpoint for 2 sprints; log old-format requests to track adoption |

**Why it's better:** Each risk is specific, calibrated (likelihood + impact), and has a concrete mitigation. The table format makes it scannable.

---

## 5. Increment Goals

**Bad:**
> - Work on the authentication module
> - Improve the UI
> - Do some refactoring

**Good:**
> - Implement the OAuth token refresh flow using `AuthService`; verified by unit test for token expiry + renewal and manual login after 30-minute idle.
> - Replace all hardcoded color hex values in `ProfileView` with `RDSColors` tokens; verified by visual comparison with Figma design and SwiftUI preview rendering.
> - Extract network retry logic from `CandidateRepository` into a reusable `RetryPolicy` protocol; verified by existing repository tests still passing and new `RetryPolicyTests` covering 3 retry scenarios.

**Why it's better:** Each goal starts with an action verb, names the specific scope, and includes how to verify "done." Each is self-contained.

---

## 6. Conciseness

**Bad:**
> It is important to note that, in order to facilitate the implementation of the authentication module, we will need to utilize the existing networking layer. Basically, the approach we are going to leverage involves implementing a solution that essentially wraps the current HTTP client. Due to the fact that the token refresh mechanism is not currently implemented, we will need to add it prior to the main authentication flow implementation.

**Good:**
> The authentication module will use the existing HTTP client. We need to add token refresh before implementing the main auth flow, since the client does not handle expired tokens today.

**Why it's better:** 2 sentences instead of 4. Same information. No filler words ("basically", "essentially", "leverage", "facilitate", "utilize", "in order to", "due to the fact that"). Leads with the point instead of throat-clearing.
