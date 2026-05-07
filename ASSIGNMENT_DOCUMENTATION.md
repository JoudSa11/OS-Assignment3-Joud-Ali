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

**Which variables**: 

**Why they need protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #2: Execution Log

**What resource**: 

**Why it needs protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 

**Number of permits and why**: 

**Where implemented**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Effect on program behavior**: 

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: 

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 

**Results**: 

**What this proves**: 

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 

**Actual values**: 

**Analysis**: 

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 

**Results**: 

**What I learned**: 

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 

**Example 2**: 

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

---

## Part 6: GitHub Repository Information

**Repository URL**: 

**Number of commits**: 

**Commit messages**: 
1. 
2. 
3. 
4. 

---

## Summary

**Total time spent on assignment**: 

**Key takeaways**: 
1. 
2. 
3. 

**Most challenging aspect**: 

**What I'm most proud of**: 

---

**End of Documentation**
