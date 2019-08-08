---
title: Overzicht van de project akoestische
titlesuffix: Azure Cognitive Services
description: Project akoestische is een akoestische engine voor 3D-interactieve ervaringen, waarbij geïntegreerde Golf fysica simulatie kan worden geïntegreerd met interactieve ontwerp besturings elementen.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: overview
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 382d4febed98b53a469ac2788e6e8c8cbf064c23
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854817"
---
# <a name="what-is-project-acoustics"></a>Wat is Project Acoustics?
Project akoestische is een golf akoestische engine voor 3D-interactieve ervaringen. De IT-modellen zijn wave-effecten zoals Diffraction, portaling en galm effecten in complexe scènes zonder hand matige zone markeringen te vereisen. Het omvat ook Game-Engine en integratie van de audio-middleware. De "filosofie van het project" is vergelijkbaar met statische belichting: maken gedetailleerde fysieke fysica offline om een fysieke basis lijn te bieden en gebruik een licht gewicht runtime met duidelijke ontwerp besturings elementen om te voldoen aan uw artistieke doelen.

![Scherm afbeelding van het vistuig van War 4 met akoestische voxels](media/gears-with-voxels.jpg)

## <a name="using-wave-physics-for-interactive-acoustics"></a>Golf fysica gebruiken voor interactieve akoestische
Op Ray gebaseerde akoestische methoden kunnen controleren op bedekking met behulp van één bron-naar-Luister-Ray-cast of een galm van het lokale scène volume met een paar stralen. Deze technieken kunnen echter onbetrouwbaar zijn omdat een Pebble occludes net zo groot is als een Boulder. Stralen hebben geen rekening met de manier waarop geluid rond objecten wordt gebocht, een verschijnsel bekend als Diffraction. Met de simulatie project akoestische wordt deze effecten vastgelegd met behulp van een simulatie op basis van een golf. Het resultaat is meer voorspelbaar en betrouwbaar.

De belangrijkste innovatie van een project is het koppelen van akoestische simulatie met traditionele ontwerp concepten. Het zet simulatie resultaten om in traditionele para meters van de audio-DSP voor bedekking, portaling en galm. De ontwerp functie maakt gebruik van besturings elementen voor dit vertaal proces. Ga naar de [pagina onderzoek project](https://www.microsoft.com/en-us/research/project/project-triton/)voor meer informatie over de belangrijkste technologieën achter de project akoestische.

![Animatie waarin een horizon taal 2D-segment met een golf door een scène wordt weer gegeven](media/wave-simulation.gif)

## <a name="setup"></a>Instellen
De integratie van de unit van het [project](unity-integration.md) is slepen en neerzetten en bevat een invoeg toepassing voor eenheids audio-engine. Breid de besturings elementen voor unit-audio bron toe door een onderdeel C# van een project akoestische controles toe te voegen aan elk audio object.

De [Unreal-integratie](unreal-integration.md) van het project bevat een editor en Game-invoeg toepassingen voor Unreal, en een WWise mixer-invoeg toepassing. Een aangepast audio onderdeel breidt de vertrouwde WWise-functionaliteit uit binnen Unreal met de ontwerp besturings elementen van Live akoestische ontwerpen. Ontwerp besturings elementen worden ook weer gegeven in WWise op de mixer-invoeg toepassing.

## <a name="workflow"></a>Werkstroom
* **Pre-maken:** Begin met het instellen van de maken door te selecteren welke geometrie reageert op akoestische, bijvoorbeeld door de lichte schachten te negeren. Bewerk vervolgens automatische materiaal toewijzingen en selecteer vervolgens navigatie gebieden om de steek proef van de listener te begeleiden. Er is geen hand matige opmaak voor galm/Portal/room-zones.
* **Maken** Een analyse stap wordt lokaal uitgevoerd, wat voxelization en andere geometrische analyses op de scène op basis van bovenstaande selecties. De resultaten worden gevisualiseerd in de editor om de installatie van de scène te controleren. Bij maken-verzen ding worden Voxel-gegevens naar Azure verzonden en wordt er een speld geluids activum teruggeleid.
* **Gezamenlijke** Laad de asset in uw niveau en u bent klaar om te Luis teren naar akoestische op uw niveau. Ontwerp de akoestische in editor met behulp van nauw keurige besturings elementen per bron. De besturings elementen kunnen ook worden gebaseerd op het niveau van scripts.

## <a name="platforms"></a>Platforms
De invoeg toepassingen voor het uitvoeren van de uitvoering van het project kunnen momenteel worden geïmplementeerd op de volgende platforms:
* Windows
* Android
* Xbox One

## <a name="download"></a>Downloaden
* [Invoeg toepassing en voor beelden van de project akoestische eenheid](https://www.microsoft.com/en-us/download/details.aspx?id=57346)
* [Projectbudget Unreal & WWise-invoeg toepassingen en-voor beelden](https://www.microsoft.com/download/details.aspx?id=58090)
  * Voor Xbox binaire bestanden en ondersteuning kunt u contact met ons opnemen via het onderstaande aanmeldings formulier

## <a name="contact-us"></a>Contact opnemen
* [Discussie over project akoestische en probleem rapportage](https://github.com/microsoft/ProjectAcoustics/issues)
* [Meld u aan voor het ontvangen van updates voor project akoestische](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u)

## <a name="next-steps"></a>Volgende stappen
* Probeer een [Snelstartgids voor project akoestische voor unititeit](unity-quickstart.md) of voor [Unreal](unreal-quickstart.md)
* Verken het [geluids ontwerp filosofie van project akoestische](design-process.md)

