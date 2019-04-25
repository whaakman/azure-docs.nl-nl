---
title: StorSimple 8000-serie systeemlimieten | Microsoft Docs
description: Beschrijving van systeemlimieten en aanbevolen grootten voor onderdelen van de StorSimple 8000-serie en verbindingen.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: c7392678-0924-46c6-9c59-1665cb9b6586
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/28/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a0053f950b36351b06d08630cbf9977f53f2ed47
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60319561"
---
# <a name="what-are-storsimple-8000-series-system-limits"></a>Wat zijn de limieten voor StorSimple 8000-serie systeem?

## <a name="overview"></a>Overzicht

StorSimple biedt schaalbare en flexibele opslag voor uw datacenter. Er zijn echter enkele beperkingen waarmee u rekening houden moet als u plannen, implementeren en gebruiken van uw StorSimple-oplossing. De volgende tabel worden deze limieten beschreven en vindt u enkele aanbevelingen zodat u het meeste uit uw StorSimple-oplossing kunt halen.

| Limiet-id | Limiet | Opmerkingen |
| --- | --- | --- |
| Maximum aantal opslagaccountreferenties |64 | |
| Maximum aantal volumecontainers |64 | |
| Maximale aantal volumes |255 | |
| Maximum aantal lokaal vastgemaakte volumes |32 | |
| Maximum aantal planningen per bandbreedtesjabloon |168 |Een planning voor elk uur, elke dag van de week (24 uur per dag * 7). |
| Maximale grootte van een gelaagd volume op fysieke apparaten |64 TB voor 8100- en 8600 |8100- en 8600 zijn fysieke apparaten. |
| Maximale grootte van een gelaagd volume op de virtuele apparaten in Azure |30 TB voor 8010 <br></br> 64 TB voor de 8020 |8010 en 8020 zijn virtuele apparaten in Azure die respectievelijk gebruikmaken van Standard Storage en Premium-opslag. |
| Maximale grootte van een lokaal vastgemaakt volume op fysieke apparaten |8,5 TB voor 8100 <br></br> 22,5 TB voor 8600 |8100- en 8600 zijn fysieke apparaten. |
| Maximum aantal iSCSI-verbindingen |512 | |
| Maximum aantal verbindingen iSCSI-initiators |512 | |
| Maximum aantal access control records per apparaat |64 | |
| Maximale aantal volumes per back-upbeleid |20 | |
| Maximum aantal back-ups per schema (in een back-upbeleid) behouden |64 | |
| Maximum aantal planningen per back-upbeleid |10 | |
| Maximum aantal momentopnamen van elk type per volume kunnen worden bewaard |256 |Hieronder vallen de lokale momentopnamen en cloud-momentopnamen. |
| Maximum aantal momentopnamen die gebruikt in elk apparaat worden kunnen |10.000 | |
| Maximale aantal volumes dat voor back-up en herstel parallel kunnen worden verwerkt of klonen |16 |<ul><li>Als er meer dan 16 volumes, worden ze opeenvolgend verwerkt; verwerking sleuven beschikbaar komen.</li><li>Nieuwe back-ups van een gekloonde of een herstelde gelaagd volume is niet mogelijk totdat de bewerking is voltooid. Back-ups zijn echter voor een lokaal volume nadat het volume online is toegestaan.</li></ul> |
| Terugzetten en herstellen van tijd voor gelaagde volumes klonen |< 2 minuten |<ul><li>Het volume wordt beschikbaar gesteld binnen twee minuten van herstel- of kloon bewerking, ongeacht de grootte van het volume.</li><li>In eerste instantie de volume prestaties mogelijk langzamer dan normaal omdat de meeste van de gegevens en metagegevens is nog steeds bevindt zich in de cloud. Prestaties verhogen als de gegevensstromen vanuit de cloud met de StorSimple-apparaat.</li><li>De totale tijd voor het downloaden van metagegevens is afhankelijk van de grootte van het toegewezen. Metagegevens is automatisch in het apparaat op de achtergrond met de snelheid van 5 minuten per TB aan gegevens toegewezen volume gebracht. Deze frequentie kan worden beïnvloed door internetbandbreedte naar de cloud.</li><li>Het terugzetten of de kloonbewerking is voltooid wanneer alle metagegevens op het apparaat is.</li><li>Back-upbewerkingen kunnen niet worden uitgevoerd totdat het herstel of de kloonbewerking is volledig is voltooid. |
| Herstellen van tijd voor lokaal vastgemaakte volumes herstellen |< 2 minuten |<ul><li>Het volume wordt beschikbaar gesteld binnen twee minuten van de herstelbewerking, ongeacht de grootte van het volume.</li><li>In eerste instantie de volume prestaties mogelijk langzamer dan normaal omdat de meeste van de gegevens en metagegevens is nog steeds bevindt zich in de cloud. Prestaties verhogen als de gegevensstromen vanuit de cloud met de StorSimple-apparaat.</li><li>De totale tijd voor het downloaden van metagegevens is afhankelijk van de grootte van het toegewezen. Metagegevens is automatisch in het apparaat op de achtergrond met de snelheid van 5 minuten per TB aan gegevens toegewezen volume gebracht. Deze frequentie kan worden beïnvloed door internetbandbreedte naar de cloud.</li><li>In tegenstelling tot gelaagde volumes, voor lokaal vastgemaakte volumes, wordt ook lokaal gegevens op het volume gedownload op het apparaat. De herstelbewerking is voltooid wanneer alle gegevens op het volume is ingesteld op het apparaat.</li><li>De bewerkingen voor het herstellen mogelijk lang. De totale tijd voor de terugzetbewerking niet voltooien, is afhankelijk van de grootte van de ingerichte lokaal volume, de Internet-bandbreedte en de bestaande gegevens op het apparaat. Back-upbewerkingen op de lokaal vastgemaakte volume zijn toegestaan terwijl de herstelbewerking uitgevoerd wordt. |
| Verwerkingssnelheid voor cloudmomentopnamen |15 minuten/TB |<ul><li>Gereed voor het uploaden, per TB aan volumegegevens in de back-up toegewezen snapshot-minimale tijd in de cloud. </li><li> Tijd van totale cloud-momentopname wordt berekend door het toevoegen van dit moment op de tijd voor het uploaden van momentopname, dat wordt beïnvloed door de Internet-bandbreedte naar de cloud. |
| Maximum aantal clients lezen/schrijven-doorvoer (wanneer worden aangeleverd vanuit de SSD-laag) * |920/720 MB/s met een enkele 10 GbE-netwerkinterface |Maximaal 2 x met MPIO- en twee netwerkinterfaces. |
| Maximum aantal clients lezen/schrijven-doorvoer (wanneer worden aangeleverd vanuit de HDD-laag) * |120/250 MB/s | |
| Maximum aantal clients lezen/schrijven-doorvoer (wanneer worden aangeleverd vanuit de cloudlaag) * voor Update 3 en hoger ** |40/60 MB/s voor gelaagde volumes<br><br>60/80 MB/s voor gelaagde volumes met de archivering optie geselecteerd tijdens het maken van volume |Lezen doorvoer is afhankelijk van clients genereren en beheren van voldoende i/o-wachtrijdiepte. <br><br>Snelheid is bereikt, is afhankelijk van de snelheid van de onderliggende opslagaccount dat wordt gebruikt. |

&#42;Maximale doorvoer per i/o-type is gemeten met 100 procent lezen en schrijven van 100 procent scenario's. Werkelijke doorvoer mogelijk lager en is afhankelijk van i/o-netwerk van de voorwaarden en combineren.

&#42;&#42;Prestatiecijfers vóór Update 3 kan lager zijn.

## <a name="next-steps"></a>Volgende stappen
Controleer de [systeemvereisten voor StorSimple](storsimple-8000-system-requirements.md).

