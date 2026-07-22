
---

# Day 1 – Backend, Database & Core Logic (10–12 hours)

## Phase 1 (1 hour) – Requirement Analysis & Product Design

### Deliverables

* ✅ Problem understanding
* ✅ Assumptions
* ✅ User roles
* ✅ User flow
* ✅ Company hierarchy
* ✅ Feature list

Output:

```
docs/

ProductDesign.md
```

---

## Phase 2 (1 hour) – Database Design

Design

```
Company

Employee

Role

ReviewCycle

Parameter

Feedback

FeedbackDetail
```

Draw ER Diagram.

Output

```
ERD.png

schema.prisma
```

---

## Phase 3 (1 hour) – Project Setup

Backend

```
NestJS

Prisma

PostgreSQL

JWT

Swagger

Docker
```

Folder

```
src

auth

employee

feedback

review-cycle

report

common
```

---

## Phase 4 (2 hours) – Authentication

Implement

```
Login

JWT

Role Middleware

Company Middleware
```

Test

```
Founder

HR

Manager

Employee
```

---

## Phase 5 (2 hours) – Employee Module

Implement

```
Create Employee

Update Employee

Get Employee

Manager Team

Employee Profile
```

---

## Phase 6 (2 hours) – Feedback Module

Implement

```
Create Review

Save Draft

Submit Review

Feedback Detail
```

Database

```
Feedback

↓

FeedbackDetail
```

---

## Phase 7 (1 hour) – Review Cycle

```
Create Cycle

Open

Close
```

---

## Phase 8 (1 hour) – Seed Data

Insert

```
Ashoka

Bright Path

Managers

Employees

Founder

HR

Parameters

Review Cycle

Sample Feedback
```

Verify

```
Priya

↓

6 Employees

Founder

↓

8 Employees
```

---

# End of Day 1

You should have:

* ✅ Database
* ✅ APIs
* ✅ Authentication
* ✅ Seed Data
* ✅ Swagger
* ✅ Working Backend

---

# Day 2 – Frontend + Reports + Polish (8–10 hours)

---

## Phase 9 (1 hour) – React Setup

```
Next.js

Tailwind

Axios

React Query

Shadcn
```

---

## Phase 10 (2 hours) – Login & Dashboard

Role-based dashboard

```
Employee

Manager

HR

Founder
```

---

## Phase 11 (2 hours) – Manager App

Pages

```
My Team

Pending Reviews

Feedback Form
```

Manager clicks

```
Rahul

↓

Rate

↓

Submit
```

---

## Phase 12 (1 hour) – Employee App

Pages

```
Latest Feedback

History

Performance Trend
```

---

## Phase 13 (1 hour) – HR Dashboard

Show

```
Manager

Completed

Pending

Progress
```

Example

| Manager | Completed | Pending |
| ------- | --------: | ------: |
| Priya   |         4 |       2 |
| Founder |         8 |       0 |

---

## Phase 14 (1 hour) – Reports

Create

```
Employee Trend

Department Average

Manager Progress
```

---

## Phase 15 (1 hour) – Testing

Verify

```
Ashoka hierarchy

Bright hierarchy

History

Pending

Submission

Login

Permissions
```

---

## Phase 16 (1 hour) – Documentation

README

```
Project

Architecture

Database

API

Assumptions

How to Run

Screenshots
```

---

# Complete Folder Structure

```
performance-evaluation-tool/

backend/
│
├── src/
│   ├── auth/
│   ├── company/
│   ├── employee/
│   ├── feedback/
│   ├── parameter/
│   ├── review-cycle/
│   ├── report/
│   └── common/
│
├── prisma/
│   ├── schema.prisma
│   └── seed.ts
│
└── docs/

frontend/
│
├── app/
├── components/
├── services/
├── hooks/
└── pages/
```

---

# Priority Order

| Priority | Task                  | Time |
| -------- | --------------------- | ---- |
| 1        | Requirement analysis  | 1 hr |
| 2        | Product & DB design   | 1 hr |
| 3        | NestJS + Prisma setup | 1 hr |
| 4        | Authentication        | 2 hr |
| 5        | Employee module       | 2 hr |
| 6        | Feedback module       | 2 hr |
| 7        | Review cycle          | 1 hr |
| 8        | Seed data             | 1 hr |
| 9        | Frontend setup        | 1 hr |
| 10       | Manager UI            | 2 hr |
| 11       | Employee UI           | 1 hr |
| 12       | HR Dashboard          | 1 hr |
| 13       | Reports               | 1 hr |
| 14       | Testing               | 1 hr |
| 15       | README & cleanup      | 1 hr |

---

# Deliverables Checklist

### Documentation

* ✅ Product design
* ✅ Assumptions
* ✅ ER diagram
* ✅ API documentation (Swagger)

### Backend

* ✅ JWT authentication
* ✅ Multi-tenant support
* ✅ Employee management
* ✅ Review cycle management
* ✅ Feedback APIs
* ✅ HR reporting APIs
* ✅ Seed data

### Frontend

* ✅ Login
* ✅ Employee dashboard
* ✅ Manager dashboard
* ✅ HR dashboard
* ✅ Feedback form
* ✅ History page

### Submission

* ✅ GitHub repository
* ✅ README with setup instructions
* ✅ Screenshots (optional but recommended)

---

## Feedback

Thank you for reviewing my submission.

I welcome any feedback on the solution, including the product thinking, system design, database modeling, API design, code structure, testing approach, or any improvements you believe could make the solution more scalable or maintainable.

Your suggestions and advice would be greatly appreciated and will help me continue improving as a software engineer.

Thank you for your time and consideration.
