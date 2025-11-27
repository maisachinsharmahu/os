# Shortest Job First (SJF) Scheduling

## 📘 Theory

Shortest Job First (SJF) is a CPU scheduling algorithm that selects the waiting process with the **smallest Burst Time** for execution next.  
It is optimal because it **minimizes the average waiting time** for a given set of processes.

SJF can be implemented in two variants:

- **Non-Preemptive SJF** → Once a process starts, it runs until completion.  
- **Preemptive SJF (SRTF)** → CPU can switch if a new process arrives with a smaller remaining burst.

This document describes **Non-Preemptive SJF**.

### ⚠️ Drawback  
SJF may cause **starvation**, where longer processes may be delayed if shorter jobs keep arriving.

---

## 📐 Algorithm

1. Input number of processes and their Burst Times.  
2. Sort the processes in **ascending order of Burst Time**.  
3. Initialize `currentTime = 0`.  
4. For each process in the sorted list:  
   - `waitingTime = currentTime`  
   - `currentTime = currentTime + burstTime`  
   - `turnAroundTime = waitingTime + burstTime`  
   - Add to totals.  
5. Compute **Average Waiting Time** and **Average Turnaround Time**.  
6. Display the results.

---

## 🧾 Code + Expected Output

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct Process {
    int id;
    int burstTime;
    int waitingTime;
    int turnAroundTime;
};

bool compareBurst(Process a, Process b) {
    return a.burstTime < b.burstTime;
}

int main() {
    int n;
    cout << "Enter number of processes: ";
    cin >> n;

    vector<Process> processes(n);
    for (int i = 0; i < n; i++) {
        processes[i].id = i + 1;
        cout << "Enter Burst Time for Process " << i + 1 << ": ";
        cin >> processes[i].burstTime;
    }

    sort(processes.begin(), processes.end(), compareBurst);

    int currentTime = 0;
    float totalWait = 0, totalTurnAround = 0;

    for (int i = 0; i < n; i++) {
        processes[i].waitingTime = currentTime;
        currentTime += processes[i].burstTime;
        processes[i].turnAroundTime = processes[i].waitingTime + processes[i].burstTime;

        totalWait += processes[i].waitingTime;
        totalTurnAround += processes[i].turnAroundTime;
    }

    cout << "\nPID\tBurst\tWaiting\tTurnaround\n";
    for (int i = 0; i < n; i++) {
        cout << processes[i].id << "\t" 
             << processes[i].burstTime << "\t" 
             << processes[i].waitingTime << "\t" 
             << processes[i].turnAroundTime << endl;
    }

    cout << "\nAverage Waiting Time: " << totalWait / n << endl;
    cout << "Average Turnaround Time: " << totalTurnAround / n << endl;

    return 0;
}

/*
EXPECTED OUTPUT:

Enter number of processes: 4
Enter Burst Time for Process 1: 6
Enter Burst Time for Process 2: 8
Enter Burst Time for Process 3: 7
Enter Burst Time for Process 4: 3

PID     Burst   Waiting Turnaround
4       3       0       3
1       6       3       9
3       7       9       16
2       8       16      24

Average Waiting Time: 7
Average Turnaround Time: 13
*/
