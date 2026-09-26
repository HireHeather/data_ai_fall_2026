# 🤝 SQL JOINs Made Easy

**Time:** ~20 min | **Level:** Total beginner 🌱 | **Where to practice:** [sql-practice.com](https://www.sql-practice.com/) (hospital.db)

## 🎯 Today's goal
Learn how to combine **two tables** into one result. That's it!

---

## 🤔 The problem

Here are the first few rows of the real `patients` table on sql-practice.com:

**`patients`**

| patient_id | first_name | last_name | city | province_id |
|---|---|---|---|---|
| 1 | Donald | Waterfield | Barrie | ON |
| 2 | Mickey | Baasha | Dundas | ON |
| 3 | Jiji | Sharma | Hamilton | ON |
| 8 | Sonny | Beckett | Port Hawkesbury | NS |

**`province_names`**

| province_id | province_name |
|---|---|
| ON | Ontario |
| NS | Nova Scotia |

❓ **What province does Sonny live in?**

The `patients` table only says **NS**. The full name is in the other table. We need to **join** them! 🙌

---

## 🔑 What do they have in common?

Both tables have a **`province_id`** column. That's how SQL matches the rows up.

> 💡 **Think of it like this:** You have one list with your friends' names + zip codes, and another list with zip codes + city names. To find what city each friend lives in, you match them up **by zip code**. 📬

---

## ⬅️ Your first JOIN: LEFT JOIN

```sql
SELECT *
FROM patients
LEFT JOIN province_names
  ON patients.province_id = province_names.province_id;
```

**Result:**

| patient_id | first_name | last_name | city | province_id | province_name |
|---|---|---|---|---|---|
| 1 | Donald | Waterfield | Barrie | ON | Ontario |
| 2 | Mickey | Baasha | Dundas | ON | Ontario |
| 3 | Jiji | Sharma | Hamilton | ON | Ontario |
| 8 | Sonny | Beckett | Port Hawkesbury | NS | Nova Scotia |

🎉 Now everything is in one place!

> 💜 **Why LEFT JOIN?** It's the JOIN I use every day. It keeps **every row** from your first table, so nothing disappears by accident. Start with your main table, then add the extra info.

### 🗣️ Read it like a sentence
| SQL | In plain English |
|---|---|
| `FROM patients` | Start with the patients table |
| `LEFT JOIN province_names` | Keep **every** patient, and bring in their province name |
| `ON patients.province_id = province_names.province_id` | Match rows where the province IDs are the same |

---

## ✏️ Pick just the columns you want

Instead of `*`, name the columns. Put the **table name + a dot** in front so SQL knows where each one lives:

```sql
SELECT patients.first_name, patients.last_name, province_names.province_name
FROM patients
LEFT JOIN province_names
  ON patients.province_id = province_names.province_id;
```

---

## 📝 Remember this pattern

```sql
SELECT columns
FROM first_table
LEFT JOIN second_table
  ON first_table.shared_column = second_table.shared_column;
```

## 🏋️ Your turn! (on sql-practice.com)

**1.** Show each patient's first name with their admission date.
*Hint: join `patients` and `admissions` on `patient_id`.*

<details><summary>Answer</summary>

```sql
SELECT patients.first_name, admissions.admission_date
FROM patients
LEFT JOIN admissions
  ON patients.patient_id = admissions.patient_id;
```
</details>

---

## 🧩 Bonus: The 4 Types of JOINs

So far everyone matched. But what if they **don't** all match? 🤔

Let's use three real patients, and **imagine** a smaller `province_names` table that's missing Nova Scotia and has British Columbia instead:

**`patients`** (left table ⬅️)

| first_name | province_id |
|---|---|
| Donald | ON |
| Mickey | ON |
| Sonny | NS |

**`province_names`** (right table ➡️, pretend version)

| province_id | province_name |
|---|---|
| ON | Ontario |
| BC | British Columbia |

👀 Notice: **Sonny's** province (NS) isn't in the right table. **BC** has no patients.

> 💡 **Party analogy:** The left table is **your** guest list. The right table is **your friend's** guest list. Each JOIN is a different way to decide who gets invited! 🎉

### 🤝 INNER JOIN: only matches on BOTH sides

```sql
SELECT * FROM patients
INNER JOIN province_names ON patients.province_id = province_names.province_id;
```

| first_name | province_id | province_name |
|---|---|---|
| Donald | ON | Ontario |
| Mickey | ON | Ontario |

Sonny and BC are left out. ❌

### ⬅️ LEFT JOIN: everyone on the LEFT ⭐ (our go-to!)

```sql
SELECT * FROM patients
LEFT JOIN province_names ON patients.province_id = province_names.province_id;
```

| first_name | province_id | province_name |
|---|---|---|
| Donald | ON | Ontario |
| Mickey | ON | Ontario |
| Sonny | NS | *NULL* |

Sonny stays! No match, so his province name is blank (`NULL`).

### ➡️ RIGHT JOIN: everyone on the RIGHT

```sql
SELECT * FROM patients
RIGHT JOIN province_names ON patients.province_id = province_names.province_id;
```

| first_name | province_id | province_name |
|---|---|---|
| Donald | ON | Ontario |
| Mickey | ON | Ontario |
| *NULL* | BC | British Columbia |

BC stays! No patient, so the name is blank.

### 🌍 FULL OUTER JOIN: EVERYONE from both sides

```sql
SELECT * FROM patients
FULL OUTER JOIN province_names ON patients.province_id = province_names.province_id;
```

| first_name | province_id | province_name |
|---|---|---|
| Donald | ON | Ontario |
| Mickey | ON | Ontario |
| Sonny | NS | *NULL* |
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

> 💡 **Real talk:** **LEFT JOIN** is your go-to! You'll see INNER JOIN in other people's code, so it's good to recognize. RIGHT JOIN is just a LEFT JOIN with the tables flipped, so you rarely need it.

---

## 📝 Remember this pattern

```sql
SELECT columns
FROM first_table
LEFT JOIN second_table
  ON first_table.shared_column = second_table.shared_column;
```

✅ **LEFT JOIN** = start with your main table and add info from another
✅ **ON** = tell SQL which column matches
✅ Don't forget the **ON** part!
✅ **INNER** = matches only, **LEFT** = all of the left, **RIGHT** = all of the right, **FULL OUTER** = everything

✅ **JOIN** = combine two tables
✅ **ON** = tell SQL which column matches
✅ Don't forget the **ON** part!
✅ **INNER** = matches only, **LEFT** = all of the left, **RIGHT** = all of the right, **FULL OUTER** = everything
