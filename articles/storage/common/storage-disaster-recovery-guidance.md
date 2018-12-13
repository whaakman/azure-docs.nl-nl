---
title: Wat te doen in het geval van een storing in de Azure-opslag | Microsoft Docs
description: Wat te doen in het geval van een Azure Storage-storing
services: storage
author: tamram
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 12/12/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 39a938d45c8f15c21b44bb5b04b1429fb4733b5a
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53323265"
---
# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>Wat te doen in het geval van een Azure Storage-storing
Bij Microsoft werken we er hard om ervoor te zorgen dat onze services altijd beschikbaar zijn. Soms zorgt ervoor dat meer dan onze invloed ons op een manier die niet-geplande storingen in een of meer regio's veroorzaken. Voor hulp bij het verwerken van deze zelden, bieden we de informatie te volgen op hoog niveau voor Azure Storage-services.

## <a name="how-to-prepare"></a>Voorbereiden
Het is essentieel dat elke klant een eigen plan voor herstel na noodgevallen voorbereidt. De inspanningen om te herstellen van een storing in de opslag normaal gesproken omvat zowel werknemers als geautomatiseerde procedures om te activeren van uw toepassingen in een werkende status hebben. Raadpleeg de documentatie voor Azure hieronder om uw eigen noodherstelplan bouwen:

