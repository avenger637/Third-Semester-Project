# EventSphere — Database Schema

**Database:** Supabase (PostgreSQL)  
**Application:** Java (JDBC)  
**Purpose:** A general-purpose Event Management System with a shared core and optional modules for weddings, conferences, workshops, seminars, sports tournaments, and other event types.

> **Design principle:** Keep shared event data in common tables. Store specialized data in module-specific tables. An event can enable any combination of modules, regardless of its template.

---

## 1. Design Overview

### Core functionality

- Event creation and management
- Event types and reusable templates
- Optional modules per event
- Organizers and event assignments
- Tasks, deadlines, and task dependencies
- Budgets and expenses
- Venues and scheduled activities
- Participants, registrations, guests, and attendance
- Vendors and services
- Speakers and sessions
- Sports teams, matches, and results
- Feedback and reporting

### Relationship summary

```text
users ──< event_organizers >── events ──< event_enabled_modules >── modules
                                  │
                                  ├──< tasks ──< task_dependencies >── tasks
                                  ├──< expenses
                                  ├──< event_venues >── venues
                                  ├──< activities
                                  ├──< registrations >── participants
                                  ├──< guests
                                  ├──< event_vendors >── vendors
                                  ├──< event_speakers >── speakers
                                  ├──< sessions
                                  ├──< teams ──< team_members >── participants
                                  ├──< matches
                                  ├──< attendance
                                  └──< feedback
```

The specialized tables are optional in practice: they are populated only when the relevant functionality is used.

---

## 2. Conventions

- Use `BIGINT GENERATED ALWAYS AS IDENTITY` for straightforward relational IDs, or UUIDs if you prefer globally unique identifiers. This schema uses `BIGINT` for readability.
- Store dates and times as `TIMESTAMPTZ` so time zones are handled correctly.
- Store monetary values as `NUMERIC(12,2)`, not floating-point types.
- Use foreign keys, `NOT NULL`, `CHECK`, and `UNIQUE` constraints to protect data integrity.
- Keep passwords out of the database in plaintext. If using Supabase Auth, use its authentication system rather than implementing password storage yourself.
- For a Java desktop application, **never include the Supabase `service_role` key in the client**. Keep secrets out of source control and configure appropriate database permissions.
- Use transactions for operations that change multiple related rows, such as creating an event and enabling its modules or promoting someone from a waiting list.

---

## 3. Core Tables

### 3.1 `users`

Application profile for an authenticated user. If Supabase Auth is used, `auth_user_id` should reference the user's ID in `auth.users`.

| Column | Type | Rules | Description |
|---|---|---|---|
| `user_id` | BIGINT | PK, identity | Internal profile ID |
| `auth_user_id` | UUID | UNIQUE, nullable | Supabase Auth user ID |
| `full_name` | TEXT | NOT NULL | User's name |
| `email` | TEXT | NOT NULL, UNIQUE | Contact email |
| `phone` | TEXT | nullable | Contact number |
| `created_at` | TIMESTAMPTZ | NOT NULL, default `now()` | Creation timestamp |

**Notes:** Do not treat a role value supplied by the client as proof of authorization. Enforce access rules through trusted application logic and database policies.

### 3.2 `event_types`

Categories such as Wedding, Conference, Workshop, Seminar, Sports Tournament, and Custom.

| Column | Type | Rules | Description |
|---|---|---|---|
| `event_type_id` | BIGINT | PK, identity | Type ID |
| `name` | TEXT | NOT NULL, UNIQUE | Type name |
| `description` | TEXT | nullable | Type description |
| `is_active` | BOOLEAN | NOT NULL, default `TRUE` | Whether users can select it |

### 3.3 `events`

The central table. Every event type has one row here.

