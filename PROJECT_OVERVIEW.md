# Asset Manager - Project Overview and Integration Guide

This document provides a comprehensive overview of the Asset Manager application, the APIs available for integration, and details on the user interface components for both teacher and student functionality. It is designed to help developers extend, integrate, or build mobile/web clients for the system.

---

## 1. Project Summary

**Asset Manager** is a full-stack web application aimed at managing school operations, including students, teachers, classes, attendance, marks, timetables, notices, and analytics. It provides role-based access control with separate dashboards for administrators, teachers, and students.

### Key Features

- User authentication (JWT)
- Role-based dashboards (admin/teacher/student)
- Class management (CRUD)
- Student & teacher profiles
- Subject assignment
- Attendance tracking
- Marks & results
- Timetable configuration and viewing
- Notices & announcements
- Chat system (class & personal)
- Real-time updates via Socket.IO
- Analytics & activity logs
- File upload for study materials

### Technology Stack

- **Backend:** Node.js, Express, TypeScript, MongoDB (Mongoose), Drizzle for database migration, Socket.IO
- **Frontend:** React with Vite, TypeScript, Tailwind CSS (assumed from UI naming), React Router
- **Deployment:** Render (backend), Vercel (frontend), MongoDB Atlas

The project is modular, with controllers/services for each resource and simple fetch/axios API client used by the frontend.

---

## 2. API Overview

All endpoints are prefixed with `/api`. Authentication is required for protected routes.

### 2.1 Authentication

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/auth/register` | POST | No | Register a new user. Body: `{email, password, name, role}` |
| `/auth/login` | POST | No | Login and receive JWT. Body: `{email, password}` |
| `/auth/me` | GET | Yes | Get current authenticated user profile |

### 2.2 Users

- User roles: `admin`, `teacher`, `student`.
- Additional endpoints may exist for admin to manage all users.

### 2.3 Classes

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/classes` | GET | Public | List classes |
| `/classes/:id` | GET | Public | View single class |
| `/classes/:id/with-students` | GET | Public | Class with students list |
| `/classes` | POST | Admin | Create class |
| `/classes/:id` | PUT | Admin | Update class |
| `/classes/:id` | DELETE | Admin | Delete class |

### 2.4 Students

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/students` | GET | Public | List students |
| `/students/:id` | GET | Public | Get student by ID |
| `/students/class/:classId` | GET | Public | Students by class |
| `/students` | POST | Admin | Add student (with password) |
| `/students/:id` | PUT | Admin/Student? | Update student |
| `/students/:id` | DELETE | Admin | Delete student |

### 2.5 Teachers

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/teachers` | GET | Public | List teachers |
| `/teachers/:id` | GET | Public | Teacher by ID |
| `/teachers` | POST | Admin | Create teacher |
| `/teachers/:id` | PUT | Admin | Update teacher |
| `/teachers/:id` | DELETE | Admin | Delete teacher |
| `/teachers/:id/assign-subject` | POST | Admin | Assign subject |
| `/teachers/:id/assign-class` | POST | Admin | Assign class |

### 2.6 Subjects

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/subjects` | GET | Public | List subjects (filter by class?) |
| `/subjects/:id` | GET | Public | View subject |
| `/subjects` | POST | Admin | Create subject |
| `/subjects/:id` | PUT | Admin | Update subject |
| `/subjects/:id` | DELETE | Admin | Delete subject |

### 2.7 Marks / Results

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/marks` | GET | Auth | All marks |
| `/marks/student/:studentId` | GET | Auth | Marks for student |
| `/marks/average/:studentId` | GET | Auth | Average score |
| `/marks/class/:classId` | GET | Auth | Class performance |
| `/marks` | POST | Admin/Teacher | Create marks record |
| `/marks/:id` | PUT | Admin/Teacher | Update marks |
| `/marks/:id` | DELETE | Admin/Teacher | Delete record |

