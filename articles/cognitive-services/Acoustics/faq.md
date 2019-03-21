---
title: Veelgestelde vragen over project akoestische
titlesuffix: Azure Cognitive Services
description: Deze pagina vindt u antwoorden op vragen die vaak worden gesteld over Project akoestische, met inbegrip van instructies downloaden en te maken van proces.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: resources
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: c43c81d42a39bda504b02eb6c053a16a2cf53aec
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58138045"
---
# <a name="project-acoustics-frequently-asked-questions"></a>Veelgestelde vragen over project akoestische

## <a name="what-is-project-acoustics"></a>Wat is Project Acoustics?

De suite Project akoestische van invoegtoepassingen is een akoestische-systeem dat geluid wave gedrag voorafgaand aan de runtime, cloudbeheer statische belichting berekent. De cloud heeft het zware werk van wave physics berekeningen, zodat de runtimekosten CPU laag is.  

## <a name="where-can-i-download-the-plugin"></a>Waar kan ik de invoegtoepassing downloaden?

U kunt downloaden Project akoestische uit de [Project akoestische Downloadcentrum](https://www.microsoft.com/en-us/download/details.aspx?id=57346).

## <a name="does-project-acoustics-support-x-platform"></a>Biedt ondersteuning voor Project akoestische <x> platform?

Ondersteuning voor project akoestische platform zich verder ontwikkelt op basis van de behoeften van klanten. Neem contact op met ons op de [Project akoestische forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=projectacoustics) om u te informeren over de ondersteuning voor extra platforms.

## <a name="is-azure-used-at-runtime"></a>Is Azure tijdens runtime gebruikt?

Nee, wordt alleen tijdens de fase precompute cloudintegratie gebruikt als onderdeel van de scène-instellingen.
 
## <a name="what-is-simulation-input"></a>Wat is de simulatie invoer? 

De invoer van de simulatie is uw 3D-scènes, virtuele omgeving of niveau spel. Project akoestische uitvoert 3D Breng wave simulaties die de physics van geluid nauw, model met inbegrip van smooth bedekking en spreiding.
 
## <a name="what-is-the-runtime-cost"></a>Wat zijn de runtimekosten?

Akoestische duurt ongeveer 0,01% van de CPU per gegevensbron per frame. RAM-gebruik is afhankelijk van de grootte van de scène en kan variëren van 10 tot 100 MB.
 
## <a name="do-i-need-to-simplify-the-level-geometry-control-triangle-count-make-meshes-watertight"></a>Heb ik nodig voor het vereenvoudigen van het niveau geometrie? Driehoek aantal bepalen? Netten waterdichte maken?

Nee. Het systeem wordt gedetailleerd niveau geometrie rechtstreeks opnemen. Dit is voxelized voor interne verwerking.
 
## <a name="whats-in-the-runtime-lookup-table"></a>Wat is in de runtime-opzoektabel?

De ACE-bestand bevat een tabel met akoestische parameters tussen talrijke bron en locatie-paren listener, evenals voxelized scène geometrie die wordt gebruikt voor de parameter interpolatie.
 
## <a name="can-project-acoustics-handle-moving-sources"></a>Aankan Project akoestische verplaatst bronnen?

Ja, Project akoestische raadpleegt de opzoektabel en de audio-DSP op elke tik bijgewerkt zodat deze kan omgaan met het verplaatsen van bronnen en listener.
 
## <a name="can-project-acoustics-handle-dynamic-geometry-closing-doors-walls-blown-away"></a>Kan Project akoestische dynamische geometrie verwerken? Sluit deuren? Wanden verbazingwekkend opslaan?

Nee. De akoestische parameters zijn joinfilters op basis van de vaste status van een niveau spel. Het is raadzaam klep geometrie buiten akoestische verlaten en vervolgens extra bedekking op basis van de status van die toe te passen en roerende game objecten met behulp van tot stand gebracht technieken.
 
## <a name="does-project-acoustics-use-acoustic-materials"></a>Maakt gebruik van Project akoestische akoestische materiaal?

Ja. Materialen worden verzameld van de fysieke materiaal namen in uw niveau absorptivity te stimuleren.
 
## <a name="what-do-the-probes-represent"></a>Wat de "tests" vertegenwoordigen?

Tests zijn een voorbeeld van mogelijke player locaties. Elke test vertegenwoordigt een afzonderlijke wave simulatie van de scène dat afkomstig is van de locatie van de test. Akoestische parameters voor de locatie van de listener worden tijdens runtime geïnterpoleerd uit in de buurt test locaties.
 
## <a name="why-spend-so-much-compute-in-the-cloud-what-does-it-buy-me"></a>Waarom besteden aan veel compute in de cloud? Wat het kopen mij?

Project akoestische biedt nauwkeurige en betrouwbare akoestische parameters zelfs voor zeer complexe virtuele omgevingen, waarbij elke architectuur aspect rekening wordt gehouden. Het biedt goede bedekking en obstakel en dynamische weerklank variatie zonder handmatig werk van het tekenen van volumes. Alle en tegelijkertijd de kosten aan het CPU licht tijdens runtime.

## <a name="what-exactly-happens-during-baking"></a>Wat er precies gebeurt tijdens het 'bakken'?

Een bake bestaat uit akoestische wave simulaties van cuboid simulatie regio's gecentreerd op elke listener-test.

## <a name="next-steps"></a>Volgende stappen
* Probeer de [Project akoestische Unity-voorbeeldinhoud](unity-quickstart.md) of [Unreal voorbeelden voor inhoud](unreal-quickstart.md)

