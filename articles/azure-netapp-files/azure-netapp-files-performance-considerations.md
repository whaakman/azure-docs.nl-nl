---
title: Prestatie-overwegingen voor Azure NetApp Files | Microsoft Docs
description: Beschrijving van prestatie-overwegingen voor Azure NetApp bestanden.
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
ms.date: 06/25/2019
ms.author: b-juche
ms.openlocfilehash: 97e3c6212edd2ade4eabb96db3543e9b3b68e2ae
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454137"
---
# <a name="performance-considerations-for-azure-netapp-files"></a>Prestatie-overwegingen voor Azure NetApp Files

De [doorvoerlimiet](azure-netapp-files-service-levels.md) voor een volume wordt bepaald door een combinatie van het quotum dat is toegewezen aan het volume en de service level geselecteerd. Wanneer u Prestatieplannen over Azure NetApp bestanden maken, moet u inzicht in verschillende overwegingen. 

## <a name="quota-and-throughput"></a>Quota en doorvoer  

De doorvoerlimiet voor de is slechts één factor van de werkelijke prestaties die zal worden gerealiseerd.  

Prestatieoverwegingen voor typische opslag, met inbegrip van lezen en schrijven combinatie, de grootte, willekeurige of opeenvolgende patronen en vele andere factoren dragen bij aan de totale prestaties die worden geleverd.  

De maximale empirische doorvoer die is waargenomen in de testfase is 4.500 MiB/s.  Op de Premium storage-laag wordt een volume-quotum van 70.31 TiB een doorvoerlimiet hoog genoeg is voor dit niveau van de prestaties van een ingericht.  

Als u volume quotum bedragen dan 70.31 TiB toewijst overweegt, kan extra quota worden toegewezen aan een volume voor het opslaan van aanvullende gegevens. Het toegevoegde quotum wordt echter niet resulteren in een verdere toename van de werkelijke doorvoer.  

Zie [prestaties benchmarks voor Azure NetApp Files](azure-netapp-files-performance-benchmarks.md) voor meer informatie.

## <a name="overprovisioning-the-volume-quota"></a>Overmatige inrichting van het volume-quotum

Als de prestaties van de workload doorvoerlimiet afhankelijk is, is het mogelijk om te overprovision van het volume-quotum voor het instellen van een hogere doorvoer en betere prestaties.  

Bijvoorbeeld, als een volume in de Premium storage-laag slechts 500 GiB van gegevens heeft, maar is 128 MiB/s aan doorvoer vereist, kunt u instellen de quota in 2 TiB zodat het doorvoerniveau van de dienovereenkomstig is ingesteld (64 MiB/s per TB * 2 TiB = 128 MiB/s).  

Als u een volume voor het bereiken van een hogere doorvoer consistent overprovision, kunt u overwegen een hoger serviceniveau in plaats daarvan.  In het bovenstaande voorbeeld kunt u de dezelfde doorvoerlimiet met de helft van het volume-quotum bereiken met behulp van de Ultra opslaglaag in plaats daarvan (128 MiB/s per TiB * 1 TiB = 128 MiB/s).

## <a name="dynamically-increasing-or-decreasing-volume-quota"></a>Dynamisch verhogen of verlagen volume quotum

Als uw prestatievereisten tijdelijke aard is, of als u beschikt over meer prestatiebehoeften voor een vaste periode, u dynamisch kunt vergroten of verkleinen van volume quota om aan te passen onmiddellijk de maximale doorvoer.  Houd rekening met de volgende overwegingen: 

* Volume quota kan worden vergroot of verkleind hoeft u geen i/o onderbreken en toegang tot het volume niet wordt onderbroken of is beïnvloed.  

    U kunt het quotum aanpassen tijdens een actieve i/o-transactie op basis van een volume.  Houd er rekening mee dat volume quotum kan nooit onder het bedrag aan logische gegevens die zijn opgeslagen in het volume worden verlaagd.

* Wanneer het quotum is gewijzigd, is de overeenkomstige wijziging in doorvoerlimiet nagenoeg onmiddellijke. 

    De wijziging niet onderbreken of invloed op de volumetoegang of i/o.  

* Aanpassen volume quotum vereist een wijziging in de grootte van de capaciteit van toepassingen.  

    De grootte van de capaciteit van toepassingen kan worden aangepast dynamisch en zonder enige impact op de beschikbaarheid van volumes of i/o.

## <a name="next-steps"></a>Volgende stappen

- [Serviceniveau's voor Azure NetApp Files](azure-netapp-files-service-levels.md)
- [Referentiepunten voor prestaties voor Azure NetApp-bestanden](azure-netapp-files-performance-benchmarks.md)