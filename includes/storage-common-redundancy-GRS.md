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
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
Geografisch redundante opslag (GRS) is zodanig ontworpen dat ten minste 99.99999999999999% (16 van 9) duurzaamheid van objecten gedurende een bepaald jaar door uw gegevens repliceren naar een secundaire regio die honderden mijl weg van de primaire regio. Als uw storage-account GRS ingeschakeld heeft, zijn uw gegevens is duurzame zelfs het geval van een volledige regionale uitval of een ramp optreedt waarbij de primaire regio kan niet worden hersteld.

Als u ervoor voor GRS kiezen, hebt u twee andere opties kiezen:

* GRS worden uw gegevens gerepliceerd naar een ander datacenter in een secundaire regio, maar deze gegevens beschikbaar zijn voor het kenmerk alleen-lezen als Microsoft een failover van de primaire naar secundaire regio initieert. 
* Geografisch redundante opslag met leestoegang (RA-GRS) is gebaseerd op GRS. RA-GRS worden uw gegevens gerepliceerd naar een ander datacenter in een secundaire regio en biedt u ook de optie voor het lezen van de secundaire regio. U kunt vanaf de secundaire ongeacht of Microsoft een failover van de primaire naar de secundaire initieert lezen met RA-GRS. 

Voor een opslagaccount met GRS of RA-GRS is ingeschakeld, worden alle gegevens eerst gerepliceerd met lokaal redundante opslag (LRS). Een update wordt eerst doorgevoerd naar de primaire locatie en wordt gerepliceerd met LRS. De update wordt vervolgens asynchroon gerepliceerd naar de secundaire regio met GRS. Wanneer gegevens worden geschreven naar de secundaire locatie, wordt het ook binnen die locatie met LRS gerepliceerd. 

De gebieden primaire en secundaire replica's in afzonderlijke foutdomeinen beheren en upgrade domeinen binnen een schaaleenheid opslag. De schaaleenheid opslag is de replicatie basic-eenheid binnen het datacenter. Replicatie op dit niveau wordt geleverd door LRS; Zie voor meer informatie [lokaal redundante opslag (LRS): gegevensredundantie lage kosten voor Azure Storage](../articles/storage/common/storage-redundancy-lrs.md).

Houd rekening met de volgende punten wanneer u beslist welke optie voor opslagreplicatie te gebruiken:

* Zone-redundante opslag (ZRS) kan biedt hoge beschikbaarheid met synchrone replicatie en een betere keuze voor sommige scenario's dan GRS of RA-GRS. Zie voor meer informatie over ZRS [ZRS](../articles/storage/common/storage-redundancy-zrs.md).
* Omdat asynchrone replicatie een vertraging omvat, er in een noodsituatie regionale is het mogelijk dat wijzigingen die nog niet is gerepliceerd naar de secundaire regio verbroken worden als de gegevens van de primaire regio kan niet worden hersteld.
* Met GRS is niet de replica beschikbaar, tenzij Microsoft failover naar de secundaire regio initieert. Als Microsoft een failover naar de secundaire regio starten, u hebt leesmachtigingen en schrijftoegang tot die gegevens na de failover is voltooid. Zie voor meer informatie [Disaster Recovery richtlijnen](../articles/storage/common/storage-disaster-recovery-guidance.md).
* Als uw toepassing moet worden gelezen vanaf de secundaire regio, schakelt u RA-GRS.

## <a name="read-access-geo-redundant-storage"></a>Geografisch redundante opslag met leestoegang

Geografisch redundante opslag met leestoegang (RA-GRS) maximaliseert de beschikbaarheid voor uw opslagaccount. RA-GRS geeft alleen-lezen toegang tot de gegevens in de secundaire locatie, naast de geo-replicatie tussen twee regio's.

Wanneer u alleen-lezen toegang tot uw gegevens op de secundaire regio, is uw gegevens beschikbaar op een secundair eindpunt en op het primaire eindpunt voor uw opslagaccount. Secundair eindpunt is vergelijkbaar met het primaire eindpunt maar voegt het achtervoegsel `–secondary` aan de accountnaam. Bijvoorbeeld, als uw primaire eindpunt voor de Blob-service is `myaccount.blob.core.windows.net`, dan is uw secundaire eindpunt `myaccount-secondary.blob.core.windows.net`. De toegangssleutels voor uw opslagaccount zijn hetzelfde voor de primaire en secundaire eindpunten.

