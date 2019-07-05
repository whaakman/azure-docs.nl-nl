---
title: Benchmark-tests voor volume, prestaties en metrische gegevens met behulp van Azure NetApp Files | Microsoft Docs
description: Benchmark-tests aanbevelingen voor het volume, prestaties en metrische gegevens met behulp van Azure NetApp bestanden bevat.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: b-juche
ms.openlocfilehash: 12ae9e313655924f11799152b5e58b77776c135c
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478808"
---
# <a name="benchmark-testing-for-volume-performance-and-metrics-using-azure-netapp-files"></a>Benchmarktests voor volumeprestaties en metrische gegevens met behulp van Azure NetApp Files

Dit artikel bevat een benchmark-tests aanbevelingen voor het volume, prestaties en metrische gegevens met behulp van Azure NetApp bestanden.

## <a name="overview"></a>Overzicht

Voor meer informatie over de prestatiekenmerken van een Azure NetApp Files-volume, kunt u het open-source-hulpprogramma [FIO](https://github.com/axboe/fio) om uit te voeren van een reeks simuleren van verschillende workloads te hanteren benchmarks. FIO kan worden geïnstalleerd op zowel Linux en Windows-besturingssystemen.  Het is een uitstekend hulpprogramma om op te halen van een momentopname van een snelle van IOPS en doorvoer voor een volume.

### <a name="vm-instance-sizing"></a>Grootte van VM-exemplaar

Zorg ervoor dat u gebruikmaakt van een virtuele machine (VM)-exemplaar dat het juiste formaat is om uit te voeren van de tests voor de beste resultaten. De volgende voorbeelden gebruiken een Standard_D32s_v3-exemplaar. Zie voor meer informatie over VM-exemplaargrootten [grootten voor Windows virtuele machines in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json) voor virtuele machines op basis van Windows en [grootten voor virtuele Linux-machines in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor virtuele machines op basis van Linux.

### <a name="azure-netapp-files-volume-sizing"></a>Grootte van de volume Azure NetApp bestanden

Zorg ervoor dat u het juiste niveau en volume quotum voor grootte van de service voor het verwachte prestatieniveau. Zie [serviceniveaus voor Azure NetApp Files](azure-netapp-files-service-levels.md) voor meer informatie.

### <a name="virtual-network-vnet-recommendations"></a>Aanbevelingen voor virtuele netwerken (VNet)

U moet de benchmark testen in hetzelfde VNet als Azure NetApp bestanden uitvoeren. In het volgende voorbeeld ziet u de aanbeveling:

![VNet-aanbevelingen](../media/azure-netapp-files/azure-netapp-files-benchmark-testing-vnet.png)

## <a name="installation-of-fio"></a>Installatie van FIO

FIO is beschikbaar in binaire indeling voor zowel Windows als Linux. Ga als volgt de sectie binaire pakketten in [FIO](https://github.com/axboe/fio) te installeren voor het platform van uw keuze.

## <a name="fio-examples-for-iops"></a>Voorbeelden van FIO voor IOPS 

De FIO voorbeelden in deze sectie gebruikt u de volgende configuratie:
* VM-exemplaargrootte: D32s_v3
* Capaciteit van toepassingen serviceniveau en de grootte: Premium / 50 TiB
* Quotum voor grootte van herstelpuntvolume: 48 TiB

De volgende voorbeelden ziet de willekeurige FIO leest en schrijft.

### <a name="fio-8k-block-size-100-random-reads"></a>FIO: 8 kB blokkeren grootte van 100% willekeurige lees

`fio --name=8krandomreads --rw=randread --direct=1 --ioengine=libaio --bs=8k --numjobs=4 --iodepth=128 --size=4G --runtime=600 --group_reporting`

### <a name="output-68k-read-iops-displayed"></a>Uitvoer: 68k gelezen IOP's weergegeven

`Starting 4 processes`  
`Jobs: 4 (f=4): [r(4)][84.4%][r=537MiB/s,w=0KiB/s][r=68.8k,w=0 IOPS][eta 00m:05s]`

### <a name="fio-8k-block-size-100-random-writes"></a>FIO: 8 kB blokkeren grootte van 100% willekeurige schrijfbewerkingen

`fio --name=8krandomwrites --rw=randwrite --direct=1 --ioengine=libaio --bs=8k --numjobs=4 --iodepth=128  --size=4G --runtime=600 --group_reporting`

### <a name="output-73k-write-iops-displayed"></a>Uitvoer: 73k schrijven IOP's weergegeven

`Starting 4 processes`  
`Jobs: 4 (f=4): [w(4)][26.7%][r=0KiB/s,w=571MiB/s][r=0,w=73.0k IOPS][eta 00m:22s]`

## <a name="fio-examples-for-bandwidth"></a>Voorbeelden van FIO voor bandbreedte

De voorbeelden in deze sectie tonen de opeenvolgende FIO leest en schrijft.

### <a name="fio-64k-block-size-100-sequential-reads"></a>FIO: 64 kB blokkeren grootte van 100% opeenvolgende leesbewerkingen

`fio --name=64kseqreads --rw=read --direct=1 --ioengine=libaio --bs=64k --numjobs=4 --iodepth=128  --size=4G --runtime=600 --group_reporting`

### <a name="output-118-gbits-throughput-displayed"></a>Uitvoer: 11,8 Gbit/s doorvoer weergegeven

`Starting 4 processes`  
`Jobs: 4 (f=4): [R(4)][40.0%][r=1313MiB/s,w=0KiB/s][r=21.0k,w=0 IOPS][eta 00m:09s]`

### <a name="fio-64k-block-size-100-sequential-writes"></a>FIO: 64 kB blokkeren grootte van 100% sequentiële schrijfbewerkingen

`fio --name=64kseqwrites --rw=write --direct=1 --ioengine=libaio --bs=64k --numjobs=4 --iodepth=128  --size=4G --runtime=600 --group_reporting`

### <a name="output-122-gbits-throughput-displayed"></a>Uitvoer: 12.2 Gbit/s doorvoer weergegeven

`Starting 4 processes`  
`Jobs: 4 (f=4): [W(4)][85.7%][r=0KiB/s,w=1356MiB/s][r=0,w=21.7k IOPS][eta 00m:02s]`

## <a name="volume-metrics"></a>Metrische gegevens over volume

Azure NetApp bestanden prestatiegegevens is beschikbaar via Azure Monitor prestatiemeteritems. De items zijn beschikbaar via de Azure portal en de REST API-GET-aanvragen. 

U kunt historische gegevens voor de volgende informatie bekijken:
* Gemiddelde latentie lezen 
* Schrijflatentie van gemiddelde 
* Gelezen IOP's (gemiddeld)
* Schrijf-IOPS (gemiddeld)
* Logische volumegrootte (gemiddeld)
* Grootte van de momentopname van volume (gemiddeld)

### <a name="using-azure-monitor"></a>Azure Monitor gebruiken 

Zoals hieronder wordt weergegeven, kunt u Azure NetApp Files items op basis van per volume op de pagina metrische gegevens openen:

![Metrische gegevens van Azure Monitor](../media/azure-netapp-files/azure-netapp-files-benchmark-monitor-metrics.png)

U kunt ook een dashboard in Azure Monitor voor Azure NetApp-bestanden maken door te gaan naar de pagina van de metrische gegevens, te filteren voor NetApp en op te geven van de volume-items van belang: 

![Azure Monitor-dashboard](../media/azure-netapp-files/azure-netapp-files-benchmark-monitor-dashboard.png)

### <a name="azure-monitor-api-access"></a>Azure Monitor-API-toegang

U kunt toegang tot Azure NetApp Files items met behulp van REST API-aanroepen. Zie [ondersteunde metrische gegevens met Azure Monitor: Microsoft.NetApp/netAppAccounts/capacityPools/Volumes](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftnetappnetappaccountscapacitypoolsvolumes) voor tellers voor de capaciteit van toepassingen en volumes.

Het volgende voorbeeld ziet een URL ophalen voor het weergeven van de grootte van logische volume:

`#get ANF volume usage`  
`curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/ANFACCOUNTGOESHERE/capacityPools/ANFPOOLGOESHERE/Volumes/ANFVOLUMEGOESHERE/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=VolumeLogicalSize`


## <a name="next-steps"></a>Volgende stappen

- [Serviceniveau's voor Azure NetApp Files](azure-netapp-files-service-levels.md)
- [Referentiepunten voor prestaties voor Azure NetApp-bestanden](azure-netapp-files-performance-benchmarks.md)