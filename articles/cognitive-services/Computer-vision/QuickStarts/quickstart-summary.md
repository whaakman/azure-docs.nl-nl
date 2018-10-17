---
title: Samenvatting van snelstarts voor Computer Vision
titleSuffix: Azure Cognitive Services
description: In deze snelstarts analyseert u een afbeelding, maakt u een miniatuur en extraheert u gedrukte en handgeschreven tekst met behulp van de Computer Vision-API.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 89502fe49f5eb4b9692d7ee4233c7c46d645b3fb
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576881"
---
# <a name="quickstart-summary"></a>Snelstart: samenvatting

Deze snelstarts bevatten informatie en voorbeelden van code om u te helpen snel aan de slag te gaan met de Computer Vision-service.

In de voorbeelden worden directe HTTP-aanroepen naar de Computer Vision-API gedaan. Zie de sectie *Snelstart voor SDK* voor voorbeelden met behulp van de Computer Vision-clientbibliotheken, die handige methoden bieden waarmee de HTTP-aanroepen worden verpakt.

Als u snel wilt experimenteren met de Computer Vision-API's, probeert u de [Open API-testconsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

U kunt de Computer Vision-service gebruiken voor het uitvoeren van de volgende taken:

* Een externe afbeelding analyseren
* Een lokale afbeelding analyseren
* Beroemdheden en oriëntatiepunten (domeinmodellen) detecteren
* Intelligent een miniatuur genereren
* Gedrukte tekst (OCR) detecteren en extraheren uit een afbeelding
* Handgeschreven tekst detecteren en extraheren uit een afbeelding

De code in elk voorbeeld is vergelijkbaar. Ze geven echter verschillende Computer Vision-functies aan, samen met verschillende technieken voor het uitwisselen van gegevens met de service, zoals:

* Met _Een miniatuur genereren_ wordt een afbeelding als een bytematrix in de hoofdtekst van het antwoord geretourneerd.
* _Een lokale afbeelding analyseren_ vereist dat de afbeelding als een bytematrix in de aanvraag moet worden opgenomen.
* _Handgeschreven tekst extraheren_ vereist twee aanroepen om de tekst op te halen.

## <a name="summary"></a>Samenvatting

| Snelstartgids               | Aanvraagparameters                          | Antwoord          |
| ------------------------ | ------------------------------------------- | ----------------  |
| Een externe afbeelding analyseren   | visualFeatures=Categories,Description,Color | JSON-tekenreeks       |
| Een lokale afbeelding analyseren    | data=image_data (bytematrix)                | JSON-tekenreeks       |
| Beroemdheden detecteren       | model=celebrities                           | JSON-tekenreeks       |
| Een miniatuur genereren     | width=200&height=150&smartCropping=true     | bytematrix        |
| Afgedrukte tekst extraheren     | language=unk&detectOrientation=true         | JSON-tekenreeks       |
| Handgeschreven tekst extraheren | handwriting=true                            | URL, JSON-tekenreeks  |

## <a name="next-steps"></a>Volgende stappen

Bekijk de Computer Vision-API's die worden gebruikt om een afbeelding te analyseren, beroemdheden en oriëntatiepunten te detecteren, een miniatuur te maken en gedrukte en handgeschreven tekst te verkrijgen.

> [!div class="nextstepaction"]
> [De Computer Vision-API's bekijken](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
