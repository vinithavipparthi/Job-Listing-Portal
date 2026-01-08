# Job Listing Portal

A comprehensive full-stack web application that connects job seekers with potential employers. Features include user authentication, job search with advanced filters, job applications, resume uploads, and separate dashboards for both job seekers and employers.

## Features

### User Authentication
- User registration (Job Seeker and Employer roles)
- Secure login with JWT tokens
- Password hashing with bcryptjs
- Session management

### Job Search
- Advanced search with filters (location, job type, salary, experience)
- Featured jobs display
- Real-time job listings
- Job details with employer information

### Profile Management
- Job seeker profiles with resume upload
- Employer profiles with company information
- Skills and experience tracking
- Profile picture upload

### Job Application
- Apply for jobs with cover letters
- Track application status (pending, reviewed, accepted, rejected)
- Save jobs for later
- View all applications

### Employer Features
- Post, edit, and delete job listings
- View all applications for posted jobs
- Application status management
- Company profile management

### Job Seeker Features
- Browse and search available jobs
- Apply for jobs
- Save jobs for later viewing
- Track application history
- Manage personal profile

## Tech Stack

### Backend
- **Framework**: Express.js
- **Database**: MongoDB
- **Authentication**: JWT (jsonwebtoken)
- **Password Hashing**: bcryptjs
- **File Upload**: multer
- **Environment**: dotenv

### Frontend
- **Framework**: React 18
- **Routing**: React Router v7
- **HTTP Client**: Axios
- **Styling**: Tailwind CSS
- **CSS**: Custom CSS with responsive design

## Project Structure

```
Job Listing Portal/
├── backend/
│   ├── models/
│   │   ├── User.js
│   │   ├── Job.js
│   │   └── Application.js
│   ├── routes/
│   │   ├── auth.js
│   │   ├── jobs.js
│   │   ├── applications.js
│   │   └── profiles.js
│   ├── middleware/
│   │   └── auth.js
│   ├── uploads/
│   ├── server.js
│   ├── package.json
│   └── .env
│
└── frontend/
    ├── public/
    │   └── index.html
    ├── src/
    │   ├── components/
    │   │   ├── Navbar.js
    │   │   └── PrivateRoute.js
    │   ├── context/
    │   │   └── AuthContext.js
    │   ├── pages/
    │   │   ├── Home.js
    │   │   ├── Login.js
    │   │   ├── Register.js
    │   │   ├── JobListing.js
    │   │   ├── JobDetails.js
    │   │   ├── DashboardSeeker.js
    │   │   ├── DashboardEmployer.js
    │   │   ├── Profile.js
    │   │   ├── CreateJob.js
    │   │   ├── EditJob.js
    │   │   ├── Applications.js
    │   │   └── SavedJobs.js
    │   ├── services/
    │   │   └── api.js
    │   ├── App.js
    │   ├── index.js
    │   └── index.css
    ├── package.json
    └── tailwind.config.js
```

## Installation & Setup

### Prerequisites
- Node.js (v14 or higher)
- MongoDB (local or cloud instance)
- npm or yarn

### Backend Setup

1. Navigate to the backend directory:
```bash
cd backend
```

2. Install dependencies:
```bash
npm install
```

3. Create a `.env` file in the backend directory:
```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/job-portal
JWT_SECRET=your_jwt_secret_key_here
NODE_ENV=development
UPLOAD_DIR=uploads
```

4. Start the backend server:
```bash
npm start
# or for development with auto-reload
npm run dev
```

The backend will run on `http://localhost:5000`

### Frontend Setup

1. Navigate to the frontend directory:
```bash
cd frontend
```

2. Install dependencies:
```bash
npm install
```

3. Create a `.env` file in the frontend directory (optional):
```env
REACT_APP_API_URL=http://localhost:5000/api
```

4. Start the React development server:
```bash
npm start
```

The frontend will run on `http://localhost:3000`

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user
- `GET /api/auth/me` - Get current user profile
- `PUT /api/auth/profile` - Update job seeker profile
- `PUT /api/auth/employer-profile` - Update employer profile
- `POST /api/auth/logout` - Logout

### Jobs
- `GET /api/jobs` - Get all jobs (with filters)
- `GET /api/jobs/:id` - Get job details
- `POST /api/jobs` - Create job (employer only)
- `PUT /api/jobs/:id` - Update job (employer only)
- `DELETE /api/jobs/:id` - Delete job (employer only)
- `GET /api/jobs/my-jobs/all` - Get employer's jobs
- `GET /api/jobs/featured/list` - Get featured jobs

