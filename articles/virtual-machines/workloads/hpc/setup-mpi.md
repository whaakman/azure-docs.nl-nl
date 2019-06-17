---
title: Instellen van de Message Passing Interface voor HPC - Azure Virtual Machines | Microsoft Docs
description: Meer informatie over het instellen van de MPI voor HPC in Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/15/2019
ms.author: amverma
ms.openlocfilehash: 1717c0d95c00e13ae0d87ab920bc88cd4f0df978
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66809840"
---
# <a name="set-up-message-passing-interface-for-hpc"></a>Instellen van de Message Passing Interface voor HPC

Message Passing Interface (MPI) werkbelastingen zijn een aanzienlijk deel van traditionele HPC-workloads. De SR-IOV ingeschakeld VM-grootten in Azure vrijwel elk smaak van MPI kunnen moet worden gebruikt. 

MPI-opdrachten uitvoeren op virtuele machines vereist instellen van partitiesleutels (p-sleutels) voor een tenant. Volg de stappen in de [detecteren partitiesleutels](#discover-partition-keys) sectie voor meer informatie over het bepalen van de p-sleutelwaarden.

## <a name="ucx"></a>UCX

[UCX](https://github.com/openucx/ucx) biedt de beste prestaties op IB en werkt met MPICH en OpenMPI.

```bash
wget https://github.com/openucx/ucx/releases/download/v1.4.0/ucx-1.4.0.tar.gz
tar -xvf ucx-1.4.0.tar.gz
cd ucx-1.4.0
./configure --prefix=<ucx-install-path>
make -j 8 && make install
```

## <a name="openmpi"></a>OpenMPI

Installeer UCX zoals eerder is beschreven.

```bash
sudo yum install –y openmpi
```

Bouw OpenMPI.

```bash
wget https://download.open-mpi.org/release/open-mpi/v4.0/openmpi-4.0.0.tar.gz
tar -xvf openmpi-4.0.0.tar.gz
cd openmpi-4.0.0
./configure --with-ucx=<ucx-install-path> --prefix=<ompi-install-path>
make -j 8 && make install
```

OpenMPI worden uitgevoerd.

```bash
<ompi-install-path>/bin/mpirun -np 2 --map-by node --hostfile ~/hostfile -mca pml ucx --mca btl ^vader,tcp,openib -x UCX_NET_DEVICES=mlx5_0:1  -x UCX_IB_PKEY=0x0003  ./osu_latency
```

Controleer uw partitiesleutel zoals hierboven vermeld.

## <a name="mpich"></a>MPICH

Installeer UCX zoals eerder is beschreven.

Bouw MPICH.

```bash
wget http://www.mpich.org/static/downloads/3.3/mpich-3.3.tar.gz
tar -xvf mpich-3.3.tar.gz
cd mpich-3.3
./configure --with-ucx=<ucx-install-path> --prefix=<mpich-install-path> --with-device=ch4:ucx
make -j 8 && make install
```

MPICH uitgevoerd.

```bash
<mpich-install-path>/bin/mpiexec -n 2 -hostfile ~/hostfile -env UCX_IB_PKEY=0x0003 -bind-to hwthread ./osu_latency
```

Controleer uw partitiesleutel zoals hierboven vermeld.

## <a name="mvapich2"></a>MVAPICH2

Bouw MVAPICH2.

```bash
wget http://mvapich.cse.ohio-state.edu/download/mvapich/mv2/mvapich2-2.3.tar.gz
tar -xv mvapich2-2.3.tar.gz
cd mvapich2-2.3
./configure --prefix=<mvapich2-install-path>
make -j 8 && make install
```

MVAPICH2 uitgevoerd.

```bash
<mvapich2-install-path>/bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=48 ./osu_latency
```

## <a name="platform-mpi-community-edition"></a>Platform MPI Community-editie

Installeer de vereiste pakketten voor MPI-Platform.

```bash
sudo yum install libstdc++.i686
sudo yum install glibc.i686
Download platform MPI at https://www.ibm.com/developerworks/downloads/im/mpi/index.html 
sudo ./platform_mpi-09.01.04.03r-ce.bin
```

Volg het installatieproces.

## <a name="intel-mpi"></a>Intel MPI

[Downloaden van Intel MPI](https://software.intel.com/mpi-library/choose-download).

De omgevingsvariabele I_MPI_FABRICS afhankelijk van de versie wijzigen. Gebruik voor Intel MPI 2018 `I_MPI_FABRICS=shm:ofa` en gebruik voor 2019, `I_MPI_FABRICS=shm:ofi`.

Proces vastmaken werkt goed voor 15, 30 en 60 PPN standaard.

## <a name="osu-mpi-benchmarks"></a>OSU MPI Benchmarks

[Downloaden OSU MPI Benchmarks] [ http://mvapich.cse.ohio-state.edu/benchmarks/ ](http://mvapich.cse.ohio-state.edu/benchmarks/) en untar.

```bash
wget http://mvapich.cse.ohio-state.edu/download/mvapich/osu-micro-benchmarks-5.5.tar.gz
tar –xvf osu-micro-benchmarks-5.5.tar.gz
cd osu-micro-benchmarks-5.5
```

Bouw Benchmarks met behulp van een bepaalde MPI-bibliotheek:

```bash
CC=<mpi-install-path/bin/mpicc>CXX=<mpi-install-path/bin/mpicxx> ./configure 
make
```

MPI Benchmarks zijn onder `mpi/` map.


## <a name="discover-partition-keys"></a>Partitiesleutels detecteren

Ontdek partitiesleutels (p-sleutels) om te communiceren met andere virtuele machines.

```bash
/sys/class/infiniband/mlx5_0/ports/1/pkeys/0
/sys/class/infiniband/mlx5_0/ports/1/pkeys/1
```

De grootste van de twee is de tenantsleutel die moet worden gebruikt met MPI. Voorbeeld: Als de volgende de p-sleutels zijn, moet 0x800b met MPI worden gebruikt.

```bash
cat /sys/class/infiniband/mlx5_0/ports/1/pkeys/0
0x800b
cat /sys/class/infiniband/mlx5_0/ports/1/pkeys/1
0x7fff
```

Gebruik de partitie dan de partitiesleutel standaard (0x7fff). UCX vereist maximaal tien van p-sleutel die moet worden gewist. Bijvoorbeeld, UCX_IB_PKEY ingesteld als 0x000b voor 0x800b.


## <a name="set-up-user-limits-for-mpi"></a>Gebruikerslimieten instellen voor MPI

Gebruikerslimieten instellen voor MPI.

```bash
cat << EOF >> /etc/security/limits.conf
*               hard    memlock         unlimited
*               soft    memlock         unlimited
*               hard    nofile          65535
*               soft    nofile          65535
EOF
```


## <a name="set-up-ssh-keys-for-mpi"></a>SSH-sleutels voor MPI instellen

Instellen van SSH-sleutels voor MPI-typen die moeten worden opgeslagen.

```bash
ssh-keygen -f /home/$USER/.ssh/id_rsa -t rsa -N ''
cat << EOF > /home/$USER/.ssh/config
Host *
    StrictHostKeyChecking no
EOF
cat /home/$USER/.ssh/id_rsa.pub >> /home/$USER/.ssh/authorized_keys
chmod 644 /home/$USER/.ssh/config
```

De bovenstaande syntaxis wordt ervan uitgegaan dat een gedeelde-basismap, anders .ssh directory moet worden gekopieerd naar elk knooppunt.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) op Azure.
