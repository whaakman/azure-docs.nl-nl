---
title: Metrische gegevens voor Azure NetApp-bestanden | Microsoft Docs
description: Beschrijving van metrische gegevens voor Azure NetApp-bestanden.
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
ms.date: 02/15/2019
ms.author: b-juche
ms.openlocfilehash: 1563b60ef26ac5e4d40f45095d0109dd9dd71570
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56672573"
---
# <a name="metrics-for-azure-netapp-files"></a>Metrische gegevens voor Azure NetApp Files

Azure Files met NetApp biedt metrische gegevens over toegewezen opslag, werkelijke opslaggebruik volume doorvoer, IOP's en latentie. Door deze metrische gegevens analyseren, krijgt u een beter begrip van de gebruik patroon en volume de prestaties van uw accounts NetApp.  

## <a name="capacity_pools"></a>Metrische gegevens voor capaciteit van toepassingen

- *De grootte van de groep toegewezen volume*  
    Dit is de grootte (GiB) van de ingerichte capaciteit van toepassingen.  
- *Toegewezen volume van toepassingen die worden gebruikt*  
    Dit is het totaal van volume quotum (GiB) in een opgegeven capaciteit van toepassingen (dat wil zeggen, het totaal van de ingerichte grootte de volumes in de capaciteit van toepassingen). Dit is de grootte die u hebt geselecteerd tijdens het maken van volume.  
- *Totaal aantal logische grootte van volume van toepassingen*  
    Dit is het totaal van alle volumes in een pool capaciteit gebruikt logische ruimte (GiB).  
- *Totale momentopname van de volumegrootte van toepassingen*  
    Dit is het totaal van incrementele logische ruimte die wordt gebruikt door de momentopnamen.  

## <a name="volumes"></a>Metrische gegevens over gebruik voor volumes

- *Volume toegewezen grootte*   
    Dit is de volumegrootte (quotum) dat is ingericht in GiB.  
- *De logische grootte van volume*   
    Dit is de totale logische ruimte die wordt gebruikt in een volume (GiB). Deze grootte bevat logische ruimte die wordt gebruikt door actieve bestandssystemen en momentopnamen.  
- *De grootte van de momentopname van volume*   
    Dit is de incrementele logische ruimte die wordt gebruikt door momentopnamen in een volume.  

## <a name="next-steps"></a>Volgende stappen

* [Inzicht in de hiërarchie van de opslag van Azure NetApp bestanden](azure-netapp-files-understand-storage-hierarchy.md)
* [Een capaciteitspool instellen](azure-netapp-files-set-up-capacity-pool.md)
* [Een volume maken voor Azure NetApp Files](azure-netapp-files-create-volumes.md)
