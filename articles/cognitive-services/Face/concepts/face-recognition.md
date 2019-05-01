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
ms.openlocfilehash: 8c0bf001c2bdbedaa041dbd10b5c7edb08eec4c6
ms.sourcegitcommit: 524625dd12e0537173616a991802075e2dc9da12
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2019
ms.locfileid: "64415997"
---
# <a name="face-recognition-concepts"></a>Face-opname-concepten

In dit artikel worden de concepten van de verschillende bewerkingen face-opname (verificatie, net als, groeperen, identificatie zoeken) en de onderliggende gegevensstructuren. Algemeen, erkenning beschrijving van het werk van het vergelijken van twee verschillende vlakken om te bepalen of ze vergelijkbaar zijn of deel uitmaken van dezelfde persoon.

## <a name="recognition-related-data-structures"></a>Gegevens met betrekking tot de opname-structuren

De opname-bewerkingen gebruik voornamelijk de volgende gegevensstructuren. Deze objecten worden opgeslagen in de cloud en kunnen worden verwezen door de ID-tekenreeksen. Id-tekenreeksen worden daarom altijd uniek zijn binnen een abonnement, terwijl de van de naamvelden kunnen worden gedupliceerd.

|Name|Description|
|:--|:--|
|**DetectedFace**| Dit is een eenmalige face weergave opgehaald door de [gezichtsdetectie](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md) bewerking. De ID verloopt 24 uur nadat deze is gemaakt.|
|**PersistedFace**| Wanneer **DetectedFace** objecten worden toegevoegd aan een groep (zoals **FaceList** of **persoon**), ze komen **PersistedFace** -objecten die kunnen worden [opgehaald](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c) op een tijd en niet verlopen.|
|**[FaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b)**/**[LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc)**| Dit is een verschillende lijst **PersistedFace** objecten. Een **FaceList** heeft een unieke ID, een naam en eventueel een tekenreeks van de gegevens.|
|**[Person](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)**| Dit is een lijst met **PersistedFace** objecten die deel uitmaken van dezelfde persoon. Er is een unieke ID, een naam en eventueel een tekenreeks van de gegevens.|
|**[PersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)**/**[LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)**| Dit is een verschillende lijst **persoon** objecten. Er is een unieke ID, een naam en eventueel een tekenreeks van de gegevens. Een **PersonGroup** moet [getrainde](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) voordat deze kan worden gebruikt in bewerkingen voor herkenning.|

## <a name="recognition-operations"></a>Herkenning van bewerkingen

In deze sectie wordt uitgelegd hoe de vier bewerkingen voor herkenning van de bovenstaande gegevensstructuren gebruiken. Zie de [overzicht](../Overview.md) voor een uitgebreide beschrijving van elke bewerking herkenning.

### <a name="verification"></a>Verificatie

De [controleren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) duurt een gezichts-ID (**DetectedFace** of **PersistedFace**) en een van beide een ander gezichts-ID of een **persoon** object en Hiermee bepaalt u of ze deel uitmaken van dezelfde persoon. Als u doorgeeft een **persoon**, kunt u eventueel doorgeven een **PersonGroup** waartoe **persoon** behoort om de prestaties verbeteren.

### <a name="find-similar"></a>Zoek vergelijkbare

De [vinden vergelijkbaar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) duurt een gezichts-ID (**DetectedFace** of **PersistedFace**) en ofwel een **FaceList** of een matrix van andere gezicht Id's. Met een **FaceList**, wordt een kleinere **FaceList** van gezichten wordt uitgevoerd die vergelijkbaar met de bepaald gezicht zijn. Met een matrix met face id's retourneert deze op dezelfde manier een kleinere matrix.

### <a name="grouping"></a>Groeperen

De [groep](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) bewerking wordt een matrix met verschillende face id's (**DetectedFace** of **PersistedFace**) en retourneert de dezelfde id's die zijn ondergebracht in verschillende kleinere matrices. Elke 'groepen' matrix bevat face id's die er ongeveer als en een matrix van één "messyGroup" face id's voor dat er geen overeenkomsten zijn gevonden.

### <a name="identification"></a>Identificatie

De [identificeren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) duurt een of meer face id's (**DetectedFace** of **PersistedFace**) en een **PersonGroup** en retourneert een lijst van **persoon** objecten die elk gezicht deel kan uitmaken. Geretourneerd **persoon** objecten zijn verpakt als **Candidate** objecten, die een voorspelling vertrouwen waarde hebben.

## <a name="input-data"></a>Invoergegevens

Gebruik de volgende tips om ervoor te zorgen dat uw invoer afbeeldingen de meest nauwkeurige resultaten geven:

* De afbeelding ondersteunde indelingen zijn JPEG, PNG-, GIF-bestand (het eerste frame), BMP.
* De grootte van de installatiekopie-bestand mag niet groter zijn dan 4 MB zijn.
* Bij het maken van **persoon** objecten, moet u foto's die zijn uitgerust met verschillende hoeken en belichting.
* Sommige gezichten worden niet herkend door technische uitdagingen:
  * Installatiekopieën met extreme belichting (bijvoorbeeld ernstige voor).
  * Obstakels blokkeren van een of beide onder ogen krijgt.
  * De verschillen in haar stijl of videodetectie haar.
  * Wijzigingen in de weergave face vanwege leeftijd.
  * Extreme gezichtsuitdrukkingen.

## <a name="next-steps"></a>Volgende stappen

Nu dat u bekend met face herkenning van concepten bent, informatie over het schrijven van een eenvoudig script waarmee gezichten op basis van een getraind **PersonGroup**.

* [Gezichten in foto's identificeren](../Face-API-How-to-Topics/HowtoIdentifyFacesinImage.md)