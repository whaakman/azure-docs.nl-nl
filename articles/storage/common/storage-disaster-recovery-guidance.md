---
title: Wat te doen in het geval van een storing Azure Storage | Microsoft Docs
description: Wat te doen in het geval van een storing Azure Storage
services: storage
documentationcenter: .net
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 8f040b0f-8926-4831-ac07-79f646f31926
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 1/19/2017
ms.author: tamram
ms.openlocfilehash: c768bdbb8e1ce2aae3eb5b6db0e48977366c83fb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>Wat te doen in het geval van een Azure Storage-storing
Bij Microsoft werken we hard om ervoor te zorgen dat onze services altijd beschikbaar zijn. Soms dwingt afgezien van onze invloed ons op een manier die niet-geplande storingen in een of meer regio's veroorzaken. Als u deze zeldzame exemplaren verwerken, bieden we het volgende op hoog niveau richtlijnen voor Azure Storage-services.

## <a name="how-to-prepare"></a>Voorbereiden
Het is essentieel dat elke klant een eigen plan voor herstel na noodgevallen voorbereidt. De inspanning om te herstellen van een storing opslag doorgaans omvat zowel operations personeel en geautomatiseerde procedures om uw toepassingen met een werkende status opnieuw activeren. Raadpleeg de documentatie voor Azure voor het bouwen van uw eigen noodherstelplan hieronder:

