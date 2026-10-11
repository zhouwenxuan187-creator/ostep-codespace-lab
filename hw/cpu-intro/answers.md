## Q1 python3 process-run.py -l 5:100,5:100
- Prediction / 预测:
```
Time PID 0 PID 1     CPU IOs
1    RUN    -         1   0
2    RUN    -         1   0
3    RUN    -         1   0
4    RUN    -         1   0
5    RUN    -         1   0
6     -    RUN        1   0
7     -    RUN        1   0
8     -    RUN        1   0
9     -    RUN        1   0
10    -    RUN        1   0
```
- Reasoning / 理由:两个进程各有 5 条 CPU 指令，CPU占用拉满，没有 I/O 操作。
- Verified result / 验证结果:准确
- Analysis / 分析:

## Q2 python3 process-run.py -l 4:100,1:0
- Prediction / 预测:
```
Time PID 0 PID 1      CPU IOs
1    RUN    -          1   0
2    RUN    -          1   0
3    RUN    -          1   0
4    RUN    -          1   0
5     -  RUN:io        0   1
6     -  BLOCKED       0   1
7     -  BLOCKED       0   1
8     -  BLOCKED       0   1
9     -  BLOCKED       0   1
10    -  BLOCKED       0   1
11   -  RUN:io_done    1   0
```
- Reasoning / 理由:默认情况下，PID 0 先运行。由于 PID 0 只包含 CPU 指令，它不会因为 I/O 而阻塞。
- Verified result / 验证结果:准确
- Analysis / 分析:

## Q3 python3 process-run.py -l 1:0,4:100
- Prediction / 预测:
```
Time PID 0    PID 1       CPU IOs
1    RUN:io   -           1   0
2    BLOCKED  RUN         1   1
3    BLOCKED  RUN         1   1
4    BLOCKED  RUN         1   1
5    BLOCKED  RUN         1   1
6    BLOCKED  -           0   1
7    RUN:io_done -        1   0
```
- Reasoning / 理由:与 Q2 相比，只是交换了进程顺序。PID 0 等待 I/O 时，操作系统可以调度 PID 1 使用 CPU，减少 CPU 空闲时间。
- Verified result / 验证结果:准确
- Analysis / 分析:

## Q4 python3 process-run.py -l 1:0,4:100 -S SWITCH_ON_END
- Prediction / 预测:
```
Time PID 0       PID 1     CPU IOs
1    RUN:io      -         1   0
2    BLOCKED     -         0   1
3    BLOCKED     -         0   1
4    BLOCKED     -         0   1
5    BLOCKED     -         0   1
6    BLOCKED     -         0   1
7    RUN:io_done -         1   0
8    -           RUN       1   0
9    -           RUN       1   0
10   -           RUN       1   0
11   -           RUN       1   0
```
- Reasoning / 理由:-S SWITCH_ON_END 的作用（当进程做 I/O 时，阻止 CPU 切换）。
- Verified result / 验证结果:准确
- Analysis / 分析:

## Q5 python3 process-run.py -l 1:0,4:100 -S SWITCH_ON_IO
- Prediction / 预测:
```
Time PID 0    PID 1       CPU IOs
1    RUN:io   -           1   0
2    BLOCKED  RUN         1   1
3    BLOCKED  RUN         1   1
4    BLOCKED  RUN         1   1
5    BLOCKED  RUN         1   1
6    BLOCKED  -           0   1
7    RUN:io_done -        1   0
```
- Reasoning / 理由:系统默认调用和Q3情况一样
- Verified result / 验证结果:准确
- Analysis / 分析:

## Q6 python3 process-run.py -l 3:0,5:100,5:100,5:100 -S SWITCH_ON_IO -I IO_RUN_LATER
- Prediction / 预测:
```
Time PID 0       PID 1    PID 2    PID 3    CPU IOs
1    RUN:io      -        -        -        1   0
2    BLOCKED     RUN      -        -        1   1
3    BLOCKED     RUN      -        -        1   1
4    BLOCKED     RUN      -        -        1   1
5    BLOCKED     RUN      -        -        1   1
6    BLOCKED     RUN      -        -        1   1
7    BLOCKED     -        RUN      -        1   0
8    BLOCKED     -        RUN      -        1   0
9    BLOCKED     -        RUN      -        1   0
10   BLOCKED     -        RUN      -        1   0
11   BLOCKED     -        RUN      -        1   0
12   BLOCKED     -        -        RUN      1   0
13   BLOCKED     -        -        RUN      1   0
14   BLOCKED     -        -        RUN      1   0
15   BLOCKED     -        -        RUN      1   0
16   BLOCKED     -        -        RUN      1   0
17   RUN:io      -        -        -        1   0
18   BLOCKED     -        -        -        0   1
19   BLOCKED     -        -        -        0   1
20   BLOCKED     -        -        -        0   1
21   BLOCKED     -        -        -        0   1
22   BLOCKED     -        -        -        0   1
23   RUN:io      -        -        -        1   0
24   BLOCKED     -        -        -        0   1
25   BLOCKED     -        -        -        0   1
26   BLOCKED     -        -        -        0   1
27   BLOCKED     -        -        -        0   1
28   BLOCKED     -        -        -        0   1
29   RUN:io_done -        -        -        1   0
```
- Reasoning / 理由:PID 0执行完第一次I/O后进入末尾重新排队。
- Verified result / 验证结果:错误
- Analysis / 分析:run:io和run_io done是一个循环不可缺少其中一项

