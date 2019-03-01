---
title: Plan uw Avere vFXT systeem - Azure
description: Wordt uitgelegd wilt doen voordat u implementeert Avere vFXT voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: v-erkell
ms.openlocfilehash: 3212befac60e3677c0b556825560cc548df42969
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56990982"
---
# <a name="plan-your-avere-vfxt-system"></a>Uw Avere vFXT-systeem plannen

In dit artikel wordt uitgelegd hoe u van plan bent een nieuwe Avere vFXT voor Azure-cluster dat wordt geplaatst en juiste grootte krijgen voor uw behoeften. 

Voordat u gaat naar de Azure Marketplace of het maken van virtuele machines, kunt u overwegen hoe het cluster communiceren met andere elementen in Azure. Abonnement aan waar de clusterbronnen bevindt zich in uw particuliere netwerk en subnetten en bepalen waar uw back-end-opslag worden. Zorg ervoor dat de clusterknooppunten die u maakt krachtig genoeg is zijn voor de ondersteuning van uw werkstroom. 

Lees verder voor meer informatie.

## <a name="resource-group-and-network-infrastructure"></a>Resource-group en netwerkinfrastructuur

Houd rekening met waar de elementen van uw vFXT Avere voor Azure-implementatie komen. Het onderstaande diagram toont een mogelijke opstelling weergegeven voor de vFXT Avere voor Azure-onderdelen:

![Diagram van het cluster netwerkcontroller en het cluster virtuele machines in één subnet. Om het subnet is grens een grens van een vnet. In het vnet bevindt zich een hexagoon die het storage-service-eindpunt; deze is verbonden met een onderbroken pijl naar een Blob-opslag buiten de vnet.](media/avere-vfxt-components-option.png)

Volg deze richtlijnen bij het plannen van de netwerkinfrastructuur van uw Avere vFXT systeem:

* Alle elementen moeten worden beheerd met een nieuw abonnement voor de implementatie van de vFXT Avere gemaakt. Voordelen zijn: 
  * Eenvoudiger kosten bijhouden: weergeven en controle alle kosten van resources, infrastructuur en compute-cycli worden in één abonnement.
  * Eenvoudiger opruimen - u kunt het hele abonnement wanneer u klaar bent met het project te verwijderen.
  * Handige partitioneren van resource quota - andere kritieke werkbelastingen beschermen tegen mogelijke Resourcebeperking door te isoleren van de Avere vFXT clients en het cluster in één abonnement. Hiermee voorkomt u conflicten bij het plaatsen van een groot aantal clients voor een high performance computing-werkstroom.

* Ga naar uw compute-clientsystemen dicht bij het cluster vFXT. Back-end-opslag kan meer externe zijn.  

* Het cluster vFXT en het cluster netwerkcontroller-VM in hetzelfde virtuele netwerk (vnet), moeten zich bevinden in dezelfde resourcegroep bevinden, en gebruikt u hetzelfde opslagaccount. De sjabloon die automatisch een cluster maken doet dit voor de meeste situaties.

* Het cluster moet zich in een eigen subnet om te voorkomen van IP-adres conflicteert met clients of bronnen berekenen. 

* De sjabloon voor het maken van cluster kunt maken van de meeste van de benodigde infrastructuurresources voor het cluster, met inbegrip van resourcegroepen, virtuele netwerken, subnetten en storage-accounts. Als u gebruiken van resources die al bestaat wilt, controleert u of ze voldoen aan de vereisten in deze tabel. 

  | Resource | Bestaande gebruiken? | Vereisten |
  |----------|-----------|----------|
  | Resourcegroep | Ja, als dit leeg is | Moet leeg zijn| 
  | Storage-account | Ja als u verbinding maakt een bestaande Blob-container na het maken van clusters <br/>  Nee als u een nieuwe Blob-container maakt tijdens het maken van clusters | Bestaande Blob-container moet leeg zijn <br/> &nbsp; |
  | Virtueel netwerk | Ja | Een storage-service-eindpunt moet bevatten als het maken van een nieuwe Azure Blob-container | 
  | Subnet | Ja |   |

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

## <a name="vfxt-node-size"></a>knooppuntgrootte vFXT

De virtuele machines die fungeren als knooppunten van het cluster de capaciteit voor aanvraag doorvoer en opslag van uw cache te bepalen. <!-- The instance type offered has been chosen for its memory, processor, and local storage characteristics. You can choose from two instance types, with different memory, processor, and local storage characteristics. -->

Elk knooppunt vFXT zijn identiek. Dat wil zeggen, als u een cluster met drie knooppunten maken hebt u drie virtuele machines van hetzelfde type en grootte. 

| Instantietype | vCPU's | Geheugen  | Lokale SSD-opslag  | Max. aantal gegevensschijven | Doorvoer per schijf zonder caching | NIC (aantal) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_E32s_v3 | 32  | 256 GiB | 512 GiB  | 32 | 51.200 IOPS <br/> 768 MBps | 16.000 MBps (8)  |

