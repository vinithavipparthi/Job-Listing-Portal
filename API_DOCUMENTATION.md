# Job Listing Portal - API Documentation

## Base URL
```
Development: http://localhost:5000/api
Production: https://your-domain.com/api
```

## Authentication
All protected endpoints require JWT token in Authorization header:
```
Authorization: Bearer {token}
```

---

## Authentication Endpoints

### 1. Register User
**POST** `/auth/register`

Request body:
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "password": "securePassword123",
  "userType": "jobSeeker",
  "companyName": "Optional (for employer only)"
}
```

Response (201):
```json
{
  "message": "Registration successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "507f1f77bcf86cd799439011",
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@example.com",
    "userType": "jobSeeker",
    "createdAt": "2024-01-08T10:30:00Z"
  }
}
```

### 2. Login User
**POST** `/auth/login`

Request body:
```json
{
  "email": "john@example.com",
  "password": "securePassword123"
}
```

Response (200):
```json
{
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "507f1f77bcf86cd799439011",
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@example.com",
    "userType": "jobSeeker"
  }
}
```

### 3. Get Current User
**GET** `/auth/me`

Headers: `Authorization: Bearer {token}`

Response (200):
```json
{
  "user": {
    "id": "507f1f77bcf86cd799439011",
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@example.com",
    "phone": "+1234567890",
    "userType": "jobSeeker",
    "bio": "Software Developer",
    "location": "New York, NY",
    "skills": ["JavaScript", "React", "Node.js"],
    "experience": "3-5 years",
    "education": "BS in Computer Science",
    "expectedSalary": 120000,
    "availability": "Immediate",
    "createdAt": "2024-01-08T10:30:00Z"
  }
}
```

### 4. Update Job Seeker Profile
**PUT** `/auth/profile`

Headers: `Authorization: Bearer {token}`

Request body:
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "phone": "+1234567890",
  "location": "San Francisco, CA",
  "bio": "Full Stack Developer",
  "skills": ["JavaScript", "Python", "React", "Node.js"],
  "experience": "3-5 years",
  "education": "BS in Computer Science",
  "expectedSalary": 150000,
  "availability": "2 weeks"
}
```

Response (200):
```json
{
  "message": "Profile updated successfully",
  "user": { /* updated user object */ }
}
```

### 5. Update Employer Profile
**PUT** `/auth/employer-profile`

Headers: `Authorization: Bearer {token}`

Request body:
```json
{
  "companyName": "Tech Company Inc",
  "companyWebsite": "https://techcompany.com",
  "companySize": "51-200",
  "industry": "Technology",
  "companyDescription": "We build amazing software solutions"
}
```

Response (200):
```json
{
  "message": "Employer profile updated successfully",
  "user": { /* updated user object */ }
}
```

### 6. Logout
**POST** `/auth/logout`

Headers: `Authorization: Bearer {token}`

Response (200):
```json
{
  "message": "Logged out successfully"
}
```

---

## Job Endpoints

### 1. Get All Jobs
**GET** `/jobs`

Query Parameters:
```
search=keyword       # Search in title, description
location=city        # Filter by location
jobType=Full-Time    # Filter by job type
experience=Senior    # Filter by experience
industry=tech        # Filter by industry
minSalary=50000      # Minimum salary
maxSalary=150000     # Maximum salary
page=1               # Page number
limit=10             # Results per page
sort=-createdAt      # Sort field
```

Example:
```
GET /api/jobs?search=developer&location=NYC&jobType=Full-Time&page=1&limit=10
```

Response (200):
```json
{
  "jobs": [
    {
      "_id": "507f1f77bcf86cd799439012",
      "title": "Senior React Developer",
      "description": "We are looking for a senior React developer...",
      "location": "New York, NY",
      "jobType": "Full-Time",
      "salaryRange": {
        "min": 100000,
        "max": 150000,
        "currency": "USD"
      },
      "experience": "Senior",
      "skills": ["React", "JavaScript", "Node.js"],
      "industry": "Technology",
      "applicationsCount": 5,
      "views": 42,
      "featured": true,
      "employer": {
        "_id": "507f1f77bcf86cd799439011",
        "firstName": "Jane",
        "lastName": "Smith",
        "companyName": "Tech Corp",
        "companyLogo": "logo-123.png"
      },
      "createdAt": "2024-01-08T10:30:00Z"
    }
  ],
  "pagination": {
    "total": 45,
    "pages": 5,
    "currentPage": 1,
    "limit": 10
  }
}
```

