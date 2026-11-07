# Engineering decisions and trade-offs

[English overview](../README.en.md) · [Visão geral em português](../README.md)

These notes reconstruct decisions from the implementation and its private planning documents. They are not historical ADRs and do not imply the portfolio owner personally made every decision. “Observed” identifies repository evidence; “Interpretation” identifies an inferred rationale.

## 1. Keep the booking workflow local

**Observed:** Zustand persists bookings and theme through AsyncStorage; JSON files support manual transfer. No business API client or server synchronization was identified. [E02, E05](evidence.md)

**Interpretation:** this keeps the operational agenda independent of a business server and reduces infrastructure needs.

**Trade-off:** separate devices do not share a booking authority, and recovery depends on retained backups. This is local persistence rather than a remote-data cache or a synchronization engine. App-level encryption and access controls were not identified.

## 2. Represent weekly recurrence as rules and exceptions

**Observed:** a weekly booking stores its starting date, weekday and excluded dates. Conflicts branch by recurrence type and compare resource identity and overlapping intervals. Weekly pairs use a direct weekday rule rather than enumerating a fixed number of weeks. [E03](evidence.md)

**Interpretation:** this keeps the model compact and avoids accepting future collisions because a finite scan ended too early.

**Trade-off:** the model is intentionally narrower than a general calendar recurrence standard. It does not demonstrate bounded series, complex recurrence rules or server-enforced concurrency. The overlap logic handles an end at midnight, not arbitrary multi-day intervals.

## 3. Reconcile reminders instead of trusting stored flags

**Observed:** the notification module compares bookings with pending OS requests, associates requests through IDs/timestamps and legacy recognition, and serializes operations through a shared Promise chain. [E04](evidence.md)

**Interpretation:** a persisted identifier is not sufficient evidence that a native request still exists. Reconciliation makes repair explicit and testable.

**Trade-off:** cancellation, native scheduling and persistence can fail separately. The queue serializes this module’s operations but is not an atomic transaction across all booking changes. Exactly-once delivery is not established.

## 4. Schedule the next eligible occurrence

**Observed:** the date calculation skips past or excluded occurrences and suppresses reminders for pending bookings. Lifecycle activity triggers renewal. Private planning explicitly limits the expected unattended period. [E04](evidence.md)

**Interpretation:** a next-occurrence strategy provides direct control over exceptions without maintaining a large scheduled horizon.

**Trade-off:** later reminders depend on another reconciliation. The case study does not promise months of unattended recurring notification delivery with the app closed. Permissions and device conditions still affect delivery.

## 5. Separate portable booking data from device handles

**Observed:** versioned backup normalization accepts older formats, defaults legacy fields and clears notification identifiers on export and import. The import screen performs explicit replacement after confirmation. [E05](evidence.md)

**Interpretation:** native request handles should be recreated on the receiving device; normalization allows data formats to evolve without discarding existing reservations.

**Trade-off:** JSON is not encrypted or signed by the application. Validation checks structure and selected values, not the complete semantic validity of every date, duplicate ID or imported conflict. Replacement is not a general merge/sync strategy.

## 6. Put test seams around rules and effects

**Observed:** recurrence/date/backup logic is extracted into helpers; the reconciler accepts cancellation, scheduling and persistence operations as parameters. Tests provide simulated operations. [E03–E05, E07](evidence.md)

**Interpretation:** these boundaries allow deterministic checks without launching native UI or requiring notification permission.

**Trade-off:** the tests cannot validate real system permission prompts, OS scheduling, file pickers, screen-reader behavior or visual layout. No E2E or measured coverage result was identified.

## 7. Configure EAS channels and version-based update compatibility

**Observed:** development, preview and production profiles have distinct channels; the two internal profiles configure internal distribution. Android production targets an APK. Updates uses an app-version runtime policy and non-development fetch/reload code. [E08](evidence.md)

**Interpretation:** the configuration prepares separate distribution paths and an OTA update mechanism.

**Trade-off:** configuration is not proof of a completed build, signature, store release or rollout. Startup update failures are not explicitly caught in the reviewed code, and no crash-monitoring integration was identified. Native releases and external service history remain unverified.
