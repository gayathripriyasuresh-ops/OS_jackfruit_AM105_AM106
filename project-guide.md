# Container Runtime with Supervisor, Logging, Kernel Monitoring, and Scheduling

## 1. Team Information
TEAM MEMBERS
- Name: GAYATHRI PRIYA V
- SRN: PES1UG24AM105
- - Name: GONU GUNTLA HEMA KOWSHIK 
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

### 3.1 Multi-container supervision
[Paste screenshot here]  
Explanation: Shows multiple containers running under one supervisor.

---

### 3.2 Metadata tracking
[Paste screenshot here]  
Explanation: Output of engine ps showing container details.

---

### 3.3 Bounded-buffer logging
[Paste screenshot here]  
Explanation: Log file output captured via logging pipeline.

---

### 3.4 CLI and IPC
[Paste screenshot here]  
Explanation: CLI command interacting with supervisor.

---

### 3.5 Soft-limit warning
[Paste screenshot here]  
Explanation: dmesg output showing soft limit exceeded.

---

### 3.6 Hard-limit enforcement
[Paste screenshot here]  
Explanation: dmesg output showing container killed.

---

### 3.7 Scheduling experiment
[Paste screenshot here]  
Explanation: cpu_hog running with NI=10 and CPU usage visible.

---

### 3.8 Clean teardown
[Paste screenshot here]  
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
