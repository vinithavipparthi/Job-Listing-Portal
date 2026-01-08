# Job Listing Portal - Quick Reference Guide

## Quick Start (30 seconds)

```bash
# Terminal 1: Backend
cd backend
npm install
npm start

# Terminal 2: Frontend (in new terminal)
cd frontend
npm install
npm start
```

**Then open:** http://localhost:3000

## Key URLs

| Component | URL | Port |
|-----------|-----|------|
| Frontend | http://localhost:3000 | 3000 |
| Backend | http://localhost:5000 | 5000 |
| API Base | http://localhost:5000/api | 5000 |
| MongoDB | mongodb://localhost:27017 | 27017 |

## Test Accounts (After Seeding)

```
Job Seeker:
Email: seeker@test.com
Password: password123

Employer:
Email: employer@test.com
Password: password123
```

## Common Commands

### Backend
```bash
# Start development server
npm start

# Install dependencies
npm install

# Run with nodemon (auto-reload)
npm run dev
```

### Frontend
```bash
# Start development server
npm start

# Build for production
npm build

# Install dependencies
npm install

# Test
npm test
```

## File Locations

| Item | Path |
|------|------|
| User Model | `backend/models/User.js` |
| Job Model | `backend/models/Job.js` |
| Application Model | `backend/models/Application.js` |
| Auth Routes | `backend/routes/auth.js` |
| Job Routes | `backend/routes/jobs.js` |
| App Routes | `backend/routes/applications.js` |
| Profile Routes | `backend/routes/profiles.js` |
| Uploaded Files | `backend/uploads/` |
| Home Page | `frontend/src/pages/Home.js` |
| API Service | `frontend/src/services/api.js` |
| Auth Context | `frontend/src/context/AuthContext.js` |

## Environment Variables

### Backend (.env)
```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/job-portal
JWT_SECRET=your_secret_key
NODE_ENV=development
UPLOAD_DIR=uploads
CLIENT_URL=http://localhost:3000
```

### Frontend (.env - Optional)
```env
REACT_APP_API_URL=http://localhost:5000/api
```

## Routes

### Public Routes
- `/` - Home
- `/login` - Login page
- `/register` - Registration page
- `/jobs` - Browse jobs

### Protected Routes (Job Seeker)
- `/seeker/dashboard` - Job seeker dashboard
- `/my-applications` - View applications
- `/saved-jobs` - Saved jobs
- `/profile` - Profile page

### Protected Routes (Employer)
- `/employer/dashboard` - Employer dashboard
- `/create-job` - Create new job
- `/edit-job/:id` - Edit job
- `/job/:id/applications` - View applications

## Main Features

### Authentication
- User registration (Job Seeker / Employer)
- User login
- JWT token handling
- Profile management

### Jobs
- Post jobs (employers)
- Search jobs (all users)
- Filter by location, type, salary
- View job details
- Edit/Delete jobs (employers)

### Applications
- Apply for jobs (job seekers)
- Track application status
- Review applicants (employers)
- Update application status

### Profiles
- Upload resume (seekers)
- Upload company logo (employers)
- Update profile information
- Save/unsave jobs

## API Quick Reference

### Auth
```
POST   /api/auth/register
POST   /api/auth/login
GET    /api/auth/me
PUT    /api/auth/profile
POST   /api/auth/logout
```

### Jobs
```
GET    /api/jobs?search=&location=&jobType=
GET    /api/jobs/:id
POST   /api/jobs
PUT    /api/jobs/:id
DELETE /api/jobs/:id
GET    /api/jobs/my-jobs/all
```

### Applications
```
POST   /api/applications
GET    /api/applications/my-applications
GET    /api/applications/:id
PUT    /api/applications/:id/status
DELETE /api/applications/:id
```

### Profiles
```
GET    /api/profiles/me
PUT    /api/profiles/seeker/update
POST   /api/profiles/upload-resume
POST   /api/profiles/upload-picture
POST   /api/profiles/save-job/:jobId
```

## Troubleshooting Quick Fixes

