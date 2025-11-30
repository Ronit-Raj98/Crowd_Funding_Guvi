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
## JDBC Implementation Notes

- **Main Application**:Crowdfunding  Application
```
// CrowdfundingApplication.java
package com.crowdfunding;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class CrowdfundingApplication {
    public static void main(String[] args) {
        SpringApplication.run(CrowdfundingApplication.class, args);
    }
}

// ============================================
// ENTITY CLASSES
// ============================================

// User.java
package com.crowdfunding.entity;

import jakarta.persistence.*;
import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;

@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(unique = true, nullable = false)
    private String email;
    
    @Column(nullable = false)
    private String password;
    
    @Column(nullable = false)
    private String fullName;
    
    private String phoneNumber;
    
    @Column(nullable = false)
    private LocalDateTime createdAt = LocalDateTime.now();
    
    @OneToMany(mappedBy = "creator", cascade = CascadeType.ALL)
    private List<Project> createdProjects = new ArrayList<>();
    
    @OneToMany(mappedBy = "backer", cascade = CascadeType.ALL)
    private List<Contribution> contributions = new ArrayList<>();

    // Constructors
    public User() {}
    
    public User(String email, String password, String fullName) {
        this.email = email;
        this.password = password;
        this.fullName = fullName;
    }

    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
    
    public String getPassword() { return password; }
    public void setPassword(String password) { this.password = password; }
    
    public String getFullName() { return fullName; }
    public void setFullName(String fullName) { this.fullName = fullName; }
    
    public String getPhoneNumber() { return phoneNumber; }
    public void setPhoneNumber(String phoneNumber) { this.phoneNumber = phoneNumber; }
    
    public LocalDateTime getCreatedAt() { return createdAt; }
    public void setCreatedAt(LocalDateTime createdAt) { this.createdAt = createdAt; }
    
    public List<Project> getCreatedProjects() { return createdProjects; }
    public void setCreatedProjects(List<Project> createdProjects) { this.createdProjects = createdProjects; }
    
    public List<Contribution> getContributions() { return contributions; }
    public void setContributions(List<Contribution> contributions) { this.contributions = contributions; }
}

// Project.java
package com.crowdfunding.entity;

import jakarta.persistence.*;
import java.math.BigDecimal;
import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;

@Entity
@Table(name = "projects")
public class Project {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String title;
    
    @Column(length = 2000)
    private String description;
    
    @Column(nullable = false)
    private BigDecimal goalAmount;
    
    @Column(nullable = false)
    private BigDecimal currentAmount = BigDecimal.ZERO;
    
    @Column(nullable = false)
    private LocalDateTime deadline;
    
    @Enumerated(EnumType.STRING)
    private ProjectStatus status = ProjectStatus.ACTIVE;
    
    @Column(nullable = false)
    private LocalDateTime createdAt = LocalDateTime.now();
    
    @ManyToOne
    @JoinColumn(name = "creator_id", nullable = false)
    private User creator;
    
    @OneToMany(mappedBy = "project", cascade = CascadeType.ALL)
    private List<Contribution> contributions = new ArrayList<>();
    
    private String category;

    // Constructors
    public Project() {}
    
    public Project(String title, String description, BigDecimal goalAmount, LocalDateTime deadline, User creator) {
        this.title = title;
        this.description = description;
        this.goalAmount = goalAmount;
        this.deadline = deadline;
        this.creator = creator;
    }

    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    
    public String getTitle() { return title; }
    public void setTitle(String title) { this.title = title; }
    
    public String getDescription() { return description; }
    public void setDescription(String description) { this.description = description; }
    
    public BigDecimal getGoalAmount() { return goalAmount; }
    public void setGoalAmount(BigDecimal goalAmount) { this.goalAmount = goalAmount; }
    
    public BigDecimal getCurrentAmount() { return currentAmount; }
    public void setCurrentAmount(BigDecimal currentAmount) { this.currentAmount = currentAmount; }
    
    public LocalDateTime getDeadline() { return deadline; }
    public void setDeadline(LocalDateTime deadline) { this.deadline = deadline; }
    
    public ProjectStatus getStatus() { return status; }
    public void setStatus(ProjectStatus status) { this.status = status; }
    
    public LocalDateTime getCreatedAt() { return createdAt; }
    public void setCreatedAt(LocalDateTime createdAt) { this.createdAt = createdAt; }
    
    public User getCreator() { return creator; }
    public void setCreator(User creator) { this.creator = creator; }
    
    public List<Contribution> getContributions() { return contributions; }
    public void setContributions(List<Contribution> contributions) { this.contributions = contributions; }
    
    public String getCategory() { return category; }
    public void setCategory(String category) { this.category = category; }
    
    public double getProgressPercentage() {
        if (goalAmount.compareTo(BigDecimal.ZERO) == 0) return 0;
        return currentAmount.divide(goalAmount, 4, BigDecimal.ROUND_HALF_UP)
                           .multiply(BigDecimal.valueOf(100))
                           .doubleValue();
    }
}

// ProjectStatus.java
package com.crowdfunding.entity;

public enum ProjectStatus {
    ACTIVE,
    FUNDED,
    EXPIRED,
    CANCELLED
}

// Contribution.java
package com.crowdfunding.entity;

import jakarta.persistence.*;
import java.math.BigDecimal;
import java.time.LocalDateTime;

@Entity
@Table(name = "contributions")
public class Contribution {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne
    @JoinColumn(name = "project_id", nullable = false)
    private Project project;
    
    @ManyToOne
    @JoinColumn(name = "backer_id", nullable = false)
    private User backer;
    
    @Column(nullable = false)
    private BigDecimal amount;
    
    @Column(nullable = false)
    private LocalDateTime contributedAt = LocalDateTime.now();
    
    private String message;

    // Constructors
    public Contribution() {}
    
    public Contribution(Project project, User backer, BigDecimal amount) {
        this.project = project;
        this.backer = backer;
        this.amount = amount;
    }

    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    
    public Project getProject() { return project; }
    public void setProject(Project project) { this.project = project; }
    
    public User getBacker() { return backer; }
    public void setBacker(User backer) { this.backer = backer; }
    
    public BigDecimal getAmount() { return amount; }
    public void setAmount(BigDecimal amount) { this.amount = amount; }
    
    public LocalDateTime getContributedAt() { return contributedAt; }
    public void setContributedAt(LocalDateTime contributedAt) { this.contributedAt = contributedAt; }
    
    public String getMessage() { return message; }
    public void setMessage(String message) { this.message = message; }
}

// ============================================
// REPOSITORY INTERFACES
// ============================================

// UserRepository.java
package com.crowdfunding.repository;

import com.crowdfunding.entity.User;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.Optional;

public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
    boolean existsByEmail(String email);
}

// ProjectRepository.java
package com.crowdfunding.repository;

import com.crowdfunding.entity.Project;
import com.crowdfunding.entity.ProjectStatus;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.List;

public interface ProjectRepository extends JpaRepository<Project, Long> {
    List<Project> findByStatus(ProjectStatus status);
    List<Project> findByCreatorId(Long creatorId);
    List<Project> findByCategory(String category);
}

// ContributionRepository.java
package com.crowdfunding.repository;

import com.crowdfunding.entity.Contribution;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.List;

public interface ContributionRepository extends JpaRepository<Contribution, Long> {
    List<Contribution> findByProjectId(Long projectId);
    List<Contribution> findByBackerId(Long backerId);
}

// ============================================
// SERVICE CLASSES
// ============================================

// UserService.java
package com.crowdfunding.service;

import com.crowdfunding.entity.User;
import com.crowdfunding.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;
import java.util.Optional;

@Service
public class UserService {
    
    @Autowired
    private UserRepository userRepository;
    
    public User registerUser(User user) {
        if (userRepository.existsByEmail(user.getEmail())) {
            throw new RuntimeException("Email already exists");
        }
        return userRepository.save(user);
    }
    
    public Optional<User> loginUser(String email, String password) {
        Optional<User> user = userRepository.findByEmail(email);
        if (user.isPresent() && user.get().getPassword().equals(password)) {
            return user;
        }
        return Optional.empty();
    }
    
    public Optional<User> getUserById(Long id) {
        return userRepository.findById(id);
    }
    
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
}

// ProjectService.java
package com.crowdfunding.service;

import com.crowdfunding.entity.Project;
import com.crowdfunding.entity.ProjectStatus;
import com.crowdfunding.repository.ProjectRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.time.LocalDateTime;
import java.util.List;
import java.util.Optional;

@Service
public class ProjectService {
    
    @Autowired
    private ProjectRepository projectRepository;
    
    public Project createProject(Project project) {
        return projectRepository.save(project);
    }
    
    public Optional<Project> getProjectById(Long id) {
        return projectRepository.findById(id);
    }
    
    public List<Project> getAllProjects() {
        return projectRepository.findAll();
    }
    
    public List<Project> getActiveProjects() {
        return projectRepository.findByStatus(ProjectStatus.ACTIVE);
    }
    
    public List<Project> getProjectsByCreator(Long creatorId) {
        return projectRepository.findByCreatorId(creatorId);
    }
    
    public List<Project> getProjectsByCategory(String category) {
        return projectRepository.findByCategory(category);
    }
    
    public void updateProjectStatus() {
        List<Project> projects = projectRepository.findByStatus(ProjectStatus.ACTIVE);
        LocalDateTime now = LocalDateTime.now();
        
        for (Project project : projects) {
            if (project.getDeadline().isBefore(now)) {
                if (project.getCurrentAmount().compareTo(project.getGoalAmount()) >= 0) {
                    project.setStatus(ProjectStatus.FUNDED);
                } else {
                    project.setStatus(ProjectStatus.EXPIRED);
                }
                projectRepository.save(project);
            }
        }
    }
}

// ContributionService.java
package com.crowdfunding.service;

import com.crowdfunding.entity.Contribution;
import com.crowdfunding.entity.Project;
import com.crowdfunding.entity.User;
import com.crowdfunding.repository.ContributionRepository;
import com.crowdfunding.repository.ProjectRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.math.BigDecimal;
import java.util.List;

@Service
public class ContributionService {
    
    @Autowired
    private ContributionRepository contributionRepository;
    
    @Autowired
    private ProjectRepository projectRepository;
    
    @Transactional
    public Contribution makeContribution(Project project, User backer, BigDecimal amount) {
        Contribution contribution = new Contribution(project, backer, amount);
        contribution = contributionRepository.save(contribution);
        
        project.setCurrentAmount(project.getCurrentAmount().add(amount));
        projectRepository.save(project);
        
        return contribution;
    }
    
    public List<Contribution> getContributionsByProject(Long projectId) {
        return contributionRepository.findByProjectId(projectId);
    }
    
    public List<Contribution> getContributionsByUser(Long userId) {
        return contributionRepository.findByBackerId(userId);
    }
}

// ============================================
// CONTROLLER CLASSES
// ============================================

// UserController.java
package com.crowdfunding.controller;

import com.crowdfunding.entity.User;
import com.crowdfunding.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import java.util.Optional;

@RestController
@RequestMapping("/api/users")
@CrossOrigin(origins = "*")
public class UserController {
    
    @Autowired
    private UserService userService;
    
    @PostMapping("/register")
    public ResponseEntity<?> register(@RequestBody User user) {
        try {
            User registered = userService.registerUser(user);
            return ResponseEntity.ok(registered);
        } catch (Exception e) {
            return ResponseEntity.badRequest().body(e.getMessage());
        }
    }
    
    @PostMapping("/login")
    public ResponseEntity<?> login(@RequestBody User loginRequest) {
        Optional<User> user = userService.loginUser(loginRequest.getEmail(), loginRequest.getPassword());
        if (user.isPresent()) {
            return ResponseEntity.ok(user.get());
        }
        return ResponseEntity.badRequest().body("Invalid credentials");
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<?> getUser(@PathVariable Long id) {
        return userService.getUserById(id)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }
}

// ProjectController.java
package com.crowdfunding.controller;

import com.crowdfunding.entity.Project;
import com.crowdfunding.entity.User;
import com.crowdfunding.service.ProjectService;
import com.crowdfunding.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/api/projects")
@CrossOrigin(origins = "*")
public class ProjectController {
    
    @Autowired
    private ProjectService projectService;
    
    @Autowired
    private UserService userService;
    
    @PostMapping
    public ResponseEntity<?> createProject(@RequestBody Project project, @RequestParam Long creatorId) {
        User creator = userService.getUserById(creatorId)
                .orElseThrow(() -> new RuntimeException("Creator not found"));
        project.setCreator(creator);
        Project created = projectService.createProject(project);
        return ResponseEntity.ok(created);
    }
    
    @GetMapping
    public ResponseEntity<List<Project>> getAllProjects() {
        return ResponseEntity.ok(projectService.getAllProjects());
    }
    
    @GetMapping("/active")
    public ResponseEntity<List<Project>> getActiveProjects() {
        return ResponseEntity.ok(projectService.getActiveProjects());
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<?> getProject(@PathVariable Long id) {
        return projectService.getProjectById(id)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }
    
    @GetMapping("/creator/{creatorId}")
    public ResponseEntity<List<Project>> getProjectsByCreator(@PathVariable Long creatorId) {
        return ResponseEntity.ok(projectService.getProjectsByCreator(creatorId));
    }
}

// ContributionController.java
package com.crowdfunding.controller;

import com.crowdfunding.entity.Contribution;
import com.crowdfunding.entity.Project;
import com.crowdfunding.entity.User;
import com.crowdfunding.service.ContributionService;
import com.crowdfunding.service.ProjectService;
import com.crowdfunding.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import java.math.BigDecimal;
import java.util.List;

@RestController
@RequestMapping("/api/contributions")
@CrossOrigin(origins = "*")
public class ContributionController {
    
    @Autowired
    private ContributionService contributionService;
    
    @Autowired
    private ProjectService projectService;
    
    @Autowired
    private UserService userService;
    
    @PostMapping
    public ResponseEntity<?> contribute(@RequestParam Long projectId, 
                                       @RequestParam Long backerId, 
                                       @RequestParam BigDecimal amount) {
        try {
            Project project = projectService.getProjectById(projectId)
                    .orElseThrow(() -> new RuntimeException("Project not found"));
            User backer = userService.getUserById(backerId)
                    .orElseThrow(() -> new RuntimeException("User not found"));
            
            Contribution contribution = contributionService.makeContribution(project, backer, amount);
            return ResponseEntity.ok(contribution);
        } catch (Exception e) {
            return ResponseEntity.badRequest().body(e.getMessage());
        }
    }
    
    @GetMapping("/project/{projectId}")
    public ResponseEntity<List<Contribution>> getProjectContributions(@PathVariable Long projectId) {
        return ResponseEntity.ok(contributionService.getContributionsByProject(projectId));
    }
    
    @GetMapping("/user/{userId}")
    public ResponseEntity<List<Contribution>> getUserContributions(@PathVariable Long userId) {
        return ResponseEntity.ok(contributionService.getContributionsByUser(userId));
    }
}
```