### Applications
- `POST /api/applications` - Apply for job
- `GET /api/applications/my-applications` - Get job seeker's applications
- `GET /api/applications/job/:jobId/all` - Get job applications (employer)
- `GET /api/applications/employer/all-applications` - Get all applications (employer)
- `GET /api/applications/:id` - Get application details
- `PUT /api/applications/:id/status` - Update application status
- `DELETE /api/applications/:id` - Withdraw application

### Profiles
- `GET /api/profiles/me` - Get user profile
- `GET /api/profiles/:userId` - Get public profile
- `PUT /api/profiles/seeker/update` - Update seeker profile
- `PUT /api/profiles/employer/update` - Update employer profile
- `POST /api/profiles/upload-resume` - Upload resume
- `POST /api/profiles/upload-picture` - Upload profile picture
- `POST /api/profiles/upload-logo` - Upload company logo
- `POST /api/profiles/save-job/:jobId` - Save job
- `DELETE /api/profiles/save-job/:jobId` - Unsave job
- `GET /api/profiles/saved-jobs/all` - Get saved jobs

## User Roles

### Job Seeker
- Browse and search for jobs
- Apply for jobs
- Save jobs
- Upload resume and profile picture
- Track application status
- Update personal profile

### Employer
- Post, edit, and delete jobs
- View applications received
- Update application status
- Manage company profile
- Upload company logo

## Key Features Implementation

### Advanced Job Search
Filter jobs by:
- Job title and keywords
- Location
- Job type (Full-Time, Part-Time, etc.)
- Experience level
- Industry
- Salary range

### Application Management
- Job seekers can track all their applications
- Employers can review applications and update status
- Status history tracking
- Application withdrawal capability

### File Uploads
- Resume upload (PDF, DOC formats)
- Profile pictures (PNG, JPG formats)
- Company logos
- Files stored in `/uploads` directory

### Security
- JWT-based authentication
- Password hashing with bcryptjs
- Protected routes for authenticated users
- Role-based access control
- Secure API endpoints

## Database Schema

### User Model
```javascript
{
  firstName, lastName, email, password,
  phone, profilePicture, userType,
  // Job Seeker fields
  resume, bio, location, skills, experience,
  education, expectedSalary, availability,
  // Employer fields
  companyName, companyWebsite, companySize,
  industry, companyDescription, companyLogo,
  // Common
  createdAt, updatedAt, isActive
}
```

### Job Model
```javascript
{
  title, description, requirements,
  responsibilities, location, jobType,
  salaryRange, skills, experience, industry,
  employer, applicationDeadline, featured,
  applicationsCount, views, isActive,
  createdAt, updatedAt
}
```

### Application Model
```javascript
{
  job, applicant, coverLetter,
  status, appliedAt, updatedAt
}
```

## Environment Variables

### Backend (.env)
```
PORT=5000
MONGODB_URI=mongodb://localhost:27017/job-portal
JWT_SECRET=your_secret_key
NODE_ENV=development
UPLOAD_DIR=uploads
CLIENT_URL=http://localhost:3000
```

### Frontend (.env)
```
REACT_APP_API_URL=http://localhost:5000/api
```

## Running the Application

1. Start MongoDB
2. Start backend: `cd backend && npm start`
3. Start frontend: `cd frontend && npm start`
4. Open browser to `http://localhost:3000`

## Default Routes

- **Home**: `/`
- **Job Listings**: `/jobs`
- **Job Details**: `/jobs/:id`
- **Login**: `/login`
- **Register**: `/register`
- **Job Seeker Dashboard**: `/seeker/dashboard`
- **Employer Dashboard**: `/employer/dashboard`
- **User Profile**: `/profile`
- **Create Job**: `/create-job`
- **Saved Jobs**: `/saved-jobs`
- **Applications**: `/my-applications`

## Future Enhancements

- Email notifications for applications
- Advanced analytics dashboard
- Job recommendations based on profile
- Messaging between employers and job seekers
- Social authentication (Google, LinkedIn)
- Payment integration for featured job postings
- Video interview integration
- Advanced search with AI-powered recommendations
- Mobile app (React Native)

## Contributing

Contributions are welcome! Please follow the code style and submit pull requests with clear descriptions.

## License

This project is open source and available under the MIT License.

## Support

For issues or questions, please create an issue in the repository or contact the development team.

---

**Built with ❤️ for job seekers and employers worldwide**
