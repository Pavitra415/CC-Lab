C Experiment 01 — Hypervisor Performance Comparison
Performance Evaluation of Type-1 and Type-2 Hypervisors

This experiment evaluates the CPU performance of two different virtualization approaches: Proxmox VE, a Type-1 hypervisor, and VMware Workstation, a Type-2 hypervisor.

Ubuntu virtual machines with comparable hardware resources were created on both platforms. The same Sysbench CPU benchmark was executed on both virtual machines using a prime number limit of 20,000 and a single CPU thread.

The Proxmox VE virtual machine achieved 1689.43 events/sec, while the VMware Workstation virtual machine achieved 1058.76 events/sec.

Table of Contents

Aim

Hypervisor Details

Common VM Configuration

Type-1 Hypervisor – Proxmox VE

Type-2 Hypervisor – VMware Workstation

Result Comparison

Performance Discussion

Conclusion

Project Structure

VM Shutdown

1. Aim

The main objectives of this experiment are:

To create and configure a virtual machine using Proxmox VE.

To create and configure a virtual machine using VMware Workstation.

To provide comparable CPU, memory, and storage resources to both virtual machines.

To install Ubuntu on both virtual machines.

To execute the same CPU benchmark in both Ubuntu virtual machines.

To collect CPU throughput and latency measurements.

To compare the performance obtained from the two virtualization environments.

To analyze the effect of virtualization architecture on CPU benchmark performance.

2. Hypervisor Details
Feature	Proxmox VE	VMware Workstation
Hypervisor Category	Type-1	Type-2
Virtualization Method	Bare-metal	Hosted
Main Technology	KVM	VMware virtualization
Host Environment	Directly on physical hardware	Runs above a host operating system
Guest OS	Ubuntu 22.04.5 LTS	Ubuntu 64-bit

A Type-1 hypervisor operates directly on the physical hardware of a computer. A Type-2 hypervisor operates as an application on top of an existing host operating system.

In this experiment, Proxmox VE provides virtualization directly on the physical machine using KVM, while VMware Workstation runs on top of a host operating system.

3. Common VM Configuration

To make the comparison reasonably consistent, similar virtual hardware resources were assigned to both virtual machines.

Resource	Proxmox VE VM	VMware Workstation VM
Operating System	Ubuntu 22.04.5 LTS	Ubuntu 64-bit
CPU	2 vCPU	2 vCPU
Memory	2048 MiB	2048 MB
Storage	20 GB	20 GB
Network	VirtIO / vmbr0	NAT
Benchmark	Sysbench CPU 1.0.20	Sysbench CPU 1.0.20
Prime Number Limit	20000	20000
Threads	1	1
Test Duration	Approximately 10 seconds	Approximately 10 seconds

The CPU benchmark was performed using a single thread so that both virtual machines were tested under the same benchmark workload.

4. Type-1 Hypervisor – Proxmox VE
4.1 VM Setup

The Proxmox VE virtual machine was configured with the following resources:

Setting	Configuration
VM Name	CC-Exp1-Type1
CPU	2 vCPU
CPU Type	x86-64-v2-AES
RAM	2048 MiB
Storage	20 GB
Network	VirtIO with vmbr0
Guest OS	Ubuntu 22.04.5 LTS
Virtualization	KVM
4.2 Commands Executed

The following commands were used to inspect the virtual machine and execute the benchmark:

hostnamectl
lscpu
free -h
df -h
top

sudo apt update
sudo apt install sysbench -y

sysbench --version

sysbench cpu --cpu-max-prime=20000 run


The CPU benchmark used the following parameter:

--cpu-max-prime=20000


This configures Sysbench to perform CPU calculations using prime numbers up to 20,000.

4.3 Benchmark Output

The important measurements obtained from the Proxmox VE virtual machine were:

Metric	Proxmox VE
Number of Threads	1
Prime Limit	20000
Execution Time	10.0006 s
Total Events	16,903
Events/sec	1689.43
Minimum Latency	0.57 ms
Average Latency	0.59 ms
Maximum Latency	1.09 ms
95th Percentile	0.68 ms
4.4 Observation

The Proxmox VE virtual machine completed 16,903 events during the approximately 10-second CPU benchmark.

The measured CPU throughput was:

1689.43 events/sec

The average latency was:

0.59 ms

The maximum measured latency was:

1.09 ms

The benchmark therefore produced relatively high CPU throughput with the given configuration.

4.5 Screenshots

The following screenshots should be included in the project:

Proxmox VE dashboard

VM configuration page

Running VM

Ubuntu console

System information

Sysbench benchmark output

Resource monitoring

5. Type-2 Hypervisor – VMware Workstation
5.1 VM Setup

The VMware Workstation virtual machine was configured with the following settings:

Setting	Configuration
CPU	2 vCPU
Processors	1 processor with 2 cores
Memory	2048 MB
Storage	20 GB
Guest OS	Ubuntu 64-bit
Network	NAT
Host CPU	12th Gen Intel Core i5-12450H
5.2 Commands Executed

The following commands were used to inspect the virtual machine and execute the benchmark:

hostnamectl
lscpu
free -h
df -h
top

sudo apt update
sudo apt install sysbench -y

sysbench --version

sysbench cpu --cpu-max-prime=20000 run


