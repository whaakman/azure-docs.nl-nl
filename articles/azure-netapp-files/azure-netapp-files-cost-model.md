---
title: Model met kosten van Azure NetApp Files | Microsoft Docs
description: Beschrijft het Kostenmodel voor Azure NetApp bestanden voor het beheren van kosten van de service.
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
ms.date: 05/01/2019
ms.author: b-juche
ms.openlocfilehash: b06e3366224b90899dd3f9f9439edf897de82794
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65524218"
---
# <a name="cost-model-for-azure-netapp-files"></a>Kostenmodel voor Azure NetApp Files 

Inzicht krijgen in het Kostenmodel voor Azure NetApp-bestanden, kunt u uw kosten beheren van de service.

## <a name="calculation-of-capacity-consumption"></a>Berekening van het capaciteitsverbruik

Azure Files met NetApp wordt in rekening gebracht op de capaciteit van de ingerichte opslag.  Ingerichte capaciteit is toegewezen door het maken van pools van capaciteit.  Capaciteit van toepassingen worden in rekening gebracht op basis van $/ ingericht-GiB/maand per uur. De minimale grootte voor een groep één capaciteit is 4 TiB en capaciteit groepen kunnen vervolgens worden uitgebreid in stappen van 1 TiB. Volumes worden gemaakt binnen de capaciteit van toepassingen.  Elk volume een quotum is toegewezen dat afloopt in stappen van de capaciteit van toepassingen is ingericht. De quota die kan worden toegewezen aan volumes bereiken van een minimum van 100 GiB tot een maximum van 92 TiB.  

Capaciteitsgebruik meegeteld voor het quotum is voor een actieve volume gebaseerd op logische (effectieve) capaciteit.

Als het verbruik van de werkelijke capaciteit van een volume het opslagquotum overschrijdt, moet het volume kunt blijven groeien. Als de grootte van het daadwerkelijke volume kleiner dan de systeemlimiet (100 TiB is) worden nog steeds schrijfbewerkingen toegestaan.  

Het totale aantal gebruikte capaciteit in een capaciteit van toepassingen op basis van de ingerichte bedrag is de som van de grootste van het toegewezen quotum of het werkelijke gebruik van alle volumes in de pool: 

   ![Berekening van het totale aantal gebruikte capaciteit](../media/azure-netapp-files/azure-netapp-files-total-used-capacity.png)

Het volgende diagram illustreert deze concepten.  
* We hebben een capaciteit van toepassingen met 4 TiB ingerichte capaciteit.  De groep bevat drie volumes.  
    * Volume 1 een quotum van 2 TiB is toegewezen en er 800 GiB van verbruik.  
    * Volume 2 een quotum van 1 TiB is toegewezen en heeft 100 GiB van verbruik.  
    * 3 volume een quotum van 500 GiB is toegewezen, maar heeft 800 GiB van verbruik (overschrijding).  
* De capaciteit van toepassingen wordt gemeten voor 4 TiB aan capaciteit (het ingerichte bedrag).  
    3,8 TiB aan capaciteit is verbruikt (2 TiB en 1 TiB quotum van Volumes 1 en 2 en 800 GiB van het werkelijke verbruik voor Volume 3). En 200 GiB van de capaciteit is overgebleven.

   ![Capaciteit van toepassingen met drie volumes](../media/azure-netapp-files/azure-netapp-files-capacity-pool-with-three-vols.png)

## <a name="overage-in-capacity-consumption"></a>Overschrijding van het capaciteitsverbruik  

Wanneer de totale capaciteit van een pool gebruikt de ingerichte capaciteit overschrijdt, schrijven van gegevens nog steeds zijn toegestaan.  Nadat de respijtperiode (1 uur) als de gebruikte capaciteit van de pool nog steeds de ingerichte capaciteit overschrijdt, klikt u vervolgens de poolgrootte automatisch verhoogd in stappen van 1 TiB totdat de ingerichte capaciteit groter dan het totale aantal gebruikte capaciteit is.  Bijvoorbeeld, in de afbeelding hierboven, als Volume 3 blijft toenemen en het werkelijke verbruik 1.2 TiB bereikt vervolgens nadat de respijtperiode is verlopen en de groep wordt automatisch worden uitgebreid tot 5 TiB.  Het resultaat is dat de capaciteit van de ingerichte objectgroep (5 TiB) groter is dan de gebruikte capaciteit (4.2 TiB).  

## <a name="manual-changes-of-the-pool-size"></a>Handmatige wijzigingen van de grootte van de groep  

