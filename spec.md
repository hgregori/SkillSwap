# Feature Specification: SkillSwap Core Platform

**Feature Branch**: `001-skillswap-core`  
**Created**: 2026-11-05  
**Status**: Draft  
**Input**: User description: "SkillSwap is a web application designed for university students to exchange skills, peer tutoring, and technical project feedback. Students can create a profile highlighting what they can teach (e.g., Next.js, Python, graphic design) and what they want to learn. The app helps users discover peers with matching interests, schedule study sessions, and track their learning goals."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Profile Creation & Skill Management (Priority: P1)

As a university student, I want to create an account and set up my profile with skills I can teach and skills I want to learn, so that other students can discover me for peer tutoring or collaborative study.

**Why this priority**: The foundation of the platform relies on students stating their teachable and desired skills. Without profiles and skill tags, discovery and matching are impossible.

**Independent Test**: Can be tested by registering a new student, adding teach skills (e.g., "Python", "Next.js") and learn skills (e.g., "Graphic Design"), and verifying the profile displays both categories correctly.

**Acceptance Scenarios**:
1. **Given** an unauthenticated visitor, **When** they register with university credentials, **Then** a student profile is created and they are logged in.
2. **Given** an authenticated student on their profile page, **When** they add "Python" under "Skills I Can Teach" and "Figma" under "Skills I Want to Learn", **Then** both skills are saved and visible on their public profile card.
3. **Given** an authenticated student, **When** they update their bio or remove a skill tag, **Then** the updated profile reflects the changes immediately.

---

### User Story 2 - Skill Directory & Peer Discovery (Priority: P1)

As a student seeking help, I want to search and filter peers by specific skills or subject areas, so that I can find a suitable peer tutor or study partner.

**Why this priority**: Core value proposition. Students need a direct, fast way to search and find other students who teach what they want to learn.

**Independent Test**: Can be tested with seeded student profiles by searching for "Next.js" or filtering by category and confirming only matching students appear with their teachable topics and rating summaries.

**Acceptance Scenarios**:
1. **Given** a directory of student profiles, **When** a user types "Python" into the search bar, **Then** the list filters to show students who have listed "Python" under skills they can teach.
2. **Given** search results, **When** a student selects a category filter (e.g., "Web Development"), **Then** the results narrow down to tutors offering skills within that category.
3. **Given** an empty search result (no tutors found for a query), **When** search yields zero matches, **Then** the system displays a clear message suggesting related skills or inviting them to post a learning request.

---

### User Story 3 - Session Booking Requests & Management (Priority: P2)

As a learner, I want to request a study or mentorship session with a specific tutor for a given topic, date, and time, and manage my bookings, so that we can coordinate our study meeting.

**Why this priority**: Enables students to convert mutual interest into an actionable session with agreed time and topic.

**Independent Test**: Can be tested by having Student A submit a booking request to Student B, Student B accepting or declining it, and both checking their session lists.

**Acceptance Scenarios**:
1. **Given** a tutor's profile page, **When** a student clicks "Request Session", selects an offered skill, specifies a proposed date/time, and provides notes, **Then** a new session request with status "Pending" is created.
2. **Given** a tutor with a pending session request, **When** the tutor accepts the request, **Then** the session status transitions to "Confirmed" and shows up in both students' schedules.
3. **Given** an existing session (pending or confirmed), **When** either student cancels the session with a reason, **Then** the status changes to "Cancelled" and both participants are notified.

---

### User Story 4 - Student Dashboard (Priority: P2)

As an active student (tutor or learner), I want a centralized dashboard showing my upcoming confirmed sessions, pending incoming/outgoing requests, and active learning goals, so that I stay organized.

**Why this priority**: Provides daily operational utility, keeping students informed of their commitments and session statuses.

**Independent Test**: Can be tested by logging into an account with known active sessions and verifying all pending requests, confirmed upcoming sessions, and recent session history are accurately grouped.

**Acceptance Scenarios**:
1. **Given** an authenticated student navigating to their dashboard, **When** the page loads, **Then** they see their upcoming confirmed sessions sorted chronologically by date/time.
2. **Given** incoming pending requests from other students, **When** the tutor views the dashboard, **Then** quick action buttons ("Accept", "Decline") are presented for each pending request.
3. **Given** completed sessions in the past, **When** viewed on the dashboard, **Then** sessions prompt the learner with a "Leave Review" action if not yet reviewed.

