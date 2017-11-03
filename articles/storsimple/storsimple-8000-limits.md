---
title: StorSimple 8000 series systeemlimieten | Microsoft Docs
description: Hierin wordt beschreven systeemlimieten en aanbevolen grootten voor StorSimple 8000 series onderdelen en verbindingen.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: c7392678-0924-46c6-9c59-1665cb9b6586
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/28/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cc3c0ad193af7625c8c4c1c2e82b6bdc8be33310
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-are-storsimple-8000-series-system-limits"></a>Wat zijn StorSimple 8000 series system beperkingen?

## <a name="overview"></a>Overzicht

StorSimple biedt schaalbare en flexibele opslag voor uw datacenter. Er zijn echter enkele beperkingen die u rekening houden moet bij het plannen, implementeren en uw StorSimple-oplossing werkt. De volgende tabel worden deze limieten beschreven en vindt u enkele aanbevelingen zodat u optimaal van uw StorSimple-oplossing kunt ophalen.

| Limiet-id | Limiet | Opmerkingen |
| --- | --- | --- |
| Maximum aantal opslagaccountreferenties |64 | |
| Maximum aantal volumecontainers |64 | |
| Maximum aantal volumes |255 | |
| Maximum aantal lokaal vastgemaakte volumes |32 | |
| Maximum aantal planningen per bandbreedtesjabloon |168 |Een planning voor elk uur wordt elke dag van de week (24 * 7). |
| Maximale grootte van een gelaagd volume op fysieke apparaten |64 TB voor 8100 en 8600 |8100 en 8600 zijn fysieke apparaten. |
| Maximale grootte van een gelaagd volume op virtuele apparaten in Azure |8010 30 TB <br></br> 64 TB voor de 8020 |8010 als de 8020 zijn virtuele apparaten in Azure die gebruikmaken van Standard-opslag- en Premium-opslag respectievelijk. |
| Maximale grootte van een lokaal vastgemaakt volume op een fysieke apparaten |8.5 TB voor 8100 <br></br> 22,5 TB voor 8600 |8100 en 8600 zijn fysieke apparaten. |
| Maximum aantal iSCSI-verbindingen |512 | |
| Maximum aantal verbindingen iSCSI-initiators |512 | |
| Maximum aantal access control records per apparaat |64 | |
| Maximum aantal volumes per back-upbeleid |20 | |
| Maximum aantal back-ups behouden per schema (in een back-upbeleid) |64 | |
| Maximum aantal planningen per back-upbeleid |10 | |
| Maximum aantal momentopnamen van elk type kunnen worden bewaard per volume |256 |Dit is inclusief lokale momentopnamen en cloud worden opgeslagen. |
| Maximum aantal momentopnamen die gebruikt in elk apparaat worden kunnen |10.000 | |
| Maximum aantal volumes die kunnen worden verwerkt in de parallelle back-up, herstel, of te klonen |16 |<ul><li>Als er meer dan 16 volumes, worden deze opeenvolgend verwerkt verwerking sleuven beschikbaar komen.</li><li>Nieuwe back-ups van een kloon of een herstelde gelaagd volume is pas mogelijk nadat de bewerking is voltooid. Back-ups zijn echter voor een lokaal volume nadat het volume online is toegestaan.</li></ul> |
| Terugzetten en herstellen tijd voor gelaagde volumes klonen |< 2 minuten |<ul><li>Het volume wordt beschikbaar gesteld binnen twee minuten na het terugzetten of kloon bewerking, ongeacht de grootte van het volume.</li><li>De prestaties van het volume is mogelijk in eerste instantie langzamer dan normaal omdat de meeste van de gegevens en metagegevens zich nog steeds in de cloud. Prestaties kan verhogen als gegevensstromen vanuit de cloud naar het StorSimple-apparaat.</li><li>De totale tijd voor het downloaden van metagegevens is afhankelijk van de grootte van het toegewezen. Metagegevens is automatisch in het apparaat op de achtergrond op de frequentie van 5 minuten per TB aan volumegegevens toegewezen gebracht. Deze snelheid kan worden beïnvloed door internetbandbreedte naar de cloud.</li><li>Het terugzetten of de kloonbewerking is voltooid als alle metagegevens op het apparaat is.</li><li>Back-upbewerkingen kunnen niet worden uitgevoerd totdat het herstel of de kloonbewerking is volledig is voltooid. |
| Herstel de tijd voor lokaal vastgemaakte volumes herstellen |< 2 minuten |<ul><li>Het volume wordt beschikbaar gesteld binnen twee minuten na de herstelbewerking, ongeacht de grootte van het volume.</li><li>De prestaties van het volume is mogelijk in eerste instantie langzamer dan normaal omdat de meeste van de gegevens en metagegevens zich nog steeds in de cloud. Prestaties kan verhogen als gegevensstromen vanuit de cloud naar het StorSimple-apparaat.</li><li>De totale tijd voor het downloaden van metagegevens is afhankelijk van de grootte van het toegewezen. Metagegevens is automatisch in het apparaat op de achtergrond op de frequentie van 5 minuten per TB aan volumegegevens toegewezen gebracht. Deze snelheid kan worden beïnvloed door internetbandbreedte naar de cloud.</li><li>In tegenstelling tot gelaagde volumes, voor lokaal vastgemaakte volumes wordt het volumegegevens ook lokaal gedownload op het apparaat. De herstelbewerking is voltooid wanneer alle gegevens op het volume is ingesteld op het apparaat.</li><li>De herstelbewerkingen mogelijk lang. De totale tijd voor de terugzetbewerking niet voltooien, is afhankelijk van de grootte van de betreffende lokale volume, de Internet-bandbreedte en de bestaande gegevens op het apparaat. Back-upbewerkingen op de lokaal vastgemaakt volume zijn toegestaan terwijl de herstelbewerking uitgevoerd wordt. |
| Verwerkingssnelheid voor cloudmomentopnamen van de |15 minuten/TB |<ul><li>Minimale tijd om aan de cloud snapshot gereed voor het uploaden, per TB aan volumegegevens in de back-up toegewezen. </li><li> Tijd van totale cloud momentopname wordt berekend door deze tijd toe te voegen aan de tijd voor het uploaden van momentopname, dat wordt beïnvloed door de Internet-bandbreedte naar de cloud. |
| Maximum aantal clients lezen/schrijven doorvoer (wanneer geleverd vanuit de SSD-laag) * |920/720 MB/s met een enkele 10 GbE-netwerkinterface |Maximaal 2 x met MPIO en twee netwerkinterfaces. |
| Maximum aantal clients lezen/schrijven doorvoer (wanneer geleverd vanuit de HDD-laag) * |120/250 MB/s | |
| Maximum aantal clients lezen/schrijven doorvoer (wanneer geleverd vanuit de cloudlaag) * voor Update 3 en hoger ** |40/60 MB/s voor gelaagde volumes<br><br>60/80 MB/s voor gelaagde volumes archivering optie is geselecteerd tijdens het maken van volume |Lees doorvoer is afhankelijk van clients genereren en bijhouden van voldoende i/o-wachtrijdiepte. <br><br>Snelheid bereikt, is afhankelijk van de snelheid van het onderliggende storage-account gebruikt. |

&#42; Maximale doorvoer per i/o-type is gemeten met 100 procent lezen en schrijven van 100 procent scenario's. Werkelijke doorvoer mogelijk lager en is afhankelijk van i/o mix en de voorwaarden.

&#42; &#42; Prestaties cijfers vóór de Update 3 mogelijk lager.

## <a name="next-steps"></a>Volgende stappen
Controleer de [StorSimple systeemvereisten](storsimple-8000-system-requirements.md).

