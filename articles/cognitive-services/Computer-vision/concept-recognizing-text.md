---
title: Voor het herkennen van afdrukken en handgeschreven tekst - Computer Vision
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot het herkennen van afdrukken en handgeschreven tekst in afbeeldingen met behulp van de Computer Vision-API.
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: 49cba0e9b6958beb07b6f074e6dc748679514525
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985308"
---
# <a name="recognizing-printed-and-handwritten-text"></a>Afgedrukte en handgeschreven tekst herkennen

Computer Vision kunt detecteren en extraheer gedrukte of handgeschreven tekst uit afbeeldingen van verschillende objecten met verschillende oppervlakken en achtergronden, zoals ontvangstbevestigingen voor e-mails, posters, visitekaartjes, letters en whiteboards.

Tekstherkenning bespaart tijd en moeite. U kunt productiever zijn door een afbeelding van tekst in plaats van deze te transcriberen. Tekstherkenning maakt het u notities digitaliseren. Deze digitalisering kunt u snel en eenvoudig zoekfuncties moeten worden geïmplementeerd. Bovendien verspilt u minder papier.

## <a name="text-recognition-requirements"></a>Vereisten voor tekst

Computer Vision kan herkennen gedrukte en handgeschreven tekst in afbeeldingen aan de volgende vereisten voldoen:

- De afbeelding moet in JPEG, PNG of BMP-indeling worden gepresenteerd
- De afbeelding moet kleiner zijn dan 4 megabyte (MB)
- De afmetingen van de afbeelding moeten liggen tussen 40, 40 en 3200 x 3200 pixels

> [!NOTE]
> Deze technologie is momenteel in preview en is alleen beschikbaar voor Engelse tekst.

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [extraheren van tekst met OCR](concept-extracting-text-ocr.md).