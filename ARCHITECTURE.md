# Job Listing Portal - Architecture & Technical Specifications

## System Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    CLIENT LAYER (React 18)                      │
│                   Port: 3000 (Development)                      │
│  ┌────────┐ ┌──────────┐ ┌──────────┐ ┌──────────────────────┐ │
│  │ Pages  │ │Components│ │ Context  │ │  Services (API)      │ │
│  │ (13)   │ │ (2)      │ │ (Auth)   │ │  (Axios + Intercept) │ │
│  └────────┘ └──────────┘ └──────────┘ └──────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                             ↕
                    NETWORK (HTTP/REST)
                             ↕
┌─────────────────────────────────────────────────────────────────┐
│              SERVER LAYER (Express.js)                          │
│            Port: 5000 (Development)                             │
│  ┌──────────┐ ┌─────────┐ ┌────────────┐ ┌──────────────────┐  │
│  │ Routes   │ │Middleware│ │ Controllers│ │File Upload       │  │
│  │ (4 sets) │ │(Auth)   │ │(Business)  │ │(Multer)          │  │
│  └──────────┘ └─────────┘ └────────────┘ └──────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                             ↕
                    MONGOOSE ODM
                             ↕
┌─────────────────────────────────────────────────────────────────┐
│            DATABASE LAYER (MongoDB)                             │
│                                                                 │
│  ┌──────────┐  ┌──────────┐  ┌──────────────┐                 │
│  │ Users    │  │ Jobs     │  │Applications  │                 │
│  │Collection│  │Collection│  │Collection    │                 │
│  └──────────┘  └──────────┘  └──────────────┘                 │
└─────────────────────────────────────────────────────────────────┘
```

## Frontend Architecture

### Component Hierarchy
```
App (Root)
│
├── Navbar
│   ├── Logo/Title
│   ├── Navigation Links
│   └── User Menu
│
├── Routes
│   ├── Public Routes
│   │   ├── Home
│   │   ├── Login
│   │   ├── Register
│   │   ├── JobListing
│   │   └── JobDetails
│   │
│   └── Protected Routes
│       ├── DashboardSeeker
│       ├── DashboardEmployer
│       ├── Profile
│       ├── CreateJob
│       ├── EditJob
│       ├── Applications
│       └── SavedJobs
│
└── AuthProvider
    └── AuthContext
```

### Data Flow
```
User Action (Click, Submit)
    ↓
Component State/Event Handler
    ↓
API Call (services/api.js)
    ↓
Axios Instance with Auth Header
    ↓
Backend API Endpoint
    ↓
Response/Error Handling
    ↓
Update Component State/Context
    ↓
Re-render Component
```

## Backend Architecture

### Request Processing Pipeline
```
HTTP Request
    ↓
Express Middleware
├── CORS Handling
├── JSON Parsing
└── Request Logging
    ↓
Route Handler
    ├── Request Validation
    └── Authentication Check (if protected)
    ↓
Auth Middleware (for protected routes)
├── JWT Verification
├── User Retrieval
└── Role Check
    ↓
Route Controller
├── Business Logic
├── Database Operations
└── Response Preparation
    ↓
Response
├── Success Response (200, 201)
└── Error Response (4xx, 5xx)
```

### API Route Structure
```
Backend Server (Express)
│
├── /api/auth (6 endpoints)
│   ├── POST /register
│   ├── POST /login
│   ├── GET /me
│   ├── PUT /profile
│   ├── PUT /employer-profile
│   └── POST /logout
│
├── /api/jobs (8 endpoints)
│   ├── GET / (with filters)
│   ├── GET /:id
│   ├── POST / (create)
│   ├── PUT /:id (update)
│   ├── DELETE /:id
│   ├── GET /my-jobs/all
│   └── GET /featured/list
│
├── /api/applications (8 endpoints)
│   ├── POST / (apply)
│   ├── GET /my-applications
│   ├── GET /job/:jobId/all
│   ├── GET /employer/all-applications
│   ├── GET /:id
│   ├── PUT /:id/status
│   └── DELETE /:id
│
└── /api/profiles (11 endpoints)
    ├── GET /me
    ├── GET /:userId
    ├── PUT /seeker/update
    ├── PUT /employer/update
    ├── POST /upload-resume
    ├── POST /upload-picture
    ├── POST /upload-logo
    ├── POST /save-job/:jobId
    ├── DELETE /save-job/:jobId
    └── GET /saved-jobs/all
