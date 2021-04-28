- top
- top -p pid
  - 第一行是任务队列信息，第二行为进程的信息，第三行为cpu信息，第四、五行为内存信息。
![5](./image/5.jpg)

- top -p pid -H 查看进程PID的每一个线程占用CPU情况

- PID转化成16进制为，jstack pid | grep a3a4

- 内存top再按M