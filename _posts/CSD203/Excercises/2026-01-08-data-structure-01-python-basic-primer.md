---
title: Python Primer – Parking System Programming Assignments
description: >-
  Practice for Python Primer such as Parking System Programming Assignments
author: [shandy]
date: 2025-09-05
categories: [(Python) Data Structure and Algorithms, Exercises]
tags: [(Python) Data Structure and Algorithms - Exercises]
sort_index: 401
# pin: true
# media_subpath: '/posts/01'
---

# Parking System Programming Assignments

The Parking Assignment System is a menu-driven Python program designed to help students practice core programming concepts through a realistic parking management scenario.

The system consists of five independent but related modules (Q1–Q5).
Each module focuses on a specific level of difficulty and programming skills, ranging from basic data processing to full system simulation.

Students can select any module from the menu and execute it independently.

**Main Menu:**

```text
===== PARKING ASSIGNMENT MENU =====
1. Q1 – Parking Fee Calculator
2. Q2 – Parking Slot Management
3. Q3 – Parking Violation Detection
4. Q4 – Smart Parking Statistics
5. Q5 – Full Parking System (Capstone)
6. Exit
==================================
```

**Code templates:**

```python
# =========================================
# PARKING SYSTEM – ALL 5 QUESTIONS IN ONE
# =========================================

# ---------- VARIABLES ----------


# =========================================
# Q1 – Parking Fee Calculator
# =========================================
def question_1():
  print()

# =========================================
# Q2 – Parking Slot Management
# =========================================
def question_2():
  print()

# =========================================
# Q3 – Parking Violation Detection
# =========================================
def question_3():
  print()

# =========================================
# Q4 – Smart Parking Statistics
# =========================================
def question_4():
  print()

# =========================================
# Q5 – Full Parking System (Menu-based)
# =========================================
def question_5():
    print()

# =========================================
# MAIN MENU
# =========================================
def main():
    while True:
        print("\n===== PARKING ASSIGNMENT MENU =====")
        print("1. Q1 – Parking Fee Calculator")
        print("2. Q2 – Parking Slot Management")
        print("3. Q3 – Parking Violation Detection")
        print("4. Q4 – Smart Parking Statistics")
        print("5. Q5 – Full Parking System")
        print("0. Exit")
        print("==================================")

        choice = input("Choose an option: ")

        if choice == "1":
            question_1()
        elif choice == "2":
            question_2()
        elif choice == "3":
            question_3()
        elif choice == "4":
            question_4()
        elif choice == "5":
            question_5()
        elif choice == "0":
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Try again.")


# ---------- RUN PROGRAM ----------
main()
```


## 1. Parking Fee Calculator

**Problem Description**

You are asked to build a program that calculates parking fees for multiple vehicles parked in a parking lot during a single day.
Each vehicle has a type and a parking duration (in hours). The system must calculate the correct fee based on predefined pricing rules.

This module calculates parking fees for a list of vehicles parked during a single day.
- Uses basic data structures such as tuples and dictionaries
- Applies arithmetic expressions and conditional logic
- Ignores invalid parking durations
- Produces total revenue and vehicle count by type

**Input:** A list of tuples, where each tuple represents one vehicle:

```python
(vehicle_id: str, vehicle_type: str, hours_parked: float)
```

**Example:**

```python
vehicles = [
    ("A01", "bike", 2.5),
    ("B02", "car", 12),
    ("C03", "truck", 15),
    ("D04", "car", -1)
]
```

**Pricing Rules**

| Vehicle Type | Price per Hour | Maximum Fee |
| ------------ | -------------- | ----------- |
| bike         | 5,000          | 30,000      |
| car          | 10,000         | 100,000     |
| truck        | 20,000         | 200,000     |

**Output**

- A dictionary showing the parking fee for each valid vehicle
- The total revenue
- The number of vehicles by type

**Rules**

- Vehicles with hours_parked <= 0 must be ignored using continue
- The fee cannot exceed the maximum fee
- Use a dict to store pricing information

**Example Output**

```python
{
    "A01": 12500,
    "B02": 100000,
    "C03": 200000
}

Total revenue: 312500
Vehicle count: {"bike": 1, "car": 1, "truck": 1}
```

## 2. Parking Slot Management System

**Problem Description**

Simulate a parking lot entry–exit management system.
The parking lot has a limited number of slots. Vehicles can enter or leave based on events.

This module simulates vehicle entry and exit in a parking lot with limited capacity.
- Manages available slots using a set
- Processes a sequence of IN/OUT events
- Prevents overcapacity and invalid exits
- Terminates processing when a STOP event is encountered

**Input**

- An integer capacity – total parking slots
- A list of parking events represented as tuples:

```python
(event_type: str, vehicle_id: str)
```

Example:

```python
capacity = 2
events = [
    ("IN", "A01"),
    ("IN", "B02"),
    ("IN", "C03"),
    ("OUT", "A01"),
    ("IN", "C03"),
    ("STOP", "")
]
```

**Output** After each event, print:
- Vehicles currently parked
- Number of available slots

**Rules**
- Use a set to store vehicles currently parked
- If the parking lot is full, reject new vehicles
- If a vehicle tries to leave but is not present, ignore it
- Stop processing events when "STOP" is encountered

**Example Output:**

```python
IN A01 → Parked: {'A01'}, Available slots: 1
IN B02 → Parked: {'A01', 'B02'}, Available slots: 0
IN C03 → Parking full
OUT A01 → Parked: {'B02'}, Available slots: 1
IN C03 → Parked: {'B02', 'C03'}, Available slots: 0
```

## 3. Parking Violation Detection System

**Problem Description**

Your task is to detect parking violations based on incorrect payment or illegal parking duration.

