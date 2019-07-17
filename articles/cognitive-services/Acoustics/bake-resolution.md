---
title: Project akoestische maken-resolutie
titlesuffix: Azure Cognitive Services
description: In dit overzicht vindt u een beschrijving van het verschil tussen grof en nauw keurige resoluties tijdens Baking akoestische.
services: cognitive-services
author: KyleStorck
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: kylsto
ms.openlocfilehash: ba7c71c666ae8f90d499bfe52303537aeb112a77
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68296704"
---
# <a name="project-acoustics-bake-resolution"></a>Project akoestische maken-resolutie
In dit overzicht vindt u een beschrijving van het verschil tussen grof en nauw keurige resoluties tijdens Baking akoestische. U kiest deze instelling tijdens de test fase van de Baking-werk stroom.

## <a name="Coarse-vs-Fine-Resolution"></a>Ruwe versus fijne resolutie

Het enige verschil tussen de instellingen grof en fijn oplossen is de frequentie waarmee de simulatie wordt uitgevoerd. Hiermee maakt u een frequentie twee keer zo hoog als grof. Dit heeft een aantal gevolgen voor de akoestische simulatie:

* De Golf lengte voor grof is twee keer zo lang als fijn, en daarom zijn de voxels twee maal zo groot.
* De simulatie tijd is direct gerelateerd aan de Voxel-grootte, waardoor een ruwe maken ongeveer 16 keer sneller is dan een fijne maken.
* Portals (bijvoorbeeld deuren of Vensters) die kleiner zijn dan de grootte van de Voxel, kunnen niet worden gesimuleerd. De instelling grof kan ertoe leiden dat sommige van deze kleinere portals niet worden gesimuleerd. Daarom geven ze geen geluid door tijdens runtime. U kunt zien of dit gebeurt door de voxels weer te geven.
* De lagere simulatie frequentie resulteert in minder Diffraction rond de hoeken en randen.
* Geluids bronnen kunnen zich niet bevinden in ' gevulde ' voxels (dat wil zeggen voxels die geometrie bevatten). Dit resulteert in geen geluid. Het is moeilijker om geluids bronnen te plaatsen, zodat ze zich niet in de grotere voxels bevinden dan bij het gebruik van de instelling fijn.
* De grotere voxels wordt in Portals intrude, zoals hieronder wordt weer gegeven. De eerste afbeelding is gemaakt met grof, terwijl de tweede dezelfde deur is als met een verfijnde resolutie. Zoals aangegeven door de rode markeringen, is er veel minder inbraak in de deur met behulp van de instelling fijn. De blauwe lijn is de deur zoals gedefinieerd door de geometrie, terwijl de rode lijn de effectief akoestische Portal is die wordt gedefinieerd door de grootte van de voxel. Hoe deze indringing afspeelt in een bepaalde situatie, hangt volledig af van de manier waarop de voxels wordt uitgelijnd met de geometrie van de portal, die wordt bepaald door de grootte en locaties van uw objecten in de scène.

![Scherm afbeelding van grof voxels vulling in Unreal](media/unreal-coarse-bake.png)

![Scherm afbeelding van een fijne voxels in een deur van Unreal](media/unreal-fine-bake.png)

## <a name="next-steps"></a>Volgende stappen

Probeer de instellingen voor grove en nauw keurigheid zelf uit met onze [Unreal](unreal-baking.md) of [Unity](unity-baking.md) -invoeg toepassingen.
