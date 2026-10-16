# 📘 Viva & Quiz Preparation Guide
## Python Assignment: Lists, Tuples, Sets & Dictionaries

> Written by a student, for a student — in plain simple language. No jargon, no confusion!

---

## 🧠 First, Understand the 4 Data Structures — In ONE Line Each

| Structure | Think of it as… | Can change? | Allows duplicates? | Ordered? |
|-----------|----------------|-------------|-------------------|---------|
| **List** `[]` | A shopping list — items in order | ✅ Yes | ✅ Yes | ✅ Yes |
| **Tuple** `()` | A printed receipt — fixed forever | ❌ No | ✅ Yes | ✅ Yes |
| **Set** `{}` | A bag of unique coins — no repeats | ✅ Yes | ❌ No | ❌ No |
| **Dictionary** `{key: value}` | A phone book — name → number | ✅ Yes | Keys: No / Values: Yes | ✅ Yes (Python 3.7+) |

---

## 🔑 Golden Rules the Professor Will Ask About

1. **Use a Set when you want to prevent duplicates** — sets automatically throw away repeated values.
2. **Use a Tuple when data should NOT change** — like event name, date, venue.
3. **Use a List when order matters and you'll add/remove items** — like a to-do list or attendance roll.
4. **Use a Dictionary when you want to look something up by a label (key)** — like looking up a student's marks by their name.

---

## 📂 Scenario 01 — University Course Registration System

### 🎯 What does this program do?
A student types in course codes. The program:
- Accepts valid courses ✅
- Rejects invalid course codes ❌
- Rejects **duplicate** course entries ❌
- Shows total credit hours at the end

### 🏗️ Why each data structure was used:

| What | Data Structure | Why |
|------|---------------|-----|
| List of successfully registered courses | **List** | We need them in the order the student registered, and we can loop through them later |
| Preventing duplicate course codes | **Set** | Sets automatically block repetition. Checking "is X in a set?" is super fast (O(1)) |
| Storing course code + credit hours together | **Tuple** | These two go together and should not be separated or modified. A tuple is perfect for bundling fixed related info |
| Full course details (code → name, credits) | **Dictionary** | We want to "look up" a course by its code. Dictionary = lookup table |

### 💡 Key Intuition:
> When the student types `CS101` twice — the **set** sees it's already there and says "nope, already registered!" — the list never even gets it. This is how we **prevent duplicate registrations**.

### 🔍 Code Insight:
```python
if user_input in registered_codes:   # CHECK SET — very fast
    rejected_duplicates.append(user_input)
else:
    registered_codes.add(user_input)              # add to SET
    course_record = (user_input, credit_hours)    # make a TUPLE
    registered_courses.append(course_record)      # add to LIST
```

### ❓ Likely Viva Questions:
- **Q: Why did you use a set instead of a list to check duplicates?**
  - A: Because sets don't allow duplicates — when you `.add()` something that already exists, it simply ignores it. Also, checking if something is in a set is faster than a list.
- **Q: Why is the course code + credit hours stored in a tuple?**
  - A: Because these two values belong together and don't need to be changed. Tuples are immutable (cannot be modified), which makes data safer.
- **Q: What does `sum(credits for _, credits in registered_courses)` do?**
  - A: It loops through the list of tuples and adds up only the credit hours (second element of each tuple). The `_` means "I don't care about the first element (course code)."

---

## 📂 Scenario 02 — Inventory and Stock Management System

### 🎯 What does this program do?
A shopkeeper manages their products:
- Stores product name, price, quantity, category
- Flags products that need restocking (quantity < 5)
- Shows total inventory value
- Allows updating price or quantity

### 🏗️ Why each data structure was used:

| What | Data Structure | Why |
|------|---------------|-----|
| Full product info | **Dictionary** | Product name is the key. Fast lookup: "Give me Apple's price" → `inventory["Apple"]` |
| Price + Quantity bundled per product | **Tuple** | Price and quantity always go together. Stored as `(price, qty)` |
| Unique categories | **Set** | We don't want "Fruits" appearing 10 times. Set auto-removes duplicates |
| Restock list | **List** | An ordered list of product names that need restocking; we might add/remove as stock is updated |

### 💡 Key Intuition:
> Why store (price, quantity) as a **tuple** inside the dictionary? Because these two values are always used together for a product. Also, when you update price or quantity, you create a **new tuple** — you can't edit a tuple in-place. This actually protects data integrity.

### 🔍 Code Insight:
```python
# Storing price and qty as a TUPLE inside the DICTIONARY
inventory["Apple"] = {"price_qty": (30.0, 50), "category": "Fruits"}

# Updating: can't modify tuple, so we create a new one
old_price, _ = inventory["Apple"]["price_qty"]
inventory["Apple"]["price_qty"] = (old_price, new_qty)  # new TUPLE
```

### ❓ Likely Viva Questions:
- **Q: Why is the restock list a list and not a set?**
  - A: Lists maintain order and can have duplicates if needed. Also, a list is better when we want to display products in a specific order or process them one by one.
