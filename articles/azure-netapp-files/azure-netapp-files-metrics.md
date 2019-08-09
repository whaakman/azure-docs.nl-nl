---
title: Metrische gegevens voor Azure NetApp Files | Microsoft Docs
description: Beschrijft de metrische gegevens voor Azure NetApp Files.
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
ms.date: 08/07/2019
ms.author: b-juche
ms.openlocfilehash: 7cf382f511d2ba8452d77bf207f36b749cb31e94
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848792"
---
# <a name="metrics-for-azure-netapp-files"></a>Metrische gegevens voor Azure NetApp Files

Azure NetApp Files voorziet in metrische gegevens over de toegewezen opslag, het werkelijke gebruik van de opslag, de door Voer van het volume, IOPS en latentie. Door deze metrische gegevens te analyseren, kunt u een beter inzicht krijgen in het gebruiks patroon en de volume prestaties van uw NetApp-accounts.  

## <a name="capacity_pools"></a>Metrische gegevens over gebruik voor capaciteits Pools

<!-- 
- *Volume pool allocated size*  
    The size (GiB) of the provisioned capacity pool  
--> 
- *Gebruikte volume groep*  
    Het totale volume quotum (GiB) in een opgegeven capaciteits groep (dat wil zeggen, het totaal van de ingerichte grootte van het volume in de capaciteits groep)  
    Dit is het formaat dat u hebt geselecteerd tijdens het maken van het volume.  
- *Totale logische grootte van volume groep*  
    Het totale aantal logische ruimte (GiB) dat wordt gebruikt voor alle volumes in een capaciteits groep  
<!-- 
- *Volume pool total snapshot size*  
    The total of incremental logical space used by the snapshots  
-->

## <a name="volumes"></a>Metrische gegevens over het gebruik voor volumes

<!-- 
- *Volume allocated size*   
    The volume size (quota) provisioned in GiB  
--> 
- *Logische volume grootte*   
    De totale logische ruimte die wordt gebruikt in een volume (GiB)  
    Deze grootte bevat logische ruimte die wordt gebruikt door actieve bestands systemen en moment opnamen.  
- *Grootte van moment opname van volume*   
   De incrementele logische ruimte die wordt gebruikt voor moment opnamen in een volume  

## <a name="performance-metrics-for-volumes"></a>Prestatie gegevens voor volumes

- *AverageReadLatency*   
    De gemiddelde tijd voor lees bewerkingen van het volume in milliseconden
- *AverageWriteLatency*   
    De gemiddelde tijd voor het schrijven van het volume in milliseconden
- *ReadIops*   
    Het aantal lees bewerkingen naar het volume per seconde
- *WriteIops*   
    Het aantal schrijf bewerkingen naar het volume per seconde

## <a name="next-steps"></a>Volgende stappen

* [Informatie over de opslaghiërarchie van Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Een capaciteitspool instellen](azure-netapp-files-set-up-capacity-pool.md)
* [Een volume maken voor Azure NetApp Files](azure-netapp-files-create-volumes.md)
