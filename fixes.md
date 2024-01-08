# Kubernetes cluster

[Overview](README.md) | [Preparation](preparation.md) | [MasterNode](master_node.md) | [WorkerNode](worker_node.md)





## Server hangs and does not respond

There was an error on the master node saying that a
task hang for more than 120 seconds. The sys log shows a page allocation error on the kernel. The server became unresponsive.

syslog
```log
ubuntu@ubuntu:~$ cat /var/log/syslog | grep kworker
Oct 23 04:23:45 ubuntu kernel: [131162.517658] kworker/2:1H: page allocation failure: order:0, mode:0x40800(GFP_NOWAIT|__GFP_COMP), nodemask=(null),cpuset=/,mems_allowed=0
Oct 23 04:23:45 ubuntu kernel: [131162.517687] CPU: 2 PID: 326139 Comm: kworker/2:1H Tainted: G         C  E     5.4.0-1044-raspi #48-Ubuntu
Oct 23 09:20:51 ubuntu kernel: [148988.080955] INFO: task kworker/3:3:361613 blocked for more than 120 seconds.
Oct 23 09:20:51 ubuntu kernel: [148988.104666] kworker/3:3     D    0 361613      2 0x00000028
Oct 23 09:26:53 ubuntu kernel: [149350.579550] INFO: task kworker/3:3:361613 blocked for more than 120 seconds.
Oct 23 09:26:53 ubuntu kernel: [149350.603299] kworker/3:3     D    0 361613      2 0x00000028
Oct 23 09:32:55 ubuntu kernel: [149713.078234] INFO: task kworker/3:3:361613 blocked for more than 120 seconds.
Oct 23 09:32:56 ubuntu kernel: [149713.101562] kworker/3:3     D    0 361613      2 0x00000028
Oct 23 09:34:56 ubuntu kernel: [149833.911252] INFO: task kworker/3:3:361613 blocked for more than 241 seconds.
Oct 23 09:34:56 ubuntu kernel: [149833.935725] kworker/3:3     D    0 361613      2 0x00000028
Oct 23 09:36:57 ubuntu kernel: [149954.744199] INFO: task kworker/3:3:361613 blocked for more than 362 seconds.
Oct 23 09:36:57 ubuntu kernel: [149954.767905] kworker/3:3     D    0 361613      2 0x00000028
Oct 23 09:40:59 ubuntu kernel: [150196.409891] INFO: task kworker/3:3:361613 blocked for more than 120 seconds.
Oct 23 09:40:59 ubuntu kernel: [150196.433834] kworker/3:3     D    0 361613      2 0x00000028
Oct 23 09:43:00 ubuntu kernel: [150317.242951] INFO: task kworker/3:3:361613 blocked for more than 241 seconds.
Oct 23 09:43:00 ubuntu kernel: [150317.266876] kworker/3:3     D    0 361613      2 0x00000028
Oct 23 09:47:01 ubuntu kernel: [150558.908940] INFO: task kworker/3:3:361613 blocked for more than 120 seconds.
Oct 23 09:47:01 ubuntu kernel: [150558.933625] kworker/3:3     D    0 361613      2 0x00000028
Oct 23 09:53:04 ubuntu kernel: [150921.407444] INFO: task kworker/3:3:361613 blocked for more than 120 seconds.
Oct 23 09:53:04 ubuntu kernel: [150921.431270] kworker/3:3     D    0 361613      2 0x00000028
Oct 23 09:55:05 ubuntu kernel: [151042.240283] INFO: task kworker/3:3:361613 blocked for more than 241 seconds.
Oct 23 09:55:05 ubuntu kernel: [151042.263643] kworker/3:3     D    0 361613      2 0x00000028
Oct 23 20:43:39 ubuntu kernel: [21630.093016] INFO: task kworker/0:0:54826 blocked for more than 120 seconds.
Oct 23 20:43:39 ubuntu kernel: [21630.116815] kworker/0:0     D    0 54826      2 0x00000028

```

@Fix under review. [NOT WORKING]
Forum post: [https://unix.stackexchange.com/questions/593335/why-does-page-allocation-failure-occur-whereas-there-are-still-enough-memoryi]

```bash

echo "vm.min_free_kbytes=16384" >> /etc/sysctl.conf

```

<!-- Try this Clear cache, ram memeory buffer and swap
 https://www.tecmint.com/clear-ram-memory-cache-buffer-and-swap-space-on-linux/ -->