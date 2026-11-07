# Testing, quality and delivery

[English overview](../README.en.md) · [Visão geral em português](../README.md)

## What was actually executed

Review date: **7 November 2026**, private source revision `404d8ca`. Commands were run against the original source without autofixes or dependency installation. The original tracked contents were preserved.

| Check | Observed result | What it establishes |
| --- | --- | --- |
| `bun run test` | Exit 0; 36 passed, 0 failed, 0 skipped | The included rule/coordination tests passed in the review environment |
| `bun run tsc --noEmit --incremental false` | Exit 0 | The configured TypeScript project passed static checking |
| ESLint over App.tsx and src, .ts/.tsx, without fixes | Exit 1; 44 errors and 443 warnings | Existing lint configuration is not clean on the reviewed tree |
| Sanitized web preview | Bundled and rendered at 390 × 844 CSS pixels; agenda/form/backup navigation captured | These screens rendered in this isolated browser session |

The browser session logged `Internal React error: Expected static flag was missing` when opening the form. The UI remained navigable. Notification support and a deprecated pointerEvents prop also produced warnings. The root cause was not investigated; this session is not a clean runtime sign-off. [E07, E11](evidence.md)

## Automated test scope

Eight test files cover:

- Brazilian/ISO date conversion and preservation of stored agenda times outside current creation slots.
- One-off and weekly reservation conflicts, independent fields, exclusions and legacy identity correction.
- Versioned/legacy backups, normalization, metadata removal, round-trip preservation and merge precedence.
- Notification permissions, legacy request recognition, next eligible occurrence and duplicate/missing-request reconciliation.

The reconciler is tested through simulated scheduling/cancellation/persistence operations. The private backup tests use bundled fixtures, which are excluded from this public repository. Test data volume is not evidence of users or business scale. [E03–E05, E07](evidence.md)

No measured coverage percentage, component-test suite, UI E2E suite or executed CI run was identified. The presence of ESLint dependencies/plugins does not imply the use of Jest or a successful lint gate.

## Build and distribution evidence

| Evidence level | Finding |
| --- | --- |
| Verified configuration | Expo development scripts for iOS, Android and web; EAS build scripts |
| Verified configuration | development and preview profiles: internal distribution and distinct channels |
| Verified configuration | production profile: Android APK target and production channel; empty production submit configuration |
| Verified configuration | iOS/Android identifiers, Expo Updates project URL and app-version runtime policy; values withheld |
| Verified historical artifact | Metro iOS/Android bundles and source maps last changed in February 2023 |
| Not identified in reviewed tracked files | Current signed APK/AAB/IPA, native ios/android projects, release tags or a versioned CI workflow |
| Requires external confirmation | Successful recent EAS builds, signing, internal installs, store listing/approval and OTA rollout |

Historic JS bundles do not prove that the current native project builds or that a signed application was distributed. Development/preview/production channels do not demonstrate separate backend environments. No external build dashboards or store accounts were queried. [E08](evidence.md)

## Reliability mechanisms and remaining limits

Observed mechanisms include localized validation/file/permission feedback, notification request reconciliation, lifecycle hydration guards and legacy data normalization. No centralized crash reporting, production telemetry or performance profiling was identified. Startup reconciliation errors are suppressed, and the update fetch path has no explicit catch. [E02, E04, E05, E08](evidence.md)

The main timeline is not virtualized. Memoization and state selectors are present, but there are no measured startup, frame-rate, memory, network or battery improvements. No percentages or user-impact claims are inferred from these implementation choices. [E10](evidence.md)

## Validation still needed before release claims

This is a list of unperformed validation, not a record of completed work: exercise notification permissions and real delivery on iOS/Android; receive a reminder with the app closed and reopen to renew the weekly occurrence; exclude/delete/restore without duplicate requests; test picker cancellation and save/share on both mobile platforms; test time-zone changes and daylight-saving boundaries; reproduce/investigate the web React error; decide which existing lint findings block delivery. The original repository was not changed to address these items.
