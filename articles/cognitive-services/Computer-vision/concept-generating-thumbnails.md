---
title: Genereren van miniaturen - Computer Vision
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot het genereren van miniaturen van afbeeldingen met behulp van de Computer Vision-API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: pafarley
ms.openlocfilehash: 7d914f394ecfcf02ed26f41cd8fe2ef799cf6103
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52966735"
---
# <a name="generating-thumbnails"></a>Miniaturen genereren

Een miniatuur is een kleiner formaat weergave van een afbeelding. Miniaturen worden gebruikt om afbeeldingen en andere gegevens op een meer betaalbare en lay-out-vriendelijk manier vertegenwoordigen. De Computer Vision-API maakt gebruik van slim bijsnijden, samen met het formaat van de afbeelding, intuïtieve miniaturen voor een bepaalde installatiekopie maken.

De generatie van miniaturen Computer Vision-algoritme werkt als volgt:
1. Verwijderen van bepaalde elementen van de installatiekopie en Identificeer de _interessegebied_&mdash;het gebied van de installatiekopie waarin de belangrijkste objecten wordt weergegeven.
1. De afbeelding op basis van het geïdentificeerde bijsnijden _interessegebied_.
1. De hoogte-breedteverhouding aanpassen aan de miniaturen dimensies doel wijzigen.

## <a name="area-of-interest"></a>Interessegebied

Wanneer u een afbeelding uploadt, de Computer Vision-API analyseert ze om te bepalen de *interessegebied*. Deze kunt vervolgens deze regio gebruiken om te bepalen hoe de afbeelding bijsnijden. De bewerking bijsnijden, echter altijd overeen met de gewenste hoogte-breedteverhouding als er een is opgegeven.

U krijgt ook de onbewerkte selectiekader box-coördinaten van deze dezelfde *interessegebied* door het aanroepen van de **areaOfInterest** API in plaats daarvan. U kunt deze gegevens vervolgens naar de oorspronkelijke afbeelding wijzigen, maar u wilt gebruiken.

## <a name="examples"></a>Voorbeelden

De miniatuur van het gegenereerde kan sterk verschillen, afhankelijk van wat u voor de hoogte, breedte en slim bijsnijden opgeeft, zoals wordt weergegeven in de volgende afbeelding.

![Miniaturen](./Images/thumbnail-demo.png)

De volgende tabel ziet u standaard miniatuurweergaven die worden gegenereerd door de Computer Vision voor installatiekopieën van het voorbeeld. De miniaturen zijn gegenereerd voor een opgegeven doel-hoogte en breedte van 50 pixels, met een slim bijsnijden ingeschakeld.

| Installatiekopie | Miniatuur |
|-------|-----------|
|![Berglandschap](./Images/mountain_vista.png) | ![Buitengebruik Mountain miniatuur](./Images/mountain_vista_thumbnail.png) |
|![Vision-analyse Bloem](./Images/flower.png) | ![Vision analyseren bloem miniatuur](./Images/flower_thumbnail.png) |
|![Vrouw op dak](./Images/woman_roof.png) | ![Vrouw plafond miniatuur](./Images/woman_roof_thumbnail.png) |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [installatiekopieën taggen](concept-tagging-images.md) en [categoriseren van beelden](concept-categorizing-images.md).