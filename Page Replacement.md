# Page Replacement Algorithms

This document covers three major page replacement policies:

- FIFO (First-In, First-Out)  
- LRU (Least Recently Used)  
- Optimal Page Replacement  

Each includes **Theory, Algorithm, Code, and Expected Output**, all combined into **ONE single code block** at the end.

---

## 1. FIFO Page Replacement

### 📘 Theory
FIFO replaces the **oldest** loaded page (the page that entered first).  
It is simple but suffers from **Belady’s Anomaly**.

### 📐 Algorithm
1. Input frames & page reference string  
2. Use queue to track insertion order  
3. If page fault:
   - If space → insert  
   - Else → remove front and insert new page  
4. Count total faults  

---

## 2. LRU Page Replacement

### 📘 Theory
LRU replaces the page that was **least recently used**.  
More efficient than FIFO and avoids Belady’s Anomaly.

### 📐 Algorithm
1. Use vector/list to store frames  
2. On page access:
   - If present → move to back  
   - If not:
     - Page fault  
     - If full → remove front  
     - Insert new page  
3. Count total faults  

---

## 3. Optimal Page Replacement

### 📘 Theory
Optimal replaces the page that will be used **farthest in the future**.  
Produces **minimum** possible faults → ideal for comparison, not implementable in real OS.

### 📐 Algorithm
1. For each page:
   - If hit → continue  
   - Else fault  
     - If space → insert  
     - Else replace page with **farthest future use**  
2. Count total faults  

---

# 🧾 FULL COMBINED CODE (FIFO + LRU + OPTIMAL) + EXPECTED OUTPUT  
(All in ONE code block)

```cpp
// -----------------------------
// FIFO PAGE REPLACEMENT
// -----------------------------
#include <iostream>
#include <vector>
#include <unordered_set>
#include <queue>
#include <algorithm>
using namespace std;

int fifoPageReplacement(int capacity, vector<int> pages) {
    unordered_set<int> s;
    queue<int> q;
    int page_faults = 0;

    for (int page : pages) {
        if (s.find(page) == s.end()) {
            page_faults++;
            if (s.size() < capacity) {
                s.insert(page);
                q.push(page);
            } else {
                int old = q.front(); q.pop();
                s.erase(old);
                s.insert(page);
                q.push(page);
            }
        }
    }
    return page_faults;
}

// -----------------------------
// LRU PAGE REPLACEMENT
// -----------------------------
int lruPageReplacement(int capacity, vector<int> pages) {
    vector<int> frames;
    int page_faults = 0;

    for (int page : pages) {
        auto it = find(frames.begin(), frames.end(), page);

        if (it == frames.end()) {
            page_faults++;
            if (frames.size() == capacity)
                frames.erase(frames.begin());
            frames.push_back(page);
        } else {
            frames.erase(it);
            frames.push_back(page);
        }
    }
    return page_faults;
}

// -----------------------------
// OPTIMAL PAGE REPLACEMENT
// -----------------------------
int predict(const vector<int>& pages, const vector<int>& fr, int index) {
    int farthest = index, res = -1;

    for (int i = 0; i < fr.size(); i++) {
        int j;
        for (j = index; j < pages.size(); j++) {
            if (fr[i] == pages[j]) {
                if (j > farthest) {
                    farthest = j;
                    res = i;
                }
                break;
            }
        }
        if (j == pages.size())
            return i; 
    }
    return (res == -1) ? 0 : res;
}

int optimalPageReplacement(int capacity, vector<int> pages) {
    vector<int> fr;
    int page_faults = 0;

    for (int i = 0; i < pages.size(); i++) {
        bool found = (find(fr.begin(), fr.end(), pages[i]) != fr.end());

        if (!found) {
            page_faults++;
            if (fr.size() < capacity)
                fr.push_back(pages[i]);
            else {
                int j = predict(pages, fr, i + 1);
                fr[j] = pages[i];
            }
        }
    }
    return page_faults;
}

int main() {
    vector<int> pages = {7,0,1,2,0,3,0,4,2,3,0,3,2};
    int capacity = 4;

    cout << "FIFO Page Faults: " << fifoPageReplacement(capacity, pages) << endl;
    cout << "LRU Page Faults: " << lruPageReplacement(capacity, pages) << endl;
    cout << "Optimal Page Faults: " << optimalPageReplacement(capacity, pages) << endl;
    return 0;
}

/*
EXPECTED OUTPUT:

FIFO Page Faults: 7
LRU Page Faults: 6
Optimal Page Faults: 6
*/
