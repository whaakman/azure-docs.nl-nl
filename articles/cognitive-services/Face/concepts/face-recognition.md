---
title: Face-opname-concepten
titleSuffix: Azure Cognitive Services
description: Meer informatie over concepten over gezichtsherkenning.
services: cognitive-services
author: PatrickFarley
manager: nitime
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: pafarley
ms.openlocfilehash: fa38c492530cb8938e49bc15e13fdd39ed5b6f1c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65890877"
---
# <a name="face-recognition-concepts"></a>Face-opname-concepten

In dit artikel worden de concepten van de bewerkingen controleren, Zoek vergelijkbare, groep en Identificeer gezichten erkenning en de onderliggende gegevensstructuren. Algemeen, erkenning beschrijving van het werk van het vergelijken van twee verschillende gezichten om te bepalen of ze vergelijkbaar met de zijn of deel uitmaken van dezelfde persoon.

## <a name="recognition-related-data-structures"></a>Gegevens met betrekking tot de opname-structuren

De opname-bewerkingen gebruik voornamelijk de volgende gegevensstructuren. Deze objecten worden opgeslagen in de cloud en kunnen worden verwezen door de ID-tekenreeksen. Id-tekenreeksen zijn altijd uniek zijn binnen een abonnement. De van naamvelden kunnen worden gedupliceerd.

|Name|Description|
|:--|:--|
|DetectedFace| Deze weergave één gezicht is opgehaald door de [gezichtsdetectie](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md) bewerking. De ID verloopt 24 uur nadat deze gemaakt.|
|PersistedFace| Wanneer DetectedFace objecten worden toegevoegd aan een groep, zoals FaceList of persoon, worden ze PersistedFace objecten. Ze kunnen worden [opgehaald](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c) op een tijd en niet verlopen.|
|[FaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b) of [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc)| Deze gegevensstructuur is een lijst met verschillende PersistedFace-objecten. Een FaceList heeft een unieke ID, een naam en eventueel een tekenreeks van de gegevens.|
|[Person](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)| Deze gegevensstructuur is een lijst met PersistedFace-objecten die deel uitmaken van dezelfde persoon. Er is een unieke ID, een naam en eventueel een tekenreeks van de gegevens.|
|[PersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) or [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)| Deze gegevensstructuur is een verschillende lijst van objecten van de persoon. Er is een unieke ID, een naam en eventueel een tekenreeks van de gegevens. Een PersonGroup moet [getrainde](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) voordat deze kan worden gebruikt in bewerkingen voor herkenning.|

## <a name="recognition-operations"></a>Herkenning van bewerkingen

In deze sectie wordt uitgelegd hoe de vier erkenning bewerkingen gebruiken voor de gegevensstructuren die eerder is beschreven. Zie voor een uitgebreide beschrijving van elke bewerking erkenning [overzicht](../Overview.md).

### <a name="verify"></a>Verifiëren

De [controleren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) bewerking nodig is een gezichts-ID van het DetectedFace of PersistedFace en een andere face ID of een object persoon en bepaalt u of ze deel uitmaken van dezelfde persoon. Als u in een persoon-object doorgeeft, kunt u eventueel doorgeven in een PersonGroup waartoe deze persoon behoort om prestaties te verbeteren.

### <a name="find-similar"></a>Zoek vergelijkbare

De [Zoek vergelijkbare](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) bewerking nodig is een gezichts-ID van het DetectedFace of PersistedFace en een FaceList of een matrix met andere face id's. Het resultaat een kleinere FaceList van gezichten wordt uitgevoerd die vergelijkbaar met de bepaald gezicht zijn met een FaceList. Met een matrix met face id's retourneert deze op dezelfde manier een kleinere matrix.

### <a name="group"></a>Groep

De [groep](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) bewerking nodig is een matrix met verschillende face id's van het DetectedFace of PersistedFace en retourneert de dezelfde id's die zijn ondergebracht in verschillende kleinere matrices. Elke 'groepen'-matrix bevat face id's die er ongeveer als. Een matrix van één 'messyGroup' bevat face id's voor dat er geen overeenkomsten zijn gevonden.

### <a name="identify"></a>Identificeren

De [identificeren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) bewerking nodig is een of meer face id's van het DetectedFace of PersistedFace en een PersonGroup en retourneert een lijst van persoonsobjecten die elk gezicht deel kan uitmaken. Persoon objecten zijn verpakt als kandidaat objecten, waarvoor een voorspelling vertrouwen waarde geretourneerd.

## <a name="input-data"></a>Invoergegevens

Gebruik de volgende tips om ervoor te zorgen dat uw invoer afbeeldingen de meest nauwkeurige resultaten geven:

* De afbeelding ondersteunde indelingen zijn JPEG, PNG-, GIF-bestand (het eerste frame), BMP.
* De grootte van de installatiekopie-bestand mag niet groter zijn dan 4 MB zijn.
* Wanneer u persoonsobjecten maakt, gebruikt u foto's die zijn uitgerust met verschillende soorten hoeken en belichting.
* Sommige gezichten kunnen niet zoals vanwege technische problemen, worden herkend:
  * Installatiekopieën met extreme belichting, bijvoorbeeld ernstige achtergrondverlichting voor.
  * Obstakels die een of beide ogen blokkeren.
  * De verschillen in haar soort of videodetectie haar.
  * Wijzigingen in gezichtsherkenning uiterlijk vanwege leeftijd.
  * Extreme gezichtsuitdrukkingen.

## <a name="next-steps"></a>Volgende stappen

Nu dat u bekend met face herkenning van concepten bent, Leer hoe u een script schrijven dat gezichten op basis van een getraind PersonGroup identificeert.

* [Gezichten in foto's identificeren](../Face-API-How-to-Topics/HowtoIdentifyFacesinImage.md)