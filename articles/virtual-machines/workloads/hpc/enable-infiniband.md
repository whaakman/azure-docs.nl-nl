---
title: Inschakelen van InifinBand met SR-IOV - virtuele Machines van Azure | Microsoft Docs
description: Informatie over het inschakelen van InfiniBand met SR-IOV.
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
ms.openlocfilehash: 879b1eed7bf4778d4d49f6f991d6d74214d33823
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537652"
---
# <a name="enable-infiniband-with-sr-iov"></a>InfiniBand met SR-IOV inschakelen


De eenvoudigste en aanbevolen methode om uw aangepaste VM-installatiekopie met InfiniBand (IB) is de InfiniBandDriverLinux of InfiniBandDriverWindows VM-extensie toevoegen aan uw implementatie.
Informatie over het gebruik van deze VM-extensies met [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc#rdma-capable-instances) en [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc#rdma-capable-instances)

Voor het handmatig configureren van InfiniBand op SR-IOV is ingeschakeld virtuele machines (momenteel HB en HC-serie), volg de onderstaande stappen. Deze stappen zijn alleen voor RHEL/CentOS. Voor Ubuntu (16.04 en 18.04) en SLES (12 SP4 en 15) werken de meegeleverde stuurprogramma's goed. Voor Ubuntu, 


## <a name="manually-install-ofed"></a>OFED handmatig installeren

Installeer de nieuwste MLNX_OFED-stuurprogramma's voor ConnectX-5 van [Mellanox](https://www.mellanox.com/page/products_dyn?product_family=26).

Voor RHEL/CentOS (voorbeeld hieronder voor 7.6):
```bash
sudo yum install -y kernel-devel python-devel
sudo yum install -y redhat-rpm-config rpm-build gcc-gfortran gcc-c++
sudo yum install -y gtk2 atk cairo tcl tk createrepo
wget --retry-connrefused --tries=3 --waitretry=5 http://content.mellanox.com/ofed/MLNX_OFED-4.5-1.0.1.0/MLNX_OFED_LINUX-4.5-1.0.1.0-rhel7.6-x86_64.tgz
tar zxvf MLNX_OFED_LINUX-4.5-1.0.1.0-rhel7.6-x86_64.tgz
sudo ./MLNX_OFED_LINUX-4.5-1.0.1.0-rhel7.6-x86_64/mlnxofedinstall --add-kernel-support
```

Voor Windows, download en installeer de WinOF-2-stuurprogramma's voor ConnectX-5 van [Mellanox](https://www.mellanox.com/page/products_dyn?product_family=32&menu_section=34)

## <a name="enable-ipoib"></a>IPoIB inschakelen

```bash
sudo sed -i 's/LOAD_EIPOIB=no/LOAD_EIPOIB=yes/g' /etc/infiniband/openib.conf
sudo /etc/init.d/openibd restart
if [ $? -eq 1 ]
then
  sudo modprobe -rv  ib_isert rpcrdma ib_srpt
  sudo /etc/init.d/openibd restart
fi
```

## <a name="assign-an-ip-address"></a>Een IP-adres toewijzen

Een IP-adres toewijzen aan de interface ib0, met behulp van:

- IP-adres aan de Interface ib0 handmatig toewijzen (als root).

    ```bash
    ifconfig ib0 $(sed '/rdmaIPv4Address=/!d;s/.*rdmaIPv4Address="\([0-9.]*\)".*/\1/' /var/lib/waagent/SharedConfig.xml)/16
    ```

OF

- Gebruik WALinuxAgent voor IP-adres toewijzen en maken deze behouden.

    ```bash
    yum install -y epel-release
    yum install -y python-pip
    python -m pip install --upgrade pip setuptools wheel
    wget "https://github.com/Azure/WALinuxAgent/archive/release-2.2.36.zip"
    unzip release-2.2.36.zip
    cd WALinuxAgent*
    python setup.py install --register-service --force
    sed -i -e 's/# OS.EnableRDMA=y/OS.EnableRDMA=y/g' /etc/waagent.conf
    sed -i -e 's/# AutoUpdate.Enabled=y/AutoUpdate.Enabled=y/g' /etc/waagent.conf
    systemctl restart waagent
    ```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) op Azure.