---

### User Story 5 - Reviews & Ratings (Priority: P3)

As a student who completed a study session, I want to leave a rating and review for my peer tutor, and edit it if needed, so that high-quality tutors are recognized and peers can make informed choices.

**Why this priority**: Fosters trust, community safety, and peer accountability, but relies on sessions having already occurred.

**Independent Test**: Can be tested after a session is marked as "Completed", submitting a 1-5 star rating and written review, verifying it appears on the tutor's profile, and updating the review.

**Acceptance Scenarios**:
1. **Given** a completed session, **When** the learner submits a 5-star rating and a review comment, **Then** the review is attached to the session and published to the tutor's profile.
2. **Given** a published review, **When** the author edits the review content or score within the allowed editing window, **Then** the tutor's updated average rating is recalculated.
3. **Given** a tutor viewing their own profile or dashboard, **When** new reviews are posted, **Then** they can view aggregate rating metrics and feedback.

---

### Edge Cases

- **Time zone differences**: How does the system handle students in different time zones booking sessions? (Sessions must store UTC and display in the user's localized time).
- **Double booking**: What happens if a tutor receives two overlapping session requests? (System alerts the tutor to schedule conflicts before accepting).
- **Mutual tutoring**: Can Student A teach Student B Python while Student B teaches Student A Design? (Yes, roles are per-session, not per-user).
- **Self-booking**: Can a student request a session with themselves? (System strictly prevents self-booking).
- **Inactive / Deleted profiles**: How are pending bookings handled if a user deactivates their account? (Active bookings are automatically cancelled with notice).

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: System MUST allow students to create, authenticate (login/logout), and manage accounts with email/password or institutional credentials.
- **FR-002**: System MUST allow each student to maintain a profile with bio, avatar/photo, contact preference, university/major, "Skills Offered" (teachable), and "Skills Desired" (learning).
- **FR-003**: System MUST provide a searchable and filterable directory of tutors, filtering by skill name, topic category, and availability.
- **FR-004**: System MUST allow students to initiate session booking requests specifying target tutor, skill/topic, proposed timestamp, duration, and optional session notes.
- **FR-005**: System MUST allow tutors to accept or decline incoming session requests.
- **FR-006**: System MUST allow either participant to cancel a pending or confirmed session prior to its start time.
- **FR-007**: System MUST provide a student dashboard displaying upcoming confirmed sessions, pending requests (incoming and outgoing), and past sessions.
- **FR-008**: System MUST allow learners to submit a rating (1-5 stars) and a textual review for completed sessions.
- **FR-009**: System MUST calculate and display aggregate rating scores and feedback count on tutor profiles.
- **FR-010**: System MUST enforce that reviews can only be submitted by participants of a confirmed and completed session.

### Key Entities *(include if feature involves data)*

- **Student / User**: Represents a student user. Attributes: ID, full name, email, avatar URL, bio, university/department, timestamps.
- **Skill**: Represents a standardized or custom subject. Attributes: ID, name, category (e.g., Programming, Design, Math).
- **UserSkill**: Join entity linking Student and Skill. Attributes: User ID, Skill ID, type (`OFFERED` vs `DESIRED`), proficiency/experience level.
- **SessionRequest / Booking**: Represents a peer tutoring session. Attributes: ID, requester ID, tutor ID, skill ID, scheduled start time, duration, status (`PENDING`, `CONFIRMED`, `DECLINED`, `CANCELLED`, `COMPLETED`), meeting notes/link.
- **Review**: Represents feedback after a session. Attributes: ID, session ID, reviewer ID, reviewee ID, rating (1-5), comments, timestamps.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Students can register and complete their profile (including adding at least 1 offered skill and 1 desired skill) in under 3 minutes.
- **SC-002**: Directory search and skill filtering returns matching tutor profiles in under 1 second for standard queries.
- **SC-003**: A student can initiate a session booking request in fewer than 4 clicks from a tutor's profile.
- **SC-004**: Over 90% of test user workflows (create profile → search tutor → request session → accept session → review session) complete without navigation or validation errors.
- **SC-005**: Dashboard correctly categorizes 100% of session requests into their appropriate statuses without page reload desync.
