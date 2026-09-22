# Performance Analysis of Type-1 and Type-2 Hypervisors

## Proxmox VE vs VMware Workstation

This repository contains the laboratory work for analyzing and comparing
the performance of a **Type-1 hypervisor** and a **Type-2 hypervisor**
using an identically configured Ubuntu virtual machine and the
**Sysbench CPU benchmark**.

The experiments covered:

-   **Type-1 Hypervisor:** Proxmox VE
-   **Type-2 Hypervisor:** VMware Workstation
-   **Guest OS:** Ubuntu 22.04.5 LTS
-   **CPU allocation:** 2 vCPU
-   **Memory allocation:** 2 GB
-   **Virtual disk:** 20 GB
-   **Benchmark:** `sysbench cpu --cpu-max-prime=20000 run`

## Student Information

  Field      Details
  ---------- ------------------
  Name       Hrishikesh Patil
  USN        01FE24BCI043
  Roll No.   109
  Division   A1

## Objectives

The experiment was performed to:

1.  Create and verify a virtual machine on a Type-1 hypervisor.
2.  Create and verify an identically configured virtual machine on a
    Type-2 hypervisor.
3.  Inspect CPU, memory, disk and virtualization information.
4.  Monitor guest resource utilization.
5.  Run a repeatable CPU benchmark using Sysbench.
6.  Record execution time, total events, events per second and latency.
7.  Compare the measured results of Type-1 and Type-2 virtualization.

## Hypervisors Used

### 1. Proxmox VE --- Type-1

Proxmox VE is used as the Type-1 hypervisor. The experiment used KVM
virtualization with an Ubuntu 22.04.5 LTS guest.

Configuration:

-   1 CPU socket
-   2 CPU cores / 2 vCPU
-   2048 MiB RAM
-   20 GB virtual disk
-   `vmbr0` network bridge
-   KVM virtualization

### 2. VMware Workstation --- Type-2

VMware Workstation is used as the Type-2 hypervisor. It runs above the
host operating system and provides virtual hardware to the Ubuntu guest.

Configuration:

-   1 processor
-   2 cores / 2 vCPU
-   2048 MB RAM
-   20 GB virtual disk
-   NAT networking
-   Ubuntu 22.04.5 LTS guest

## Experimental Workflow

The same general workflow was followed for both hypervisors:

``` text
Create Virtual Machine
        ↓
Configure CPU, RAM, Disk and Network
        ↓
Install Ubuntu 22.04.5 LTS
        ↓
Verify VM Configuration
        ↓
Monitor Resource Utilization
        ↓
Install Sysbench
        ↓
Run CPU Benchmark
        ↓
Record Performance Metrics
        ↓
Compare Results
```

## Commands Used

### Verify System Information

``` bash
hostnamectl
lscpu
free -h
df -h
```

### Monitor Resources

``` bash
top
```

Press `q` to exit `top`.

### Install Sysbench

``` bash
sudo apt update
sudo apt install sysbench -y
sysbench --version
```

### Run CPU Benchmark

``` bash
sysbench cpu --cpu-max-prime=20000 run
```

### Shut Down the VM

``` bash
sudo poweroff
```

## Results

### Type-1 --- Proxmox VE

  Metric                          Result
  ----------------- --------------------
  Hypervisor                  Proxmox VE
  Type                            Type-1
  Guest OS            Ubuntu 22.04.5 LTS
  CPU                             2 vCPU
  Memory                            2 GB
  Disk                             20 GB
  Execution Time               10.0005 s
  Total Events                    15,877
  Events/sec                    1,587.47
  Average Latency                0.63 ms
  Minimum Latency                0.59 ms
  Maximum Latency                1.34 ms

### Type-2 --- VMware Workstation

  Metric                          Result
  ----------------- --------------------
  Hypervisor          VMware Workstation
  Type                            Type-2
  Guest OS            Ubuntu 22.04.5 LTS
  CPU                             2 vCPU
  Memory                            2 GB
  Disk                             20 GB
  Execution Time               10.0004 s
  Total Events                    14,410
  Events/sec                    1,440.80
  Average Latency                0.69 ms
  Minimum Latency                0.65 ms
  Maximum Latency                1.89 ms

