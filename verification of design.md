
---

# Requirement 3

> **Seed data for at least two companies, matching the setups described above.**

The interviewer is checking whether your **database model actually supports different organization structures**, not just whether you inserted random data.

---

# Company 1 — Ashoka Textiles

### Given in Problem

```
COO
   │
Rohan
   │
Priya
   │
Rahul
Neha
Ankit
Pooja
Ravi
Sanjay
```

Also

* Priya reviews 6 employees.
* Rohan reviews Priya.
* Rohan reports to COO.

---

## Company Table

| id | name            |
| -- | --------------- |
| 1  | Ashoka Textiles |

---

## Employee Table

| id | Name   | Role          | manager_id | company_id |
| -- | ------ | ------------- | ---------- | ---------- |
| 1  | COO    | Founder/Admin | NULL       | 1          |
| 2  | Rohan  | Manager       | 1          | 1          |
| 3  | Priya  | Manager       | 2          | 1          |
| 4  | Rahul  | Employee      | 3          | 1          |
| 5  | Neha   | Employee      | 3          | 1          |
| 6  | Ankit  | Employee      | 3          | 1          |
| 7  | Pooja  | Employee      | 3          | 1          |
| 8  | Ravi   | Employee      | 3          | 1          |
| 9  | Sanjay | Employee      | 3          | 1          |
| 10 | Kavita | HR            | 1          | 1          |

---

### Verify

Can Priya review six employees?

```
SELECT *
FROM Employee
WHERE manager_id=3;
```

Returns

```
Rahul

Neha

Ankit

Pooja

Ravi

Sanjay
```

✔ Works.

---

Can Rohan review Priya?

```
manager_id(Priya)=2
```

✔ Works.

---

Can HR check pending managers?

HR

```
Kavita
```

gets

```
Manager

Priya

Pending

Rohan

Completed
```

✔ Works.

---

# Company 2 — Bright Path Consulting

Problem says

Founder directly reviews 8 employees.

Hierarchy

```
Founder

├── Aman
├── Rita
├── Mohit
├── Sneha
├── Arjun
├── Divya
├── Karan
└── Nisha
```

---

## Company

| id | Name                   |
| -- | ---------------------- |
| 2  | Bright Path Consulting |

---

## Employee

| id | Name    | Role     | manager_id | company_id |
| -- | ------- | -------- | ---------- | ---------- |
| 20 | Founder | Founder  | NULL       | 2          |
| 21 | Aman    | Employee | 20         | 2          |
| 22 | Rita    | Employee | 20         | 2          |
| 23 | Mohit   | Employee | 20         | 2          |
| 24 | Sneha   | Employee | 20         | 2          |
| 25 | Arjun   | Employee | 20         | 2          |
| 26 | Divya   | Employee | 20         | 2          |
| 27 | Karan   | Employee | 20         | 2          |
| 28 | Nisha   | Employee | 20         | 2          |
| 29 | Anjali  | HR       | 20         | 2          |

---

Verify

```
SELECT *
FROM Employee
WHERE manager_id=20;
```

Returns

```
8 Employees
```

Exactly what assignment requires.

✔ Works.

---

# Review Cycle

```
ReviewCycle

id

company_id

month

year

status
```

Example

| id | company     | month | year |
| -- | ----------- | ----- | ---- |
| 1  | Ashoka      | July  | 2026 |
| 2  | Bright Path | July  | 2026 |

Each company has its own review cycle.

---

# Feedback Example

Priya reviews Rahul

Feedback

| id | employee | reviewer | cycle |
| -- | -------- | -------- | ----- |
| 1  | Rahul    | Priya    | July  |

Feedback Detail

| parameter     | score | comment      |
| ------------- | ----- | ------------ |
| Ownership     | 5     | Excellent    |
| Communication | 4     | Good updates |
| Quality       | 5     | Very good    |
| Teamwork      | 4     | Helpful      |
| Learning      | 5     | Fast learner |

---

Founder reviews Aman

Feedback

| id | employee | reviewer | cycle |
| -- | -------- | -------- | ----- |
| 2  | Aman     | Founder  | July  |

Works exactly the same.

---

# Verify Every Assignment Statement

### Statement 1

> Priya gives feedback to six team members.

```
manager_id = Priya
```

✔ Supported.

---

### Statement 2

> Priya receives feedback from Rohan.

```
Priya.manager_id = Rohan
```

Rohan becomes reviewer.

✔ Supported.

---

### Statement 3

> Founder reviews eight employees directly.

```
manager_id = Founder
```

✔ Supported.

---

### Statement 4

> HR wants pending submissions.

Suppose

```
Feedback

Rahul

Submitted

Neha

Submitted

Ankit

Pending

Pooja

Pending

Ravi

Submitted

Sanjay

Submitted
```

Progress

```
Priya

4 / 6
```

HR dashboard

```
Manager

Completed

Pending

Priya

4 completed

2 pending

Founder

8 completed

0 pending
```

✔ Supported.

---

### Statement 5

> Employee wants history.

Rahul

```
July

Ownership

5
```

```
August

Ownership

4
```

```
September

Ownership

5
```

Since feedback is stored per **review cycle**, history is preserved.

✔ Supported.

---

# One Improvement I'd Recommend

Your current hierarchy uses only `manager_id`, which is good enough for this assignment.

To make it production-ready, add a **ReportingRelationship** table instead of relying solely on `manager_id`.

```
ReportingRelationship

id

company_id

employee_id

manager_id

start_date

end_date

is_active
```

Why?

* Tracks manager changes over time.
* Preserves historical reporting lines.
* Supports reorganizations without losing history.
* Common in enterprise HR systems like Workday, SAP SuccessFactors, and Oracle HCM.

---

# Final Verdict

| Requirement                   | Status |
| ----------------------------- | ------ |
| Multi-company support         | ✅      |
| Different company hierarchies | ✅      |
| Founder reviews directly      | ✅      |
| Multi-level manager hierarchy | ✅      |
| HR monitoring                 | ✅      |
| Monthly review cycles         | ✅      |
| Employee history              | ✅      |
| Fixed performance parameters  | ✅      |
| Normalized database           | ✅      |
| Seed data matches assignment  | ✅      |

---

## Feedback

Thank you for reviewing my submission.

I welcome any feedback on the solution, including the product thinking, system design, database modeling, API design, code structure, testing approach, or any improvements you believe could make the solution more scalable or maintainable.

Your suggestions and advice would be greatly appreciated and will help me continue improving as a software engineer.

Thank you for your time and consideration.
