---
title: Afgedrukte, handgeschreven tekst herkennen - Computer Vision
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot het herkennen van afdrukken en handgeschreven tekst in afbeeldingen met behulp van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 48ce15a11c3e3282535420f3e1bb1915276d70f5
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56313174"
---
# <a name="recognizing-printed-and-handwritten-text"></a>Afgedrukte en handgeschreven tekst herkennen

Computer Vision kunt detecteren en extraheer gedrukte of handgeschreven tekst uit afbeeldingen van verschillende objecten met verschillende oppervlakken en achtergronden, zoals ontvangstbevestigingen voor e-mails, posters, visitekaartjes, letters en whiteboards.

De tekstfunctie is heel vergelijkbaar met [optische tekenherkenning (OCR)](concept-extracting-text-ocr.md), maar in tegenstelling tot OCR het asynchroon wordt uitgevoerd en maakt gebruik van modellen voor spraakherkenning bijgewerkt.

> [!NOTE]
> Deze technologie is momenteel in preview en is alleen beschikbaar voor Engelse tekst.

## <a name="text-recognition-requirements"></a>Vereisten voor tekst

Computer Vision kan herkennen gedrukte en handgeschreven tekst in afbeeldingen aan de volgende vereisten voldoen:

- De afbeelding moet in JPEG, PNG of BMP-indeling worden gepresenteerd
- De afbeelding moet kleiner zijn dan 4 megabyte (MB)
- De afmetingen van de afbeelding moet tussen 50 x 50 en 4200 x 4200 pixels

## <a name="next-steps"></a>Volgende stappen

Zie de [herkennen tekst referentiedocumentatie voor](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) voor meer informatie.