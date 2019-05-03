---
title: Inktgegevens verzenden naar de API Ink-herkenning
titlesuffix: Azure Cognitive Services
description: Meer informatie over het aanroepen van de Ink Analyzer-API voor verschillende toepassingen
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 5a212c7332d085c15baabef8650572162c47903d
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027094"
---
# <a name="send-ink-data-to-the-ink-recognizer-api"></a>Inktgegevens verzenden naar de API Ink-herkenning 

Digitale inkt verwijst naar de technologieën waarmee u digitale representaties van invoer zoals handschriftherkenning en tekeningen. Dit is doorgaans bereikt met behulp van een digitizer waarmee de verplaatsing van apparaten, zoals een stylus wordt vastgelegd. Apparaten steeds meer gaan om in te schakelen rijke digitale bij ervaringen, kunt kunstmatige intelligentie en machine learning u de opname van schriftelijke vormen en tekst in een context. De Ink-herkenning-API kunt u voor het verzenden van ink-lijnen en gedetailleerde informatie over deze. 

## <a name="the-ink-recognizer-api-vs-ocr-services"></a>De Ink herkenning API vs. OCR-services

De Ink-herkenning-API maakt geen gebruik van optische tekens Recognition(OCR). OCR-services verwerken de pixelgegevens van installatiekopieën voor herkenning van handgeschreven tekst en tekst. Dit wordt soms offline erkenning genoemd. In plaats daarvan vereist de Ink herkenning API dat gegevens van de digitale ink-lijn die opgenomen, zoals de apparaat-invoer wordt gebruikt. Verwerken van digitale Inktgegevens op deze manier kan meer nauwkeurige herkenningsresultaten ten opzichte van OCR-services produceren. 

## <a name="sending-ink-data"></a>Inktgegevens te verzenden

De Ink herkenning API vereist de X- en Y-coördinaten die staan voor de ink-lijnen die zijn gemaakt door een invoer-apparaat vanaf het moment dat deze maakt deel uit van de detectie voor aanvallen op wanneer deze ingetrokken. De punten van elke streek moeten een tekenreeks met door komma's gescheiden waarden, en worden opgemaakt in JSON als in het onderstaande voorbeeld. Elke ink-lijn moet bovendien een unieke ID hebben in elke aanvraag. Als de ID wordt herhaald in dezelfde aanvraag, wordt de API een fout geretourneerd. Voor de resultaten van de meest nauwkeurige herkenning, hebt u ten minste acht cijfers na het decimaalteken. De oorsprong (0,0) van het canvas wordt ervan uitgegaan dat de linkerbovenhoek van het bij canvas.

> [!NOTE]
> Het volgende voorbeeld is geen geldige JSON. U kunt een volledige Ink herkenning JSON-aanvraag vinden op [GitHub](https://go.microsoft.com/fwlink/?linkid=2089909).
 
```json
{
  "language": "en-US",
  "strokes": [
   {
    "id": 43,
    "points": 
        "5.1365, 12.3845,
        4.9534, 12.1301,
        4.8618, 12.1199,
        4.7906, 12.2217,
        4.7906, 12.5372,
        4.8211, 12.9849,
        4.9534, 13.6667,
        5.0958, 14.4503,
        5.3299, 15.2441,
        5.6555, 16.0480,
        ..."
   },
    ...
  ]
}
```

## <a name="ink-recognizer-response"></a>Inkt herkenning antwoord

De Ink herkenning-API retourneert een antwoord analyses over de objecten die deze herkend uit de ink-inhoud. Het antwoord bevat de opname-eenheden die de relaties tussen verschillende ink lijnen beschrijven. Bijvoorbeeld, worden lijnen die uniek, afzonderlijke vormen maken in verschillende eenheden opgenomen. Elke eenheid bevat gedetailleerde informatie over de ink-lijnen met inbegrip van de herkende object, de coördinaten en andere kenmerken van de tekening.

## <a name="shapes-recognized-by-the-ink-recognizer-api"></a>Vormen herkend door de Ink-herkenning-API

De Ink-herkenning-API kan de meest gebruikte vormen in notities kunt identificeren. De onderstaande afbeelding ziet u enkele eenvoudige voorbeelden. Zie voor een volledige lijst van vormen en andere inhoud ink wordt herkend door de API, de [API-verwijzing artikel](https://go.microsoft.com/fwlink/?linkid=2089907). 

![De lijst met vormen herkend door de Ink-herkenning-API](../media/shapes.png)

## <a name="recommended-calling-patterns"></a>Aanbevolen aanroepen van patronen

U kunt de Ink herkenning REST-API aanroepen in verschillende patronen op basis van uw toepassing. 

### <a name="user-initiated-api-calls"></a>Door de gebruiker geïnitieerde API-aanroepen

Als u een app die invoer van gebruiker (bijvoorbeeld een opmerking nemen of een aantekening app) bouwt nu, kunt u ze van controle biedt wanneer en welke ink wordt verzonden naar de API Ink-herkenning. Deze functionaliteit is vooral nuttig wanneer de tekst en vormen beide aanwezig zijn op het canvas zijn en de verschillende acties uitvoeren voor elke gebruikers willen. Overweeg de selectiefuncties (zoals een lasso of andere geometrische Selectiehulpmiddel) waarmee gebruikers kunnen kiezen welke gegevens worden verzonden naar de API.  

### <a name="app-initiated-api-calls"></a>App gestart API-aanroepen

U kunt ook uw app Roep de API voor de Ink-herkenning hebben na een time-out. Door de huidige ink-lijnen regelmatig naar de API verzendt, kunt u herkenningsresultaten opslaan als ze worden gemaakt bij het verbeteren van de reactietijd van de API. U kunt bijvoorbeeld verzenden een line-of-handgeschreven tekst naar de API na het detecteren van de gebruiker deze is voltooid. 

Resultaten van de herkenning van tevoren met vindt u informatie over de kenmerken van ink-lijnen en hun relatie met elkaar. Welke lijnen worden bijvoorbeeld gegroepeerd om te vormen de hetzelfde woord, regel, lijst, punt of vorm. Deze informatie kunt van uw app ink selectiefuncties verbeteren door de mogelijkheid om beveiligingsgroepen te selecteren van lijnen in één keer, bijvoorbeeld.

## <a name="integrate-the-ink-recognizer-api-with-windows-ink"></a>De Ink-herkenning API integreren met Windows Ink

[Windows Ink](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions) biedt hulpprogramma's en technologieën voor efficiëntere digitale bij ervaringen op uiteenlopende tal van apparaten. U kunt het platform Windows Ink combineren met de Ink-API te maken van toepassingen die worden weergegeven en digitale inkt lijnen worden geïnterpreteerd.

## <a name="next-steps"></a>Volgende stappen

* [Wat is de API Ink-herkenning?](../overview.md)
* [Ink-herkenning REST API-naslaginformatie](https://go.microsoft.com/fwlink/?linkid=2089907)

* Beginnen met het verzenden van digitale inkt lijn gegevens met behulp van:
    * [C#](../quickstarts/csharp.md)
    * [Java](../quickstarts/java.md)
    * [JavaScript](../quickstarts/javascript.md)