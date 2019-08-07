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
ms.date: 08/06/2019
ms.author: b-juche
ms.openlocfilehash: 9a56fb27fdf9e196291942041d68b249d7f16648
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68839247"
---
# <a name="metrics-for-azure-netapp-files"></a>Metrische gegevens voor Azure NetApp Files

Azure NetApp Files voorziet in metrische gegevens over de toegewezen opslag, het werkelijke gebruik van de opslag, de door Voer van het volume, IOPS en latentie. Door deze metrische gegevens te analyseren, kunt u een beter inzicht krijgen in het gebruiks patroon en de volume prestaties van uw NetApp-accounts.  

## <a name="capacity_pools"></a>Metrische gegevens over gebruik voor capaciteits Pools

<!-- 
- *Volume pool allocated size*  
    This is the size (GiB) of the provisioned capacity pool.  
--> 
- *Gebruikte volume groep*  
    Dit is het totale volume quotum (GiB) in een bepaalde capaciteits groep (dat wil zeggen, het totaal van de ingerichte grootte van het volume in de capaciteits pool). Dit is het formaat dat u hebt geselecteerd tijdens het maken van het volume.  
- *Totale logische grootte van volume groep*  
    Dit is het totale aantal logische ruimte (GiB) dat wordt gebruikt voor alle volumes in een capaciteits groep.  
<!-- 
- *Volume pool total snapshot size*  
    This is the total of incremental logical space used by the snapshots.  
-->

## <a name="volumes"></a>Metrische gegevens over het gebruik voor volumes

<!-- 
- *Volume allocated size*   
    This is the volume size (quota) provisioned in GiB.  
--> 
- *Logische volume grootte*   
    Dit is de totale logische ruimte die wordt gebruikt in een volume (GiB). Deze grootte bevat logische ruimte die wordt gebruikt door actieve bestands systemen en moment opnamen.  
- *Grootte van moment opname van volume*   
    Dit is de incrementele logische ruimte die wordt gebruikt voor moment opnamen in een volume.  

## <a name="next-steps"></a>Volgende stappen

* [Informatie over de opslaghiërarchie van Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Een capaciteitspool instellen](azure-netapp-files-set-up-capacity-pool.md)
* [Een volume maken voor Azure NetApp Files](azure-netapp-files-create-volumes.md)
