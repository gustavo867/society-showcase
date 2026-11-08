# Evidence register

[English overview](../README.en.md) · [Visão geral em português](../README.md)

This case study was prepared from private source at revision `404d8ca` on **7 November 2026**. References below name private artifacts without reproducing their contents or linking to private infrastructure. Readers cannot inspect the private implementation through this documentation repository. Executed checks are point-in-time observations, not ongoing CI badges.

**Verified** means directly supported by the inspected repository or a recorded command/session result. **Inferred** means an interpretation. **Not identified** means evidence was insufficient in the reviewed surface. **Owner-confirmed** means information explicitly supplied by the project owner. **Requires confirmation** means an external fact or personal attribution remains unresolved.

## E00 — Technology versions

**Verified:** installed package manifests identify React Native 0.86.3, React 19.2.3, Expo 57.0.26, TypeScript 6.0.3, Zustand 4.0.0, AsyncStorage 2.2.0, React Navigation native 6.0.11/native-stack 6.7.0/bottom-tabs 6.6.1, styled-components 6.5.3 and Expo Notifications 57.0.21. Application imports establish actual use; package.json and bun.lock establish declared/resolved dependencies. Declared packages with no identified application usage are not promoted as implemented features.

## E01 — Product and navigation

**Verified:** `src/routes/index.tsx:59`, `:82`, `:97`; `src/@types/navigation.d.ts:3`; `src/screens/Scheduler/Schedules/index.tsx:87`, `:167`, `:180`, `:206`, `:227`; `src/screens/Scheduler/CreateSchedule/index.tsx:81`; `src/screens/Scheduler/Schedules/components/ScheduleCard/index.tsx:29`. These implement agenda/backup tabs, booking creation, confirmation, filters, exclusions and details. iOS/Android targets and web scripts are configured in app.json/package.json. **Inferred:** venue operators are the primary audience. **Requires confirmation:** actual adoption and native platform use.

## E02 — State and hydration

**Verified:** `src/store/schedules/index.ts:7`, `:12`, `:34`, `:36`; `src/store/theme/index.ts:11`; `App.tsx:22`, `:64`; `src/screens/Scheduler/Backup/index.tsx:38`, `:43`, `:223`. Zustand persists through AsyncStorage, the backup screen tracks hydration and startup reminders wait for hydrated state. No application encryption or remote synchronization was identified.

## E03 — Temporal rules

**Verified:** `src/helpers/date.ts:7`, `:12`, `:21`; `src/helpers/scheduleAvailability.ts:63`, `:72`, `:97`, `:104`; `src/helpers/agendaSlots.ts:6`. Tests in scheduleAvailability.test.ts cover recurrence beyond a finite annual horizon, overlapping intervals, independent fields and exclusions. **Inferred:** compact recurrence rules improve maintainability compared with materializing a long occurrence horizon. No performance metric was measured.

## E04 — Native reminders

**Verified:** `src/modules/notifications/hooks/useNotification.ts:15`, `:26`, `:49`, `:79`, `:107`; `src/modules/notifications/reconciliation.ts:18`, `:89`; `src/modules/notifications/nextOccurrence.ts:44`; `src/modules/notifications/permissions.ts:6`; `src/modules/notifications/legacyReminder.ts:17`; `App.tsx:22`. These establish local scheduling, serial execution, matching/reuse, cancellation, next-occurrence rules and lifecycle triggers. Private planning documents explicitly constrain renewal to later app activity rather than indefinite unattended delivery. **Requires confirmation:** native delivery under real device/OS conditions.

## E05 — Backup evolution

**Verified:** `src/helpers/scheduleBackup.ts:4`, `:41`, `:64`, `:74`, `:86`, `:108`; `src/screens/Scheduler/Backup/index.tsx:55`, `:87`, `:183`, `:223`; scheduleBackup.test.ts. Accepts legacy/versioned envelopes, normalizes fields/status, strips notification handles and merges by ID. Backup import replaces current state after confirmation. Validation is structural, not a complete conflict/security validator. No private fixtures or fixture record counts are included here.

## E06 — Platform integration and UI

**Verified:** `src/screens/Scheduler/Backup/index.tsx:4`, `:136`, `:150`, `:163`, `:206`; `src/screens/Scheduler/CreateSchedule/index.tsx:152`, `:169`; `src/components/NavigationHeader/index.tsx:20`; `src/theme/index.ts:3`; `App.tsx:64`. These show browser/native file paths, keyboard/safe-area treatment, shared theming and accessibility labels. The existence of labels is not an accessibility audit.

## E07 — Executed quality checks

**Verified on 7 November 2026:** bun run test → exit 0, 36 passed, no failures/skips, across eight files. bun run tsc --noEmit --incremental false → exit 0. ESLint over App.tsx and src with .ts/.tsx, no fixes → exit 1, 44 errors and 443 warnings in 57 files, with findings in 35. Tests use Node test runner and tsx; reconciliation operations are simulated. No coverage metric or executed CI result was identified.

## E08 — Delivery evidence

**Verified configuration:** `package.json:8`, `:12`; `eas.json:5`, `:12`, `:18`, `:26`; `app.json:11`, `:18`, `:52`, `:55`; `App.tsx:48`. These support development targets, EAS profiles/channels, an Android production APK target and Expo Updates fetch/reload. Values identifying accounts/infrastructure are withheld. **Verified historical artifact:** dist metadata and bundles, last changed in commit `11c8b30` in February 2023. **Not identified:** current signed native package, versioned CI workflow or release tag. **Requires confirmation:** store publication, recent builds and OTA rollout.

## E09 — Attribution

**Owner-confirmed:** Gustavo developed the application in full, with Codex assisting development. The project uses a workflow designed for AI-agent collaboration, with documentation that provides context and guides agents to follow the project's structure. These authorship and workflow statements reflect the owner's explicit confirmation; they are not inferred solely from commit metadata. Codex use is confirmed without asserting exclusive use of a particular assistance tool.

**Verified Git metadata:** the reviewed HEAD history contains 28 commits. Sixteen use the author name Gustavo; twelve use another historical alias. Additional local tool checkpoints are not counted as feature deliveries. Maintenance diffs under Gustavo include `a84b2e3` (SDK/config updates), `ea2fd20` (backup workflow), `d705b88` (rules, reconciliation and tests), `dab1bdd` (slot-boundary correction) and `d15ab7c` (backup/project setup evolution). Earlier application/recurrence/notification implementation appears under the historical alias. Private planning documents also show an agent-assisted workflow.

Git author names are metadata rather than a complete authorship record. The owner's confirmation supplies the authorship context that the initial review could not establish from Git alone. It does not change the separate scope of third-party visual assets described in [media provenance](../assets/README.md).

## E10 — Performance scope

**Verified:** `src/screens/Scheduler/Schedules/index.tsx:102`–`:149` memoizes derived calendar/agenda values; selectors read selected Zustand state. The timeline maps rows inside ScrollView (`:339`). A generic ModalSelect contains a FlatList, but its use as the active agenda list was not identified. Notification metadata updates replace the booking array (`useNotification.ts:26`). **Not identified:** profiling, measured gains, remote cache/pagination or centralized monitoring.

## E11 — Sanitized media session

**Verified:** the web preview was run from a separate sanitized copy. The viewport was set to 390 × 844 before loading. Synthetic teams replaced private-looking data, branded configuration was replaced, infrastructure values were removed and bundled backups were emptied. The agenda, booking form and backup tab were captured. The form session logged “Internal React error: Expected static flag was missing”; its cause was not investigated. These captures prove rendering/navigation in that session, not an E2E workflow, native release or clean runtime.
