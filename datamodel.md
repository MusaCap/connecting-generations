# Data Model for Bridge – Connecting Generations

## Entities & Relationships

---

## 1. Users

**Table: `Users`**

| Field              | Type        | Description                                     |
|-------------------|-------------|-------------------------------------------------|
| id                | UUID        | Primary key                                     |
| first_name        | String      | First name                                      |
| last_name         | String      | Last name                                       |
| email             | String      | Unique email address                            |
| phone             | String      | Optional, for verification                      |
| password_hash     | String      | Hashed password                                 |
| birth_date        | Date        | Used to calculate age                          |
| age               | Integer     | Derived field                                   |
| location          | String      | City, ZIP or coordinates                        |
| about_me          | Text        | User description or introduction                |
| role              | Enum        | `older_adult`, `younger_adult`                 |
| interests         | Array<Text> | Tags or keywords for matching                   |
| preferred_contact | Enum        | `text`, `video`, `phone`                        |
| is_verified       | Boolean     | Background check passed (if applicable)         |
| created_at        | Timestamp   |                                                 |
| updated_at        | Timestamp   |                                                 |

---

## 2. Matches

**Table: `Matches`**

| Field           | Type      | Description                                      |
|----------------|-----------|--------------------------------------------------|
| id             | UUID      | Primary key                                      |
| user1_id       | UUID      | FK to `Users`                                    |
| user2_id       | UUID      | FK to `Users`                                    |
| match_score    | Float     | Calculated compatibility score                   |
| initiated_by   | UUID      | Who sent the initial request                     |
| status         | Enum      | `pending`, `accepted`, `declined`, `blocked`     |
| created_at     | Timestamp |                                                  |
| updated_at     | Timestamp |                                                  |

---

## 3. Messages

**Table: `Messages`**

| Field         | Type      | Description                                      |
|--------------|-----------|--------------------------------------------------|
| id           | UUID      | Primary key                                      |
| match_id     | UUID      | FK to `Matches`                                  |
| sender_id    | UUID      | FK to `Users`                                    |
| message_type | Enum      | `text`, `image`, `audio`, `video_call_invite`    |
| content      | Text      | Message body or media URL                        |
| created_at   | Timestamp |                                                  |

---

## 4. Activities & Groups

**Table: `Groups`**

| Field        | Type      | Description                                      |
|-------------|-----------|--------------------------------------------------|
| id          | UUID      | Primary key                                      |
| name        | String    | Group or activity name                           |
| description | Text      | Description of the group/activity                |
| category    | String    | e.g., "Book Club", "Tech Help", etc.             |
| created_by  | UUID      | FK to `Users`                                    |
| created_at  | Timestamp |                                                  |

**Table: `GroupMemberships`**

| Field     | Type      | Description                |
|----------|-----------|----------------------------|
| id       | UUID      | Primary key                |
| group_id | UUID      | FK to `Groups`             |
| user_id  | UUID      | FK to `Users`              |
| role     | Enum      | `member`, `moderator`      |

---

## 5. Events

**Table: `Events`**

| Field        | Type      | Description                              |
|-------------|-----------|------------------------------------------|
| id          | UUID      | Primary key                              |
| title       | String    | Event name                               |
| description | Text      | Details about the event                  |
| location    | String    | Physical or virtual (e.g., Zoom link)    |
| event_date  | Timestamp | Scheduled time                           |
| group_id    | UUID      | FK to `Groups` (optional)                |
| created_by  | UUID      | FK to `Users`                            |

**Table: `EventParticipants`**

| Field     | Type      | Description             |
|----------|-----------|-------------------------|
| id       | UUID      | Primary key             |
| event_id | UUID      | FK to `Events`          |
| user_id  | UUID      | FK to `Users`           |
| status   | Enum      | `going`, `maybe`, `declined` |

---

## 6. Reports & Safety

**Table: `Reports`**

| Field        | Type      | Description                              |
|-------------|-----------|------------------------------------------|
| id          | UUID      | Primary key                              |
| reporter_id | UUID      | FK to `Users` (who is reporting)         |
| reported_id | UUID      | FK to `Users` (who is being reported)    |
| reason      | Text      | Free-text reason or incident description |
| type        | Enum      | `harassment`, `spam`, `inappropriate`, etc. |
| status      | Enum      | `open`, `investigating`, `resolved`      |
| created_at  | Timestamp |                                          |

---

## 7. Feedback

**Table: `Feedback`**

| Field       | Type      | Description                      |
|------------|-----------|----------------------------------|
| id         | UUID      | Primary key                      |
| user_id    | UUID      | FK to `Users`                    |
| content    | Text      | Feedback or suggestion text      |
| rating     | Integer   | 1–5 satisfaction rating (optional) |
| created_at | Timestamp |                                  |

---

## Notes

- All `UUID` fields are universally unique identifiers.
- All `created_at` and `updated_at` timestamps are in UTC.
- Indexing should be added to high-read fields (e.g., `user_id`, `match_id`) for performance.
- Consider localization support for future versions.

