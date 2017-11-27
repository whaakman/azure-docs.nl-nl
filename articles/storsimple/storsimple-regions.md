---
title: Beschikbaarheid in regio's StorSimple | Microsoft Docs
description: Legt uit Azure-regio's waarin de verschillende modellen van de StorSimple-apparaat beschikbaar zijn.
services: storsimple
documentationcenter: 
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2017
ms.author: alkohli
ms.openlocfilehash: d47109d541a3df93d9234e27e53d1538f6bc4c6e
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="available-regions-for-your-storsimple"></a>Beschikbare regio's voor uw StorSimple

## <a name="overview"></a>Overzicht

De Azure-datacenters werken in meerdere locaties over de hele wereld om te voldoen aan de eisen van prestaties, vereisten en voorkeuren met betrekking tot de locatie van gegevens van de klant. Een Azure Geografie is een bepaald gebied van de wereld met ten minste één Azure-regio. Een Azure-regio is een gebied binnen een Geografie, met een of meer datacenters.

Het kiezen van een Azure-regio is zeer belangrijk en de keuze van de regio is beïnvloed door factoren, zoals gegevens hand vestigingsplaats en onafhankelijkheid, servicebeschikbaarheid, prestaties, kosten en redundantie. Ga voor meer informatie over het kiezen van een regio naar [die Azure-gebied is geschikt voor mij?](https://azure.microsoft.com/overview/datacenters/how-to-choose/)

De keuze van de regio is specifiek voor StorSimple-oplossing bepaald door de volgende factoren:

- De regio's waar de service Manager voor StorSimple-apparaat beschikbaar is.
- De landen waar de fysieke StorSimple, een cloud of een virtueel apparaat beschikbaar is.
- De regio's waar de storage-accounts StorSimple gegevensopslag gevonden voor optimale prestaties worden moeten.

Deze zelfstudie wordt de beschikbaarheid van de regio voor de service Manager voor StorSimple-apparaat, lokale fysieke en de cloud-apparaten beschreven. De informatie in dit artikel is van toepassing op StorSimple 8000 en 1200 reeks apparaten.

## <a name="region-availability-for-storsimple-device-manager-service"></a>Beschikbaarheid in regio's voor service Manager voor StorSimple-apparaat

StorSimple-apparaat Manager-service wordt momenteel ondersteund in 12 openbare gebieden en 2 Azure Government regio's.

U definieert een regio of locatie wanneer u de service Manager voor StorSimple-apparaat voor het eerst maakt. In het algemeen wordt een locatie die het dichtst bij de geografische regio waarin het apparaat is geïmplementeerd gekozen. Maar het apparaat en de service kunnen ook worden geïmplementeerd op verschillende locaties.

Hier volgt een lijst met regio's waar StorSimple-apparaat Manager-service is beschikbaar voor openbare Azure-cloud en kan worden geïmplementeerd.

![storsimple-apparaat-manager-service-regio 's](./media/storsimple-region/storsimple-device-manager-service-regions.png)

De service Manager voor StorSimple-apparaat is voor Azure Government cloud, beschikbaar in Gov ons Iowa en Gov ons Virginia datacenters.

## <a name="region-availability-for-data-stored-in-storsimple"></a>Beschikbaarheid in regio's voor gegevens die zijn opgeslagen in StorSimple

StorSimple-gegevens fysiek worden opgeslagen in Azure storage-accounts en deze accounts zijn beschikbaar in alle Azure-regio's. Wanneer u een Azure storage-account maakt, wordt de primaire locatie van het opslagaccount is gekozen en die bepaalt de regio waar de gegevens zich bevindt.

Wanneer u eerst een StorSimple-apparaat Manager-service maken en een opslagaccount koppelen, kunnen uw StorSimple-apparaat Manager-service en Azure storage zich in twee verschillende locaties. In dat geval moet u de StorSimple--apparaatbeheerfunctie en het Azure Storage-account afzonderlijk maken.

Kies in het algemeen de dichtstbijzijnde regio met uw service voor uw opslagaccount. Echter, de dichtstbijzijnde Microsoft Azure-regio daadwerkelijk mogelijk niet de regio met de laagste latentie. De latentie waarmee de prestaties van de netwerk-service is en wordt daarmee de prestaties van de oplossing. Dus als u een opslagaccount in een andere regio kiest, is het belangrijk te weten wat de latenties tussen uw service en de regio die aan uw storage-account gekoppeld.

Als u van een StorSimple-Cloud-apparaat gebruikmaakt, klikt u vervolgens raden wij aan dat de service en het bijbehorende opslagaccount in dezelfde regio zijn. Storage-accounts in een andere regio kunnen leiden tot slechte prestaties.

## <a name="availability-of-storsimple-device"></a>Beschikbaarheid van StorSimple-apparaat

Afhankelijk van het model, kunnen de StorSimple-apparaten beschikbaar zijn in verschillende regio's of andere landen.

### <a name="storsimple-physical-device-models-81008600"></a>Fysieke StorSimple-apparaat (modellen 8100/8600)

Als u een StorSimple-8100 of 8600 fysiek apparaat gebruikt, is het apparaat is beschikbaar in de volgende landen.

| #  | Land        | #  | Land     | #  | Land      | #  | Land              |
|----|----------------|----|-------------|----|--------------|----|----------------------|
| 1  | Australië      | 16 | Hongkong   | 31 | Nieuw-Zeeland  | 46 | Zuid-Afrika         |
| 2  | Oostenrijk        | 17 | Hongarije     | 32 | Nigeria      | 47 | Zuid-Korea          |
| 3  | Bahrein        | 18 | IJsland     | 33 | Noorwegen       | 48 | Spanje                |
| 4  | België        | 19 | India       | 34 | Peru         | 49 | Sri Lanka            |
| 5  | Brazilië         | 20 | Indonesië   | 35 | Filippijnen  | 50 | Zweden               |
| 6  | Canada         | 21 | Ierland     | 36 | Polen       | 51 | Zwitserland          |
| 7  | Chili          | 22 | Israël      | 37 | Portugal     | 52 | Taiwan               |
| 8  | Colombia       | 23 | Italië       | 38 | Puerto Rico  | 53 | Thailand             |
| 9  | Tsjechië | 24 | Japan       | 39 | Qatar        | 54 | Turkije               |
| 10 | Denemarken        | 25 | Kenia       | 40 | Roemenië      | 55 | Oekraïne              |
| 11 | Egypte          | 26 | Koeweit      | 41 | Rusland       | 56 | Verenigde Arabische Emiraten |
| 12 | Finland        | 27 | Macau       | 42 | Saoedi-Arabië | 57 | Verenigd Koninkrijk       |
| 13 | Frankrijk         | 28 | Maleisië    | 43 | Singapore    | 58 | Verenigde Staten        |
| 14 | Duitsland        | 29 | Mexico      | 44 | Slowakije     | 59 | Vietnam              |
| 15 | Griekenland         | 30 | Nederland | 45 | Slovenië     | 60 | Kroatië              |

Deze lijst wordt gewijzigd wanneer meer landen worden toegevoegd. Voor een meest recente lijst van de locaties, gaat u naar de bijlage opslagmatrix voorwaarden in de [Product voorwaarden](https://www.microsoft.com/en-us/Licensing/product-licensing).

Microsoft kunt verzenden fysieke hardware en hardware reserveonderdelen vervanging voor StorSimple om de locaties in de bovenstaande lijst te bieden.

> [!IMPORTANT]
> Plaats een fysiek StorSimple-apparaat niet in een regio waar StorSimple wordt niet ondersteund. Microsoft kan niet worden voor het verzenden van een vervangende onderdelen naar landen waar StorSimple wordt niet ondersteund.

### <a name="storsimple-cloud-appliance-models-80108020"></a>Cloud van de StorSimple-apparaat (modellen 8010/8020)

Als u een 8010 StorSimple Cloud toestel of 8020, klikt u vervolgens wordt het apparaat ondersteund en beschikbaar in alle regio's waar de onderliggende virtuele machine wordt ondersteund. De 8010 gebruikt een _Standard_A3_ VM die wordt ondersteund in alle Azure-regio's.

Premium-opslag maakt gebruik van de 8020 en _Standard_DS3_ VM voor het maken van een cloud-apparaat. De 8020 wordt ondersteund in Azure-regio's die ondersteuning bieden voor Premium-opslag en _Standard_DS3_ virtuele Azure-machines. Gebruik [deze lijst](https://azure.microsoft.com/regions/services/) om te kijken of zowel **Virtuele Machines > DS-serie** als **Opslag > Schijfruimte** beschikbaar is in uw regio.

### <a name="storsimple-virtual-array-model-1200"></a>Virtuele StorSimple-matrix (Model 1200)

Als een reeks 1200 virtuele StorSimple-matrix, wordt klikt u vervolgens de installatiekopie van de virtuele schijf ondersteund in alle Azure-regio's.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de [prijzen voor verschillende StorSimple modellen](https://azure.microsoft.com/pricing/calculator/#storsimple2).
* Meer informatie over [het beheren van uw StorSimple-opslagaccount](storsimple-8000-manage-storage-accounts.md).
* Meer informatie over het [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).
