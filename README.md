# Crowd_Funding_Guvi
A modern crowdfunding app built with Java and web tech. Create, manage, and contribute to fundraising campaigns for students, startups, and causes. Features  transparent dashboards, and a user-friendly experience to drive community-powered impact. Crowdfunding Platform is a full-stack web application built with Spring Boot that allows users to create crowdfunding campaigns, back projects, and track funding progress in real-time. Features complete CRUD operations with JPA entities, RESTful APIs, and a responsive frontend interface.


### USAGE AND FUNCTIONALITIES

 ##  Features


 - **User Authentication - Register/Login with email validation

 - **Project Management - Create, view, and track campaigns

 - **Real-time Funding - Contributions update project progress instantly

 - **Auto Status Updates - Projects auto-transition (ACTIVE → FUNDED/EXPIRED)

 - **Responsive UI - Modern single-page application

 - **H2 Database - Embedded database for instant setup

- **REST APIs - Clean, documented endpoints

## Technology Stack

- **Backend**:	Spring Boot 3.3.4, Spring Data JPA, Hibernate
- **Database**:	H2 Database (embedded)
- **Frontend**:	HTML5, CSS3, Vanilla JavaScript, Fetch API
- **Build Tool**:	Maven
- **Java Version**:	17

## Prerequisites
- **Java 17 or higher
- **Maven 3.6+
- **Any IDE (IntelliJ IDEA / VS Code / Eclipse)

## Installation

### 1. Clone the repository

```bash
 git clone https://github.com/Ronit-Raj98/crowdfunding-platform.git
cd crowdfunding-platform
```

### 2. Install dependencies
```bash
mvn clean install
```
### 3. Run the application
```bash
mvn spring-boot:run
```

### 4. Access the Application

Open your browser and go to

 Frontend:    http://localhost:8080/frontend.html
 H2 Console:  http://localhost:8080/h2-console
 API Base:    http://localhost:8080/api/

## Project Structure

```
crowdfunding-platform/
├── src/
│   └── main/
│       ├── java/com/crowdfunding/
│       │   ├── CrowdfundingApplication.java
│       │   ├── entity/
│       │   │   ├── User.java
│       │   │   ├── Project.java
│       │   │   ├── ProjectStatus.java
│       │   │   └── Contribution.java
│       │   ├── repository/
│       │   │   ├── UserRepository.java
│       │   │   ├── ProjectRepository.java
│       │   │   └── ContributionRepository.java
│       │   ├── service/
│       │   │   ├── UserService.java
│       │   │   ├── ProjectService.java
│       │   │   └── ContributionService.java
│       │   └── controller/
│       │       ├── UserController.java
│       │       ├── ProjectController.java
│       │       └── ContributionController.java
│       └── resources/
│           └── application.properties
├── pom.xml
└── frontend.html

```
## User Registration
```
{
  "fullName": "John Doe",
  "email": "john@example.com",
  "password": "password123",
  "phoneNumber": "+1234567890"
}
```
## Login & Get User ID
```
POST http://localhost:8080/api/users/login
{
  "email": "john@example.com",
  "password": "password123"
}
```
## Future Enhancements
 
 - **WT Token Authentication

 - **MySQL/PostgreSQL Production Database

 - **Image Upload for Projects

 - **Email Notifications

 - **Payment Gateway (Razorpay/Stripe)

 - **Admin Dashboard

 - **Advanced Search & Filters

 - **Project Categories & Tags