The correct Sysbench CPU parameter is:

--cpu-max-prime=20000

5.3 Benchmark Output

The important measurements obtained from the VMware Workstation virtual machine were:

Metric	VMware Workstation
Number of Threads	1
Prime Limit	20000
Execution Time	10.0002 s
Total Events	10,589
Events/sec	1058.76
Minimum Latency	0.72 ms
Average Latency	0.94 ms
Maximum Latency	5.32 ms
95th Percentile	1.61 ms
5.4 Observation

The VMware Workstation virtual machine completed 10,589 events during the approximately 10-second benchmark.

The measured CPU throughput was:

1058.76 events/sec

The average latency was:

0.94 ms

The maximum measured latency was:

5.32 ms

Compared with the Proxmox VE result, the VMware Workstation VM processed fewer benchmark events during the test period.

5.5 Screenshots

The following screenshots should be included in the project:

VMware VM hardware configuration

Ubuntu VM running

System configuration information

Sysbench benchmark result

6. Result Comparison

The benchmark results obtained from both virtual machines are summarized below.

Parameter	Proxmox VE	VMware Workstation
Hypervisor Type	Type-1	Type-2
Execution Time	10.0006 s	10.0002 s
Total Events	16,903	10,589
Events/sec	1689.43	1058.76
Minimum Latency	0.57 ms	0.72 ms
Average Latency	0.59 ms	0.94 ms
95th Percentile	0.68 ms	1.61 ms
Maximum Latency	1.09 ms	5.32 ms

Therefore, the measured Proxmox VE throughput was approximately 59.6% higher than the VMware Workstation throughput in this experiment.
Performance Chart

The following values can be used to create a comparison chart.

CPU Throughput
Proxmox VE          █████████████████████████████████  1689.43 events/sec
VMware Workstation  ████████████████████               1058.76 events/sec

Average Latency
Proxmox VE          ██████                              0.59 ms
VMware Workstation  █████████                           0.94 ms

Maximum Latency
Proxmox VE          ███                                 1.09 ms
VMware Workstation  ███████████████                     5.32 ms


For the project, the graphical comparison can be saved as:

screenshots/comparison/01-hypervisor-performance-comparison.png

7. Performance Discussion

The benchmark results show different CPU performance measurements between the two virtualization environments even though comparable virtual CPU, memory, storage, and benchmark configurations were used.
8. Conclusion

This experiment compared CPU benchmark performance between a Type-1 virtualization environment using Proxmox VE and a Type-2 virtualization environment using VMware Workstation.

Both virtual machines were configured with comparable resources:

2 virtual CPUs

2 GB RAM

20 GB storage

Ubuntu guest operating system

Sysbench CPU benchmark

Prime limit of 20,000

Single benchmark thread

Approximately 10-second test duration

The measured results were:

Metric	Proxmox VE	VMware Workstation
CPU Throughput	1689.43 events/sec	1058.76 events/sec
Average Latency	0.59 ms	0.94 ms
95th Percentile	0.68 ms	1.61 ms
Maximum Latency	1.09 ms	5.32 ms

In this particular experiment, the Proxmox VE VM produced higher measured CPU throughput and lower measured latency than the VMware Workstation VM.

The results demonstrate that the virtualization environment and its configuration can influence virtual-machine CPU benchmark performance. However, additional controlled test runs and identical host hardware/configurations would be required to make a broader comparison of virtualization platforms.

9. Project Structure
CC-Experiment-01-Hypervisor-Analysis/
│
├── README.md
│
├── screenshots/
│   │
│   ├── type1-proxmox/
│   │   ├── 01-proxmox-dashboard.png
│   │   ├── 02-proxmox-vm-configuration.png
│   │   ├── 03-proxmox-vm-running.jpeg
│   │   ├── 04-proxmox-ubuntu-console.jpeg
│   │   ├── 05-proxmox-system-configuration.jpeg
│   │   ├── 06-proxmox-sysbench-result.jpeg
│   │   └── 07-proxmox-resource-monitoring.jpeg
│   │
│   ├── type2-vmware/
│   │   ├── 01-vmware-vm-configuration.jpeg
│   │   ├── 02-vmware-vm-running.jpeg
│   │   ├── 03-vmware-system-configuration.jpeg
│   │   └── 04-vmware-sysbench-result.jpeg
│   │
│   └── comparison/
│       └── 01-hypervisor-performance-comparison.png
│
└── results/
    └── performance-analysis.md

10. VM Shutdown
Ubuntu VM

The Ubuntu virtual machine can be safely shut down using:

sudo poweroff


Alternatively, the Ubuntu desktop shutdown option can be used.

VMware Workstation

The VMware Workstation guest can also be shut down through:

VM → Power → Shut Down Guest


Using a proper guest shutdown procedure helps prevent filesystem corruption and ensures that the virtual machine is powered off cleanly.

Final Result

The primary benchmark results from the experiment are:

Proxmox VE
CPU Throughput : 1689.43 events/sec
Average Latency: 0.59 ms

VMware Workstation
CPU Throughput : 1058.76 events/sec
Average Latency: 0.94 ms


The experiment therefore records a 59.6% difference in measured CPU throughput, with Proxmox VE producing the higher throughput in this specific test environment.
