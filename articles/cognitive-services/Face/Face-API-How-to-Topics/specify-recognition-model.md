---
title: Het opgeven van een model opname - Face-API
titleSuffix: Azure Cognitive Services
description: Dit artikel wordt beschreven hoe u kunt kiezen welk model u erkenning voor gebruik met uw Azure-Face-API-toepassing.
services: cognitive-services
author: longl
manager: nitinme
ms.service: cognitive-services
ms.component: face-api
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: longl
ms.openlocfilehash: 33348e637143b923719425b9674f99a475d848d9
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58806658"
---
# <a name="specify-a-face-recognition-model"></a>Geef een gezicht opname-model

Deze handleiding wordt beschreven hoe u een model face erkenning voor gezichtsherkenning, identificatie en gelijkenis zoeken met behulp van de Face-API van Azure.

De Face-API maakt gebruik van machine learning-modellen voor het uitvoeren van bewerkingen op menselijke gezichten in afbeeldingen. We blijven voor het verbeteren van de nauwkeurigheid van onze modellen op basis van feedback van klanten en ontwikkelingen in het onderzoek en we leveren deze verbeteringen als gegevensmodellen kunnen bijwerken. Ontwikkelaars hebben altijd de optie om op te geven welke versie van het model van de spraakherkenning face ze graag willen gebruiken. ze kunnen het beste past bij hun gebruiksscenario model kiezen.

