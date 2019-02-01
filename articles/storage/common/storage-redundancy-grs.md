---
title: Geografisch redundante opslag (GRS) voor regio-overschrijdend duurzaamheid in Azure Storage | Microsoft Docs
description: Geografisch redundante opslag (GRS) worden uw gegevens gerepliceerd tussen twee regio's die grote afstand van elkaar. GRS beschermt tegen hardwarefouten in het datacenter, evenals de regionale rampen.
services: storage
author: tolandmike
ms.service: storage
ms.topic: article
ms.date: 10/20/2018
ms.author: jeking
ms.subservice: common
ms.openlocfilehash: 2dc409743ce94ecb73e351b839a5a2fb09eadab2
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55512098"
---
# <a name="geo-redundant-storage-grs-cross-regional-replication-for-azure-storage"></a>Geografisch redundante opslag (GRS): Regio-overschrijdend-replicatie voor Azure Storage
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-grs.md)]

## <a name="read-access-geo-redundant-storage"></a>Geografisch redundante opslag met leestoegang
Geo-redundante opslag met leestoegang (RA-GRS) maximale beschikbaarheid voor uw opslagaccount. RA-GRS biedt alleen-lezen toegang tot de gegevens in de secundaire locatie, naast de geo-replicatie tussen twee regio's.

Wanneer u alleen-lezen toegang tot uw gegevens in de secundaire regio, zijn uw gegevens is beschikbaar op een secundaire eindpunt, maar ook op het primaire eindpunt voor uw opslagaccount. Het secundaire eindpunt is vergelijkbaar met het primaire eindpunt, maar voegt het achtervoegsel `–secondary` tot de accountnaam. Bijvoorbeeld, als uw primaire eindpunt voor de Blob-service is `myaccount.blob.core.windows.net`, dan is het secundaire eindpunt `myaccount-secondary.blob.core.windows.net`. De toegangssleutel voor uw storage-account zijn hetzelfde voor de primaire en secundaire eindpunten.

Enkele overwegingen waarmee u rekening moet houden bij het gebruik van RA-GRS:

* Uw toepassing heeft voor het beheren van welk eindpunt het wordt interactie met bij het gebruik van RA-GRS.
* Omdat asynchrone replicatie een vertraging omvat, kunnen wijzigingen die nog niet hebt zijn gerepliceerd naar de secundaire regio worden verbroken als gegevens van de primaire regio niet kan worden hersteld.
* U kunt de laatste synchronisatietijd van uw opslagaccount kunt controleren. Tijd van laatste synchronisatie is een GMT datum/tijd-waarde. Alle primaire schrijfbewerkingen voordat u de tijd van laatste synchronisatie hebt geschreven naar de secundaire locatie, wat betekent dat ze zijn beschikbaar om te lezen vanaf de secundaire locatie. Primaire schrijft na de laatste synchronisatietijd kan of mogelijk niet beschikbaar voor leesbewerkingen nog. U kunt deze waarde met behulp van een query de [Azure-portal](https://portal.azure.com/), [Azure PowerShell](storage-powershell-guide-full.md), of van een van de Azure Storage-clientbibliotheken.
* Als u de failover van een account (preview) van een GRS of RA-GRS-account naar de secundaire regio hebt gestart, hersteld schrijftoegang tot dat account nadat de failover is voltooid. Zie voor meer informatie, [Disaster recovery en storage-account failover (preview)](storage-disaster-recovery-guidance.md).
* RA-GRS is bedoeld voor hoge beschikbaarheid. Voor richtlijnen voor schaalbaarheid, raadpleegt u de [controlelijst voor prestaties](storage-performance-checklist.md).
* Zie voor suggesties voor het ontwerpen voor hoge beschikbaarheid met RA-GRS, [het ontwerpen van maximaal beschikbare toepassingen met RA-GRS-opslag](storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>Wat is de RPO en de RTO met GRS?

**Recovery Point Objective (RPO):** GRS en RA-GRS, de opslag voor de service asynchroon geo-gerepliceerd de gegevens van het primaire naar de secundaire locatie. In het geval dat de primaire regio niet beschikbaar is, kunt u de failover van een account (preview) naar de secundaire regio uitvoeren. Wanneer u een failover hebt gestart, is het mogelijk dat recente wijzigingen die nog niet hebt is geo-replicatie verloren. Het aantal minuten van mogelijke gegevens verloren gegaan staat bekend als de RPO. De RPO geeft het punt in tijd waarop gegevens kunnen worden hersteld. Azure-opslag wordt doorgaans een RPO van minder dan 15 minuten is, maar er is momenteel geen SLA voor hoe lang geo-replicatie plaatsvindt.

**Beoogde hersteltijd (RTO):** De RTO is een maateenheid voor hoe lang het duurt om de failover uitvoeren en ophalen van het opslagaccount weer online. De tijd voor het uitvoeren van de failover bevat de volgende acties:

   * De tijd totdat de klant de failover van de storage-account van de primaire naar de secundaire regio initieert.
   * De tijd die nodig is door Azure uit te voeren van de failover door te wijzigen van de primaire DNS-vermeldingen om te verwijzen naar de secundaire locatie.

## <a name="paired-regions"></a>Gekoppelde regio's 
Als u een opslagaccount maakt, selecteert u de primaire regio voor het account. De gekoppelde secundaire regio wordt bepaald op basis van de primaire regio, en kan niet worden gewijzigd. Zie voor actuele informatie over regio's ondersteund door Azure [zakelijke continuïteit en herstel na noodgevallen (BCDR): Gekoppelde Azure-regio's](../../best-practices-availability-paired-regions.md).

## <a name="see-also"></a>Zie ook
- [Azure Storage-replicatie](storage-redundancy.md)
- [Lokaal redundante opslag (LRS): Gegevensredundantie lage kosten voor Azure Storage](storage-redundancy-lrs.md)
- [Zone-redundante opslag (ZRS): Maximaal beschikbare toepassingen voor Azure Storage](storage-redundancy-zrs.md)
