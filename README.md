# Container Runtime with Supervisor, Logging, Kernel Monitoring, and Scheduling

## 1. Team Information
TEAM MEMBERS
- Name: GAYATHRI PRIYA V
- SRN: PES1UG24AM105
- Name: GONU GUNTLA HEMA KOWSHIK 
- SRN: PES1UG24AM106
- Course: Operating Systems Lab  

---

## 2. Build, Load, and Run Instructions

### 2.1 Build the project
make

---

### 2.2 Load kernel module
sudo insmod monitor.ko

---

### 2.3 Verify control device
ls -l /dev/container_monitor

If not present:
sudo mknod /dev/container_monitor c <MAJOR> 0
sudo chmod 666 /dev/container_monitor

---

### 2.4 Start supervisor
sudo ./engine supervisor ./rootfs-base

---

### 2.5 Create container root filesystems
cp -a ./rootfs-base ./rootfs-alpha  
cp -a ./rootfs-base ./rootfs-beta  

---

### 2.6 Start containers
sudo ./engine start alpha ./rootfs-alpha /bin/sh --soft-mib 48 --hard-mib 80  
sudo ./engine start beta ./rootfs-beta /bin/sh --soft-mib 64 --hard-mib 96  

---

### 2.7 List containers
sudo ./engine ps

---

### 2.8 View logs
sudo ./engine logs alpha

---

### 2.9 Run workloads
sudo ./engine run ./memory_hog  
sudo ./engine run ./cpu_hog 30  

---

### 2.10 Stop containers
sudo ./engine stop alpha  
sudo ./engine stop beta  

---

### 2.11 Inspect kernel logs
sudo dmesg | tail

---

### 2.12 Cleanup
sudo rmmod monitor

---

## 3. Demo with Screenshots

### 3.1 Multi-container supervision and  Metadata tracking
<img width="940" height="515" alt="image" src="https://github.com/user-attachments/assets/0006c8f5-a380-4e28-9635-96d5ea179d7d" />
  
Explanation: Shows multiple containers running under one supervisor and Output of engine ps showing container details.

---

### 3.2 Bounded-buffer logging & CLI
Explanation: Log file output captured via logging pipeline and CLI command interacting with supervisor.
<img width="940" height="492" alt="image" src="https://github.com/user-attachments/assets/d1370d3c-3b7d-49c6-945a-71e4c11084ff" />

---

### 3.3 IPC
<img width="940" height="398" alt="image" src="https://github.com/user-attachments/assets/073f59ca-bffa-4468-9286-6d1b2865f41b" />
Explanation: We used a pipe to capture container output, a bounded circular buffer for intermediate storage, and synchronized producer-consumer threads using mutex and condition variables to ensure no data loss and avoid deadlocks.
---

### 3.4 Soft-limit warning & Hard-limit enforcement
<img width="940" height="179" alt="image" src="https://github.com/user-attachments/assets/6f862385-c6bd-4e83-9b33-aec8a96b02a4" />
<img width="940" height="106" alt="image" src="https://github.com/user-attachments/assets/ee65dec8-7c4a-4937-9149-6e66e465fbfa" />
Explanation: dmesg output showing soft limit exceeded.

---

### 3.5 Scheduling experiment
<img width="924" height="556" alt="image" src="https://github.com/user-attachments/assets/f3ad8ebf-23ed-4c5b-9405-b9d1dbe078d4" />
<img width="940" height="926" alt="image" src="https://github.com/user-attachments/assets/885dc218-248f-428b-bce4-c78fd5cb9b88" />
Explanation: cpu_hog running with NI=10 and CPU usage visible.

---

### 3.6 Clean teardown
<img width="788" height="97" alt="Screenshot 2026-04-22 115412" src="https://github.com/user-attachments/assets/f79908f1-5c7f-4116-8702-4e977fc9f423" />
Explanation: No engine processes remaining after shutdown.

---

## 4. Engineering Analysis

### 4.1 Isolation Mechanisms
The runtime uses PID, UTS, and mount namespaces to isolate containers. Each container has its own process tree and filesystem view. However, all containers share the same host kernel.

---

### 4.2 Supervisor and Process Lifecycle
A long-running supervisor manages containers, tracks metadata, and reaps child processes using wait(). This prevents zombie processes and ensures clean lifecycle handling.

---

### 4.3 IPC, Threads, and Synchronization
The system uses pipes for logging and a CLI communication channel. A bounded buffer is implemented with producer-consumer threads using mutexes and condition variables to avoid race conditions.

---

### 4.4 Memory Management and Enforcement
RSS measures physical memory used. Soft limits generate warnings, while hard limits terminate processes. Enforcement is done in kernel space for reliability.

---

### 4.5 Scheduling Behavior
CPU priority is controlled using setpriority. Higher nice values reduce CPU share. The experiment demonstrates fair scheduling behavior.

---

## 5. Design Decisions and Tradeoffs

Supervisor: Centralized control simplifies management but introduces a single point of failure.  

IPC: Pipes and CLI communication are simple but not highly scalable.  

Logging: Bounded buffer ensures no data loss but adds complexity.  

Kernel Monitor: Kernel-level enforcement is reliable but harder to implement.  

Scheduling: setpriority is simple but offers limited control compared to cgroups.  

---

## 6. Scheduler Experiment Results

Default priority: Higher CPU usage  
Nice = 10: Reduced CPU share  

This shows Linux scheduling balances fairness and responsiveness.

---

## 7. Project Structure

engine.c  
monitor.c  
monitor_ioctl.h  
cpu_hog.c  
memory_hog.c  
Makefile  
README.md  

---

## 8. Conclusion

This project demonstrates container isolation, lifecycle management, IPC, logging, kernel memory monitoring, and CPU scheduling.