| Column | Type | Rules | Description |
|---|---|---|---|
| `event_id` | BIGINT | PK, identity | Event ID |
| `event_type_id` | BIGINT | FK → `event_types.event_type_id`, NOT NULL | Selected category |
| `name` | TEXT | NOT NULL | Event name |
| `description` | TEXT | nullable | Event description |
| `start_time` | TIMESTAMPTZ | NOT NULL | Event start |
| `end_time` | TIMESTAMPTZ | NOT NULL | Event end |
| `location_text` | TEXT | nullable | General location/address |
| `expected_attendees` | INTEGER | CHECK `>= 0`, nullable | Estimated attendance |
| `budget_limit` | NUMERIC(12,2) | CHECK `>= 0`, nullable | Planned budget |
| `status` | TEXT | NOT NULL, default `'planning'` | `planning`, `published`, `active`, `completed`, `cancelled` |
| `created_by` | BIGINT | FK → `users.user_id`, NOT NULL | User who created it |
| `created_at` | TIMESTAMPTZ | NOT NULL, default `now()` | Creation timestamp |
| `updated_at` | TIMESTAMPTZ | NOT NULL, default `now()` | Last update timestamp |

**Validation:** `end_time > start_time`. Add a database check constraint for this.

### 3.4 `event_organizers`

Many-to-many relationship between users and events. An event may have multiple organizers, and a user may organize multiple events.

| Column | Type | Rules | Description |
|---|---|---|---|
| `event_id` | BIGINT | FK → `events.event_id`, PK part | Event |
| `user_id` | BIGINT | FK → `users.user_id`, PK part | Organizer |
| `organizer_role` | TEXT | NOT NULL, default `'organizer'` | `owner`, `organizer`, `viewer` |
| `assigned_at` | TIMESTAMPTZ | NOT NULL, default `now()` | Assignment timestamp |

**Primary key:** (`event_id`, `user_id`)

### 3.5 `modules`

Catalog of optional functionality that can be enabled for events.

| Column | Type | Rules | Description |
|---|---|---|---|
| `module_id` | BIGINT | PK, identity | Module ID |
| `name` | TEXT | NOT NULL, UNIQUE | Stable name, e.g. `guest_management` |
| `display_name` | TEXT | NOT NULL | UI label |
| `description` | TEXT | nullable | Module purpose |
| `is_active` | BOOLEAN | NOT NULL, default `TRUE` | Whether module is available |

Suggested modules: `registration`, `guest_management`, `seating`, `vendors`, `speakers`, `agenda`, `attendance`, `feedback`, `teams`, `competition`.

### 3.6 `event_enabled_modules`

Stores the modules enabled for each event.

| Column | Type | Rules | Description |
|---|---|---|---|
| `event_id` | BIGINT | FK → `events.event_id`, PK part | Event |
| `module_id` | BIGINT | FK → `modules.module_id`, PK part | Module |
| `enabled_at` | TIMESTAMPTZ | NOT NULL, default `now()` | When enabled |

**Primary key:** (`event_id`, `module_id`)

An event template should suggest a default module set; users should still be able to enable or disable modules individually.

### 3.7 `event_templates`

Reusable starting configurations such as Wedding, Conference, Workshop, and Sports Tournament.

| Column | Type | Rules | Description |
|---|---|---|---|
| `template_id` | BIGINT | PK, identity | Template ID |
| `name` | TEXT | NOT NULL, UNIQUE | Template name |
| `description` | TEXT | nullable | Template description |
| `event_type_id` | BIGINT | FK → `event_types.event_type_id`, nullable | Suggested event type |
| `is_active` | BOOLEAN | NOT NULL, default `TRUE` | Whether template is available |

### 3.8 `template_modules`

Modules suggested by each template.

| Column | Type | Rules | Description |
|---|---|---|---|
| `template_id` | BIGINT | FK → `event_templates.template_id`, PK part | Template |
| `module_id` | BIGINT | FK → `modules.module_id`, PK part | Suggested module |
| `is_default` | BOOLEAN | NOT NULL, default `TRUE` | Whether enabled by default |

**Primary key:** (`template_id`, `module_id`)

---

## 4. Tasks, Dependencies, and Scheduling

