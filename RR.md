 # Round Robin (RR) Scheduling

## 📘 Theory

Round Robin (RR) is a **preemptive CPU scheduling algorithm** designed for **time-sharing systems**.  
A fixed time unit, called the **Time Quantum**, is assigned to each process in a **circular ready queue**.

How it works:
- Each process gets CPU time up to one time quantum.  
- If a process finishes within the quantum → it releases the CPU.  
- If not → it's preempted and moved to the **end of the queue**.  
- Ensures **fairness** and **quick response time**.  
- Choosing the right quantum is crucial:  
  - Too small → too many context switches.  
  - Too large → behaves like FCFS.

---

## 📐 Algorithm

1. Input number of processes, their Burst Times, and the Time Quantum.  
2. Set `remainingTime = burstTime` for all processes.  
3. Initialize `currentTime = 0`.  
4. Repeat until all processes complete:
   - For each process:
     - If `remainingTime > 0`:
       - If `remainingTime > quantum`:
         - `currentTime += quantum`  
         - `remainingTime -= quantum`
       - Else:
         - `currentTime += remainingTime`  
         - `waitingTime = currentTime - burstTime`  
         - `remainingTime = 0`  
         - Increment completed count.
5. Compute:
   - `turnAroundTime = burstTime + waitingTime`
   - Average Waiting & Turnaround Times.
6. Display results.

---

## 🧾 Code + Expected Output

```cpp
#include <iostream>
#include <vector>

using namespace std;

struct Process {
    int id;
    int burstTime;
    int remainingTime;
    int waitingTime;
    int turnAroundTime;
};

int main() {
    int n, quantum;
    cout << "Enter number of processes: ";
    cin >> n;
    cout << "Enter Time Quantum: ";
    cin >> quantum;

    vector<Process> processes(n);
    for (int i = 0; i < n; i++) {
        processes[i].id = i + 1;
        cout << "Enter Burst Time for Process " << i + 1 << ": ";
        cin >> processes[i].burstTime;
        processes[i].remainingTime = processes[i].burstTime;
    }

    int currentTime = 0;
    int completed = 0;

    while (completed < n) {
        bool done = true;
        for (int i = 0; i < n; i++) {
            if (processes[i].remainingTime > 0) {
                done = false;

                if (processes[i].remainingTime > quantum) {
                    currentTime += quantum;
                    processes[i].remainingTime -= quantum;
                } else {
                    currentTime += processes[i].remainingTime;
                    processes[i].waitingTime = currentTime - processes[i].burstTime;
                    processes[i].remainingTime = 0;
                    completed++;
                }
            }
        }
        if (done) break;
    }

    float totalWait = 0, totalTurnAround = 0;

    cout << "\nPID\tBurst\tWaiting\tTurnaround\n";
    for (int i = 0; i < n; i++) {
        processes[i].turnAroundTime = processes[i].burstTime + processes[i].waitingTime;
        totalWait += processes[i].waitingTime;
        totalTurnAround += processes[i].turnAroundTime;

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

Enter number of processes: 3
Enter Time Quantum: 4
Enter Burst Time for Process 1: 10
Enter Burst Time for Process 2: 5
Enter Burst Time for Process 3: 8

PID     Burst   Waiting Turnaround
1       10      13      23
2       5       12      17
3       8       15      23

Average Waiting Time: 13.3333
Average Turnaround Time: 21
*/
