Comrades Marathon API - README
Project Overview
The Comrades Marathon API is a comprehensive backend system designed to manage marathon events, user registrations, enrollments, results, and payments. Built with ASP.NET Core, this RESTful API provides a robust platform for event organizers, participants, and administrators.

Technology Stack
Framework: ASP.NET Core

Authentication: JWT (JSON Web Tokens)

Database: SQL Server

API Style: RESTful

Architecture: Repository Pattern

Database Schema
Entity Relationship Diagram (ERD)
The system consists of the following main entities:

Core Entities
User - System users (organizers, participants, admins)

Event - Marathon events created by organizers

Category - Race categories within events (e.g., 5km, 10km, half-marathon)

Enrollment - Junction table linking participants to categories (many-to-many)

Result - Race results linked to enrollments

Payment - Payment records linked to enrollments

Relationships
User → Event: One-to-Many (One organizer creates many events)

Event → Category: One-to-Many (One event has multiple categories)

User → Enrollment → Category: Many-to-Many (Participants can enroll in multiple categories)

Enrollment → Result: One-to-One (One result per enrollment)

Enrollment → Payment: One-to-One (One payment per enrollment)

API Endpoints
🔐 Authentication
Method	Endpoint	Description	Role
POST	/api/auth/login	Authenticate user and return JWT token	Public
POST	/api/auth/signin	Register new user account	Public
👤 User Accounts
Method	Endpoint	Description	Role
GET	/api/users/email	Get current authenticated user's profile	Any (Logged in)
PUT	/api/users/profile	Update current user's profile	Any (Logged in)
GET	/api/users/{id}	Get user's public profile	Any (Logged in)
GET	/api/users	List all users (Admin only)	Admin
📅 Events
Method	Endpoint	Description	Role
GET	/api/events	List all events with filtering and pagination	Public
GET	/api/events/{id}	Get detailed event information	Public
POST	/api/events	Create a new event	Organizer
PUT	/api/events/{id}	Update an existing event	Organizer (Owner only)
DELETE	/api/events/{id}	Delete an event	Organizer (Owner only)
🏷️ Categories
Method	Endpoint	Description	Role
GET	/api/events/{eventId}/categories	Get all categories for an event	Public
POST	/api/events/{eventId}/categories	Add a new category to an event	Organizer
PUT	/api/categories/{id}	Update a category	Organizer
DELETE	/api/categories/{id}	Delete a category	Organizer
📝 Enrollments
Method	Endpoint	Description	Role
GET	/api/events/{eventId}/enrolments	Get all enrollments for an event	Organizer
POST	/api/events/{eventId}/enrol	Enroll participant in an event	Any (Logged in)
GET	/api/users/enrolments	Get current user's enrollments	Any (Logged in)
PUT	/api/enrolments/{id}	Update enrollment status	Participant/Organizer
DELETE	/api/enrolments/{id}	Cancel enrollment	Participant/Organizer
🏆 Results
Method	Endpoint	Description	Role
GET	/api/events/{eventId}/results	Get all results for an event	Public
GET	/api/enrolments/{id}/result	Get results for a specific enrollment	Participant/Organizer
POST	/api/enrolments/{id}/result	Add/record result for an enrollment	Organizer
PUT	/api/results/{id}	Update an existing result	Organizer
DELETE	/api/results/{id}	Delete a result	Organizer
📊 Dashboard
Method	Endpoint	Description	Role
GET	/api/dashboard/organiser	Get organizer's event statistics	Organizer
GET	/api/dashboard/participant	Get participant's race statistics	Any (Logged in)
Authentication & Authorization
JWT Authentication
The API uses JWT tokens for authentication. Include the token in the Authorization header:

text
Authorization: Bearer <your-jwt-token>
Role-Based Access
Public: No authentication required

Any (Logged in): Requires valid JWT token

Organizer: Requires Organizer role

Admin: Requires Admin role

Organizer (Owner only): Must be the event/category creator
