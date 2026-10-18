# Chapter 7: CPU Scheduling

## Q1: FIFO vs SJF
- Prediction:FIFO 平均响应时间：200
            FIFO 平均周转时间：400
            SJF 平均响应时间：200
            SJF 平均周转时间：400
- Simulation results:

- Explanation:
三个作业的长度都是 200。FIFO 按输入顺序执行，SJF 选择最短作业执行；由于长度完全相同，两种算法的执行顺序效果相同。
作业	首次运行时间	完成时间	响应时间	周转时间
Job 1	0	           200	        0	      200
Job 2	200	           400	        00	       400
Job 3	400	           600	        00	       600
平均值	—	            —	        200	        400
因此，FIFO 和 SJF 的平均响应时间、平均周转时间都相同。

## Q2: Different Job Lengths
- Prediction:
FIFO 平均响应时间：133.33
FIFO 平均周转时间：333.33
SJF 平均响应时间：133.33
SJF 平均周转时间：333.33
- Simulation results:
- Explanation:
题目给出的作业顺序是 100、200、300，恰好已经从短到长排列。因此 FIFO 的执行顺序与 SJF 相同。
作业	首次运行时间	完成时间	响应时间	周转时间
Job 1（100）  0	        100	         0	      100
Job 2（200）  100	    300	         100	  300
Job 3（300）  300	    600	         300	  600
平均值	        —	    —	         133.33	  333.33

## Q3: Round-Robin, Quantum = 1
- Prediction:
平均响应时间：1
平均周转时间：464
- Simulation results:
- Explanation:
假设三个作业的长度分别为 100、200、300，并且都在时间 0 到达。RR（轮转调度）让每个作业最多运行一个时间单位，然后切换到下一个作业。
三个作业首次运行的时间分别为 0、1、2。
平均响应时间为：
(0 + 1 + 2) / 3 = 1
按照标准轮转调度逻辑，三个作业的完成时间预计分别为 298、497、597。
平均周转时间为：
(298 + 497 + 597) / 3 = 464
RR 可以让每个作业较早获得 CPU 使用机会，但作业需要轮流运行，因此可能需要更长的时间才能完成。

## Q4: When SJF and FIFO Have Equal Turnaround Time
- Answer:
当所有作业长度相同，或者作业已经按照从短到长的顺序排列时，SJF 和 FIFO 的周转时间相同。
- Explanation:
FIFO 按照作业到达顺序执行，SJF 优先执行最短的作业。如果两种算法选择的执行顺序相同，那么每个作业的开始时间和完成时间也相同，因此周转时间相同。
如果较长的作业排在较短的作业前面，FIFO 可能让短作业等待更久，而 SJF 会优先执行短作业，两种算法的周转时间就可能不同。

## Q5: When SJF and RR Have Equal Response Time
- Answer:
当 RR 的时间片足够长，使每个作业都能在时间片结束前完成，并且两种算法采用相同的作业执行顺序时，它们的响应时间相同。
- Explanation:
SJF 是非抢占式调度，作业开始运行后会一直运行到完成。RR 是抢占式调度，当作业用完时间片后，会切换到其他作业。

如果时间片足够长，每个作业都能在被抢占之前完成，那么 RR 不会在作业执行过程中切换到其他作业。在执行顺序相同的情况下，两种算法会让各个作业在相同的时间首次运行，因此响应时间相同。

## Q6: SJF Response Time as Job Length Increases
- Answer:
- Explanation:

## Q7: RR Response Time as Job Count Increases
- Answer:
- Worst-case formula:
- Explanation: