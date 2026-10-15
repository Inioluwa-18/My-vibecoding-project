# Living PRD

> Module 4 · Production Specs. Refactor for readability; extract a living PRD that stays true as the build evolves.

## Problem

_What user problem does this solve? Tie to the validated hypothesis._

Christians who want intentional time with God may need to move between Bible, audio, and devotional apps. This can interrupt a session and scatter their saved passages, notes, and reading progress.

Hypothesis: Bringing Scripture, Christian audio, devotionals, and a personal library into one peaceful experience will make it easier to begin—and return to—intentional time with God.

Validation boundary: The prototype demonstrates that these activities can work together, and the creator has confirmed the coffee/beige visual direction and preference for genuine photography. However, no user-research findings or engagement results were supplied. The hypothesis is therefore supported by product direction and technical feasibility, not yet validated by user behavior.

The product should validate reduced friction and repeat use, rather than assume that combining features creates value.

## Users & jobs

- **Primary user:** Primary user: A Christian seeking a simple, personal space for regular Scripture reading, reflective listening, and devotionals, primarily on a phone.
- **Job to be done:** Primary job to be done:  When I have time to spend with God, help me begin reading, listening, or reflecting without switching between apps, so I can stay present and return to where I stopped.  Supporting jobs:  Read an available Bible translation and adjust reading comfort. Save passages, highlight verses, and record private thoughts. Listen while moving between reading and devotional screens. Resume reading and find saved content across devices. Read a daily reflection whose imagery supports its Scripture.

## Scope

- **In:** In — working prototype

Real Clerk account creation, sign-in, account management, and sign-out in the development environment.
Responsive Home, Bible, Listen, Study, My Library, and Profile destinations.
Bible reading through an external provider, currently supporting configured KJV and WEB availability.
Reference search, chapter navigation, reading preferences, highlights, bookmarks, saved passages, and private Bible notes.
Four original devotionals with saving and completion tracking.
Daily Home devotional selection with Scripture-matched photographs and a nature-photo fallback.
Six playable, browser-generated instrumental demos, favorites, persistent playback, and personal playlists.
PostgreSQL-backed, account-scoped personal records and synchronization.
In — intended MVP, not yet complete

Guided Time With God sessions.
Bible plans and plan progress.
Focus Mode controlling this app’s notifications.
Whole-app light/dark/system themes and broader preferences.
Additional player controls, including seeking, shuffle, and repeat.
- **Out (explicitly):** Out

A licensed commercial Christian music catalog.
Unlicensed copyrighted Bible translations or recordings.
Podcasts, sermons, courses, books, communities, events, and marketplace operations.
Payments, purchases, and creator publishing.
Offline downloads.
Claims that the web app can block all device notifications.
Spotify or YouVersion interface replication.

## Requirements

| # | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| 1 | Account access — implemented | Must | Users can create an account, sign in, manage their account, and sign out. Protected content and APIs require authentication. |
| 2 | Private, persistent personal data — implemented | Must | _____ |
| # | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| 3 | Responsive navigation | Must | Six destinations remain accessible through mobile bottom navigation and tablet/desktop sidebar navigation. Layouts remain usable at narrow phone/folded widths without horizontal overflow. |
| 4 | Rights-aware Bible reading | Must | Users can select an available translation, book, and chapter; search by reference; and see attribution. Unavailable translations and provider failures produce explicit states, never invented Scripture. |
| 5 | Reading tools | Should | Users can adjust font size and reading width, highlight verses, bookmark/save passages, and create, edit, or delete private Bible notes. Records retain translation and passage context. Scripture sharing excludes private notes. |
| 6 | Personalized Home | Should | Home shows account-relevant reading and saved-content entry points. The daily devotional updates with the local calendar day, and its title, Scripture, photo, and reading link refer to the same entry. |
| 7 | Photographic devotional imagery | Should | Each current devotional uses an approved, Scripture-relevant real photograph. Missing or failed artwork uses a separate nature photograph; a second failure produces an accessible unavailable state without repeated retries. |
| 8 | Devotionals | Should | Users can open Scripture, reflection, and prayer; save a devotional; and track completion across sessions. The collection is not presented as a large editorial catalog. |
| 9 | Persistent listening | Should | Original demos play audibly, remain playing during in-app navigation, and support play/pause, previous/next, volume, and favorites. Playback failures are visible. Commercial streaming is not implied. |
| 10 | Personal playlists | Should | Users can create, rename, and delete playlists; add, remove, and reorder demo tracks; and play them sequentially. Stale edits require refresh rather than silently overwriting newer changes. |
| 11 | My Library | Should | Users can find saved audio and devotionals, Bible records, reading history, and playlists through the relevant library views. Resume links open the associated content. |
| 12 | Bible plans | Should | Users can start a plan, open its daily readings, track progress, and complete it. Progress persists to their account. |
| 13 | Time With God | Should | Users can choose 15, 30, 45, or 60 minutes, a spiritual focus, and an audio preference; then follow a guided session with actual progress and completion state. |
| 14 | Focus Mode and themes | Should | Time With God defaults to Focus Mode on. Settings explain its in-app notification limits and support the specified notification choices. Light/dark/system themes apply consistently. |
| 15 | Advanced listening controls | Could | Seeking changes the audible playback position; shuffle and repeat behave consistently with the active playlist. Controls must not merely simulate these effects visually. |
| 16 | Accessibility and recovery | Must | Core flows support keyboard navigation, visible focus, accessible labels, readable contrast, and meaningful loading, empty, and error states. Responsive screenshots alone do not constitute a full accessibility audit. |


## Data & events

_What gets stored, what gets tracked._

Data & events
Real data and services

Identity: Clerk-managed account information and sessions.
Persistence: PostgreSQL records for account ownership, saved audio, Bible bookmarks and reading history, Bible records/preferences, devotional saves/completion, playlists, and playlist items.
Synchronization: Account library refreshes on window focus and periodically while open.
Legacy import: Existing device records are preserved and imported idempotently; replaying an imported snapshot must not restore subsequently removed saves.
Scripture: Genuine text fetched through the configured Bible provider. KJV availability requires attention to territorial rights; WEB is identified as public domain.
Photography: Real, locally served photographs with documented source and license information.
Demo, limited, or absent

Audio is genuinely playable but consists of six browser-generated instrumental demos, not streamed commercial recordings.
Devotionals are four original entries, not a live publishing service. Daily selection rotates this collection rather than supplying a newly published devotional every day.
Time With God, Bible plans, broader settings, and future marketplace/resource models must not be represented as completed behavior.
Photo-sharing cards and expanded photo-permission records are ongoing work, not confirmed deliverables of this snapshot.
The inspected application does not establish implemented product-event analytics or measured hypothesis validation.
Proposed events—not confirmed instrumentation

Events	Purpose
signup, first_bible_read	Measure activation.
verse_highlighted, verse_bookmarked, note_created	Understand use of personal reading tools.
devotional_started, devotional_completed	Measure reflection engagement.
audio_played, audio_completed, playlist_created	Understand listening adoption.
plan_started, plan_completed	Measure future plan participation.
time_with_god_started, time_with_god_completed	Measure the proposed signature experience.
Events should contain only necessary identifiers and metadata—not note text, email addresses, or private reflection content. Future-feature events should begin only when those behaviors exist.

To evaluate the hypothesis, measure time to first meaningful activity, return use, completed sessions, and cross-module use, alongside interviews about distraction and convenience. Targets remain undecided.

## Open questions

Open questions
What research or results support the “validated hypothesis”? Who participated, and what specifically was validated?
Is the first release intended to validate the current read/listen/reflect experience, or must Time With God be included?
What activation and retention thresholds would justify further investment?
Which countries will launch first, and what translation and recording rights are required there?
Will Christian audio ultimately come from direct creator agreements, a licensed distributor, or another source?
Who owns devotional theology, editorial review, photographic selection, and publishing frequency?
Should the four-entry rotation remain during testing, or should readers receive genuinely new daily content?
What retention, export, deletion, and account-deletion policies should apply to private spiritual records?
Which additional accessibility, production-authentication, security, and cross-device checks are required before public release?