Als u een nieuwe gebruiker bent, raden wij dat u de nieuwste model gebruiken. Lees verder voor meer informatie over deze opgeven in verschillende bewerkingen van de Face vermeden modelconflicten. Als u een ervaren gebruiker bent en u niet zeker weet of u moet overschakelen naar de nieuwste model, gaat u naar de [evalueren van de verschillende modellen](#evaluate-different-models) sectie om te evalueren van het nieuwe model en vergelijk de resultaten met behulp van de huidige gegevensset.

## <a name="prerequisites"></a>Vereisten

U moet bekend zijn met de concepten van AI-gezichtsdetectie en identificatie. Als u niet eerst deze handleidingen zien:

* [Hoe u gezichtsherkenning in een afbeelding](HowtoDetectFacesinImage.md)
* [Gezichten in een afbeelding identificeren](HowtoIdentifyFacesinImage.md)

## <a name="detect-faces-with-specified-model"></a>Detecteer gezichten met opgegeven model

Gezichtsdetectie identificeert de visual oriëntatiepunten van menselijke gezichten en hun locaties omsluitende vindt. Ook extraheert het gezicht van functies en slaat ze op voor gebruik in de identificatie. Al deze informatie vormt de weergave van een gezicht.

De opname-model wordt gebruikt wanneer de face-functies zijn uitgepakt, zodat u een versie van het model opgeven kunt bij het uitvoeren van de analyse-bewerking.

Wanneer u de [Face - detecteren] API, toewijzen van de versie van het model met de `recognitionModel` parameter. De beschikbare waarden zijn:

* `recognition_01`
* `recognition_02`

Desgewenst kunt u de _returnRecognitionModel_ parameter (standaard **false**) om aan te geven of _recognitionModel_ moet worden geretourneerd in antwoord. Dus een aanvraag-URL voor de [Face - detecteren] REST-API wordt er als volgt:

`https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes][&recognitionModel][&returnRecognitionModel]
&subscription-key=<Subscription key>`

Als u van de clientbibliotheek gebruikmaakt, kunt u de waarde voor toewijzen `recognitionModel` door door te geven van een tekenreeks voor de versie.
Als u laat u het niet-toegewezen, de versie van het standaard (_recognition_01_) wordt gebruikt. Zie het volgende codevoorbeeld voor de .NET-clientbibliotheek.

```csharp
string imageUrl = "http://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceServiceClient.Face.DetectWithUrlAsync(imageUrl, true, true, recognitionModel: "recognition_02", returnRecognitionModel: true);
```

## <a name="identify-faces-with-specified-model"></a>Identificeer gezichten met opgegeven model

De Face-API kunt face gegevens ophalen uit een installatiekopie en koppel deze aan een **persoon** object (via de [toevoegen face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API-aanroepen, bijvoorbeeld), en meerdere **persoon** objecten kunnen worden opgeslagen samen in een **PersonGroup**. Vervolgens een nieuw gezicht kan worden vergeleken op basis van een **PersonGroup** (met de [Face - Identify] aanroepen), en de overeenkomende persoon binnen die groep kan worden geïdentificeerd.

Een **PersonGroup** moet beschikken over één unieke erkenning model voor alle van de **persoon**s, en u kunt dit opgeven met behulp van de `recognitionModel` parameter bij het maken van de groep ([PersonGroup - Create] of [LargePersonGroup - Create]). Als u niet dat deze parameter, de oorspronkelijke opgeven `recognition_01` model wordt gebruikt. Een groep gebruikt altijd de opname-model die is gemaakt, en nieuwe gezichten worden gekoppeld aan dit model wanneer ze worden toegevoegd. Dit kan niet worden gewijzigd na het maken van een groep. Om te zien welke model een **PersonGroup** is geconfigureerd, gebruikt u de [PersonGroup - Get] API met de _returnRecognitionModel_ parameter ingesteld als **waar**.

Zie het volgende codevoorbeeld voor de .NET-clientbibliotheek.

```csharp
// Create an empty PersonGroup with "recognition_02" model
string personGroupId = "mypersongroupid";
await faceServiceClient.PersonGroup.CreateAsync(personGroupId, "My Person Group Name", recognitionModel: "recognition_02");
```

In deze code een **PersonGroup** met ID `mypersongroupid` is gemaakt, en deze is ingesteld voor het gebruik de _recognition_02_ model om op te halen van de face-functies.

Dienovereenkomstig, moet u opgeven welk model moet worden gebruikt bij het detecteren van gezichten te vergelijken met dit **PersonGroup** (via de [Face - detecteren] API). Het model dat u moet altijd consistent met de **PersonGroup**van configuratie; anders mislukt de bewerking vanwege niet-compatibele modellen.

Er is geen wijziging in de [Face - Identify] API; u hoeft alleen te geven van de versie van het model wordt gedetecteerd.

## <a name="find-similar-faces-with-specified-model"></a>Soortgelijke gezichten met opgegeven model zoeken

U kunt ook een model erkenning van gelijkenis zoeken opgeven. U kunt de versie van het model met `recognitionModel` bij het maken van de face-lijst met [FaceList - maken] API of [LargeFaceList - maken]. Als u niet dat deze parameter, de oorspronkelijke opgeven `recognition_01` model wordt gebruikt. Een lijst met face gebruikt altijd de opname-model die is gemaakt, en nieuwe gezichten worden gekoppeld aan dit model wanneer ze worden toegevoegd. Dit kan niet worden gewijzigd nadat de is gemaakt. Als u wilt zien welke een lijst met face is geconfigureerd met model, gebruikt u de [FaceList - Get] API met de _returnRecognitionModel_ parameter ingesteld als **waar**.

Zie het volgende codevoorbeeld voor de .NET-clientbibliotheek.

```csharp
await faceServiceClient.FaceList.CreateAsync(faceListId, "My face collection", recognitionModel: "recognition_02");
```

Deze code maakt een lijst met de face-naam `My face collection`, met de _recognition_02_ model voor het ophalen van de functie. Wanneer u deze lijst face voor soortgelijke gezichten om een nieuwe gedetecteerde gezicht te zoeken, die face moet zijn gedetecteerd ([Face - detecteren]) met behulp van de _recognition_02_ model. Zoals in de vorige sectie moet het model consistent.

Er is geen wijziging in de [Face - Zoek vergelijkbare] API; u alleen de versie van het model in detectie opgeven.

## <a name="verify-faces-with-specified-model"></a>Verifieer gezichten met opgegeven model

De [Face - Verify] API controleert of twee gezichten bij dezelfde persoon horen. Er is geen wijziging in de API controleren met betrekking tot de modellen voor spraakherkenning, maar u kunt alleen gezichten die zijn gedetecteerd met hetzelfde model vergelijken. Ja, de twee gezichten beide moet zijn gedetecteerd met behulp van `recognition_01` of `recognition_02`.

## <a name="evaluate-different-models"></a>Evalueren van de verschillende modellen

Als u wilt vergelijken van de prestaties van de _recognition_01_ en _recognition_02_ modellen voor uw gegevens, moet u naar:

1. Twee **PersonGroup**(s) met _recognition_01_ en _recognition_02_ respectievelijk.
1. De installatiekopiegegevens van uw gezichten detecteren en registreren te gebruiken **persoon**s voor deze twee **PersonGroup**s, en het activeren van de training met verwerken [PersonGroup - Train] API.
1. Testen met [Face - Identify] op zowel **PersonGroup**s en vergelijk de resultaten.

Als u normaal gesproken een drempelwaarde voor betrouwbaarheid (een waarde tussen 0 en één die hoe zeker het model moet worden omgezet bepaalt naar een gezicht identificeren) opgeeft, moet u mogelijk gebruik van verschillende drempelwaarden voor verschillende modellen. Een drempel voor één model is niet bedoeld om te worden gedeeld naar een andere en niet per se de resultaten hetzelfde resultaat.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u de opname-model te gebruiken met andere service-Face-API's. Vervolgens gaat u als volgt een snelstartgids om aan de slag met behulp van gezichtsherkenning.

* [Detecteer gezichten in een afbeelding](../quickstarts/csharp-detect-sdk.md)

[Face - detecteren]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d
[Face - Zoek vergelijkbare]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237
[Face - Identify]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239
[Face - Verify]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a
[PersonGroup - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244
[PersonGroup - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246
[PersonGroup Person - Add Face]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b
[PersonGroup - Train]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249
[LargePersonGroup - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d
[FaceList - maken]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b
[FaceList - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c
[LargeFaceList - maken]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc
