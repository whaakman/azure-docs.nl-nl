---
title: Miniaturen genereren
titleSuffix: Computer Vision - Cognitive Services - Azure
description: Concepten met betrekking tot het genereren van miniaturen van afbeeldingen met behulp van de Computer Vision in Azure Cognitive Services.
services: cognitive-services
author: deken
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: 92de0c0a428c9010188131a768074234241ed604
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44725361"
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