* [Herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](/azure/architecture/resiliency/disaster-recovery-high-availability-azure-applications.md)
* [Technische richtlijnen voor flexibiliteit van Azure](/azure/architecture/resiliency.md)
* [Azure Site Recovery-service](https://azure.microsoft.com/services/site-recovery/)
* [Azure Storage-replicatie](storage-redundancy.md)
* [Azure Backup-service](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>Het vaststellen van
De aanbevolen manier om te bepalen van de status van de Azure-service is zich abonneren op de [Azure Service Health Dashboard](https://azure.microsoft.com/status/).

## <a name="what-to-do-if-a-storage-outage-occurs"></a>Wat te doen als er een storing opslag optreedt
Als een of meer opslagservices tijdelijk niet beschikbaar op een of meer regio's zijn, zijn er twee opties voor u te overwegen. Als u directe toegang tot uw gegevens willen, overweeg dan optie 2.

### <a name="option-1-wait-for-recovery"></a>Optie 1: Wacht voor herstel
In dit geval is geen actie ondernemen vereist. We werken naar eer en geweten voor het herstellen van de beschikbaarheid van de Azure-service. U kunt de servicestatus bewaken op de [Azure Service Health Dashboard](https://azure.microsoft.com/status/).

### <a name="option-2-copy-data-from-secondary"></a>Optie 2: Gegevens kopiëren van secundaire
Als u hebt gekozen [geografisch redundante opslag met leestoegang (RA-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (aanbevolen) voor uw storage-accounts, hebt u alleen toegang tot uw gegevens van de secundaire regio. U kunt hulpprogramma's gebruiken zoals [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md), en de [bibliotheek voor gegevensverplaatsing van Azure](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) gegevens uit de secundaire regio kopiëren naar een ander opslagaccount in een unimpacted regio en wijst u uw toepassingen aan dit opslagaccount voor zowel lezen als beschikbaarheid schrijven.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>Wat ze kunnen verwachten als een opslag-failover wordt uitgevoerd
Als u hebt gekozen [geografisch redundante opslag (GRS)](storage-redundancy.md#geo-redundant-storage) of [geografisch redundante opslag met leestoegang (RA-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (aanbevolen), Azure Storage houdt uw gegevens duurzaam in twee gebieden (primair en secundair). In beide regio's houdt de Azure Storage voortdurend meerdere replica's van uw gegevens.

Wanneer een regionale noodgeval is van invloed op uw primaire regio, wordt eerst geprobeerd om de service in deze regio te herstellen. Afhankelijk van de aard van de sitedatabase en de effecten, in sommige soms we niet mogelijk om terug te zetten van de primaire regio. Op dat moment wordt uitgevoerd, er een geo-failover. De regio-overschrijdende gegevensreplicatie is een asynchrone proces waarbij een vertraging kunt uitgevoerd, zodat het mogelijk dat de wijzigingen die nog niet is gerepliceerd naar de secundaire regio mogelijk verloren gegaan. U kunt een query de ['Tijd van laatste synchronisatie' van uw opslagaccount](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) voor meer informatie over de replicatiestatus.

Een aantal punten met betrekking tot de geo-failover-ervaring van opslag:

* Opslag-geo-failover wordt alleen geactiveerd door het team van Azure Storage: Er is geen klant-actie vereist.
* Uw bestaande service-eindpunten van opslag voor blobs, tabellen, wachtrijen en -bestanden blijven hetzelfde na het foutherstel. de Microsoft geleverde DNS-vermelding moet worden bijgewerkt om de primaire regio overschakelt naar de secundaire regio.  Microsoft kan deze update automatisch als onderdeel van het proces geo-failover wordt uitgevoerd.
* Voor en tijdens de geo-failover, u hebt geen toegang voor schrijven naar uw opslagaccount als gevolg van de impact van de sitedatabase, maar u kunt nog steeds lezen vanaf de secundaire als uw storage-account is geconfigureerd als een RA-GRS.
* Wanneer de geo-failover is voltooid en de wijzigingen van de DNS-doorgegeven, worden de lees- en schrijftoegang heeft tot uw storage-account hervat; Dit verwijst naar uw secundaire eindpunt wordt. 
* Houd er rekening mee dat u schrijftoegang hebt als u GRS of RA-GRS geconfigureerd voor het opslagaccount. 
* U kunt een query ['Laatste Geo Failover keer' van uw opslagaccount](https://msdn.microsoft.com/library/azure/ee460802.aspx) voor meer informatie.
* Na de failover uw storage-account volledig functioneert, maar in een 'gedegradeerde' status, zoals deze wordt daadwerkelijk gehost in een zelfstandige regio met geen geo-replicatie mogelijk. Om dit risico te beperken, wordt de oorspronkelijke primaire regio herstellen en voert u een geo-failback voor het herstellen van de oorspronkelijke status. Als de oorspronkelijke primaire regio is een onherstelbare, wordt er een andere secundaire regio toewijzen.
  Voor meer informatie over de infrastructuur van Azure Storage geo-replicatie, raadpleegt u het artikel op de blog van Storage-team over [opties voor redundantie en RA-GRS](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

## <a name="best-practices-for-protecting-your-data"></a>Aanbevolen procedures voor het beveiligen van uw gegevens
Er zijn enkele aanbevolen aanpak voor back-up van de gegevens van uw opslag op regelmatige basis.

* VM-schijven – gebruiken de [Azure Backup-service](https://azure.microsoft.com/services/backup/) back-up van de VM-schijven die worden gebruikt door uw virtuele machines in Azure.
* Blok-blobs – maken een [momentopname](https://msdn.microsoft.com/library/azure/hh488361.aspx) van elk blok-blob of de blobs kopieert naar een ander opslagaccount in met behulp van een andere regio [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md), of de [bibliotheek voor gegevensverplaatsing van Azure](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).
* Gebruik tabellen – [AzCopy](storage-use-azcopy.md) gegevens in de tabel exporteren naar een ander opslagaccount in een andere regio.
* Bestanden – gebruiken [AzCopy](storage-use-azcopy.md) of [Azure PowerShell](storage-powershell-guide-full.md) uw bestanden kopiëren naar een ander opslagaccount in een andere regio.

Voor informatie over het maken van toepassingen die volledig te van de functie voor RA-GRS profiteren check [ontwerpen van maximaal beschikbare toepassingen RA-GRS-opslag](../storage-designing-ha-apps-with-ragrs.md)

