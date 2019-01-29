---
title: Veelgestelde vragen over Project akoestische
titlesuffix: Azure Cognitive Services
description: Deze pagina vindt u antwoorden op vragen die vaak worden gesteld over Project akoestische, met inbegrip van instructies downloaden en te maken van proces.
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 07e65e6ff544d6372197010a2b9d7f3f647eeb0f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55166557"
---
# <a name="frequently-asked-questions"></a>Veelgestelde vragen

## <a name="what-is-project-acoustics"></a>Wat is Project Acoustics?

De Project akoestische Unity-invoegtoepassing is een akoestische-systeem dat geluid wave gedrag voorafgaand aan de runtime, cloudbeheer statische belichting berekent. De cloud heeft het zware werk van wave physics berekeningen, zodat de runtimekosten CPU laag is.  

## <a name="where-can-i-download-the-plugin"></a>Waar kan ik de invoegtoepassing downloaden?

Als u een beoordeling wilt geven van de akoestische invoegtoepassing, kunt u zich [hier](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u) registreren om deel te nemen aan de Designer Preview.

## <a name="is-azure-used-at-runtime"></a>Is Azure tijdens runtime gebruikt?

Nee, wordt alleen tijdens de fase precompute cloudintegratie gebruikt als onderdeel van de scène-instellingen.
 
## <a name="what-is-simulation-input"></a>Wat is de simulatie invoer? 

De invoer van de simulatie is uw 3D-scènes, virtuele omgeving of niveau spel. Project akoestische uitvoert 3D Breng wave simulaties die de physics van geluid nauw, model met inbegrip van smooth bedekking en spreiding.
 
## <a name="what-is-the-runtime-cost"></a>Wat zijn de runtimekosten?

Akoestische duurt ongeveer 0,01% van de CPU per gegevensbron per frame. RAM-gebruik is afhankelijk van de grootte van de scène en kan variëren van 10 tot 100 MB.
 
## <a name="do-i-need-to-simplify-the-level-geometry-control-triangle-count-make-meshes-watertight"></a>Heb ik nodig voor het vereenvoudigen van het niveau geometrie? Driehoek aantal bepalen? Netten waterdichte maken?

Nee. Het systeem wordt gedetailleerd niveau geometrie rechtstreeks opnemen. Dit is voxelized voor interne verwerking.
 
## <a name="whats-in-the-runtime-lookup-table"></a>Wat is in de runtime-opzoektabel?

De ACE-bestand is een tabel met akoestische parameters tussen verschillende bron- en -listener locatie-paren.
 
## <a name="can-it-handle-moving-sources"></a>Kan het verwerken van verplaatst bronnen?

Ja, de **Microsoft Acoustics** ruimtelijk-invoegtoepassing voor Unity raadpleegt u de opzoektabel op elke maatstreepjes audio verwerken met de huidige locaties van de bron- en -listener. Van de ruimtelijk DSP werkt probleemloos de akoestische verwerken van parameters op elke maatstreepjes.
 
## <a name="can-it-handle-dynamic-geometry-closing-doors-walls-blown-away"></a>Kan het verwerken van dynamische geometrie? Sluit deuren? Wanden verbazingwekkend opslaan?

Nee. De akoestische parameters zijn joinfilters op basis van de vaste status van een niveau spel. We stellen klep geometrie buiten akoestische verlaten, en vervolgens extra bedekking op basis van de status van die en roerende game objecten met behulp van tot stand gebrachte technieken toepassen.
 
## <a name="does-it-handle-materials"></a>Wordt het materiaal verwerkt?

Ja. Materialen worden verzameld van de fysieke materiaal namen in uw niveau absorptivity te stimuleren.
 
## <a name="what-do-the-probes-represent"></a>Wat de "tests" vertegenwoordigen?

Tests zijn een voorbeeld van mogelijke player locaties. Elke test vertegenwoordigt een afzonderlijke wave simulatie van de scène dat afkomstig is van de locatie van de test. Akoestische parameters voor de locatie van de listener worden tijdens runtime geïnterpoleerd uit in de buurt test locaties.
 
## <a name="why-spend-so-much-compute-in-the-cloud-what-does-it-buy-me"></a>Waarom besteden aan veel compute in de cloud? Wat het kopen mij?

Project akoestische biedt nauwkeurige en betrouwbare akoestische parameters zelfs voor zeer complexe virtuele omgevingen, waarbij elke architectuur aspect rekening wordt gehouden. Het biedt goede bedekking/obstakel zonder handmatig werk en dynamische weerklank variatie zonder te tekenen van volumes. Alle en tegelijkertijd de kosten aan het CPU licht tijdens runtime.

## <a name="what-exactly-happens-during-baking"></a>Wat er precies gebeurt tijdens het 'bakken'?

Het systeem rekening gehouden met potentiële player locaties voor het genereren van een set met gelijke tussenruimte posities van 'Test'-voorbeeld. Er bestaat een bake voor een niveau van onafhankelijke taken voor elke test: Het systeem wordt rekening gehouden met een cuboid "Simulatie regio" is gericht op de test en biedt een gedetailleerde wave simulatie in die regio resolutie van maximaal 25 cm.

## <a name="next-steps"></a>Volgende stappen
* Verken de [voorbeeld scène](sample-walkthrough.md)

