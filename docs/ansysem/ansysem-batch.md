[TOC]

<div style="page-break-after: always;"></div> <!-- 强制分页 -->

---
# 脚本计算
```bash
#!/bin/bash 
#SBATCH -J HFSS
#SBATCH -p wzacnormal04
#SBATCH -N 2 
#SBATCH --ntasks-per-node=60
#SBATCH --exclusive

module purge
#export LD_PRELOAD=/work/home/tianzheng21/lib/my.so
source /work/home/tianzheng21/apprepo/hfss/2024r1-none/scripts/env.sh
WORK_DIR=`pwd`   ##获取当前路径
cd ${WORK_DIR}
EXEC=$PROGLIST
INPUT_FILE=${WORK_DIR}/weidai_dan_suofang0.5.aedt
HOST_FILE=$(generate_pbs_nodefile)
cat ${HOST_FILE} > ${WORK_DIR}/HOST_STRING

HOST_STRING=""
for i_node in `cat ${HOST_FILE} | uniq`; do
    i_ppn=`cat ${HOST_FILE} | grep ${i_node} | wc -l`
    if [ -z ${HOST_STRING} ];then
        HOST_STRING="${i_node}:-1:${i_ppn}"
    else
        HOST_STRING="${HOST_STRING},${i_node}:-1:${i_ppn}"
    fi
done

rm -fr *.lock

#程序运行命令
# ssh $HOSTNAME "cd ${WORK_DIR};$EXEC  -auto -distributed  -machinelist list="${HOST_STRING}" -monitor -ng -batchoptions  'HPCLicenseType'='Pack'  -batchsolve $INPUT_FILE"

## 用openmpi运行
$EXEC -distributed -machinelist list="${HOST_STRING}" -auto -monitor -ng -batchoptions " 'HFSS/DefaultProcessPriority'='Normal' 'HFSS/EnableGPU'='0' 'HFSS/EnableGPUForSBR'='1' 'HFSS/MPIVendor'='Open MPI' 'HFSS/MPIVersion'='Default' 'HFSS/RemoteSpawnCommand'='SSH' " -batchsolve $INPUT_FILE 

## 用 intelmpi 2021 运行 
$EXEC -distributed -machinelist list="${HOST_STRING}" -auto -monitor -ng -batchoptions " 'HFSS/DefaultProcessPriority'='Normal' 'HFSS/EnableGPU'='0' 'HFSS/EnableGPUForSBR'='1' 'HFSS/MPIVendor'='Intel' 'HFSS/MPIVersion'='2021' 'HFSS/RemoteSpawnCommand'='SSH' " -batchsolve $INPUT_FILE
```

!!! tip
    版本提升后脚本识别有点小问题，旧脚本提交，会报指令不识别等，其实根本原因就是" "里的内容，没有传递好
