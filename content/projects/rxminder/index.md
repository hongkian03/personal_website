---
title: "Rxminder"
description: "A four-person medication tracking app for patients and caregivers; I led the FastAPI backend, prescription scheduling engine, and a 37-test pytest suite with over 80% backend coverage."
date: 2025-09-01
lastmod: 2025-12-01
draft: false
role: "Backend lead and developer"
period: "Sep 2025 – Dec 2025"
status: "Completed"
featured: false
category: "Software Engineering"
summary: "A four-person Boston University CS411 team built a medication tracking app for patients and caregivers. I led the FastAPI backend, the SQLModel schema, the prescription scheduling engine, and a 37-test pytest suite with over 80% backend source coverage. The team earned an A- on the final demonstration."
stack:
  - React Native
  - TypeScript
  - Expo
  - FastAPI
  - SQLModel
  - SQLite
  - Passlib
  - bcrypt
  - pytest
  - Pydantic
links: []
---

## Context

Rxminder is a medication and prescription tracking application for patients and optional secondary users such as caregivers. The project was a semester-long Boston University CS411 course project, with requirements that emphasized software design, requirements engineering, development practices, and testing rather than production polish. I worked as backend lead and developer on a four-person team from September through December 2025, and the team finished with an A- on the local demonstration and final presentation. The app was demonstrated locally and was not deployed for external users.

## Contribution

- Designed the backend architecture and database schema from scratch.
- Built approximately 90% of the core prescription and reminder backend before teammate additions and adjustments.
- Implemented the core user, login, prescription, schedule, and reminder behavior.
- Added password hashing with Passlib and bcrypt as the minimum credential-security control for the class demonstration.
- Integrated the existing React Native medication state layer with the backend APIs.
- Designed the reminder-generation and notification-scheduling workflow.
- Led discussions about requirements, architecture, database design, and technical planning.
- Led the design of the core automated API tests, using AI for boilerplate cases while personally reasoning through multi-step and integration behavior.
- Maintained and ran the combined backend test suite and coverage workflow.

## Approach

### Architecture

The mobile client is an Expo application using React Native and TypeScript. React Navigation coordinates the login, medication, subuser, and pharmacy screens. A React context store maps backend prescription records into client medication state. Expo Notifications schedules local device notifications. Expo Location and React Native Maps support the teammate-owned pharmacy locator. The medication and notification initialization paths use a hard-coded development user ID of 1, so the demo did not fully connect authenticated identity to all client data operations.

The backend is a FastAPI service that exposes ten route handlers for users, login, subusers, prescriptions, reminders, and pharmacies. SQLModel maps application models to a file-backed SQLite database. Pydantic validation enforces email format, username and password length, ISO date and time format, non-past start dates, frequency bounds, dosage bounds, and date ordering. Passlib with bcrypt hashes passwords and verifies login credentials. CORS was enabled for local client and backend integration. I chose FastAPI because the team was already familiar with Python, and SQLite provided a small portable database with minimal setup for a local demonstration.

### Data model

The normalized schema separates four entities:

- User: credentials and an optional parent_user_id for the caregiver relationship.
- Medication: owner, drug name, dosage, frequency, and notes.
- Schedule: medication, date range, creation time, and next reminder.
- Reminder: schedule, trigger time, status, and message.

Foreign keys use cascade deletion, so deleting a user removes their medications, schedules, and reminders, and deleting a medication removes its schedules and reminders. The design followed the course work on object models, class diagrams, interaction diagrams, and database design.

### Prescription and reminder workflow

For a requested daily frequency f, the backend computes an interval of 86,400 divided by f seconds. Starting from a selected time, it generates f evenly spaced times modulo 24 hours for every day in the inclusive prescription date range. Creating a prescription performs five operations in one database session: it creates a medication record, creates its schedule, generates reminder records for the date range, stores all reminders, and sets next_reminder to the earliest future reminder. This was my design decision to give users an automatic approximate schedule when they supplied only the number of daily doses.

When a prescription changes, the backend updates the existing medication record to preserve its identity, deletes prior schedule records (cascading to their reminders), rebuilds the schedule from updated and retained values, and generates a replacement set of reminders. The principal engineering challenge was avoiding stale reminders after a prescription was edited partway through its date range. The database regeneration behavior is tested. End-to-end cancellation and replacement of already scheduled operating-system notifications was not validated on a physical device.

