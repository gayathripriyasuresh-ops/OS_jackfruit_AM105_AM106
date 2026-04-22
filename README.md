# Multi-Container Runtime
A lightweight Linux container runtime in C with a long-running supervisor and a kernel-space memory monitor.

Team Information

- Name: Gayathri Priya V | SRN: PES1UG24AM105
- Name: G Hema Kowshik| SRN: PES1UG24AM106

### 1. Fork the Repository

1. Go to [github.com/shivangjhalani/OS-Jackfruit](https://github.com/shivangjhalani/OS-Jackfruit)
2. Click **Fork** (top-right)
3. Clone your fork:

```bash
git clone https://github.com/<your-username>/OS-Jackfruit.git
cd OS-Jackfruit
```
<img width="940" height="611" alt="image" src="https://github.com/user-attachments/assets/d500ce2b-606f-4f91-ba30-7c8deff6b985" />

Run the Environment Check
```bash
cd boilerplate
chmod +x environment-check.sh
sudo ./environment-check.sh
```
<img width="1083" height="333" alt="image" src="https://github.com/user-attachments/assets/d4f3c766-b3c3-47e1-af57-10742cce8543" />

Prepare the Root Filesystem
```bash
mkdir rootfs-base
wget https://dl-cdn.alpinelinux.org/alpine/v3.20/releases/x86_64/alpine-minirootfs-3.20.3-x86_64.tar.gz
tar -xzf alpine-minirootfs-3.20.3-x86_64.tar.gz -C rootfs-base
# Make one writable copy per container you plan to run
cp -a ./rootfs-base ./rootfs-alpha
cp -a ./rootfs-base ./rootfs-beta
```
<img width="820" height="511" alt="image" src="https://github.com/user-attachments/assets/386d215b-e317-4e7c-9b0b-cb4e90eb8c39" />

## TASK 1
## 	Multi-container supervision & Metadata tracking
<img width="940" height="515" alt="image" src="https://github.com/user-attachments/assets/0ac61986-cc53-4e35-87f8-da4c1cdc1980" />

## TASK 2
## Bounded-buffer logging & CLI 
<img width="940" height="492" alt="image" src="https://github.com/user-attachments/assets/3d1eb0e4-92e6-4679-b64c-7a243406ca5f" />
## **TASK 3**
## IPC
<img width="940" height="398" alt="image" src="https://github.com/user-attachments/assets/88a4d219-aa86-483d-8a8b-62406dc50ebd" />

## Soft-limit warning & Hard-limit enforcement

<img width="940" height="179" alt="image" src="https://github.com/user-attachments/assets/d5e71561-a725-4a40-af6f-b4128579eede" />
<img width="940" height="106" alt="image" src="https://github.com/user-attachments/assets/08b93386-6e4b-49be-bbc2-cfd4b04ebdab" />
##Scheduling experiment
## Clean teardown
