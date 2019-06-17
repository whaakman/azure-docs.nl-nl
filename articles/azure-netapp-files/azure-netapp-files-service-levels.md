---
title: Serviceniveaus voor Azure NetApp Files | Microsoft Docs
description: Beschrijving van doorvoerprestaties voor het niveau van de service van Azure NetApp bestanden.
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
ms.openlocfilehash: 1f9c427045c9d42f6a11cc4bcc798cfc47a4428c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65523111"
---
# <a name="service-levels-for-azure-netapp-files"></a>Serviceniveau's voor Azure NetApp Files
Serviceniveaus zijn een kenmerk van een capaciteit van toepassingen. Serviceniveaus zijn gedefinieerd en onderscheiden aan de hand van de toegestane maximale doorvoer voor een volume in de capaciteit van toepassingen op basis van het quotum dat is toegewezen aan het volume.

## <a name="supported-service-levels"></a>Ondersteunde serviceniveaus

Azure NetApp-bestanden ondersteunt drie serviceniveaus: *Ultra*, *Premium*, en *Standard*. 

* <a name="Ultra"></a>Ultra-opslag

    De Ultra storage-laag heeft maximaal 128 MiB/s aan doorvoer per 1 TiB volume quota die zijn toegewezen. 

* <a name="Premium"></a>Premium-opslag

    De Premium storage-laag heeft maximaal 64 MiB/s aan doorvoer per 1 TiB volume quota die zijn toegewezen. 

* <a name="Standard"></a>Standard-opslag

    De prijscategorie Standard-opslag biedt tot 16 MiB/s aan doorvoer per 1 TiB volume quota die zijn toegewezen.

## <a name="throughput-limits"></a>Doorvoerlimieten

De maximale doorvoer voor een volume wordt bepaald door de combinatie van de volgende factoren:
* Het serviceniveau van de capaciteit van toepassingen die het volume behoort
* Het quotum dat is toegewezen aan het volume  

Dit concept wordt weergegeven in het onderstaande diagram:

![Service level afbeelding](../media/azure-netapp-files/azure-netapp-files-service-levels.png)

In het bovenstaande voorbeeld 1, een volume van een capaciteit van toepassingen met de Premium storage-laag die is toegewezen 2 TiB quotum wordt toegewezen doorvoer van maximaal 128 MiB/s (2 TiB * 64 MiB/s). In dit scenario is van toepassing, ongeacht de grootte van de capaciteit van toepassingen of het daadwerkelijke volume verbruik.

In voorbeeld 2 hierboven, een volume van een capaciteit van toepassingen met de Premium storage-laag die is toegewezen 100 GiB van quota worden toegewezen een doorvoerlimiet van 6.25 MiB/s (0.09765625 TiB * 64 MiB/s). In dit scenario is van toepassing, ongeacht de grootte van de capaciteit van toepassingen of het daadwerkelijke volume verbruik.

## <a name="next-steps"></a>Volgende stappen

- Bekijk de [pagina met prijzen van Azure NetApp Files](https://azure.microsoft.com/pricing/details/storage/netapp/) voor informatie over de prijs van verschillende serviceniveaus
- Zie [Kostenmodel voor Azure NetApp Files](azure-netapp-files-cost-model.md) voor de berekening van het capaciteitsverbruik in een capaciteit van toepassingen 
- [Een capaciteitspool instellen](azure-netapp-files-set-up-capacity-pool.md)