### 4.1 `tasks`

General-purpose tasks for any event.

| Column | Type | Rules | Description |
|---|---|---|---|
| `task_id` | BIGINT | PK, identity | Task ID |
| `event_id` | BIGINT | FK → `events.event_id`, NOT NULL | Parent event |
| `title` | TEXT | NOT NULL | Task title |
| `description` | TEXT | nullable | Details |
| `assigned_to` | BIGINT | FK → `users.user_id`, nullable | Responsible user |
| `deadline` | TIMESTAMPTZ | nullable | Due date |
| `priority` | TEXT | NOT NULL, default `'medium'` | `low`, `medium`, `high`, `urgent` |
| `status` | TEXT | NOT NULL, default `'pending'` | `pending`, `in_progress`, `completed`, `cancelled` |
| `created_at` | TIMESTAMPTZ | NOT NULL, default `now()` | Creation timestamp |

### 4.2 `task_dependencies`

Directed dependency from one task to another: `task_id` cannot begin until `depends_on_task_id` is complete.

| Column | Type | Rules | Description |
|---|---|---|---|
| `task_id` | BIGINT | FK → `tasks.task_id`, PK part | Dependent task |
| `depends_on_task_id` | BIGINT | FK → `tasks.task_id`, PK part | Prerequisite task |

**Primary key:** (`task_id`, `depends_on_task_id`)

**Validation:** Prevent a task from depending on itself. Application logic should also prevent cycles. If dependencies must be within the same event, validate that rule in the service layer or with a database trigger.

### 4.3 `venues`

Reusable venue records.

| Column | Type | Rules | Description |
|---|---|---|---|
| `venue_id` | BIGINT | PK, identity | Venue ID |
| `name` | TEXT | NOT NULL | Venue name |
| `address` | TEXT | nullable | Address |
| `capacity` | INTEGER | CHECK `> 0`, nullable | Maximum capacity |
| `contact_name` | TEXT | nullable | Venue contact |
| `contact_phone` | TEXT | nullable | Contact number |
| `notes` | TEXT | nullable | Additional details |

### 4.4 `event_venues`

Associates events with venues. Supports events using multiple venues and venues reused across events.

| Column | Type | Rules | Description |
|---|---|---|---|
| `event_id` | BIGINT | FK → `events.event_id`, PK part | Event |
| `venue_id` | BIGINT | FK → `venues.venue_id`, PK part | Venue |
| `usage_notes` | TEXT | nullable | Purpose or arrangement |

**Primary key:** (`event_id`, `venue_id`)

### 4.5 `activities`

Scheduled activities within an event, such as a talk, ceremony, match, meal, or workshop segment.

| Column | Type | Rules | Description |
|---|---|---|---|
| `activity_id` | BIGINT | PK, identity | Activity ID |
| `event_id` | BIGINT | FK → `events.event_id`, NOT NULL | Parent event |
| `venue_id` | BIGINT | FK → `venues.venue_id`, nullable | Venue, if applicable |
| `title` | TEXT | NOT NULL | Activity title |
| `description` | TEXT | nullable | Details |
| `start_time` | TIMESTAMPTZ | NOT NULL | Start |
| `end_time` | TIMESTAMPTZ | NOT NULL | End |
| `activity_type` | TEXT | nullable | E.g. talk, ceremony, meal, match |
| `status` | TEXT | NOT NULL, default `'planned'` | `planned`, `confirmed`, `completed`, `cancelled` |

**Validation:** `end_time > start_time`. Detect venue/time overlaps in application logic or with appropriate PostgreSQL exclusion constraints if needed.

---

## 5. Participant Registration, Guests, and Attendance

These concepts are related but not identical. Registrations are suitable for events with sign-ups; guests are useful for invitation/RSVP workflows such as weddings. An event may use one or both.

### 5.1 `participants`

People who may participate in events. A participant record can be reused across multiple events.

