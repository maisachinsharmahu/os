# Producer-Consumer Problem

## 📘 Theory

The **Producer-Consumer Problem**, also called the **Bounded Buffer Problem**, is a classic synchronization problem in operating systems.  
It involves two types of processes:

- **Producer** → Generates data and adds it to a shared buffer  
- **Consumer** → Takes data out of the shared buffer

Both processes use a **fixed-size shared buffer**, which introduces two constraints:

1. **Producer must wait when the buffer is full**  
2. **Consumer must wait when the buffer is empty**

In C++, synchronization is handled using:

- `std::mutex` → ensures mutual exclusion  
- `std::condition_variable` → allows efficient blocking/waking without busy-waiting  
- `std::unique_lock` → used with `condition_variable` for safe locking  

This ensures both processes operate correctly without conflicts or race conditions.

---

## 📐 Algorithm

### **Shared Resources**
- A fixed-size buffer (e.g., `deque<int>`)  
- `mutex` for locking  
- `condition_variable` for full/empty signaling  

---

### **Producer Thread**
1. Acquire lock  
2. If buffer is full → wait  
3. Insert item into buffer  
4. Print `"Produced: [item]"`  
5. Release lock  
6. Notify consumer  

---

### **Consumer Thread**
1. Acquire lock  
2. If buffer is empty → wait  
3. Remove item from buffer  
4. Print `"Consumed: [item]"`  
5. Release lock  
6. Notify producer  
7. Stop when last consumed value is `1`  

---

### **Main**
- Start the producer and consumer threads  
- Join both threads  

---

## 🧾 Code + Expected Output

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <deque>
#include <vector>

using namespace std;

std::mutex mtx;
std::condition_variable cv;
std::deque<int> buffer;
const unsigned int max_buffer_size = 5;

void producer(int val) {
    while (val) {
        std::unique_lock<std::mutex> lock(mtx);
        cv.wait(lock, [] { return buffer.size() < max_buffer_size; });
        
        buffer.push_back(val);
        cout << "Produced: " << val << endl;
        val--;
        
        lock.unlock();
        cv.notify_all();
    }
}

void consumer() {
    while (true) {
        std::unique_lock<std::mutex> lock(mtx);
        cv.wait(lock, [] { return buffer.size() > 0; });
        
        int val = buffer.front();
        buffer.pop_front();
        cout << "Consumed: " << val << endl;
        
        lock.unlock();
        cv.notify_all();
        
        if (val == 1) break;
    }
}

int main() {
    thread t1(producer, 10);
    thread t2(consumer);

    t1.join();
    t2.join();

    return 0;
}

/*
EXPECTED OUTPUT:

Produced: 10
Produced: 9
Produced: 8
Produced: 7
Produced: 6
Consumed: 10
Produced: 5
Consumed: 9
Produced: 4
Consumed: 8
Produced: 3
Consumed: 7
Produced: 2
Consumed: 6
Produced: 1
Consumed: 5
Consumed: 4
Consumed: 3
Consumed: 2
Consumed: 1
*/
