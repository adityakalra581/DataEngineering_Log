# Class 12 Computer Science (083) — Chapter-wise Notes

### (Structured to match the 14-chapter layout of the Sumita Arora 2026 edition)

---

## Chapter 1: Python Revision Tour — I

**Tokens in Python** — the smallest individual units of a program.

- **Keywords**: reserved words with special meaning (`if`, `else`, `while`, `def`, `True`, `None`, etc.) — cannot be used as variable names.
- **Identifiers**: names given to variables, functions, classes. Rules: must start with a letter or underscore, can contain letters/digits/underscore, case-sensitive, cannot be a keyword.
- **Literals**: fixed values written directly in code (numbers, strings, `True`/`False`, `None`).
- **Operators**: symbols that perform operations (`+`, `-`, `==`, `and`, etc.)
- **Punctuators**: symbols like `()`, `{}`, `[]`, `:`, `,` that structure code.

**Variables and Assignment**

- Python is **dynamically typed** — you don't declare a type; it's inferred from the value assigned, and can change if reassigned.

```python
x = 10        # x is int
x = "hello"   # now x is str — perfectly legal in Python
```

- **Multiple assignment** in one line:

```python
a, b, c = 1, 2, 3
x = y = z = 0      # all three assigned the same value
```

**Data Types**

- `int`, `float`, `complex`, `bool`, `str`, and container types (`list`, `tuple`, `dict`, `set`) — covered in depth in Chapter 2.

**Mutable vs Immutable**

- **Immutable** (cannot be changed after creation): `int`, `float`, `str`, `tuple`, `bool`.
- **Mutable** (can be changed in place): `list`, `dict`, `set`.
- This matters most when passing these to functions (see Chapter 3) — mutable objects can be changed by a function; immutable ones can't be changed in place.

**Expressions and Operators**

- Arithmetic: `+ - * / // % **` (`//` is floor division, `**` is exponentiation)
- Relational: `== != > < >= <=` (return `True`/`False`)
- Logical: `and`, `or`, `not`
- **Type casting**: explicit conversion using `int()`, `float()`, `str()`, etc.

```python
a = int("25")       # string to int
b = float(10)        # int to float
c = str(3.14)         # float to string
```

- **Math library functions** (`import math`): `math.sqrt()`, `math.ceil()`, `math.floor()`, `math.pow()`.

**Conditional Statements**

```python
if marks >= 90:
    print("A1")
elif marks >= 80:
    print("A2")
else:
    print("Needs improvement")
```

- **Nested if**: an `if` inside another `if` — used when a second condition only matters if the first is true.

**Looping Statements**

- **`for` loop**: iterates over a sequence (string, list, range, etc.) — used when the number of iterations is known/finite.

```python
for i in range(1, 6):
    print(i)
```

- **`while` loop**: repeats as long as a condition is true — used when the number of iterations is not known in advance.

```python
i = 1
while i <= 5:
    print(i)
    i += 1
```

**Jump Statements**

- `break` — exits the loop immediately.
- `continue` — skips the rest of the current iteration, moves to the next.

**Loop else & Nested Loops**

- A loop's `else` block runs only if the loop completes WITHOUT hitting a `break` — a feature unique to Python, worth highlighting since it surprises most students.

```python
for i in range(5):
    if i == 10:
        break
else:
    print("Loop completed without break")   # this WILL print here
```

- **Nested loops**: a loop inside another loop — common in pattern-printing and matrix-style problems.

---

## Chapter 2: Python Revision Tour — II

**Strings**

- Immutable — once created, individual characters cannot be reassigned (`s[0] = 'x'` raises an error).
- **Traversal**: iterate character by character using a `for` loop or indexing.
- **String Slicing**: `s[start:stop:step]` extracts a substring.

```python
s = "Aditya"
print(s[0:3])     # Adi
print(s[::-1])     # atyidA (reversed)
```

- **String operators**: `+` (concatenation), `*` (repetition), `in`/`not in` (membership).
- **String functions**: `len()`, `upper()`, `lower()`, `find()`, `replace()`, `split()`, `strip()`, `join()`.

**Lists**

- Ordered, **mutable** collection, can hold mixed data types.

```python
marks = [90, 85, 78]
marks.append(95)       # add element
marks[0] = 100          # modify element — allowed, unlike strings
marks.remove(78)        # remove by value
```