```

## Database Schema Relationships

```
┌─────────────────────────────────────────────────────────────┐
│                        USERS Collection                      │
│  _id: ObjectId                                              │
│  firstName, lastName, email, password                       │
│  userType: 'jobSeeker' | 'employer'                         │
│  [Job Seeker: resume, skills, experience, etc.]           │
│  [Employer: companyName, industry, etc.]                   │
│  appliedJobs: [ApplicationId]   ─┐                          │
│  postedJobs: [JobId]          ─┐ │                          │
│  savedJobs: [JobId]           │ │                           │
└─────────────────────────────────────────────────────────────┘
                    ↓                 ↓
        ┌───────────┴──────────┬──────┴──────────┐
        │                      │                 │
        ↓                      ↓                 ↓
┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│ JOBS Collection  │  │APPLICATIONS      │  │JOBS Collection   │
│ _id: ObjectId    │  │Collection        │  │_id: ObjectId     │
│ title, desc      │  │_id: ObjectId     │  │title, desc       │
│ location, etc    │  │job: JobId ───────┼→ │location, etc     │
│ employer: UserId ┼→ │applicant: UserId ┼→ │employer: UserId  │
│ applications: [] │  │coverLetter       │  │applications: []  │
│ salaryRange      │  │status            │  │salaryRange       │
│ createdAt        │  │appliedAt         │  │createdAt         │
└──────────────────┘  └──────────────────┘  └──────────────────┘
```

## Authentication Flow

```
User Registration
    ↓
POST /api/auth/register
├── Validate Input
├── Check Email Uniqueness
├── Hash Password (bcryptjs)
├── Save User to DB
├── Generate JWT Token
└── Return Token + User Data

User Login
    ↓
POST /api/auth/login
├── Validate Input
├── Find User by Email
├── Compare Password Hash
├── Generate JWT Token
└── Return Token + User Data

Protected Request
    ↓
Request with Authorization Header
├── Extract Token from Header
├── Verify Token (JWT)
├── Get User from DB
├── Attach User to req.user
└── Continue to Route Handler

Token Expiration
    ↓
Token Expires (30 days)
    ↓
401 Unauthorized Response
    ↓
Frontend Removes Token
    ↓
Redirect to Login
```

## File Upload Flow

```
User Selects File
    ↓
Frontend Form Submit
├── Create FormData
├── Append File
└── POST to /api/profiles/upload-[type]
    ↓
Backend Multer Middleware
├── Validate File Type
├── Validate File Size
├── Create Unique Filename
└── Save to /uploads Directory
    ↓
Update User Document
├── Store Filename in DB
└── Return File Path
    ↓
Frontend Receives Response
├── Update UI
└── Show Success Message
```

## Job Application Flow

### For Job Seekers
```
Browse Jobs
    ↓
View Job Details
    ↓
Click Apply Button
    ↓
Prompt for Cover Letter
    ↓
Submit Application
    ↓
POST /api/applications
├── Validate Job Exists
├── Check No Duplicate Application
├── Create Application Document
├── Update User.appliedJobs
├── Update Job.applications
└── Return Confirmation
    ↓
Show Success Message
    ↓
Redirect to Dashboard
```

### For Employers
```
View Received Applications
    ↓
GET /api/applications/job/:jobId/all
├── Get All Applications for Job
├── Populate Applicant Details
└── Return with Pagination
    ↓
Review Applicant
    ↓
Update Application Status
    ↓
PUT /api/applications/:id/status
├── Verify Job Ownership
├── Update Status
└── Return Updated Application
    ↓
Show Status Change
```

## Search & Filter Architecture

```
User Input
├── Search Term
├── Location Filter
├── Job Type Filter
├── Experience Level
├── Industry
└── Salary Range
    ↓
Build Query Object
├── Text Search Query
├── Location Regex Match
├── Job Type Array Match
├── Experience Level Array
├── Industry Regex Match
└── Salary Range Comparison
    ↓
MongoDB Query
    ↓
Execute with Pagination
├── Skip: (page-1) * limit
└── Limit: number of results
    ↓
