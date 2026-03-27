📚 Library Management System (LMS)
📝 Description

The Library Management System (LMS) is a full-stack web application designed to efficiently manage both physical and digital library operations within an academic environment. The system supports multiple user roles — Students, Librarians, and Faculty — and enables seamless interaction with library resources through a centralized platform.

It provides functionalities such as book borrowing, fine management, digital resource access, and analytics, ensuring an organized and scalable solution for modern libraries.

🚀 Features
👤 Student
User Registration & Authentication (Email-based)
Secure password validation (8–20 characters, standard constraints)
Access to:
Digital library (external resources like open-source book links)
Physical library catalog
Book Operations:
Search books (online/offline)
Borrow books (physical)
Return books
Reissue books (after due date)
Fine Management:
View calculated fines
Track overdue books
Insights Dashboard:
Time spent reading
Books accessed
Access to:
Notes (semester-wise & department-wise)
Previous exam papers
Study materials

📚 Librarian (Admin)
Staff Registration (Staff ID + Email)
Book Management:
Add, update, delete books
Manage inventory (quantity, availability)
Transaction Management:
Issue books
Accept returns
Approve/reject reissue requests
Fine System:
Calculate fines automatically
Collect and update fine status
Monitoring:
Track overdue books
View user borrowing history
Administrative Controls:
Set borrowing duration
Manage return deadlines
Generate reports (usage, fines, availability)

👨‍🏫 Faculty
Registration & Login (Faculty role)
Resource Management:
Upload notes (PDF)
Upload question papers & mock tests
Edit/Delete uploaded resources
Assign:
Book references
External learning links
Borrow books (same as student privileges)

📦 Core Modules
📘 Book Management

Each book includes:

Title
Author
Category
ISBN / ISSN
Edition
Publication Year
Quantity
Available Copies
Digital Access Link (if applicable)

🔄 Borrowing System
Student selects book → adds to cart
Borrow date is set manually
System automatically assigns:
Return deadline = 7 days
If overdue:
Fine is calculated automatically
After due date:
Student can:
Return book
Request reissue

💰 Fine Calculation
Fine is calculated based on delay:
Fine = Number of Late Days × Daily Fine Rate
Fine is visible to both:
Student
Librarian

📂 Digital Library
Open-source book links (no restrictions)
Faculty-uploaded resources:
Notes
Question papers
Mock tests
Organized by:
Department
Semester

📊 Analytics
Tracks:
Time spent on reading
Books accessed
Provides insights into user behavior

🗄️ Database Schema (MySQL)
Users Table
CREATE TABLE users (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    password VARCHAR(255),
    role ENUM('STUDENT', 'LIBRARIAN', 'FACULTY'),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
Books Table
CREATE TABLE books (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255),
    author VARCHAR(255),
    category VARCHAR(100),
    isbn VARCHAR(50),
    issn VARCHAR(50),
    edition VARCHAR(50),
    year INT,
    quantity INT,
    available INT,
    digital_link TEXT
);
Transactions Table
CREATE TABLE transactions (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    user_id BIGINT,
    book_id BIGINT,
    borrow_date DATE,
    due_date DATE,
    return_date DATE,
    status ENUM('BORROWED', 'RETURNED', 'OVERDUE'),
    fine DOUBLE DEFAULT 0,
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (book_id) REFERENCES books(id)
);
Resources Table
CREATE TABLE resources (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255),
    type ENUM('NOTES', 'PAPER', 'MOCK'),
    file_url TEXT,
    department VARCHAR(50),
    semester INT,
    uploaded_by BIGINT,
    FOREIGN KEY (uploaded_by) REFERENCES users(id)
);
Analytics Table
CREATE TABLE analytics (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    user_id BIGINT,
    book_id BIGINT,
    time_spent INT,
    accessed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

🧩 System Design
Architecture
Client (React)
      ↓
REST API (Spring Boot)
      ↓
Service Layer (Business Logic)
      ↓
Repository Layer (JPA/Hibernate)
      ↓
MySQL Database



Key Design Principles
Role-Based Access Control (RBAC)
RESTful API architecture
Layered backend structure
Separation of concerns
Scalable and modular design


🔌 API Endpoints
Authentication
POST /api/auth/register
POST /api/auth/login
Books
GET    /api/books
POST   /api/books
PUT    /api/books/{id}
DELETE /api/books/{id}
Transactions
POST /api/borrow
POST /api/return
POST /api/reissue
GET  /api/user/history
Resources
POST /api/resources/upload
GET  /api/resources

🛠️ Tech Stack
Layer	Technology
Frontend	React.js
Backend	Spring Boot
Database	MySQL
ORM	Hibernate (JPA)
Security	Spring Security
Build Tool	Maven

⚙️ Installation & Setup
Backend (Spring Boot)
Generate project using Spring Initializr with:
Spring Web
Spring Data JPA
MySQL Driver
Spring Security
Configure application.properties:
spring.datasource.url=jdbc:mysql://localhost:3306/lms
spring.datasource.username=root
spring.datasource.password=yourpassword

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
Run the application:
mvn spring-boot:run
Frontend (React)
Initialize React project:
npx create-react-app lms-frontend
cd lms-frontend
Install dependencies:
npm install axios react-router-dom

Start the application:
npm start
🔐 Security
Password encryption using BCrypt
Role-based authorization
Secure authentication endpoints
Input validation for all forms
🔮 Future Enhancements
Online payment gateway for fines
Email/SMS notifications
AI-based book recommendations
Real-time availability tracking
Mobile application support
📌 Conclusion

The Library Management System is a robust and scalable solution designed to streamline library operations by integrating digital and physical resource management. With a well-structured architecture and role-based functionality, it ensures efficiency, usability, and adaptability for modern educational institutions.
