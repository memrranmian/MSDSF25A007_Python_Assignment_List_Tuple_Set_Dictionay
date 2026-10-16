# Python Assignment: Scenario-Based Problem Solving
## Core Data Structures: Lists, Tuples, Sets, and Dictionaries

**Course:** Data Analytics (DA-F26)  
**Student Roll No:** MSDSF25A007  
**Project Notebook:** [`MSDSF25A007_Python_Assignment_List_Tuple_Set_Dictionay.ipynb`](./MSDSF25A007_Python_Assignment_List_Tuple_Set_Dictionay.ipynb)

---

## 📌 Executive Summary

This repository contains the complete, production-grade solutions for Assignment 4 on Python's core data structures: **Lists**, **Tuples**, **Sets**, and **Dictionaries**. 

Each scenario simulates a real-world software system featuring interactive user input loops, robust input validation, case-insensitive and plural-tolerant data handling, and comprehensive statistical summaries.

---

## 🏗️ Architecture: When & Why Each Data Structure is Used

Understanding the specific characteristics and Big-O efficiency of each data structure is fundamental to writing performant Python code and acing technical evaluations.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       PYTHON DATA STRUCTURE SELECTOR                        │
└─────────────────────────────────────────────────────────────────────────────┘
                                       │
                    Is the data key-value paired?
                       /                       \
                     YES                       NO
                     /                           \
         [ DICTIONARY ]                    Is uniqueness required?
         • Fast O(1) Lookups                  /             \
         • Associative data                 YES             NO
         • Course / Product info            /                 \
                                      [ SET ]           Is data mutable?
                                      • O(1) membership      /     \
                                      • Auto-dedup         YES     NO
                                      • Category lists     /         \
                                                      [ LIST ]    [ TUPLE ]
                                                      • Order     • Fixed record
                                                      • Dynamic   • Immutable
                                                      • Rosters   • Coordinates
