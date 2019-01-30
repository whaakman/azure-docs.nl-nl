---
title: Wat is een woordenlijst? -Aangepaste Translator
titleSuffix: Azure Cognitive Services
description: Een woordenlijst is een uitgelijnde document een lijst van zinnen of zinnen (en de vertalingen) die u wilt dat altijd Microsoft Translator geeft op dezelfde manier te vertalen. Woordenboeken worden soms ook wel glossaria of term bases.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: bc297fd4cdda8cdb3dc618b73e91d2f4e31e8b6a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55212406"
---
# <a name="what-is-a-dictionary"></a>Wat is een woordenlijst?

Een woordenlijst is een uitgelijnde twee documenten die Hiermee geeft u een lijst van zinnen of zinnen en de bijbehorende vertalingen. Gebruik een woordenlijst in uw training, als u wilt dat Microsoft Translator altijd alle exemplaren van de bron woordgroep of zin vertalen met behulp van de vertaling die u hebt opgegeven in de woordenlijst. Woordenboeken worden soms glossaria of term bases genoemd. U kunt de woordenlijst zien als een brute force 'kopiëren en vervang' voor alle bepalingen in de lijst.

Woordenboeken kan uitsluitend worden gebruikt voor projecten in de taal-paren die beschikken over een volledig ondersteunde Microsoft neurale machinevertalingen (NMT) achter deze. Zie http://www.aka.ms/translatorlanguages voor een volledige lijst met talen.

## <a name="phrase-dictionary"></a>Woordgroep woordenlijst 
Wanneer u een woordenlijst woordgroep opneemt in uw model te trainen, wordt een woord of zinsdeel vermeld omgezet in de manier waarop die u hebt opgegeven. De rest van de zin wordt zoals gewoonlijk omgezet. U kunt een woordgroep-woordenlijst gebruiken om op te geven die mag niet worden vertaald door te geven van de dezelfde niet-vertaalde zin in de bron en doel-bestand in de woordenlijst.

## <a name="sentence-dictionary"></a>Zin woordenlijst
De woordenlijst zin kunt u een exacte doel vertaling voor een zin bron opgeven. De hele ingediende zin moet overeenkomen met de bron dictionary-vermelding voor een overeenkomst van de woordenlijst zin optreden.  Als er slechts een deel van de zin overeenkomt met, niet overeen met de vermelding is.  Wanneer een overeenkomst wordt gedetecteerd, wordt de vermelding van het doel van de woordenlijst zin worden geretourneerd.

## <a name="dictionary-only-trainings"></a>U kunt alleen-woordenlijst
U kunt een model met behulp van alleen de gegevens van de woordenlijst trainen. Om dit te doen, selecteert u alleen de woordenlijst-document (of meerdere woordenlijst documenten) die u wilt opnemen en tik op model maken. Omdat dit een alleen-woordenlijst training is, is er geen minimum aantal training zinnen vereist. Uw model wordt meestal veel sneller dan een standaard training training voltooid.  De resulterende modellen gebruikt de Microsoft baseline modellen voor vertaling met de toevoeging van de woordenlijsten die u hebt toegevoegd.  U krijgt een controlelijst.

>[!Note]
>Aangepaste Translator heeft geen zin uitlijnen woordenlijstbestanden, het is dus belangrijk dat er een gelijk aantal bron- en zinnen zijn / zinnen in de woordenlijst documenten en dat ze exact zijn uitgelijnd.

## <a name="recommendations"></a>Aanbevelingen

- Woordenboeken zijn geen vervanging voor een getraind model met trainingsgegevens.  Woordenboeken wordt in feite zoeken en vervangen woorden of zinnen.  Laat het systeem meer uit uw trainingsmateriaal in volledige zinnen is doorgaans een betere keuze dan het gebruik van een woordenlijst. 
- De woordgroep woordenlijst moet spaarzaam worden gebruikt. Als een woord in een zin wordt vervangen, wordt de context in die zin verloren gaan of die beperkt zijn voor het omzetten van de rest van de zin. Het resultaat is dat terwijl de woordgroep of woord in de zin op basis van de woordenlijst woordgroep wordt vertaald, vaak heeft nadelige gevolgen voor de kwaliteit van de algehele vertaling van de zin.
- De woordgroep woordenlijst werkt goed voor samenstellingen zoals productnamen ('Microsoft SQL Server'), juiste namen ("plaats van Hamburg") of functies van het product ("Draaitabel'). Werkt niet net zo goed voor termen of bijvoeglijke naamwoorden omdat deze zijn meestal sterk verbogen in de bron of in de doel-taal. Vermijd woordgroep dictionary-vermeldingen voor andere waarde dan samenstellingen. 
- Wanneer u een woordenlijst, weer hoofdletters en leestekens in uw vertalingen hoofdlettergebruik en leestekens die zijn opgegeven in het doelbestand. Gebruik van hoofdletters en leestekens worden genegeerd bij het identificeren van overeenkomsten tussen uw invoer zin en de bron-zinnen in de woordenlijstbestand. Stel dat we een Engels, Spaans systeem waarmee een woordenlijst getraind die opgegeven "plaats Hamburg" in het bronbestand en "Ciudad de hamburg" in het doelbestand. Als ik de vertaling van een zin die de zin "plaats Hamburg" opgenomen aangevraagd, vervolgens "plaats Hamburg" wordt gezocht naar Mijn woordenlijstbestand voor de vermelding "Plaats Hamburg", en zou toewijzen aan "Ciudad de hamburg" in de laatste vertaling.
- Als een woord wordt meer dan één keer in een woordenlijstbestand weergegeven, gebruik het systeem altijd de laatste post opgegeven. De woordenlijst mag niet meerdere vertalingen van hetzelfde woord bevatten.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [richtlijnen voor het documentindelingen](document-formats-naming-convention.md).
