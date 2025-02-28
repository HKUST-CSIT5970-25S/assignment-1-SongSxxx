[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: SONG, Ge
### Student Id: 21143075
### Email: gsongab@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > Your answer goes here.

**The measurement tool I use is phoronix-test-suite, an open source collection of cross-platform benchmarking tools that can be used to test a wide range of hardware and software performance, including CPU and memory performance. 
The CPU test command: phoronix-test-suite run pts/compress-7zip. It is used to test the CPU It is used to test the performance of the CPU in compression and decompression tasks. The compression rating indicates the average number of instructions per second that the CPU is able to execute during a 7-Zip compression task. The higher the value, the better the CPU's performance on the compression task. The Decompression rating represents the average number of instructions per second that the CPU can execute in the 7-Zip decompression task, and reflects the CPU decompression performance. This test suite was chosen because compression and decompression are common CPU-intensive tasks and can effectively evaluate the CPU's ability in data processing.
The memory test command: phoronix-test-suite run pts/ramspeed. pts/ramspeed specifies the memory test suite, which is used to test performance metrics such as read and write speeds of memory. Select this suite to fully evaluate the performance of the memory subsystem. The test type is set to Type: Copy - Benchmark: Integer. The test results indicate the amount of data that the memory is able to transfer per second, which is indicative of the memory's data transfer speed performance.**
 *****
2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` | Compression：3558 MIPS; Decompression:3073 MIPS |    10630.60 MB/s     |
    | `t2.medium`  | Compression：9979 MIPS; Decompression:5871 MIPS |    19111.86 MB/s    |
    | `c5d.large` | Compression：7297 MIPS; Decompression:4909 MIPS  |   13164.58 MB/s   |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

**The performance difference between CPU and memory can be seen in the table.In terms of resource allocation, t2.micro has 1 vCPU and 1GB of memory, t2.medium has 2 vCPUs and 4GB of memory, and c5d.large has 2 vCPUs and 4GB of memory. The comparison results show that the performance of EC2 instances correlates with the increase of vCPU and memory resources, but it is not strictly proportional.
CPU performance: t2.medium doubles the vCPU and memory resources relative to t2.micro, and its CPU compression and decompression performance is significantly higher than that of t2.micro, reflecting the effect of the increased resources on the performance. However, c5d.large and t2.medium have the same number of vCPUs and memory, but the CPU performance of c5d.large is lower than that of t2.medium, which shows that the CPU performance is also related to the CPU architecture and instance design.
Memory performance: The memory performance of t2.medium is higher than t2.micro, reflecting the performance improvement brought by the increase of memory resources. However, c5d.large has the same memory as t2.medium, but its memory performance is lower than that of t2.medium, indicating that the memory performance is also constrained by other factors of instance design.**
*****
## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` | 3.98 Gbits/sec(4075.52Mbps) | 0.221 ms |
    | `m5.large` - `m5.large`   | 4.97 Gbits/sec(5089.28Mbps) | 0.124 ms |
    | `c5n.large` - `c5n.large` | 4.96 Gbits/sec(5079.04Mbps) | 0.153 ms |
    | `t3.medium` - `c5n.large` | 2.30 Gbits/sec(2355.20Mbps) | 0.775 ms |
    | `m5.large` - `c5n.large`  | 4.95 Gbits/sec(5068.80Mbps) | 0.150 ms |
    | `m5.large` - `t3.medium`  | 2.76 Gbits/sec(2826.24Mbps) | 0.586 ms |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

**According to the test results, it can be seen that: between instances of the same type, the bandwidth is relatively high and the RTT is low, the network transmission speed is fast and the latency is low. Between different types of instances, the bandwidth and RTT are more different. t3.medium communicates with other types of instances with significantly lower bandwidth and higher RTT than the same type of instances, which may be related to the network performance specification of the instances themselves.**
 *****
2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      | 33.4 Mbits/sec | 64.568 ms|
    | N. Virginia - N. Virginia | 4.78 Gbits/sec(4894.72Mbps)| 0.138 ms |
    | Oregon - Oregon           | 4.77 Gbits/sec(4884.48Mbps) | 0.217 ms |
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.

**From the data in the table, it can be seen that instances deployed in different regions generally have lower TCP bandwidth and higher round-trip time (RTT) than instances deployed in the same region, given the same AMI and instance type.**
