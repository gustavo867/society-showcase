# Five engineering challenges

[Back to the overview](../README.md)

The implementations below are evidenced by the private repository. Benefits describe behavior visible in code or tests. They are not business metrics, performance measurements or claims of personal authorship.

## 1. Prevent conflicts across unbounded weekly reservations

**Context:** an existing weekly reservation can conflict with another series or a one-off booking far beyond its starting date. Excluded occurrences and independent fields change the result.

**Why it matters:** checking a finite future window can silently accept collisions after that window. Comparing only equal start times also misses overlapping intervals.

**Implemented solution:** normalize field identity; compare intervals; evaluate one-off/one-off, weekly/one-off and weekly/weekly cases separately. Apply the start date and excluded dates when evaluating a concrete occurrence. Weekly pairs are compared directly by weekday and interval.

**Technologies and patterns:** TypeScript, date-fns, pure domain helpers and regression tests.

**Observable benefit:** passing tests cover old weekly starts, a one-off occurrence beyond 53 weeks, different resources, overlapping intervals and excluded dates. The direct weekly rule has no fixed annual horizon.

**Limits:** weekly recurrence is a constrained model without an end-date concept. Checking candidate conflicts scans the stored array, and no other device/server participates in the decision. [E03, E07](evidence.md)

## 2. Keep local reminders consistent with the operating system

**Context:** application metadata can outlive a native request. Startup, foreground activation, confirmation and restoration can trigger overlapping scheduling attempts.

**Why it matters:** trusting a stored flag can leave a booking without a reminder; scheduling on every activation can create duplicates.

**Implemented solution:** a native adapter reads pending requests, and a testable reconciler matches them to bookings and the next eligible timestamp. It reuses one valid request, cancels duplicates or obsolete requests and recreates a missing one. A Promise queue serializes native operations; legacy request detection supports older reminders.

**Technologies and patterns:** Expo Notifications, application lifecycle hooks, reconciliation, effect serialization and operation injection.

**Observable benefit:** passing simulated-operation tests demonstrate request reuse, duplicate cancellation and one replacement across repeated reconciliation with the resulting pending request.

**Limits:** this does not establish real-device delivery or exactly-once behavior. A single next occurrence is scheduled. Renewal requires later application activity; permission errors, cancellations and persistence are not one transaction. [E04, E07](evidence.md)

## 3. Evolve backups without carrying obsolete device state

**Context:** historical data uses both a persisted-store envelope and later versioned backups, including inconsistent legacy field identifiers and missing status.

**Why it matters:** a format update can make existing data unreadable; imported notification handles cannot be assumed valid on a different installation.

**Implemented solution:** parse incoming JSON as unknown, check its structure, accept known versions and apply shared normalization on import/export. Repair known field identity inconsistencies, default missing status and strip native notification metadata. A Map-based merge lets later backup entries replace earlier entries with the same ID.

**Technologies and patterns:** TypeScript type guards, schema versioning, normalization, JSON and identity-based deduplication.

**Observable benefit:** passing tests cover legacy/current imports, version normalization, metadata removal, empty observations, duplicate-ID precedence and invalid/unknown formats.

**Limits:** validation does not fully enforce temporal semantics, imported reservation conflicts or maximum file size. Files are not encrypted or signed. The public case study includes none of the private fixtures. [E05, E07](evidence.md)

## 4. Respect hydration and order destructive restoration

**Context:** the initial Zustand state is empty until asynchronous persistence finishes. Restore also changes application data and native reminders.

**Why it matters:** exporting too early can produce an empty backup; replacing data while old reminders remain scheduled can create misleading notifications.

**Implemented solution:** the backup screen tracks hydration and busy state, disables actions until ready, shows the local record count and asks for confirmation. The replacement flow cancels existing reminders before replacing bookings and then reconciles imported reminders. Application startup also defers notification reconciliation until hydration.

**Technologies and patterns:** Zustand persist lifecycle callbacks, explicit UI state, user confirmation and ordered asynchronous effects.

**Observable benefit:** hydration guards and cancellation-before-replacement are explicit in the implementation. Failure feedback explains when reminder updates need another application activation.

**Limits:** no rollback coordinates every native cancellation with store replacement. Guards are local to the backup flow rather than a general hydration gate over every screen. No automated UI test of the full restore transaction was identified. [E02, E04, E05](evidence.md)

## 5. Adapt one application to different platform interfaces

**Context:** browsers and native mobile apps expose different download, file-selection and sharing mechanisms. Keyboard and safe-area behavior also vary.

**Why it matters:** shared React Native screens do not remove differences in operating-system capabilities or permission flows.

**Implemented solution:** the web save path creates a Blob/download link; native save uses directory/file APIs, native share uses Expo Sharing, and import reads browser or native file objects. The UI distinguishes cancellation from failure and adapts keyboard/safe-area behavior.

**Technologies and patterns:** React Native Platform, Expo FileSystem/DocumentPicker/Sharing, Safe Area Context and shared navigation/components.

**Observable benefit:** distinct implementation paths exist for browser download, native files and native sharing, with localized user feedback. Sanitized web captures show the agenda, form and backup screens.

**Limits:** native picker behavior, notification delivery and OS lifecycle scenarios still need platform testing. The captured web session logged a React navigation-time runtime error, whose root cause was not investigated in this documentation task. [E01, E06, E11](evidence.md)
