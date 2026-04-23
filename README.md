# 🚗 Car Wash Simulation — CS241 Assignment 2

> **Cairo University — Faculty of Computers and Artificial Intelligence**
> **CS241: Operating Systems – 1**

![Build](https://github.com/YOUR_USERNAME/CarWashSimulator/actions/workflows/build.yml/badge.svg)
![Java](https://img.shields.io/badge/Java-17%2B-orange?logo=java)
![License](https://img.shields.io/badge/license-MIT-blue)

---

## 📖 Overview

A multithreaded **Car Wash & Gas Station Simulator** built in Java, implementing the classic **Producer-Consumer Problem** using the **Bounded Buffer** pattern.

The simulation models a service station with:
- A **fixed-size waiting area** (bounded buffer / queue)
- A configurable number of **service bays (pumps)** running concurrently
- Full **synchronization** using custom semaphores and a mutex to prevent race conditions
- A **Swing-based GUI** showing live pump status, the waiting queue, and an activity log

---

## 🧠 Concepts Demonstrated

| Concept | Implementation |
|---|---|
| Producer-Consumer Problem | `Car` (producer) → Queue → `Pump` (consumer) |
| Bounded Buffer | `LinkedList` queue with fixed capacity |
| Semaphores | Custom `Semaphore` class with `P()` / `V()` |
| Mutex | `synchronized (mutex)` block for queue access |
| Multithreading | Each car and each pump runs as its own `Thread` |
| GUI | Java Swing (`JFrame`, `JLabel`, `JTextArea`) |

---

## 🏗️ Project Structure

```
CarWashSimulator/
├── src/
│   └── ServiceStation.java      # All classes in one file (as per submission rules)
├── out/                         # Compiled .class files (auto-generated, git-ignored)
├── .github/
│   └── workflows/
│       └── build.yml            # GitHub Actions: auto-compile on push
├── .gitignore
└── README.md
```

---

## 🔧 Classes

### `ServiceStation` *(Main)*
- Reads user input: waiting area size, number of pumps, car arrival order
- Initializes shared resources: queue, semaphores (`empty`, `full`, `pumps`), mutex
- Spawns `Pump` consumer threads and `Car` producer threads
- Hosts helper methods: `addToQueue()`, `removeFromQueue()`, `incrementProcessedCars()`

### `Semaphore`
- Custom implementation (as required by the lab spec)
- `P()` — wait/decrement; blocks the thread if value < 0
- `V()` — signal/increment; wakes up a waiting thread if value ≤ 0

### `Car` *(Producer)*
- Extends `Thread`
- Calls `empty.P()` → waits if queue is full
- Adds itself to the shared queue
- Calls `full.V()` → signals a pump that a car is ready

### `Pump` *(Consumer)*
- Extends `Thread`
- Calls `full.P()` → waits if no cars in queue
- Removes a car from the queue
- Acquires a service bay, simulates service (random 3–5s), releases the bay
- Calls `empty.V()` → signals that a queue slot is free

### `CarWashGUI`
- Java Swing GUI dashboard
- **Service Bays panel** (top): green = Free, orange = Occupied, yellow = Servicing
- **Waiting Area panel** (center): shows which cars are currently queued
- **Activity Log** (bottom): scrollable real-time log of all events

---

## ▶️ How to Run

### Prerequisites
- Java 17+ installed ([Download JDK](https://adoptium.net/))

### Compile
```bash
mkdir -p out
javac -d out src/ServiceStation.java
```

### Run
```bash
java -cp out ServiceStation
```

### Sample Input
```
Enter garage waiting area size (1-10): 5
Enter number of pumps/service bays (at least 1): 3
Enter cars arriving (order, comma-separated): C1,C2,C3,C4,C5
```

---

## 📋 Sample Output

```
C1 arrived
C2 arrived
C3 arrived
C4 arrived
Pump 1: C1 Occupied
Pump 2: C2 Occupied
Pump 3: C3 Occupied
C4 arrived and waiting
C5 arrived
C5 arrived and waiting
Pump 1: C1 login
Pump 1: C1 begins service at Bay 1
Pump 2: C2 login
Pump 2: C2 begins service at Bay 2
Pump 3: C3 login
Pump 3: C3 begins service at Bay 3
Pump 1: C1 finishes service
Pump 1: Bay 1 is now free
Pump 2: C2 finishes service
Pump 2: Bay 2 is now free
Pump 1: C4 login
Pump 1: C4 begins service at Bay 1
Pump 3: C3 finishes service
Pump 3: Bay 3 is now free
Pump 2: C5 login
Pump 2: C5 begins service at Bay 2
Pump 1: C4 finishes service
Pump 1: Bay 1 is now free
Pump 2: C5 finishes service
Pump 2: Bay 2 is now free
All cars processed; simulation ends
```

---

## 🖥️ GUI Preview

The GUI shows three live panels:

- 🟢 **Green** pump label → Bay is **free**
- 🟠 **Orange** pump label → Car just **arrived/occupied** the bay
- 🟡 **Yellow** pump label → Car is actively being **serviced**
- 🔵 **Blue** queue slot → Slot is **occupied** by a waiting car
- ⬜ **Grey** queue slot → Slot is **empty**

---

## 👥 Team

| Name | ID |
|---|---|
| Haneen Hisham | 20236023 |
| Shaza Moatasem | 20236050 |
| Ziad Tarek | 20236043 |

> Cairo University — Faculty of Computers and Artificial Intelligence
> CS241: Operating Systems – 1, 2024

---

## 📜 License

This project is submitted for academic purposes. Feel free to use it as a reference for learning OS synchronization concepts.
