# Assignment 3 - Complete Documentation

**Student Name**: Joud Ali Al-sahli
**Student ID**: 445052278
**Date Submitted**: May 7 ,2026

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [May 7, 2026, 02:00 PM]
**What I implemented**: Project setup and repository linking. My first step was updating the studentID to my actual university ID.

**Challenges encountered**: I was trying to understand how my specific ID would act as a "seed" and change the process generation sequence.

**How I solved it**: I reviewed the code logic to ensure the random generation was correctly tied to the seed I entered.

**Testing approach**: Ran the code once to make sure the environment was working and there were no basic startup errors.

**Time spent**: 1hour

---

### Entry 2 - [May 7 , 3:00 PM]
**What I implemented**: Task 1 & 2: Added ReentrantLock to protect the shared counters and the execution log.

**Challenges encountered**: It was a bit tricky to decide exactly where to start and end the locks to avoid locking the code for too long.

**How I solved it**: I used the try-finally pattern to guarantee that the lock is always released, even if the code hits an error.

**Testing approach**: Monitored the execution log to ensure there was no garbled text or thread interference.

**Time spent**: 1hour 30 min

---

### Entry 3 - [May 7, 4:30 PM]
**What I implemented**: Task 3: Integrated the Semaphore to manage CPU access for the threads.

**Challenges encountered**: The main challenge was making sure only one process was "executing" at any given time.

**How I solved it**: I initialized the semaphore with exactly 1 permit so it acts as a gatekeeper for the CPU.

**Testing approach**: Watched the console output to verify that progress bars were printing one after another, not overlapping.

**Time spent**: 1hour

---

### Entry 4 - [May 7 ,5;30 PM ]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

### Entry 5 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:
In the original code, there were two major race conditions. First, the shared counters like contextSwitchCount were unsafe because the ++ operation isn't atomic; two threads could read the same value and overwrite each other. Second, the executionLog used a standard ArrayList, which isn't thread-safe. Concurrent access there would likely cause a ConcurrentModificationException or corrupt the log data.

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:
The main difference I found is that a ReentrantLock is like a personal key; only the thread that locks it can unlock it. I used it to protect data integrity for the counters. A Semaphore, however, works like a "ticket" system. I used it for the CPU simulation with one permit to ensure the program doesn't physically run more than one process at the exact same millisecond.

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:
A deadlock is a "freeze" where threads wait for each other forever. To prevent this, I did two things: First, I always used finally { unlock(); } so the lock is released no matter what. Second, I designed the code so a thread never tries to grab a second lock while holding one, which breaks the "Circular Wait" condition that causes deadlocks.

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:
I decided to use fine-grained locking (separate locks for each counter). I chose this because the counters (waiting time, completed processes, etc.) are independent. If I used one big lock for everything, it would slow down the threads unnecessarily. With separate locks, one thread can update the waiting time while another updates the switch count at the same time, making the program much more efficient.

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: contextSwitchCount, completedProcessCount, totalWaitingTime

**Why they need protection**: These are shared static variables updated by multiple process threads. Without protection, a "Race Condition" occurs where two threads might try to increment the same variable simultaneously, causing some updates to be lost.

**Synchronization mechanism used**: ReentrantLock (Fine-grained locks: contextSwitchLock, completedProcessLock, waitingTimeLock)

**Code snippet**:
```java
public static void incrementContextSwitch() {
    contextSwitchLock.lock();
    try {
        contextSwitchCount++;
    } finally {
        contextSwitchLock.unlock();
    }
}
```

**Justification**: 

---
Using separate locks for each counter allows for better concurrency, as threads don't have to wait for a global lock to update independent variables.
### Critical Section #2: Execution Log

**What resource**: List<String> executionLog (specifically an ArrayList).

**Why it needs protection**: ArrayList is not thread-safe. If multiple threads call .add() at the same time, it can lead to a ConcurrentModificationException or data corruption within the list.

