---
title: Computer Vision, afgedrukt/handgeschreven tekst herkennen
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot het herkennen van afdrukken en handgeschreven tekst in afbeeldingen met behulp van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: bcaa990cc2186a5f1eecdbbca91804c92370277c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66357183"
---
# <a name="recognize-printed-and-handwritten-text"></a>Afgedrukte en handgeschreven tekst herkennen

Computer Vision biedt een aantal services die Detecteer en extraheer gedrukte of handgeschreven tekst die wordt weergegeven in afbeeldingen. Dit is handig in tal van scenario's zoals notities, medische dossiers, beveiliging en bankieren. De volgende drie secties gedetailleerd drie verschillende tekstherkenning API's, elk geoptimaliseerd voor verschillende gebruiksvoorbeelden.

## <a name="read-api"></a>Lezen-API

De API lezen detecteert tekstinhoud in een afbeelding met behulp van onze nieuwste modellen voor spraakherkenning en wordt de herkende tekst geconverteerd naar een machinaal leesbare tekenstroom. Dit geoptimaliseerd voor tekst maken veel afbeeldingen (zoals documenten die digitaal zijn gescand) en voor installatiekopieën met een groot aantal visual ruis. Het zal bepalen welk model u erkenning wilt gebruiken voor elke regel tekst, afbeeldingen met afdrukken en handgeschreven tekst ondersteunen. De lezen-API wordt asynchroon uitgevoerd omdat grotere documenten enige tijd duren kunnen om een resultaat te retourneren.

De leesbewerking onderhoudt de oorspronkelijke regel groeperingen van herkende woorden in de uitvoer ervan weergegeven. Elke regel wordt geleverd met het selectiekader vak coördinaten en elk woord in de regel is ook een eigen coördinaten. Als een woord is herkend met weinig vertrouwen, wordt deze informatie ook overgebracht. Zie de [lezen API-referentiedocumenten](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) voor meer informatie.

> [!NOTE]
> Deze functie is alleen beschikbaar voor Engelse tekst.

### <a name="image-requirements"></a>Vereisten voor installatiekopieën

De API alleen werkt met de installatiekopieën die voldoen aan de volgende vereisten:

- De afbeelding moet worden gepresenteerd in JPEG, PNG-, BMP, PDF-bestand of TIFF-indeling.
- De afmetingen van de afbeelding moet tussen 50 x 50 en 4200 x 4200 pixels. PDF-pagina's moet 17 x 17 inches of kleiner.
- De grootte van de afbeelding moet minder dan 20 MB (Megabytes).

### <a name="limitations"></a>Beperkingen

Als u van een abonnement gratis-laag gebruikmaakt, verwerkt de lezen-API alleen de eerste twee pagina's van een PDF-bestand of een TIFF-document. Met een betaald abonnement worden maximaal 200 pagina's verwerkt. Houd er ook rekening mee dat de API een maximum van 300 regels per pagina detecteert.

## <a name="ocr-optical-character-recognition-api"></a>OCR (optische tekenherkenning) API

Computer-Vision-API voor optische tekenherkenning (OCR) is vergelijkbaar met de API lezen, maar deze wordt synchroon uitgevoerd en niet is geoptimaliseerd voor grote documenten. Het maakt gebruik van een eerdere opname-model, maar werkt met meer talen. Zie [taalondersteuning](language-support.md#text-recognition) voor een volledige lijst van de ondersteunde talen.

Indien nodig, corrigeert OCR de rotatie van de herkende tekst door de verschuiving van de rotatiesnelheid in graden over de afbeelding van de horizontale as worden geretourneerd. OCR biedt ook de frame-coördinaten van elk woord, zoals te zien is in de volgende afbeelding.

![Een afbeelding wordt gedraaid en de tekst wordt gelezen en afgebakend](./Images/vision-overview-ocr.png)

Zie de [OCR-referentiedocumenten](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) voor meer informatie.

### <a name="image-requirements"></a>Vereisten voor installatiekopieën

De API OCR werkt op de installatiekopieën die voldoen aan de volgende vereisten:

* De afbeelding moet worden gepresenteerd in JPEG, PNG-, GIF of BMP-indeling.
* De grootte van de afbeelding moet tussen 50 x 50 en 4200 x 4200 pixels.
* De tekst in de afbeelding kan worden gedraaid in meerdere van 90 graden plus een kleine hoek van maximaal 40 graden.

### <a name="limitations"></a>Beperkingen

Fout-positieven kunnen op foto's waarin tekst dominante wordt afkomstig zijn van gedeeltelijk herkende woorden. Precisie kan op sommige foto's, met name foto's zonder tekst, variëren afhankelijk van het type afbeelding.

## <a name="recognize-text-api"></a>Herkennen tekst-API

> [!NOTE]
> Het herkennen van Text-API is afgeschaft en vervangen door de lezen-API. De API lezen vergelijkbare mogelijkheden heeft en is bijgewerkt voor het afhandelen van PDF-, TIFF- en -bestanden met meerdere pagina's.

De herkennen tekst-API is vergelijkbaar met OCR, maar het asynchroon wordt uitgevoerd en maakt gebruik van modellen voor spraakherkenning bijgewerkt. Zie de [herkennen tekst-API-referentiedocumenten](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) voor meer informatie.

### <a name="image-requirements"></a>Vereisten voor installatiekopieën

De Text-API herkent werkt met de installatiekopieën die voldoen aan de volgende vereisten:

- De afbeelding moet worden weergegeven in JPEG, PNG of BMP-indeling.
- De afmetingen van de afbeelding moet tussen 50 x 50 en 4200 x 4200 pixels.
- De grootte van de afbeelding moet minder dan 4 megabyte (MB).

## <a name="limitations"></a>Beperkingen

De nauwkeurigheid van de tekst herkenning van bewerkingen, is afhankelijk van de kwaliteit van de installatiekopieën. De volgende factoren kunnen ertoe leiden dat een onjuist lezen:

* Wazige afbeeldingen.
* Handgeschreven of cursieve tekst.
* Artistieke lettertypestijlen.
* Kleine lettergrootte.
* Complexe achtergronden, schaduwen of schittering op de tekst of vervorming van het perspectief.
* Grote of ontbreekt hoofdletters aan het begin van woorden.
* Subscript, superscript of doorgehaalde tekst.

## <a name="next-steps"></a>Volgende stappen

Ga als volgt de [gedrukte tekst (OCR) extraheren](./quickstarts/csharp-print-text.md) snelstartgids tekstherkenning implementeren in een eenvoudige C# app.
