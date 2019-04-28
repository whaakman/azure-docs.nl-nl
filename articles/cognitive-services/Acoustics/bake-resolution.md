---
title: Project akoestische Bake resolutie
titlesuffix: Azure Cognitive Services
description: In dit overzicht wordt het verschil tussen grof en fijn oplossingen tijdens het akoestische bakken beschreven.
services: cognitive-services
author: KyleStorck
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: how-to
ms.date: 04/05/2019
ms.author: KyleStorck
ms.openlocfilehash: 7dbf63ba39c5dcdebb363cfc37a45f0216a07497
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61335444"
---
# <a name="project-acoustics-bake-resolution"></a>Project akoestische Bake resolutie
In dit overzicht wordt het verschil tussen grof en fijn oplossingen tijdens het akoestische bakken beschreven. U kiezen deze instelling tijdens de stap tests van de werkstroom gebakken.

## <a name="Coarse-vs-Fine-Resolution"></a>Fijn Grof Visual Studio-oplossing

Het enige verschil tussen de instellingen voor het omzetten van grove en goed is de frequentie waarmee de simulatie wordt uitgevoerd. Fijn maakt gebruik van een twee keer zo hoog als grof frequentie. Dit heeft een aantal gevolgen op de akoestische simulatie:

* De golflengte voor grof twee keer zo lang als fijn is, en daarom de voxels tweemaal zo groot zijn.
* De tijd van de simulatie is direct gerelateerd aan de voxel grootte, waardoor een grof bake ongeveer 16 keer sneller dan een prima bake.
* Portals (bijvoorbeeld deuren of windows) kleiner is dan de grootte van de voxel niet kunnen worden gesimuleerd. De grof instelling kan ertoe leiden dat sommige van deze kleinere portals voor het niet worden gesimuleerd; Daarom wordt niet ze doorgegeven via geluid tijdens runtime. U kunt zien als dit gebeurt door het voxels weer te geven.
* De frequentie van de lagere simulatie resulteert in minder diffraction rond hoeken en randen.
* Geluid bronnen kunnen niet worden gevonden in "filled" voxels (dat wil zeggen voxels die geometry bevatten). Dit resulteert in geen geluid. Het is moeilijker geluid bronnen plaatsen, zodat ze niet binnen de grotere voxels van grove vallen dan wanneer u het fijn instelling.
* De grotere voxels wordt inbreuk maken meer bij portals, zoals hieronder wordt weergegeven. De eerste afbeelding is gemaakt met behulp van abrupte, terwijl de tweede is de dezelfde poort met fijn resolutie. Zoals aangegeven door de rode markeringen, moet u er veel minder inbreuk in de poort die met behulp van de instelling van de orde is. De blauwe lijn is de poort, zoals gedefinieerd door de geometrie, terwijl de rode lijn de effectieve akoestische portal gedefinieerd door de grootte van de voxel wordt. Hoe deze indringers wordt afgespeeld uit in een bepaalde situatie is afhankelijk van de volledig op hoe de voxels uitgelijnd met de geometrie van de portal, die wordt bepaald door de grootte en de locaties van de objecten in de scène.

![Schermafbeelding van grove voxels invullen van een poort in Unreal](media/unreal-coarse-bake.png)

![Schermafbeelding van fijn voxels in een poort in Unreal](media/unreal-fine-bake.png)

## <a name="next-steps"></a>Volgende stappen

Probeer de instellingen voor het omzetten van grove en fijn zelf met behulp van onze [Unreal](unreal-baking.md) of [Unity](unity-baking.md) invoegtoepassingen.