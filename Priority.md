# Priority Scheduling

## 📘 Theory

Priority Scheduling is a CPU scheduling algorithm where each process is assigned a **priority value**, and the CPU is allocated to the process with the **highest priority**.

- In this implementation, **higher numerical value = higher priority**.  
- The scheduling can be **preemptive** or **non-preemptive**.  
- A major drawback is **starvation**, where low-priority processes may wait indefinitely.  
- **Aging** is a common solution, where waiting processes gradually increase in priority.

This version demonstrates **Non-Preemptive Priority Scheduling**.

---

## 📐 Algorithm

1. Input:
   - Number of processes  
   - Burst Time for each process  
   - Priority of each process  
2. Sort the processes by **priority in descending order** (higher value = higher priority).  
3. Initialize `currentTime = 0`.  
4. For each process:
   - `waitingTime = currentTime`  
   - `currentTime += burstTime`  
   - `turnAroundTime = waitingTime + burstTime`  
5. Compute:
   - Total & average waiting time  
   - Total & average turnaround time  
6. Display results sorted by priority.

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
    int priority;
    int waitingTime;
    int turnAroundTime;
};

bool comparePriority(Process a, Process b) {
    return a.priority > b.priority; 
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
        cout << "Enter Priority for Process " << i + 1 << " (Higher Number = Higher Priority): ";
        cin >> processes[i].priority;
    }

    sort(processes.begin(), processes.end(), comparePriority);

    int currentTime = 0;
    float totalWait = 0, totalTurnAround = 0;

    for (int i = 0; i < n; i++) {
        processes[i].waitingTime = currentTime;
        currentTime += processes[i].burstTime;
        processes[i].turnAroundTime = processes[i].waitingTime + processes[i].burstTime;

        totalWait += processes[i].waitingTime;
        totalTurnAround += processes[i].turnAroundTime;
    }

    cout << "\nPID\tPriority\tBurst\tWaiting\tTurnaround\n";
    for (int i = 0; i < n; i++) {
        cout << processes[i].id << "\t" 
             << processes[i].priority << "\t\t"
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

Enter number of processes: 3
Enter Burst Time for Process 1: 10
Enter Priority for Process 1 (Higher Number = Higher Priority): 2
Enter Burst Time for Process 2: 5
Enter Priority for Process 2 (Higher Number = Higher Priority): 1
Enter Burst Time for Process 3: 8
Enter Priority for Process 3 (Higher Number = Higher Priority): 3

PID     Priority        Burst   Waiting Turnaround
3       3               8       0       8
1       2               10      8       18
2       1               5       18      23

Average Waiting Time: 8.66667
Average Turnaround Time: 16.3333
*/