## Performance Comparison

  Metric              Proxmox VE (Type-1)   VMware Workstation (Type-2)
  ----------------- --------------------- -----------------------------
  Execution Time                10.0005 s                     10.0004 s
  Total Events                     15,877                        14,410
  Events/sec                     1,587.47                      1,440.80
  Average Latency                 0.63 ms                       0.69 ms
  Minimum Latency                 0.59 ms                       0.65 ms
  Maximum Latency                 1.34 ms                       1.89 ms

Based on the supplied benchmark results, the Proxmox VE run recorded
**1,587.47 events/sec**, while the VMware Workstation run recorded
**1,440.80 events/sec**. The recorded average latency was **0.63 ms**
for Proxmox VE and **0.69 ms** for VMware Workstation.

The two execution times were almost identical in this particular run, so
the results should be interpreted as measurements from the supplied
experimental setup rather than as universal performance characteristics
of all Type-1 and Type-2 hypervisors.

## Resource Monitoring

The experiments also included resource monitoring.

### Proxmox VE

The Proxmox VE management interface was used to observe:

-   CPU usage
-   Memory usage
-   Network traffic
-   Disk I/O

The Ubuntu guest was also monitored using `top`, `free -h` and `df -h`.

### VMware Workstation

The Ubuntu guest was monitored using:

-   `top`
-   `free -h`
-   `df -h`

The supplied VMware screenshots show the guest's CPU, memory, filesystem
and Sysbench outputs.

## Type-1 vs Type-2 Architecture

### Type-1

``` text
Physical Hardware
       ↓
Proxmox VE / KVM
       ↓
Ubuntu Virtual Machine
```

A Type-1 hypervisor operates directly on the physical machine's
hardware.

### Type-2

``` text
Physical Hardware
       ↓
Host Operating System
       ↓
VMware Workstation
       ↓
Ubuntu Virtual Machine
```

A Type-2 hypervisor operates above the host operating system. Guest CPU,
memory, storage and networking resources are mediated through the
virtualization software and host OS.

## Key Observations

-   Both experiments used Ubuntu 22.04.5 LTS with 2 vCPU, 2 GB RAM and a
    20 GB virtual disk.
-   The guest operating system successfully identified its
    virtualization environment.
-   Standard Linux commands were used to verify CPU, memory and disk
    configuration.
-   `top` was used for live resource monitoring.
-   Sysbench provided a reproducible CPU workload using
    `--cpu-max-prime=20000`.
-   The Proxmox VE experiment recorded higher events/sec in the supplied
    results.
-   The VMware Workstation experiment recorded an almost identical total
    execution time but fewer total events.
-   Virtualization overhead, host workload, resource contention and VM
    configuration can influence measured performance.

## Repository Contents

A suggested repository structure is:

``` text
hypervisor-performance-analysis/
│
├── README.md
│
├── Type-1/
│   └── Type-1.docx
│
├── Type-2/
│   └── Type-2.docx
│
└── Lab-Manual/
    └── Lab-Manual-Hypervisor-Performance-Analysis.docx
```

## Documentation

The project documentation contains:

-   **Part 1:** Performance Analysis Using Type-1 Hypervisor --- Proxmox
    VE
-   **Part 2:** Performance Analysis Using Type-2 Hypervisor --- VMware
    Workstation
-   **Lab Manual:** Step-by-step procedure for creating the VMs,
    verifying configuration, monitoring resources and running Sysbench.

## Conclusion

This laboratory experiment demonstrates how virtualization architecture
can be studied using controlled VM configurations and a repeatable CPU
benchmark. Proxmox VE represents the Type-1 setup, while VMware
Workstation represents the Type-2 setup. The recorded Sysbench results
provide a direct experimental basis for comparing the two configurations
under the supplied test conditions.

## References

-   Proxmox VE --- Type-1 hypervisor experiment documentation
-   VMware Workstation --- Type-2 hypervisor experiment documentation
-   Performance Analysis of Type-1 and Type-2 Hypervisors --- Lab Manual
-   Sysbench CPU benchmark