### 2. Get Job Details
**GET** `/jobs/:id`

Response (200):
```json
{
  "job": {
    "_id": "507f1f77bcf86cd799439012",
    "title": "Senior React Developer",
    "description": "Full job description...",
    "requirements": "5+ years experience with React...",
    "responsibilities": "Lead development...",
    "location": "New York, NY",
    "jobType": "Full-Time",
    "salaryRange": { "min": 100000, "max": 150000, "currency": "USD" },
    "skills": ["React", "JavaScript", "TypeScript"],
    "experience": "Senior",
    "industry": "Technology",
    "applicationDeadline": "2024-02-08T00:00:00Z",
    "applicationsCount": 5,
    "views": 42,
    "featured": true,
    "isActive": true,
    "employer": {
      "_id": "507f1f77bcf86cd799439011",
      "firstName": "Jane",
      "lastName": "Smith",
      "email": "jane@techcorp.com",
      "phone": "+1234567890",
      "companyName": "Tech Corp",
      "companyDescription": "Leading tech company...",
      "companyWebsite": "https://techcorp.com",
      "companyLogo": "logo-123.png"
    },
    "createdAt": "2024-01-08T10:30:00Z",
    "updatedAt": "2024-01-08T10:30:00Z"
  }
}
```

### 3. Create Job
**POST** `/jobs`

Headers: `Authorization: Bearer {token}`
Note: User must be an employer

Request body:
```json
{
  "title": "Senior React Developer",
  "description": "We are looking for a talented React developer...",
  "requirements": "5+ years experience with React and Node.js",
  "responsibilities": "Lead front-end development, mentor team...",
  "location": "New York, NY",
  "jobType": "Full-Time",
  "salaryRange": {
    "min": 100000,
    "max": 150000,
    "currency": "USD"
  },
  "skills": ["React", "JavaScript", "TypeScript", "Node.js"],
  "experience": "Senior",
  "industry": "Technology",
  "applicationDeadline": "2024-02-08T00:00:00Z",
  "featured": false
}
```

Response (201):
```json
{
  "message": "Job created successfully",
  "job": { /* job object */ }
}
```

### 4. Update Job
**PUT** `/jobs/:id`

Headers: `Authorization: Bearer {token}`
Note: Only employer who created the job can update

Request body: (same as create, all fields optional)

Response (200):
```json
{
  "message": "Job updated successfully",
  "job": { /* updated job object */ }
}
```

### 5. Delete Job
**DELETE** `/jobs/:id`

Headers: `Authorization: Bearer {token}`
Note: Only employer who created the job can delete

Response (200):
```json
{
  "message": "Job deleted successfully"
}
```

### 6. Get Employer's Jobs
**GET** `/jobs/my-jobs/all`

Headers: `Authorization: Bearer {token}`
Note: User must be an employer

Response (200):
```json
{
  "jobs": [ /* array of employer's jobs */ ],
  "count": 5
}
```

### 7. Get Featured Jobs
**GET** `/jobs/featured/list`

Response (200):
```json
{
  "jobs": [ /* array of featured jobs */ ]
}
```

---

## Application Endpoints

### 1. Apply for Job
**POST** `/applications`

Headers: `Authorization: Bearer {token}`
Note: User must be a job seeker

Request body:
```json
{
  "jobId": "507f1f77bcf86cd799439012",
  "coverLetter": "I am very interested in this position because..."
}
```

Response (201):
```json
{
  "message": "Application submitted successfully",
  "application": {
    "_id": "507f1f77bcf86cd799439013",
    "job": {
      "_id": "507f1f77bcf86cd799439012",
      "title": "Senior React Developer"
    },
    "applicant": {
      "_id": "507f1f77bcf86cd799439011",
      "firstName": "John",
      "lastName": "Doe"
    },
    "coverLetter": "I am very interested...",
    "status": "pending",
    "appliedAt": "2024-01-08T10:30:00Z"
  }
}
```

### 2. Get My Applications
**GET** `/applications/my-applications`

Headers: `Authorization: Bearer {token}`
Note: Job seeker only

