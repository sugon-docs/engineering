[TOC]

<div style="page-break-after: always;"></div> <!-- 强制分页 -->

---
# 图形
## 图形计算设置
### 导入算例  <div id="font0"></div>
点击左侧的`File`按钮，`Open`选中`.aedt`文件打开
![导入算例](./attachment/load_case.png)

### 并行设置
设置`Intelmpi`为`2021`
![设置mpi版本](./attachment/intelmpi_2021.png)

设置成功可以在提交作业的[Batchoptions](#font2)里查看

!!! tip
    `IntelMPI 2021`可以在高版本`glibc`上使用，默认是`2018`在武汉、青海等集群上就默认用不了，或者设置成`OpenMPI`，也可以使用，但需要[开启Beat功能](#font1)

<div style="page-break-after: always;"></div> <!-- 强制分页 -->

### 提交作业
点开左侧项目数，`Analysis`下右键`Setup1`,点击`Submit Job`
![提交作业](./attachment/submit.png)

`Analysis Specification`下`Batchoptions`会打印当前使用的`MPI`信息<div id="font2"></div>
![MPI信息](./attachment/mpi_version.png)

点击`Compute Resources`，勾选上`automatic settings`，设置好节点和核心数，`Submit Job`提交作业

![并行设置](./attachment/parallel_setting.png)

提交成功之后作业界面会被关闭

<div style="page-break-after: always;"></div> <!-- 强制分页 -->

### 监控作业
`Tools` > `Job Management` > `Monitor Jobs`
![监控作业1](./attachment/monitor_job1.png)

选中正在计算的`.aedt`文件打开，会打印出作业运行的相关日志
![监控作业2](./attachment/monitor_job2.png)

![监控作业3](./attachment/monitor_job3.png)

等待日志结束，作业完成后，再重新打开[导入算例](#font0)查看

### 补充 
#### 开启`Beat`，切换至`OpenMPI` <div id="font1"></div>

`Tools > Options > General Options`
![开启Beat](./attachment/beat.png)

![打开OpenMPI](./attachment/openmpi.png)

<div style="page-break-after: always;"></div> <!-- 强制分页 -->