Enkele overwegingen rekening moet houden bij het gebruik van RA-GRS:

* Uw toepassing is voor het beheren van welk eindpunt het interactie is met bij gebruik van RA-GRS.
* Aangezien asynchrone replicatie moet worden van een vertraging, wijzigingen die nog niet is gerepliceerd naar de secundaire regio zijn mogelijk verloren gegaan als gegevens niet worden hersteld vanaf de primaire regio, bijvoorbeeld in het geval van een regionale noodgeval.
* U kunt de laatste synchronisatietijd van uw storage-account controleren. Tijd van laatste synchronisatie is een GMT datum/tijd-waarde. Alle primaire schrijfbewerkingen voordat de laatste synchronisatietijd hebt geschreven naar de secundaire locatie, wat betekent dat ze beschikbaar zijn om te lezen op de secundaire locatie. Primaire schrijft nadat de tijd van laatste synchronisatie al dan niet meer beschikbaar zijn voor leesbewerkingen nog. U kunt deze waarde met behulp van een query de [Azure-portal](https://portal.azure.com/), [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md), of vanuit een van de clientbibliotheken van Azure Storage.
* Als Microsoft failover naar de secundaire regio initieert, u hebt leesmachtigingen en schrijftoegang tot die gegevens na de failover is voltooid. Zie voor meer informatie [Disaster Recovery richtlijnen](../articles/storage/common/storage-disaster-recovery-guidance.md).
* Zie voor meer informatie over het overschakelen naar de secundaire regio [wat te doen als een Azure Storage-storing optreedt,](../articles/storage/common/storage-disaster-recovery-guidance.md).
* RA-GRS is bedoeld voor hoge beschikbaarheid. Voor richtlijnen voor schaalbaarheid, raadpleegt u de [prestaties controlelijst](../articles/storage/common/storage-performance-checklist.md).
* Zie voor suggesties over het ontwerpen voor hoge beschikbaarheid met RA-GRS [ontwerpen van maximaal beschikbare toepassingen RA-GRS opslag](../articles/storage/common/storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>Wat is de RPO en RTO met GRS?
**Beoogde herstelpunt (RPO):** In GRS en RA-GRS, de opslag service asynchroon geo replicatie van de gegevens van de primaire naar de secundaire locatie. In het geval van een regionale noodgeval op de primaire regio, wordt een failover naar de secundaire regio uitgevoerd door Microsoft. Als een failover gebeurt, is het mogelijk dat recente wijzigingen die nog niet geogerepliceerde zijn verloren. Het aantal minuten van mogelijke gegevens verloren wordt aangeduid als de RPO en geeft het punt in de tijd waarop gegevens kunnen worden hersteld. Azure-opslag heeft een RPO van minder dan 15 minuten normaal, maar er is momenteel geen SLA op hoe lang geo-replicatie neemt.

**Beoogde hersteltijd (RTO) uitgevoerd:** de RTO is een meting van hoe lang het duurt het storage-account weer online en voert u de failover. De tijd voor het uitvoeren van de failover, bevat de volgende acties:

   * De tijd Microsoft vereist om te bepalen of de gegevens kunnen worden hersteld op de primaire locatie, of als een failover nodig is.
   * De tijd voor het uitvoeren van de failover van het opslagaccount door het wijzigen van de primaire DNS-vermeldingen om te verwijzen naar de secundaire locatie.

   Microsoft hecht de verantwoordelijkheid van ernstig waarmee uw gegevens behouden blijven. Als er een kans van het herstellen van de gegevens in de primaire regio, wordt Microsoft de failover uit te stellen en zich richten op uw gegevens herstellen. Een toekomstige versie van de service kunt u voor het activeren van een failover op accountniveau zodat u de RTO zelf wilt beheren.

## <a name="paired-regions"></a>Gekoppelde regio 's 

Als u een opslagaccount maakt, selecteert u de primaire regio voor het account. De gekoppelde secundaire regio wordt bepaald op basis van de primaire regio en kan niet worden gewijzigd. Zie voor actuele informatie over regio's die worden ondersteund door Azure [zakelijke continuïteit en herstel na noodgevallen (BCDR): Azure-gebieden gekoppeld](../articles/best-practices-availability-paired-regions.md).
