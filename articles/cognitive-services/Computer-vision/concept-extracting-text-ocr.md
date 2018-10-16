---
title: Tekst met OCR - Computer Vision extraheren
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot het extraheren van tekst met optische tekenherkenning () met behulp van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 52b6265722d5cfbf8baf54e1785ace627aa0892f
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341664"
---
# <a name="extracting-text-with-ocr"></a>Tekst extraheren met OCr

Computer Vision-technologie voor optische tekenherkenning (OCR) detecteert tekstinhoud in een afbeelding en worden de herkende tekst naar een machinaal leesbare tekenstroom geëxtraheerd. U kunt het resultaat voor zoeken en talloze andere doeleinden, zoals medische dossiers, beveiliging en bankieren. Hij detecteert de taal automatisch. OCR bespaart tijd en biedt gebruikers gemak door ze om foto's van tekst in plaats van de tekst te transcriberen.

OCR 25 talen worden ondersteund. Deze talen zijn: Arabisch, vereenvoudigd Chinees, traditioneel Chinees, Tsjechisch, Deens, Nederlands, Engels, Fins, Frans, Duits, Grieks, Hongaars, Italiaans, Japans, Koreaans, Noors, Pools, Portugees, Roemeens, Russisch, Servisch (Cyrillisch en Latijns) Slowaaks, Spaans, Zweeds en Turks.

Indien nodig, corrigeert OCR de rotatie van de herkende tekst, in graden rond de afbeelding van de horizontale as. OCR biedt het frame-coördinaten van elk woord, zoals te zien is in de volgende afbeelding.

![OCR-overzicht](./Images/vision-overview-ocr.png)

## <a name="ocr-requirements"></a>OCR-vereisten

Computer Vision kan extraheren tekst met behulp van OCR van afbeeldingen aan de volgende vereisten voldoen:

* De afbeelding moet worden weergegeven in de JPEG-, PNG-, GIF- of BMP-indeling
* De grootte van de afbeelding moet tussen 50 x 50 en 4200 x 4200 pixels


De afbeelding kan worden gedraaid in meerdere van 90 graden plus een kleine hoek van maximaal 40 graden.

## <a name="improving-ocr-accuracy"></a>Verbetering van de nauwkeurigheid van OCR

De nauwkeurigheid van de tekstherkenning, is afhankelijk van de kwaliteit van de installatiekopie. Een lezen onnauwkeurige kan worden veroorzaakt door de volgende situaties:

* Fuzzy afbeeldingen.
* Handgeschreven of cursieve tekst.
* Artistieke lettertypestijlen.
* Kleine tekengrootte.
* Complexe achtergronden, schaduwen of schitterende via vervorming van tekst of een perspectief.
* Ontbreekt of grote hoofdletters aan het begin van woorden
* Subscript, superscript of doorgehaalde tekst.

### <a name="ocr-limitations"></a>OCR-beperkingen

Fout-positieven kunnen op foto's waarin tekst dominante wordt afkomstig zijn van gedeeltelijk herkende woorden. Precisie kan op sommige foto's, met name foto's zonder tekst, veel variëren afhankelijk van het type afbeelding.

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [herkennen van afdrukken en handgeschreven tekst](concept-recognizing-text.md).
