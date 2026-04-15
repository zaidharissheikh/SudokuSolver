# Sudoku CSP Solver

A highly efficient, Python-based Sudoku solver built using Constraint Satisfaction Problem (CSP) techniques. This solver is capable of cracking Sudoku puzzles of any difficulty—from easy boards to "Very Hard" variants designed to thwart basic logic by aggressively pruning the search space.

## 🚀 Features

This solver implements several advanced AI and CSP techniques to ensure fast and mathematically sound puzzle resolution:

* **AC-3 Algorithm (Arc Consistency):** Performs initial constraint propagation to drastically reduce the domain of possible values for each cell before any guessing occurs. For easy boards, this solves the puzzle instantly.
* **Backtracking Search:** A depth-first search algorithm used to explore possible assignments when logic alone isn't enough.
* **Minimum Remaining Values (MRV) Heuristic:** When the solver must guess, it selects the cell with the fewest possible remaining valid options. This minimizes the chance of taking a wrong path.
* **Maintaining Arc Consistency:** Uses a recursive implementation of Forward Checking. The moment a cell's domain is passively reduced to a single value, the solver immediately propagates that constraint to its neighbors, catching dead ends instantly.

## 🧠 How It Works

1.  **Initialization:** The board is parsed, and every empty cell is assigned a domain of `{1-9}`. Known cells are assigned a domain of their given value. Neighbors (row, column, and 3x3 subgrid) are mapped for every cell.
2.  **Constraint Propagation:** The `ac3()` method runs first. It iteratively removes values from cell domains if they violate constraints with their neighbors. 
3.  **Search:** If AC-3 cannot fully solve the board, `backtrack()` is called.
4.  **Guess & Propagate:** The solver picks the safest cell (MRV), assigns a value, and runs `forward_check()`. If the forward check results in any neighbor's domain shrinking to `0`, the solver instantly undoes the guess and tries the next number.

## 📊 Performance Benchmarks

The integration of recursive forward checking and MRV allows the solver to handle exponentially difficult boards with minimal wasted effort. 

| Difficulty | Backtrack Calls | Failures (Dead Ends) | Notes |
| :--- | :--- | :--- | :--- |
| **Easy** | 1 | 0 | Solved entirely by initial AC-3 pass. |
| **Medium** | 2 | 0 | MRV makes a perfect guess; instantly collapses the board. |
| **Hard** | 7 | 2 | Search required, but bad paths are pruned almost immediately. |
| **Very Hard** | 56 | 43 | Highly aggressive pruning prevents thousands of standard search calls. |

*Note: A failure indicates the solver correctly identified and aborted an invalid search branch early.*

## 💻 Usage

To use the solver, pass a 9x9 2D list to the `SudokuCSP` class. Use `0` to represent empty cells.
