# Reader-Writer Problem (Using Semaphores)

## 📘 Theory

The **Reader-Writer Problem** is a classic synchronization problem involving shared resources such as databases or files. There are two types of processes:

- **Readers** → Only read the shared data  
- **Writers** → Can read and also modify the shared data  

### ✅ Constraints
- Multiple readers can read **simultaneously**.  
- **Writers must have exclusive access** — when a writer is writing, no other reader or writer can access the resource.  

This implementation follows the **First Readers-Writers Problem (Reader Priority)**:

- If a reader is reading, **new readers are allowed** to join.  
- Writers must wait until all readers finish.  
- This may cause **writer starvation** if readers continuously arrive.

### Synchronization Tools Used
- `Semaphore wrt` → Ensures **mutual exclusion** for writers  
- `Semaphore mutex` → Ensures mutual exclusion while modifying `read_count`  
- `int read_count` → Keeps track of the number of active readers  

---

## 📐 Algorithm

### **Shared Variables**
- `Semaphore wrt = 1` → Controls writer access  
- `Semaphore mutex = 1` → Controls updates to `read_count`  
- `int read_count = 0`  

---

### **Writer Process**
1. `wait(wrt)` → Gain exclusive access  
2. Perform writing  
3. `signal(wrt)` → Release access  

---

### **Reader Process**

#### Entry Section
1. `wait(mutex)`  
2. Increment `read_count`  
3. If `read_count == 1` → `wait(wrt)` (block writers)  
4. `signal(mutex)`  

#### Critical Section
- Perform reading  

#### Exit Section
1. `wait(mutex)`  
2. Decrement `read_count`  
3. If `read_count == 0` → `signal(wrt)` (let writers in)  
4. `signal(mutex)`  

---

## 🧾 Code + Expected Output

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <vector>

using namespace std;

// Simple Semaphore class implementation for C++
class Semaphore {
private:
    std::mutex mtx;
    std::condition_variable cv;
    int count;

public:
    Semaphore(int count_ = 0) : count(count_) {}

    void wait() {
        std::unique_lock<std::mutex> lock(mtx);
        cv.wait(lock, [this]() { return count > 0; });
        count--;
    }

    void signal() {
        std::unique_lock<std::mutex> lock(mtx);
        count++;
        cv.notify_one();
    }
};

// Shared Resources
Semaphore wrt(1);   // Controls access for writers
Semaphore mtx(1);   // Protects read_count
int read_count = 0;
int shared_data = 0;

void writer(int id) {
    wrt.wait();
    
    // Critical Section (Writing)
    shared_data++;
    cout << "Writer " << id << " modified data to " << shared_data << endl;
    
    wrt.signal();
}

void reader(int id) {
    // Entry Section
    mtx.wait();
    read_count++;
    if (read_count == 1) {
        wrt.wait(); // First reader blocks writers
    }
    mtx.signal();

    // Critical Section (Reading)
    cout << "Reader " << id << " read data: " << shared_data << endl;

    // Exit Section
    mtx.wait();
    read_count--;
    if (read_count == 0) {
        wrt.signal(); // Last reader releases writers
    }
    mtx.signal();
}

int main() {
    thread t1(reader, 1);
    thread t2(reader, 2);
    thread t3(writer, 1);
    thread t4(reader, 3);
    thread t5(writer, 2);

    t1.join();
    t2.join();
    t3.join();
    t4.join();
    t5.join();

    return 0;
}

/*
EXPECTED OUTPUT:

Reader 1 read data: 0
Reader 2 read data: 0
Writer 1 modified data to 1
Reader 3 read data: 1
Writer 2 modified data to 2

(Note: Actual order of outputs may vary due to thread scheduling, 
but readers will never read while a writer is writing.)
*/