| Column | Type | Rules | Description |
|---|---|---|---|
| `participant_id` | BIGINT | PK, identity | Participant ID |
| `full_name` | TEXT | NOT NULL | Name |
| `email` | TEXT | nullable | Email |
| `phone` | TEXT | nullable | Phone |
| `created_at` | TIMESTAMPTZ | NOT NULL, default `now()` | Creation timestamp |

A unique email constraint is optional because some people may not have email addresses, and real-world data may need normalization.

### 5.2 `registrations`

Tracks participant registration for an event.

| Column | Type | Rules | Description |
|---|---|---|---|
| `registration_id` | BIGINT | PK, identity | Registration ID |
| `event_id` | BIGINT | FK → `events.event_id`, NOT NULL | Event |
| `participant_id` | BIGINT | FK → `participants.participant_id`, NOT NULL | Participant |
| `registered_at` | TIMESTAMPTZ | NOT NULL, default `now()` | Registration time |
| `status` | TEXT | NOT NULL, default `'registered'` | `registered`, `waitlisted`, `cancelled` |
| `notes` | TEXT | nullable | Optional notes |

**Constraint:** `UNIQUE (event_id, participant_id)` prevents duplicate registrations.

**Capacity rule:** Registration count and capacity checks must be performed transactionally to avoid overbooking when concurrent registrations occur.

### 5.3 `guests`

Invitation and RSVP tracking, especially for weddings and formal events.

| Column | Type | Rules | Description |
|---|---|---|---|
| `guest_id` | BIGINT | PK, identity | Guest ID |
| `event_id` | BIGINT | FK → `events.event_id`, NOT NULL | Event |
| `full_name` | TEXT | NOT NULL | Guest name |
| `email` | TEXT | nullable | Email |
| `phone` | TEXT | nullable | Phone |
| `invitation_status` | TEXT | NOT NULL, default `'pending'` | `pending`, `sent`, `accepted`, `declined` |
| `plus_one_allowed` | BOOLEAN | NOT NULL, default `FALSE` | Whether guest may bring another person |
| `notes` | TEXT | nullable | Dietary or organizer notes; avoid storing unnecessary sensitive details |

### 5.4 `attendance`

Records attendance for registered participants. If attendance for guests is required, extend the model or add a guest-specific attendance table rather than using ambiguous IDs.

| Column | Type | Rules | Description |
|---|---|---|---|
| `attendance_id` | BIGINT | PK, identity | Attendance record ID |
| `event_id` | BIGINT | FK → `events.event_id`, NOT NULL | Event |
| `participant_id` | BIGINT | FK → `participants.participant_id`, NOT NULL | Participant |
| `attended` | BOOLEAN | NOT NULL, default `FALSE` | Whether present |
| `checked_in_at` | TIMESTAMPTZ | nullable | Check-in time |

**Constraint:** `UNIQUE (event_id, participant_id)`.

---

## 6. Budget, Expenses, and Vendors

### 6.1 `expenses`

Records expenses against an event.

| Column | Type | Rules | Description |
|---|---|---|---|
| `expense_id` | BIGINT | PK, identity | Expense ID |
| `event_id` | BIGINT | FK → `events.event_id`, NOT NULL | Event |
| `category` | TEXT | NOT NULL | E.g. venue, catering, equipment |
| `description` | TEXT | nullable | Expense details |
| `amount` | NUMERIC(12,2) | NOT NULL, CHECK `>= 0` | Amount |
| `payment_status` | TEXT | NOT NULL, default `'pending'` | `pending`, `paid`, `cancelled` |
| `expense_date` | DATE | nullable | Date incurred |
| `created_at` | TIMESTAMPTZ | NOT NULL, default `now()` | Creation timestamp |

### 6.2 `vendors`

Reusable vendor/contact records.

