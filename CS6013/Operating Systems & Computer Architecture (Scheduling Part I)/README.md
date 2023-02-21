# Lecture for today: Wrap-up context switch, learn more about process scheduling (policy)

## Vocab

* workload - set of job descriptions (arrival time, run time)
  * Job: View as current CPU burst of a process
  * Process alternates between CPU and I/O
  * Moves between ready and blocked queues

* Scheduler - Logic that decides which job is ready to run
* Metric - measurement of scheduling quality

## Scheduling Performance Metrics


* $\colorbox{blue}{Minimize turnaround time}$
  * Do not want to wait long for job to complete
  * Completion - arrival_time
* $\colorbox{blue}{Minimize response time}$
  * Schedule interactive jobs promptly so users see output quickly
  * Initial schedule time - arrival time
* Minimize waiting time
  * Do not want to spend much time in Ready queue
* Maximize throughput
  * Want many jobs to complete per unit of time
* Maximize resource utilization
  * Keep expensive devices busy (So it want waste money on super computer)
* $\colorbox{blue}{Minimize overhead}$
  * Reduce number of context switches
* $\colorbox{blue}{Maximize fairness}$
  * All job get same amount of CPU over some time interval

## Workload Assumptions

1. Each job runs for the same amount of time. ***- false***
2. All jobs arrive at the same time ***- false***
3. All jobs only use the CPU (no I/O) ***- false***
4. Run-time of each job is known ***- false***

## Scheduling Basics

| Workloads        | Schedulers | Metrics                |
| ---------------- | ---------- | ---------------------- |
| Arrvival_time    | FIFO       | turnaround_time        |
| Run_time         | SJF        | response_time          |
|                  | STCF       |                        |
|                  | RR       |                        |

$$ TurnaroundTime = completionTime - arrivalTime $$

### FIFO

* FIFO: First In, First Out
  * Also called FCFS (First come first served)
  * run jobs in arrival_time order

![FIFO Gantt Chart](/CS6013/Operating%20Systems%20&%20Computer%20Architecture%20(Scheduling%20Part%20I)/Images/FIFO-Gantt-Chart.png)
> Gantt chart: Illustrates how jobs are scheduled over time on a CPU

$$ 20S = (10 + 20 + 30) / 3 $$

10 = Process A, 20 = Process B, 30 = Process C

![FIFO Gantt Chart](/CS6013/Operating%20Systems%20&%20Computer%20Architecture%20(Scheduling%20Part%20I)/Images/FIFO-Gantt-Chart-2.png)
Average turnaround time: 70s (60 + 70 + 80) / 3 ( ***<ins>Convoy Effect</ins>*** )

Problem with FIFO Scheduler: $\colorbox{blue}{Turnaround time suffers when short jobs wait behind long jobs.}$

### SJF

* SJF (Shortest Job First)
  * Choose job with smallest run_time

![SJF Gantt Chart](/CS6013/Operating%20Systems%20&%20Computer%20Architecture%20(Scheduling%20Part%20I)/Images/SJF-Gantt-Chart.png)
Average turnaround time: (80 + 10 + 20) / 3 = ~36.7s
Shorter job before longer job improves turnaround time of short more than it harms turnaround time of long.
> Still a useless algorithm since we stll don't know which job is shortest in advanced.

![SJF Gantt Chart](/CS6013/Operating%20Systems%20&%20Computer%20Architecture%20(Scheduling%20Part%20I)/Images/SJF-Gantt-Chart-2.png)
Avearge turnaround time: (60 + (70 - 10) + (80 - 10)) / 3 = 63.3s

### Preemptive Scheduling

* Non-preemptive
  * FIFO & SJF are $\colorbox{blue}{non-preemptive}$
  * Only schedule new job when previous job voluntarily relinquishes CPU (perform I/O or exits)

* Preemptive
  * STCF (<ins>Shortest Time-to-Completion First</ins>) is $\colorbox{blue}{preemptive}$. Potentially schedule different job at any point by taking CPU away from running job.
  * Always run job that will complete the quickest ($\colorbox{blue}{Allow interuptions}$).

### STCF

![STCF Gantt Chart](/CS6013/Operating%20Systems%20&%20Computer%20Architecture%20(Scheduling%20Part%20I)/Images/STCF-Gantt-Chart.png)
Average turnaround time with SJF: 63.3s
Average turnaround time with STCF: 36.7s

### Reponse Time

![Response time](/CS6013/Operating%20Systems%20&%20Computer%20Architecture%20(Scheduling%20Part%20I)/Images/Response-time.png)
$$ reponseTime = firstRunTime - arrivalTime $$

FIFO. SJF and STCF have poor response time

#### RR

* RR (Round Robin)
  * Alternate read processes
  * Switch after fixed-length $\colorbox{blue}{time-slice}$ (or $\colorbox{blue}{quantum}$)

![FIFO RR Response time](/CS6013/Operating%20Systems%20&%20Computer%20Architecture%20(Scheduling%20Part%20I)/Images/FIFO-RR-Response-time.png)
FIFO average response time = (0 + 5 + 10) / 3 = 5
RR average response time = (0 + 1 + 2) / 3 = 1

RR pro - Average turnaround time with equal job lengths is bad
RR con - If run time unknown, short jobs get change to run, finish fast fair.
