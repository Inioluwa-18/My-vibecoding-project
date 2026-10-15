# PROMPTS.md: Living Prompt Pack

> Module 3 · Prompt Chaining. Re-architect the build with prompt chains; capture the reusable ones here.

## How to use this pack

_Each prompt is a reusable step. Chain them: the output of one becomes the input to the next._

## Prompt chain: Faith & Flow - Guided Session Recovery Chain

### Step 1: Clicking "Begin your quiet moment" currently leads to an HTTP 500 error rather than a successful session experience.  First, inspect the current routes, components, and implemented functionality. Reuse existing screens and components wherever possible. Do not assume the guided-session player is missing if it already exists.
```
Clicking "Begin your quiet moment" currently leads to an HTTP 500 error rather than a successful session experience.

First, inspect the current routes, components, and implemented functionality. Reuse existing screens and components wherever possible. Do not assume the guided-session player is missing if it already exists.

Screens and flow

Based on the actual implementation, complete any missing parts of this journey:

The user selects "Begin your quiet moment".

The app provides feedback while the session is being prepared.

If the session loads successfully, the user reaches the existing guided-session experience or a new session screen if one is genuinely missing.

The session experience should support the intended stages of settling, Scripture, reflection, and prayer, using the content and functionality already available.

The user can return home without losing their place unnecessarily.

If the session cannot open, the user receives a clear recovery experience rather than a raw server response.

If adding a session screen is necessary, maintain a clear hierarchy for the session title, duration, Scripture or devotional content, relevant audio controls if supported, and the next action.

Visual direction

Use the attached screenshot of the current Faith & Flow Home screen as the visual reference.

Preserve:

The dark forest-green background.

Sage-green accents and muted natural tones.

Elegant serif headings paired with readable sans-serif body text.

Rounded cards, restrained borders, and generous spacing.

The calm, reflective atmosphere of a Christian devotional product.

Do not copy the reference screenshot as a flat image. Implement functional UI using the existing project architecture.

Technical constraints

Inspect existing code and routing before making changes.

Reuse existing components and data structures where possible.

Preserve the current Home and Evidence navigation.

Do not create duplicate routes or duplicate components unnecessarily.

Do not invent a functioning audio backend, authentication system, or session persistence if these are not implemented.

Do not expose raw server responses to the user.

Preserve existing working functionality and avoid unrelated redesigns.

Acceptance criteria

The existing Home screen remains recognisable.

The guided-session journey is complete to the extent supported by the current architecture.

The user has a clear way to return home.

Missing functionality is identified rather than falsely presented as working.

The prototype remains consistent with the supplied visual reference.
```

### Step 2: Behavior, hard-code the states
```
1. Guided-session loading state

Trigger: The user selects "Begin your quiet moment" and the app begins opening the session.

Display:
"Preparing your quiet moment…"

Requirements:

Show a clear loading indicator.

Prevent repeated clicks from initiating duplicate requests.

Keep the user informed while the request is in progress.

Remove the loading state when the request succeeds or fails.

Do not simulate successful session loading if the actual request fails.

2. Guided-session error state

Trigger: The request to open the guided session fails, including an HTTP 500 response.

Display:
"We couldn't open your session. Your quiet moment is still here. Please try again."

Actions:

Primary action: "Try again"

Secondary action: "Back to Home"

Requirements:

Retry the actual session-opening operation.

Provide feedback while retrying.

Do not show raw HTML, stack traces, internal server responses, or infrastructure details.

Do not claim that a session has been saved or that progress is preserved unless this is verified.

If the retry fails, keep the friendly error message visible and allow the user to recover.

3. History-loading error state

Trigger: The request for user session history fails.

Display:
"We couldn't load your history. You can still begin a session."

Action:
"Try again"

Requirements:

Keep the user able to begin a new session when the existing architecture permits it.

Distinguish a failed history request from a successful request that returns no history.

Avoid allowing the history error to unnecessarily block the Home screen.

4. Empty history state

Trigger: History loads successfully but contains no previous sessions.

Display:
"Your journey begins here."

Supporting text:
"Start a quiet moment with God, and your journey will grow one session at a time."

Action:
"Begin a quiet moment"

Requirements:

Only display this state when history successfully loads and is genuinely empty.

Do not use it as a substitute for a network or server error.

5. General implementation constraints

Use the existing project framework, routing, and request-handling patterns.

Connect actions to actual existing functions wherever possible.

Do not introduce fake backend responses or hard-code a successful result to hide the HTTP 500 error.

Do not change unrelated styling or functionality.

Verify the loading, success, error, retry, and empty states where the existing implementation makes testing possible.

Clearly report any state that cannot be tested because the backend or required service is unavailable.
```

### Step 3: Refine, one surgical polish
```
The current screenshot shows a message containing an HTTP 500 response and raw HTML from the server. The Home screen also displays a history-loading error.

First, audit

Identify:

Where the session-opening error is rendered.

Where the raw server response enters the user interface.

Whether the current error component can be reused.

Whether retry and back-navigation actions are already implemented.

Whether the history error and session-opening error are handled separately.

Briefly explain the specific UI and usability problems you find before implementing the refinement.

Then make the smallest necessary change

Improve the session-opening error experience so that:

The error message is understandable, calm, and appropriate for a Christian devotional app.

The primary action is clearly labelled "Try again".

The user can return to Home.

Technical details are logged or handled internally where appropriate, not rendered as user-facing copy.

The layout follows the existing Faith & Flow visual language.

The error does not occupy excessive space or push the main navigation out of view unnecessarily.

Use this copy:
"We couldn't open your session. Your quiet moment is still here. Please try again."

If an error component already exists, refine it instead of creating a duplicate.

Constraints

Do not redesign the Home screen.

Do not alter the Evidence screen.

Do not introduce new features.

Do not change unrelated components, typography, colours, spacing, routes, or data models.

Do not pretend the underlying HTTP 500 issue is fixed by changing the message.

Preserve all working functionality.

Final verification

Explain what changed and what remains unresolved. Confirm whether the session-opening operation itself succeeds, or whether the interface has only been improved to handle the failure gracefully.

Do not change anything else beyond what is required for this refinement.
```

## Reusable techniques learned

- _____
- _____

## What broke (and the fix)

_Where a single mega-prompt failed and chaining fixed it._

The server error that comes up after the "Start your quiet moment"