### 2.8 Attendance

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/attendance` | GET | Auth | All attendance records |
| `/attendance/student/:studentId` | GET | Auth | Student attendance |
| `/attendance/percentage/:studentId` | GET | Auth | Attendance percentage |
| `/attendance` | POST | Admin/Teacher | Create attendance |
| `/attendance/:id` | PUT | Admin/Teacher | Update attendance |
| `/attendance/:id` | DELETE | Admin/Teacher | Delete attendance |

Teacher-specific:
| `/teacher/attendance/mark` (POST) |
| `/teacher/attendance/:classId` (GET) |

### 2.9 Timetable

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/timetable` | GET | Auth | All timetable slots |
| `/timetable/class/:classId` | GET | Auth | Class timetable |
| `/timetable` | POST | Admin | Create slot |
| `/timetable/:id` | PUT | Admin | Update slot |
| `/timetable/:id` | DELETE | Admin | Delete slot |

#### Timetable Config
| `/timetable-config/:classId` | GET |
| `/timetable-config` | POST |
| `/timetable-config/:classId` | DELETE |

### 2.10 Notices

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/notices` | GET | Public | List notices |
| `/notices/recent` | GET | Public | Recent notices |
| `/notices/:id` | GET | Public | Single notice |
| `/notices` | POST | Auth | Create notice |
| `/notices/:id` | PUT | Auth | Update notice |
| `/notices/:id` | DELETE | Auth | Delete notice |

### 2.11 Chat

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/chat/class/:classId` | GET | Auth | Class chat history |
| `/chat/class/send` | POST | Auth | Send class message |
| `/chat/personal/:userId` | GET | Auth | Personal chat history |
| `/chat/personal/send` | POST | Auth | Send personal message |

Sockets also support real-time communication:
- Event `joinRoom` with `{room}`
- Event `message` for broadcasting

### 2.12 Teacher APIs

In addition to public endpoints, teachers have specialized paths:

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/teacher/dashboard` | GET | Teacher | Personal dashboard data |
| `/teacher/timetable` | GET | Teacher | Own timetable |
| `/teacher/quiz` | POST/GET/PUT/DELETE | Teacher | Quiz management |
| `/teacher/attendance/mark` | POST |
| `/teacher/punch-in` | POST |
| `/teacher/punch-history` | GET |
| `/teacher/chat/class/:classId` | GET |
| `/teacher/chat/class/send` | POST |
| `/teacher/chat/personal/:userId` | GET |
| `/teacher/chat/personal/send` | POST |
| `/teacher/material/upload` | POST | Form-data upload |
| `/teacher/material/:classId` | GET |
| `/teacher/analytics` | GET |
| `/teacher/result` | POST/PUT |

### 2.13 Student APIs

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/student/dashboard` | GET | Student | Personal dashboard |
| `/student/quizzes` | GET | Student |
| `/student/quiz/submit` | POST |
| `/student/quiz/result/:quizId` | GET |
| `/student/attendance` | GET |
| `/student/results` | GET |
| `/student/timetable` | GET |

### 2.14 Admin APIs

Admins have access to system-wide endpoints such as:

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/admin/teacher-punch-logs` | GET |
| `/admin/analytics` | GET |
| `/admin/activities` | GET/PUT |

### 2.15 Activities & Analytics

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/activities` | GET | Auth | List recent activities |
| `/activities/:id/read` | PUT | Auth | Mark as read |
| `/dashboard/summary` | GET | Auth | Summary data |
| `/dashboard/performance` | GET | Auth | Class performance |
| `/analytics` | GET | Auth | Query by student/teacher |

### 2.16 Additional Resources

- `/acts`: previous placeholder

**Note:** Many endpoints accept query parameters for pagination (`limit`, `skip`), sorting, and filtering.

---

## 3. UI Overview

The frontend is built using React with the following structure:

### Pages

- `Dashboard.tsx` – main dashboard showing statistics
- `LoginPage.tsx` – login form
- `AnalyticsPage.tsx` – detailed charts
- `StudentsPage.tsx` – manage students (list, add, edit)
- `TeachersPage.tsx` – manage teachers
- `ClassesPage.tsx` – manage classes
- `SubjectsPage.tsx` – manage subjects
- `TimetablePage.tsx` – view timetable
- `NoticesPage.tsx` – view/post notices
- `ApiExample.tsx` – sample API usage
- `not-found.tsx` – 404 page

Each page fetches data from the corresponding API endpoints via `lib/api.ts`.

### Layouts & Components