The backend reminder endpoint returns the next pending reminder for each medication, marks it as scheduled, and advances the schedule to the following pending reminder. The mobile client fetches reminders during initialization, when the application becomes active, and after a notification is tapped, then passes them to Expo Notifications. The client rejects invalid or expired timestamps and contains mapping logic intended to avoid duplicate scheduling. However, the backend response did not include the reminder_id consumed by that mapping logic, so verified end-to-end deduplication was not achieved. The final time handling appends a fixed -05:00 offset to timestamps without an offset, which supported the Eastern Standard Time demo but does not handle other device time zones or daylight-saving transitions. Expo notifications were exercised in an iOS simulator development workflow, where the team could not validate actual notification delivery, and no physical-device delivery, load, or scale metric is available.

### Authentication and caregiver accounts

I implemented basic account creation and login with bcrypt password hashing. Login verifies a submitted password and returns user data. The implementation does not issue session tokens or protect prescription routes with authorization middleware, so it is best described as basic or manual authentication for a class demo rather than production-grade authentication.

The teammate-owned subuser feature links secondary users to a primary account through parent_user_id. Prescription listing resolves a subuser to the primary user's medications, allowing the caregiver concept to be demonstrated.

### Testing

The current suite passes all 37 backend tests: 14 core tests in backend/tests.py and 23 teammate-authored pharmacy tests in backend/test_pharmacies.py. The core suite uses pytest, FastAPI TestClient, and an isolated temporary SQLite database for each test. It covers account creation, duplicate handling, prescription creation, listing, update, and deletion, schedule regeneration, validation boundaries, subusers, and foreign-key cascades. The pharmacy suite uses unittest.mock to isolate Google Places behavior. I did not author the pharmacy suite but cleaned and maintained the combined testing and coverage workflow.

The saved coverage report shows main.py at 78%, models.py at 100%, utils.py at 92%, and approximately 81% weighted coverage across those backend source files, excluding test files. The full 90% report includes test files and should not be presented as application-source coverage. The most precise description is "over 80% backend source coverage."

## Outcome and learning

The team completed the local demonstration and final presentation and earned an A- on the course. The deployable, tested backend, the prescription workflow, and the test coverage all matched the course expectations. The deployment, physical-device notification, authorization, and healthcare-integration work were all identified as future scope.

The clearest personal lessons were about the boundary between a class demonstration and a production healthcare application. The reminders work well enough for a four-person team to demo an account, a prescription, and a local notification on a developer laptop, but the fixed -05:00 offset, the hard-coded development user ID, the missing session tokens, and the unverified physical-device notification delivery would all need to change before any patient relied on the app. I also learned how much of test design is about picking the boundaries. Coverage numbers are a useful summary, but what is equally if not more important is whether the test exercises the multi-step behavior the user actually experiences.

## Reflections

I was quite excited to take this class, typically taken by juniors and seniors, as a sophomore. I viewed it as a great chance to gain experience with and learn about software engineering best practices, and work with a team developing software (just like I recently had with [MAGK](/content/projects/magk/) but with a MUCH longer development runway and a larger emphasis on professional practices like test-driven development).

The tech in this project wasn't super mind-blowing. Nearly all coding was outsourced to an LLM (as college students typically did in 2025), but the big takeaway I had in this class and project was that, coding is literally such a small part of software engineering. A lot more of our time was spent on stuff like requirements engineering, drawing data flow tables, designing and writing tests, etc. I will not lie, as a typical CS student at the time discouraged by the increasing prevalence of AI-assisted coding, this perspective was a huge fresh breath of air. My fascination with software development (and honestly CS overall as a subject) lies in the problem-solving and the meticulous design process. With LLMs slowly but surely taking over in the coding side of things, at least there remains some human parts of the design process that won't be as easily automated (or perhaps it will be in due time regardless).

Test-driven development is something that totally does not exist in the eyes of a hobbyist or someone working on a personal project. However, having to do it for this class actually gave me a fresh new perspective of software engineering, alongside requirements engineering. It quite makes sense to write code with certain tests and functionality in mind, as those components provide a good scaffold on which one can base one's mental model on. That said, I typically just outsource the writing of simple unit tests to an LLM, and then spend more time intentionally designing more complex tests. That's something that could certainly have been improved on in this project, as I feel that my designed tests didn't effectively capture typical user experience flows.