Query Parameters:
```
status=pending    # Filter by status (pending, reviewed, accepted, rejected)
page=1            # Page number
limit=10          # Results per page
```

Response (200):
```json
{
  "applications": [
    {
      "_id": "507f1f77bcf86cd799439013",
      "job": {
        "_id": "507f1f77bcf86cd799439012",
        "title": "Senior React Developer",
        "location": "New York, NY",
        "jobType": "Full-Time",
        "salaryRange": { "min": 100000, "max": 150000 },
        "employer": { /* employer info */ }
      },
      "status": "pending",
      "appliedAt": "2024-01-08T10:30:00Z"
    }
  ],
  "pagination": {
    "total": 5,
    "pages": 1,
    "currentPage": 1
  }
}
```

### 3. Get Application
**GET** `/applications/:id`

Headers: `Authorization: Bearer {token}`

Response (200):
```json
{
  "application": {
    "_id": "507f1f77bcf86cd799439013",
    "job": { /* full job object */ },
    "applicant": {
      "_id": "507f1f77bcf86cd799439011",
      "firstName": "John",
      "lastName": "Doe",
      "email": "john@example.com",
      "phone": "+1234567890",
      "bio": "Software Developer",
      "resume": "resume-123.pdf",
      "skills": ["JavaScript", "React"]
    },
    "coverLetter": "I am very interested...",
    "status": "pending",
    "appliedAt": "2024-01-08T10:30:00Z"
  }
}
```

### 4. Update Application Status
**PUT** `/applications/:id/status`

Headers: `Authorization: Bearer {token}`
Note: Only employer of the job can update

Request body:
```json
{
  "status": "accepted"
}
```

Status options: `pending`, `reviewed`, `accepted`, `rejected`

Response (200):
```json
{
  "message": "Application status updated",
  "application": { /* updated application */ }
}
```

### 5. Withdraw Application
**DELETE** `/applications/:id`

Headers: `Authorization: Bearer {token}`
Note: Job seeker only

Response (200):
```json
{
  "message": "Application withdrawn successfully"
}
```

### 6. Get Job Applications (Employer)
**GET** `/applications/job/:jobId/all`

Headers: `Authorization: Bearer {token}`
Note: Employer only, must own the job

Query Parameters:
```
status=pending    # Filter by status
page=1            # Page number
limit=10          # Results per page
```

Response (200):
```json
{
  "applications": [
    {
      "_id": "507f1f77bcf86cd799439013",
      "applicant": {
        "_id": "507f1f77bcf86cd799439011",
        "firstName": "John",
        "lastName": "Doe",
        "email": "john@example.com",
        "phone": "+1234567890",
        "bio": "Software Developer",
        "skills": ["JavaScript", "React"]
      },
      "job": { "_id": "507f1f77bcf86cd799439012", "title": "Senior React Developer" },
      "status": "pending",
      "appliedAt": "2024-01-08T10:30:00Z"
    }
  ],
  "pagination": { /* pagination info */ }
}
```

---

## Profile Endpoints

### 1. Get My Profile
**GET** `/profiles/me`

Headers: `Authorization: Bearer {token}`

Response (200):
```json
{
  "profile": { /* full user profile */ }
}
```

### 2. Get Public Profile
**GET** `/profiles/:userId`

Response (200):
```json
{
  "profile": {
    "id": "507f1f77bcf86cd799439011",
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@example.com",
    "phone": "+1234567890",
    "profilePicture": "pic-123.jpg",
    "userType": "jobSeeker",
    "location": "New York, NY",
    "bio": "Full Stack Developer",
    "skills": ["JavaScript", "React", "Node.js"],
    "experience": "3-5 years"
  }
}
```

### 3. Update Job Seeker Profile
**PUT** `/profiles/seeker/update`

Headers: `Authorization: Bearer {token}`
Note: Job seeker only

