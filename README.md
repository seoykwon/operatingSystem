# Operating-System
This repository contains SKKU's OS assignment using xv6.

**Make sure you use x86 architecture in order to boot xv6 kernel.**
If you are a MAC user, emulating x86, or virtualizing x86 in ARM processor will not fulfill the pre-requisites of xv6.
You need to prepare a server that has x86 architecture.
You can either use a Windows OS and WSL, or ssh to a x86 server. 

**Use gcc version 11 as other gcc version make issues in compling.**

I currently use a Windows computer to use Ubuntu 24.04. Use WSL.
```
sudo apt update
sudo apt-get install gcc-11
sudo apt install qemu-system
```
If your Ubuntu has gcc-13 as default, you won't be able to compile xv6 saying there is a infinite loop. Follow the following instruction to downgrade your gcc version: <https://webhostinggeeks.com/howto/how-to-downgrade-gcc-version-on-ubuntu/>

``` make qemu-nox ``` to boot the xv6.

``` Ctrl + 'a' -> 'x' ``` to exit the booted xv6.

## 1. System Call (getnice, setnice, ps)
- add booting message

![image](https://github.com/user-attachments/assets/6b022713-60cb-4a6e-9930-31a655a9e45c)

in [init.c]

#### getnice, setnice, ps
- edit [usys.S] [syscall.h] [syscall.c] [sysproc.c] [proc.h] [proc.c] [defs.h] [user.h] [mytest.c]

#### getnice()
- prints nice value of given pid (process id)

#### setnice()
- sets nice value of given pid

#### ps()
- prints process information: name, pid, state, priority
- when given existing pid, it prints process information of given pid
- when 0 is given, ps() prints every process' information
- when non-existent pid is given, nothing gets printed out
  
## 2. CFS Scheduler

- Makefile cpu to 1, so that we can test our programmed scheduler.
- Add variables in proc struct such as runtime, vruntime, time_slice, run_d_w (runtime/weight). 
- The default nice value is 20.
- The Scheduler function in [proc.c] uses p1, p2, most_p.
- P1 runs over ptable to calculate total_weight to calculate time slice.
- P2 runs over ptable to find the process that has the least vruntime and saves the pid in most_p.
- Time slice calculation : ```most_p->time_slice = ((10 * (weight_table[most_p->nice])) / total_weight) + ( (10 * (weight_table[most_p->nice])) % total_weight !=0 );```
- Saves most_p in c->proc, then use ```swtch(&(c->scheduler), most_p->context)``` to go to [trap.c].

(I could've used priority_queue instead of looping through the whole array, but the TA told my class to just loop through it)

## 3. Virtual Memory (mmap)

## 4. Page Replacement (Swap-in, swap-out)

## 5. File Extension