- `DashboardLayout.tsx` – main layout with sidebar & header
- `components/dashboard` – various cards/graphs
- `components/shared` – reusable items e.g., `NotificationDropdown`
- `components/ui` – UI primitives (buttons, inputs)

### Hooks

- `use-mobile.tsx` – responsive detection
- `use-toast.tsx` – notification toasts

### Lib

- `api.ts` – simple fetch-based API client with token injection
- `hooks.ts` – global React hooks
- `queryClient.ts` – React Query setup (if used)
- `store.ts` – global state (context or Redux)
- `utils.ts` – utility functions

### Integration Points

- **Authentication:** state stored in `localStorage.token`; API attaches header
- **Chat:** uses Socket.IO; socket initialized in `lib/api` or separate file
- **Analytics:** uses chart libraries for graphs

### UI Flow for Teachers/Students

The UI presents menus based on user role:

- **Dashboard:** summary stats, quick actions
- **Manage Resources:** CRUD forms for students/teachers/classes/subjects
- **Attendance:** take/view attendance; teacher-specific marking interface
- **Quizzes & Results:** create/submit/view quizzes
- **Timetable:** drag/drop configuration (admin) and view (student/teacher)
- **Notices:** post announcements
- **Chat:** class and personal chat windows
- **Profile & Settings:** update user details, view attendance/marks (students)

### UI Notes for Developers

- Components use Tailwind CSS classes (if available) for styling.
- Form validation handled in frontend or via backend responses.
- API call errors are surfaced with toasts.

### Mobile / Responsive

Hook `use-mobile` adapts layout for mobile screens. Sidebars may collapse to top navigation.

---

## 4. Integration Guidance

To integrate this system with another application (mobile app / external UI):

1. **Authentication:**
   - POST to `/auth/login` to obtain JWT.
   - Store token securely on client.
   - Attach `Authorization: Bearer <token>` header to subsequent requests.

2. **API Consumption Patterns:**
   - Use standard REST client (fetch, axios, retrofit, etc.).
   - Follow endpoints outlined in Section 2.
   - Handle errors using the `{success: false, message}` response format.

3. **Real-time Features:**
   - Connect to Socket.IO server at `https://<backend-url>`.
   - `socket.emit('joinRoom', { room })` to join a class or personal room.
   - Listen for `'message'` events.

4. **Role-Based Views:**
   - Inspect `user.role` returned by `/auth/me`.
   - Conditionally render UI based on `admin`, `teacher`, or `student`.

5. **File Uploads:**
   - Use multipart/form-data to `/teacher/material/upload` (requires teacher role).
   - Field names: `file`, `title`, `description`, `classId`.

6. **Pagination & Filtering:**
   - Many GET endpoints support `?limit=20&skip=0&sort=-createdAt` etc.
   - Use `search` or `filter` query parameters where supported.

7. **CORS:**
   - Allowed origins include localhost (for dev) and any `*.vercel.app` domains.

8. **Environment Variables:**
   - FRONTEND_URL - set to your client URL
   - MONGO_URI, JWT_SECRET, NODE_ENV etc.

---

## 5. Extending the Project

### Adding New Features

1. **Backend**: create new controller + service in `backend/src/controllers` and `services`.
   - Add route in `backend/src/routes/*` and register in `routes.ts`.
   - Add Mongoose model as needed in `models/`
   - Update `types/custom.d.ts` if you add new request type.
   - Write unit tests (if present) in `tests/`

2. **Frontend**: create new page under `src/pages`. Add route in router configuration.
   - Create components in appropriate folders.
   - Use `lib/api.ts` to call new APIs and display data.
   - Add new hooks or utilities as needed.

3. **UI**: follow existing styling pattern (Tailwind/utility classes). Use shared components.

### Mobile App

A mobile client can use the REST APIs and Socket.IO. Use React Native, Flutter, or any platform.

- Login -> store JWT
- Use same endpoints but adjust for mobile UI
- Local caching and offline support as needed

---

## 6. Conclusion

The Asset Manager project is a complete school management solution with a clear separation of concerns, a modular codebase, and extensible APIs for multiple frontends. Whether you are building additional features, integrating a mobile app, or customizing the UI, the detailed API reference and UI information above should guide your development.

For any questions or help with extending the system, refer to the repository code or contact the original author.

Happy coding! 🎓
