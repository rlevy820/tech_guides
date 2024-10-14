
# Python Basics Cheat Sheet

## Python Basics

### 1. Variables and Data Types
```python
# Variable assignment
x = 10
y = "Hello, World!"
z = True

# Data types
a = 5       # int
b = 5.0     # float
c = "text"  # str
d = [1, 2, 3]  # list
e = (1, 2, 3)  # tuple
f = {"key": "value"}  # dict
```

### 2. Control Flow
```python
# If-else statement
x = 10
if x > 5:
    print("x is greater than 5")
else:
    print("x is less than or equal to 5")

# For loop
for i in range(5):
    print(i)

# While loop
i = 0
while i < 5:
    print(i)
    i += 1
```

### 3. Functions
```python
# Function definition
def greet(name):
    return f"Hello, {name}!"

print(greet("Ryan"))
```

### 4. Lists and Dictionaries
```python
# Lists
my_list = [1, 2, 3, 4]
my_list.append(5)
print(my_list)

# Dictionaries
my_dict = {"name": "Alice", "age": 25}
print(my_dict["name"])
```

### 5. File I/O
```python
# Reading a file
with open("file.txt", "r") as file:
    content = file.read()
    print(content)

# Writing to a file
with open("file.txt", "w") as file:
    file.write("Hello, file!")
```

---

## NumPy Basics

### 1. Importing NumPy
```python
import numpy as np
```

### 2. Creating Arrays
```python
# Create an array
arr = np.array([1, 2, 3, 4, 5])

# Create arrays of zeros or ones
zeros = np.zeros((2, 3))
ones = np.ones((3, 3))
```

### 3. Array Operations
```python
# Basic operations
arr = np.array([1, 2, 3])
arr2 = np.array([4, 5, 6])
print(arr + arr2)  # [5 7 9]
print(arr * 2)     # [2 4 6]

# Element-wise operations
print(np.sqrt(arr))  # Square root
```

### 4. Slicing and Indexing
```python
arr = np.array([1, 2, 3, 4, 5])
print(arr[1:3])  # [2, 3]
print(arr[-1])   # 5
```

---

## Pandas Basics

### 1. Importing Pandas
```python
import pandas as pd
```

### 2. Creating DataFrames
```python
# Create a DataFrame from a dictionary
data = {
    "Name": ["Alice", "Bob", "Charlie"],
    "Age": [25, 30, 35]
}
df = pd.DataFrame(data)
print(df)
```

### 3. Reading and Writing Data
```python
# Reading a CSV file
df = pd.read_csv("data.csv")

# Writing to a CSV file
df.to_csv("output.csv", index=False)
```

### 4. DataFrame Operations
```python
# Access columns
print(df["Name"])

# Filter rows
print(df[df["Age"] > 25])

# Describe the DataFrame
print(df.describe())
```

### 5. Handling Missing Data
```python
# Fill missing values
df.fillna(0, inplace=True)

# Drop missing values
df.dropna(inplace=True)
```

---