Schijfcache per knooppunt kan worden geconfigureerd en kunt rage van 1000 GB tot 8000 GB. 4 TB per knooppunt is de aanbevolen cachegrootte voor Standard_E32s_v3 knooppunten.

Lees voor meer informatie over deze VM's, de Microsoft Azure-documentatie:

* [Grootten van virtuele machines geoptimaliseerd voor geheugen](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)

## <a name="account-quota"></a>Accountquotum

Zorg ervoor dat uw abonnement heeft de capaciteit om uit te voeren van het cluster van Avere vFXT, evenals alle computing of client-systemen wordt gebruikt. Lezen [quotum voor het cluster vFXT](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster) voor meer informatie.

## <a name="back-end-data-storage"></a>Back-end-gegevensopslag

Waar moet de Avere vFXT cluster opslaan van uw gegevens wanneer deze zich niet in de cache? Bepaal of uw werkset lange termijn opgeslagen in een nieuwe Blob-container of in een bestaande cloud of de hardware-opslagsysteem. 

Als u gebruiken van Azure Blob-opslag voor de back-end wilt, maakt u een nieuwe container als onderdeel van het cluster vFXT te maken. Deze optie maakt en configureert de nieuwe container zodat deze klaar voor gebruik als het cluster gereed is. 

Lezen [de vFXT Avere maken voor Azure](avere-vfxt-deploy.md#create-the-avere-vfxt-for-azure) voor meer informatie.

> [!NOTE]
> Alleen lege Blob storage-containers kunnen worden gebruikt als core filter voor het Avere vFXT-systeem. De vFXT moet mogelijk zijn voor het beheren van de opslag zonder om bestaande gegevens te behouden. 
>
> Lezen [om gegevens te verplaatsen naar het cluster vFXT](avere-vfxt-data-ingest.md) voor meer informatie over het kopiëren van gegevens naar de nieuwe container van het cluster efficiënt met behulp van clientcomputers en de Avere vFXT-cache.

Als u een bestaande on-premises opslag-systeem gebruiken wilt, moet u deze toevoegen aan het cluster vFXT nadat deze is gemaakt. Lezen [opslag configureren](avere-vfxt-add-storage.md) voor gedetailleerde instructies over het toevoegen van een bestaand opslagsysteem aan het Avere vFXT-cluster.

## <a name="cluster-access"></a>Toegang tot het cluster 

De vFXT Avere voor Azure-cluster bevindt zich in een privé-subnet en het cluster heeft geen openbaar IP-adres. U moet een methode van de toegang tot de privé-subnet voor Clusterbeheer en clientverbindingen hebben. 

Access-opties zijn onder andere:

* Host Jump - openbaar IP-adres toewijzen aan een afzonderlijke virtuele machine binnen het particuliere netwerk en maken van een SSL-tunnel naar de clusterknooppunten. 

  > [!TIP]
  > Als u een openbaar IP-adres ingesteld op de clustercontroller, kunt u deze als de jump-host. Lezen [Cluster netwerkcontroller als host gaat](#cluster-controller-as-jump-host) voor meer informatie.

* Virtueel particulier netwerk (VPN): Configureer een punt-naar-site of site-naar-site VPN-verbinding met uw particuliere netwerk.

* Met Azure ExpressRoute - configureren van een particuliere verbinding via een ExpressRoute-partner. 

Lees voor meer informatie over deze opties de [Azure Virtual Network-documentatie over internetcommunicatie](../virtual-network/virtual-networks-overview.md#communicate-with-the-internet).

### <a name="cluster-controller-as-jump-host"></a>Cluster-netwerkcontroller als host gaat

Als u een openbaar IP-adres ingesteld op de clustercontroller, kunt u deze als host voor een korte inleiding contact opnemen met de Avere vFXT-cluster op basis van buiten het privé-subnet. Echter, omdat de controller toegangsrechten heeft voor het wijzigen van de clusterknooppunten, Hiermee maakt u een kleine beveiligingsrisico.  

Voor een betere beveiliging voor een domeincontroller met een openbaar IP-adres maakt het script voor implementatie automatisch een netwerkbeveiligingsgroep die binnenkomende toegang tot alleen poort 22 beperkt. Kunt u het systeem verder beschermen met vergrendelen omlaag toegang tot het bereik van IP-bronadressen - dat wil zeggen, alleen verbindingen toestaan vanaf computers die u wilt gebruiken voor toegang tot het cluster.

Bij het maken van het cluster, kunt u al dan niet te maken van een openbaar IP-adres op de clustercontroller. 

* Als u een nieuw vnet of een nieuw subnet maken, wordt de clustercontroller een openbaar IP-adres worden toegewezen.
* Als u een bestaand vnet en subnet selecteert, wordt de clustercontroller alleen privé IP-adressen hebben. 

## <a name="next-step-understand-the-deployment-process"></a>Volgende stap: Informatie over het implementatieproces

[Implementatie-overzicht](avere-vfxt-deploy-overview.md) biedt het totaalbeeld van alle de stappen die nodig zijn om te maken van een vFXT Avere voor Azure-systeem en bereid gegevens leveren.  