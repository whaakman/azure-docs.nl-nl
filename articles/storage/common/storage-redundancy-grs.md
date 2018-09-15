---
title: Geografisch redundante opslag (GRS) voor regio-overschrijdend duurzaamheid in Azure Storage | Microsoft Docs
description: Geografisch redundante opslag (GRS) worden uw gegevens gerepliceerd tussen twee regio's die grote afstand van elkaar. GRS beschermt tegen hardwarefouten in het datacenter, evenals de regionale rampen.
services: storage
author: tolandmike
ms.service: storage
ms.topic: article
ms.date: 03/20/2018
ms.author: jeking
ms.component: common
ms.openlocfilehash: eee9998280f2105fee5144b520bc8000b086f941
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45603922"
---
# <a name="geo-redundant-storage-grs-cross-regional-replication-for-azure-storage"></a>Geografisch redundante opslag (GRS): regio-overschrijdend-replicatie voor Azure Storage
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-grs.md)]

## <a name="read-access-geo-redundant-storage"></a>Geografisch redundante opslag met leestoegang
Geo-redundante opslag met leestoegang (RA-GRS) maximale beschikbaarheid voor uw opslagaccount. RA-GRS biedt alleen-lezen toegang tot de gegevens in de secundaire locatie, naast de geo-replicatie tussen twee regio's.

Wanneer u alleen-lezen toegang tot uw gegevens in de secundaire regio, zijn uw gegevens is beschikbaar op een secundaire eindpunt, maar ook op het primaire eindpunt voor uw opslagaccount. Het secundaire eindpunt is vergelijkbaar met het primaire eindpunt, maar voegt het achtervoegsel `–secondary` tot de accountnaam. Bijvoorbeeld, als uw primaire eindpunt voor de Blob-service is `myaccount.blob.core.windows.net`, dan is het secundaire eindpunt `myaccount-secondary.blob.core.windows.net`. De toegangssleutel voor uw storage-account zijn hetzelfde voor de primaire en secundaire eindpunten.

Enkele overwegingen waarmee u rekening moet houden bij het gebruik van RA-GRS:

* Uw toepassing heeft voor het beheren van welk eindpunt het wordt interactie met bij het gebruik van RA-GRS.
* Omdat asynchrone replicatie een vertraging omvat, kunnen wijzigingen die nog niet is gerepliceerd naar de secundaire regio worden verbroken als gegevens kunnen niet worden hersteld van de primaire regio, bijvoorbeeld in het geval van een regionaal noodgeval.
* U kunt de laatste synchronisatietijd van uw opslagaccount kunt controleren. Tijd van laatste synchronisatie is een GMT datum/tijd-waarde. Alle primaire schrijfbewerkingen voordat u de tijd van laatste synchronisatie hebt geschreven naar de secundaire locatie, wat betekent dat ze zijn beschikbaar om te lezen vanaf de secundaire locatie. Primaire schrijft na de laatste synchronisatietijd kan of mogelijk niet beschikbaar voor leesbewerkingen nog. U kunt deze waarde met behulp van een query de [Azure-portal](https://portal.azure.com/), [Azure PowerShell](storage-powershell-guide-full.md), of van een van de Azure Storage-clientbibliotheken.
* Als Microsoft failover naar de secundaire regio initieert, wordt hebt u gelezen en schrijftoegang tot die gegevens na de failover is voltooid. Zie voor meer informatie, [Disaster Recovery Guidance](storage-disaster-recovery-guidance.md).
* Zie voor meer informatie over het overschakelen naar de secundaire regio [wat te doen als een Azure Storage-storing](storage-disaster-recovery-guidance.md).
* RA-GRS is bedoeld voor hoge beschikbaarheid. Voor richtlijnen voor schaalbaarheid, raadpleegt u de [controlelijst voor prestaties](storage-performance-checklist.md).
* Zie voor suggesties voor het ontwerpen voor hoge beschikbaarheid met RA-GRS, [het ontwerpen van maximaal beschikbare toepassingen met RA-GRS-opslag](storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>Wat is de RPO en de RTO met GRS?
**Recovery Point Objective (RPO):** In GRS en RA-GRS, de opslag service asynchroon geo-gerepliceerd de gegevens van het primaire naar de secundaire locatie. In het geval van een regionaal noodgeval in de primaire regio, wordt een failover naar de secundaire regio uitgevoerd door Microsoft. Als een failover gebeurt, is het mogelijk dat recente wijzigingen die nog niet geo-replicatie zijn verloren. Het aantal minuten van mogelijke gegevens verloren wordt aangeduid als de RPO en dit geeft het punt in tijd waarop gegevens kunnen worden hersteld. Azure-opslag wordt doorgaans een RPO van minder dan 15 minuten is, maar er is momenteel geen SLA voor hoe lang geo-replicatie plaatsvindt.

**Beoogde hersteltijd (RTO):** de RTO bepaalt een maateenheid voor hoe lang het duurt om de failover uitvoeren en ophalen van het opslagaccount weer online is. De tijd voor het uitvoeren van de failover bevat de volgende acties:

   * De tijd Microsoft vereist om te bepalen of de gegevens kunnen worden hersteld op de primaire locatie, of als failover nodig is.
   * De tijd om uit te voeren van de failover van het opslagaccount dat door het veranderen van de primaire DNS-vermeldingen om te verwijzen naar de secundaire locatie.

   Microsoft neemt de verantwoordelijkheid van de ernstig waarmee uw gegevens behouden blijven. Als er een kans van het herstellen van de gegevens in de primaire regio, wordt Microsoft vertraging van de failover en richt u bij het herstellen van uw gegevens. 

## <a name="paired-regions"></a>Gekoppelde regio's 
Als u een opslagaccount maakt, selecteert u de primaire regio voor het account. De gekoppelde secundaire regio wordt bepaald op basis van de primaire regio, en kan niet worden gewijzigd. Zie voor actuele informatie over regio's ondersteund door Azure [zakelijke continuïteit en herstel na noodgevallen (BCDR): gekoppelde regio's Azure](../../best-practices-availability-paired-regions.md).

## <a name="see-also"></a>Zie ook
- [Azure Storage-replicatie](storage-redundancy.md)
- [Lokaal redundante opslag (LRS): de gegevensredundantie lage kosten voor Azure Storage](storage-redundancy-lrs.md)
- [Zone-redundante opslag (ZRS): toepassingen met hoge beschikbaarheid Azure Storage](storage-redundancy-zrs.md)