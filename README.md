# 📚 Teacher Attendance System with Face Verification

A comprehensive web-based teacher attendance management system with face verification capabilities, real-time dashboards, and administrative override features.

🌐 **Live Demo:** [https://teacherattendance.altechspheresolns.com/](https://teacherattendance.altechspheresolns.com/)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Live Demo Access](#live-demo-access)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [System Requirements](#system-requirements)
- [Installation Guide](#installation-guide)
- [Database Setup](#database-setup)
- [Configuration](#configuration)
- [Folder Structure](#folder-structure)
- [Usage Guide](#usage-guide)
- [API Endpoints](#api-endpoints)
- [Troubleshooting](#troubleshooting)
- [Security Considerations](#security-considerations)
- [Future Enhancements](#future-enhancements)
- [License](#license)

---

## 🎯 Overview

The **Teacher Attendance System** automates the process of tracking teacher attendance in educational institutions. It features:

- 📸 **Webcam-based check-in** with face capture
- 🔍 **Face verification module** for admin review
- 📊 **Real-time dashboards** with live statistics
- 📈 **Monthly attendance reports** with visual progress bars
- 👥 **Role-based access** (Admin / Teacher)
- 🎨 **Customizable UI** (sidebar color, font, system name)
- ✍️ **Admin override capability** with notes

---

## 🌐 Live Demo Access

You can test the system immediately at:

**URL:** [https://teacherattendance.altechspheresolns.com/](https://teacherattendance.altechspheresolns.com/)

### Demo Credentials

| Role | Email | Password |
|------|-------|----------|
| **Admin** | `admin@example.com` | `admin123` |
| **Teacher** | `teacher@example.com` | `teacher123` |

> ⚠️ **Note:** The demo is for evaluation purposes only. Face recognition uses simulated matching. For full functionality, install locally.

---

## ✨ Features

### For Teachers
- Mark daily attendance with webcam capture
- View personal attendance history
- Check attendance status (Present/Late/Absent)
- View monthly reports

### For Admins
- **Dashboard** - Live statistics (Present, Absent, Late counts)
- **Manage Teachers** - Add, edit, delete teacher profiles with photos
- **Face Verification** - Compare captured faces with database photos, override mismatches
- **Reports** - Generate monthly attendance summaries with progress bars
- **Settings** - Customize system name, sidebar color, font family and size
- **Bulk Verification** - Verify multiple attendance records at once

### Face Verification Features
- Match score calculation (0-100%)
- Visual side-by-face comparison
- Admin override with mandatory notes
- Verification audit trail (who verified when)

---

## 🛠 Technology Stack

| Component | Technology |
|-----------|------------|
| **Backend** | PHP 7.4+ / 8.x |
| **Database** | MySQL 5.7+ / MariaDB |
| **Frontend** | HTML5, CSS3, JavaScript (ES6) |
| **UI Framework** | Bootstrap 5, Bootstrap Icons |
| **Camera API** | WebRTC / getUserMedia |
| **Image Processing** | PHP GD Library |
| **Build Tool** | Vite (optional) |

---

## 💻 System Requirements

### Local Installation
- PHP 7.4 or higher (with MySQLi and GD extensions enabled)
- MySQL 5.7+ or MariaDB 10.2+
- Apache / Nginx web server
- Modern browser with WebRTC support (Chrome, Firefox, Edge)
- HTTPS recommended (required for camera access on non-localhost domains)

### Hosting Requirements
- PHP memory limit: 128MB minimum
- Upload max filesize: 10MB
- Post max size: 10MB
- Allow URL fopen: On (for some features)

---

## 📥 Installation Guide

### Option 1: Quick Local Setup (XAMPP/WAMP/MAMP)

1. **Download and extract the zip file**
   ```bash
   unzip Face Verification Attendance Tracker.zip
   # or extract manually
Copy to web root

XAMPP: C:\xampp\htdocs\attendance\

WAMP: C:\wamp\www\attendance\

MAMP: /Applications/MAMP/htdocs/attendance/

Linux: /var/www/html/attendance/

Start your web server and MySQL

Import the database

Open phpMyAdmin (http://localhost/phpmyadmin)

Create a new database: teacher_attendance

Import teacher_attendance_database.sql file that is pasted in this repository

Configure database connection

Edit config/database.php:

php
define('DB_HOST', 'localhost');
define('DB_USER', 'root');        // your database username
define('DB_PASS', '');            // your database password
define('DB_NAME', 'teacher_attendance');
Set folder permissions

bash
# Linux/Mac
chmod 755 uploads/
chmod 755 uploads/teachers/
chmod 755 uploads/attendance/

# Windows - usually not required
Access the system

Open browser: http://localhost/attendance/

Register first admin at: http://localhost/attendance/register_admin.php

Option 2: Using Docker (if available)
bash
# Build and run with Docker Compose
docker-compose up -d

# Access at http://localhost:8080
Option 3: Deploy to Shared Hosting (cPanel)
Upload files via File Manager or FTP to your public_html folder

Create MySQL database in cPanel

Import database via phpMyAdmin

Update config/database.php with your hosting database credentials

Set proper permissions (755 for folders, 644 for files)

🗄️ Database Setup
Step 1: Create Database
sql
CREATE DATABASE teacher_attendance
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
Step 2: Import Schema
Run the database/schema.sql file:

bash
mysql -u username -p teacher_attendance < database/schema.sql
Or import via phpMyAdmin.

Step 3: Database Tables
The system creates 6 tables:

Table	Description
users	Admin and teacher login accounts
teachers	Teacher profiles with photos
attendance	Daily check-in/check-out records
face_verification_logs	Face matching scores and admin overrides
reports	Monthly attendance summaries (cached)
system_settings	UI customization settings
admin_verifications	Email verification codes
⚙️ Configuration
Database Configuration (config/database.php)
php
<?php
define('DB_HOST', 'localhost');
define('DB_USER', 'your_username');
define('DB_PASS', 'your_password');
define('DB_NAME', 'teacher_attendance');

$mysqli = new mysqli(DB_HOST, DB_USER, DB_PASS, DB_NAME);
$mysqli->set_charset("utf8");

if ($mysqli->connect_error) {
    die("Connection failed: " . $mysqli->connect_error);
}
?>
System Settings
Once installed, admins can customize via Settings page:

System Name (appears in header)

Sidebar Color (hex code like #2c3e50)

Font Family (Arial, Inter, Roboto, etc.)

Font Size (px, rem, or em)

Attendance Threshold
The "Late" threshold is set to 08:30 AM. To modify:

php
// In api/mark_attendance.php
$late_time = '08:30:00';  // Change this value
📁 Folder Structure
text
teacher-attendance-system/
│
├── api/                          # REST API endpoints
│   └── face_verification_api.php # Face verification operations
│
├── assets/                       # Static assets
│   ├── css/
│   └── js/
│
├── config/                       # Configuration files
│   └── database.php
│
├── database/                     # Database schema
│   └── schema.sql
│
├── includes/                     # Shared PHP components
│   ├── auth.php
│   ├── header.php
│   ├── sidebar.php
│   └── footer.php
│
├── uploads/                      # Uploaded files (writable)
│   ├── teachers/                 # Teacher profile photos
│   └── attendance/               # Captured face images
│
├── index.php                     # Redirect to login
├── login.php                     # Authentication page
├── register_admin.php            # First admin registration
├── logout.php                    # Session destroy
├── dashboard.php                 # Live statistics
├── attendance.php                # Webcam check-in
├── teachers.php                  # Teacher management (admin)
├── reports.php                   # Monthly reports
├── compare.php                   # Face verification (admin)
├── settings.php                  # System settings (admin)
│
└── README.md                     # This file
🚀 Usage Guide
First Time Setup
Register Admin Account

Navigate to: http://yourdomain.com/register_admin.php

Fill in your details (email and password)

You'll receive a verification code (check your email or database for testing)

Login

Go to login.php

Enter admin email and password

Add Teachers

Go to Manage Teachers

Click "Add Teacher"

Fill in details and upload a photo (used for face verification)

Customize Settings (Optional)

Go to Settings

Change system name, sidebar color, fonts

Daily Operations
For Teachers:
Login with teacher credentials

Go to Attendance page

Select your name from dropdown

Allow camera access

Click "Capture & Check In"

System records check-in time and status (Present/Late)

For Admins:
Dashboard - Monitor today's attendance

Face Verification - Review low-match attendance records

Click "Verify" on any record

Compare captured face vs database photo

Choose "Confirm Match" or "Mark as Not Matching"

Add notes (required for mismatches)

Reports - Generate monthly summaries

🔌 API Endpoints
All endpoints are accessed via api/face_verification_api.php

Action	Method	Parameters	Description
get_teachers	GET	none	List all teachers
get_attendances	GET	status, teacher_id, date, attendance_status	Filtered attendance records
get_verification_details	GET	attendance_id, teacher_id	Details for verification modal
override_attendance	POST	attendance_id, teacher_id, confirm_match, admin_notes	Admin override decision
get_stats	GET	none	Overall statistics
bulk_verify	POST	attendance_ids[], confirm_match	Bulk verification
Example API Call
javascript
// Fetch attendance records
fetch('api/face_verification_api.php?action=get_attendances&status=unverified')
  .then(response => response.json())
  .then(data => console.log(data));
🔧 Troubleshooting
Common Issues and Solutions
Issue	Solution
White screen after login	Check PHP error logs. Enable display_errors in php.ini
Database connection error	Verify credentials in config/database.php
Camera not working	Use HTTPS or localhost. Check browser permissions
Upload fails	Check folder permissions (755) and PHP upload limits
Face comparison errors	Ensure GD library is enabled in PHP
404 errors	Check if mod_rewrite is enabled (Apache)
Session timeout too quick	Increase session.gc_maxlifetime in php.ini
Debug Mode
Enable debug mode in api/face_verification_api.php:

php
error_reporting(E_ALL);
ini_set('display_errors', 1);
Log Files
Check these locations for errors:

PHP logs: /var/log/php_errors.log (Linux) or XAMPP/WAMP logs

Web server logs: Apache/Nginx error logs

Browser console: F12 > Console tab

🔒 Security Considerations
Implemented Security Features
✅ Password hashing (bcrypt)

✅ Prepared statements (SQL injection prevention)

✅ HTML escaping (XSS protection)

✅ Role-based access control

✅ File extension validation

✅ Session-based authentication

Recommendations for Production
🔐 Enable HTTPS (required for camera on non-localhost)

🔐 Move config/database.php outside web root

🔐 Implement CSRF tokens on forms

🔐 Add rate limiting on login attempts

🔐 Set proper file permissions (755 for folders, 644 for files)

🔐 Disable display_errors in production

🔐 Add MIME type validation for uploads

🔐 Implement session regeneration on login

.htaccess Security (Apache)
apache
# Force HTTPS
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]

# Security Headers
Header set X-Frame-Options "DENY"
Header set X-Content-Type-Options "nosniff"
Header set X-XSS-Protection "1; mode=block"
🚧 Future Enhancements
Integration with face-api.js for real face recognition

Check-out functionality with time tracking

Email notifications for low match scores

PDF/CSV report exports

Mobile responsive enhancements

Multi-branch/school support

Push notifications

API documentation with Swagger

📝 License
This project is licensed under the MIT License - see below:

text
MIT License

Copyright (c) 2026 Altech Sphere Solutions

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions...

Full license text available in LICENSE file.
🙏 Support
For issues, questions, or contributions:

📧 Email: support@altechspheresolns.com

🌐 Website: https://altechspheresolns.com

🐛 Issue Tracker: GitHub Issues

📊 Quick Start Commands
bash
# Clone repository
git clone https://github.com/yourusername/teacher-attendance-system.git

# Copy files to web server
cp -r teacher-attendance-system/* /var/www/html/attendance/

# Set permissions (Linux)
chmod -R 755 uploads/
chmod 644 config/database.php

# Import database
mysql -u root -p teacher_attendance < database/schema.sql

# Start using!
# Visit: http://localhost/attendance/


Last Updated: April 2026 | Version 2.0.0

text

This README.md file includes:

1. **Live demo URL** with credentials
2. **Complete installation instructions** for local and hosting environments
3. **Database setup guide**
4. **Folder structure** reference
5. **API documentation** for your verification endpoints
6. **Troubleshooting** common issues
7. **Security recommendations**
8. **Quick start commands**