U kunt handmatig vergroten of verkleinen van de grootte van de groep. Echter, de volgende beperkingen zijn van toepassing:
* Minimum- en servicebeperkingen  
    Zie het artikel over [resourcelimieten](azure-netapp-files-resource-limits.md).
* Een interval van 1 TiB na de eerste 4-TiB minimumaankoopprijs
* Minimale facturering interval van één uur
* Grootte van de ingerichte groep kan niet worden verkleind tot minder dan de totale capaciteit gebruikt in de groep.

## <a name="behavior-of-maximum-size-pool-overage"></a>Gedrag van overschrijding van de maximale grootte van toepassingen   

De maximale grootte van een capaciteit van toepassingen die u kunt maken of het formaat is 500 TiB.  Wanneer de totale capaciteit in een pool capaciteit gebruikt overschrijdt 500 TiB, treden de volgende situaties:
* Schrijven van gegevens wordt nog steeds worden toegestaan (als het volume dan het Systeemmaximum van 100 TiB is).
* Nadat de respijtperiode van één uur de pool automatisch wordt gewijzigd in 1 TiB stapsgewijs is verhoogd, totdat de groepscapaciteit ingericht totale gebruikte capaciteit overschrijdt.
* De extra ingericht en capaciteit die van meer dan 500 TiB kan niet worden gebruikt om toe te wijzen volume quotum in rekening gebracht. Deze worden ook niet gebruikt om uit te breiden prestatielimieten QoS.  
    Zie [serviceniveaus](azure-netapp-files-service-levels.md) over prestatielimieten en QoS-grootte.

Het volgende diagram illustreert deze concepten:
* We hebben een capaciteit van toepassingen met een Premium storage-laag en een 500 TiB-capaciteit. De groep bevat negen volumes.
    * Volumes 1 tot en met 8 zijn een quotum van 60 TiB toegewezen.  Het totale aantal gebruikte capaciteit is 480 TiB.  
        Elk volume heeft een QoS-limiet van 3,75 GiB/s aan doorvoer (60 TiB * 64 MiB/s).  
    * Volume 9 is een quotum van 20 TiB toegewezen.  
        Volume 9 heeft een QoS-limiet van 1,25 GiB/s aan doorvoer (60 TiB * 64 MiB/s).
* Volume 9 is een overschrijding scenario. 25 TiB van het werkelijke verbruik heeft.  
    * Nadat de respijtperiode van één uur, wordt de capaciteit van toepassingen in 505 TiB worden aangepast.  
        Dat wil zeggen, de totale capaciteit gebruikt 8 * 60 =-TiB quotum voor Volumes van 1 tot en met 8 en 25 TiB van het werkelijke verbruik voor Volume 9.
    * De gefactureerde capaciteit is 505 TiB.
    * Volume-quotum voor Volume 9 kan niet worden verhoogd (omdat de totale toegewezen quota voor de groep mag niet meer dan 500 TiB).
    * Extra QoS doorvoer kan niet worden toegewezen (omdat de totale QoS voor de pool is nog steeds op basis van 500 TiB).

   ![Capaciteit van toepassingen met negen volumes](../media/azure-netapp-files/azure-netapp-files-capacity-pool-with-nine-vols.png)

## <a name="capacity-consumption-of-snapshots"></a>Capaciteit verbruik van momentopnamen 

Het capaciteitsgebruik van momentopnamen in Azure NetApp bestanden wordt in rekening gebracht op basis van het quotum van het bovenliggende volume.  Als gevolg hiervan, deelt deze het hetzelfde tarief als de capaciteit van toepassingen die het volume behoort.  Echter, in tegenstelling tot de actieve volume snapshot gebruik wordt gemeten op basis van de incrementele capaciteit is verbruikt.  Azure NetApp bestanden-momentopnamen zijn differentiële van aard. Afhankelijk van de wijzigingssnelheid van de gegevens gebruiken de momentopnamen vaak veel minder capaciteit dan de logische capaciteit van het actieve volume. Bijvoorbeeld, wordt ervan uitgegaan dat u hebt een momentopname van een volume 500 GiB dat alleen 10 GiB van differentiële gegevens. De capaciteit die in rekening voor het volume-quotum voor deze momentopname gebracht wordt zou 10 GiB, niet 500 GiB zijn. 

## <a name="next-steps"></a>Volgende stappen

* [Azure NetApp bestanden pagina met prijzen](https://azure.microsoft.com/pricing/details/storage/netapp/)
* [Serviceniveau's voor Azure NetApp Files](azure-netapp-files-service-levels.md)
* [Resourcelimieten voor Azure NetApp Files](azure-netapp-files-resource-limits.md)