- **List vs String**: both support slicing/indexing, but lists are mutable, strings are not.
- **Making a true copy of a list**: `new_list = old_list.copy()` or `new_list = old_list[:]` — using `new_list = old_list` only copies the reference, not the data (a common exam trap).
- **List functions**: `len()`, `sort()`, `append()`, `insert()`, `pop()`, `extend()`, `index()`.

**Tuples**

- Ordered, **immutable** collection — once created, cannot be modified.

```python
point = (3, 4)
# point[0] = 5   # this would raise an error
```

- **Tuple vs List**: tuples are immutable and generally faster; lists are mutable and more flexible. Use tuples for fixed data that shouldn't change (e.g., coordinates, RGB values).
- **Tuple functions**: `len()`, `count()`, `index()`.

**Dictionaries**

- Unordered (conceptually; insertion order preserved since Python 3.7) collection of **key-value pairs**. Keys must be unique and immutable (string, int, tuple); values can be anything.

```python
student = {"name": "Aditya", "marks": 90}
print(student["name"])       # access by key
student["grade"] = "A"        # add new key-value pair
```

- **Characteristics**: fast lookup by key, no duplicate keys allowed, mutable.
- **Dictionary functions**: `keys()`, `values()`, `items()`, `get()`, `update()`, `pop()`.

**Sorting Techniques**

- **Bubble Sort**: repeatedly compares and swaps adjacent elements — see trace example in SQL/data-structures revision notes.
- **Insertion Sort**: builds the sorted list one element at a time, inserting each new element into its correct position among the already-sorted elements.

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j+1] = arr[j]
            j -= 1
        arr[j+1] = key
    return arr

print(insertion_sort([5, 2, 9, 1, 5]))
```

**Bubble vs Insertion — the comparison exam question**: Bubble sort repeatedly swaps adjacent out-of-order pairs across full passes; insertion sort grows a sorted section by inserting each element into its correct place — insertion sort is generally more efficient on nearly-sorted data.

---

## Chapter 3: Working with Functions

**Why functions?** Avoid code repetition, improve readability, enable modular/reusable design.

**Python function types**

- **Built-in functions**: `print()`, `len()`, `type()` — provided by Python itself.
- **User-defined functions**: created using `def`.
- **Library/module functions**: from imported modules (covered in Chapter 4).

```python
def greet(name):
    print(f"Hello, {name}!")

greet("Aditya")   # calling/invoking the function
```

**Arguments and Parameters**

- **Parameter**: the variable listed in the function definition.
- **Argument**: the actual value passed when calling the function.

**Types of arguments**

- **Positional/Required**: matched to parameters by position/order.
- **Default**: parameter has a pre-set value, used if no argument is passed for it.
- **Keyword/Named**: argument passed using `parameter_name=value`, order doesn't matter.

```python
def student(name, marks=50):       # marks has a default value
    print(name, marks)

student("Rahul")                    # uses default marks=50
student("Aditya", 90)                # positional
student(name="Priya", marks=85)      # keyword
student(marks=95, name="Rahul")      # keyword, order doesn't matter
```

**Returning values**

- `return` sends a value back to the caller; a function without `return` implicitly returns `None`.
- A function can **return multiple values** as a tuple:

```python
def min_max(numbers):
    return min(numbers), max(numbers)

low, high = min_max([3, 7, 1, 9])
```

**Scope of Variables**

- **Local scope**: variable defined inside a function, accessible only within it.
- **Global scope**: variable defined outside any function, accessible everywhere (read access by default inside functions; needs the `global` keyword to modify it from within a function).

```python
x = 10  # global

def show():
    x = 20   # this creates a NEW local x, doesn't affect the global one
    print(x)  # 20

show()
print(x)  # still 10
```

- **Name resolution**: Python looks up a name using the LEGB rule — Local → Enclosing → Global → Built-in.

**Mutable/Immutable properties in function calls**

- Passing a **mutable** object (like a list) to a function allows the function to modify the original object's contents.
- Passing an **immutable** object (like an int or string) means the function works on a copy — the original stays unchanged outside the function.

```python
def modify_list(lst):
    lst.append(100)   # modifies the ORIGINAL list

def modify_num(n):
    n += 10            # does NOT affect the original variable

