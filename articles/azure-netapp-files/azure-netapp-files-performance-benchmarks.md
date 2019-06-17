---
title: Referentiepunten voor prestaties voor Azure NetApp Files | Microsoft Docs
description: Beschrijft de resultaten van de prestaties benchmark-tests voor Azure NetApp bestanden op het volumeniveau van het.
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
ms.date: 04/22/2019
ms.author: b-juche
ms.openlocfilehash: aca0668fc364518fe45d9fe94d089ee366b25676
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64870880"
---
# <a name="performance-benchmarks-for-azure-netapp-files"></a>Prestatiebenchmarks voor Azure NetApp Files

Dit artikel beschrijft de resultaten van de prestaties benchmark-tests voor Azure NetApp bestanden op het volumeniveau van het. 

## <a name="sample-application-used-for-the-tests"></a>Voorbeeldtoepassing die wordt gebruikt voor de tests

Prestatietests zijn uitgevoerd met een voorbeeldtoepassing met behulp van Azure NetApp bestanden. De toepassing heeft de volgende kenmerken: 

* Een Linux-gebaseerde toepassing die is gebouwd voor de cloud
* Kan worden lineair geschaald met extra virtuele machines (VM's) om te verhogen compute-kracht, indien nodig
* Snelle toegankelijkheid van de data lake is vereist
* I/o-patronen die soms willekeurige en soms sequentiële heeft 
    * Een willekeurige patroon is de lage latentie voor grote hoeveelheden i/o vereist. 
    * Een opeenvolgend patroon is vereist voor grote hoeveelheden bandbreedte. 

## <a name="about-the-workload-generator"></a>Over de generator werkbelasting

De resultaten zijn afkomstig van Vdbench samenvatting bestanden. [Vdbench](https://www.oracle.com/technetwork/server-storage/vdbench-downloads-1901681.html) is een opdrachtregelprogramma waarmee schijf-i/o-werkbelastingen voor het valideren van de prestaties van opslagruimte worden gegenereerd. De configuratie van de client-server gebruikt, is schaalbaar.  Het bevat een enkele gemengde master/client en 14 toegewezen client VM's.

## <a name="about-the-tests"></a>Over de tests

De tests zijn ontworpen voor het identificeren van de limieten die mogelijk van toepassing zijn op de voorbeeldtoepassing en de reactietijd die curven tot aan de limieten.  

De volgende tests zijn uitgevoerd: 

* 100% 8 KiB willekeurige lezen
* 100% 8 KiB willekeurig schrijven
* 100% 64 KiB sequentieel lezen
* 100% 64 KiB sequentiële schrijfbewerkingen
* 50% 64 KiB opeenvolgende lees-, 50% 64 KiB opeenvolgende schrijven
* 50% 8 KiB willekeurige lees-, 50% 8 KiB willekeurig schrijven

## <a name="bandwidth"></a>Bandbreedte

Azure Files met NetApp biedt meerdere [serviceniveaus](azure-netapp-files-service-levels.md). Elk serviceniveau biedt een andere hoeveelheid bandbreedte per TiB ingerichte capaciteit (volume quotum). De limiet voor bandbreedte voor een volume wordt ingericht op basis van de combinatie van het serviceniveau en het volume-quotum. Houd er rekening mee dat de limiet voor bandbreedte slechts één factor is bij het bepalen van de werkelijke hoeveelheid doorvoer die zal worden gerealiseerd.  

4\.500 MiB is momenteel de hoogste doorvoer die door een workload op basis van een enkel volume in de testfase is bereikt.  Met het serviceniveau Premium, wordt een volume-quotum van 70.31 TiB voldoende bandbreedte om te realiseren van deze doorvoer per de berekening van de onderstaande inrichten: 

![Bandbreedte-formule](../media/azure-netapp-files/azure-netapp-files-bandwidth-formula.png)

![Quotum en het serviceniveau](../media/azure-netapp-files/azure-netapp-files-quota-service-level.png)

## <a name="throughput-intensive-workloads"></a>Doorvoer-intensieve workloads

De test doorvoer gebruikt Vdbench en een combinatie van 12xD32s V3 storage-VM's. De voorbeeld-volume in de test bereikt de volgende doorvoerwaarden:

![Doorvoer testen](../media/azure-netapp-files/azure-netapp-files-throughput-test.png)

## <a name="io-intensive-workloads"></a>I/O-intensieve workloads

De i/o-test gebruikt Vdbench en een combinatie van 12xD32s V3 storage-VM's. De voorbeeld-volume in de test bereikt de volgende i/o-getallen:

![I/o-test](../media/azure-netapp-files/azure-netapp-files-io-test.png)

## <a name="latency"></a>Latentie

De afstand tussen de test-VM's en het Azure NetApp Files-volume heeft een invloed op de i/o-prestaties.  De onderstaande tabel vergelijkt de IOPS-waarde ten opzichte van latentie respons-curven voor twee verschillende sets virtuele machines.  Een set van virtuele machines is in de buurt van Azure NetApp bestanden en de andere set verdere is afwezig.  Houd er rekening mee dat de wachttijd voor de verdere set van virtuele machines heeft dit gevolgen voor de hoeveelheid IOP's op een bepaald niveau van parallelle uitvoering behaald.  Leesbewerkingen op basis van een volume kunnen ongeacht 300.000 IOPS overschrijden, zoals hieronder weergegeven: 

![Latentie van onderzoek](../media/azure-netapp-files/azure-netapp-files-latency-study.png)

## <a name="summary"></a>Samenvatting

Latentie van gevoelige werkbelastingen (databases) kunnen een één milliseconde reactietijd hebben. De transactionele uitvoering kan meer dan 300 kB IOPS zijn voor één volume.

Doorvoer-gevoelige-toepassingen (voor het streamen en imaging) kunnen hebben 4.5GiB / s-doorvoer.