## Q7 python3 process-run.py -l 3:0,5:100,5:100,5:100 -S SWITCH_ON_IO -I IO_RUN_IMMEDIATE
- Prediction / 预测:
```
Time PID 0       PID 1    PID 2    PID 3    CPU IOs
1    RUN:io      -        -        -        1   0
2    BLOCKED     RUN      -        -        1   1
3    BLOCKED     RUN      -        -        1   1
4    BLOCKED     RUN      -        -        1   1
5    BLOCKED     RUN      -        -        1   1
6    BLOCKED     RUN      -        -        1   1
7    RUN:io      -        -        -        1   0
8    BLOCKED     -        RUN      -        1   1
9    BLOCKED     -        RUN      -        1   1
10   BLOCKED     -        RUN      -        1   1
11   BLOCKED     -        RUN      -        1   1
12   BLOCKED     -        RUN      -        1   1
13   RUN:io      -        -        -        1   0
14   BLOCKED     -        -        RUN      1   1
15   BLOCKED     -        -        RUN      1   1
16   BLOCKED     -        -        RUN      1   1
17   BLOCKED     -        -        RUN      1   1
18   BLOCKED     -        -        RUN      1   1
19   RUN:io_done -        -        -        1   0
```
- Reasoning / 理由:IO_RUN_IMMEDIATE 在这种场景下效率明显更高，因为 PID 0 的 I/O 完成时机刚好和其他进程跑完的时机重合，插队后 CPU 和 IO 设备可以一直并行工作，没有任何空闲。
- Verified result / 验证结果:错误
- Analysis / 分析:run:io和run_io done是一个循环不可缺少其中一项

## Q8 python3 process-run.py -s 1 -l 3:50,3:50
- Prediction / 预测:
```
s1:
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7        BLOCKED          DONE                           1
  8*   RUN:io_done          DONE             1          
  9         RUN:io          DONE             1          
 10        BLOCKED          DONE                           1
 11        BLOCKED          DONE                           1
 12        BLOCKED          DONE                           1
 13        BLOCKED          DONE                           1
 14        BLOCKED          DONE                           1
 15*   RUN:io_done          DONE             1          

 s2:
 Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED        RUN:io             1             1
  4        BLOCKED       BLOCKED                           2
  5        BLOCKED       BLOCKED                           2
  6        BLOCKED       BLOCKED                           2
  7*   RUN:io_done       BLOCKED             1             1
  8         RUN:io       BLOCKED             1             1
  9*       BLOCKED   RUN:io_done             1             1
 10        BLOCKED        RUN:io             1             1
 11        BLOCKED       BLOCKED                           2
 12        BLOCKED       BLOCKED                           2
 13        BLOCKED       BLOCKED                           2
 14*   RUN:io_done       BLOCKED             1             1
 15        RUN:cpu       BLOCKED             1             1
 16*          DONE   RUN:io_done             1          
s3:
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED        RUN:io             1             1
  4        BLOCKED       BLOCKED                           2
  5        BLOCKED       BLOCKED                           2
  6        BLOCKED       BLOCKED                           2
  7        BLOCKED       BLOCKED                           2
  8*   RUN:io_done       BLOCKED             1             1
  9*       RUN:cpu         READY             1          
 10           DONE   RUN:io_done             1          
 11           DONE        RUN:io             1          
 12           DONE       BLOCKED                           1
 13           DONE       BLOCKED                           1
 14           DONE       BLOCKED                           1
 15           DONE       BLOCKED                           1
 16           DONE       BLOCKED                           1
 17*          DONE   RUN:io_done             1          
 18           DONE       RUN:cpu             1     

```
- Reasoning / 理由:虽然默认 CPU 与 I/O 能够交替执行，但 I/O 完成后仅回到就绪队列尾部重新排队，而非立即抢占 CPU，因此整体效率低于直接插队运行的策略。
- Verified result / 验证结果:
- Analysis / 分析:

