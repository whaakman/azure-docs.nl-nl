---
title: Het opgeven van een model - Face-API
titleSuffix: Azure Cognitive Services
description: Dit artikel wordt beschreven hoe u kunt kiezen welk model face detection gebruiken met uw Azure-Face-API-toepassing.
services: cognitive-services
author: yluiu
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: yluiu
ms.openlocfilehash: 26ab3cb247309aa21791ca5a984f39ef40ce9a78
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249620"
---
# <a name="specify-a-face-detection-model"></a>Een model voor gezichtsdetectie opgeven

Deze handleiding laat zien hoe u een model voor face opgeven voor de Face-API van Azure.

De Face-API maakt gebruik van machine learning-modellen voor het uitvoeren van bewerkingen op menselijke gezichten in afbeeldingen. We blijven voor het verbeteren van de nauwkeurigheid van onze modellen op basis van feedback van klanten en ontwikkelingen in het onderzoek en we leveren deze verbeteringen als gegevensmodellen kunnen bijwerken. Ontwikkelaars hebben altijd de optie om op te geven welke versie van het model voor face ze graag willen gebruiken. ze kunnen het beste past bij hun gebruiksscenario model kiezen.

Lees verder voor meer informatie over het model voor het gezicht in bepaalde bewerkingen face opgeven. De Face-API maakt gebruik van gezichtsherkenning wanneer er een installatiekopie van een gezicht naar een andere vorm van gegevens converteert.

Als u niet zeker weet of u de nieuwste model moet gebruiken, gaat u naar de [evalueren van de verschillende modellen](#evaluate-different-models) sectie om te evalueren van het nieuwe model en vergelijk de resultaten met behulp van de huidige gegevensset.

## <a name="prerequisites"></a>Vereisten

U moet bekend zijn met het concept van AI-gezichtsdetectie. Als u niet, ziet u de face detection conceptuele handleiding of gebruiksaanwijzing:

* [Face detection-concepten](../concepts/face-detection.md)
* [Hoe u gezichtsherkenning in een afbeelding](HowtoDetectFacesinImage.md)

## <a name="detect-faces-with-specified-model"></a>Detecteer gezichten met opgegeven model

Gezichtsdetectie zoekt naar de locatie van het omsluitende van menselijke gezichten en hun visuele oriëntatiepunten identificeert. Deze van de face-functies worden uitgepakt en slaat ze op voor later gebruik in [erkenning](../concepts/face-recognition.md) bewerkingen.

Wanneer u gebruikt de [Face - detecteren] API, kunt u de versie van het model met de `detectionModel` parameter. De beschikbare waarden zijn:

* `detection_01`
* `detection_02`

Een aanvraag-URL voor de [Face - detecteren] REST-API wordt er als volgt:

`https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes][&recognitionModel][&returnRecognitionModel][&detectionModel]&subscription-key=<Subscription key>`

Als u van de clientbibliotheek gebruikmaakt, kunt u de waarde voor toewijzen `detectionModel` door te geven in de juiste tekenreeks. Als u dit niet-toegewezen laten staan, de API de standaardversie van het model wordt gebruikt (`detection_01`). Zie het volgende codevoorbeeld voor de .NET-clientbibliotheek.

```csharp
string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, false, false, recognitionModel: "recognition_02", detectionModel: "detection_02");
```

## <a name="add-face-to-person-with-specified-model"></a>Face persoon met opgegeven model toevoegen

De Face-API kunt face gegevens ophalen uit een installatiekopie en koppel deze aan een **persoon** object via de [PersonGroup persoon - Face toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API. In deze API-aanroep kunt u de detectie-model op dezelfde manier als in [Face - detecteren].

Zie het volgende codevoorbeeld voor de .NET-clientbibliotheek.

```csharp
// Create a PersonGroup and add a person with face detected by "detection_02" model
string personGroupId = "mypersongroupid";
await faceClient.PersonGroup.CreateAsync(personGroupId, "My Person Group Name", recognitionModel: "recognition_02");

string personId = (await faceClient.PersonGroupPerson.CreateAsync(personGroupId, "My Person Name")).PersonId;

string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
await client.PersonGroupPerson.AddFaceFromUrlAsync(personGroupId, personId, imageUrl, detectionModel: "detection_02");
```

Deze code maakt een **PersonGroup** met ID `mypersongroupid` en voegt een **persoon** toe. En vervolgens een gezicht wordt toegevoegd aan deze **persoon** met behulp van de `detection_02` model. Als u geen opgeeft de *detectionModel* parameter, de API gebruikt het standaardmodel `detection_01`.

> [!NOTE]
> U hoeft te gebruiken van het model voor dezelfde voor alle gezichten in een **persoon** object, en u hoeft niet hetzelfde detectie model gebruiken bij het detecteren van nieuwe gezichten om te vergelijken met een **persoon** object (in de [Face - Identify] API, bijvoorbeeld).

## <a name="add-face-to-facelist-with-specified-model"></a>Face aan FaceList met opgegeven model toevoegen

U kunt ook een model opgeven wanneer u een gezicht aan een bestaande toevoegen **FaceList** object. Zie het volgende codevoorbeeld voor de .NET-clientbibliotheek.

```csharp
await faceClient.FaceList.CreateAsync(faceListId, "My face collection", recognitionModel: "recognition_02");

string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
await client.FaceList.AddFaceFromUrlAsync(faceListId, imageUrl, detectionModel: "detection_02");
```

Deze code maakt een **FaceList** met de naam `My face collection` en voegt u een gezicht aan met de `detection_02` model. Als u geen opgeeft de *detectionModel* parameter, de API gebruikt het standaardmodel `detection_01`.

> [!NOTE]
> U hoeft te gebruiken van het model voor dezelfde voor alle gezichten in een **FaceList** object, en u hoeft niet hetzelfde detectie model gebruiken bij het detecteren van nieuwe gezichten om te vergelijken met een **FaceList** object.

## <a name="evaluate-different-models"></a>Evalueren van de verschillende modellen

De verschillende face detection-modellen zijn geoptimaliseerd voor verschillende taken. Zie de volgende tabel voor een overzicht van de verschillen.

|**detection_01**  |**detection_02**  |
|---------|---------|
|Standaardoptie voor alle face detection-bewerkingen. | Uitgebracht in mei 2019 en beschikbaar is (optioneel) in alle face detection-bewerkingen.
|Niet geoptimaliseerd voor kleine, side-weergave of fuzzy gezichten.  | Verbeterde nauwkeurigheid op kleine, side-weergave en fuzzy gezichten. |
|Met deze eigenschap wordt geconfronteerd met kenmerken (hoofd houding, leeftijd, emotie, enzovoort) als ze zijn opgegeven in de aanroep van de analyse. |  Retourneert geen face kenmerken.     |
|Met deze eigenschap wordt geconfronteerd met oriëntatiepunten als ze zijn opgegeven in de aanroep van de analyse.   | Retourneert geen gezichtsoriëntatiepunten.  |

De beste manier om te vergelijken de prestaties van de `detection_01` en `detection_02` modellen is om ze te gebruiken op een voorbeeldgegevensset. Aanroep wordt aangeraden de [Face - detecteren] API op een groot aantal installatiekopieën, met name afbeeldingen van de vele gezichten of gezichten die moeilijk te zien, met behulp van elk model detectie. Let op het aantal gezichten die elk model retourneert.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u de detectie-model te gebruiken met andere Face-API's. Vervolgens gaat u als volgt een snelstartgids om aan de slag met behulp van gezichtsherkenning.

* [Detecteer gezichten in een afbeelding (.NET SDK)](../quickstarts/csharp-detect-sdk.md)

[Face - detecteren]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d
[Face - Find Similar]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237
[Face - Identify]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239
[Face - Verify]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a
[PersonGroup - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244
[PersonGroup - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246
[PersonGroup Person - Add Face]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b
[PersonGroup - Train]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249
[LargePersonGroup - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d
[FaceList - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b
[FaceList - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c
[FaceList - Add Face]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250
[LargeFaceList - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc
