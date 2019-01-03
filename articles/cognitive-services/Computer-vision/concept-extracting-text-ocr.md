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
ms.custom: seodec18
ms.openlocfilehash: 07d3f5d365fa5c552ccb61c97532a9931b7e282e
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53583677"
---
# <a name="extracting-text-with-optical-character-recognition"></a>Tekst met optische tekenherkenning extraheren

Computer Vision-technologie voor optische tekenherkenning (OCR) detecteert tekstinhoud in een afbeelding en worden de herkende tekst naar een machinaal leesbare tekenstroom geëxtraheerd. U kunt het resultaat gebruiken voor zoekopdrachten en talloze andere doeleinden, zoals medische dossiers, beveiliging en bankieren. Taal wordt automatisch gedetecteerd. Met OCR besparen gebruikers tijd en kunnen ze eenvoudig foto’s van tekst maken in plaats van deze over te schrijven.

OCR ondersteunt 25 talen. Deze talen zijn: Arabisch, Chinees (traditioneel), Chinees (vereenvoudigd), Deens, Duits, Engels, Fins, Frans, Grieks, Hongaars, Italiaans, Japans, Koreaans, Nederlands, Noors, Pools, Portugees, Roemeens, Russisch, Servisch (Cyrillisch en Latijns) Slowaaks, Spaans, Tsjechisch, Turks en Zweeds.

Via OCR wordt zo nodig de draaiing van de herkende tekst gecorrigeerd, in graden, rond de horizontale as van de afbeelding. OCR biedt het frame-coördinaten van elk woord, zoals te zien is in de volgende afbeelding.

![Een diagram met een afbeelding wordt gedraaid en de tekst wordt gelezen en afgebakend](./Images/vision-overview-ocr.png)

## <a name="ocr-requirements"></a>OCR-vereisten

Computer Vision kan extraheren tekst met behulp van OCR van afbeeldingen aan de volgende vereisten voldoen:

* De afbeelding moet worden weergegeven in de JPEG-, PNG-, GIF- of BMP-indeling
* De grootte van de afbeelding moet tussen 50 x 50 en 4200 x 4200 pixels


De afbeelding kan worden gedraaid in meerdere van 90 graden plus een kleine hoek van maximaal 40 graden.

## <a name="improving-ocr-accuracy"></a>Verbetering van de nauwkeurigheid van OCR

De nauwkeurigheid van de tekstherkenning is afhankelijk van de kwaliteit van de afbeelding. Een onnauwkeurige aflezing kan worden veroorzaakt door de volgende situaties:

* Wazige afbeeldingen.
* Handgeschreven of cursieve tekst.
* Artistieke lettertypestijlen.
* Kleine lettergrootte.
* Complexe achtergronden, schaduwen of schittering op de tekst of vervorming van het perspectief.
* Te grote of geen hoofdletter aan het begin van woorden
* Subscript, superscript of doorgehaalde tekst.

### <a name="ocr-limitations"></a>OCR-beperkingen

Fout-positieven kunnen op foto's waarin tekst dominante wordt afkomstig zijn van gedeeltelijk herkende woorden. Precisie kan op sommige foto's, met name foto's zonder tekst, veel variëren afhankelijk van het type afbeelding.

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [herkennen van afdrukken en handgeschreven tekst](concept-recognizing-text.md).