* [Beschikbaarheidscontrolelijst](https://docs.microsoft.com/azure/architecture/checklist/availability)
* [Robuuste toepassingen ontwerpen voor Azure](https://docs.microsoft.com/azure/architecture/resiliency/)
* [Azure Site Recovery-service](https://azure.microsoft.com/services/site-recovery/)
* [Azure Storage-replicatie](https://docs.microsoft.com/azure/storage/common/storage-redundancy)
* [Azure Backup-service](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>Het vaststellen van
De aanbevolen manier om te bepalen van de status van de Azure-service is om u te abonneren op de [Azure Service Health Dashboard](https://azure.microsoft.com/status/).

## <a name="what-to-do-if-a-storage-outage-occurs"></a>Wat te doen als een Storage-storing
Als een of meer opslagservices tijdelijk niet beschikbaar op een of meer regio's zijn, zijn er twee opties voor u te overwegen. Als u directe toegang tot uw gegevens willen, houd rekening met optie 2.

### <a name="option-1-wait-for-recovery"></a>Optie 1: Wachten op herstel
In dit geval is geen actie van uw kant vereist. Er wordt hard gewerkt om te herstellen van de beschikbaarheid van Azure Services. U kunt de servicestatus controleren op de [Azure Service Health Dashboard](https://azure.microsoft.com/status/).

### <a name="option-2-copy-data-from-secondary"></a>Optie 2: Gegevens uit de secundaire regio kopiëren
Als u ervoor hebt gekozen [geo-redundante opslag met leestoegang (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage) (aanbevolen) voor uw storage-accounts, hebt u alleen toegang tot uw gegevens van de secundaire regio. U kunt hulpprogramma's zoals [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md), en de [-bibliotheek voor gegevensverplaatsing van Azure Data](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) gegevens te kopiëren van de secundaire regio naar een ander opslagaccount in een unimpacted regio en vervolgens verwijst u uw toepassingen die storage-account voor zowel lezen en schrijven van beschikbaarheid.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>Wat u kunt verwachten als een opslag-failover is uitgevoerd
Als u ervoor hebt gekozen [geografisch redundante opslag (GRS)](storage-redundancy-grs.md) of [geo-redundante opslag met leestoegang (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage) (aanbevolen), Azure Storage blijven uw gegevens duurzaam in twee regio's (primaire en secundaire). In beide regio's houdt de Azure Storage voortdurend meerdere replica's van uw gegevens.

Wanneer een regionaal noodgeval is van invloed op de primaire regio, zullen we eerst proberen te herstellen van de service in die regio voor de best mogelijke combinatie van RTO en RPO. Afhankelijk van de aard van de na noodgevallen en de effecten, in sommige zeldzame gevallen we niet mogelijk om terug te zetten van de primaire regio. Op dat moment gaat een geo-failover uitvoeren. Regio-overschrijdende gegevensreplicatie is een asynchroon proces dat betrekking heeft op een vertraging, zodat het mogelijk dat de wijzigingen die nog niet is gerepliceerd naar de secundaire regio mogelijk verloren gaan.

Een aantal punten met betrekking tot de opslagervaring voor geo-failover:

* Opslag-geo-failover wordt alleen geactiveerd door het team van Azure Storage: Er is geen klant-actie vereist. De failover wordt geactiveerd wanneer het Azure Storage-team heeft alle opties van het herstellen van gegevens in dezelfde regio, waarmee u de best mogelijke combinatie van RTO en RPO uitgeput.
* Uw bestaande storage service-eindpunten voor blobs, tabellen, wachtrijen en bestanden blijven hetzelfde na de failover; de Microsoft geleverde DNS-vermelding moet worden bijgewerkt als u wilt overschakelen van de primaire regio naar de secundaire regio. Microsoft zal deze update automatisch als onderdeel van de geo-failover-proces uitvoeren.
* Voorafgaand aan en tijdens de geo-failover, geen toegang voor schrijven naar uw opslagaccount vanwege de impact van de na noodgevallen, maar u kunt nog wel lezen vanaf de secundaire server als uw storage-account is geconfigureerd als RA-GRS.
* Wanneer de geo-failover is voltooid en de DNS-wijzigingen doorgegeven, worden lees- en schrijftoegang tot uw storage-account hersteld als u GRS of RA-GRS. Het eindpunt dat eerder het secundaire eindpunt is wordt uw primaire eindpunt. 
* U kunt de status van de primaire locatie en de query het laatst geo-failover voor uw opslagaccount controleren. Zie voor meer informatie, [Storage-Accounts: eigenschappen](https://docs.microsoft.com/rest/api/storagerp/storageaccounts/getproperties).
* Na de failover, uw storage-account wordt volledig functioneren, maar in een 'slechter' status, zoals deze wordt gehost in een zelfstandige regio met geen geo-replicatie mogelijk. Om dit risico te beperken, we herstellen de oorspronkelijke primaire regio en voert u een geo-failback voor het herstellen van de oorspronkelijke staat. Als de oorspronkelijke primaire regio niet kan worden hersteld, wordt er een andere secundaire regio toewijzen.

## <a name="best-practices-for-protecting-your-data"></a>Aanbevolen procedures voor het beveiligen van uw gegevens
Er zijn enkele aanbevolen benaderingen voor het back-up van de gegevens van uw opslag op gezette tijden.

* VM-schijven: Gebruik de [Azure Backup-service](https://azure.microsoft.com/services/backup/) back-up van de VM-schijven die worden gebruikt door uw Azure virtual machines.
* Blok-blobs – Schakel [voorlopig verwijderen](../blobs/storage-blob-soft-delete.md) ter bescherming tegen op objectniveau verwijderingen worden overschreven, en de blobs kopieert naar een ander opslagaccount in met behulp van een andere regio [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md), of de [-bibliotheek voor gegevensverplaatsing van Azure Data](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).
* Gebruik van tabellen: [AzCopy](storage-use-azcopy.md) voor het exporteren van gegevens in de tabel in een ander opslagaccount in een andere regio.
* Bestanden – [AzCopy](storage-use-azcopy.md) of [Azure PowerShell](storage-powershell-guide-full.md) uw bestanden naar een ander opslagaccount in een andere regio te kopiëren.

Voor informatie over het maken van toepassingen die van de RA-GRS-functie profiteren, Bekijk [het ontwerpen van maximaal beschikbare toepassingen met RA-GRS-opslag](../storage-designing-ha-apps-with-ragrs.md)
