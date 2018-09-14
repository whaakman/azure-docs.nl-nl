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
ms.openlocfilehash: 922f4d5dd8c71bc9365523b1d539d1b0754fff15
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45580936"
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