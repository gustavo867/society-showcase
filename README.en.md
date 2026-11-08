<img src="assets/icon.svg" width="64" height="64" alt="">

# Society Scheduler

**Booking management for a small-sided football venue, built with React Native, Expo and TypeScript.**

[Português](README.md) · [AI-assisted development](#ai-assisted-development) · [Architecture](#architecture) · [LinkedIn](https://www.linkedin.com/in/gustavo-santana-83ba611a6/)

An agenda for checking availability, managing weekly bookings and tracking confirmations. The app combines local persistence, on-device reminders and data backups, addressing scheduling conflicts, compatibility across data versions and consistency between application state and the operating system.

## See the app

<a href="assets/demo.gif"><img src="assets/demo.gif" width="280" alt="Screen tour of the agenda, booking form and backup"></a>

*Tour of three real web-preview captures with fictitious data. The GIF presents screens; it is not an interaction recording or a native release.*

<details>
<summary>View the three screens in detail</summary>

| Agenda | New booking | Backup |
| --- | --- | --- |
| ![Agenda with fictitious teams](assets/screenshots/agenda.png) | ![Booking form with fictitious data](assets/screenshots/create.png) | ![Backup with demonstration records](assets/screenshots/backup.png) |

Captured at 390 × 844 CSS pixels. [Media provenance and scope](assets/README.md).

</details>

## Features

- **Agenda and availability:** daily and weekly navigation, a monthly calendar, search and filters.
- **Recurring bookings:** weekly reservations with per-date exclusions and overlap checks per field.
- **Confirmation and reminders:** booking status tracking and local notification coordination.
- **Backup and restoration:** JSON import/export, legacy-format compatibility and confirmation before replacing data.

## AI-assisted development

I developed this application with assistance from **Codex**, using a workflow designed for development with AI agents.

Project documentation gives agents the application context and the structure they should follow. It is part of the development process:

- **Shared context:** document application behavior and project decisions to guide each task.
- **Structure as a reference:** guide changes through the application's existing organization.
- **Continuity across tasks:** maintain documented guidance so agents follow the same patterns as the code evolves.

This workflow integrates Codex assistance with the project's context and structure. [Authorship and process record](docs/evidence.md#e09--attribution).

## Architecture

Screens use domain rules and Zustand actions; AsyncStorage persists local state. A notification module coordinates operating-system requests, while platform file APIs support manual backup transfer between installations.

![Architecture: React Native screens, domain rules, Zustand, AsyncStorage, notifications and files](assets/architecture.png)

The app keeps data on the device. No business backend or remote synchronization was identified. [Components and data flows](docs/architecture.md).

## Three engineering decisions

| Challenge | Implemented solution | Why it matters |
| --- | --- | --- |
| Conflicts between weekly bookings | Compare recurrence, field and intervals without a fixed annual horizon | Avoids restricting conflict detection to an arbitrary window of weeks |
| Local state differs from system reminders | Reconcile pending requests and serialize native effects | Enables valid reminders to be reused, duplicates to be cancelled and missing requests to be recreated |
| Older data on another installation | Normalize versioned backups and strip device notification identifiers | Preserves compatibility and allows reminders to be rebuilt at the destination |

Reminders schedule the next eligible occurrence and are renewed through app activity. Restoration waits for state hydration and requires replacement confirmation. [Decisions and trade-offs](docs/engineering-decisions.md).

## Applied technologies

| Area | Technologies |
| --- | --- |
| Mobile application | React Native, Expo, React, TypeScript |
| State and persistence | Zustand, AsyncStorage |
| Navigation and UI | React Navigation, styled-components, Safe Area Context |
| Dates and validation | date-fns, Yup and TypeScript domain rules |
| Native integrations | Expo Notifications, FileSystem, DocumentPicker, Sharing and Updates |
| Tests and tooling | Node test runner, tsx, ESLint, Bun and EAS configuration |

[Inspected versions and usage evidence](docs/evidence.md#e00--technology-versions).

## Testing and quality

The review on **7 November 2026** recorded **36 passing tests** and **TypeScript checking with no errors**. The suite covers scheduling rules, backups and notification coordination through simulated operations.

ESLint findings and an observed web-preview runtime error remain open. Full results and outstanding native validation are documented in [Testing, quality and delivery](docs/testing-and-delivery.md); the checks performed do not establish production readiness.

Further reading: [five engineering challenges](docs/technical-challenges.md) · [evidence register](docs/evidence.md).

## About this repository

This is the public showcase of an application with private source code. **Society Scheduler** is its portfolio presentation name. The repository contains documentation and sanitized images, with no application source code or installable package. [Public material scope](docs/disclosure.md).

**Gustavo** · [Professional profile on LinkedIn](https://www.linkedin.com/in/gustavo-santana-83ba611a6/)
