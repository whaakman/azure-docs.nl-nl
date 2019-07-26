---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: 07e7cc991f127bf4bb4f466c0108962786e45bce
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68423559"
---
Let op de schuif regelaar voor de **waarschijnlijkheids drempelwaarde** in het linkerdeel venster van het tabblad **prestaties** . Dit is het betrouwbaarheids niveau dat een voor spelling moet hebben om als correct te worden beschouwd (met het oog op de berekening van de precisie en het terughalen). 

Wanneer u Voorspellings aanroepen met een hoge drempel waarde interpreteert, retour neren ze doorgaans resultaten met een hoge precisie op&mdash;basis van de kosten voor het intrekken van de gedetecteerde classificaties, maar veel blijven niet gedetecteerd. Een lage waarschijnlijkheids drempel waarde wordt het&mdash;tegenovergestelde van de werkelijke classificaties gedetecteerd, maar er zijn meer valse positieven in die set. Daarom moet u de drempel waarde voor de kans instellen op basis van de specifieke behoeften van uw project. Later, wanneer u de resultaten van de voor spelling op de client ontvangt, moet u dezelfde drempel waarde voor de kans gebruiken als u hier gebruikt.