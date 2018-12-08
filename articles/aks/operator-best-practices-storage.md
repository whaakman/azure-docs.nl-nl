---
title: Operator aanbevolen procedures - opslag in Azure Kubernetes Services (AKS)
description: Meer over de best practices uit de cluster-operator voor opslag, versleuteling van gegevens en back-ups in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: iainfou
ms.openlocfilehash: 691decb88188a428edfeab1ea9e99c48876b6d9f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53109842"
---
# <a name="best-practices-for-storage-and-backups-in-azure-kubernetes-service-aks"></a>Aanbevolen procedures voor opslag en back-ups in Azure Kubernetes Service (AKS)

Bij het maken en beheren van clusters in Azure Kubernetes Service (AKS), moeten uw toepassingen vaak opslag. Het is belangrijk om te begrijpen van de prestatiebehoeften en toegang tot methoden voor het gehele product zodat u de juiste opslag voor toepassingen kunt opgeven. De grootte van het AKS-knooppunt kan van invloed zijn op deze opslagopties. U moet ook plannen voor manieren om te testen, het herstelproces voor de gekoppelde opslag en back-up.

Deze aanbevolen procedures voor richt zich op de opslagoverwegingen voor het clusteroperators. In dit artikel leert u:

> [!div class="checklist"]
> * Welke soorten opslag zijn beschikbaar
> * Hoe goed grootte van een AKS-knooppunten voor opslagprestaties
> * Verschillen tussen de dynamische en statische toewijzing van volumes
> * Manieren om een back-up en Beveilig uw gegevensvolumes

## <a name="choose-the-appropriate-storage-type"></a>Het juiste opslagtype

**Aanbevolen procedurerichtlijn** -begrijpt de behoeften van uw toepassing om op te halen van de juiste opslag. Hoge prestaties, SSD-opslag voor workloads voor productie gebruiken. Plan voor opslag op het netwerk wanneer er meerdere gelijktijdige verbindingen nodig.

Toepassingen vereisen vaak verschillende typen en snelheid van opslag. Moeten uw toepassingen opslag die verbinding maakt met afzonderlijke schillen of verdeeld over meerdere schillen zijn? Is de opslag voor alleen-lezen toegang tot gegevens en het schrijven van grote hoeveelheden gestructureerde gegevens? Deze opslag moet bepalen het meest geschikte type opslag te gebruiken.

De volgende tabel geeft een overzicht van de typen beschikbare opslag en de bijbehorende mogelijkheden:

| Use-case | Volume-invoegtoepassing | Eenmaal lezen/schrijven | Alleen-lezen veel | Lezen/schrijven veel |
|----------|---------------|-----------------|----------------|-----------------|
| Gedeelde configuratie       | Azure Files   | Ja | Ja | Ja |
| Gestructureerde app-gegevens        | Azure-schijven   | Ja | Nee  | Nee  |
| App-gegevens, alleen-lezen-shares | [Dysk (preview)][dysk] | Ja | Ja | Nee  |
| Niet-gestructureerde gegevens, bewerkingen in het bestandssysteem | [BlobFuse (preview)][blobfuse] | Ja | Ja | Ja |

De twee belangrijkste typen opslag die is geleverd voor volumes in AKS worden ondersteund door Azure-schijven of Azure Files. Voor een betere beveiliging beide soorten opslag Azure Storage Service Encryption (SSE) gebruiken standaard die gegevens in rust worden versleuteld. Schijven kunnen niet op dit moment worden versleuteld met Azure Disk Encryption op het niveau van het AKS-knooppunten.

Azure Files zijn momenteel beschikbaar in de Standard-prestatielaag. Azure-schijven zijn beschikbaar in Standard en Premium prestatielagen:

