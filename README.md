# 🔗 SQL Joins Investigation

## 📌 Project Description

This activity is part of the **Google Cybersecurity Professional Certificate**. As a security analyst trainee, I am responsible for retrieving the information my team needs from the company database to support a security investigation. Using **MariaDB** (provided by Google), I combined data from multiple tables using SQL joins.

I completed the following:

- Used an **INNER JOIN** to identify which employees are using which machines.
- Used **LEFT** and **RIGHT JOIN**s to find machines that do not belong to any user, and users who do not have a machine assigned.
- Used an **INNER JOIN** to list all login attempts made by all employees.

**Tools used:** MariaDB, SQL

---

## 1️⃣ Retrieve all records from the machines table

```sql
SELECT *
FROM machines;
```

<img width="885" height="675" alt="01-machines-table" src="https://github.com/user-attachments/assets/008a98fc-b19d-4040-a9a3-e39fe7cd3cf9" />
*Screenshot provided by Google Cybersecurity Certificate lab environment*

**Key Observations:**
- The `machines` table contains hardware information such as `device_id` and `operating_system`.
- It does **not** show employee names, so this query alone is not enough for the investigation.
- `machines` only knows about the device, while the `employees` table only knows about the person. A join is needed to connect them.

---

## 2️⃣ Identify which employees are using which machines (INNER JOIN)

I performed an inner join between the `machines` and `employees` tables on the `device_id` column.

```sql
SELECT *
FROM machines
INNER JOIN employees ON machines.device_id = employees.device_id;
```

<img width="882" height="673" alt="02-inner-join-machines-employees" src="https://github.com/user-attachments/assets/4c161268-b4af-4d12-8a38-795d78f3a1bb" />
*Screenshot provided by Google Cybersecurity Certificate lab environment*

**Key Observations:**
- An `INNER JOIN` returns only the rows where `device_id` matches in **both** tables.
- Each row now shows the machine details together with the employee who uses it.

> 💡 **Note:** The lab uses `SELECT *` for simplicity. In real-world work, I would specify the columns for clarity and readability, for example:
> ```sql
> SELECT machines.device_id, employees.username, machines.operating_system
> FROM machines
> INNER JOIN employees ON machines.device_id = employees.device_id;
> ```

---

## 3️⃣ Find unassigned machines and users without machines (LEFT & RIGHT JOIN)

### 3.1 LEFT JOIN: keep everything from the left table

A `LEFT JOIN` keeps every row from the first table (`machines`), even if there is no match in the second table.

**Goal:** See every machine, including ones that are not assigned to anyone.

```sql
SELECT *
FROM machines
LEFT JOIN employees ON machines.device_id = employees.device_id;
```

<img width="886" height="712" alt="03-left-join" src="https://github.com/user-attachments/assets/d2d97918-1016-4426-b501-f7fbf5ab29f5" />
*Screenshot provided by Google Cybersecurity Certificate lab environment*

**Key Observations:**
- All machines are listed.
- If a machine has no user, the employee columns show `NULL`.

### 3.2 RIGHT JOIN: keep everything from the right table

A `RIGHT JOIN` keeps every row from the second table (`employees`), even if there is no match in the first table.

**Goal:** See every employee, including those who have not been issued a machine.

```sql
SELECT *
FROM machines
RIGHT JOIN employees ON machines.device_id = employees.device_id;
```

<img width="886" height="676" alt="04-right-join" src="https://github.com/user-attachments/assets/88de3ecd-f010-4782-9d3e-e39377a59128" />
*Screenshot provided by Google Cybersecurity Certificate lab environment*

**Key Observations:**
- All employees are listed.
- If an employee has no machine, the machine columns show `NULL`.

---

## 4️⃣ Retrieve login attempt data for all employees (INNER JOIN)

To continue investigating the security incident, I needed the login attempts made by employees. I performed an inner join on the `employees` and `log_in_attempts` tables, linking them on the common `username` column.

```sql
SELECT *
FROM employees
INNER JOIN log_in_attempts ON employees.username = log_in_attempts.username;
```

<img width="885" height="637" alt="05-inner-join-login-attempts" src="https://github.com/user-attachments/assets/20e4e3b5-3800-48f7-ac30-c02617161947" />
*Screenshot provided by Google Cybersecurity Certificate lab environment*

**Key Observations:**
- Each login attempt is now linked to the employee who made it.
- This makes it easier to investigate suspicious activity by person, department, or device.

---

## ✅ Summary

In this activity, I gained practical experience using SQL joins to combine data from multiple tables in MariaDB to support a security investigation:

- **INNER JOIN:** returns only matching rows between tables (employees with machines, employees with login attempts).
- **LEFT JOIN:** keeps all rows from the left table to find machines with no assigned user.
- **RIGHT JOIN:** keeps all rows from the right table to find employees with no assigned machine.

These skills help security analysts connect separate pieces of data, such as devices, users, and login activity, to build a complete picture during an investigation.