**Synchronization mechanism used**: ReentrantLock (logLock)

**Code snippet**:
```java
public static void logExecution(String message) {
    logLock.lock();
    try {
        executionLog.add(message);
    } finally {
        logLock.unlock();
    }
}
```

**Justification**: 

---
The lock ensures that only one thread can modify the list at any given time, preserving the integrity of the execution history.
### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: To simulate a physical CPU core by limiting the number of threads that can be in the "Running" state.

**Number of permits and why**: 1 permit. Since it's a uniprocessor simulation, only one process should occupy the CPU at a time.

**Where implemented**: Inside the run() method of the Process class.

**Code snippet**:
```java
SharedResources.cpuSemaphore.acquire();
try {
    // ... CPU execution logic and printing ...
} finally {
    SharedResources.cpuSemaphore.release();
}
```

**Effect on program behavior**: 

---
It serializes the execution, ensuring that progress bars and console outputs for different processes do not overlap or print out of order.
## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
Ran the program 5 times using the same student ID.
```

**Results**: 
In all 5 runs, the "Total Waiting Time" and "Total Context Switches" were identical.

**Why synchronization is necessary**: 
Without it, race conditions would cause different totals in every run because some increments would fail depending on thread timing.

**Conclusion**: 

---
The locks successfully made the program deterministic.

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: Increased the number of processes to 30 to stress the shared log.

**Results**: 0 exceptions occurred.

**What this proves**: The logLock is correctly protecting the ArrayList from simultaneous writes.

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: Completed Processes should equal numProcesses.

**Actual values**: Match (e.g., 16/16).

**Analysis**: The logic for incrementing counters in the finally blocks ensures no process is missed even if it yields or completes.

---

### Test 4: Different Scenarios
**Scenario tested**:Changing timeQuantum to a very small value (500ms).

**Purpose**: Changing timeQuantum to a very small value (500ms).

**Results**: The "Total Context Switches" count increased significantly as expected.

**What I learned**: Smaller time quanta increase overhead but improve responsiveness in a real OS.

---

## Part 5: Reflection and Learning

### What I learned about synchronization:
Synchronization is essential whenever multiple threads share the same memory space. I learned that even a simple ++ operation is not safe in a multi-threaded environment. The most important concept I grasped was the "Critical Section" and how to protect it using ReentrantLock. I also learned that using try-finally is a safety net to prevent deadlocks, ensuring that a lock is always released. Balancing performance with safety was a challenge, specifically choosing between fine-grained and coarse-grained locks. Finally, I realized that Semaphores are powerful tools for managing resource limits, like simulating a single-core CPU.

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: Online Banking Systems: Preventing two people from withdrawing the same money from a shared account at the exact same time.

**Example 2**: 

---

### How I would explain synchronization to others:

Remember Assignment 1 where all processes printed at once and everything looked like a mess? Synchronization is like adding a 'Talking Stick' to the classroom. Even though everyone wants to talk (Threads), only the person holding the stick (Lock) is allowed to speak. When they finish, they pass the stick to the next person. This way, we hear everyone clearly without any shouting matches or confusion."

---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/JoudSa11/OS-Assignment3-Joud-Ali

**Number of commits**: 6

**Commit messages**: 
1.  Initial project setup with Student ID.
2. Implemented ReentrantLocks for counters and logs.
3. Added CPU Semaphore for thread control.
4. Final UI cleanup and documentation.

---

## Summary

**Total time spent on assignment**: 9 hours.

**Key takeaways**: 
1. Race conditions are hard to see but easy to fix with locks.
2. Semaphores are great for limiting hardware resources.
3. Always release locks in a finally block.

**Most challenging aspect**:Debugging why progress bars were overlapping before I correctly placed the Semaphore. 

**What I'm most proud of**: Creating a clean, color-coded terminal UI that reflects a real-time OS scheduler.

---

**End of Documentation**
