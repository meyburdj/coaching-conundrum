# Coaching Conundrum

Coaching Conundrum features a backend written in Python using Flask, and a frontend written in TypeScript using React with the Next.js App directory. It utilizes a RESTful API and a PostgreSQL database schema, both documented below. The frontend employs server-side rendering and server components, alongside client components and client-side fetching, to provide a user-friendly interface. This setup allows coaches and students to perform a wide range of interactive CRUD operations on a booking calendar. Below is a component tree representing the main frontend components, followed by detailed backend database schema and API documentation.

## Component Tree
 ![Component Tree for Frontend](component_tree.drawio.svg)

# Coaching Conundrum Backend

## Models

### User
- `id`: Integer, Primary Key
- `name`: String
- `phone_number`: String
- `role`: String ('student' or 'coach')

### Session
- `id`: Integer, Primary Key
- `coach_id`: Integer, Foreign Key (Users)
- `start_time`: Datetime
- `student_id`: Integer, Foreign Key (Users, Nullable)

### SessionReview
- `id`: Integer, Primary Key
- `session_id`: Integer, Foreign Key (Session)
- `satisfaction_score`: Integer
- `notes`: Text

## API Endpoints

### User Endpoints

- **GET /user/id**
  - Reads record for User with the corresponding id. Used to retrieve `phone_number`.

- **POST /user**
  - Creates a new User.

### Session Endpoints

- **GET /sessions**
  - Optional param `selected_time`: Gives all available sessions for that month.
  - Optional param `available=true` or `false`: Gives only available or booked sessions based on the presence of `student_id`.

- **POST /session**
  - Creates a Session record. Can only be done if request is made with `id` corresponding to a coach.

- **GET /session/id**
  - Reads the record with the corresponding `id`.

- **PATCH /session/id**
  - Updates the record to have `student_id`.

- **DELETE /session/id**
  - Deletes the record associated with that `session_id`.

### SessionReview Endpoints

- **POST /session/id/review**
  - Creates a new SessionReview record. Can only be made by the coach associated with that `session_id`.

### Coach Endpoints

- **GET /session/coach/id**
  - Reads a list of all sessions where `id` is equal to the `coach_id`.
  - Joins `Session.id` with `SessionReview.session_id`.

### Student Endpoints

- **GET /session/student/id**
  - Reads a list of all sessions where `id` is equal to the `student_id`.

