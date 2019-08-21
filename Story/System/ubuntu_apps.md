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

<!-- vim-markdown-toc -->

<!-- more -->

# System Monitor

## GUI

pass

## Command Line

**htop**
:   htop is an interactive, commnad-line based system-monitor for viewing the running process on PC.

> sudo apt install htop  


**glances**
:   monitor the total CPU, RAM, and Disk I/O of PC.

>   sudo apt-get install glances  


**lm-sensors**
:   detect each relevant device (processors, fans, etc) and prepare to measure the temperature.

>   sudo apt-get install lm-sensors  
>   Usage:  
>
> > ```sh  
> >    sudo sensors-detect  
> >    sudo watch sensors  
> > ```


**nvidia-smi**
:   GPU memory, power usage, temperature, fan speed and etc.

> Usage:  
>
> > ```sh
> > watch nvidia-smi
> > ```

[参考1](https://medium.com/@george.seif94/the-4-best-command-line-tools-for-monitoring-your-cpu-ram-and-gpu-usage-692e3053000f)


**gpustat**

:  GPU info, fan, power, temperature, pid

> pip install gpustat