This module detects parking violations based on incorrect payment or illegal parking behavior.
- Validates vehicle types and parking duration
- Detects underpayment and overstaying
- Records violations in a dictionary
- Calculates total unpaid revenue

**Input:** A dictionary where:

```python
vehicle_id: (vehicle_type, hours_parked, amount_paid)
```

Example:
```python
records = {
    "A01": ("car", 5, 30000),
    "B02": ("bike", 30, 10000),
    "C03": ("truck", 10, 150000),
    "D04": ("bus", 3, 50000)
}
```

**Output**

- A dictionary of violating vehicles and reasons
- Total unpaid amount

**Violation Conditions**: A vehicle is considered in violation if:
- Paid amount < required parking fee
- Parking duration > 24 hours
- Vehicle type is invalid

**Example Output**

```python 
{
    "B02": "Exceeded maximum parking duration",
    "D04": "Invalid vehicle type"
}

Total unpaid amount: 20000
```

## 4. Smart Parking Statistics Analyzer (Hard)

**Problem Description**

Analyze parking data to produce usage statistics.

This module analyzes historical parking records to generate useful statistics.
- Computes average parking duration by vehicle type
- Identifies the longest parked vehicle
- Counts unique vehicles using a set
- Ignores invalid or inconsistent records

**Input:** A list of records: 

```python
(vehicle_id, vehicle_type, hour_in, hour_out)
```

**Example:**

```python
data = [
    ("A01", "car", 8, 12),
    ("B02", "bike", 9, 10),
    ("C03", "car", 7, 18),
    ("D04", "truck", 14, 13)
]
```

**Output:**

- Average parking duration per vehicle type
- Vehicle with the longest parking duration
- Number of unique vehicles

**Rules**
- Ignore invalid records where hour_out < hour_in
- Use a set to count unique vehicles
- Do not use external libraries

**Example Output** 

```text
Average duration:
car: 7.5 hours
bike: 1.0 hours

Longest parking vehicle: C03 (11 hours)
Unique vehicles: 3
```

## 5. Full Parking System Simulation

**Problem Description**

Build a complete interactive parking system simulation.

This is the most advanced module, integrating all previous concepts into a complete parking system.
- Provides an interactive menu-based interface
- Supports real-time vehicle check-in and check-out
- Tracks parking status, revenue, and history
- Applies peak-hour pricing rules
- Produces a final summary report on exit

**Rules:**

| Item                     | Value                        |
| ------------------------ | ---------------------------- |
| Maximum parking slots    | 50                           |
| Supported vehicle types  | bike, car, truck             |
| Parking fee rule         | fee = hours × price_per_hour |
| Peak hours surcharge     | +20% (7–9, 17–19)            |
| Data structures required | set, dict, tuple             |


Input/Output follow structure:

| Option | Description         |
| ------ | ------------------- |
| 1      | Vehicle check-in    |
| 2      | Vehicle check-out   |
| 3      | Show parking status |
| 4      | Show revenue report |
| 0      | Exit system         |

### Option 1 – Vehicle Check-in

**Input**

| Input Field  | Type   | Description                      |
| ------------ | ------ | -------------------------------- |
| vehicle_id   | string | Unique identifier of the vehicle |
| vehicle_type | string | One of: `bike`, `car`, `truck`   |
| hour_in      | float  | Entry time (0–24)                |

**Rules**

| Condition              | Action          |
| ---------------------- | --------------- |
| Parking is full        | Reject check-in |
| vehicle_type invalid   | Reject check-in |
| vehicle already parked | Reject check-in |
| hour_in not in 0–24    | Reject check-in |


**Output**

| Result  | Message                             |
| ------- | ----------------------------------- |
| Success | `Vehicle <id> parked successfully.` |
| Failure | Error message explaining the reason |

### Option 2 – Vehicle Check-out

**Input**

| Input Field | Type   | Description               |
| ----------- | ------ | ------------------------- |
| vehicle_id  | string | Vehicle to be checked out |
| hour_out    | float  | Exit time (0–24)          |

Rules:

| Condition          | Action           |
| ------------------ | ---------------- |
| vehicle not found  | Reject check-out |
| hour_out ≤ hour_in | Reject check-out |

**Fee Calculation**

| Step                | Formula                        |
| ------------------- | ------------------------------ |
| Parking duration    | `hour_out − hour_in`           |
| Base fee            | `duration × price_per_hour`    |
| Peak hour surcharge | `fee × 1.2` if hour_in in peak |

**Output**

| Result  | Message                             |
| ------- | ----------------------------------- |
| Success | `Parking fee: <amount> VND`         |
| Failure | Error message explaining the reason |

### Option 3 – Show Parking Status

**Output**

| Field                 | Description                |
| --------------------- | -------------------------- |
| Total parked vehicles | Current number of vehicles |
| Vehicle list          | vehicle_id, type, hour_in  |
| Available slots       | Remaining capacity         |

**Example**

```python
Vehicles currently parked: 2
A01 (car), entered at 8.0
B02 (bike), entered at 9.5
Available slots: 48
```

## Option 4 – Show Revenue Report

**Output**

| Field                  | Description               |
| ---------------------- | ------------------------- |
| Total revenue          | Sum of all collected fees |
| Vehicles by type       | Count per vehicle type    |
| Longest parked vehicle | vehicle_id and duration   |

**Example Output**

```python
Total revenue: 360000
car: 3
bike: 2
truck: 1
Longest parked vehicle: C03 (12.0 hours)
```

### Option 0 – Exit System

**Output**

| Action       | Result                     |
| ------------ | -------------------------- |
| Exit program | Print final revenue report |
| Program ends | No further input accepted  |