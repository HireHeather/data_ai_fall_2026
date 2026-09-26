# 🤝 SQL JOINs Made Easy

**Time:** ~20 min | **Level:** Total beginner 🌱 | **Where to practice:** [sql-practice.com](https://www.sql-practice.com/) (hospital.db)

## 🎯 Today's goal
Learn how to combine **two tables** into one result. That's it!

---

## 🤔 The problem

Here's a tiny slice of two hospital tables. *(The names are made up to keep it simple.)*

**`patients`**

| first_name | province_id |
|---|---|
| Anna | ON |
| Ben | NS |
| Cara | ON |

**`province_names`**

| province_id | province_name |
|---|---|
| ON | Ontario |
| NS | Nova Scotia |

❓ **What province does Ben live in?**

The `patients` table only says **NS**. The full name is in the other table. We need to **join** them! 🙌

---

## 🔑 What do they have in common?

Both tables have a **`province_id`** column. That's how SQL matches the rows up.

> 💡 **Think of it like this:** You have one list with your friends' names + zip codes, and another list with zip codes + city names. To find what city each friend lives in, you match them up **by zip code**. 📬

---

## 🤝 Your first JOIN

```sql
SELECT *
FROM patients
JOIN province_names
  ON patients.province_id = province_names.province_id;
```

**Result:**

| first_name | province_id | province_name |
|---|---|---|
| Anna | ON | Ontario |
| Ben | NS | Nova Scotia |
| Cara | ON | Ontario |

🎉 Now everything is in one place!

### 🗣️ Read it like a sentence
| SQL | In plain English |
|---|---|
| `FROM patients` | Start with the patients table |
| `JOIN province_names` | Bring in the province names table |
| `ON patients.province_id = province_names.province_id` | Match rows where the province IDs are the same |

---

## ✏️ Pick just the columns you want

Instead of `*`, name the columns. Put the **table name + a dot** in front so SQL knows where each one lives:

```sql
SELECT patients.first_name, patients.last_name, province_names.province_name
FROM patients
JOIN province_names
  ON patients.province_id = province_names.province_id;
```

---

## 🏋️ Your turn! (on sql-practice.com)

**1.** Show each patient's first name, last name, and full province name.
*Hint: it's the query right above. Type it yourself!* ✍️

**2.** Show each admission's diagnosis with the doctor's last name.
*Hint: join `admissions` and `doctors`. The matching columns have different names: `attending_doctor_id` and `doctor_id`.*

<details><summary>Answer</summary>

```sql
SELECT admissions.diagnosis, doctors.last_name
FROM admissions
JOIN doctors
  ON admissions.attending_doctor_id = doctors.doctor_id;
```
</details>

**3.** Show each patient's first name with their admission date.
*Hint: join `patients` and `admissions` on `patient_id`.*

<details><summary>Answer</summary>

```sql
SELECT patients.first_name, admissions.admission_date
FROM patients
JOIN admissions
  ON patients.patient_id = admissions.patient_id;
```
</details>

---

## 🧩 Bonus: The 4 Types of JOINs

So far everyone matched. But what if they **don't** all match? 🤔

**`patients`** (left table ⬅️)

| first_name | province_id |
|---|---|
| Anna | ON |
| Ben | NS |
| Cara | ZZ |

**`province_names`** (right table ➡️)

| province_id | province_name |
|---|---|
| ON | Ontario |
| NS | Nova Scotia |
| BC | British Columbia |

👀 Notice: **Cara's** province (ZZ) isn't in the right table. **BC** has no patients.

> 💡 **Party analogy:** The left table is **your** guest list. The right table is **your friend's** guest list. Each JOIN is a different way to decide who gets invited! 🎉

### 🤝 INNER JOIN: only matches on BOTH sides

```sql
SELECT * FROM patients
INNER JOIN province_names ON patients.province_id = province_names.province_id;
```

| first_name | province_id | province_name |
|---|---|---|
| Anna | ON | Ontario |
| Ben | NS | Nova Scotia |

Cara and BC are left out. ❌

### ⬅️ LEFT JOIN: everyone on the LEFT

```sql
SELECT * FROM patients
LEFT JOIN province_names ON patients.province_id = province_names.province_id;
```

| first_name | province_id | province_name |
|---|---|---|
| Anna | ON | Ontario |
| Ben | NS | Nova Scotia |
| Cara | ZZ | *NULL* |

Cara stays! No match, so her province name is blank (`NULL`).

### ➡️ RIGHT JOIN: everyone on the RIGHT

```sql
SELECT * FROM patients
RIGHT JOIN province_names ON patients.province_id = province_names.province_id;
```

| first_name | province_id | province_name |
|---|---|---|
| Anna | ON | Ontario |
| Ben | NS | Nova Scotia |
| *NULL* | BC | British Columbia |

BC stays! No patient, so the name is blank.

### 🌍 FULL OUTER JOIN: EVERYONE from both sides

```sql
SELECT * FROM patients
FULL OUTER JOIN province_names ON patients.province_id = province_names.province_id;
```

| first_name | province_id | province_name |
|---|---|---|
| Anna | ON | Ontario |
| Ben | NS | Nova Scotia |
| Cara | ZZ | *NULL* |
| *NULL* | BC | British Columbia |

Nobody gets left out! 🥳

*(To keep things simple, these results show `province_id` just once.)*

### 🧠 Quick comparison

| JOIN type | Who's included? |
|---|---|
| 🤝 **INNER** | Only matches on both sides |
| ⬅️ **LEFT** | Everything from the left + matches |
| ➡️ **RIGHT** | Everything from the right + matches |
| 🌍 **FULL OUTER** | Everything from both sides |

> 💡 **Real talk:** Most people use **INNER** and **LEFT** almost all the time. RIGHT JOIN is just a LEFT JOIN with the tables flipped!

---

## 📝 Remember this pattern

```sql
SELECT columns
FROM first_table
JOIN second_table
  ON first_table.shared_column = second_table.shared_column;
```

✅ **JOIN** = combine two tables
✅ **ON** = tell SQL which column matches
✅ Don't forget the **ON** part!
✅ **INNER** = matches only, **LEFT** = all of the left, **RIGHT** = all of the right, **FULL OUTER** = everything