- **Q: What is the set used for here?**
  - A: The set holds all **unique category names** like {"Fruits", "Dairy", "Grains"}. Even if 10 products belong to "Fruits", the set only shows it once.
- **Q: How do you calculate total inventory value?**
  - A: `price × quantity` for each product, then add them all up: `sum(price * qty for each product)`.

---

## 📂 Scenario 03 — Event Registration and Attendance Management

### 🎯 What does this program do?
A university event (like TechFest):
- Lets students register using their registration number
- Prevents duplicate registrations
- Marks attendance only for registered participants
- Shows who attended, who was absent, and attendance percentage

### 🏗️ Why each data structure was used:

| What | Data Structure | Why |
|------|---------------|-----|
| Event name, date, venue | **Tuple** | Event details are fixed once announced — tuple is immutable, perfect for this |
| Registration number → name mapping | **Dictionary** | To look up "who is REG001?" quickly |
| Set of registered reg. numbers | **Set** | Fast duplicate checking + fast "is this person registered?" check |
| List of attendees | **List** | Attendance order matters; we can count how many attended using `len()` |

### 💡 Key Intuition:
> The **set** does double duty here:
> 1. Prevents duplicate registrations (can't add same reg number twice)
> 2. Used to check if someone is registered before marking them present
>
> The key rule: **"Only registered people can be marked present"** — we check `if reg_num not in registered_reg_numbers` before adding to the attendance list.

### 🔍 Code Insight:
```python
# TUPLE – event info is fixed
event_details = ("TechFest 2026", "2026-11-10", "Main Auditorium")

# Checking registration via SET before marking attendance
def mark_attendance(reg_num):
    if reg_num not in registered_reg_numbers:  # SET lookup
        print("Not registered – cannot mark present!")
        return
    attendees_list.append(reg_num)  # only then add to LIST
```

### ❓ Likely Viva Questions:
- **Q: Why use a tuple for event details?**
  - A: Event name, date, and venue don't change once the event is announced. Tuple is immutable, so no one can accidentally change them in the code.
- **Q: How do you calculate attendance percentage?**
  - A: `(number of attendees / total registered) × 100`
- **Q: What happens if an unregistered person tries to mark attendance?**
  - A: The program checks the set — if their reg number isn't in the set, it prints an error and doesn't add them to the attendees list.
- **Q: How do you find absentees?**
  - A: Loop through all registered numbers — anyone NOT in the `attendees_list` is an absentee.

---

## 📂 Scenario 04 — Online Examination Result Analyzer

### 🎯 What does this program do?
A teacher analyzes exam results:
- Stores marks for each student in each subject
- Calculates total, average, and grade per student
- Finds class topper, failed students, class average
- Shows all unique grades used in the class

### 🏗️ Why each data structure was used:

| What | Data Structure | Why |
|------|---------------|-----|
| List of subject names | **Tuple** | Subjects in an exam are fixed. Using a tuple ensures no subject is accidentally added or removed |
| Class roster (student names) | **List** | Ordered list of students; we iterate through them one by one |
| Student marks per subject | **Dictionary** | Nested dict: `student_name → {subject: marks}`. Fast lookup by student name |
| Distinct grades in the class | **Set** | Auto-removes duplicates. If 5 students get "A", the set only shows "A" once |

### 💡 Key Intuition:
> Why is `subjects` a **tuple** and `class_roster` a **list**?
> - Subjects are fixed for the exam — they don't change. → **Tuple**
> - The class roster might have students added or removed. → **List**
>
> This is a great example of choosing the right data structure based on whether data changes or not.

### 🔍 Code Insight:
```python
# TUPLE for subjects — cannot be modified
subjects = ("Mathematics", "Physics", "Chemistry", "English", "Computer Science")

# DICTIONARY — student → their marks in each subject
student_marks = {
    "Alice": {"Mathematics": 92, "Physics": 88, ...},
    "Bob":   {"Mathematics": 45, "Physics": 55, ...},
}

# Grade logic
def get_grade(average):
    if average >= 80: return "A"
    elif average >= 70: return "B"
    elif average >= 60: return "C"
    elif average >= 50: return "D"
    else: return "F"

# Find topper using max() on dictionary
topper = max(results, key=lambda s: results[s]["average"])
```

### ❓ Likely Viva Questions:
- **Q: Why use a set for distinct grades?**
  - A: Because we only want to know *which grades appear in the class*, not how many times. A set automatically removes duplicates, so even if 10 students get "B", the set shows "B" only once.
- **Q: How do you validate marks?**
  - A: Check if each mark is between 0 and 100. If not, raise a `ValueError` — this stops invalid data from entering the system.
- **Q: How do you find the class topper?**
  - A: Use `max()` with a `key` that compares average scores: `max(results, key=lambda s: results[s]["average"])`.
- **Q: How do you identify failed students?**
  - A: Check if **any** single subject mark is below 50 using `any(m < 50 for m in marks.values())`.
- **Q: What is `lambda` in Python?**
  - A: A lambda is a small, one-line anonymous function. `lambda s: results[s]["average"]` means "for a given student s, return their average" — used here to tell `max()` what to compare.

---

## 📂 Scenario 05 — Personal Expense Tracker and Spending Analysis

### 🎯 What does this program do?
A student tracks their daily expenses:
- Adds expenses with description, category, and amount
- Rejects negative amounts (validation)
- Searches expenses by category
- Shows total spending, category-wise breakdown, highest single expense, top spending category

### 🏗️ Why each data structure was used:

| What | Data Structure | Why |
|------|---------------|-----|
| All expense records | **List** | Maintains the chronological order of expenses; can be looped through |
| Each individual expense | **Tuple** | `(description, category, amount)` — three fields that belong together and shouldn't be edited after recording |
| Category → total spent | **Dictionary** | Maps each category to how much was spent in it. Fast update and lookup |
| All unique categories | **Set** | Auto-deduplication. Even if "Food" is added 10 times, the set shows it once |

### 💡 Key Intuition:
> Each expense is stored as a **tuple** inside a **list**. Think of it like a table:
> - The **list** is the whole table (all rows)
> - Each **tuple** is one row (one expense entry)
> - The **dictionary** is like a "group by category" summary
> - The **set** just tells you "what categories exist?"

### 🔍 Code Insight:
```python
# Adding an expense
record = ("Lunch at canteen", "Food", 85.00)  # TUPLE
expense_records.append(record)                  # add to LIST
unique_categories.add("Food")                   # add to SET
category_totals["Food"] = category_totals.get("Food", 0) + 85.00  # update DICT

# Finding highest expense — max() on list of tuples, compare by 3rd element (amount)
highest_expense = max(expense_records, key=lambda x: x[2])

# Finding top spending category — max value in dictionary
top_category = max(category_totals, key=category_totals.get)
```

### ❓ Likely Viva Questions:
- **Q: Why is each expense record a tuple?**
  - A: Because once you've recorded an expense, you shouldn't be able to change it (immutability = data safety). A tuple is the right choice for fixed records.
- **Q: How do you reject negative amounts?**
  - A: With a simple `if amount < 0: return` check before adding to the list.
- **Q: What does `.get("Food", 0)` do in the dictionary?**
  - A: It tries to get the value for "Food". If "Food" doesn't exist yet in the dictionary, it returns 0 (instead of crashing with a KeyError). Then we add the new amount to it.
- **Q: How do you find the highest spending category?**
  - A: `max(category_totals, key=category_totals.get)` — this finds the key (category name) with the highest value (total amount).
- **Q: How do you search by category?**
  - A: Filter the list of tuples: `[rec for rec in expense_records if rec[1] == category]`

---

## 🎓 Quick Cheat Sheet — Choosing the Right Data Structure

```
Need to store items in ORDER?                  → List or Tuple
  └── Items will CHANGE (add/remove)?          → List  [ ]
  └── Items are FIXED, don't change?           → Tuple ( )

Need to store KEY → VALUE pairs?               → Dictionary { key: value }
  └── "Look up X by its name/code/ID"

Need only UNIQUE items (no duplicates)?        → Set { }
  └── "Which categories exist?" / "Is X already registered?"
```

---

## ⚡ Common Python Syntax to Remember

```python
# List
my_list = []
my_list.append(item)          # add item
len(my_list)                  # how many items?
for item in my_list:          # loop through

# Tuple
my_tuple = (value1, value2)   # create
a, b = my_tuple               # unpack (destructure)
my_tuple[0]                   # access by index

# Set
my_set = set()
my_set.add(item)              # add item (no duplicates)
item in my_set                # check membership (very fast)

# Dictionary
my_dict = {}
my_dict[key] = value          # add/update
my_dict.get(key, default)     # safe lookup (no crash if missing)
for key, value in my_dict.items():  # loop through all pairs
max(my_dict, key=my_dict.get) # find key with max value
```

---

## 💬 Phrases to Say in Viva (that impress professors!)

| Instead of saying... | Say this ✅ |
|---------------------|-----------|
| "I used a list to store stuff" | "I used a list because it maintains insertion order and allows dynamic addition of elements" |
| "Set doesn't have duplicates" | "Sets enforce uniqueness and provide O(1) average-time membership testing" |
| "Tuple can't be changed" | "Tuples are immutable, which ensures data integrity for fixed records like event details" |
| "Dictionary maps one thing to another" | "Dictionary provides O(1) key-based lookup, making it ideal for associating registration numbers with names" |

---

## 📝 Summary Table — Which Scenario Uses What

| Scenario | List | Tuple | Set | Dictionary |
|----------|------|-------|-----|------------|
| 01: Course Registration | Registered courses | (code, credits) | Prevent duplicate codes | Course catalogue |
| 02: Inventory | Restock list | (price, qty) | Unique categories | Product info |
| 03: Attendance | Attendees | Event details | Registered reg. numbers | Reg# → Name |
| 04: Exam Results | Class roster | Subject names | Distinct grades | Student marks |
| 05: Expense Tracker | All expense records | Each expense entry | Unique categories | Category totals |

---

*Good luck in your viva! You've got this! 🚀*
