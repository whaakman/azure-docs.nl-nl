---
title: Bekende problemen met HB-serie en HC-serie VM's - Azure Virtual Machines | Microsoft Docs
description: Meer informatie over bekende problemen met HB-serie VM-grootten in Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/07/2019
ms.author: amverma
ms.openlocfilehash: 8d4b57fb2fee3849e102868c86fe3cab465fc70d
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707783"
---
# <a name="known-issues-with-hb-series-and-hc-series-vms"></a>Bekende problemen met VM's uit de HB-serie en HC-serie

In dit artikel bevat de meest voorkomende problemen en oplossingen bij het gebruik van HB-serie en HC-serie VM's.

## <a name="dram-on-hb-series"></a>DRAM op HB-serie

HB-serie VM's kunnen alleen 228 GB aan RAM-geheugen op de Gast-VM's op dit moment beschikbaar. Dit komt door een bekende beperking van Azure-hypervisor om te voorkomen dat wordt toegewezen aan de lokale DRAM van AMD CCX van (NUMA domeinen) gereserveerd voor de Gast-VM's.

## <a name="accelerated-networking"></a>Versneld netwerken

Versneld netwerken in Azure is niet ingeschakeld op dit moment, maar wordt als wij via de Preview-periode. Wij stellen klanten wanneer deze functie wordt ondersteund.

## <a name="qp0-access-restriction"></a>qp0 toegangsbeperking

0 is niet toegankelijk is voor de Gast-VM's om te voorkomen dat de toegang van laag niveau hardware tot beveiligingsproblemen, wachtrij-paar leiden kan. Dit moet alleen van invloed op acties doorgaans die zijn gekoppeld aan het beheer van de NIC ConnectX-5 en het uitvoeren van sommige diagnostische InfiniBand, zoals ibdiagnet, maar geen toepassingen voor eindgebruikers zelf.

## <a name="ud-transport"></a>UD Transport

Bij het starten ondersteunen de HB - en HC-serie geen dynamisch verbonden Transport (DCT). Ondersteuning voor DCT worden na verloop van tijd uitgevoerd. Betrouwbare verbinding (RC) en onbetrouwbare Datagram (UD) transporten worden ondersteund.

## <a name="gss-proxy"></a>GSS-Proxy

GSS Proxy heeft een bekend probleem in CentOS/RHEL 7.5 die als een aanzienlijke prestaties en reactiesnelheid boete bij gebruik met NFS kunt manifest. Dit kan grotendeels worden opgevangen met:

```console
sed -i 's/GSS_USE_PROXY="yes"/GSS_USE_PROXY="no"/g' /etc/sysconfig/nfs
```

## <a name="cache-cleaning"></a>Cache opschonen

Op HPC-systemen is het vaak nuttig zijn voor het opschonen van het geheugen, nadat een taak is voltooid voordat de volgende gebruiker hetzelfde knooppunt is toegewezen. Na het uitvoeren van toepassingen in Linux merkt u misschien dat uw beschikbare geheugen terwijl uw buffer geheugen toeneemt vermindert, ondanks de toepassingen die niet wordt uitgevoerd.

![Schermafbeelding van de opdrachtprompt](./media/known-issues/cache-cleaning-1.png)

Met behulp van `numactl -H` ziet u welke NUMAnode(s) het geheugen de buffer wordt opgeslagen met (eventueel alle). In Linux, op gebruikers te ruimen de caches in drie manieren om terug te keren naar de buffer geschreven of in de cache opgeslagen geheugen met 'gratis'. U moet dit de hoofdmap of sudo-machtigingen hebben.

```console
echo 1 > /proc/sys/vm/drop_caches [frees page-cache]
echo 2 > /proc/sys/vm/drop_caches [frees slab objects e.g. dentries, inodes]
echo 3 > /proc/sys/vm/drop_caches [cleans page-cache and slab objects]
```

![Schermafbeelding van de opdrachtprompt](./media/known-issues/cache-cleaning-2.png)

## <a name="kernel-warnings"></a>Kernel-waarschuwingen

Mogelijk ziet u de volgende kernel-waarschuwingsberichten bij het opstarten van een VM HB-serie onder Linux.

```console
[  0.004000] WARNING: CPU: 4 PID: 0 at arch/x86/kernel/smpboot.c:376 topology_sane.isra.3+0x80/0x90
[  0.004000] sched: CPU #4's llc-sibling CPU #0 is not on the same node! [node: 1 != 0]. Ignoring dependency.
[  0.004000] Modules linked in:
[  0.004000] CPU: 4 PID: 0 Comm: swapper/4 Not tainted 3.10.0-957.el7.x86_64 #1
[  0.004000] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007 05/18/2018
[  0.004000] Call Trace:
[  0.004000] [<ffffffffb8361dc1>] dump_stack+0x19/0x1b
[  0.004000] [<ffffffffb7c97648>] __warn+0xd8/0x100
[  0.004000] [<ffffffffb7c976cf>] warn_slowpath_fmt+0x5f/0x80
[  0.004000] [<ffffffffb7c02b34>] ? calibrate_delay+0x3e4/0x8b0
[  0.004000] [<ffffffffb7c574c0>] topology_sane.isra.3+0x80/0x90
[  0.004000] [<ffffffffb7c57782>] set_cpu_sibling_map+0x172/0x5b0
[  0.004000] [<ffffffffb7c57ce1>] start_secondary+0x121/0x270
[  0.004000] [<ffffffffb7c000d5>] start_cpu+0x5/0x14
[  0.004000] ---[ end trace 73fc0e0825d4ca1f ]---
```

U kunt deze waarschuwing negeren. Dit komt door een bekende beperking van de Azure-hypervisor die na verloop van tijd wordt opgelost.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [high performance computing](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) in Azure.
