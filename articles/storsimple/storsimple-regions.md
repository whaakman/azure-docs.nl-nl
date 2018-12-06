---
title: Beschikbaarheid in regio's StorSimple | Microsoft Docs
description: Legt uit Azure-regio's waarin de verschillende modellen van de StorSimple-apparaat beschikbaar zijn.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2017
ms.author: alkohli
ms.openlocfilehash: 7e35aa8120c455b8cc0d0881a9ceb5120c57deff
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52969665"
---
# <a name="available-regions-for-your-storsimple"></a>Beschikbare regio's voor uw StorSimple

## <a name="overview"></a>Overzicht

De Azure-datacenters worden uitgevoerd in meerdere regio's over de hele wereld om te voldoen aan de eisen van de klant van prestaties, vereisten en voorkeuren wat betreft de locatie van gegevens. Een Azure-Geografie is een bepaald gebied van de hele wereld met ten minste één Azure-regio. Een Azure-regio is een gebied in een geografisch gebied, met een of meer datacenters.

Het kiezen van een Azure-regio is erg belangrijk en de keuze van de regio wordt beïnvloed door factoren zoals gegevensresidentie en soevereiniteit, servicebeschikbaarheid, prestaties, kosten en redundantie. Voor meer informatie over hoe u een regio kiest, gaat u naar [die Azure-regio is geschikt voor mij?](https://azure.microsoft.com/overview/datacenters/how-to-choose/)

De keuze van de regio is specifiek voor StorSimple-oplossing bepaald door de volgende factoren:

- De regio's waar de StorSimple Device Manager-service beschikbaar is.
- De landen waar de fysieke StorSimple, een cloud of een virtueel apparaat beschikbaar is.
- De regio's waar de opslagaccounts die opslaan van StorSimple-gegevens geplaatst voor optimale prestaties worden.

Deze zelfstudie wordt de beschikbaarheid in regio voor de StorSimple Device Manager-service, de fysieke on-premises en de cloud-apparaten beschreven. De informatie in dit artikel is van toepassing op StorSimple 8000 en apparaten van 1200-serie.

## <a name="region-availability-for-storsimple-device-manager-service"></a>Beschikbaarheid in regio's voor StorSimple Device Manager-service

StorSimple Device Manager-service wordt momenteel ondersteund in 12 openbare regio's en 2 Azure Government-regio's.

U definieert een regio of locatie wanneer u de service StorSimple Device Manager voor het eerst maakt. In het algemeen is een locatie die het dichtst bij de geografische regio waar het apparaat is geïmplementeerd gekozen. Maar het apparaat en de service kunnen ook worden geïmplementeerd op verschillende locaties.

Hier volgt een lijst met regio's waar StorSimple Device Manager-service is beschikbaar voor openbare Azure-cloud en kan worden geïmplementeerd.

![storsimple-apparaat-manager-service-regio 's](./media/storsimple-region/storsimple-device-manager-service-regions.png)

De StorSimple Device Manager-service is voor Azure Government-cloud, beschikbaar in VS (overheid)-Iowa en VS (overheid) Virginia datacenters.

## <a name="region-availability-for-data-stored-in-storsimple"></a>Beschikbaarheid in regio's voor gegevens die zijn opgeslagen in StorSimple

StorSimple-gegevens fysiek worden opgeslagen in Azure storage-accounts en deze accounts zijn beschikbaar in alle Azure-regio's. Wanneer u een Azure storage-account maakt, wordt de primaire locatie van het opslagaccount dat is gekozen en waarmee wordt bepaald met de regio waar de gegevens zich bevinden.

Wanneer u eerst een StorSimple Device Manager-service maken en een opslagaccount koppelen, kunnen uw StorSimple Device Manager-service en Azure storage zich in twee verschillende locaties bevinden. In dat geval moet u de StorSimple--apparaatbeheerfunctie en het Azure Storage-account afzonderlijk maken.

Kies in het algemeen de dichtstbijzijnde regio voor uw service voor uw storage-account. Echter, de dichtstbijzijnde regio voor de Microsoft Azure mogelijk niet de regio met de laagste latentie. Het is de latentie die bepaalt de netwerkprestaties van de service en wordt daarmee de prestaties van de oplossing. Dus als u een opslagaccount in een andere regio kiest, is het belangrijk te weten wat de latenties zijn tussen uw service en de regio die is gekoppeld aan uw storage-account.

Als u een StorSimple-Cloudapparaat gebruikt, klikt u vervolgens het beste dat de service en het bijbehorende opslagaccount zich in dezelfde regio bevinden. Storage-accounts in een andere regio kunnen leiden tot slechte prestaties.

## <a name="availability-of-storsimple-device"></a>Beschikbaarheid van StorSimple-apparaat

Afhankelijk van het model, kunnen de StorSimple-apparaten beschikbaar zijn in verschillende fysieke locaties of landen.

### <a name="storsimple-physical-device-models-81008600"></a>Fysieke StorSimple-apparaat (modellen 8100/8600)

Als u een StorSimple 8100 of 8600 fysiek apparaat, is het apparaat is beschikbaar in de volgende landen.

| #  | Land        | #  | Land     | #  | Land      | #  | Land              |
|----|----------------|----|-------------|----|--------------|----|----------------------|
| 1  | Australië      | 16 | Hongkong   | 31 | Nieuw-Zeeland  | 46 | Zuid-Afrika         |
| 2  | Oostenrijk        | 17 | Hongarije     | 32 | Nigeria      | 47 | Zuid-Korea          |
| 3  | Bahrein        | 18 | IJsland     | 33 | Noorwegen       | 48 | Spanje                |
| 4  | België        | 19 | India       | 34 | Peru         | 49 | Sri Lanka            |
| 5  | Brazilië         | 20 | Indonesië   | 35 | Filipijnen  | 50 | Zweden               |
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

Deze lijst wordt gewijzigd wanneer er meer landen worden toegevoegd. Voor een actuele lijst van de verschillende geografische gebieden, gaat u naar de bijlage Storage Array voorwaarden in de [productvoorwaarden](https://www.microsoft.com/en-us/licensing/product-licensing/products).

Microsoft kunt verzenden fysieke hardware en hardware reserveonderdelen vervanging voor StorSimple om de locaties in de voorgaande lijst te bieden.

> [!IMPORTANT]
> Plaats een fysiek StorSimple-apparaat niet in een regio waar StorSimple wordt niet ondersteund. Microsoft is niet mogelijk zijn voor het verzenden van eventuele vervangende onderdelen naar landen waar StorSimple wordt niet ondersteund.

### <a name="storsimple-cloud-appliance-models-80108020"></a>StorSimple-Cloudapparaat (8010/8020 modellen)

Als u een StorSimple Cloud Appliance 8010 of 8020, klikt u vervolgens wordt het apparaat ondersteund en beschikbaar in alle regio's waar de onderliggende virtuele machine wordt ondersteund. De 8010 gebruikt een _Standard_A3_ virtuele machine die wordt ondersteund in alle Azure-regio's.

De 8020 maakt gebruik van premium storage en _Standard_DS3_ VM naar een cloudapparaat maken. De 8020 wordt ondersteund in Azure-regio's die ondersteuning bieden voor Premium Storage en _Standard_DS3_ virtuele Azure-machines. Gebruik [deze lijst](https://azure.microsoft.com/regions/services/) om te kijken of zowel **Virtuele Machines > DS-serie** als **Opslag > Schijfruimte** beschikbaar is in uw regio.

### <a name="storsimple-virtual-array-model-1200"></a>StorSimple Virtual Array (Model 1200)

Als u een 1200-serie StorSimple Virtual Array, wordt klikt u vervolgens de installatiekopie van de virtuele schijf ondersteund in alle Azure-regio's.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de [prijzen voor verschillende StorSimple modellen](https://azure.microsoft.com/pricing/calculator/#storsimple2).
* Meer informatie over [beheren van uw StorSimple-opslagaccount](storsimple-8000-manage-storage-accounts.md).
* Meer informatie over het [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).
