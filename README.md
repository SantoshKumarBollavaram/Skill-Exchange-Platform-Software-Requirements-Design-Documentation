# Skill-Exchange-Platform-Software-Requirements-Design-Documentation
This use case describes the process of a user finding another member, sending a mentorship request, and the recipient accepting that request to establish an active mentorship connection.
# Skill Exchange Platform — Software Requirements & Design Documentation

**Student Name:** BOLLAVARAM SANTOSH KUMAR  
**SRN:** PES2UG24CS123  
**Section:** B  

---

## 1. Requirements Specification

### Functional Requirements

| ID | Type | Description | Priority | Acceptance Criteria | Rationale |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **FR-001** | Functional | The system shall allow users to create and update profiles listing skills taught, skills wanted, experience level, availability, and preferred interaction mode. | High | A user can successfully save profile details, and the updated skills/availability are immediately searchable by others. | Essential for establishing user identities and making skill-matching possible. |
| **FR-002** | Functional | The system shall enable users to filter and search for other members based on skills, experience level, availability, and preferred interaction mode. | High | Querying with specific filters returns a list of matching active user profiles within 2 seconds. | Core value proposition requires users to efficiently discover compatible mentorship partners. |
| **FR-003** | Functional | The system shall support sending, accepting, and rejecting 1-on-1 mentorship requests. | High | Recipient receives a notification; accepting moves the connection status to "Active" and enables session scheduling. | Establishes formal agreement before scheduling and interaction occur. |
| **FR-004** | Functional | The system shall allow users in an active mentorship connection to schedule sessions, track completion, and submit post-session ratings/feedback. | High | Both participants can pick a time slot, mark a session complete, and submit a 1–5 star rating with text feedback. | Needed to manage engagement logistics and collect feedback data for reputation tracking. |
| **FR-005** | Functional | The system shall calculate and maintain a member reputation score based on post-session ratings and allow users to create/invite members to skill-learning groups. | Medium | A user's public profile displays an updated average rating score after every submitted review, and group owners can send trackable invites. | Promotes accountability, trust, and community scaling via group learning. |

### Non-Functional Requirements

| ID | Type | Description | Priority | Acceptance Criteria | Rationale |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **NFR-001** | Performance & Latency | The search engine shall return filtered user query results within 1.0 second for a database containing up to 100,000 active member profiles. | High | **Pass:** 95th percentile response time for multi-filter queries is under 1.0 second.<br>**Fail:** Search execution exceeds 2.0 seconds under nominal load. | Fast search results ensure high user engagement and smooth matchmaking. |
| **NFR-002** | Security & Data Privacy | The platform shall enforce strict authentication and Role-Based Access Control (RBAC), and encrypt all direct messaging and personal contact details using AES-256 and TLS 1.3. | High | **Pass:** Unauthenticated endpoints return 401 Unauthorized; private user PII is never exposed via public APIs.<br>**Fail:** Contact details or private messages are visible in plaintext network payloads. | Protects member privacy and prevents unsolicited scraping or harassment. |
| **NFR-003** | Availability & Reliability | The platform shall maintain 99.9% uptime during active hours, with automated daily database backups and a Recovery Point Objective (RPO) of less than 15 minutes. | High | **Pass:** System failover to redundant instances completes with zero session loss during outages.<br>**Fail:** Service disruption exceeds 45 minutes in a monthly period. | Users depend on reliable calendar scheduling and access to active mentoring sessions. |
| **NFR-004** | Scalability | The system architecture shall support up to 5,000 concurrent active users and handle at least 200 simultaneous scheduled session handshakes without performance degradation. | Medium | **Pass:** CPU and memory usage remain under 75% during simulated peak traffic spikes.<br>**Fail:** Request timeouts (HTTP 504) occur during high-volume group creation or search events. | Accommodates rapid platform growth and high concurrent traffic during peak evening hours. |
| **NFR-005** | Usability & Responsiveness | The web platform must be fully responsive across desktop, tablet, and mobile browsers, adhering to WCAG 2.1 Level AA accessibility guidelines. | Medium | **Pass:** 95% of first-time users can search for a mentor and dispatch a request within 3 minutes.<br>**Fail:** Layout breaks or becomes unusable on viewport widths below 768px. | Ensures accessibility for diverse learners connecting from mobile devices and varying screen sizes. |

---

## 2. UML Use-Case Diagram

```plantuml
@startuml
left to right direction
skinparam packageStyle rectangle

actor "User" as user
actor "Recipient Member" as recipient
actor "System" as sys

rectangle "Skill Exchange Platform" {
  usecase "Manage Profile" as UC_Profile
  usecase "Search Members" as UC_Search
  usecase "Send Mentorship Request" as UC_Request
  usecase "Respond to Request" as UC_Respond
  usecase "Schedule Mentoring Session" as UC_Schedule
  usecase "Provide Rating & Feedback" as UC_Feedback
  usecase "Update Reputation Score" as UC_Reputation
  usecase "Manage Skill-Learning Groups" as UC_Groups
  usecase "Send Calendar Invite / Notification" as UC_Notify

  user --> UC_Profile
  user --> UC_Search
  user --> UC_Request
  user --> UC_Schedule
  user --> UC_Feedback
  user --> UC_Groups

  recipient --> UC_Respond
  
  UC_Schedule ..> UC_Notify : <<include>>
  UC_Feedback ..> UC_Reputation : <<include>>
  UC_Request ..> UC_Respond : <<extend>>
  
  UC_Reputation --> sys
}
@enduml
