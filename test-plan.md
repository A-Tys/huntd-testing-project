# Test Plan — Huntd QA Project

---

## 1. Introduction

### 1.1 Purpose
The purpose of this test plan is to define the testing scope, objectives, strategy, test types, test techniques, deliverables, and testing approach for the Huntd web and mobile applications.

This document describes the planned testing activities required to verify that the applications function according to specified requirements and to identify defects affecting functionality, usability, and security.

### 1.2 Project Overview
Huntd is a job search and application tracking platform designed to help users search for job opportunities, manage applications, and communicate with recruiters. The platform includes a web application and a mobile application.

The web application provides functionality for candidate search, recruiter and candidate registration, chats, profile management, and Web3 companies and jobs exploration.

The mobile application is an MVP version focused on communication and profile-related functionality, including chat features, IAM functionality, and profile settings.

---

## 2. Context of Testing

### 2.1 Test Scope

**In scope:**
- Main Page (For Companies, For Engineers)
- Candidates List (Filters, Candidate Cards, Candidate Profile)
- Sign Up (As Recruiter, As Candidate)
- Sign In
- Chats
- Profile
- Footer
- Web3 Companies & Jobs
- Mobile App (IAM, Chat Features, Profile Settings)

**Out of scope:**
- Admin functionality
- Question/feedback form
- Regulatory and contractual acceptance testing
- Performance testing
- Regression testing
- Mobile feature/improvement requests

### 2.2 Test Objectives
The main objectives of testing are:
- Verify that the Huntd web and mobile applications function according to specified requirements
- Identify, document, and report defects discovered during testing
- Verify the functionality of core features including:
  - Authentication
  - Candidate search and filtering
  - Candidate profiles
  - Chats
  - Profile management
  - Web3 companies and jobs functionality
- Validate usability and user experience across web and mobile platforms
- Ensure sufficient test coverage for all in-scope modules
- Perform limited compatibility testing on supported browsers and mobile devices
- Perform basic security verification including authentication, input validation, injection attempt checks

### 2.3 Test Basis
- Module requirements document
- Huntd web application (live app)
- Huntd mobile application (MVP)

### 2.4 Assumptions
- The Huntd web and mobile applications are accessible and available for testing during the planned testing period
- The module requirements document is considered the primary and approved source of requirements for testing
- All in-scope features are implemented and available for testing

### 2.5 Constraints
- Single tester — no peer review available
- No access to source code — security testing is performed using a grey-box approach
- No admin access — admin-related scenarios are out of scope
- The mobile application has no written specifications — testing is based on implemented functionality only
- Candidate profile activation requires admin approval within 24–48 hours and cannot be managed independently during testing
- Web testing is performed using Microsoft Edge browser (64-bit)
- All required test data is created manually by the tester
- Testing is performed in the live production environment since no dedicated test environment is available

---

## 3. Test Strategy

### 3.1 Test Types
- **Functional Testing** — verifying that all application features function according to specified requirements
- **Non-functional Testing:**
  - **Usability Testing** — validating the user experience and interface of the Huntd web and mobile applications
  - **Security Testing** — basic security verification including authentication, input validation, injection attempts, and cookie security using a grey-box approach via DevTools
  - **Compatibility Testing** — limited verification of application behavior across supported browsers and mobile devices
- **Black-box Testing** — testing based on specifications and expected system behavior without access to internal implementation details
- **Grey-box Testing** — testing performed with partial knowledge of the internal system structure using browser DevTools

### 3.2 Test Techniques

**Black-box Test Techniques:**
- Equivalence Partitioning
- Boundary Value Analysis
- Decision Table Testing
- State Transition Testing
- Use Case Testing

### 3.3 Levels of Testing
- System testing will be performed for the Huntd web and mobile applications
- End-to-end testing will be performed for critical user flows such as registration, authentication, candidate search, profile management, and chat functionality

---

## 4. Test Deliverables
The following testing deliverables will be prepared during the testing process:
- Test Plan
- Test Cases for web application features
- Test Cases for mobile application features
- Requirements Traceability Matrix (RTM)
- Permission Testing Table
- Jira stories and related tasks
- Bug Reports
- Test Runs in TestRail
- Test Summary Report
- Test execution statistics for web and mobile applications

---

## 5. Entry and Exit Criteria

### 5.1 Entry Criteria
Testing activities may begin when the following conditions are met:
- Requirements and feature descriptions are available
- The application is accessible for testing
- In-scope features are implemented and available for testing
- Test documentation can be created and maintained during the testing process

### 5.2 Exit Criteria
Testing activities may be considered complete when the following conditions are met:
- All planned test cases are executed
- Identified defects are documented and reported
- Test execution results are documented
- Test deliverables are completed
- Test Summary Report is prepared

---

## 6. Risks
- Limited testing time may reduce overall test coverage
- Missing mobile application specifications may lead to incomplete validation
- Production environment limitations may affect testing stability and data consistency
- Limited browser and device availability may reduce compatibility coverage
- Some identified defects may remain unresolved within the academy project scope
