# EventSphere — Event Management System
## DSA Semester Project Description

**Project Name:** EventSphere  
**Project Type:** Desktop-Based Event Management System  
**Programming Language:** Java  
**GUI:** Java Swing  
**Database:** PostgreSQL  
**Database Connectivity:** JDBC  
**IDE:** IntelliJ IDEA  

---

## 1. Introduction

EventSphere is a Java desktop application designed to simplify the creation, organization, and management of events. It provides separate interfaces for administrators, organizers, and participants. Each user can access features according to their role, while the system stores application data in a PostgreSQL database.

The project focuses on applying **Data Structures and Algorithms (DSA)** in a practical application. Java data structures and algorithms will be used to manage, search, sort, organize, and process event-related data. PostgreSQL will primarily provide persistent storage and user authentication.

## 2. Project Objectives

- Develop a role-based event management application.
- Apply data structures and algorithms to real-world event operations.
- Provide separate interfaces for Admin, Organizer, and Participant accounts.
- Store application data persistently using PostgreSQL.
- Authenticate users through database-backed login.
- Restrict access so users can only view and manage data permitted by their roles.
- Keep the application practical and manageable within the semester project timeline.

## 3. User Roles and Features

### 3.1 Admin

The Admin manages the overall system and its finances.

- **Create Events:** Create events, assign organizers, and allocate budgets.
- **Dashboard:** View an overview of events and system activity.
- **Finances:** Monitor overall event budgets and expenditures.
- **Organizer Payments:** Manage payments or salaries allocated to organizers.

### 3.2 Organizer

The Organizer manages events assigned to them.

- **Dashboard:** View assigned events and relevant event information.
- **Events:** Manage event date, time, venue, and schedule.
- **Requests:** Submit requests for items or resources required for an event.
- **Transactions:** View event expenditures, such as venue and material costs.
- **Registration Fees:** View collected participant registration fees when applicable.
- **Participants:** View registered participants and cancelled registrations for assigned events.

### 3.3 Participant

The Participant can explore events and manage their registrations.

- **Dashboard:** View available or relevant events.
- **Register:** Register for an event.
- **Registration Management:** View their own registrations and registration status.
- **Registration Fee:** Pay a registration fee when an event requires one.

## 4. Database Design

The application uses PostgreSQL for persistent storage and authentication. The database stores users, role profiles, events, budgets, requests, registrations, payments, and event expenses.

The main entities include:

- `Users`
- `Admin_Profiles`
- `Organizer_Profiles`
- `Participant_Profiles`
- `Events`
- `Event_Budget`
- `Event_Requests`
- `Registrations`
- `Payments`
- `Event_Expenses`
- `Organizer_Payments` (for organizer salaries or payments, if included)

The database maintains relationships between users, organizers, events, participants, and financial records. User passwords are stored as secure hashes rather than plain text.

## 5. DSA Focus

DSA is a central part of EventSphere. Data will be loaded from the database into Java structures when needed, processed in Java, and persisted back to PostgreSQL after relevant changes.

The project may use the following structures and algorithms where they provide meaningful functionality:

| Data Structure / Algorithm | Application |
|---|---|
| Linked List | Store and manage collections of events, participants, or registrations in memory |
| Hash Table | Fast lookup of users, events, or registrations using IDs |
| Queue | Process event resource requests in order |
| Sorting Algorithms | Sort events, participants, or transactions by selected fields |
| Searching Algorithms | Search events or participants by name, ID, or other criteria |
| Stack (optional) | Support undo operations or recent action tracking |
| BST (optional) | Maintain ordered data and support structured searching |
| Graph (optional) | Represent event schedule dependencies if required |

The exact structures will be selected according to the final implementation and project time. They will not be added only for demonstration; each structure should support a meaningful feature.

## 6. Authentication and Access Control

EventSphere uses database-backed authentication. During login, the application verifies the user's credentials against the stored user record and establishes an authenticated session.

Access is restricted according to the authenticated user's role:

- **Admin:** Access to system-wide event and financial information permitted by the application.
- **Organizer:** Access only to events assigned to that organizer and their related information.
- **Participant:** Access only to their own registrations and personal information.

Authorization checks are performed in the application service layer, not only by hiding or disabling GUI components. The system does not rely on user IDs supplied by the interface to determine ownership.

## 7. System Architecture

The application follows a layered structure:

1. **Presentation Layer:** Java Swing interfaces for Admin, Organizer, and Participant.
2. **Service Layer:** Handles application rules, authentication, authorization, and event operations.
3. **DSA Layer:** Contains custom data structures and algorithms used to process data in Java.
4. **Repository / Database Layer:** Uses JDBC to retrieve and store data in PostgreSQL.
5. **Database:** Provides persistent storage for application records and user credentials.

This separation keeps the code organized and makes the data structures, algorithms, and business logic easier to test.

## 8. Expected Outcome

The final application will provide a functional event management system with role-based interfaces, database-backed authentication, persistent event records, and basic financial and registration management.

It will also demonstrate how data structures and algorithms can be integrated into a practical Java application rather than relying entirely on database queries for searching, sorting, and processing.

## 9. Scope and Limitations

To keep the project achievable within the semester, EventSphere will focus on its core features:

- User login and role-based access
- Event creation and assignment
- Event scheduling and resource requests
- Participant registration
- Basic budgets, expenses, and payments
- Java-based data processing using selected DSA concepts

Advanced features such as online payment gateway integration, notifications, complex analytics, and extensive automation are outside the initial scope unless time permits.

---

**Project:** EventSphere — Event Management System  
**Course:** Data Structures and Algorithms  
**Implementation:** Java Swing + PostgreSQL + JDBC
