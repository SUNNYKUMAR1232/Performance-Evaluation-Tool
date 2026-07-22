# Performance Evaluation Tool – Product Design 

---

# 1. Problem Understanding

## Business Problem

Companies conduct employee performance reviews every month. Managers evaluate their direct reports based on predefined performance parameters and provide scores with comments.

Many organizations still perform this process manually using Excel sheets, emails, or Google Forms, resulting in:

* No centralized record of feedback
* Difficult tracking of pending reviews
* No historical performance trends
* HR spends significant time following up
* Managers have no structured workflow
* Employees cannot easily view past feedback

The objective is to build a centralized Performance Evaluation System that simplifies the monthly review process.

---

# 2. Product Goal

The system should allow companies to

* Manage employees
* Maintain reporting hierarchy
* Conduct monthly performance reviews
* Track pending reviews
* Store historical feedback
* Allow employees to view previous evaluations
* Allow HR to monitor completion status

The application should support multiple companies using a single platform (Multi-Tenant SaaS).

---

# 3. Users

## Founder

Highest authority.

Can

* View company reports
* Review direct reports
* View completion status
* Manage organization

---

## HR

Responsible for administration.

Can

* Add employees
* Manage employee hierarchy
* Create review cycles
* Track pending submissions
* Generate reports

Cannot modify submitted feedback.

---

## Manager

Responsible for reviewing direct reports.

Can

* View team
* Submit monthly feedback
* Edit feedback until review cycle closes
* View previous feedback given

---

## Employee

Can

* View own feedback
* View performance history
* Track improvement
* Cannot edit submitted reviews

---

# 4. Example Company Hierarchy(assumption)

```
Founder
│
├── HR 1
├── HR 2
│
├── Engineering Manager
│      │
│      ├── Employee A
│      ├── Employee B
│      ├── Employee C
│
├── Sales Manager
│      │
│      ├── Employee D
│      ├── Employee E
│
└── Finance Manager
       │
       ├── Employee F
       └── Employee G
```

Every employee has exactly one manager.

Manager is also an employee.

Founder has no manager.

---

# 5. Functional Requirements

## Authentication

* Login
* Logout
* JWT Authentication

---

## Employee Management

HR can

* Add employee
* Update employee
* Disable employee
* Assign manager

---

## Monthly Review

Manager can

* Open employee profile
* Give scores
* Write comments
* Submit review

---

## HR Dashboard

HR can

* See completed reviews
* See pending reviews
* Track manager progress
* Export report

---

## Employee Dashboard

Employee can

* View latest review
* View review history
* View parameter trends

---

# 6. Performance Parameters

Fixed parameters

```
Ownership

Communication

Quality of Work

Teamwork

Learning & Growth
```

These remain same across all companies.

---

# 7. Product Workflow

## Step 1

HR creates review cycle

```
July 2026
```

↓

---

## Step 2

Managers receive pending reviews

```
Review Rahul

Review Neha

Review Amit
```

↓

---

## Step 3

Manager submits review

↓

---

## Step 4

Employee receives review

↓

---

## Step 5

HR monitors completion

```
Completed

Pending

Overdue
```

---

# 8. System Architecture

```
                Frontend

        Employee App
        HR App
        Founder Dashboard

                │

            REST API

                │

        Authentication

                │

      Business Services

 Employee Service

 Review Service

 Organization Service

 Report Service

                │

            PostgreSQL
```

---

# 9. Database Design

---

## Company

```
Company

id
name
industry
created_at
```

---

## Employee

```
Employee

id

company_id

manager_id

name

email

designation

department

role

status

joining_date
```

manager_id references Employee.id

Example

```
Founder

manager_id = NULL

Engineering Manager

manager_id = Founder

Employee A

manager_id = Engineering Manager
```

---

## Role

```
Role

Founder

HR

Manager

Employee
```

A Manager is simply an employee with direct reports.

---

## Performance Parameter

```
Parameter

id

name
```

```
Ownership

Communication

Quality

Teamwork

Learning
```

---

## Review Cycle

```
ReviewCycle

id

company_id

month

year

status

created_at
```

Status

```
Draft

Open

Closed
```

---

## Feedback

Represents one review.

```
Feedback

id

review_cycle_id

employee_id

reviewer_id

submitted_at

status
```

Status

```
Draft

Submitted
```

---

## Feedback Detail

Stores parameter-level evaluation.

```
FeedbackDetail

id

feedback_id

parameter_id

score

comment
```

Example

```
Ownership

5

Excellent ownership
```

---

# Database Relationship

```
Company

│

Employee

│

Review Cycle

│

Feedback

│

Feedback Detail

│

Parameter
```

---

# Why FeedbackDetail?

Instead of

```
ownership_score

communication_score

quality_score
```

we normalize.

Advantages

* Cleaner
* Extensible
* Easier reporting
* Better analytics

---

