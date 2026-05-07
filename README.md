# B7A1 — Advanced Problem Solving with TypeScript & OOP

## Overview

This repository contains the completed assignment for **B7A1**, covering fundamental TypeScript concepts including data typing, interfaces, generics, class inheritance, union types, and utility types.

---

## File Structure

```
├── solutions.ts   — All 7 coding solutions
├── blog-1.md      — Blog: any vs unknown & type narrowing
├── blog-2.md      — Blog: Pick and Omit utility types (DRY principle)
└── README.md      — This file
```

---

## Problem Summaries

| # | Function / Class | Concept |
|---|---|---|
| 1 | `filterEvenNumbers` | Array filtering |
| 2 | `reverseString` | String manipulation |
| 3 | `checkType` | Union types & type guards |
| 4 | `getProperty` | Generics with key constraints |
| 5 | `toggleReadStatus` | Interfaces & object spreading |
| 6 | `Person` / `Student` | Class inheritance |
| 7 | `getIntersection` | Array intersection with Set |

---

## Blog Topics Covered

1. **`any` vs `unknown` and Type Narrowing** — Why `any` creates type safety holes and how `unknown` with narrowing guards is the correct approach for unpredictable data.

2. **`Pick` and `Omit` Utility Types** — How these utilities derive specialized interface slices from a master type, keeping code DRY and reducing maintenance overhead.

---

## How to Run

```bash
# Install TypeScript if needed
npm install -g typescript

# Compile
tsc solutions.ts

# Run
node solutions.js
```
