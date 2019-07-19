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
ms.openlocfilehash: 3aa5310589101fa66fd70cc8d5449fbef80f02fa
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68286179"
---
Geografisch redundante opslag (GRS) is ontworpen om ten minste 99.99999999999999% (16 9) duurzaamheid van objecten over een bepaald jaar te bieden door uw gegevens te repliceren naar een secundaire regio die honderden kilo meters van de primaire regio is. Als voor uw opslag account GRS is ingeschakeld, zijn uw gegevens duurzaam, zelfs in het geval van een volledige regionale onderbreking of een ramp waarbij de primaire regio niet kan worden hersteld.

Als u ervoor kiest voor GRS, hebt u twee gerelateerde opties waaruit u kunt kiezen:

* GRS repliceert uw gegevens naar een ander Data Center in een secundaire regio, maar deze gegevens zijn alleen-lezen als micro soft een failover initieert van de primaire naar de secundaire regio. 
* Geografisch redundante opslag met lees toegang (RA-GRS) is gebaseerd op GRS. RA-GRS repliceert uw gegevens naar een ander Data Center in een secundaire regio en biedt u ook de mogelijkheid om te lezen uit de secundaire regio. Met RA-GRS kunt u lezen uit de secundaire regio, ongeacht of micro soft een failover initieert van de primaire naar de secundaire regio. 

Voor een opslag account waarvoor GRS of RA-GRS is ingeschakeld, worden alle gegevens eerst gerepliceerd met lokaal redundante opslag (LRS). Een update wordt eerst doorgevoerd op de primaire locatie en gerepliceerd met behulp van LRS. De update wordt vervolgens asynchroon gerepliceerd naar de secundaire regio met behulp van GRS. Wanneer gegevens naar de secundaire locatie worden geschreven, worden deze ook gerepliceerd binnen die locatie met behulp van LRS. 

De primaire en secundaire regio's beheren replica's in afzonderlijke fout domeinen en upgraden domeinen binnen een opslag schaal eenheid. De eenheid voor opslag schaal is de basis replicatie-eenheid binnen het Data Center. Replicatie op dit niveau wordt verzorgd door LRS. Zie [voor meer informatie lokaal redundante opslag (LRS): Gegevensredundantie met lage kosten voor Azure Storage](../articles/storage/common/storage-redundancy-lrs.md).

Houd bij het bepalen van de te gebruiken replicatie optie de volgende punten in acht:

* Zone-redundante opslag (ZRS) biedt een hoge Beschik baarheid met synchrone replicatie en is mogelijk een betere keuze voor sommige scenario's dan GRS of RA-GRS. Zie [ZRS](../articles/storage/common/storage-redundancy-zrs.md)voor meer informatie over ZRS.
* Asynchrone replicatie vergt een vertraging van de tijd dat gegevens naar de primaire regio worden geschreven, naar wanneer deze wordt gerepliceerd naar de secundaire regio. In het geval van een regionale ramp kunnen wijzigingen die nog niet zijn gerepliceerd naar de secundaire regio, verloren gaan als de gegevens niet kunnen worden hersteld vanuit de primaire regio.
* Met GRS is de replica niet beschikbaar voor lees-of schrijf toegang tenzij micro soft een failover naar de secundaire regio initieert. In het geval van een failover hebt u lees-en schrijf toegang tot deze gegevens nadat de failover is voltooid. Zie [richt lijnen voor herstel na nood gevallen](../articles/storage/common/storage-disaster-recovery-guidance.md)voor meer informatie.
* Als uw toepassing moet worden gelezen uit de secundaire regio, schakelt u RA-GRS in.