- *Premium* schijven worden ondersteund door hoogwaardige SSD-schijven (SSD's). Premium-schijven worden aanbevolen voor alle werkbelastingen voor productie.
- *Standard* schijven worden ondersteund door traditionele draaiende schijven (HDD's) en zijn geschikt voor archivering of weinig gebruikte gegevens.

Inzicht in de prestatiebehoeften van de toepassing en toegang tot patronen om te kiezen van de juiste opslaglaag. Zie voor meer informatie over Managed Disks grootten en prestatielagen [overzicht Azure Managed Disks][managed-disks]

### <a name="create-and-use-storage-classes-to-define-application-needs"></a>Maken en storage-klassen gebruiken voor het definiëren van de vereisten voor toepassing

Het type van de opslagcapaciteit die u gebruikt is gedefinieerd met behulp van Kubernetes *Opslagklassen*. De opslagklasse wordt vervolgens waarnaar wordt verwezen in de implementatie of pod-specificatie. Deze definities werken samen om de juiste opslag maken en verbinden met schillen. Zie voor meer informatie, [Opslagklassen in AKS][aks-concepts-storage-classes].

## <a name="size-the-nodes-for-storage-needs"></a>Het formaat van de knooppunten voor opslagbehoeften

**Aanbevolen procedurerichtlijn** -de grootte van elk knooppunt een maximumaantal schijven ondersteunt. Grootte van verschillende bieden ook verschillende hoeveelheden lokale opslag en netwerkbandbreedte. Plan voor uw toepassing te voldoen aan de juiste hoeveelheid knooppunten implementeren.

AKS-knooppunten worden uitgevoerd als virtuele Azure-machines. Verschillende typen en -grootten van virtuele machine zijn beschikbaar. Elke VM-grootte biedt een andere hoeveelheid kernbronnen, zoals CPU en geheugen. Deze VM-grootten hebben een maximumaantal schijven dat kan worden gekoppeld. Prestaties van de opslag wordt ook varieert tussen VM-grootten voor de maximale lokale en bijgevoegde schijf IOPS (invoer/uitvoer-bewerkingen per seconde).

Als uw toepassingen Azure-schijven naar hun opslagoplossing vereisen, plannen en kies een passende knooppunt VM-grootte. De hoeveelheid CPU en geheugen is de enige factor niet wanneer u een VM-grootte kiest. De opslagmogelijkheden zijn ook belangrijk. Bijvoorbeeld, zowel de *Standard_B2ms* en *Standard_DS2_v2* VM-grootten omvatten een vergelijkbare CPU en geheugenbronnen. De mogelijke opslagprestaties is verschillend zijn, zoals wordt weergegeven in de volgende tabel:

| Knooppunttype en de grootte | vCPU | Geheugen (GiB) | Max. aantal gegevensschijven | Max zonder caching schijf-IOPS | Max. zonder caching doorvoer (MBps) |
|--------------------|------|--------------|----------------|------------------------|--------------------------------|
| Standard_B2ms      | 2    | 8            | 4              | 1,920                  | 22.5                           |
| Standard_DS2_v2    | 2    | 7            | 8              | 6,400                  | 96                             |

Hier de *Standard_DS2_v2* kunt dubbele het aantal gekoppelde schijven en drie tot vier keer de hoeveelheid van het IOPS en schijf-doorvoer. Als u alleen de belangrijkste compute-resources bekeken en vergeleken met kosten, u kunt ervoor kiezen de *Standard_B2ms* VM grootte en slechte opslagprestaties en beperkingen hebben. Werken met uw ontwikkelingsteam toepassing om te begrijpen van de opslagvereisten voor capaciteit en prestaties. Kies de juiste VM-grootte voor de AKS-knooppunten te bereiken of overschrijden de prestatiebehoeften. Regelmatig basislijn toepassingen om aan te passen van VM-grootte indien nodig.

Zie voor meer informatie over beschikbare VM-grootten, [grootten voor virtuele Linux-machines in Azure][vm-sizes].

## <a name="dynamically-provision-volumes"></a>Dynamisch inrichten van volumes

**Aanbevolen procedurerichtlijn** - beheer-overhead en kunt u schalen, niet statisch maken en toewijzen van permanente volumes te verminderen. Gebruik van dynamisch inrichten. In uw storage-klassen, definieert u het juiste reclaim beleid overbodige om opslagkosten te minimaliseren zodra schillen zijn verwijderd.

Wanneer u nodig hebt om te koppelen van opslag aan schillen, gebruikt u permanente volumes. Deze permanente volumes kunnen handmatig of dynamisch worden gemaakt. Handmatig maken van permanente volumes beheeroverhead wordt toegevoegd en beperkt u de mogelijkheid om te schalen. Gebruik dynamische permanent volume inrichten voor opslagbeheer vereenvoudigen en kunnen uw toepassingen groeien en schalen naar behoefte.

![Permanent volume claims in een cluster Azure Kubernetes-Services (AKS)](media/concepts-storage/persistent-volume-claims.png)

Een claim permanent volume (PVC) kunt u opslag naar behoefte dynamisch te maken. De onderliggende Azure-schijven worden gemaakt als schillen deze aanvragen. In het definitie van de schil aanvragen u een volume moet worden gemaakt en gekoppeld aan een ontworpen koppelpad

Zie voor de concepten over het dynamisch maken en gebruiken van volumes, [permanente Volumes Claims][aks-concepts-storage-pvcs].

Overzicht van deze volumes in actie zien hoe u het dynamisch maken en gebruiken van een permanent volume met [Azure Disks] [ dynamic-disks] of [Azure Files][dynamic-files].

Als onderdeel van uw opslag klassedefinities, stelt u de juiste *reclaimPolicy*. Deze reclaimPolicy bepaalt het gedrag van de onderliggende Azure storage-resource als de schil wordt verwijderd en het permanent volume is mogelijk niet meer vereist. De onderliggende resource voor opslag kan worden verwijderd of worden bewaard voor gebruik met een toekomstige schil. De reclaimPolicy kunt instellen op *behouden* of *verwijderen*. Inzicht in de behoeften van uw toepassing en implementeren van reguliere controles voor de opslag die worden bewaard om te minimaliseren, de hoeveelheid niet-gebruikte opslag die wordt gebruikt en kosten in rekening gebracht.

Zie voor meer informatie over opslagopties klasse [opslag vrijmaken beleid][reclaim-policy].

## <a name="secure-and-back-up-your-data"></a>Beveilig en back-up van uw gegevens

**Aanbevolen procedurerichtlijn** : Maak een Back-up van uw gegevens met behulp van een geschikt hulpprogramma voor het opslagtype, zoals Heptio Ark of Azure Site Recovery. Controleer of de integriteit en beveiliging van deze back-ups.

Als uw toepassingen opslaan en gebruiken gegevens persistent gemaakt op schijven of in bestanden, moet u regelmatig back-ups of momentopnamen van die gegevens. Azure-schijven kunnen momentopname van de ingebouwde technologieën gebruiken. U moet mogelijk een haakje voor uw toepassingen leegmaken worden geschreven naar de schijf voordat u de momentopnamebewerking niet uitvoeren. [Heptio Ark] [ heptio-ark] back-up van permanente volumes en aanvullende clusterresources en configuraties. Als u niet [status verwijderen uit uw toepassingen][remove-state], back-up van de gegevens van permanente volumes en regelmatig testen om te controleren of de integriteit van gegevens en de processen die nodig zijn de bewerkingen voor het herstellen.

Informatie over de beperkingen van de verschillende methoden voor gegevensback-ups en als u stilleggen uw gegevens voorafgaand aan de momentopname wilt. Gegevensback-ups kunnen niet per se u uw toepassingsomgeving van de implementatie van het cluster te herstellen. Zie voor meer informatie over deze scenario's, [aanbevolen procedures voor zakelijke continuïteit en herstel na noodgevallen in AKS][best-practices-multi-region].

## <a name="next-steps"></a>Volgende stappen

In dit artikel is gericht op de opslag aanbevolen procedures in AKS. Zie voor meer informatie over de basisprincipes van opslag in Kubernetes [concepten van opslag voor toepassingen in AKS][aks-concepts-storage].

<!-- LINKS - External -->
[heptio-ark]: https://github.com/heptio/ark
[dysk]: https://github.com/Azure/kubernetes-volume-drivers/tree/master/flexvolume/dysk
[blobfuse]: https://github.com/Azure/azure-storage-fuse

<!-- LINKS - Internal -->
[aks-concepts-storage]: concepts-storage.md
[vm-sizes]: ../virtual-machines/linux/sizes.md
[dynamic-disks]: azure-disks-dynamic-pv.md
[dynamic-files]: azure-files-dynamic-pv.md
[reclaim-policy]: concepts-storage.md#storage-classes
[aks-concepts-storage-pvcs]: concepts-storage.md#persistent-volume-claims
[aks-concepts-storage-classes]: concepts-storage.md#storage-classes
[managed-disks]: ../virtual-machines/linux/managed-disks-overview.md
[best-practices-multi-region]: operator-best-practices-multi-region.md
[remove-state]: operator-best-practices-multi-region.md#remove-service-state-from-inside-containers