my_list = [1, 2, 3]
modify_list(my_list)
print(my_list)   # [1, 2, 3, 100] — changed!

num = 5
modify_num(num)
print(num)   # still 5 — unchanged
```

This is one of the most important and most-tested conceptual points in this chapter — worth a dedicated session.

---

## Chapter 4: Using Functions Defined in Modules

**Library vs Module**

- A **module** is a single `.py` file containing reusable functions/variables.
- A **library** is a collection of related modules.

**Importing modules**

```python
import math                    # import entire module
print(math.sqrt(16))

from math import sqrt, pi      # import specific objects
print(sqrt(16), pi)

import math as m               # import with an alias
print(m.sqrt(16))
```

**Commonly used standard modules for board exams**

*`random` module* — generates random numbers.

```python
import random
print(random.random())          # random float between 0 and 1
print(random.randint(1, 10))    # random integer between 1 and 10 (inclusive)
print(random.choice([1,2,3]))   # randomly picks one element from a sequence
```

*`string` module* — provides string constants.

```python
import string
print(string.ascii_lowercase)   # 'abcdefghijklmnopqrstuvwxyz'
print(string.digits)             # '0123456789'
```

*`statistics` module* — statistical calculations.

```python
import statistics
data = [10, 20, 30, 40]
print(statistics.mean(data))     # average
print(statistics.median(data))   # middle value
```

**Exam tip**: Students often get confused on `import X` vs `from X import Y` — with the first, you must prefix every call with `X.`; with the second, you call the function directly without a prefix.

---

## Chapter 5: Exception Handling

**What is an exception?** A runtime error that disrupts normal program flow (distinct from a syntax error, which prevents the program from running at all).

**try-except-else-finally**

```python
try:
    a = int(input("Enter numerator: "))
    b = int(input("Enter denominator: "))
    result = a / b
except ZeroDivisionError:
    print("Cannot divide by zero!")
except ValueError:
    print("Please enter valid integers.")
else:
    print("Result:", result)          # runs only if no exception
finally:
    print("Execution complete.")       # always runs
```

**Common built-in exceptions**

| Exception | Cause |
| --- | --- |
| `ZeroDivisionError` | Division by zero |
| `ValueError` | Invalid value for an operation |
| `TypeError` | Operation on incompatible types |
| `IndexError` | List/string index out of range |
| `KeyError` | Dictionary key doesn't exist |
| `FileNotFoundError` | File doesn't exist when opening in read mode |

**Handling multiple errors**

```python
try:
    x = int(input("Enter a number: "))
    y = 10 / x
except (ValueError, ZeroDivisionError) as e:
    print("Error occurred:", e)
```

**raise** — manually triggers an exception, often for custom validation.

```python
def set_age(age):
    if age < 0:
        raise ValueError("Age cannot be negative")
    return age
```

**Exam trap**: order of `except` blocks matters — a general exception class placed before a specific one makes the specific block unreachable.

---

## Chapter 6: File Handling

**Why files?** Variables lose their values once a program ends; files provide persistent storage.

**File types**

- **Text files**: human-readable characters, opened in `'t'` mode (default).
- **Binary files**: stored as raw bytes, opened in `'b'` mode, used with `pickle` for Python objects.
- **CSV files**: comma-separated tabular text data, handled with the `csv` module.

**Opening/closing files**

| Mode | Meaning |
| --- | --- |
| `'r'` | Read (file must exist) |
| `'w'` | Write (creates/overwrites) |
| `'a'` | Append |
| `'r+'` | Read and write |

```python
f = open("data.txt", "r")
# ... operations ...
f.close()

# Preferred: auto-closes even if an error occurs
with open("data.txt", "r") as f:
    content = f.read()
```

**Working with text files**

```python
with open("students.txt", "w") as f:
    f.write("Aditya,90\n")
    f.write("Rahul,85\n")

with open("students.txt", "r") as f:
    for line in f:
        print(line.strip())   # strip() removes the trailing whitespace/newline
```

- `flush()` — forces buffered data to be written to disk immediately without closing the file.
- **File pointer**: tracks the current read/write position; `seek()` moves it, `tell()` reports its current position.

**Working with binary files — pickling/unpickling**

```python
import pickle

data = {"name": "Aditya", "marks": 90}
with open("data.dat", "wb") as f:
    pickle.dump(data, f)        # pickling — writing object to binary file