- **Application Configuration Files (# pom.xml.txt)
```
# application.properties
# Server Configuration
spring.application.name=crowdfunding-platform
server.port=8080

# Database Configuration (H2 for development)
spring.datasource.url=jdbc:h2:mem:crowdfunding
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

# H2 Console
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

# JPA/Hibernate
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# ==================================================
# For MySQL (uncomment and configure when ready)
# ==================================================
# spring.datasource.url=jdbc:mysql://localhost:3306/crowdfunding_db
# spring.datasource.username=root
# spring.datasource.password=yourpassword
# spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
# spring.jpa.database-platform=org.hibernate.dialect.MySQLDialect
# spring.jpa.hibernate.ddl-auto=update

# ==================================================
# pom.xml
# ==================================================
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.0</version>
        <relativePath/>
    </parent>
    
    <groupId>com.crowdfunding</groupId>
    <artifactId>crowdfunding-platform</artifactId>
    <version>1.0.0</version>
    <name>Crowdfunding Platform</name>
    <description>Online Crowdfunding Application</description>
    
    <properties>
        <java.version>17</java.version>
    </properties>
    
    <dependencies>
        <!-- Spring Boot Web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        
        <!-- Spring Boot Data JPA -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        
        <!-- H2 Database (for development) -->
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>
        
        <!-- MySQL Driver (uncomment when using MySQL) -->
        <!--
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <scope>runtime</scope>
        </dependency>
        -->
        
        <!-- Validation -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>
        
        <!-- Spring Boot DevTools (optional, for hot reload) -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        
        <!-- Lombok (optional, reduces boilerplate) -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        
        <!-- Spring Boot Test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
    
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```
- **Simple Frontend Interface (frontend.html.txt)
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Crowdfunding Platform</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
        }
        
        header {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            margin-bottom: 30px;
        }
        
        h1 {
            color: #667eea;
            text-align: center;
        }
        
        .tabs {
            display: flex;
            gap: 10px;
            margin-top: 20px;
            justify-content: center;
        }
        
        .tab-btn {
            padding: 10px 20px;
            border: none;
            background: #f0f0f0;
            cursor: pointer;
            border-radius: 5px;
            font-size: 14px;
            transition: all 0.3s;
        }
        
        .tab-btn.active {
            background: #667eea;
            color: white;
        }
        
        .content-section {
            display: none;
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        
        .content-section.active {
            display: block;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #333;
        }
        
        input, textarea, select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 14px;
        }
        
        textarea {
            resize: vertical;
            min-height: 100px;
        }
        
        button {
            background: #667eea;
            color: white;
            padding: 12px 30px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            transition: background 0.3s;
        }
        
        button:hover {
            background: #5568d3;
        }
        
        .projects-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }
        
        .project-card {
            background: #f9f9f9;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        
        .project-card h3 {
            color: #667eea;
            margin-bottom: 10px;
        }
        
        .progress-bar {
            background: #e0e0e0;
            height: 20px;
            border-radius: 10px;
            overflow: hidden;
            margin: 10px 0;
        }
        
        .progress-fill {
            background: linear-gradient(90deg, #667eea, #764ba2);
            height: 100%;
            transition: width 0.3s;
        }
        
        .status-badge {
            display: inline-block;
            padding: 5px 10px;
            border-radius: 15px;
            font-size: 12px;
            font-weight: bold;
            margin-top: 10px;
        }
        
        .status-active {
            background: #4caf50;
            color: white;
        }
        
        .status-funded {
            background: #2196f3;
            color: white;
        }
        
        .contribute-btn {
            margin-top: 10px;
            width: 100%;
            padding: 8px;
            font-size: 14px;
        }
        
        .message {
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 20px;
            display: none;
        }
        
        .message.success {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }
        
        .message.error {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }
        
        .user-info {
            background: #f0f7ff;
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>🚀 Crowdfunding Platform</h1>
            <div class="tabs">
                <button class="tab-btn active" onclick="showTab('register')">Register</button>
                <button class="tab-btn" onclick="showTab('login')">Login</button>
                <button class="tab-btn" onclick="showTab('projects')">Browse Projects</button>
                <button class="tab-btn" onclick="showTab('create')">Create Project</button>
            </div>
        </header>

        <div id="message" class="message"></div>
        <div id="userInfo" class="user-info" style="display: none;"></div>

        <!-- Register Section -->
        <div id="register" class="content-section active">
            <h2>Register New Account</h2>
            <form onsubmit="register(event)">
                <div class="form-group">
                    <label>Full Name</label>
                    <input type="text" id="regName" required>
                </div>
                <div class="form-group">
                    <label>Email</label>
                    <input type="email" id="regEmail" required>
                </div>
                <div class="form-group">
                    <label>Password</label>
                    <input type="password" id="regPassword" required>
                </div>
                <div class="form-group">
                    <label>Phone Number</label>
                    <input type="tel" id="regPhone">
                </div>
                <button type="submit">Register</button>
            </form>
        </div>

        <!-- Login Section -->
        <div id="login" class="content-section">
            <h2>Login</h2>
            <form onsubmit="login(event)">
                <div class="form-group">
                    <label>Email</label>
                    <input type="email" id="loginEmail" required>
                </div>
                <div class="form-group">
                    <label>Password</label>
                    <input type="password" id="loginPassword" required>
                </div>
                <button type="submit">Login</button>
            </form>
        </div>

        <!-- Browse Projects Section -->
        <div id="projects" class="content-section">
            <h2>Active Projects</h2>
            <button onclick="loadProjects()">Refresh Projects</button>
            <div id="projectsList" class="projects-grid"></div>
        </div>

        <!-- Create Project Section -->
        <div id="create" class="content-section">
            <h2>Create New Project</h2>
            <form onsubmit="createProject(event)">
                <div class="form-group">
                    <label>Project Title</label>
                    <input type="text" id="projectTitle" required>
                </div>
                <div class="form-group">
                    <label>Description</label>
                    <textarea id="projectDesc" required></textarea>
                </div>
                <div class="form-group">
                    <label>Goal Amount ($)</label>
                    <input type="number" id="projectGoal" min="1" required>
                </div>
                <div class="form-group">
                    <label>Category</label>
                    <select id="projectCategory">
                        <option>Technology</option>
                        <option>Art</option>
                        <option>Music</option>
                        <option>Film</option>
                        <option>Games</option>
                        <option>Education</option>
                        <option>Social</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>Deadline</label>
                    <input type="datetime-local" id="projectDeadline" required>
                </div>
                <button type="submit">Create Project</button>
            </form>
        </div>
    </div>

    <script>
        const API_URL = 'http://localhost:8080/api';
        let currentUser = null;

        function showTab(tab) {
            document.querySelectorAll('.content-section').forEach(s => s.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
            document.getElementById(tab).classList.add('active');
            event.target.classList.add('active');
            
            if (tab === 'projects') {
                loadProjects();
            }
        }

        function showMessage(text, type) {
            const msg = document.getElementById('message');
            msg.textContent = text;
            msg.className = `message ${type}`;
            msg.style.display = 'block';
            setTimeout(() => msg.style.display = 'none', 5000);
        }

        function updateUserInfo() {
            const userInfo = document.getElementById('userInfo');
            if (currentUser) {
                userInfo.innerHTML = `<strong>Logged in as:</strong> ${currentUser.fullName} (${currentUser.email})`;
                userInfo.style.display = 'block';
            } else {
                userInfo.style.display = 'none';
            }
        }

        async function register(e) {
            e.preventDefault();
            const data = {
                fullName: document.getElementById('regName').value,
                email: document.getElementById('regEmail').value,
                password: document.getElementById('regPassword').value,
                phoneNumber: document.getElementById('regPhone').value
            };

            try {
                const response = await fetch(`${API_URL}/users/register`, {
                    method: 'POST',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify(data)
                });

                if (response.ok) {
                    showMessage('Registration successful! Please login.', 'success');
                    showTab('login');
                } else {
                    const error = await response.text();
                    showMessage(error, 'error');
                }
            } catch (error) {
                showMessage('Error: ' + error.message, 'error');
            }
        }

        async function login(e) {
            e.preventDefault();
            const data = {
                email: document.getElementById('loginEmail').value,
                password: document.getElementById('loginPassword').value
            };

            try {
                const response = await fetch(`${API_URL}/users/login`, {
                    method: 'POST',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify(data)
                });

                if (response.ok) {
                    currentUser = await response.json();
                    showMessage('Login successful!', 'success');
                    updateUserInfo();
                    showTab('projects');
                } else {
                    showMessage('Invalid credentials', 'error');
                }
            } catch (error) {
                showMessage('Error: ' + error.message, 'error');
            }
        }

        async function loadProjects() {
            try {
                const response = await fetch(`${API_URL}/projects/active`);
                const projects = await response.json();
                
                const container = document.getElementById('projectsList');
                container.innerHTML = '';

                projects.forEach(project => {
                    const progress = (project.currentAmount / project.goalAmount * 100).toFixed(1);
                    const card = document.createElement('div');
                    card.className = 'project-card';
                    card.innerHTML = `
                        <h3>${project.title}</h3>
                        <p>${project.description.substring(0, 100)}...</p>
                        <p><strong>Goal:</strong> $${project.goalAmount}</p>
                        <p><strong>Raised:</strong> $${project.currentAmount}</p>
                        <div class="progress-bar">
                            <div class="progress-fill" style="width: ${Math.min(progress, 100)}%"></div>
                        </div>
                        <p>${progress}% funded</p>
                        <span class="status-badge status-${project.status.toLowerCase()}">${project.status}</span>
                        <br>
                        <button class="contribute-btn" onclick="contribute(${project.id})">Contribute</button>
                    `;
                    container.appendChild(card);
                });
            } catch (error) {
                showMessage('Error loading projects: ' + error.message, 'error');
            }
        }

        async function createProject(e) {
            e.preventDefault();
            
            if (!currentUser) {
                showMessage('Please login first', 'error');
                return;
            }

            const data = {
                title: document.getElementById('projectTitle').value,
                description: document.getElementById('projectDesc').value,
                goalAmount: document.getElementById('projectGoal').value,
                category: document.getElementById('projectCategory').value,
                deadline: document.getElementById('projectDeadline').value
            };

            try {
                const response = await fetch(`${API_URL}/projects?creatorId=${currentUser.id}`, {
                    method: 'POST',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify(data)
                });

                if (response.ok) {
                    showMessage('Project created successfully!', 'success');
                    e.target.reset();
                    showTab('projects');
                } else {
                    showMessage('Error creating project', 'error');
                }
            } catch (error) {
                showMessage('Error: ' + error.message, 'error');
            }
        }

        async function contribute(projectId) {
            if (!currentUser) {
                showMessage('Please login first', 'error');
                return;
            }

            const amount = prompt('Enter contribution amount ($):');
            if (!amount || amount <= 0) return;

            try {
                const response = await fetch(`${API_URL}/contributions?projectId=${projectId}&backerId=${currentUser.id}&amount=${amount}`, {
                    method: 'POST'
                });

                if (response.ok) {
                    showMessage('Contribution successful! Thank you!', 'success');
                    loadProjects();
                } else {
                    showMessage('Error making contribution', 'error');
                }
            } catch (error) {
                showMessage('Error: ' + error.message, 'error');
            }
        }
    </script>
</body>
</html>
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

