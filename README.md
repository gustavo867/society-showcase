<img src="assets/icon.svg" width="64" height="64" alt="">

# Society Scheduler

### React Native engineering case study · iOS and Android targets

[Português](README.pt-BR.md) · [Architecture](docs/architecture.md) · [Engineering decisions](docs/engineering-decisions.md) · [Technical challenges](docs/technical-challenges.md) · [Testing and delivery](docs/testing-and-delivery.md)

Society Scheduler is an editorial name for a private application that manages reservations for a small-sided football venue. This documentation-only showcase explains the engineering behind its local data model, recurring bookings and native reminders. The original brand and implementation remain private.

## Architecture at a glance

React Native screens → domain helpers and Zustand → AsyncStorage. Expo Notifications reconciles local reminders with operating-system requests; platform file APIs handle manual JSON backup transfer. No business backend or remote synchronization was identified. [Architecture details](docs/architecture.md)

## Screen tour — sanitized web captures

![Screen tour assembled from three sanitized web captures](assets/demo.gif)

This GIF is a slideshow of real captures, not an interaction recording or a native release demo. [Media provenance](assets/README.md)

## Problem and product

Venue operators need to see availability, avoid overlapping reservations, manage weekly bookings and preserve their records when changing devices. The application addresses these workflows through a locally persisted agenda and manual JSON backup transfer. The operator audience is inferred from the implemented workflows; adoption and business impact have not been independently established. [E01, E02](docs/evidence.md)

Implemented features include date/week navigation, a monthly calendar, search and filters, booking creation and confirmation, weekly recurrence with per-date exclusions, booking details, local reminders, and backup import/export. There are configured iOS and Android targets and a web preview path. [E01, E03, E04](docs/evidence.md)

## Three technical decisions

- **Recurring availability:** explicit comparisons between one-off and weekly reservations avoid a fixed annual conflict-checking horizon and respect resource identity and excluded occurrences. [E03](docs/evidence.md)
- **Reminder reconciliation:** application state is compared with pending operating-system notification requests; valid requests are reused, duplicates are cancelled, and effects are serialized. [E04](docs/evidence.md)
- **Data evolution:** versioned backup import normalizes legacy records and removes device-specific notification metadata before rebuilding local reminders. [E05](docs/evidence.md)

Hydration guards and explicit replacement confirmation also protect restoration from acting on its initial empty state. [E02, E05](docs/evidence.md)

## Technology stack

| Area | Applied technologies |
| --- | --- |
| Mobile foundation | React Native 0.86.3, React 19.2.3, Expo 57.0.26 |
| Language and navigation | TypeScript 6.0.3; React Navigation native stack and bottom tabs |
| State and persistence | Zustand 4.0.0; AsyncStorage 2.2.0 |
| UI | styled-components, Expo Vector Icons, Safe Area Context, Size Matters |
| Rules and validation | date-fns; Yup with additional domain validation |
| Platform integrations | Expo Notifications, FileSystem, DocumentPicker, Sharing and Updates |
| Quality and packaging | Node test runner through tsx, ESLint, Bun and EAS configuration |

Versions identify the inspected local installation, not proof of a successful native release. Reanimated is declared in the project but is not presented here as an implemented animation feature. No business API client or backend implementation was identified. [E00, E06, E08](docs/evidence.md)

## Architecture

![Conceptual architecture of the private application](assets/architecture.png)

Screens call domain helpers and Zustand actions. Booking state is persisted through AsyncStorage. A notification adapter coordinates native effects through a testable reconciler. Backup import/export connects the local model to platform file APIs. This is a small modular application; the case study does not assign a framework label beyond what the implementation supports. [Architecture and boundaries](docs/architecture.md)

## Technical challenges

The deeper case studies cover recurring conflict detection, reconciling native reminders, preserving legacy backups, ordering hydration and destructive restore operations, and adapting file workflows across platforms. Each explains the implementation, observable benefit and remaining limitation. [Read the five challenges](docs/technical-challenges.md)

## Tests, quality and delivery

The repository review on **7 November 2026** executed **36 tests successfully** and completed TypeScript checking with exit code 0. The tests exercise rules, migrations, permissions and notification coordination using simulated operations; they do not prove native notification delivery. ESLint reported **44 errors and 443 warnings**. [E07](docs/evidence.md)

EAS profiles define internal development/preview distribution and an Android production APK target. Expo Updates is configured. Store publication, a current signed native build, executed CI and production OTA rollout were not verified. A web screenshot session also logged a React runtime error during navigation; its cause remains uninvestigated. [Testing scope and delivery evidence](docs/testing-and-delivery.md)

## Screenshots and walkthrough

These are real captures from an isolated, sanitized **web preview at 390 × 844 CSS pixels**, using fictitious teams. Branding, infrastructure identifiers and bundled real-looking backup data were removed from the preview copy. They are not screenshots of validated native releases. [Media provenance](assets/README.md)

| Agenda | Booking form | Backup |
| --- | --- | --- |
| ![Agenda with fictitious teams](assets/screenshots/agenda.png) | ![Booking form with fictitious data](assets/screenshots/create.png) | ![Backup screen with synthetic record count](assets/screenshots/backup.png) |

[Open the animated screen walkthrough](assets/demo.gif). The GIF is a sequence of these captured screens, not a recording of an end-to-end transaction.

## My contribution: evidence and boundaries

The reviewed private Git history attributes maintenance commits to **Gustavo** for SDK/dependency updates, backup workflows, scheduling rules, notification reconciliation, tests and project setup. These are Git-attributed changes, not a claim of exclusive authorship. Historical author aliases, exact personal responsibilities, collaboration and use of assistance tools still require confirmation. [Attribution record](docs/evidence.md#e09--attribution)

## Private source and disclosure

This repository contains documentation and sanitized media only. It includes no proprietary source code, business records, private endpoints, credentials, native packages or source maps. It is not an installable application. Publication of this documentation and its sanitized images was authorized with the original name withheld. [Disclosure scope](docs/disclosure.md)

Connect with Gustavo on [LinkedIn](https://www.linkedin.com/in/gustavo-santana-83ba611a6/).
