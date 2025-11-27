# Memory Allocation Strategies

This document covers three standard contiguous memory allocation strategies:

- **First Fit**
- **Best Fit**
- **Worst Fit**

Each section explains the **Theory**, **Algorithm**, and includes **All Codes in ONE single code block** at the end with expected outputs.

---

# 1. First Fit Allocation

## 📘 Theory
First Fit allocates the **first available block** that is large enough to serve the process.  
It is fast because it stops searching once a suitable block is found.  
However, it causes **external fragmentation** near the beginning of memory.

---

## 📐 Algorithm
1. Input memory blocks & process sizes  
2. Initialize allocation array to -1  
3. For each process:
   - Scan blocks from the start  
   - If block ≥ process size → allocate  
   - Reduce block size  
4. Print allocation results  

---

# 2. Best Fit Allocation

## 📘 Theory
Best Fit searches for the **smallest block** that is large enough for the process.  
This minimizes leftover space but creates many tiny unusable holes.

---

## 📐 Algorithm
1. For each process:
   - Search all blocks  
   - Choose block with **minimum sufficient space**  
   - Allocate and reduce size  
2. Print results  

---

# 3. Worst Fit Allocation

## 📘 Theory
Worst Fit allocates the **largest available block**.  
Idea: leave a reasonably large leftover block after allocation.  
However, it quickly breaks large blocks into unusable small pieces.

---

## 📐 Algorithm
1. For each process:
   - Search all blocks  
   - Select block with **maximum size**  
   - Allocate and reduce its size  
2. Print results  

---

# 🧾 FULL COMBINED CODE (FIRST FIT + BEST FIT + WORST FIT) + EXPECTED OUTPUT  
(All in ONE SINGLE CODE BLOCK)

```cpp
#include <iostream>
#include <vector>
using namespace std;

// -----------------------------
// FIRST FIT
// -----------------------------
void firstFit(vector<int> blocks, vector<int> processes) {
    int m = blocks.size(), n = processes.size();
    vector<int> allocation(n, -1);

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (blocks[j] >= processes[i]) {
                allocation[i] = j;
                blocks[j] -= processes[i];
                break;
            }
        }
    }

    cout << "\nFirst Fit Allocation:\n";
    cout << "Process No.\tProcess Size\tBlock No.\n";
    for (int i = 0; i < n; i++) {
        cout << " " << i+1 << "\t\t" << processes[i] << "\t\t";
        if (allocation[i] != -1) cout << allocation[i] + 1;
        else cout << "Not Allocated";
        cout << endl;
    }
}

// -----------------------------
// BEST FIT
// -----------------------------
void bestFit(vector<int> blocks, vector<int> processes) {
    int m = blocks.size(), n = processes.size();
    vector<int> allocation(n, -1);

    for (int i = 0; i < n; i++) {
        int bestIdx = -1;
        for (int j = 0; j < m; j++) {
            if (blocks[j] >= processes[i]) {
                if (bestIdx == -1 || blocks[j] < blocks[bestIdx])
                    bestIdx = j;
            }
        }

        if (bestIdx != -1) {
            allocation[i] = bestIdx;
            blocks[bestIdx] -= processes[i];
        }
    }

    cout << "\nBest Fit Allocation:\n";
    cout << "Process No.\tProcess Size\tBlock No.\n";
    for (int i = 0; i < n; i++) {
        cout << " " << i+1 << "\t\t" << processes[i] << "\t\t";
        if (allocation[i] != -1) cout << allocation[i] + 1;
        else cout << "Not Allocated";
        cout << endl;
    }
}

// -----------------------------
// WORST FIT
// -----------------------------
void worstFit(vector<int> blocks, vector<int> processes) {
    int m = blocks.size(), n = processes.size();
    vector<int> allocation(n, -1);

    for (int i = 0; i < n; i++) {
        int worstIdx = -1;
        for (int j = 0; j < m; j++) {
            if (blocks[j] >= processes[i]) {
                if (worstIdx == -1 || blocks[j] > blocks[worstIdx])
                    worstIdx = j;
            }
        }

        if (worstIdx != -1) {
            allocation[i] = worstIdx;
            blocks[worstIdx] -= processes[i];
        }
    }

    cout << "\nWorst Fit Allocation:\n";
    cout << "Process No.\tProcess Size\tBlock No.\n";
    for (int i = 0; i < n; i++) {
        cout << " " << i+1 << "\t\t" << processes[i] << "\t\t";
        if (allocation[i] != -1) cout << allocation[i] + 1;
        else cout << "Not Allocated";
        cout << endl;
    }
}

// -----------------------------
// MAIN PROGRAM
// -----------------------------
int main() {
    vector<int> blocks = {100, 500, 200, 300, 600};
    vector<int> processes = {212, 417, 112, 426};

    firstFit(blocks, processes);
    bestFit(blocks, processes);
    worstFit(blocks, processes);

    return 0;
}

/*
EXPECTED OUTPUT:

First Fit Allocation:
Process No.     Process Size    Block No.
 1              212             2
 2              417             5
 3              112             2
 4              426             Not Allocated

Best Fit Allocation:
Process No.     Process Size    Block No.
 1              212             4
 2              417             2
 3              112             3
 4              426             5

Worst Fit Allocation:
Process No.     Process Size    Block No.
 1              212             5
 2              417             2
 3              112             5
 4              426             Not Allocated
*/
