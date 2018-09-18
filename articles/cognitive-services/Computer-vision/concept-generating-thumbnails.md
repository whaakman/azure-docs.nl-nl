---
title: Genereren van miniaturen - Computer Vision
titleSuffix: Azure Cognitive Services
description: Concepten met betrekking tot het genereren van miniaturen van afbeeldingen met behulp van de Computer Vision-API.
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: ff16fa4d30fb60e6b1376027a40242af23be04d1
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985342"
---
# <a name="generating-thumbnails"></a>Miniaturen genereren

Een miniatuur is een kleine weergave van een afbeelding op volledige grootte. Uiteenlopende apparaten zoals telefoons, tablets en pc's maakt u een andere gebruiker gebruikerservaring (UX)-indelingen en miniatuurweergaven nodig. Met slim bijsnijden, kunt deze Computer Vision-functie u het probleem is opgelost.

Na het uploaden van een afbeelding, Computer Vision genereert een miniatuur van hoge kwaliteit en analyseert de objecten op de afbeelding om te identificeren de *regio van belang* (ROI). Deze kan eventueel bijsnijden de afbeelding om te voldoen aan de vereisten van het rendement op investering opleveren. De miniatuur van het gegenereerde kan met behulp van een hoogte-breedteverhouding die verschilt van de hoogte-breedteverhouding van de oorspronkelijke afbeelding, om aan uw behoeften te worden weergegeven.

De miniaturen algoritme werkt als volgt:

1. Hiermee verwijdert u storende elementen van de installatiekopie en identificeert het object, de regio van belang zijn.
2. Snijdt de afbeelding op basis van het geïdentificeerde regio van belang zijn.
3. Hiermee wijzigt u de hoogte-breedteverhouding aanpassen aan de grootte van het doel miniatuur.

De miniatuur van het gegenereerde kan sterk verschillen, afhankelijk van wat u voor de hoogte, breedte en slim bijsnijden opgeeft, zoals wordt weergegeven in de volgende afbeelding.

![Miniaturen](./Images/thumbnail-demo.png)

## <a name="thumbnail-generation-examples"></a>Voorbeelden van miniaturen te genereren

De volgende tabel ziet u standaard miniatuurweergaven die worden gegenereerd door de Computer Vision voor installatiekopieën van het voorbeeld. De miniaturen zijn gegenereerd voor een opgegeven doel-hoogte en breedte van 50 pixels, met een slim bijsnijden ingeschakeld.

| Installatiekopie | Miniatuur |
|-------|-----------|
|![Outdoor Mountain](./Images/mountain_vista.png) | ![Buitengebruik Mountain miniatuur](./Images/mountain_vista_thumbnail.png) |
|![Vision bloem analyseren](./Images/flower.png) | ![Vision analyseren bloem miniatuur](./Images/flower_thumbnail.png) |
|![Vrouw plafond](./Images/woman_roof.png) | ![Vrouw plafond miniatuur](./Images/woman_roof_thumbnail.png) |

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [installatiekopieën taggen](concept-tagging-images.md) en [categoriseren van beelden](concept-categorizing-images.md).