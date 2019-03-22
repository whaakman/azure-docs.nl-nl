---
title: Overzicht van project akoestische
titlesuffix: Azure Cognitive Services
description: Project akoestische is een akoestische-engine voor 3D-interactieve ervaringen, integratie van sparen wave physics simulatie met interactieve ontwerp besturingselementen.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: overview
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 1fc125322b83a0eb51095fac21dee05c7ffb39c1
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58313614"
---
# <a name="what-is-project-acoustics"></a>Wat is Project Acoustics?
Project akoestische is een engine voor het akoestische van wave voor 3D-interactieve ervaringen. Deze wave effecten als diffraction, portaling en weerklank effecten in complexe schermen zonder handmatige zone markup-modellen. Dit omvat ook game-engine en audio middleware-integratie. Project akoestische filosofie is vergelijkbaar met statische verlichting: gedetailleerde physics offline maken voor een fysieke basislijn en gebruikt een lichtgewicht runtime met expressieve ontwerp besturingselementen om uw artistieke doelstellingen te behalen.

![Schermafdruk van Gears of War 4 akoestische voxels weergeven](media/gears-with-voxels.jpg)

## <a name="using-wave-physics-for-interactive-acoustics"></a>Met behulp van wave physics voor interactieve akoestische
Op basis van ray akoestische methoden kunnen controleren met behulp van één bron-naar-listener ray cast bedekking of weerklank door het schatten van de lokale scène volume met een paar stralen station. Maar deze technieken onbetrouwbare kunnen zijn omdat een pebble occludes zo veel een boulder. Stralen account geen voor de manier waarop geluid bochten om objecten, een verschijnsel diffraction genoemd. Project akoestische simulatie bevat deze effecten met behulp van een simulatie op basis van wave. Het resultaat is beter voorspelbare en betrouwbare.

Project akoestische belangrijke innovatie is het paar akoestische simulatie met traditionele, gezonde ontwerpconcepten. Zet simulatieresultaten in traditionele audio DSP-parameters voor bedekking, portaling en weerklank. Controle van dit proces pakketvertaling maakt gebruik van de ontwerpfunctie. Voor meer informatie over de belangrijkste technologieën achter Project akoestische, gaat u naar de [research-projectpagina](https://www.microsoft.com/en-us/research/project/project-triton/).

![Een horizontale 2D-segment van wave doorgeven via een scène met animatie](media/wave-simulation.gif)

## <a name="setup"></a>Instellen
[Akoestische Unity-integratie van project](unity-integration.md) slepen en neerzetten en is voorzien van een invoegtoepassing van de audio-engine Unity. De Unity-besturingselementen voor audio gegevensbronnen verbeteren door het koppelen van een Project akoestische C# onderdeel van de besturingselementen op elk audio-object.

[Integratie van akoestische Unreal project](unreal-integration.md) -editor en game-invoegtoepassingen voor Unreal en een Wwise mixer-invoegtoepassing bevat. Een aangepast onderdeel audio breidt vertrouwde Wwise functionaliteit binnen Unreal met live akoestische ontwerp besturingselementen. Ontwerp besturingselementen worden ook weergegeven in Wwise op het mixer-invoegtoepassing.

## <a name="workflow"></a>Werkstroom
* **Vooraf maken:** Beginnen met het instellen van de bake door te selecteren welke geometrie reageert op akoestische, door bijvoorbeeld lichte schachten wordt genegeerd. Bewerk vervolgens automatische materiaal toewijzingen en navigatiegebieden voor de handleiding listener steekproeven selecteren. Er is geen handmatige markup voor weerklank/portal/ruimte zones.
* **Maken van:** Een stap voor analyse wordt lokaal uitgevoerd, welke voxelization en andere geometrische analyses uitvoeren op de scène op basis van bovenstaande selecties is. Resultaten worden weergegeven in de editor om te controleren van de scène setup. Verzending van bake voxel gegevens uit naar Azure worden verzonden en je weer toegang krijgen een akoestische game asset.
* **Runtime:** Laden van de asset in het abonnement en u bent klaar om te luisteren naar akoestische in het abonnement. Ontwerp het akoestische bevinden zich in de editor met behulp van besturingselementen voor nauwkeurige gegevensbronnen. De besturingselementen kunnen ook worden aangestuurd vanuit niveau uitvoeren van scripts.

## <a name="platforms"></a>Platformen
De runtime-Project akoestische invoegtoepassingen kan op dit moment worden geïmplementeerd voor de volgende platforms:
* Windows
* Android
* Xbox One

## <a name="download"></a>Downloaden
* [Project akoestische Unity-invoegtoepassing en voorbeelden](https://www.microsoft.com/en-us/download/details.aspx?id=57346)
* [Project akoestische Unreal & Wwise-invoegtoepassingen en -voorbeelden](https://www.microsoft.com/download/details.aspx?id=58090)
  * Voor binaire bestanden voor Xbox en ondersteuning contact met ons opnemen via de registreren onderstaande formulier

## <a name="contact-us"></a>Contact opnemen
* [Project akoestische forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=projectacoustics)
* [Zich registreren voor het ontvangen van updates op het Project akoestische](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u)

## <a name="next-steps"></a>Volgende stappen
* Probeer een [Project akoestische quickstart voor Unity](unity-quickstart.md) of voor [Unreal](unreal-quickstart.md)
* Verken de [ontwerpplan van Project akoestische geluid](design-process.md)