with open("data.dat", "rb") as f:
    loaded = pickle.load(f)      # unpickling — reading object back
    print(loaded)
```

- **Searching** in a binary file: read records one by one (in a loop, catching `EOFError` at the end) and check a condition.
- **Updating** a binary file: typically done by reading all records, modifying the required one in memory, and rewriting the entire file.

**Working with CSV files**

```python
import csv

with open("data.csv", "w", newline="") as f:
    writer = csv.writer(f)
    writer.writerow(["Name", "Marks"])
    writer.writerow(["Aditya", 90])

with open("data.csv", "r") as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)
```

**Exam trap**: forgetting `newline=""` when writing CSVs on Windows causes blank lines between rows.

---

## Chapter 7: Data Structures

**What is a data structure?** A way of organizing and storing data so it can be accessed and modified efficiently.

**Elementary data representation / types of data structures (conceptual overview)**

- **Linear**: Arrays/Lists, Stacks, Queues, Linked Lists — elements arranged in sequence.
- **Non-linear**: Trees, Graphs — elements arranged hierarchically or in networks (conceptual mention at this level, not implemented in Python here).

**Stack — the main focus of this chapter**

- **LIFO (Last In, First Out)** — the last element pushed is the first popped.
- Core operations: Push (add to top), Pop (remove from top), Peek (view top), isEmpty.

```python
stack = []

def push(item):
    stack.append(item)

def pop():
    if not stack:
        print("Stack is empty")
        return None
    return stack.pop()

def peek():
    return stack[-1] if stack else None

push(10)
push(20)
push(30)
print("Popped:", pop())   # 30
print("Top now:", peek()) # 20
```

**Stack applications — most exam-relevant part**

- **Infix to Postfix/Prefix conversion**: stacks help track operators and precedence.
- **Postfix expression evaluation**:

```python
def evaluate_postfix(expr):
    tokens = expr.split()
    s = []
    for token in tokens:
        if token.isdigit():
            s.append(int(token))
        else:
            b = s.pop()
            a = s.pop()
            if token == '+': s.append(a + b)
            elif token == '-': s.append(a - b)
            elif token == '*': s.append(a * b)
            elif token == '/': s.append(a / b)
    return s.pop()

print(evaluate_postfix("5 3 + 2 *"))  # (5+3)*2 = 16
```

- **Reversing a list/string**, **checking balanced parentheses** — other common stack applications worth demonstrating.

**Queue — conceptual + implementation**

- **FIFO (First In, First Out)** — the first element added is the first removed.

```python
from collections import deque

