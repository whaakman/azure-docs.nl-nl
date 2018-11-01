---
title: Plan uw Avere vFXT systeem - Azure
description: Wordt uitgelegd wilt doen voordat u implementeert Avere vFXT voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: f0e5523565dc561ed457dbc340835ad1889cb876
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50633966"
---
# <a name="plan-your-avere-vfxt-system"></a>Plan uw Avere vFXT-systeem

In dit artikel wordt uitgelegd hoe u van plan bent een nieuwe Avere vFXT voor Azure-cluster om te controleren of het cluster die u maakt is geplaatst en juiste grootte krijgen voor uw behoeften. 

Voordat u gaat naar de Azure Marketplace of het maken van virtuele machines, kunt u overwegen hoe het cluster communiceren met andere elementen in Azure. Abonnement aan waar de clusterbronnen bevindt zich in uw particuliere netwerk en subnetten en bepalen waar uw back-end-opslag worden. Zorg ervoor dat de clusterknooppunten die u maakt krachtig genoeg is zijn voor de ondersteuning van uw werkstroom. 

Lees verder voor meer informatie.

## <a name="resource-group-and-network-infrastructure"></a>Resource-group en netwerkinfrastructuur

Houd rekening met waar de elementen van uw vFXT Avere voor Azure-implementatie komen. Het onderstaande diagram toont een mogelijke opstelling weergegeven voor de vFXT Avere voor Azure-onderdelen:

![Diagram van het cluster netwerkcontroller en het cluster virtuele machines in één subnet. Om het subnet is grens een grens van een vnet. In het vnet bevindt zich een hexagoon die het storage-service-eindpunt; deze is verbonden met een onderbroken pijl naar een Blob-opslag buiten de vnet.](media/avere-vfxt-components-option.png)

Volg deze richtlijnen bij het plannen van de netwerkinfrastructuur van uw Avere vFXT systeem:

* Alle elementen moeten worden beheerd met een nieuw abonnement voor de implementatie van de vFXT Avere gemaakt. Deze strategie vereenvoudigt het bijhouden van kosten en opschonen, en helpt ook bij resourcequota partitie. Omdat de vFXT Avere wordt gebruikt met een groot aantal clients, voorkomt het isoleren van de clients en het cluster in één abonnement dat andere kritieke werkbelastingen mogelijk Resourcebeperking tijdens het inrichten van client.

* Ga naar uw compute-clientsystemen dicht bij het cluster vFXT. Back-end-opslag kan meer externe zijn.  

* Zoek het cluster vFXT en het cluster netwerkcontroller-VM in hetzelfde virtuele netwerk (vnet) en in dezelfde resourcegroep voor het gemak. Ze moeten ook hetzelfde opslagaccount gebruiken. 

* Het cluster moet zich in een eigen subnet om te voorkomen van IP-adres conflicteert met clients of bronnen berekenen. 

## <a name="ip-address-requirements"></a>Vereisten voor IP-adres 

Zorg ervoor dat van uw cluster subnet een groot genoeg zijn IP-adresbereik heeft voor de ondersteuning van het cluster. 

Het Avere vFXT-cluster maakt gebruik van de volgende IP-adressen:

* IP-adres van één cluster management. Dit adres naar een ander knooppunt in het cluster kunt verplaatsen, maar is altijd beschikbaar zodat u verbinding met het configuratieprogramma Avere het Configuratiescherm maken kunt.
* Voor elk clusterknooppunt:
  * Ten minste één client-IP-adres. (Alle clientgerichte adressen worden beheerd door van het cluster *vserver*, die kunnen verplaatsen tussen de knooppunten zo nodig.)
  * Een IP-adres voor communicatie binnen het cluster
  * IP-adres van één exemplaar (toegewezen aan de virtuele machine)

Als u Azure Blob-opslag gebruikt, kan er ook IP-adressen van uw cluster vnet vereist:  

* Een Azure Blob storage-account vereist ten minste vijf IP-adressen. Houd er rekening mee met deze vereiste als u Blob-opslag in hetzelfde vnet als uw cluster vinden.
* Als u Azure Blob-opslag die zich buiten het virtuele netwerk voor uw cluster gebruikt, moet u een service-eindpunt van de opslag binnen het vnet maken. Dit eindpunt maakt geen gebruik van een IP-adres.

