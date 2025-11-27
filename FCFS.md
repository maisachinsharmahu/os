# First-Come, First-Served (FCFS) Scheduling

## 📘 Theory

First-Come, First-Served (FCFS) is the **simplest non-preemptive scheduling algorithm**.  
The process that **arrives first** gets the CPU first.  
It is implemented using a **FIFO (First-In, First-Out) queue**.

When a process enters the ready queue, its PCB is added to the tail of the queue.  
However, FCFS often suffers from the **convoy effect**, where short processes wait behind long ones, causing:

- Poor CPU utilization  
- High waiting time  
- Slow response time  

Thus, FCFS is not ideal for interactive or time-sharing systems.

---

## 📐 Algorithm

1. Input number of processes, their **Burst Time** and **Arrival Time**.  
2. Sort processes in **ascending order of Arrival Time**.  
3. Initialize `currentTime = 0`.  
4. For each process:
   - If `currentTime < arrivalTime`, CPU is idle → set `currentTime = arrivalTime`.  
   - Compute `waitingTime = currentTime - arrivalTime`.  
   - Update `currentTime += burstTime`.  
   - Compute `turnAroundTime = waitingTime + burstTime`.  
5. Compute **average waiting time** and **average turnaround time**.  
6. Display all process information.

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
    int arrivalTime;
    int waitingTime;
    int turnAroundTime;
};

bool compareArrival(Process a, Process b) {
    return a.arrivalTime < b.arrivalTime;
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
        cout << "Enter Arrival Time for Process " << i + 1 << ": ";
        cin >> processes[i].arrivalTime;
    }

    sort(processes.begin(), processes.end(), compareArrival);

    int currentTime = 0;
    float totalWait = 0, totalTurnAround = 0;

    for (int i = 0; i < n; i++) {
        if (currentTime < processes[i].arrivalTime) {
            currentTime = processes[i].arrivalTime;
        }

        processes[i].waitingTime = currentTime - processes[i].arrivalTime;
        currentTime += processes[i].burstTime;
        processes[i].turnAroundTime = processes[i].waitingTime + processes[i].burstTime;

        totalWait += processes[i].waitingTime;
        totalTurnAround += processes[i].turnAroundTime;
    }

    cout << "\nPID\tArrival\tBurst\tWaiting\tTurnaround\n";
    for (int i = 0; i < n; i++) {
        cout << processes[i].id << "\t"
             << processes[i].arrivalTime << "\t"
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
Enter Arrival Time for Process 1: 0
Enter Burst Time for Process 2: 5
Enter Arrival Time for Process 2: 1
Enter Burst Time for Process 3: 8
Enter Arrival Time for Process 3: 2

PID     Arrival Burst   Waiting Turnaround
1       0       10      0       10
2       1       5       9       14
3       2       8       13      21

Average Waiting Time: 7.33333
Average Turnaround Time: 15
*/