queue = deque()
queue.append("A")     # enqueue
queue.append("B")
print(queue.popleft()) # dequeue -> A
```

- **Circular Queue**: wraps the rear around to the start of the array when it reaches the end, reusing freed-up space — solves the wasted-space problem of a simple linear queue.

**Linked Lists and Trees** — mentioned conceptually at this level (what they are, how they differ from arrays) without full Python implementation expected in most board papers; check your specific year's sample paper for how deep to go.

---

## Chapter 8: Computer Networks — I

**Why networks?** Resource sharing, communication, centralized data management, cost reduction.

**Types of Networks by geographical spread**

| Type | Range |
| --- | --- |
| PAN | \~10m (e.g., Bluetooth) |
| LAN | Single building/campus |
| MAN | A city |
| WAN | Country/globe (e.g., Internet) |

**Evolution of networking**

- **ARPANET**: the precursor to the modern Internet, developed for research/defense communication.
- **The Internet**: the global network of interconnected networks.
- **Interspace**: a client-server software enabling real-time, simultaneous collaboration (e.g., shared virtual spaces) — a less commonly tested but still mentioned historical term.

**Switching Techniques**

- **Circuit Switching**: dedicated path for the entire session (e.g., traditional telephone calls) — reliable, but wastes capacity when idle.
- **Message Switching**: entire message sent as one unit, stored and forwarded hop-by-hop.
- **Packet Switching**: data broken into packets, routed independently, reassembled at destination (how the Internet works) — efficient bandwidth use.

**Transmission Media — Wired**

- **Twisted Pair Cable**: cheap, moderate speed, used in telephone/LAN.
- **Coaxial Cable**: better shielding, higher bandwidth, used in cable TV.
- **Optical Fiber**: highest speed, immune to electromagnetic interference, expensive.

**Transmission Media — Wireless**

- **Microwave**: line-of-sight, long-distance tower-to-tower links.
- **Radio Wave**: omnidirectional, can penetrate walls, used in Wi-Fi.
- **Satellite**: covers very large areas, used for remote connectivity and GPS.

**Network Topologies**

- **Bus**: single shared cable; cheap but one failure can break the whole network.
- **Star**: all devices connect to a central hub/switch; easy to manage, but hub failure affects everyone.
- **Tree**: hierarchical combination of star networks; scalable but depends on the backbone/root. (Ring and Mesh are also worth knowing from general networking theory even where not explicitly listed in this book's edition — check your sample paper.)

---

## Chapter 9: Computer Networks — II

**Network Devices**

| Device | Function |
| --- | --- |
| Modem | Converts digital signal to analog (and back) for transmission over telephone lines |
| RJ-45 | Connector used for Ethernet/LAN cables |
| NIC (Network Interface Card) | Hardware that allows a device to connect to a network |
| Hub | Broadcasts incoming data to ALL ports (no intelligence) |
| Switch | Sends data only to the specific destination port |
| Repeater | Amplifies/regenerates a signal to extend transmission distance |
| Bridge | Connects two LAN segments, filtering traffic between them |
| Router | Connects different networks, routes data using IP addresses |
| Gateway | Connects networks using different protocols, translates between them |
| WiFi Card | Enables wireless network connectivity |

**Network Protocols**

| Protocol | Purpose |
| --- | --- |
| HTTP | Transfers web pages (Hypertext Transfer Protocol) |
| FTP | Transfers files between computers |
| TCP/IP | Core protocol suite governing how data is broken into packets, addressed, and delivered across networks |
| SLIP/PPP | Protocols for connecting a computer to the Internet over a serial line/telephone |
| VoIP | Voice over Internet Protocol — enables voice calls over the Internet |
| Telnet | Allows remote login to another computer over a network |

**Internetworking Terms**

- **WWW**: the system of interlinked documents accessed via the Internet.
- **Web Browser**: software to access/display web pages (Chrome, Firefox).
- **Web Server**: software/hardware that hosts and serves web pages.
- **URL**: the address used to locate a resource on the web.
- **Domain Name System (DNS)**: translates human-readable domain names into IP addresses.
- **Web Hosting**: service that makes a website accessible on the Internet.
- **HTML**: the markup language used to structure web pages.
- **XML**: a markup language for storing/transporting structured data (not for display, unlike HTML).

**Exam tip**: Protocol and device tables above are very commonly tested as matching/short-answer questions — worth drilling as flashcards.

---

## Chapter 10: Relational Databases

**Purpose of a DBMS** Solves the problems of flat-file storage — data redundancy, inconsistency, lack of security, and difficulty with concurrent multi-user access.

**Relational Model terminology**

| Term | Meaning |
| --- | --- |
| Relation | A table |
| Tuple | A row (record) |
| Attribute | A column (field) |
| Domain | Set of valid values for an attribute |
| Degree | Number of columns |
| Cardinality | Number of rows |

**Keys**

- **Candidate Key**: an attribute(s) that can uniquely identify a tuple.
- **Primary Key**: the chosen candidate key; cannot be NULL, must be unique.
- **Alternate Key**: candidate keys not chosen as primary.
- **Foreign Key**: an attribute referencing the primary key of another table, enforcing relationships.

**Views** A **View** is a virtual table — it doesn't store data itself, but presents data derived from one or more real tables via a stored query.

```sql
CREATE VIEW PassedStudents AS
SELECT RollNo, Name, Marks
FROM Student
WHERE Marks >= 33;