| Column | Type | Rules | Description |
|---|---|---|---|
| `vendor_id` | BIGINT | PK, identity | Vendor ID |
| `name` | TEXT | NOT NULL | Vendor/business name |
| `service_type` | TEXT | nullable | Catering, photography, sound, etc. |
| `contact_name` | TEXT | nullable | Contact person |
| `email` | TEXT | nullable | Email |
| `phone` | TEXT | nullable | Phone |
| `notes` | TEXT | nullable | Additional information |

### 6.3 `event_vendors`

Associates vendors with events and records the agreed service and cost.

| Column | Type | Rules | Description |
|---|---|---|---|
| `event_id` | BIGINT | FK → `events.event_id`, PK part | Event |
| `vendor_id` | BIGINT | FK → `vendors.vendor_id`, PK part | Vendor |
| `service_description` | TEXT | nullable | Service for this event |
| `agreed_cost` | NUMERIC(12,2) | CHECK `>= 0`, nullable | Agreed cost |
| `payment_status` | TEXT | NOT NULL, default `'pending'` | `pending`, `partial`, `paid`, `cancelled` |

**Primary key:** (`event_id`, `vendor_id`)

---

## 7. Conference and Workshop Modules

### 7.1 `speakers`

Speaker/instructor profiles.

| Column | Type | Rules | Description |
|---|---|---|---|
| `speaker_id` | BIGINT | PK, identity | Speaker ID |
| `full_name` | TEXT | NOT NULL | Name |
| `email` | TEXT | nullable | Email |
| `organization` | TEXT | nullable | Affiliation |
| `bio` | TEXT | nullable | Short biography |
| `specialization` | TEXT | nullable | Area of expertise |

### 7.2 `event_speakers`

Connects speakers to events.

| Column | Type | Rules | Description |
|---|---|---|---|
| `event_id` | BIGINT | FK → `events.event_id`, PK part | Event |
| `speaker_id` | BIGINT | FK → `speakers.speaker_id`, PK part | Speaker |
| `role` | TEXT | nullable | Host, speaker, instructor, panelist |

**Primary key:** (`event_id`, `speaker_id`)

### 7.3 `sessions`

Individual sessions within an event, particularly useful for conferences and workshops.

| Column | Type | Rules | Description |
|---|---|---|---|
| `session_id` | BIGINT | PK, identity | Session ID |
| `event_id` | BIGINT | FK → `events.event_id`, NOT NULL | Parent event |
| `title` | TEXT | NOT NULL | Session title |
| `description` | TEXT | nullable | Details |
| `speaker_id` | BIGINT | FK → `speakers.speaker_id`, nullable | Primary speaker/instructor |
| `venue_id` | BIGINT | FK → `venues.venue_id`, nullable | Location |
| `start_time` | TIMESTAMPTZ | NOT NULL | Start |
| `end_time` | TIMESTAMPTZ | NOT NULL | End |
| `capacity` | INTEGER | CHECK `>= 0`, nullable | Session capacity |

**Validation:** `end_time > start_time`. If sessions need multiple speakers, replace the single `speaker_id` relationship with a junction table such as `session_speakers`.

### 7.4 `feedback`

Participant feedback for an event or a specific session. To keep the first version simple, feedback is associated with an event; session-specific feedback can optionally reference a session.

| Column | Type | Rules | Description |
|---|---|---|---|
| `feedback_id` | BIGINT | PK, identity | Feedback ID |
| `event_id` | BIGINT | FK → `events.event_id`, NOT NULL | Event |
| `participant_id` | BIGINT | FK → `participants.participant_id`, NOT NULL | Participant |
| `session_id` | BIGINT | FK → `sessions.session_id`, nullable | Optional session |
| `rating` | INTEGER | NOT NULL, CHECK `BETWEEN 1 AND 5` | Rating |
| `comments` | TEXT | nullable | Comments |
| `submitted_at` | TIMESTAMPTZ | NOT NULL, default `now()` | Submission timestamp |

If each participant may submit only one feedback record per event, add a unique constraint on `(event_id, participant_id)`. If feedback is allowed once per session, use an appropriate uniqueness rule that accounts for event-level versus session-level feedback.