```

### Quick Technical Comparison

| Data Structure | Syntax | Mutability | Ordering | Duplicates Allowed | Primary Purpose in this Assignment |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **List** | `[item1, item2]` | Mutable (Modifiable) | Ordered (Indexed) | Yes | Sequential records, class rosters, logs, and attendee tracking |
| **Tuple** | `(item1, item2)` | Immutable (Fixed) | Ordered (Indexed) | Yes | Bundled read-only records: (code, credits), (price, qty), (name, date, venue) |
| **Set** | `{item1, item2}` | Mutable (Add/Remove) | Unordered | **No (Unique only)** | O(1) fast membership lookups, duplicate entry rejection, distinct categories/grades |
| **Dictionary** | `{key: value}` | Mutable | Ordered (Python 3.7+) | Unique Keys, Duplicate Values | Direct associative mapping, lookup catalogs, and cumulative category metrics |

---

## 🔬 Scenario Breakdown & Technical Implementations

### Scenario 01: University Course Registration System
* **Domain Problem:** A student registers for academic courses. Duplicate registrations must be flagged and rejected, while calculating total cumulative credit hours.
* **Applied Data Structures:**
  - **Dictionary:** Pre-populated university `course_catalogue` mapping course code to `(name, credit_hours)`.
  - **Set:** `registered_codes` set acts as an $O(1)$ fast guard to detect and reject repeated course codes immediately.
  - **Tuple:** Each registered course is packaged as an immutable `(course_code, credit_hours)` record.
  - **List:** `registered_courses` preserves the exact chronological registration order.
* **Key Features:**
  - Dynamic user prompt with instant validation against the course catalog.
  - Rejection logging for duplicate attempts.
  - Graceful exit via `done` or `Escape` key (empty string handling).

---

### Scenario 02: Inventory and Stock Management System
* **Domain Problem:** A shopkeeper tracks products, prices, quantities, and categories, flagging low-stock items and supporting real-time updates.
* **Applied Data Structures:**
  - **Dictionary:** Core store inventory (`inventory[product_name] = {"price_qty": (price, qty), "category": str}`).
  - **Tuple:** Bundles `(price, quantity)` per product to enforce paired atomic updates.
  - **Set:** Dynamically aggregates distinct product categories across the entire catalog.
  - **List:** Automatically records products requiring restock where $\text{quantity} < 5$.
* **Key Features:**
  - **Intelligent Product Matcher:** Case-insensitive and singular/plural tolerant (`apple` $\leftrightarrow$ `Apples`), preventing fragmented inventory.
  - **Cumulative Restock:** Entering existing products increments current stock instead of wiping it.
  - Strict input sanitization rejecting negative prices and quantities.

---

### Scenario 03: Event Registration and Attendance Management
* **Domain Problem:** Society event organizer registers participants, takes live event attendance, and computes absentee lists and attendance percentages.
* **Applied Data Structures:**
  - **Tuple:** Fixed event metadata: `("TechFest 2026", "2026-11-10", "Main Auditorium")`.
  - **Dictionary:** Associates each registration ID with participant name (`reg_number -> name`).
  - **Set:** `registered_reg_numbers` prevents duplicate registrations and acts as a security check.
  - **List:** `attendees_list` chronologically records attendee tuples `(reg_num, name)` as participants arrive.
* **Key Features:**
  - Dual-phase interactive flow: Phase 1 (Registration) $\rightarrow$ Phase 2 (Attendance Check-in).
  - Unregistered participant rejection: Anyone not present in the registration set cannot be marked present.
  - Accurate calculation of absentees via set differences and formatted percentage reporting.

---

### Scenario 04: Online Examination Result Analyzer
* **Domain Problem:** Analyzing class examination performance across multi-subject exams, enforcing grade boundaries and calculating academic statistics.
* **Applied Data Structures:**
  - **Tuple:** Examination subjects `("Mathematics", "Physics", "Chemistry", "English", "Computer Science")` ensuring a fixed syllabus.
  - **List:** `class_roster` maintains student sequence.
  - **Dictionary:** Nested student marks dictionary (`student_marks[student] = {subject: score}`).
  - **Set:** Collects distinct grades achieved across the cohort.
* **Grade Boundaries:**
  - **A:** $80 - 100$
  - **B:** $70 - 79$
  - **C:** $60 - 69$
  - **D:** $50 - 59$
  - **F:** $< 50$
* **Key Features:**
  - Full keyboard-driven data entry for student names and multi-subject scores.
  - Real-time numerical bounds checking ($0 \le \text{mark} \le 100$).
  - Automatic identification of class topper, failed students ($\ge 1$ subject below 50), class average, and distinct grade sets.

---

### Scenario 05: Personal Expense Tracker and Spending Analysis
* **Domain Problem:** A personal finance manager tracking daily student expenses with category classification, localized to Pakistani Rupee (PKR).
* **Applied Data Structures:**
  - **Tuple:** Immutable expense record `(description, category, amount)`.
  - **List:** `expense_records` preserves chronological log of transactions.
  - **Dictionary:** `category_totals` aggregates spending totals by category key.
  - **Set:** `unique_categories` catalogs distinct spending categories.
* **Key Features:**
  - Interactive expense logging with validation rejecting negative monetary amounts.
  - Edge case handling: Clean graceful output if zero expenses are recorded.
  - Search engine: Filter expenses by specific categories and retrieve category subtotals.
  - Analytical metrics: Total expenditure (PKR), highest single transaction, and largest spending category.

---

## 🚀 How to Run the Notebook

### Prerequisites
- Python 3.10+
- Jupyter Notebook or VS Code / Antigravity IDE with Jupyter extension

### Execution Steps
1. Clone the repository:
   ```bash
   git clone https://github.com/memrranmian/MSDSF25A007_Python_Assignment_List_Tuple_Set_Dictionay.git
   cd MSDSF25A007_Python_Assignment_List_Tuple_Set_Dictionay
   ```
2. Launch Jupyter Notebook:
   ```bash
   jupyter notebook
   ```
3. Open `MSDSF25A007_Python_Assignment_List_Tuple_Set_Dictionay.ipynb` and execute the cells sequentially.
4. When prompted by `input()`, enter your values or type `done` / press `Enter` to conclude entry.


👨‍💻 Author & Academic Attribution
* **Name:** M Imran Mian
* **GitHub:** [@memrranmian](https://github.com/memrranmian)