| Problem | Solution |
|---------|----------|
| `ECONNREFUSED` | MongoDB not running: `mongod` |
| `EADDRINUSE` | Port in use: Change PORT in .env |
| `Cannot find module` | Run `npm install` in that directory |
| `CORS error` | Check CLIENT_URL in backend .env |
| `401 Unauthorized` | Token expired or invalid, login again |
| `404 Not Found` | Route doesn't exist, check spelling |

## Important Files to Check

1. **Backend Server**: `backend/server.js`
2. **Authentication**: `backend/middleware/auth.js`
3. **User Model**: `backend/models/User.js`
4. **Frontend App**: `frontend/src/App.js`
5. **Auth Context**: `frontend/src/context/AuthContext.js`
6. **API Service**: `frontend/src/services/api.js`

## Useful Code Snippets

### Making API Call from Frontend
```javascript
import { jobsAPI } from '../services/api';

// Get all jobs
const response = await jobsAPI.getJobs({ search: 'Engineer', location: 'NYC' });
```

### Protected Component
```javascript
import PrivateRoute from '../components/PrivateRoute';

<PrivateRoute userType="jobSeeker">
  <DashboardSeeker />
</PrivateRoute>
```

### Using Auth Context
```javascript
import { useAuth } from '../context/AuthContext';

const { user, login, logout, isAuthenticated } = useAuth();
```

## Performance Tips

1. Use pagination in API calls
2. Filter jobs on backend, not frontend
3. Cache user profile
4. Lazy load images
5. Use React DevTools to check renders

## Security Reminders

- ✅ Never commit .env file
- ✅ Change JWT_SECRET in production
- ✅ Use HTTPS in production
- ✅ Validate all inputs
- ✅ Keep dependencies updated
- ✅ Use environment variables for secrets

## Development Workflow

1. Make changes
2. Save files (auto-reload enabled)
3. Test in browser
4. Check console for errors
5. Commit changes to git

## Next Steps After Setup

1. ✅ Create user accounts
2. ✅ Post a test job
3. ✅ Apply for a job
4. ✅ Update application status
5. ✅ Explore all features
6. ✅ Review the code
7. ✅ Customize as needed
8. ✅ Deploy when ready

## Useful Resources

- [MongoDB Docs](https://docs.mongodb.com/)
- [Express Guide](https://expressjs.com/)
- [React Docs](https://react.dev/)
- [JWT Intro](https://jwt.io/introduction)
- [Axios Docs](https://axios-http.com/)

## Database Quick Commands

### Check Collections
```
use job-portal
show collections
```

### View Documents
```
db.users.findOne()
db.jobs.find()
db.applications.find()
```

### Clear Database
```
use job-portal
db.dropDatabase()
```

## Git Commands

```bash
# Initialize repo
git init

# Add all files
git add .

# Commit changes
git commit -m "message"

# Push to remote
git push origin main

# See status
git status
```

## Frontend Components Structure

```
Navbar
├── Navigation links
└── User menu

Pages
├── Home
├── Login/Register
├── Job Listing
├── Job Details
└── Dashboards

Context
└── AuthContext (manages login state)

Services
└── api.js (all API calls)
```

## Backend Route Structure

```
/api/auth
├── register
├── login
├── me
├── profile
└── logout

/api/jobs
├── GET all
├── GET :id
├── POST create
├── PUT :id
└── DELETE :id

/api/applications
├── POST apply
├── GET my-applications
├── PUT :id/status
└── DELETE :id

/api/profiles
├── GET me
├── PUT update
├── POST upload
└── POST save-job
```

## Quick Debugging

### Check if MongoDB is running
```bash
# On Windows
tasklist | findstr mongo

# On Mac
brew services list

# Universal
ps aux | grep mongod
```

### Check if ports are in use
```bash
# Port 5000
lsof -i :5000

# Port 3000
lsof -i :3000
```

### View backend logs
Check terminal where `npm start` is running

### View frontend logs
Press F12 in browser → Console tab

## Deployment Preparation

1. Build frontend: `npm run build`
2. Change NODE_ENV to production
3. Update MONGODB_URI
4. Change JWT_SECRET
5. Configure CORS
6. Set CLIENT_URL
7. Upload to hosting service

---

**Quick Links**
- [Full Documentation](README.md)
- [Setup Guide](SETUP.md)
- [Project Summary](PROJECT_SUMMARY.md)

**Support:** Check the documentation or troubleshooting section
