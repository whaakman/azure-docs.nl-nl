---
title: Wat is Project Acoustics?
titlesuffix: Azure Cognitive Services
description: De invoegtoepassing Project Acoustics Unity biedt occlusie, reverberatie en spatialisatie voor projecten die gericht zijn op virtual reality (VR) en traditionele schermen.
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: overview
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 175203269f524f5c680345602e6382f5c26f1bd1
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55178899"
---
# <a name="what-is-project-acoustics"></a>Wat is Project Acoustics?
De invoegtoepassing Project Acoustics Unity biedt occlusie, reverberatie en spatialisatie voor projecten die gericht zijn op virtual reality (VR) en traditionele schermen. De invoegtoepassing biedt een manier om game-akoestiek te ontwerpen waarmee de bedoelingen van de ontwerper als een laag over een met fysica gesimuleerde golfbeweging wordt aangebracht.

## <a name="why-use-acoustics-in-virtual-environments"></a>Waarom wordt akoestiek in een virtuele omgeving gebruikt?
Mensen gebruiken audiovisuele aanwijzingen om de wereld om hen heen te kunnen begrijpen. Door middel van een combinatie van ruimtelijk geluid en akoestiek beleeft een gebruiker de virtuele wereld intensiever. Het akoestische hulpmiddel dat hier wordt beschreven, analyseert virtuele werelden om zo een realistische, akoestische simulatie te creëren. Het ondersteunt het ontwerpproces na de simulatie. Bij de analyse zijn de geometrie en de materialen voor elk oppervlak in de wereld betrokken. De simulatie omvat parameters als richting van een inkomend signaal (portaling), reverberatievermogen, vervaltijden en occlusie- en obstructie-effecten.

## <a name="how-does-this-approach-to-acoustics-work"></a>Hoe werkt deze aanpak met betrekking tot akoestiek?
Het systeem is afhankelijk van een offlineberekening van de virtuele wereld, waardoor een complexere situatie mogelijk is dan wanneer de analyse tijdens runtime zou worden uitgevoerd. De offlineberekening levert een opzoektabel met akoestische parameters op. Een ontwerper geeft de regels op die tijdens runtime op de parameters worden toegepast. Door de regels wat aan te passen kunnen hyperrealistische effecten worden bereikt voor een sterkere emotionele ervaring of minder realistische scenes om meer achtergrondgeluiden te krijgen.

## <a name="design-process-comparison"></a>Vergelijking van de ontwerpprocessen
De invoegtoepassing Proces Acoustics ondersteunt een nieuw ontwerpproces voor akoestiek in Unity-scenes. Dit ontwerpproces laat zich het beste uitleggen door het te vergelijken met een van de huidige populaire benaderingen van akoestiek.

### <a name="typical-approach-to-acoustics-today"></a>Gebruikelijke, hedendaagse benadering van akoestiek
Bij een gebruikelijke benadering van de akoestiek worden reverberatievolumes getekend:

![Ontwerpweergave](media/reverbZonesAltSPace2.png)

Vervolgens worden de parameters voor elke zone enigszins aangepast:

![Ontwerpweergave](media/TooManyReverbParameters.png)

Ten slotte wordt er raytracing-logica aan toegevoegd voor de juiste wijze van toepassen van occlusie- en obstructiefilters gedurende de hele scene, en padzoeklogica voor portaling. Deze code kan extra kosten tijdens runtime met zich meebrengen. Er zijn ook problemen met gladde hoeken en er is sprake van edge cases bij onregelmatig gevormde scenes.

### <a name="an-alternative-approach-with-physics-based-design"></a>Een alternatieve benadering met op fysica gebaseerd ontwerp
Met de benadering van de Unity-invoegtoepassing van Project Acoustics beschikt u over een statische vorm en statische materialen voor een scene. Omdat de scene uit voxels is opgebouwd en er geen gebruik wordt gemaakt van raytracing, hebt u geen vereenvoudigd of waterdicht akoestisch gaas nodig. Het is evenmin noodzakelijk de scene met reverberatievolumes te verbeteren. Met de invoegtoepassing wordt de scene naar de cloud geüpload, waarbij van op fysica gebaseerde golfsimulatie gebruik wordt gemaakt. Het resultaat wordt als een opzoektabel in uw project geïntegreerd en kan voor een fraai uiterlijk of game-effecten worden aangepast.

![Ontwerpweergave](media/GearsWithVoxels.jpg)

## <a name="requirements"></a>Vereisten
* Unity 2018.2+ voor akoestische creaties en Unity 5.2+ voor audio-ontwerp en -implementatie
* Windows 64-bits Unity Editor
* Azure Batch-abonnement voor akoestische creaties
* De runtime voor scripting van Unity moet worden ingesteld op .NET 4.x equivalent

## <a name="platform-support"></a>Platformondersteuning
* Windows-desktop (x86 en AMD64)
* Windows UWP (x86, AMD64 en ARM)
* Android (x86 en ARM64)

## <a name="download"></a>Downloaden
Als u een beoordeling wilt geven van de akoestische invoegtoepassing, kunt u zich [hier](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u) registreren om deel te nemen aan de Designer Preview.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [ontwikkelproces](design-process.md)
* Aan de slag met het [integreren van akoestiek in uw Unity-project](getting-started.md)