SELECT * FROM PassedStudents;
```

Why use views: simplicity (hide complex queries), security (show only specific columns to certain users), and data independence.

**MySQL Database System**

- **MySQL Server (Instance)**: the background process that stores and manages databases, processes queries (typically on port 3306).
- **MySQL Client**: the tool used to connect to the server and send commands — could be the command-line `mysql` tool, MySQL Workbench, or a Python program using a connector.

**Classification of SQL Statements (recap, detailed in Chapter 12)**

- DDL (structure), DML (data), DQL (retrieval) — covered fully with examples in Chapter 12.

---

## Chapter 11: Simple Queries in SQL

**SQL Elements**

- **Literals**: fixed values (numbers, strings) written directly in a query.
- **Data types**: numeric (`INT`, `TINYINT`, `SMALLINT`, `MEDIUMINT`, `BIGINT`, `FLOAT`, `DECIMAL`), string (`CHAR`, `VARCHAR`), date/time (`DATE`, `TIME`, `DATETIME`).
- **NULL values**: represent "unknown"/absence of data — NOT the same as zero or an empty string.
- **Comments**: `-- single line` or `/* multi-line */`.

**CHAR vs VARCHAR**

- `CHAR(n)`: fixed-length, always reserves `n` characters (padded with spaces).
- `VARCHAR(n)`: variable-length, uses only as much storage as the actual data needs.

**FLOAT vs DECIMAL**

- `FLOAT`: approximate precision, fine for values where tiny rounding differences don't matter.
- `DECIMAL(m,d)`: exact precision — `m` total digits, `d` after the decimal point. Correct choice for money/marks.

**The SELECT command**

```sql
SELECT * FROM Student;                           -- all columns
SELECT Name, Marks FROM Student;                  -- specific columns
SELECT DISTINCT ClassID FROM Student;              -- removes duplicates
DESC Student;                                       -- shows table structure
```

**Scalar expressions and aliases**

```sql
SELECT Marks, Marks*5 AS Percentage FROM Student;   -- AS gives a column alias
```

**Handling NULLs**

```sql
SELECT * FROM Student WHERE Email IS NULL;
SELECT * FROM Student WHERE Email IS NOT NULL;
```

**Exam trap**: `= NULL` never works — always use `IS NULL` / `IS NOT NULL`.

**WHERE clause with operators**

- Relational: `= != > < >= <=`
- Logical: `AND OR NOT`
- Range: `BETWEEN ... AND ...`
- List: `IN (...)`
- Pattern: `LIKE` with `%` (any sequence of characters) and `_` (exactly one character)

```sql
SELECT * FROM Student WHERE Marks BETWEEN 60 AND 90;
SELECT * FROM Student WHERE ClassID IN (101, 102);
SELECT * FROM Student WHERE Name LIKE 'A%';          -- starts with A
SELECT * FROM Student WHERE Name LIKE '_d%';          -- 2nd letter is 'd'
```

---

## Chapter 12: Table Creation and Data Manipulation Commands

**Databases in MySQL**

```sql
CREATE DATABASE school;
USE school;              -- opening/selecting a database
DROP DATABASE school;    -- removing a database
```

**Creating tables with constraints**

```sql
CREATE TABLE Student (
    RollNo INT PRIMARY KEY,
    Name VARCHAR(30) NOT NULL,
    Email VARCHAR(50) UNIQUE,
    Marks INT CHECK (Marks >= 0 AND Marks <= 100),
    Status VARCHAR(10) DEFAULT 'Active',
    ClassID INT,
    FOREIGN KEY (ClassID) REFERENCES Class(ClassID)
);
```

| Constraint | Meaning |
| --- | --- |
| PRIMARY KEY | Unique identifier, no NULLs, one per table |
| NOT NULL | Value is mandatory |
| UNIQUE | No duplicate values, multiple allowed per table |
| DEFAULT | Pre-set value if none provided |
| CHECK | Restricts values to a condition |
| FOREIGN KEY | References another table's primary key |

**DML Commands**

```sql
INSERT INTO Student VALUES (1, 'Aditya', 'a@mail.com', 90, 'Active', 101);
UPDATE Student SET Marks = 95 WHERE RollNo = 1;
DELETE FROM Student WHERE RollNo = 1;
```

**More DDL — ALTER and DROP**

```sql
ALTER TABLE Student ADD Age INT;              -- add column
ALTER TABLE Student MODIFY Marks DECIMAL(5,2); -- change datatype
ALTER TABLE Student DROP COLUMN Age;           -- remove column
DROP TABLE Student;                             -- delete entire table
```

**Exam tip**: Know the difference between `DELETE` (removes specific rows, can target with `WHERE`, DML) and `DROP TABLE` (removes the entire table structure + data, DDL) — a very frequently confused pair.

---

## Chapter 13: Grouping Records, Joins in SQL

**Types of SQL functions**

- **Single-row functions**: operate on each row individually (string/math/date functions).
- **Aggregate (multi-row) functions**: operate on a set of rows, return one summary value — `SUM()`, `AVG()`, `COUNT()`, `MAX()`, `MIN()`.

**GROUP BY**

```sql
SELECT ClassID, AVG(Marks) AS AvgMarks
FROM Student
GROUP BY ClassID;
```

- **Grouping on multiple columns**: groups are formed based on unique combinations of all listed columns.

```sql
SELECT ClassID, Status, COUNT(*) 
FROM Student 
GROUP BY ClassID, Status;
```

**HAVING clause** — filters groups AFTER aggregation (unlike `WHERE`, which filters rows before grouping, and cannot use aggregate functions).

```sql
SELECT ClassID, AVG(Marks) AS AvgMarks
FROM Student
GROUP BY ClassID
HAVING AVG(Marks) > 75;
```

**Non-group expressions with GROUP BY** — generally, any column in the `SELECT` list that isn't inside an aggregate function must also appear in the `GROUP BY` clause, otherwise the result is ambiguous/invalid in strict SQL mode.

**Joins**

- **Cartesian Product**: combining every row of one table with every row of another (no condition).

```sql
SELECT * FROM Student, Class;
```

- **Table Aliases**: short names for tables, making queries more readable, especially in joins.

```sql
SELECT S.Name, C.ClassName
FROM Student S, Class C
WHERE S.ClassID = C.ClassID;
```

- **Equi Join**: joins tables based on an equality condition between matching columns.
- **Natural Join**: automatically joins tables based on columns with the same name — convenient but requires careful schema design to avoid unintended matches.

```sql
-- Equi Join
SELECT Student.Name, Class.ClassName
FROM Student, Class
WHERE Student.ClassID = Class.ClassID;
```

- **Additional search conditions in joins**: you can combine a join condition with further filtering using `AND`.

```sql
SELECT S.Name, C.ClassName
FROM Student S, Class C
WHERE S.ClassID = C.ClassID AND S.Marks > 80;
```

---

## Chapter 14: Interface Python with MySQL

**Why connect Python to MySQL?** Lets a Python program store, retrieve, and manipulate data in a real database instead of flat files — the basis of most real applications, including the board exam project.

**Steps for creating a database connectivity application**

1. Import the connector module.
2. Establish a connection to the MySQL server (host, user, password, database).
3. Create a cursor object to execute queries.
4. Execute SQL commands via the cursor.
5. Fetch results (for SELECT) or commit changes (for INSERT/UPDATE/DELETE).
6. Close the connection.

**Connecting using `pymysql`**

```python
import pymysql

