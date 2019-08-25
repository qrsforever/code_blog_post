---

title: Ubuntu系统软件必备

date: 2019-08-15 11:05:35
tags: [System]
categories: [Story]

---

<!-- vim-markdown-toc GFM -->

* [System Monitor](#system-monitor)
    * [GUI](#gui)
    * [Command Line](#command-line)
        * [vmstat](#vmstat)
        * [htop](#htop)
        * [glances](#glances)
        * [lm-sensors](#lm-sensors)
        * [nvidia-smi](#nvidia-smi)
        * [gpustat](#gpustat)

<!-- vim-markdown-toc -->

<!-- more -->

# System Monitor

## GUI

pass

## Command Line

### vmstat

reports information about processes, memory, paging, block IO, traps, disks and cpu activity.

### htop

htop is an interactive, commnad-line based system-monitor for viewing the running process on PC.

> sudo apt install htop


### glances

Glances  is  a  free (LGPL) cross-platform curses-based system monitoring tool which aims to present a
maximum of information in a minimum of space.

This tool is written in Python and uses the psutil library to fetch the statistical values from key
elements, like CPU, load average, memory, network, disks, file  systems,  pro‐cesses and so on.

>   sudo apt-get install glances

Usage:

> case1:
>
> > ```sh
> >  glances --time 5 --process-filter python --disable-bold --disable-left-sidebar --process-short-name
> > ```
> > > ```
> > > 10-255-20-131 (debian stretch/sid 64bit / Linux 4.4.0-157-generic)   Uptime: 2:40:36
> > >
> > > CPU      49.4%  nice:     0.4%    LOAD    4-core     MEM     41.9%  active:    3.50G   SWAP      0.0%
> > > user:    31.1%  irq:      0.0%    1 min:    2.03     total:  7.80G  inactive:  1.71G   total:   8.00G
> > > system:  17.5%  iowait:   1.7%    5 min:    1.31     used:   3.27G  buffers:   81.0M   used:        0
> > > idle:    48.9%  steal:    0.0%    15 min:   1.02     free:   4.53G  cached:    2.58G   free:    8.00G
> > >
> > > Processes filter: python (press ENTER to edit)
> > > TASKS 9 (33 thr), 0 run, 9 slp, 0 oth sorted automatically by cpu_percent, flat view
> > >
> > >   CPU%  MEM%  VIRT   RES   PID USER        NI S    TIME+ IOR/s IOW/s Command
> > >   89.6  30.6 19.9G 2.38G 20424 root         0 D  3:45.60     0     0 python
> > >    1.4   1.9 1.48G  155M  2023 root         0 S 36:46.18     0     0 python
> > >    1.0  25.9 19.8G 2.02G 22214 root         0 S  0:02.28     0     0 python
> > >    0.0   0.2 80.5M 16.2M  1922 root         0 S  0:02.16     0     0 python
> > >    0.0   1.9 1.42G  154M  1942 root         0 S  0:02.10     0     0 python
> > >    0.0   1.9 1.44G  148M 20399 root         0 S  0:02.14     0     0 python
> > >    0.0   0.1 53.5M 10.7M 20430 root         0 S  0:00.40     0     0 python
> > >    0.0  25.9 19.8G 2.02G 22212 root         0 S  0:02.22     0     0 python
> > >    0.0  25.9 19.8G 2.02G 22213 root         0 S  0:02.24     0     0 python
> > >    0.0  25.9 19.8G 2.02G 22215 root         0 S  0:02.22     0     0 python
> > > ```
>
> case2:
>
> > ```sh
> >  glances --time 5 --percpu --hide-kernel-threads --disable-network
> > ```
> > > ```
> > > 10-255-20-131 (debian stretch/sid 64bit / Linux 4.4.0-157-generic)   Uptime: 2:50:22
> > >
> > > PER CPU   50.0%  50.0%  50.0%  50.0%    LOAD    4-core     MEM     40.7%  active:    3.40G   SWAP      0.0%
> > > user:      2.0%   1.0%   1.0%   2.0%    1 min:    2.18     total:  7.80G  inactive:  1.71G   total:   8.00G
> > > system:    1.0%   2.0%   4.0%   0.0%    5 min:    2.06     used:   3.17G  buffers:   81.8M   used:        0
> > > idle:      3.0%   3.0%   1.0%   2.0%    15 min:   1.57     free:   4.63G  cached:    2.58G   free:    8.00G
> > >
> > > DISK I/O     R/s    W/s   TASKS 94 (220 thr), 1 run, 93 slp, 0 oth sorted automatically by cpu_percent, flat view
> > > loop0          0      0
> > > loop1          0      0     CPU%  MEM%  VIRT   RES   PID USER        NI S    TIME+ IOR/s IOW/s Command
> > > loop2          0      0     76.7   0.3  108M 24.4M 30137 dc2-user     0 R  0:00.37     0     0 /usr/bin/python3 /usr/bin/glances --time 5 --percpu --hide-kernel-threads --disable-network
> > > loop3          0      0     76.1  30.6 19.9G 2.38G 20424 root         0 S 13:32.55     0     0 python run_tasks.py --hypes /tmp/tmpf7uyhjqd/hypes.json --viz_pid 20399 --viz_port 8140 --tmp_d
> > > loop4          0      0     30.2   1.9 1.48G  155M  2023 root         0 S 36:55.69     0     0 /usr/bin/python /home/dc2-user/work/qrs/torchcv-release/test/flask_services/cauchy_services.py
> > > loop5          0      0      0.0   0.1 37.1M 5.84M     1 root         0 S  0:39.44     0     0 /sbin/init
> > > loop6          0      0      0.0   0.0 34.6M 3.46M   661 root         0 S  0:02.57     0     0 /lib/systemd/systemd-journald
> > > loop7          0      0      0.0   0.0 92.6M 1.52M   705 root         0 S  0:00.00     0     0 /sbin/lvmetad -f
> > > vda            0      0      0.0   0.0 43.3M 3.77M   725 root         0 S  0:00.54     0     0 /lib/systemd/systemd-udevd
> > > vda1           0      0      0.0   0.0 98.0M 2.49M   976 systemd-t    0 S  0:00.30     0     0 /lib/systemd/systemd-timesyncd
> > >                              0.0   0.2  333M 15.3M  1143 root         0 S  0:00.30     0     0 /usr/sbin/smbd -D
> > > FILE SYS    Used  Total      0.0   0.1  325M 5.71M  1145 root         0 S  0:00.00     0     0 /usr/sbin/smbd -D
> > > / (vda1)   62.9G  77.5G      0.0   0.1  333M 6.60M  1175 root         0 S  0:00.30     0     0 /usr/sbin/smbd -D
> > >                              0.0   0.0 15.7M  868K  1177 root         0 S  0:00.00     0     0 /sbin/dhclient -1 -v -pf /run/dhclient.ens3.pid -lf /var/lib/dhcp/dhclient.ens3.leases -I -df /
> > >                              0.0   0.0 5.10M  148K  1278 root         0 S  0:00.42     0     0 /sbin/iscsid
> > >                              0.0   0.0 5.59M 3.43M  1279 root       -10 S  0:02.30     0     0 /sbin/iscsid
> > >                              0.0   0.0 4.29M 1.20M  1283 root         0 S  0:00.00     0     0 /usr/sbin/acpid
> > >                              0.0   0.0 31.2M 2.93M  1285 root         0 S  0:04.63     0     0 /usr/sbin/cron -f
> > >                              0.0   0.0  250M 3.23M  1287 syslog       0 S  0:01.00     0     0 /usr/sbin/rsyslogd -n
> > >                              0.0   0.0 93.1M 1.35M  1289 root         0 S  0:00.00     0     0 /usr/bin/lxcfs /var/lib/lxcfs/
> > >                              0.0   0.0 25.4M 2.13M  1291 root         0 S  0:00.00     0     0 /usr/sbin/atd -f
> > >
> > > ```


### lm-sensors

detect each relevant device (processors, fans, etc) and prepare to measure the temperature.

>   sudo apt-get install lm-sensors

Usage:

> case:
>
> > ```sh
> >    sudo sensors-detect
> >    sudo watch sensors
> > ```

### nvidia-smi

GPU memory, power usage, temperature, fan speed and etc.

Usage:

> case1: display a list of GPUs connected to the system
>
> > ```sh
> >   nvidia-smi --list-gpus
> > ```
> > > ```
> > >  GPU 0: Tesla P4 (UUID: GPU-e9cdea1d-74a3-d355-9f98-42ddf83c1893)
> > > ```
>
> case2: device monitoring
>
> > ```sh
> > nvidia-smi dmon --id 0 --delay 3 --count 3 --select cpu
> >    c:Proc and Mem Clocks, p:Power Usage and Temperature, u:Utilization
> > ```
> > > ```
> > > # gpu  mclk  pclk   pwr gtemp mtemp    sm   mem   enc   dec
> > > # Idx   MHz   MHz     W     C     C     %     %     %     %
> > >     0  2999  1531    33    73     -     0     0     0     0
> > >     0  2999  1531    33    72     -     0     0     0     0
> > >     0  2999  1518    34    72     -    79    79     0     0
> > > ```
>
> case3: process monitoring
>
> > ```sh
> >  nvidia-smi pmon --count 3 --select um
> > ```
> > > ```
> > > # gpu        pid  type    sm   mem   enc   dec    fb   command
> > > # Idx          #   C/G     %     %     %     %    MB   name
> > >     0       5913     C    72    68     0     0  3225   python
> > >     0       5913     C    66    61     0     0  3225   python
> > >     0       5913     C    71    64     0     0  3225   python
> > > ```

[参考1](https://medium.com/@george.seif94/the-4-best-command-line-tools-for-monitoring-your-cpu-ram-and-gpu-usage-692e3053000f)

[参考2](https://varblog.org/blog/2018/05/24/profiling-and-optimizing-machine-learning-model-training-with-pytorch/)

### gpustat

:  GPU info, fan, power, temperature, pid

> pip install gpustat

Usage:

> case1:
>
> > ```sh
> > gpustat  -cpu -F -P -i 3 --gpuname-width 12 --no-color --no-header
> > ```
> > > ```
> > > [0] Tesla P4     | 44'C,  ?? %,   0 %,   28 /  75 W |  1059 /  7606 MB | root:python/7468(1049M)
> > > ```
>
> case2: json
>
> > ```sh
> >  watch -n 3 gpustat  -cpu -F -P --json
> > ```
> > > ```json
> > > {
> > >     "hostname": "10-255-20-131",
> > >     "query_time": "2019-08-23T11:28:42.900873",
> > >     "gpus": [
> > >         {
> > >             "index": 0,
> > >             "uuid": "GPU-e9cdea1d-74a3-d355-9f98-42ddf83c1893",
> > >             "name": "Tesla P4",
> > >             "temperature.gpu": 67,
> > >             "fan.speed": null,
> > >             "utilization.gpu": 0,
> > >             "power.draw": 32,
> > >             "enforced.power.limit": 75,
> > >             "memory.used": 3219,
> > >             "memory.total": 7606,
> > >             "processes": [
> > >                 {
> > >                     "username": "root",
> > >                     "command": "python",
> > >                     "gpu_memory_usage": 3209,
> > >                     "pid": 7468
> > >                 }
> > >             ]
> > >         }
> > >     ]
> > > }
> > > ```
