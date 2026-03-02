# Asset Manager - Complete API Documentation

**Base URL:** `http://localhost:5001/api`  
**Latest Update:** March 2, 2026

---

## Table of Contents

1. [Authentication APIs](#authentication-apis)
2. [Public APIs (No Auth Required)](#public-apis)
3. [Teacher APIs (Teacher Role Required)](#teacher-apis)
4. [Student APIs (Student Role Required)](#student-apis)
5. [Admin APIs (Admin Role Required)](#admin-apis)
6. [Chat APIs (Authenticated Users)](#chat-apis)

---

## Authentication APIs

### 1. Register User

**Endpoint:** `POST /auth/register`  
**Authentication:** None  
**Description:** Register a new user (student, teacher, or admin)

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "securePassword123",
  "name": "John Doe",
  "role": "student" // or "teacher" or "admin"
}
```

**Response (Success - 201):**
```json
{
  "success": true,
  "data": {
    "_id": "507f1f77bcf86cd799439011",
    "email": "user@example.com",
    "name": "John Doe",
    "role": "student",
    "token": "eyJhbGciOiJIUzI1NiIs..."
  }
}
```

---

### 2. Login User

**Endpoint:** `POST /auth/login`  
**Authentication:** None  
**Description:** Login with email and password to get JWT token

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "securePassword123"
}
```

**Response (Success - 200):**
```json
{
  "success": true,
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIs...",
    "user": {
      "_id": "507f1f77bcf86cd799439011",
      "email": "user@example.com",
      "name": "John Doe",
      "role": "student"
    }
  }
}
```

**Headers Required:**
```
Authorization: Bearer <token>
```

---

### 3. Get Current User

**Endpoint:** `GET /auth/me`  
**Authentication:** Yes (JWT Token)  
**Description:** Get authenticated user's profile information

**Response (Success - 200):**
```json
{
  "success": true,
  "data": {
    "_id": "507f1f77bcf86cd799439011",
    "email": "user@example.com",
    "name": "John Doe",
    "role": "student"
  }
}
```

---

## Public APIs

**Note:** These APIs do NOT require authentication and are accessible to anyone.

### 1. Classes

#### 1.1 Get All Classes
**Endpoint:** `GET /classes`  
**Description:** Retrieve all classes

**Response (Success - 200):**
```json
{
  "success": true,
  "data": [
    {
      "_id": "507f1f77bcf86cd799439011",
      "name": "10",
      "capacity": 20,
      "status": "active",
      "teacherId": "507f1f77bcf86cd799439012"
    }
  ]
}
```

#### 1.2 Get Class by ID
**Endpoint:** `GET /classes/:id`  
**Path Parameters:**
- `id` (required): Class ID

**Response (Success - 200):** Same structure as single class object

#### 1.3 Get Class with Students
**Endpoint:** `GET /classes/:id/with-students`  
**Path Parameters:**
- `id` (required): Class ID

**Response (Success - 200):**
```json
{
  "success": true,
  "data": {
    "_id": "507f1f77bcf86cd799439011",
    "name": "10",
    "students": [
      {
        "_id": "507f1f77bcf86cd799439013",
        "firstName": "Raj",
        "rollNumber": "001"
      }
    ]
  }
}
```

#### 1.4 Create Class
**Endpoint:** `POST /classes`  
**Request Body:**
```json
{
  "name": "10",
  "capacity": 20,
  "teacherId": "507f1f77bcf86cd799439012"
}
```

#### 1.5 Update Class
**Endpoint:** `PUT /classes/:id`  
**Path Parameters:**
- `id` (required): Class ID

**Request Body:** Same as Create

#### 1.6 Delete Class
**Endpoint:** `DELETE /classes/:id`  
**Path Parameters:**
- `id` (required): Class ID

---

### 2. Students

#### 2.1 Get All Students
**Endpoint:** `GET /students`  
**Description:** Retrieve all students

**Response (Success - 200):**
```json
{
  "success": true,
  "data": [
    {
      "_id": "507f1f77bcf86cd799439013",
      "admissionNumber": "1122",
      "firstName": "Raj",
      "lastName": "Kumar",
      "email": "raj@example.com",
      "rollNumber": "001",
      "classId": "507f1f77bcf86cd799439011",
      "status": "active",
      "attendance": 85,
      "averageScore": 75
    }
  ]
}
```

#### 2.2 Get Student by ID
**Endpoint:** `GET /students/:id`  
**Path Parameters:**
- `id` (required): Student ID

#### 2.3 Get Students by Class
**Endpoint:** `GET /students/class/:classId`  
**Path Parameters:**
- `classId` (required): Class ID

**Description:** Get all students belonging to a specific class

#### 2.4 Create Student
**Endpoint:** `POST /students`  
**Request Body:**
```json
{
  "admissionNumber": "1122",
  "firstName": "Raj",
  "lastName": "Kumar",
  "email": "raj@example.com",
  "password": "studentPassword123",
  "rollNumber": "001",
  "classId": "507f1f77bcf86cd799439011",
  "gender": "male",
  "status": "active",
  "academicYear": "2025-26"
}
```

#### 2.5 Update Student
**Endpoint:** `PUT /students/:id`  
**Path Parameters:**
- `id` (required): Student ID

**Request Body:** Same as Create (partial updates allowed)

#### 2.6 Delete Student
**Endpoint:** `DELETE /students/:id`  
**Path Parameters:**
- `id` (required): Student ID

---

### 3. Teachers

#### 3.1 Get All Teachers
**Endpoint:** `GET /teachers`  
**Description:** Retrieve all teachers

#### 3.2 Get Teacher by ID
**Endpoint:** `GET /teachers/:id`  
**Path Parameters:**
- `id` (required): Teacher ID

#### 3.3 Create Teacher
**Endpoint:** `POST /teachers`  
**Request Body:**
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "password": "teacherPassword123",
  "qualification": "B.Tech",
  "experience": 5,
  "status": "active"
}
```

#### 3.4 Update Teacher
**Endpoint:** `PUT /teachers/:id`  
**Path Parameters:**
- `id` (required): Teacher ID

#### 3.5 Delete Teacher
**Endpoint:** `DELETE /teachers/:id`  
**Path Parameters:**
- `id` (required): Teacher ID

#### 3.6 Assign Subject to Teacher
**Endpoint:** `POST /teachers/:id/assign-subject`  
**Path Parameters:**
- `id` (required): Teacher ID

**Request Body:**
```json
{
  "subjectId": "507f1f77bcf86cd799439020"
}
```

#### 3.7 Assign Class to Teacher
**Endpoint:** `POST /teachers/:id/assign-class`  
**Path Parameters:**
- `id` (required): Teacher ID

**Request Body:**
```json
{
  "classId": "507f1f77bcf86cd799439011"
}
```

---

### 4. Subjects

#### 4.1 Get All Subjects
**Endpoint:** `GET /subjects`  
**Query Parameters (Optional):**
- `classId`: Filter subjects by class

#### 4.2 Get Subject by ID
**Endpoint:** `GET /subjects/:id`  
**Path Parameters:**
- `id` (required): Subject ID

#### 4.3 Create Subject
**Endpoint:** `POST /subjects`  
**Request Body:**
```json
{
  "name": "Mathematics",
  "code": "MATH101",
  "classId": "507f1f77bcf86cd799439011",
  "teacherIds": []
}
```

#### 4.4 Update Subject
**Endpoint:** `PUT /subjects/:id`  
**Path Parameters:**
- `id` (required): Subject ID

#### 4.5 Delete Subject
**Endpoint:** `DELETE /subjects/:id`  
**Path Parameters:**
- `id` (required): Subject ID

---

### 5. Marks / Scores

#### 5.1 Get All Marks
**Endpoint:** `GET /marks`  
**Description:** Retrieve all marks records

#### 5.2 Get Marks by Student
**Endpoint:** `GET /marks/student/:studentId`  
**Path Parameters:**
- `studentId` (required): Student ID

**Description:** Get all marks for a specific student

#### 5.3 Get Average Score for Student
**Endpoint:** `GET /marks/average/:studentId`  
**Path Parameters:**
- `studentId` (required): Student ID

**Response (Success - 200):**
```json
{
  "success": true,
  "data": {
    "average": 78.5
  }
}
```

#### 5.4 Get Class Performance
**Endpoint:** `GET /marks/class/:classId`  
**Path Parameters:**
- `classId` (required): Class ID

**Description:** Get performance data for all students in a class

**Response (Success - 200):**
```json
{
  "success": true,
  "data": [
    {
      "studentName": "Raj Kumar",
      "average": 78.5,
      "marks": [85, 92, 65]
    }
  ]
}
```

#### 5.5 Create Marks Record
**Endpoint:** `POST /marks`  
**Request Body:**
```json
{
  "studentId": "507f1f77bcf86cd799439013",
  "subjectId": "507f1f77bcf86cd799439020",
  "classId": "507f1f77bcf86cd799439011",
  "marks": 85,
  "totalMarks": 100,
  "date": "2026-03-02"
}
```

#### 5.6 Update Marks Record
**Endpoint:** `PUT /marks/:id`  
**Path Parameters:**
- `id` (required): Marks ID

**Request Body:** Same as Create (partial updates allowed)

#### 5.7 Delete Marks Record
**Endpoint:** `DELETE /marks/:id`  
**Path Parameters:**
- `id` (required): Marks ID

---

### 6. Attendance

#### 6.1 Get All Attendance Records
**Endpoint:** `GET /attendance`  
**Description:** Retrieve all attendance records

#### 6.2 Get Attendance by Student
**Endpoint:** `GET /attendance/student/:studentId`  
**Path Parameters:**
- `studentId` (required): Student ID

**Description:** Get attendance records for a specific student

#### 6.3 Get Attendance Percentage
**Endpoint:** `GET /attendance/percentage/:studentId`  
**Path Parameters:**
- `studentId` (required): Student ID

**Response (Success - 200):**
```json
{
  "success": true,
  "data": {
    "total": 20,
    "present": 17,
    "percentage": 85
  }
}
```

#### 6.4 Create Attendance Record
**Endpoint:** `POST /attendance`  
**Request Body:**
```json
{
  "classId": "507f1f77bcf86cd799439011",
  "date": "2026-03-02",
  "students": [
    {
      "studentId": "507f1f77bcf86cd799439013",
      "status": "present" // or "absent"
    }
  ]
}
```

#### 6.5 Update Attendance Record
**Endpoint:** `PUT /attendance/:id`  
**Path Parameters:**
- `id` (required): Attendance ID

**Request Body:** Same as Create

#### 6.6 Delete Attendance Record
**Endpoint:** `DELETE /attendance/:id`  
**Path Parameters:**
- `id` (required): Attendance ID

---

### 7. Timetable

#### 7.1 Get All Timetable Slots
**Endpoint:** `GET /timetable`  
**Description:** Retrieve all timetable slots

#### 7.2 Get Timetable by Class
**Endpoint:** `GET /timetable/class/:classId`  
**Path Parameters:**
- `classId` (required): Class ID

#### 7.3 Create Timetable Slot
**Endpoint:** `POST /timetable`  
**Request Body:**
```json
{
  "classId": "507f1f77bcf86cd799439011",
  "day": "Monday",
  "period": 1,
  "subjectId": "507f1f77bcf86cd799439020",
  "teacherId": "507f1f77bcf86cd799439012",
  "startTime": "09:00",
  "endTime": "10:00"
}
```

#### 7.4 Update Timetable Slot
**Endpoint:** `PUT /timetable/:id`  
**Path Parameters:**
- `id` (required): Timetable ID

#### 7.5 Delete Timetable Slot
**Endpoint:** `DELETE /timetable/:id`  
**Path Parameters:**
- `id` (required): Timetable ID

---

### 8. Timetable Configuration

#### 8.1 Get Timetable Config
**Endpoint:** `GET /timetable-config/:classId`  
**Path Parameters:**
- `classId` (required): Class ID

#### 8.2 Save Timetable Config
**Endpoint:** `POST /timetable-config`  
**Request Body:**
```json
{
  "classId": "507f1f77bcf86cd799439011",
  "totalPeriods": 6,
  "periodDuration": 45,
  "breakDuration": 15
}
```

#### 8.3 Delete Timetable Config
**Endpoint:** `DELETE /timetable-config/:classId`  
**Path Parameters:**
- `classId` (required): Class ID

---

### 9. Notices

#### 9.1 Get All Notices
**Endpoint:** `GET /notices`  
**Description:** Retrieve all notices

#### 9.2 Get Recent Notices
**Endpoint:** `GET /notices/recent`  
**Query Parameters:**
- `limit` (optional, default: 10): Number of recent notices to fetch

#### 9.3 Get Notice by ID
**Endpoint:** `GET /notices/:id`  
**Path Parameters:**
- `id` (required): Notice ID

#### 9.4 Create Notice
**Endpoint:** `POST /notices`  
**Request Body:**
```json
{
  "title": "Holiday Announcement",
  "description": "Classes will be suspended on March 8 due to teachers' training.",
  "postedBy": "507f1f77bcf86cd799439012",
  "targetAudience": "all" // or "students", "teachers", "admin"
}
```

#### 9.5 Update Notice
**Endpoint:** `PUT /notices/:id`  
**Path Parameters:**
- `id` (required): Notice ID

#### 9.6 Delete Notice
**Endpoint:** `DELETE /notices/:id`  
**Path Parameters:**
- `id` (required): Notice ID

---

### 10. Analytics & Dashboard

#### 10.1 Get Dashboard Summary
**Endpoint:** `GET /dashboard/summary`  
**Description:** Get overall dashboard statistics

**Response (Success - 200):**
```json
{
  "success": true,
  "data": {
    "totalStudents": 45,
    "totalTeachers": 8,
    "totalClasses": 5,
    "totalNotices": 12,
    "teacherAttendancePercentage": 94,
    "avgPerformance": 82,
    "activities": []
  }
}
```

#### 10.2 Get Performance by Class
**Endpoint:** `GET /dashboard/performance`  
**Description:** Get performance metrics for each class

**Response (Success - 200):**
```json
{
  "success": true,
  "data": [
    {
      "name": "Class 10",
      "score": 78
    },
    {
      "name": "Class 9",
      "score": 82
    }
  ]
}
```

#### 10.3 Get Analytics by Type
**Endpoint:** `GET /analytics`  
**Query Parameters:**
- `type` (required): "teacher" or "student"
- `teacherId` or `studentId` (required, depending on type)

**Example:** `/analytics?type=student&studentId=507f1f77bcf86cd799439013`

**Response (Success - 200):**
```json
{
  "success": true,
  "data": {
    "student": {
      "id": "507f1f77bcf86cd799439013",
      "name": "Raj Kumar",
      "rollNumber": "001"
    },
    "attendance": 85,
    "averagePercentage": 78,
    "subjectPerformance": [],
    "attendanceTrend": []
  }
}
```

---

### 11. Activities

#### 11.1 Get Recent Activities
**Endpoint:** `GET /activities`  
**Query Parameters:**
- `limit` (optional, default: 10): Number of activities to fetch

**Response (Success - 200):**
```json
{
  "success": true,
  "data": [
    {
      "_id": "507f1f77bcf86cd799439030",
      "type": "student_created",
      "title": "New Student Created",
      "description": "Student 'Raj Kumar' has been enrolled",
      "userId": "507f1f77bcf86cd799439012",
      "targetId": "507f1f77bcf86cd799439013",
      "createdAt": "2026-03-02T07:25:46.085Z"
    }
  ]
}
```

#### 11.2 Mark Activity as Read
**Endpoint:** `PUT /activities/:id/read`  
**Path Parameters:**
- `id` (required): Activity ID

---

## Teacher APIs

**Authentication:** Required (JWT Token with `teacher` role)

---

### 1. Teacher Dashboard

**Endpoint:** `GET /teacher/dashboard`  
**Description:** Get teacher's personalized dashboard data

**Response (Success - 200):**
```json
{
  "success": true,
  "data": {
    "name": "John Doe",
    "subjects": [],
    "classes": [],
    "todayCount": 0,
    "attendancePercentage": 0
  }
}
```

---

### 2. Teacher Timetable

**Endpoint:** `GET /teacher/timetable`  
**Description:** Get teacher's timetable

---

### 3. Quiz Management (Teacher)

#### 3.1 Create Quiz
**Endpoint:** `POST /teacher/quiz`  
**Request Body:**
```json
{
  "title": "Math Midterm",
  "classId": "507f1f77bcf86cd799439011",
  "questions": [],
  "duration": 60,
  "totalMarks": 100
}
```

#### 3.2 Get Quizzes by Class
**Endpoint:** `GET /teacher/quiz/:classId`  
**Path Parameters:**
- `classId` (required): Class ID

#### 3.3 Update Quiz
**Endpoint:** `PUT /teacher/quiz/:quizId`  
**Path Parameters:**
- `quizId` (required): Quiz ID

#### 3.4 Delete Quiz
**Endpoint:** `DELETE /teacher/quiz/:quizId`  
**Path Parameters:**
- `quizId` (required): Quiz ID

---

### 4. Mark Attendance (Teacher)

#### 4.1 Mark Attendance
**Endpoint:** `POST /teacher/attendance/mark`  
**Request Body:**
```json
{
  "classId": "507f1f77bcf86cd799439011",
  "date": "2026-03-02",
  "students": [
    {
      "studentId": "507f1f77bcf86cd799439013",
      "status": "present"
    }
  ]
}
```

#### 4.2 Get Attendance by Class
**Endpoint:** `GET /teacher/attendance/:classId`  
**Path Parameters:**
- `classId` (required): Class ID

---

### 5. Punch In/Out (Teacher)

#### 5.1 Punch In
**Endpoint:** `POST /teacher/punch-in`  
**Request Body:**
```json
{
  "timestamp": "2026-03-02T09:00:00Z"
}
```

#### 5.2 Get Punch History
**Endpoint:** `GET /teacher/punch-history`  
**Description:** Get teacher's punch records

---

### 6. Chat (Teacher)

#### 6.1 Get Class Chat
**Endpoint:** `GET /teacher/chat/class/:classId`  
**Path Parameters:**
- `classId` (required): Class ID

#### 6.2 Send Class Message
**Endpoint:** `POST /teacher/chat/class/send`  
**Request Body:**
```json
{
  "classId": "507f1f77bcf86cd799439011",
  "message": "Today's lesson will focus on algebra"
}
```

#### 6.3 Get Personal Chat
**Endpoint:** `GET /teacher/chat/personal/:userId`  
**Path Parameters:**
- `userId` (required): User ID to chat with

#### 6.4 Send Personal Message
**Endpoint:** `POST /teacher/chat/personal/send`  
**Request Body:**
```json
{
  "recipientId": "507f1f77bcf86cd799439020",
  "message": "Hi, can we discuss the upcoming exam?"
}
```

---

### 7. Study Material (Teacher)

#### 7.1 Upload Material
**Endpoint:** `POST /teacher/material/upload`  
**Request Body (Multipart/Form-Data):**
- `classId`: Class ID
- `title`: Material title
- `description`: Material description
- `file`: File upload

#### 7.2 Get Materials by Class
**Endpoint:** `GET /teacher/material/:classId`  
**Path Parameters:**
- `classId` (required): Class ID

---

### 8. Teacher Analytics

**Endpoint:** `GET /teacher/analytics`  
**Description:** Get teacher's analytics and insights

---

### 9. Results Management (Teacher)

#### 9.1 Create Result
**Endpoint:** `POST /teacher/result`  
**Request Body:**
```json
{
  "studentId": "507f1f77bcf86cd799439013",
  "examName": "Midterm",
  "marksObtained": 85,
  "totalMarks": 100
}
```

#### 9.2 Publish Result
**Endpoint:** `PUT /teacher/result/publish/:id`  
**Path Parameters:**
- `id` (required): Result ID

---

## Student APIs

**Authentication:** Required (JWT Token with `student` role)

---

### 1. Student Dashboard

**Endpoint:** `GET /student/dashboard`  
**Description:** Get student's personalized dashboard with grades, attendance, etc.

---

### 2. Quizzes (Student)

#### 2.1 List Available Quizzes
**Endpoint:** `GET /student/quizzes`  
**Description:** Get all available quizzes for the student

#### 2.2 Submit Quiz
**Endpoint:** `POST /student/quiz/submit`  
**Request Body:**
```json
{
  "quizId": "507f1f77bcf86cd799439040",
  "answers": [
    {
      "questionId": "Q1",
      "answer": "Option A"
    }
  ]
}
```

#### 2.3 Get Quiz Result
**Endpoint:** `GET /student/quiz/result/:quizId`  
**Path Parameters:**
- `quizId` (required): Quiz ID

---

### 3. Attendance (Student)

**Endpoint:** `GET /student/attendance`  
**Description:** Get student's attendance records and percentage

---

### 4. Results (Student)

**Endpoint:** `GET /student/results`  
**Description:** Get student's exam results and scores

---

### 5. Timetable (Student)

**Endpoint:** `GET /student/timetable`  
**Description:** Get student's class timetable

---

## Admin APIs

**Authentication:** Required (JWT Token with `admin` role)

---

### 1. Get Teacher Punch Logs

**Endpoint:** `GET /admin/teacher-punch-logs`  
**Description:** Get attendance/punch logs for all teachers

---

### 2. Admin Analytics

**Endpoint:** `GET /admin/analytics`  
**Description:** Get system-wide analytics

---

### 3. Activities (Admin)

#### 3.1 Get Activities
**Endpoint:** `GET /admin/activities`  
**Query Parameters:**
- `limit` (optional, default: 10): Number of activities to fetch

#### 3.2 Mark Activity as Read
**Endpoint:** `PUT /admin/activities/:id/read`  
**Path Parameters:**
- `id` (required): Activity ID

---

## Chat APIs

**Authentication:** Required (JWT Token)

These endpoints are available to authenticated users (students, teachers, admin).

---

### 1. Get Class Chat

**Endpoint:** `GET /chat/class/:classId`  
**Path Parameters:**
- `classId` (required): Class ID

**Allowed Roles:** Teacher, Student, Admin

---

### 2. Send Class Message

**Endpoint:** `POST /chat/class/send`  
**Request Body:**
```json
{
  "classId": "507f1f77bcf86cd799439011",
  "message": "Message content here",
  "timestamp": "2026-03-02T10:30:00Z"
}
```

**Allowed Roles:** Teacher, Student

---

### 3. Get Personal Chat

**Endpoint:** `GET /chat/personal/:userId`  
**Path Parameters:**
- `userId` (required): User ID to chat with

**Allowed Roles:** Any authenticated user

---

### 4. Send Personal Message

**Endpoint:** `POST /chat/personal/send`  
**Request Body:**
```json
{
  "recipientId": "507f1f77bcf86cd799439020",
  "message": "Personal message",
  "timestamp": "2026-03-02T10:30:00Z"
}
```

**Allowed Roles:** Any authenticated user

---

## Common Response Formats

### Success Response (2xx)
```json
{
  "success": true,
  "data": {}
}
```

### Error Response (4xx, 5xx)
```json
{
  "success": false,
  "error": "Error message here",
  "message": "Detailed error description"
}
```

---

## HTTP Status Codes

| Code | Meaning |
|------|---------|
| 200 | OK - Request succeeded |
| 201 | Created - Resource created successfully |
| 400 | Bad Request - Invalid parameters |
| 401 | Unauthorized - No/invalid token |
| 403 | Forbidden - Insufficient permissions |
| 404 | Not Found - Resource not found |
| 409 | Conflict - Resource already exists |
| 500 | Server Error - Internal server error |

---

## Authentication Headers

All authenticated endpoints require the following header:

```
Authorization: Bearer <jwt_token>
```

Replace `<jwt_token>` with the token received from `/auth/login`.

---

## Query & Pagination Standards

Some endpoints support pagination and filtering:

```
GET /endpoint?limit=20&skip=0&sort=-createdAt
```

**Common Query Parameters:**
- `limit`: Number of records per page (default varies)
- `skip`: Number of records to skip
- `sort`: Sort field (prefix with `-` for descending)
- `search`: Search query for text fields
- `filter`: Additional filter criteria

---

## Real-Time Updates

The frontend uses Socket.IO for real-time notifications and chat:

**Connection Event:**
```javascript
io.on('connection', (socket) => {
  socket.on('joinRoom', ({ room }) => {
    socket.join(room);
  });
  
  socket.on('message', (msg) => {
    io.to(msg.room).emit('message', msg);
  });
});
```

---

## Testing the APIs

### Using cURL

```bash
# Register
curl -X POST http://localhost:5001/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "password123",
    "name": "Test User",
    "role": "student"
  }'

# Login
curl -X POST http://localhost:5001/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "password123"
  }'

# Get Classes (with token)
curl -X GET http://localhost:5001/api/classes \
  -H "Authorization: Bearer <token_from_login>"
```

### Using Postman

1. Import the API endpoints into Postman
2. Set the `Authorization` header in the collection with value: `Bearer {{token}}`
3. Store token in Postman environment after login
4. Use `{{token}}` in any authenticated endpoint

---

## Error Handling Examples

### Missing Required Field
```json
{
  "success": false,
  "message": "Validation failed: 'email' is required"
}
```

### Invalid Token
```json
{
  "success": false,
  "error": "Invalid or expired token"
}
```

### Insufficient Permissions
```json
{
  "success": false,
  "error": "You do not have permission to perform this action"
}
```

---

## Rate Limiting

Currently, there is no rate limiting implemented. For production, consider implementing rate limiting middleware.

---

## CORS Configuration

CORS is enabled for all origins by default. For production, update `FRONTEND_URL` environment variable to restrict origins.

---

## Database Collections

- **users**: User accounts (students, teachers, admins)
- **classes**: Class/batch information
- **students**: Student profiles
- **teachers**: Teacher profiles
- **subjects**: Subject information
- **marks**: Student marks and scores
- **attendance**: Attendance records
- **timetable**: Class schedules
- **timetable_config**: Timetable configuration
- **notices**: Notices and announcements
- **messages**: Chat messages
- **quizzes**: Quiz data
- **quiz_submissions**: Student quiz responses
- **results**: Exam results
- **activities**: System activity logs
- **punch_logs**: Teacher punch in/out records
- **study_materials**: Educational materials

---

## Environment Variables Required

```
MONGO_URI=mongodb://localhost:27017/asset-manager
JWT_SECRET=your_secret_key_here
FRONTEND_URL=http://localhost:5173
PORT=5001
NODE_ENV=development
```

---

## Support & Bug Reports

For issues or feature requests, contact the development team or submit a GitHub issue.

**Last Updated:** March 2, 2026  
**API Version:** 1.0