conn = pymysql.connect(
    host="localhost",
    user="root",
    password="pass",
    database="school"
)
cursor = conn.cursor()

cursor.execute("SELECT * FROM Student WHERE Marks > 80")
for row in cursor.fetchall():
    print(row)

conn.close()
```

**Parameterised queries** — using placeholders instead of directly embedding values in the SQL string. This is both the exam-expected method AND the correct real-world practice (prevents SQL injection and handles data types/quoting correctly).

```python
cursor.execute("INSERT INTO Student VALUES (%s, %s, %s)", (2, 'Rahul', 85))
conn.commit()   # required to permanently save the change
```

**Performing Insert and Update queries**

```python
def add_student(conn):
    cursor = conn.cursor()
    roll = int(input("Roll No: "))
    name = input("Name: ")
    marks = int(input("Marks: "))
    cursor.execute("INSERT INTO Student VALUES (%s, %s, %s)", (roll, name, marks))
    conn.commit()
    print("Record added successfully.")

def update_marks(conn):
    cursor = conn.cursor()
    roll = int(input("Enter Roll No to update: "))
    new_marks = int(input("New Marks: "))
    cursor.execute("UPDATE Student SET Marks=%s WHERE RollNo=%s", (new_marks, roll))
    conn.commit()
    print("Record updated.")
```

**Exam trap**: Forgetting `conn.commit()` after INSERT/UPDATE/DELETE — the change executes in the session but is never permanently saved to the database without it.

**Project tie-in**: this chapter is what makes the Class 12 practical project possible — combine this with Chapter 6 (file handling) and Chapters 10-13 (SQL) to build a complete menu-driven, database-backed application for the practical exam.

---

