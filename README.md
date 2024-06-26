# Operating-System(E0-253)
Project for E0-253 Operating System IISc

## Overview
This repository contains three projects as part of the Operating Systems coursework. Each project focuses on critical aspects of modern operating systems, namely core isolation, process pinning, and implementing a task scheduler, process checkpointing

### Project 0: Core Isolation and Process Pinning 
#### (Kernel Programming, System Call Implementation, Kernel Debugging)
This project involves isolating a CPU core and pinning specific processes to that core. The goal is to create a controlled environment for testing and performance analysis.

### Project 1: Rotating Staircase Deadline Scheduler (RSDL) - An O(1), Starvation Free Scheduler for Linux Kernel
#### (Linux Scheduling, Kernel Programming, RSDL)
This project focuses on building and integrating a new task scheduler within the Linux kernel. The scheduler, known as the Rotating Staircase Deadline Scheduler (RSDL), ensures fairness, bounded latency, no starvation, and good interactivity. The scheduler was configured to schedule normal processes running on a particular isolated core. Used CPU-SETs, CPU-Hotplugging functionality for isolating the core.

### Project 2: Process Checkpointing - Context-Based Memory State Management
#### (Kernel Programming, Page fault handling, System-call Implementation)
Implemented an in-kernel page fault handling mechanism to facilitate saving or restoring process context to mitigate risk associated with unsafe code execution. Compared to fork this light weight mechanism observed speedup of 30x


## Project 0: Core Isolation and Process Pinning

### Background
In modern operating systems, isolating CPU cores can significantly improve the performance of specific tasks by preventing interference from other processes. This project involves creating a test bed by isolating a CPU core and pinning high and low priority processes to the isolated core.

### Implementation
1. **Kernel Space**: Implement a system call to isolate CPU 0.
2. **User Space**: Schedule two processes with the highest and lowest priorities to run exclusively on CPU 0.

### Deliverables
- A single kernel patch containing all changes.
- Modified user space file (`main.c`).

### Instructions
1. **Download the Linux Kernel**:
    ```bash
    wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.0.19.tar.xz
    ```
2. **Implement Core Isolation**: Modify the kernel to isolate CPU 0.
3. **Process Pinning**: Schedule processes to run on the isolated core.

## Project 1: Rotating Staircase Deadline Scheduler (RSDL)

### Background
The task scheduler is a core component of any multitasking operating system. This project involves building and integrating the Rotating Staircase Deadline Scheduler (RSDL) into the Linux kernel. RSDL is an O(1) scheduler that guarantees fairness and bounded latency.

### Implementation
- **Scheduler Design**: RSDL uses two arrays, `active` and `expired`, each maintaining a list of runqueues based on priority levels.
- **Task Scheduling**: Tasks are scheduled from highest to lowest priority, with each task receiving a fixed quota of 5 time units.

### Key Features
- **Fairness and No Starvation**: Ensures all tasks get a fair share of CPU time.
- **Bounded Latency**: Limits the maximum waiting time for tasks.
- **Interactivity**: Enhances system responsiveness.

### Deliverables
- A single kernel patch containing all changes.
- User space files for testing the scheduler.

### Instructions
1. **Download the Linux Kernel**:
    ```bash
    wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.0.19.tar.xz
    ```
2. **Implement RSDL**: Modify the kernel to implement the RSDL scheduler.
3. **Testing**: Ensure all processes running on CPU 0 are scheduled using the RSDL scheduler.

## Getting Started

### Prerequisites
- Linux kernel version 6.0.19
- Git
- Basic knowledge of kernel development

### Building the Kernel
1. **Download and extract the kernel source**:
    ```bash
    wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.0.19.tar.xz
    tar -xf linux-6.0.19.tar.xz
    cd linux-6.0.19
    ```
2. **Apply the kernel patch**:
    ```bash
    git apply /path/to/your/patch.patch
    ```
3. **Build and install the kernel**:
    ```bash
    make -j$(nproc)
    sudo make modules_install
    sudo make install
    ```

### Running the Scheduler
- **Boot your system with the new kernel**.
- **Verify core isolation and process pinning**:
    ```bash
    ./main
    ```
- **Test the RSDL scheduler** by running your custom tests.

## Project 2 : Process Checkpointing - Context-Based Memory State Management

### Overview
This project is part of the E0-253: Operating Systems course assignment. The goal is to implement an efficient method to save and restore program states in user-space programs, as an alternative to the fork-based method in Linux.

### Background
The C programming language is inherently unsafe as it allows arbitrary memory access. This project aims to build a system call for user-space programs to save and restore their program state in memory. This is particularly useful when executing potentially unsafe code sections.

### Objective
To introduce a context-based method in Linux to save and restore program state without creating an additional process. This will be achieved by implementing a new system call named `mmcontext`.

### System Call
The `mmcontext` system call accepts an integer argument:
- `0` to save the program state.
- `1` to restore the program state.

### Requirements
1. Save/restore the state of dynamically allocated anonymous memory.
2. Return `0` on success or an appropriate error code on failure.
3. Allow only one context to be saved at a time.
4. Support single-threaded programs only.
5. Ensure no memory leaks upon process termination.

## Implementation Details
### Key Components
- **Memory State Management**: Manage the state of dynamically allocated anonymous memory.
- **Error Handling**: Handle errors like invalid arguments, existing contexts, and more.
- **Performance Optimization**: Ensure minimal overhead compared to the fork-based method.

### Assumptions
- Ignore the state of the process stack, files, signals, and registers.
- Ensure address space remains unaltered between save and restore calls.
- Clean up process state upon termination to avoid memory leaks.

## Evaluation
The implementation will be evaluated based on:
1. **Functional Correctness**: The system call should work for various program sizes.
2. **Performance**: Compared against the fork-based method in terms of throughput.

## Getting Started
### Prerequisites
- Linux kernel source code
- GCC compiler
- Git

### Setup
1. Clone the repository:
   ```bash
   git clone https://github.com/csl-iisc/e0253-os-2023.git

## References
- [Linux Kernel Archives](https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.0.19.tar.xz)
- [RSDL Wikipedia](https://ck.fandom.com/wiki/RSDL)
- [Linux Kernel Mailing List](https://lore.kernel.org/lkml/200703050104.09603.kernel@kolivas.org/T/)
- [Linux Man Pages](https://man7.org/linux/man-pages/man7/sched.7.html)
- [YouTube Video on RSDL](https://www.youtube.com/watch?v=vjkBzcurHsY)

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments
- Con Kolivas for the RSDL scheduler concept.
- Operating Systems course instructors for project guidance.

---

Feel free to contribute by submitting issues or pull requests to improve this project.