Return Results with Metadata
├── Jobs Array
├── Total Count
├── Current Page
└── Total Pages
```

## Error Handling Architecture

```
Error Occurs
    ↓
├─→ Validation Error
│   └─→ 400 Bad Request
│
├─→ Authentication Error
│   ├─→ No Token: 401 Unauthorized
│   └─→ Invalid Token: 401 Unauthorized
│
├─→ Authorization Error
│   └─→ 403 Forbidden
│
├─→ Resource Not Found
│   └─→ 404 Not Found
│
├─→ Duplicate Entry
│   └─→ 409 Conflict
│
└─→ Server Error
    └─→ 500 Internal Server Error
        ↓
        Log Error
        ↓
        Return Generic Error Message
```

## Security Architecture

```
User Authentication
    ↓
├── Input Validation
├── Password Hashing (bcryptjs)
├── JWT Token (Signed)
└── Token Stored in localStorage
    ↓
Request Headers
    ↓
├── Authorization: Bearer [token]
├── CORS Headers Checked
└── Content-Type Validated
    ↓
Backend Verification
    ↓
├── JWT Signature Verified
├── Token Not Expired
├── User Still Active
└── Role-Based Access Check
    ↓
Database Access
    ↓
├── Mongoose Validation
├── File Type Validation
├── File Size Validation
└── No SQL Injection (MongoDB)
```

## Deployment Architecture (Production Ready)

```
Production Environment
│
├── Frontend
│   ├── Build: npm run build
│   ├── Host on: Vercel/Netlify/AWS S3
│   └── CDN: CloudFlare/CloudFront
│
├── Backend
│   ├── Host on: Heroku/Railway/EC2
│   ├── Process Manager: PM2
│   └── HTTPS: SSL Certificate
│
├── Database
│   ├── MongoDB Atlas (Cloud)
│   ├── Automated Backups
│   └── Replica Sets for HA
│
└── File Storage
    ├── AWS S3 or
    ├── Cloud Storage or
    └── Server Storage with Backups
```

## Performance Optimization Strategies

```
Frontend Optimization
├── Code Splitting
├── Lazy Loading Pages
├── Image Optimization
├── Caching Strategies
└── Minification

Backend Optimization
├── Database Indexing
│   └── On: email, userType, location, jobType
├── API Caching
├── Pagination (10 items/page)
├── Query Optimization
└── Connection Pooling

Database Optimization
├── Indexes on Frequently Searched Fields
├── Aggregate Pipelines for Complex Queries
└── TTL Indexes for Temporary Data
```

## Technology Stack Breakdown

```
Frontend Stack
├── React 18 (UI Framework)
├── React Router v7 (Routing)
├── Axios (HTTP Client)
├── Context API (State Management)
├── Tailwind CSS (Styling)
└── CSS3 (Advanced Styling)

Backend Stack
├── Node.js (Runtime)
├── Express.js (Web Framework)
├── MongoDB (Database)
├── Mongoose (ODM)
├── JWT (Authentication)
├── bcryptjs (Password Security)
├── multer (File Upload)
└── CORS (Cross-Origin)

DevOps & Tools
├── npm (Package Manager)
├── Git (Version Control)
├── .env (Configuration)
└── Postman (API Testing)
```

## Scalability Considerations

```
Current Setup (Single Server)
├── Frontend: Static files + Single SPA
├── Backend: Single Node process
├── Database: Single MongoDB instance
└── Files: Server local storage

Scalable Setup
├── Frontend
│   ├── Multiple servers with Load Balancer
│   ├── CDN for static assets
│   └── Caching at edge
│
├── Backend
│   ├── Horizontal scaling with Load Balancer
│   ├── Multi-process with PM2
│   ├── Microservices (future)
│   └── Message Queue for async tasks
│
├── Database
│   ├── MongoDB Atlas Sharding
│   ├── Read Replicas
│   └── Connection Pooling
│
└── Files
    ├── AWS S3 for file storage
    ├── CloudFront CDN
    └── Distributed caching
```

---

This comprehensive architecture ensures:
- ✅ Clear separation of concerns
- ✅ Scalable design
- ✅ Maintainable codebase
- ✅ Secure operations
- ✅ Good performance
- ✅ Easy deployment