U hebt de mogelijkheid om te zoeken netwerkbronnen en Blob storage (indien gebruikt) in verschillende resourcegroepen uit het cluster.

## <a name="vfxt-node-sizes"></a>knooppuntgrootten vFXT 

De virtuele machines die fungeren als knooppunten van het cluster de capaciteit voor aanvraag doorvoer en opslag van uw cache te bepalen. U kunt kiezen uit twee exemplaartypen, met verschillende geheugen, processor en kenmerken van lokale opslag. 

Elk knooppunt vFXT zijn identiek. Dat wil zeggen, als u een cluster met drie knooppunten maken hebt u drie virtuele machines van hetzelfde type en grootte. 

| Instantietype | vCPU's | Geheugen  | Lokale SSD-opslag  | Max. aantal gegevensschijven | Doorvoer per schijf zonder caching | NIC (aantal) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D16s_v3 | 16  | 64 GiB  | 128 GiB  | 32 | 25,600 IOPS <br/> 384 MBps | 8000 MBps (8) |
| Standard_E32s_v3 | 32  | 256 GiB | 512 GiB  | 32 | 51.200 IOPS <br/> 768 MBps | 16.000 MBps (8)  |

Schijfcache per knooppunt kan worden geconfigureerd en kunt rage van 1000 GB tot 8000 GB. 1 TB per knooppunt is de aanbevolen cachegrootte voor Standard_D16s_v3 knooppunten en 4 TB per knooppunt wordt aanbevolen voor Standard_E32s_v3 knooppunten.

Lees de volgende Microsoft Azure-documenten voor meer informatie over deze VM's:

* [Grootten van virtuele machines voor algemeen gebruik](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general)
* [Grootten van virtuele machines geoptimaliseerd voor geheugen](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)

## <a name="account-quota"></a>Accountquotum

Zorg ervoor dat uw abonnement heeft de capaciteit om uit te voeren van het cluster van Avere vFXT, evenals alle computing of client-systemen wordt gebruikt. Lezen [quotum voor het cluster vFXT](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster) voor meer informatie.

## <a name="back-end-data-storage"></a>Back-end-gegevensopslag

Wanneer deze zich niet in de cache, worden de werkset opgeslagen in een nieuwe Blob-container of in een bestaande cloud of de hardware-opslagsysteem?

Als u gebruiken van Azure Blob-opslag voor de back-end wilt, maakt u een nieuwe container als onderdeel van het cluster vFXT te maken. Gebruik de ``create-cloud-backed-container`` script voor implementatie- en toeleveringsgegevens de storage-account voor de nieuwe Blob-container. Deze optie maakt en configureert de nieuwe container zodat deze klaar voor gebruik als het cluster gereed is. Lezen [knooppunten maken en configureren van het cluster](avere-vfxt-deploy.md#create-nodes-and-configure-the-cluster) voor meer informatie.

> [!NOTE]
> Alleen lege Blob storage-containers kunnen worden gebruikt als core filter voor het Avere vFXT-systeem. De vFXT moet mogelijk zijn voor het beheren van de opslag zonder om bestaande gegevens te behouden. 
>
> Lezen [om gegevens te verplaatsen naar het cluster vFXT](avere-vfxt-data-ingest.md) voor meer informatie over het kopiëren van gegevens naar de nieuwe container van het cluster efficiënt met behulp van clientcomputers en de Avere vFXT-cache.

Als u een bestaande on-premises opslag-systeem gebruiken wilt, moet u deze toevoegen aan het cluster vFXT nadat deze is gemaakt. De ``create-minimal-cluster`` implementatiescript maakt u een cluster vFXT met er is geen back-end-opslag. Lezen [opslag configureren](avere-vfxt-add-storage.md) voor gedetailleerde instructies over het toevoegen van een bestaand opslagsysteem aan het Avere vFXT-cluster. 

## <a name="next-step-understand-the-deployment-process"></a>Volgende stap: inzicht in het implementatieproces

[Implementatie-overzicht](avere-vfxt-deploy-overview.md) biedt het totaalbeeld van alle de stappen die nodig zijn om te maken van een vFXT Avere voor Azure-systeem en bereid gegevens leveren.  