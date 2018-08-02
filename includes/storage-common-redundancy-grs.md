---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/26/2018
ms.author: jeking
ms.custom: include file
ms.openlocfilehash: f5b6e0e74bbab33b9ae6fbacca5c55ea434d3e41
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399954"
---
Geografisch redundante opslag (GRS) is zodanig ontworpen dat ten minste 99,99999999999999% (16 9's) duurzaamheid van objecten in een bepaald jaar door uw gegevens te repliceren naar een secundaire regio die grote afstand van de primaire regio. Als uw storage-account GRS is ingeschakeld heeft, zijn uw gegevens zijn duurzame zelfs in geval van een volledige regionale stroomstoring of een ramp waarin de primaire regio kan niet worden hersteld.

Als u hebt gekozen voor GRS, hebt u twee andere opties kunt kiezen uit:

* GRS worden uw gegevens gerepliceerd naar een ander datacenter in een secundaire regio, maar deze beschikbaar is voor alleen-lezen zijn als Microsoft een failover van de primaire naar secundaire regio initieert. 
* Geo-redundante opslag met leestoegang (RA-GRS) is gebaseerd op GRS. RA-GRS worden uw gegevens gerepliceerd naar een ander datacenter in een secundaire regio, en biedt u ook de optie voor het lezen van de secundaire regio. Met RA-GRS, kunt u lezen van de secundaire, ongeacht of Microsoft een failover van de primaire naar de secundaire initieert. 

Voor een opslagaccount met GRS of RA-GRS wordt ingeschakeld, worden alle gegevens eerst gerepliceerd met lokaal redundante opslag (LRS). Een update is eerst gericht op de primaire locatie en gerepliceerd met behulp van LRS. De update wordt vervolgens asynchroon gerepliceerd naar de secundaire regio met GRS. Wanneer gegevens worden geschreven naar de secundaire locatie, is het ook gerepliceerd binnen die locatie met behulp van LRS. 

De primaire en secundaire regio replica's beheren in afzonderlijke foutdomeinen en upgrade-domeinen binnen een opslagschaaleenheid. De opslagschaaleenheid is de basic-replicatie-eenheid binnen het datacenter. Replicatie op dit niveau wordt geleverd door LRS; Zie voor meer informatie, [lokaal redundante opslag (LRS): de gegevensredundantie lage kosten voor Azure Storage](../articles/storage/common/storage-redundancy-lrs.md).

Houd er rekening mee bij het bepalen van welke replicatieoptie te gebruiken:

* Zone-redundante opslag (ZRS) biedt hoge beschikbaarheid met synchrone replicatie en mogelijk een betere keuze voor enkele scenario's dan GRS of RA-GRS. Zie voor meer informatie over ZRS [ZRS](../articles/storage/common/storage-redundancy-zrs.md).
* Omdat asynchrone replicatie een vertraging omvat, in het geval van een regionaal noodgeval is het mogelijk dat wijzigingen die nog niet is gerepliceerd naar de secundaire regio verloren gaan als de gegevens van de primaire regio niet kan worden hersteld.
* Met GRS, de replica is niet beschikbaar tenzij Microsoft failover naar de secundaire regio initieert. Als Microsoft een failover naar de secundaire regio te starten, wordt hebt u gelezen en schrijftoegang tot die gegevens na de failover is voltooid. Zie voor meer informatie, [Disaster Recovery Guidance](../articles/storage/common/storage-disaster-recovery-guidance.md).
* Als de toepassing nodig heeft om te lezen van de secundaire regio, schakelt u RA-GRS.

## <a name="read-access-geo-redundant-storage"></a>Geografisch redundante opslag met leestoegang

Geo-redundante opslag met leestoegang (RA-GRS) maximale beschikbaarheid voor uw opslagaccount. RA-GRS biedt alleen-lezen toegang tot de gegevens in de secundaire locatie, naast de geo-replicatie tussen twee regio's.

Wanneer u alleen-lezen toegang tot uw gegevens in de secundaire regio, zijn uw gegevens is beschikbaar op een secundaire eindpunt, maar ook op het primaire eindpunt voor uw opslagaccount. Het secundaire eindpunt is vergelijkbaar met het primaire eindpunt, maar voegt het achtervoegsel `–secondary` tot de accountnaam. Bijvoorbeeld, als uw primaire eindpunt voor de Blob-service is `myaccount.blob.core.windows.net`, dan is het secundaire eindpunt `myaccount-secondary.blob.core.windows.net`. De toegangssleutel voor uw storage-account zijn hetzelfde voor de primaire en secundaire eindpunten.

Enkele overwegingen waarmee u rekening moet houden bij het gebruik van RA-GRS:

* Uw toepassing heeft voor het beheren van welk eindpunt het wordt interactie met bij het gebruik van RA-GRS.
* Omdat asynchrone replicatie een vertraging omvat, kunnen wijzigingen die nog niet is gerepliceerd naar de secundaire regio worden verbroken als gegevens kunnen niet worden hersteld van de primaire regio, bijvoorbeeld in het geval van een regionaal noodgeval.
* U kunt de laatste synchronisatietijd van uw opslagaccount kunt controleren. Tijd van laatste synchronisatie is een GMT datum/tijd-waarde. Alle primaire schrijfbewerkingen voordat u de tijd van laatste synchronisatie hebt geschreven naar de secundaire locatie, wat betekent dat ze zijn beschikbaar om te lezen vanaf de secundaire locatie. Primaire schrijft na de laatste synchronisatietijd kan of mogelijk niet beschikbaar voor leesbewerkingen nog. U kunt deze waarde met behulp van een query de [Azure-portal](https://portal.azure.com/), [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md), of van een van de Azure Storage-clientbibliotheken.
* Als Microsoft failover naar de secundaire regio initieert, wordt hebt u gelezen en schrijftoegang tot die gegevens na de failover is voltooid. Zie voor meer informatie, [Disaster Recovery Guidance](../articles/storage/common/storage-disaster-recovery-guidance.md).
* Zie voor meer informatie over het overschakelen naar de secundaire regio [wat te doen als een Azure Storage-storing](../articles/storage/common/storage-disaster-recovery-guidance.md).
* RA-GRS is bedoeld voor hoge beschikbaarheid. Voor richtlijnen voor schaalbaarheid, raadpleegt u de [controlelijst voor prestaties](../articles/storage/common/storage-performance-checklist.md).
* Zie voor suggesties voor het ontwerpen voor hoge beschikbaarheid met RA-GRS, [het ontwerpen van maximaal beschikbare toepassingen met RA-GRS-opslag](../articles/storage/common/storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>Wat is de RPO en de RTO met GRS?
**Recovery Point Objective (RPO):** In GRS en RA-GRS, de opslag service asynchroon geo-gerepliceerd de gegevens van het primaire naar de secundaire locatie. In het geval van een regionaal noodgeval in de primaire regio, wordt een failover naar de secundaire regio uitgevoerd door Microsoft. Als een failover gebeurt, is het mogelijk dat recente wijzigingen die nog niet geo-replicatie zijn verloren. Het aantal minuten van mogelijke gegevens verloren wordt aangeduid als de RPO en dit geeft het punt in tijd waarop gegevens kunnen worden hersteld. Azure-opslag wordt doorgaans een RPO van minder dan 15 minuten is, maar er is momenteel geen SLA voor hoe lang geo-replicatie plaatsvindt.

**Beoogde hersteltijd (RTO):** de RTO bepaalt een maateenheid voor hoe lang het duurt om de failover uitvoeren en ophalen van het opslagaccount weer online is. De tijd voor het uitvoeren van de failover bevat de volgende acties:

   * De tijd Microsoft vereist om te bepalen of de gegevens kunnen worden hersteld op de primaire locatie, of als failover nodig is.
   * De tijd om uit te voeren van de failover van het opslagaccount dat door het veranderen van de primaire DNS-vermeldingen om te verwijzen naar de secundaire locatie.

   Microsoft neemt de verantwoordelijkheid van de ernstig waarmee uw gegevens behouden blijven. Als er een kans van het herstellen van de gegevens in de primaire regio, wordt Microsoft vertraging van de failover en richt u bij het herstellen van uw gegevens. Een toekomstige versie van de service kunt u een failover op het accountniveau van een te activeren zodat u kunt de RTO bepaalt zelf beheren.

## <a name="paired-regions"></a>Gekoppelde regio 's 

Als u een opslagaccount maakt, selecteert u de primaire regio voor het account. De gekoppelde secundaire regio wordt bepaald op basis van de primaire regio, en kan niet worden gewijzigd. Zie voor actuele informatie over regio's ondersteund door Azure [zakelijke continuïteit en herstel na noodgevallen (BCDR): gekoppelde regio's Azure](../articles/best-practices-availability-paired-regions.md).
