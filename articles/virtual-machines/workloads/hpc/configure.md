---
title: High Performance Computing - virtuele Machines van Azure | Microsoft Docs
description: Meer informatie over High Performance Computing in Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/07/2019
ms.author: amverma
ms.openlocfilehash: e8ff4147130dfeff14be41ed292b51ed34966df0
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537676"
---
# <a name="optimization-for-linux"></a>Optimalisatie voor Linux

In dit artikel ziet u enkele belangrijke technieken voor het optimaliseren van uw installatiekopie van het besturingssysteem. Meer informatie over [InfiniBand inschakelen](enable-infiniband.md) en het optimaliseren van de installatiekopieën van het besturingssysteem.

## <a name="update-lis"></a>LIS bijwerken

Als u implementeert met behulp van een aangepaste installatiekopie (bijvoorbeeld een ouder besturingssysteem, zoals CentOS/RHEL 7.4 of 7.5), werkt u LIS op de virtuele machine.

```bash
wget https://aka.ms/lis
tar xzf lis
pushd LISISO
./upgrade.sh
```

## <a name="reclaim-memory"></a>Vrijmaken van geheugen

Verbeter de efficiëntie door het vrijmaken van geheugen om te voorkomen dat externe geheugentoegang automatisch.

```bash
echo 1 >/proc/sys/vm/zone_reclaim_mode
```

Om te maken nadat de virtuele machine opnieuw wordt opgestart behouden:

```bash
echo "vm.zone_reclaim_mode = 1" >> /etc/sysctl.conf sysctl -p
```

## <a name="disable-firewall-and-selinux"></a>Firewall- en SELinux uitschakelen

Firewall- en SELinux uitschakelen.

```bash
systemctl stop iptables.service
systemctl disable iptables.service
systemctl mask firewalld
systemctl stop firewalld.service
systemctl disable firewalld.service
iptables -nL
sed -i -e's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

## <a name="disable-cpupower"></a>Cpupower uitschakelen

Cpupower uitschakelen.

```bash
service cpupower status
if enabled, disable it:
service cpupower stop
sudo systemctl disable cpupower
```

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [InfiniBand inschakelen](enable-infiniband.md) en het optimaliseren van installatiekopieën van het besturingssysteem.

* Meer informatie over [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) op Azure.