# 10. API Design

---

## Authentication

```
POST /auth/login
```

```
POST /auth/logout
```

---

## Employee

```
GET /employees

GET /employees/{id}

POST /employees

PUT /employees/{id}

DELETE /employees/{id}
```

---

## Manager

```
GET /manager/team
```

Returns

```
Employees under manager
```

---

```
GET /manager/pending-reviews
```

Returns

```
Pending employees
```

---

```
POST /feedback
```

Submit review.

---

```
PUT /feedback/{id}
```

Update draft review.

---

## Employee

```
GET /my-feedback
```

Latest review.

---

```
GET /my-feedback/history
```

Historical reviews.

---

## HR

```
GET /hr/review-status
```

Returns

```
Manager

Completed

Pending

Total
```

---

```
GET /hr/report
```

Company report.

---

## Review Cycle

```
POST /review-cycle

GET /review-cycle

PUT /review-cycle/{id}
```

---

# 11. UI Modules

## Employee App

Dashboard

```
Latest Review

Performance Trend

Review History
```

---

## Manager App

```
My Team

Pending Reviews

Completed Reviews
```

---

Review Form

```
Ownership

★★★★☆

Reason

Communication

★★★☆☆

Reason
```

---

## HR Dashboard

```
Employees

Managers

Review Progress

Reports
```

---

## Founder Dashboard

```
Company Performance

Department Performance

Review Completion

Analytics
```

---

# 12. Reporting

Manager Completion

```
Priya

5/6

83%
```

---

Department Average

```
Engineering

4.4

Sales

3.8

Finance

4.1
```

---

Employee Trend

```
Ownership

Jan

4

Feb

5

Mar

5

Apr

4
```

---

# 13. Assumptions

* Every employee belongs to one company.
* Every employee has one reporting manager except Founder.
* One employee receives one review per review cycle.
* Only direct manager can review an employee.
* Review parameters are fixed.
* HR manages review cycles.
* Founder can review direct reports.
* Submitted reviews cannot be edited after cycle closes.

---

# 14. Edge Cases & Possible Solutions

| Edge Case                                  | Problem                                                   | Possible Solution                                                                                                   |
| ------------------------------------------ | --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Employee joins mid-cycle                   | Should they receive review?                               | Add a rule: employees joining after a cutoff date participate from the next review cycle.                           |
| Employee resigns                           | Historical reviews should remain available.               | Mark employee as **Inactive** instead of deleting records (soft delete).                                            |
| Manager changes during a review cycle      | Which manager submits the review?                         | Lock reviewer when the review cycle opens, or transfer pending reviews to the new manager based on business policy. |
| Manager has no direct reports              | Empty review list.                                        | Return an empty state with "No reviews assigned".                                                                   |
| Duplicate review submission                | Multiple reviews for the same employee in the same month. | Add a unique constraint on `(review_cycle_id, employee_id)` and validate before saving.                             |
| Review submitted after deadline            | Late submissions affect reporting.                        | Automatically mark as **Late** or prevent submission after cycle closure.                                           |
| Employee transferred to another department | Past reports become inconsistent.                         | Store department/designation snapshot in the Feedback record if historical reporting is required.                   |
| Founder has no manager                     | Hierarchy traversal fails.                                | Allow `manager_id = NULL` for the Founder.                                                                          |
| HR accidentally deletes an employee        | Historical feedback is lost.                              | Use soft delete (`status = Inactive`) instead of physical deletion.                                                 |
| Review cycle reopened                      | Employees may have already viewed feedback.               | Track review version or maintain an audit log for changes.                                                          |
| Parameter list changes in the future       | Old reviews become incompatible.                          | Version parameters or associate parameters with review cycles to preserve historical consistency.                   |
| Multi-company data leakage                 | Users see another company's data.                         | Enforce `company_id` filtering on every query and validate JWT tenant information.                                  |
| Unauthorized access                        | Employees access others' reviews.                         | Implement Role-Based Access Control (RBAC) and ownership checks on all APIs.                                        |
| Concurrent submissions                     | Two browser tabs overwrite each other.                    | Use optimistic locking (`updated_at`/version field) or database transactions.                                       |

---

# 15. Future Enhancements

* Self-evaluation before manager review
* Peer (360°) feedback
* Goal/KPI tracking (OKRs)
* Review approval workflow
* Notifications and reminders
* Performance Improvement Plans (PIPs)
* AI-generated feedback summaries
* Analytics dashboards with trends and benchmarking
* Export reports (PDF/Excel)
* Integration with HRMS/Payroll systems

---

## Feedback

Thank you for reviewing my submission.

I welcome any feedback on the solution, including the product thinking, system design, database modeling, API design, code structure, testing approach, or any improvements you believe could make the solution more scalable or maintainable.

Your suggestions and advice would be greatly appreciated and will help me continue improving as a software engineer.

Thank you for your time and consideration.