---

## 8. Sports Tournament Module

### 8.1 `teams`

Teams participating in an event.

| Column | Type | Rules | Description |
|---|---|---|---|
| `team_id` | BIGINT | PK, identity | Team ID |
| `event_id` | BIGINT | FK → `events.event_id`, NOT NULL | Tournament event |
| `name` | TEXT | NOT NULL | Team name |
| `seed_number` | INTEGER | CHECK `> 0`, nullable | Optional tournament seed |

**Constraint:** `UNIQUE (event_id, name)`.

### 8.2 `team_members`

Links participants to teams.

| Column | Type | Rules | Description |
|---|---|---|---|
| `team_id` | BIGINT | FK → `teams.team_id`, PK part | Team |
| `participant_id` | BIGINT | FK → `participants.participant_id`, PK part | Member |
| `member_role` | TEXT | nullable | Captain, player, substitute, etc. |

**Primary key:** (`team_id`, `participant_id`)

If a participant must not belong to multiple teams in the same tournament, enforce that rule in application logic or with a suitable database design/constraint.

### 8.3 `matches`

Fixture and match schedule.

| Column | Type | Rules | Description |
|---|---|---|---|
| `match_id` | BIGINT | PK, identity | Match ID |
| `event_id` | BIGINT | FK → `events.event_id`, NOT NULL | Tournament event |
| `round_name` | TEXT | nullable | Group stage, semifinal, final, etc. |
| `team_a_id` | BIGINT | FK → `teams.team_id`, nullable | First team |
| `team_b_id` | BIGINT | FK → `teams.team_id`, nullable | Second team |
| `venue_id` | BIGINT | FK → `venues.venue_id`, nullable | Match venue |
| `scheduled_at` | TIMESTAMPTZ | nullable | Start time |
| `status` | TEXT | NOT NULL, default `'scheduled'` | `scheduled`, `in_progress`, `completed`, `cancelled` |

**Validation:** Team A and Team B must differ when both are assigned. Validate that both teams belong to the same event.

### 8.4 `match_results`

Stores the result of a match.

| Column | Type | Rules | Description |
|---|---|---|---|
| `match_id` | BIGINT | PK, FK → `matches.match_id` | Match |
| `team_a_score` | INTEGER | CHECK `>= 0`, nullable | First team's score |
| `team_b_score` | INTEGER | CHECK `>= 0`, nullable | Second team's score |
| `winner_team_id` | BIGINT | FK → `teams.team_id`, nullable | Winner, if applicable |
| `result_notes` | TEXT | nullable | Draw, walkover, or other result notes |
| `recorded_at` | TIMESTAMPTZ | NOT NULL, default `now()` | Result timestamp |

Validate that the winner is one of the two teams in the match. Not every sport permits a draw, so result rules should be defined by the application or a sport-specific configuration.

---

## 9. Suggested Indexes

PostgreSQL creates indexes for primary keys and unique constraints. Add indexes for frequent filters and joins.

```sql
CREATE INDEX idx_events_start_time ON events (start_time);
CREATE INDEX idx_events_type_status ON events (event_type_id, status);
CREATE INDEX idx_tasks_event_deadline ON tasks (event_id, deadline);
CREATE INDEX idx_tasks_assigned_status ON tasks (assigned_to, status);
CREATE INDEX idx_activities_event_start ON activities (event_id, start_time);
CREATE INDEX idx_registrations_event_status ON registrations (event_id, status);
CREATE INDEX idx_attendance_event ON attendance (event_id);
CREATE INDEX idx_expenses_event ON expenses (event_id);
CREATE INDEX idx_sessions_event_start ON sessions (event_id, start_time);
CREATE INDEX idx_matches_event_schedule ON matches (event_id, scheduled_at);
```

Do not add indexes to every column automatically. Indexes improve some reads but add storage and write overhead.

---

## 10. Useful Constraints and Business Rules

Implement these rules in the database wherever possible, and in the Java service layer where they require multi-row logic.