Request body:
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "phone": "+1234567890",
  "location": "San Francisco, CA",
  "bio": "Full Stack Developer",
  "skills": ["JavaScript", "Python", "React"],
  "experience": "3-5 years",
  "education": "BS Computer Science",
  "expectedSalary": 150000,
  "availability": "2 weeks"
}
```

Response (200):
```json
{
  "message": "Profile updated successfully",
  "profile": { /* updated profile */ }
}
```

### 4. Update Employer Profile
**PUT** `/profiles/employer/update`

Headers: `Authorization: Bearer {token}`
Note: Employer only

Request body:
```json
{
  "companyName": "Tech Corp",
  "companyWebsite": "https://techcorp.com",
  "companySize": "51-200",
  "industry": "Technology",
  "companyDescription": "Leading tech company...",
  "phone": "+1234567890"
}
```

Response (200):
```json
{
  "message": "Employer profile updated successfully",
  "profile": { /* updated profile */ }
}
```

### 5. Upload Resume
**POST** `/profiles/upload-resume`

Headers:
```
Authorization: Bearer {token}
Content-Type: multipart/form-data
```

Form Data:
- `resume` (file): PDF file

Response (200):
```json
{
  "message": "Resume uploaded successfully",
  "resume": "resume-123.pdf",
  "resumePath": "/uploads/resume-123.pdf"
}
```

### 6. Upload Profile Picture
**POST** `/profiles/upload-picture`

Headers:
```
Authorization: Bearer {token}
Content-Type: multipart/form-data
```

Form Data:
- `profilePicture` (file): PNG or JPG file

Response (200):
```json
{
  "message": "Profile picture uploaded successfully",
  "profilePicture": "pic-123.jpg",
  "picturePath": "/uploads/pic-123.jpg"
}
```

### 7. Upload Company Logo
**POST** `/profiles/upload-logo`

Headers:
```
Authorization: Bearer {token}
Content-Type: multipart/form-data
```

Form Data:
- `companyLogo` (file): PNG or JPG file

Response (200):
```json
{
  "message": "Company logo uploaded successfully",
  "companyLogo": "logo-123.png",
  "logoPath": "/uploads/logo-123.png"
}
```

### 8. Save Job
**POST** `/profiles/save-job/:jobId`

Headers: `Authorization: Bearer {token}`
Note: Job seeker only

Response (200):
```json
{
  "message": "Job saved successfully"
}
```

### 9. Unsave Job
**DELETE** `/profiles/save-job/:jobId`

Headers: `Authorization: Bearer {token}`
Note: Job seeker only

Response (200):
```json
{
  "message": "Job removed from saved"
}
```

### 10. Get Saved Jobs
**GET** `/profiles/saved-jobs/all`

Headers: `Authorization: Bearer {token}`
Note: Job seeker only

Response (200):
```json
{
  "savedJobs": [
    {
      "_id": "507f1f77bcf86cd799439012",
      "title": "Senior React Developer",
      "location": "New York, NY",
      "jobType": "Full-Time",
      "salaryRange": { "min": 100000, "max": 150000 },
      "employer": { /* employer info */ }
    }
  ]
}
```

---

## Error Responses

### 400 Bad Request
```json
{
  "message": "Please provide all required fields"
}
```

### 401 Unauthorized
```json
{
  "message": "Invalid email or password"
}
```

### 403 Forbidden
```json
{
  "message": "Access denied. Insufficient permissions"
}
```

### 404 Not Found
```json
{
  "message": "Job not found"
}
```

### 409 Conflict
```json
{
  "message": "You have already applied for this job"
}
```

### 500 Server Error
```json
{
  "message": "Server error",
  "error": "Internal server error message"
}
```

---

## Status Codes Summary

| Code | Meaning |
|------|---------|
| 200 | OK - Request successful |
| 201 | Created - Resource created |
| 400 | Bad Request - Invalid input |
| 401 | Unauthorized - Authentication required |
| 403 | Forbidden - Insufficient permissions |
| 404 | Not Found - Resource not found |
| 409 | Conflict - Duplicate entry |
| 500 | Server Error - Internal error |

---

## Testing with cURL

### Register User
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "firstName":"John",
    "lastName":"Doe",
    "email":"john@example.com",
    "password":"password123",
    "userType":"jobSeeker"
  }'
```

### Login
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email":"john@example.com",
    "password":"password123"
  }'
```

### Get Jobs with Filters
```bash
curl "http://localhost:5000/api/jobs?location=NYC&jobType=Full-Time&page=1"
```

### Apply for Job
```bash
curl -X POST http://localhost:5000/api/applications \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer {token}" \
  -d '{
    "jobId":"507f1f77bcf86cd799439012",
    "coverLetter":"I am interested in this position..."
  }'
```

---

This API documentation provides a complete reference for all endpoints, including request/response formats, authentication requirements, and error handling.
