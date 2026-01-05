# Job Portal Project

A comprehensive Django-based job portal application that connects employers and job seekers. The platform allows employers to post jobs and manage applications, while job seekers can apply for positions and track their applications.

## Table of Contents

- [Features](#features)
- [Technology Stack](#technology-stack)
- [Installation](#installation)
- [Configuration](#configuration)
- [Database Models](#database-models)
- [Views and Functionality](#views-and-functionality)
- [Templates](#templates)
- [URL Patterns](#url-patterns)
- [Usage](#usage)
- [Testing](#testing)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

## Features

### For Employers:
- User registration and authentication
- Company profile management
- Job posting with detailed requirements
- Job listing management (view, edit, delete)
- Applicant management (view applications, shortlist, reject)
- Application status tracking

### For Job Seekers:
- User registration and authentication
- Personal profile management
- Job browsing and searching
- Job application with resume upload
- Application status tracking
- Application history

### General Features:
- User authentication system
- Role-based access control (Employer/JobSeeker)
- File upload for resumes
- Responsive design
- Message system for user feedback

## Technology Stack

- **Backend**: Django 5.0+
- **Database**: SQLite (default), configurable for PostgreSQL/MySQL
- **Frontend**: HTML5, CSS3, Bootstrap (via templates)
- **Authentication**: Django's built-in authentication system
- **File Storage**: Django's file storage system

## Installation

### Prerequisites

- Python 3.8+
- pip (Python package manager)
- Virtual environment (recommended)

### Setup Instructions

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd job_portal_project
   ```

2. **Create and activate virtual environment:**
   ```bash
   python -m venv env
   # On Windows:
   env\Scripts\activate
   # On macOS/Linux:
   source env/bin/activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Apply database migrations:**
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

5. **Create superuser (optional):**
   ```bash
   python manage.py createsuperuser
   ```

6. **Run the development server:**
   ```bash
   python manage.py runserver
   ```

7. **Access the application:**
   Open your browser and go to `http://127.0.0.1:8000/`

## Configuration

### Settings

The main configuration is in `settings.py`. Key settings include:

- **DEBUG**: Set to `False` in production
- **ALLOWED_HOSTS**: Configure for production deployment
- **DATABASES**: Configure database connection
- **STATIC_URL**: Static files configuration
- **AUTH_USER_MODEL**: Custom user model (`job_portal_app.PortalUserModel`)

### Environment Variables

For production deployment, consider using environment variables for:
- SECRET_KEY
- Database credentials
- Email settings (if implementing email functionality)

## Database Models

### PortalUserModel
Custom user model extending Django's AbstractUser.

**Fields:**
- `user_type`: Choice field (Employer/JobSeeker)

### EmployerModel
Profile model for employers.

**Fields:**
- `employer`: OneToOneField to PortalUserModel
- `company_name`: CharField
- `address`: TextField

### JobSeekerModel
Profile model for job seekers.

**Fields:**
- `seeker`: OneToOneField to PortalUserModel
- `full_name`: CharField
- `contact_number`: CharField
- `last_education`: CharField
- `skills`: TextField

### JobPostModel
Model for job postings.

**Fields:**
- `posted_by`: ForeignKey to EmployerModel
- `job_title`: CharField
- `description`: CharField
- `skills_required`: CharField
- `salary`: IntegerField
- `deadline`: DateField
- `posted_date`: DateField (auto_now_add)

### ApplyJobModel
Model for job applications.

**Fields:**
- `applied_by`: ForeignKey to JobSeekerModel
- `job`: ForeignKey to JobPostModel
- `resume`: FileField
- `status`: Choice field (Pending/Shortlisted/Rejected)
- `applied_date`: DateField (auto_now_add)

## Views and Functionality

### Authentication Views

#### `register_func`
- Handles user registration
- Creates appropriate profile based on user type
- Redirects to login on success

#### `login_func`
- Handles user authentication
- Redirects to dashboard on success

#### `logout_func`
- Logs out the user
- Redirects to login page

### Profile Views

#### `profile`
- Displays user profile information

#### `update_profile`
- Allows users to update their profile
- Different forms for employers and job seekers

### Job Management Views

#### `job_list`
- Displays jobs based on user type
- Job seekers see all jobs, employers see their posted jobs

#### `add_job`
- Allows employers to create new job postings

#### `update_job`
- Allows employers to edit their job postings

#### `delete_job`
- Allows employers to delete their job postings

### Application Views

#### `applied_job`
- Handles job applications from job seekers
- Includes resume upload

#### `my_application`
- Shows job seeker's application history

#### `applicant_list`
- Shows employers all applicants for a specific job

#### `shortlisted` / `rejected`
- Allows employers to update application status

## Templates

### Base Templates

#### `base.html`
- Main layout template
- Includes navigation and common structure
- Modern dark theme design

#### `nav.html`
- Navigation bar component

#### `message.html`
- Message display component

### Authentication Templates

#### `login.html`
- User login form

#### `register.html`
- User registration form

### Main Templates

#### `dashboard.html`
- User dashboard after login

#### `profile.html`
- User profile display

#### `update-profile.html`
- Profile update form

### Job Templates

#### `job-list.html`
- List of jobs with actions

#### `add-job.html`
- Job creation form

#### `update-job.html`
- Job editing form

#### `applicant-list.html`
- List of applicants for a job

### Application Templates

#### `apply-job.html`
- Job application form with resume upload

#### `my-application.html`
- User's application history

## URL Patterns

| URL Pattern | View | Name | Description |
|-------------|------|------|-------------|
| `register/` | register_func | register_func | User registration |
| `` | login_func | login_func | User login (root URL) |
| `logout/` | logout_func | logout_func | User logout |
| `dashboard/` | dashboard | dashboard | User dashboard |
| `profile/` | profile | profile | User profile |
| `update-profile/` | update_profile | update_profile | Profile update |
| `job-list/` | job_list | job_list | Job listings |
| `add-job/` | add_job | add_job | Add new job |
| `update-job/<id>/` | update_job | update_job | Update job |
| `delete-job/<id>/` | delete_job | delete_job | Delete job |
| `apply-job/<id>/` | applied_job | applied_job | Apply for job |
| `my-applications/` | my_application | my_application | User's applications |
| `applicant-list/<id>/` | applicant_list | applicant_list | Job applicants |
| `shortlisted/<id>/` | shortlisted | shortlisted | Shortlist applicant |
| `rejected/<id>/` | rejected | rejected | Reject applicant |

## Usage

### For Employers

1. Register as an Employer
2. Complete company profile
3. Post jobs with requirements
4. View and manage applications
5. Update application status

### For Job Seekers

1. Register as a JobSeeker
2. Complete personal profile
3. Browse available jobs
4. Apply with resume upload
5. Track application status

## Testing

### Running Tests

```bash
python manage.py test
```

### Test Coverage

The project includes basic test structure in `job_portal_app/tests.py`. Consider expanding test coverage for:

- Model tests
- View tests
- Form validation tests
- Integration tests

## Deployment

### Production Checklist

1. Set `DEBUG = False`
2. Configure `ALLOWED_HOSTS`
3. Set up production database
4. Configure static files serving
5. Set up proper logging
6. Configure email settings (if needed)
7. Set up SSL certificate
8. Configure web server (nginx/apache)

### Example Production Settings

```python
DEBUG = False
ALLOWED_HOSTS = ['yourdomain.com', 'www.yourdomain.com']

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'job_portal_db',
        'USER': 'db_user',
        'PASSWORD': 'db_password',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}

STATIC_ROOT = BASE_DIR / 'staticfiles'
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Ensure all tests pass
6. Submit a pull request

### Code Style

- Follow PEP 8 Python style guide
- Use meaningful variable and function names
- Add docstrings to functions and classes
- Keep functions small and focused

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

For support and questions:
- Create an issue in the repository
- Contact the development team

## Future Enhancements

- Email notifications for application updates
- Advanced search and filtering for jobs
- Resume parsing and skill matching
- Interview scheduling system
- Rating and review system
- Mobile application
- Multi-language support
- Advanced analytics dashboard
