# Job Listing Portal - Setup Guide

## Quick Start Guide

This guide will help you set up and run the Job Listing Portal on your local machine.

## Prerequisites

- **Node.js**: Version 14.0 or higher ([Download](https://nodejs.org/))
- **MongoDB**: Local installation or [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) cloud account
- **Git**: For version control ([Download](https://git-scm.com/))
- **npm**: Comes with Node.js

## Step 1: Clone the Project

```bash
# If you have git
git clone <repository-url>
cd Job\ Listing\ Portal

# Or manually navigate to the project folder
cd "path/to/Job Listing Portal"
```

## Step 2: MongoDB Setup

### Option A: Local MongoDB
1. [Download and install MongoDB](https://docs.mongodb.com/manual/installation/)
2. Start MongoDB service:
   ```bash
   # On Windows (if installed as service)
   net start MongoDB
   
   # On Mac (if installed via brew)
   brew services start mongodb-community
   
   # Or run mongod directly
   mongod
   ```

### Option B: MongoDB Atlas (Cloud)
1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free account
3. Create a cluster
4. Get your connection string: `mongodb+srv://username:password@cluster.mongodb.net/job-portal`

## Step 3: Backend Setup

1. Navigate to backend directory:
```bash
cd backend
```

2. Install dependencies:
```bash
npm install
```

3. Create `.env` file with the following content:
```env
# Server
PORT=5000
NODE_ENV=development

# Database
MONGODB_URI=mongodb://localhost:27017/job-portal
# Or for MongoDB Atlas:
# MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/job-portal

# JWT
JWT_SECRET=your_super_secret_jwt_key_change_this_in_production

# File Upload
UPLOAD_DIR=uploads

# Client URL (for CORS)
CLIENT_URL=http://localhost:3000
```

4. Start the backend server:
```bash
npm start
```

You should see:
```
✓ MongoDB connected successfully
✓ Server running on http://localhost:5000
✓ Environment: development
```

## Step 4: Frontend Setup

In a new terminal/command prompt:

1. Navigate to frontend directory:
```bash
cd frontend
```

2. Install dependencies:
```bash
npm install
```

3. Create `.env` file (optional, uses default):
```env
REACT_APP_API_URL=http://localhost:5000/api
```

4. Start the React development server:
```bash
npm start
```

The application should automatically open at `http://localhost:3000`

## Step 5: Test the Application

### Create a Job Seeker Account
1. Click "Register" on the homepage
2. Select "Job Seeker" as the user type
3. Fill in your details and click "Create Account"
4. You'll be redirected to the Job Seeker dashboard

### Create an Employer Account
1. Click "Register" again in a new window/tab
2. Select "Employer" as the user type
3. Fill in company details and click "Create Account"
4. You'll be redirected to the Employer dashboard

### Test Job Creation & Application
1. **As Employer**: Go to "Create Job" and post a test job
2. **As Job Seeker**: Search for the job and apply with a cover letter
3. **As Employer**: View applications in your dashboard and update their status

## Troubleshooting

### MongoDB Connection Error
```
Error: MongoDB connection error
```
**Solution**: 
- Ensure MongoDB is running (`mongod` command)
- Check your MONGODB_URI in `.env`
- For Atlas, verify IP whitelist includes your IP

### Port Already in Use
```
Error: listen EADDRINUSE: address already in use :::5000
```
**Solution**:
- Change PORT in `.env` to a different port (e.g., 5001)
- Or kill the process using the port

### CORS Error
```
Access to XMLHttpRequest blocked by CORS policy
```
**Solution**:
- Ensure CLIENT_URL in backend `.env` matches frontend URL
- Restart backend server

### Frontend Can't Connect to Backend
```
Network Error connecting to http://localhost:5000/api
```
**Solution**:
- Ensure backend is running on port 5000
- Check that REACT_APP_API_URL in frontend `.env` is correct
- Try accessing `http://localhost:5000/api/health` in browser

### npm install Issues
```
npm ERR! code ERESOLVE
```
**Solution**:
```bash
# Use npm 8 or higher
npm install --legacy-peer-deps
```

## Project Structure Overview

```
Job Listing Portal/
├── backend/              # Express server
│   ├── models/          # MongoDB schemas
│   ├── routes/          # API endpoints
│   ├── middleware/      # Auth & validation
│   ├── uploads/         # File storage
│   ├── server.js        # Main server file
│   └── package.json
│
├── frontend/            # React app
│   ├── public/          # Static files
│   ├── src/
│   │   ├── components/  # Reusable components
│   │   ├── pages/       # Page components
│   │   ├── context/     # React context (Auth)
│   │   ├── services/    # API calls
│   │   └── App.js       # Root component
│   └── package.json
│
└── README.md           # This file
```

## API Endpoints Quick Reference

### Auth Endpoints
- `POST /api/auth/register` - Register user
- `POST /api/auth/login` - Login user
- `GET /api/auth/me` - Get current user

### Job Endpoints
- `GET /api/jobs` - List all jobs
- `POST /api/jobs` - Create job (employer)
- `PUT /api/jobs/:id` - Update job (employer)
- `DELETE /api/jobs/:id` - Delete job (employer)

### Application Endpoints
- `POST /api/applications` - Apply for job
- `GET /api/applications/my-applications` - My applications
- `PUT /api/applications/:id/status` - Update status

### Profile Endpoints
- `GET /api/profiles/me` - Get my profile
- `PUT /api/profiles/seeker/update` - Update seeker profile
- `POST /api/profiles/upload-resume` - Upload resume

## Common Tasks

### Reset Database
```bash
# Delete MongoDB database named 'job-portal'
# In MongoDB shell:
use job-portal
db.dropDatabase()
```

### View Uploaded Files
```
# Files are stored in backend/uploads/
backend/uploads/
  ├── resume-*.pdf
  ├── picture-*.jpg
  └── logo-*.png
```

### Change Application Port
Edit `backend/.env`:
```env
PORT=5001  # Change this
```

## Deployment Checklist

Before deploying to production:

- [ ] Change JWT_SECRET to a strong random string
- [ ] Update MONGODB_URI to production database
- [ ] Set NODE_ENV=production
- [ ] Set CLIENT_URL to production frontend URL
- [ ] Enable HTTPS
- [ ] Configure environment variables on hosting service
- [ ] Set up proper error logging
- [ ] Configure file upload limits
- [ ] Set up database backups
- [ ] Enable CORS properly for production

## Performance Tips

1. **Database Indexing**: Indexes are already set on frequently searched fields
2. **Pagination**: Job listings use pagination (10 jobs per page)
3. **Lazy Loading**: Images load as needed
4. **API Optimization**: Multiple endpoints accept filters

## Security Considerations

- ✅ Password hashing with bcryptjs
- ✅ JWT-based authentication
- ✅ Protected API routes
- ✅ CORS enabled
- ✅ File upload validation
- ✅ Role-based access control

## Additional Resources

- [MongoDB Documentation](https://docs.mongodb.com/)
- [Express.js Guide](https://expressjs.com/)
- [React Documentation](https://react.dev/)
- [JWT Documentation](https://jwt.io/)

## Getting Help

If you encounter issues:
1. Check the troubleshooting section above
2. Review application logs in terminal
3. Check browser console (F12) for frontend errors
4. Verify all services are running (MongoDB, backend, frontend)

## Next Steps

After setup:
1. Explore the application as both a job seeker and employer
2. Test job posting and application features
3. Review the code structure
4. Customize styling and branding
5. Deploy to production when ready

---

**Happy coding! 🚀**
