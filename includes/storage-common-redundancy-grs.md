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
ms.openlocfilehash: efa593d0ff0043d81574b67192deed30933e1e40
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219848"
---
Geografisch redundante opslag (GRS) is zodanig ontworpen dat ten minste 99,99999999999999% (16 9's) duurzaamheid van objecten in een bepaald jaar door uw gegevens te repliceren naar een secundaire regio die grote afstand van de primaire regio. Als uw storage-account GRS is ingeschakeld heeft, zijn uw gegevens zijn duurzame zelfs in geval van een volledige regionale stroomstoring of een ramp waarbij de primaire regio niet hersteld.

Als u hebt gekozen voor GRS, hebt u twee andere opties kunt kiezen uit:

* GRS worden uw gegevens gerepliceerd naar een ander datacenter in een secundaire regio, maar deze beschikbaar is voor alleen-lezen zijn als Microsoft een failover van de primaire naar secundaire regio initieert. 
* Geo-redundante opslag met leestoegang (RA-GRS) is gebaseerd op GRS. RA-GRS worden uw gegevens gerepliceerd naar een ander datacenter in een secundaire regio, en biedt u ook de optie voor het lezen van de secundaire regio. Met RA-GRS, kunt u lezen van de secundaire regio, ongeacht of Microsoft een failover van de primaire naar secundaire regio initieert. 

Voor een opslagaccount met GRS of RA-GRS wordt ingeschakeld, worden alle gegevens eerst gerepliceerd met lokaal redundante opslag (LRS). Een update is eerst gericht op de primaire locatie en gerepliceerd met behulp van LRS. De update wordt vervolgens asynchroon gerepliceerd naar de secundaire regio met GRS. Wanneer gegevens worden geschreven naar de secundaire locatie, wordt het ook binnen die locatie met behulp van LRS gerepliceerd. 

De primaire en secundaire regio replica's beheren in afzonderlijke foutdomeinen en upgrade-domeinen binnen een opslagschaaleenheid. De opslagschaaleenheid is de basic-replicatie-eenheid binnen het datacenter. Replicatie op dit niveau wordt geleverd door LRS; Zie voor meer informatie, [lokaal redundante opslag (LRS): de gegevensredundantie lage kosten voor Azure Storage](../articles/storage/common/storage-redundancy-lrs.md).

Houd er rekening mee bij het bepalen van welke replicatieoptie te gebruiken:

* Zone-redundante opslag (ZRS) biedt hoge beschikbaarheid met synchrone replicatie en mogelijk een betere keuze voor enkele scenario's dan GRS of RA-GRS. Zie voor meer informatie over ZRS [ZRS](../articles/storage/common/storage-redundancy-zrs.md).
* Asynchrone replicatie omvat een vertraging vanaf het moment dat gegevens worden geschreven naar de primaire regio op wanneer deze worden gerepliceerd naar de secundaire regio. In het geval van een regionaal noodgeval wijzigingen die nog niet hebt zijn gerepliceerd naar de secundaire regio mogelijk verloren als die gegevens van de primaire regio niet kan worden hersteld.
* Met GRS, de replica is niet beschikbaar voor lezen of schrijven, tenzij Microsoft een failover naar de secundaire regio initieert. In het geval van een failover, u zult hebt gelezen en schrijftoegang tot die gegevens na de failover is voltooid. Zie voor meer informatie, [Disaster recovery guidance](../articles/storage/common/storage-disaster-recovery-guidance.md).
* Als de toepassing nodig heeft om te lezen van de secundaire regio, schakelt u RA-GRS.