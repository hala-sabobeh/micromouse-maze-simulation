# 🐭 Micromouse Maze Simulation

**Course:** Interfacing Techniques — ENCS4380  
**Institution:** Birzeit University — Electrical and Computer Engineering Department

---

## 📌 Project Overview

This repository contains the simulation phase of the Micromouse project. Three navigation algorithms were implemented and tested in the **mms simulator** on both **8×8** and **16×16** mazes. The goal is to compare algorithm performance in terms of steps to center and unique cells visited.

---

## 📁 Repository Structure

```
maze_simulation/
│
├── Micromouse_Simulation_Report_.pdf     ← Full simulation report
│
├── 8x8_maze_test1.txt                    ← 8×8 maze layout (test 1)
├── 8x8_maze_test2.txt                    ← 8×8 maze layout (test 2)
│
├── Floodfill/
│   ├── API.py                            ← mms Python API wrapper
│   ├── floodfill.py                      ← Standard Floodfill algorithm
│   ├── optimized_floodfill.py            ← Optimized Floodfill (3-phase)
│   ├── floodfill 8x8.mp4                 ← Demo: Standard Floodfill on 8×8
│   ├── floodfill 16x16.mp4              ← Demo: Standard Floodfill on 16×16
│   ├── optimized 8x8.mp4                ← Demo: Optimized Floodfill on 8×8
│   └── optimized 16x16.mp4             ← Demo: Optimized Floodfill on 16×16
│
├── LeftHand/
│   ├── API.h                             ← mms C++ API header
│   ├── API.cpp                           ← mms C++ API implementation
│   ├── Main.cpp                          ← Left-hand wall following algorithm
│   ├── wall-following left 8x8.mp4      ← Demo: Left-hand on 8×8
│   └── wall-following left 16x16.mp4   ← Demo: Left-hand on 16×16
│
└── RightHand/
    ├── API.h
    ├── API.cpp
    ├── Main.cpp                          ← Right-hand wall following algorithm
    ├── wall-following right 8x8.mp4     ← Demo: Right-hand on 8×8
    └── wall-following right 16x16.mp4  ← Demo: Right-hand on 16×16
```

---

## 🧠 Algorithms

### 1. Wall-Following — Left & Right Hand Rule (C++)

The simplest navigation strategy. The mouse keeps one hand touching a wall and follows it around the maze.

- ✅ Works on simple/small mazes (8×8)
- ❌ Fails on 16×16 mazes with isolated wall structures — if the start and center are not connected to the same continuous wall, the mouse loops forever (DNF)
- The **Left-hand** and **Right-hand** versions are mirror implementations of the same principle

---

### 2. Standard Floodfill (Python)

Assigns a distance value to every cell based on its distance to the center. The mouse always moves to the neighbor with the **lowest flood value**.

- When a new wall is discovered mid-run, all flood values are **recalculated using BFS**
- ✅ Successfully solves all maze sizes (8×8 and 16×16)
- Guarantees the shortest **currently known** path at every step

---

### 3. Optimized Floodfill — 3-Phase (Python)

Extends Standard Floodfill with three distinct phases:

| Phase | Description |
|-------|-------------|
| **Phase 1 — Explore** | Navigate to center using Floodfill, mapping all discovered walls |
| **Phase 2 — Return** | Navigate back to start (0,0), continuing to map new walls |
| **Phase 3 — Speed Run** | Navigate to center on the **fully known optimal path** — no recalculation needed |

- ✅ Phase 3 speed run can be up to **79% faster** than Standard Floodfill on complex mazes
- This is the strategy used by real micromouse competition robots

---

## ⚙️ Setup & Running

### Requirements

- [mms simulator](https://github.com/mackorone/mms) (Windows) — **not included**, download separately
- Python 3.x (for Floodfill algorithms)
- A C++ compiler (for Wall-Following) — e.g. MinGW, MSVC, or g++

---

### ▶️ Running Floodfill (Python)

1. Open the **mms simulator**
2. Click **"+"** next to the Mouse dropdown
3. Set the following:
   - **Directory:** path to your `Floodfill/` folder
   - **Run Command:** `python floodfill.py` or `python optimized_floodfill.py`
4. Click **Build**, then **Run**

---

### ▶️ Running Wall-Following (C++)

1. Compile using:
   ```bash
   g++ -o myMouse Main.cpp API.cpp
   ```
2. Open the **mms simulator**
3. Click **"+"** next to the Mouse dropdown
4. Set the following:
   - **Directory:** path to `LeftHand/` or `RightHand/`
   - **Run Command:** `.\myMouse.exe`
5. Click **Run**

---

### 🗺️ Loading 8×8 Mazes

1. In the simulator, click the **folder icon** next to the Maze dropdown
2. Navigate to and select `8x8_maze_test1.txt` or `8x8_maze_test2.txt`

> For 16×16 mazes, the simulator includes built-in example mazes — no file needed.

---

## 📊 Results Summary

| Algorithm | Maze | Size | Steps to Center | Unique Cells Visited |
|-----------|------|------|-----------------|----------------------|
| Wall-Following Left | Examples 1–3 | 16×16 | DNF  | — |
| Wall-Following Left | Test 1 | 8×8 | 20 | 12 |
| Wall-Following Right | Test 1 | 8×8 | 18 | 11 |
| Standard Floodfill | Example 1 | 16×16 | 276 | 118 |
| Standard Floodfill | Example 4 | 16×16 | 576 | 199 |
| Optimized Floodfill | Example 4 | 16×16 | 120 *(speed run)* | 143 |

> 📄 For full detailed results and analysis, see **Micromouse_Simulation_Report_.pdf**

---

## 📹 Demo Videos

All demo recordings (`.mp4`) are included inside each algorithm's folder. They show the mouse navigating in the mms simulator in real time.

| Video | Location |
|-------|----------|
| Standard Floodfill — 8×8 | `Floodfill/floodfill 8x8.mp4` |
| Standard Floodfill — 16×16 | `Floodfill/floodfill 16x16.mp4` |
| Optimized Floodfill — 8×8 | `Floodfill/optimized 8x8.mp4` |
| Optimized Floodfill — 16×16 | `Floodfill/optimized 16x16.mp4` |
| Left-Hand Wall Following — 8×8 | `LeftHand/wall-following left 8x8.mp4` |
| Left-Hand Wall Following — 16×16 | `LeftHand/wall-following left 16x16.mp4` |
| Right-Hand Wall Following — 8×8 | `RightHand/wall-following right 8x8.mp4` |
| Right-Hand Wall Following — 16×16 | `RightHand/wall-following right 16x16.mp4` |

---

## 📝 Notes

- The **mms simulator** executable is **not included**. Download it from [github.com/mackorone/mms](https://github.com/mackorone/mms)
- The `API.py`, `API.cpp`, and `API.h` files are the standard mms communication wrappers — do not modify them
- The mouse always starts at cell **(0, 0)** facing **North**
- Center cells: **(7,7), (7,8), (8,7), (8,8)** for 16×16 | **(3,3), (3,4), (4,3), (4,4)** for 8×8
