---
title: 磁盘性能测试工具FIO的简单使用
date: 2017-10-24 18:09:00
tags: [Performance]
---

#### 简介

FIO是一个用来进行磁盘IO压力测试的工具，可以产生足够多的任意类型的负载 (arbitrary load)，测试并得出对应的比较详细而易懂的报告。FIO支持13种不同的I/O引擎，包括：sync,mmap, libaio, posixaio, SG v3, splice, null, network, syslet,guasi, solarisaio 等等。

​	[FIO官网](http://freecode.com/projects/fio)

​	[2.1.10版本下载](http://brick.kernel.dk/snaps/fio-2.1.10.tar.gz)

#### 安装

官方默认提供的版本是源码安装，安装前需要确认是否有安装GCC。以下是安装步骤：

```shell
# wget直接下载
$ wget http://brick.kernel.dk/snaps/fio-2.1.10.tar.gz
# 解压：
$ tar -zxvf fio-2.1.10.tar.gz
# 编译安装：
$ cd fio-2.1.10
$ ./configure
$ make
$ make install
# 检查是否安装成功：
$ fio
```

### 使用

简单例子：

```shell
# 当前目录下，建立5G大小的test文件进行顺序读，单个线程以4k一个块大小执行，最大执行时间为1000秒，最后结果汇总输出报告，报告名为reportName
$ fio -filename=./test -direct=1 -iodepth 1 -thread -rw=read -ioengine=psync -bs=4k -size=5G -numjobs=1 -runtime=1000 -group_reporting -name=reportName
```

参数说明：


| 参数              | 解析                                       |
| --------------- | ---------------------------------------- |
| filename        | 测试文件名称，在需要测试的挂载的盘的目录下                    |
| direct=1        | direct=1表示测试过程绕过机器自带的buffer，使测试结果更真实     |
| rw              | read/write/randread/randwrite，读写模式[顺序读/顺序写/随机读/随机写] |
| bs              | 单次io的块大小                                 |
| bsrange         | 设定数据块大小范围                                |
| size            | 测试文件总大小                                  |
| numjobs         | 线程数                                      |
| runtime         | 设置执行最大时间，不设置则运行至size设置的大小全部完成            |
| ioengine        | 设定io引擎类型                                 |
| group_reporting | 显示结果汇总每个进程的信息                            |
| name            | 输出报告的名称                                  |

报告例子：

```
read : io=40960MB, bw=238459KB/s, iops=59614, runt=175892msec
    clat (usec): min=35, max=22978, avg=131.27, stdev=61.13
     lat (usec): min=35, max=22979, avg=131.49, stdev=61.14
    clat percentiles (usec):
     |  1.00th=[   74],  5.00th=[   90], 10.00th=[   98], 20.00th=[  107],
     | 30.00th=[  114], 40.00th=[  119], 50.00th=[  125], 60.00th=[  131],
     | 70.00th=[  139], 80.00th=[  149], 90.00th=[  169], 95.00th=[  195],
     | 99.00th=[  258], 99.50th=[  286], 99.90th=[  346], 99.95th=[  378],
     | 99.99th=[  564]
    bw (KB  /s): min=23680, max=35216, per=12.53%, avg=29882.13, stdev=1601.01
    lat (usec) : 50=0.03%, 100=11.33%, 250=87.42%, 500=1.20%, 750=0.01%
    lat (usec) : 1000=0.01%
    lat (msec) : 2=0.01%, 4=0.01%, 10=0.01%, 20=0.01%, 50=0.01%
  cpu          : usr=2.97%, sys=21.49%, ctx=10524303, majf=0, minf=819
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued    : total=r=10485760/w=0/d=0, short=r=0/w=0/d=0
     latency   : target=0, window=0, percentile=100.00%, depth=1
```

报告的重要指标解析：

```
1. read/write 表示是读或者写部分的报告
2. io表示当次执行完毕的io文件大小，bw表示带宽，iops表示每秒io次数，runt表示执行时间
3. clat percentiles表示提交延时的排名分部
4. lat表示响应时间，slat是提交延时，clat是完成延时
5. lat部分的数据表示完成时延具体分布在那些时间段，可以用来直观对比不同磁盘的响应时延分步
5. util表示磁盘利用率
```

