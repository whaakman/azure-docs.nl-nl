---
title: Tekst met optische tekenherkenning (OCR) - Computer Vision extraheren
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot het extraheren van tekst met optische tekenherkenning () met behulp van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: deb73eb9fdd6879a5fbe1fed820bf92b2d627b65
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56310437"
---
# <a name="extract-text-with-optical-character-recognition"></a>Haal de tekst met optische tekenherkenning

Computer-Vision optische tekenherkenning (OCR) functie detecteert tekstinhoud in een installatiekopie en de herkende tekst converteert naar een machinaal leesbare tekenstroom. U kunt het resultaat voor veel doeleinden, zoals zoeken, medische dossiers, beveiliging en bankieren. 

OCR 25 talen worden ondersteund: Arabisch, Chinees (traditioneel), Chinees (vereenvoudigd), Deens, Duits, Engels, Fins, Frans, Grieks, Hongaars, Italiaans, Japans, Koreaans, Nederlands, Noors, Pools, Portugees, Roemeens, Russisch, Servisch (Cyrillisch en Latijns) Slowaaks, Spaans, Tsjechisch, Turks en Zweeds. OCR detecteert automatisch de taal van de gedetecteerde tekst.

Indien nodig, corrigeert OCR de rotatie van de herkende tekst door de verschuiving van de rotatiesnelheid in graden over de afbeelding van de horizontale as worden geretourneerd. OCR biedt ook de frame-coördinaten van elk woord, zoals te zien is in de volgende afbeelding.

![Een diagram met een afbeelding wordt gedraaid en de tekst wordt gelezen en afgebakend](./Images/vision-overview-ocr.png)

## <a name="image-requirements"></a>Vereisten voor installatiekopieën

Computer Vision kan extraheren tekst met behulp van OCR van afbeeldingen aan de volgende vereisten voldoen:

* De afbeelding moet worden weergegeven in de JPEG-, PNG-, GIF- of BMP-indeling
* De grootte van de afbeelding moet tussen 50 x 50 en 4200 x 4200 pixels
* De tekst in de afbeelding kan worden gedraaid in meerdere van 90 graden plus een kleine hoek van maximaal 40 graden.

## <a name="improving-ocr-accuracy"></a>Verbetering van de nauwkeurigheid van OCR

De nauwkeurigheid van de tekstherkenning is afhankelijk van de kwaliteit van de afbeelding. Een lezen onnauwkeurige kan worden veroorzaakt door het volgende:

* Wazige afbeeldingen.
* Handgeschreven of cursieve tekst.
* Artistieke lettertypestijlen.
* Kleine lettergrootte.
* Complexe achtergronden, schaduwen of schittering op de tekst of vervorming van het perspectief.
* Te grote of geen hoofdletter aan het begin van woorden
* Subscript, superscript of doorgehaalde tekst.

### <a name="ocr-limitations"></a>OCR-beperkingen

Fout-positieven kunnen op waarin tekst dominante wordt afbeeldingen afkomstig zijn van gedeeltelijk herkende woorden. Precisie kan op sommige afbeeldingen, met name foto's zonder tekst, veel variëren afhankelijk van het type afbeelding.

## <a name="next-steps"></a>Volgende stappen

Zie de [OCR-referentiedocumentatie](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) voor meer informatie.
