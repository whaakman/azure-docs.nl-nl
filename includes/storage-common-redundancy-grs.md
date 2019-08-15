---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/28/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 28b53864abc37a880a9573cc9657231aff862336
ms.sourcegitcommit: df7942ba1f28903ff7bef640ecef894e95f7f335
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69029776"
---
Geografisch redundante opslag (GRS) is ontworpen om ten minste 99.99999999999999% (16 9) duurzaamheid van objecten over een bepaald jaar te bieden door uw gegevens te repliceren naar een secundaire regio die honderden kilo meters van de primaire regio is. Als voor uw opslag account GRS is ingeschakeld, zijn uw gegevens duurzaam, zelfs in het geval van een volledige regionale onderbreking of een ramp waarbij de primaire regio niet kan worden hersteld.

Als u ervoor kiest voor GRS, hebt u twee gerelateerde opties waaruit u kunt kiezen:

* GRS repliceert uw gegevens naar een ander Data Center in een secundaire regio, maar deze gegevens zijn alleen-lezen als micro soft een failover initieert van de primaire naar de secundaire regio.
* Geografisch redundante opslag met lees toegang (RA-GRS) is gebaseerd op GRS. RA-GRS repliceert uw gegevens naar een ander Data Center in een secundaire regio en biedt u ook de mogelijkheid om te lezen uit de secundaire regio. Met RA-GRS kunt u lezen uit de secundaire regio, ongeacht of micro soft een failover initieert van de primaire naar de secundaire regio.

Voor een opslag account waarvoor GRS of RA-GRS is ingeschakeld, worden alle gegevens eerst gerepliceerd met lokaal redundante opslag (LRS). Een update wordt eerst doorgevoerd op de primaire locatie en gerepliceerd met behulp van LRS. De update wordt vervolgens asynchroon gerepliceerd naar de secundaire regio met behulp van GRS. Wanneer gegevens naar de secundaire locatie worden geschreven, worden deze ook gerepliceerd binnen die locatie met behulp van LRS.

De primaire en secundaire regio's beheren replica's in afzonderlijke fout domeinen en upgraden domeinen binnen een opslag schaal eenheid. De eenheid voor opslag schaal is de basis replicatie-eenheid binnen het Data Center. Replicatie op dit niveau wordt verzorgd door LRS. Zie [voor meer informatie lokaal redundante opslag (LRS): Gegevensredundantie met lage kosten voor Azure Storage](../articles/storage/common/storage-redundancy-lrs.md).

Houd bij het bepalen van de te gebruiken replicatie optie de volgende punten in acht:

* Geo-zone-redundante opslag (GZRS) biedt een hoge Beschik baarheid en maximale duurzaamheid door gegevens synchroon te repliceren over drie Azure-beschikbaarheids zones en vervolgens gegevens asynchroon te repliceren naar de secundaire regio. U kunt ook lees toegang tot de secundaire regio inschakelen. GZRS is ontworpen om ten minste 99.99999999999999% (16 9) duurzaamheid van objecten in een bepaald jaar te bieden. Zie [geo-zone-redundante opslag voor hoge Beschik baarheid en maximale duurzaamheid (preview)](../articles/storage/common/storage-redundancy-gzrs.md)voor meer informatie over GZRS.
* Zone-redundante opslag (ZRS) biedt een hoge Beschik baarheid met synchrone replicatie over drie Azure-beschikbaarheids zones en is mogelijk een betere keuze voor sommige scenario's dan GRS of RA-GRS. Zie [ZRS](../articles/storage/common/storage-redundancy-zrs.md)voor meer informatie over ZRS.
* Asynchrone replicatie vergt een vertraging van de tijd dat gegevens naar de primaire regio worden geschreven, naar wanneer deze wordt gerepliceerd naar de secundaire regio. In het geval van een regionale ramp kunnen wijzigingen die nog niet zijn gerepliceerd naar de secundaire regio, verloren gaan als de gegevens niet kunnen worden hersteld vanuit de primaire regio.
* Met GRS is de replica niet beschikbaar voor lees-of schrijf toegang tenzij micro soft een failover naar de secundaire regio initieert. In het geval van een failover hebt u lees-en schrijf toegang tot deze gegevens nadat de failover is voltooid. Zie [richt lijnen voor herstel na nood gevallen](../articles/storage/common/storage-disaster-recovery-guidance.md)voor meer informatie.
* Als uw toepassing moet worden gelezen uit de secundaire regio, schakelt u RA-GRS of RA-GZRS (preview) in.