1. An event's end time must be after its start time.
2. An activity/session's end time must be after its start time.
3. Event budget, expense amounts, and agreed vendor costs cannot be negative.
4. A participant can have only one registration per event.
5. A participant can have only one attendance record per event.
6. A rating must be between 1 and 5.
7. A task cannot depend on itself; task dependencies must not form cycles.
8. An event cannot exceed its capacity due to simultaneous registrations.
9. A match's teams must belong to the same tournament event.
10. A match winner must be one of the participating teams.
11. A venue cannot be double-booked where overlapping bookings are prohibited.
12. A user can manage only events for which they have appropriate permissions.
13. An event's enabled modules should control which relevant functionality is exposed in the UI.
14. Removing a module should not silently delete its historical records. Decide whether to preserve, archive, or explicitly remove data.

For a student project, use ordinary `CHECK`, `UNIQUE`, and foreign-key constraints first. Add triggers or advanced PostgreSQL features only when there is a clear need.

---

## 11. Supabase Security Notes

If the application is used only by a trusted organizer on a local machine, the security setup still matters because the database is hosted online.

- Prefer Supabase Auth for user identity if users sign in.
- Enable and configure Row Level Security (RLS) for tables exposed through Supabase APIs.
- Do not put the `service_role` key in Java source code or a distributed desktop application.
- A direct JDBC connection does not automatically apply Supabase API/RLS behavior in the same way as PostgREST requests; configure database roles and grants appropriately for the chosen connection method.
- Never commit database passwords or keys to Git.
- Use prepared statements for SQL queries.
- Restrict organizer access to events they are permitted to manage.
- Keep personal information to what the event actually needs.

For a larger public-facing system, a backend service between the Java UI and database is safer than distributing database credentials to every client. For a coursework prototype, follow the access model approved by your instructor and document its limitations.

---

## 12. Recommended Implementation Order

To keep the project manageable, implement the schema in stages.

### Stage 1 — Core
- `users`
- `event_types`
- `events`
- `event_organizers`
- `modules`
- `event_enabled_modules`
- `event_templates`
- `template_modules`

### Stage 2 — Shared event operations
- `tasks`
- `task_dependencies`
- `venues`
- `event_venues`
- `activities`
- `expenses`

### Stage 3 — Registration and services
- `participants`
- `registrations`
- `guests`
- `attendance`
- `vendors`
- `event_vendors`

### Stage 4 — Specialized modules
- `speakers`
- `event_speakers`
- `sessions`
- `feedback`
- `teams`
- `team_members`
- `matches`
- `match_results`

You can initially implement only the core and two or three specialized modules. The database is designed to allow later expansion without requiring a separate application for each event type.

---

## 13. DSA Opportunities Supported by This Schema

| DSA concept | Application feature | Persistent data |
|---|---|---|
| Hash table | In-memory lookup by event/task/participant ID | `events`, `tasks`, `participants` |
| Min-heap / priority queue | Retrieve next task by priority and deadline | `tasks` |
| Queue | First-in, first-out registration waiting list | `registrations` |
| Merge Sort | Sort tasks or events by chosen criteria | `tasks`, `events` |
| Graph + topological sort | Task dependency ordering | `tasks`, `task_dependencies` |
| Searching | Search event names, tasks, or participants | Relevant core tables |

The database remains the source of truth. Custom Java data structures should be used where they provide a clear algorithmic demonstration; do not use an in-memory structure as the only place to store important records.

---

## 14. Final Recommendation

This schema is a **proposed starting point**, not a requirement to implement every table immediately. For a third-semester DSA project, start with the core tables, tasks, venues, expenses, registrations, and one or two specialized modules. Test those end-to-end before expanding.

Before creating the final database, agree as a team on:
- Whether users will sign in or only organizers will use the app.
- Whether registration is public or organizer-managed.
- Which three event types/modules must work fully for the demonstration.
- Whether your instructor requires manual implementation of specific data structures.
