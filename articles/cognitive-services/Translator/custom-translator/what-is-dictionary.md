---
title: Wat is een woorden lijst? -Aangepaste Translator
titleSuffix: Azure Cognitive Services
description: Een woorden lijst is een uitgelijnd document waarin een lijst met zinsdelen of zinnen (en de bijbehorende vertalingen) wordt opgegeven die u altijd wilt laten vertalen met micro soft Translator. Woorden lijsten worden soms ook wel Glossaries of term bases genoemd.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 5103526956b5041771a1d8e4abb5e8800b971059
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68595385"
---
# <a name="what-is-a-dictionary"></a>Wat is een woorden lijst?

Een woorden lijst is een uitgelijnd paar documenten waarin een lijst met zinsdelen of zinnen en de bijbehorende vertalingen wordt opgegeven. Gebruik een woorden lijst in uw training als u wilt dat micro soft Translator altijd alle exemplaren van de bron woord groep of zin moet vertalen met behulp van de vertaling die u in de woorden lijst hebt gegeven. Woorden lijsten worden soms Glossaries of term bases genoemd. U kunt de woorden lijst beschouwen als een brute kracht ' kopiëren en vervangen ' voor alle termen die u vermeldt.

Woorden boeken werken alleen voor projecten in taal paren met een volledig ondersteund NMT-systeem (micro soft Neural Machine Translation) achter de computers. [Bekijk de volledige lijst met talen](https://docs.microsoft.com/azure/cognitive-services/translator/language-support#customization).

## <a name="phrase-dictionary"></a>Woordgroepen lijst
Wanneer u een woordgroepen lijst in uw model hebt opgenomen, wordt een wille keurig woord of zinsdeel omgezet in de manier die u hebt opgegeven. De rest van de zin wordt op de gebruikelijke manier vertaald. U kunt een woordgroepen woordenlijst gebruiken om woord groepen op te geven die niet moeten worden vertaald door dezelfde niet-vertaalde woord groep te bieden in het bron-en doel bestand in de woorden lijst.

## <a name="sentence-dictionary"></a>Woorden lijst voor zinnen
Met de woorden lijst voor zinnen kunt u een exacte doel omzetting voor een bron zin opgeven. Voor een zin die overeenkomt met de woorden lijst, moet de gehele verzonden zin overeenkomen met de vermelding van de bron woordenlijst.  Als slechts een deel van de zin overeenkomt, komt de invoer niet overeen.  Wanneer een overeenkomst wordt gedetecteerd, wordt de doel vermelding van de woorden lijst geretourneerd.

## <a name="dictionary-only-trainings"></a>Alleen-woordenlijst trainingen
U kunt een model trainen met alleen woordenlijst gegevens. Als u dit wilt doen, selecteert u alleen het woordenlijst document (of meerdere woordenlijst documenten) die u wilt opnemen en tikt u op model maken. Aangezien dit een alleen-woordenlijst training is, is er geen minimum aantal trainings zinnen vereist. Het model voert doorgaans veel sneller training uit dan een standaard training.  De resulterende modellen gebruiken de micro soft Baseline-modellen voor vertaling, waarbij de toegevoegde woorden lijsten worden toegevoegd.  Het is niet mogelijk om een test rapport op te halen.

>[!Note]
>Aangepaste vertaler heeft geen zin om woordenlijst bestanden uit te lijnen. het is dus belang rijk dat er een gelijk aantal bron-en doel zinnen/zinnen in uw woordenlijst documenten staat en dat ze nauw keurig zijn uitgelijnd.

## <a name="recommendations"></a>Aanbevelingen

- Woorden boeken vormen geen vervanging voor een getraind model met trainings gegevens.  Woorden boeken en vervangen woorden of zinnen in wezen.  Het systeem leren van uw trainings materiaal in volledige zinnen is over het algemeen een betere keuze dan het gebruik van een woorden lijst.
- De woorden lijst woordgroepen moet spaarzaam worden gebruikt. Wanneer een woord groep in een zin wordt vervangen, gaat de context in die zin verloren of is deze beperkt voor het vertalen van de rest van de zin. Het resultaat is dat als de woord groep of het woord in de zin volgens de woorden lijst van de woord groep wordt vertaald, de algehele Vertaal kwaliteit van de zin vaak in orde is.
- De woorden lijst woordgroepen werkt goed voor samengestelde naam woorden, zoals product namen ("Microsoft SQL Server"), juiste namen ("City of Hamburg") of functies van het product ("Pivot Table"). Deze werkt niet gelijk aan woorden of bijvoegingen, omdat deze doorgaans zeer verbuigd zijn in de bron of in de doel taal. Vermijd vermeldingen van woordgroepen woordenlijst voor alle, maar samengestelde samen stellingen.
- Wanneer u een woorden boek gebruikt, wordt het hoofdletter gebruik en interpunctie in uw doel bestand weer gegeven. Hoofdletter gebruik en interpunctie worden genegeerd bij het identificeren van overeenkomsten tussen de invoer zinnen en de bron zinnen in het woordenlijst bestand. Stel dat we een Engels aan het Spaanse systeem hebben getraind dat een woorden lijst gebruikt die ' City of Hamburg ' in het bron bestand heeft opgegeven, en ' Ciudad de Hamburg ' in het doel bestand. Als ik de vertaling van een zin met de zinsnede ' City of Hamburg ' heeft aangevraagd, komt ' City of Hamburg ' overeen met mijn woordenlijst bestand voor de vermelding ' City of Hamburg ' en zou deze worden toegewezen aan ' Ciudad de Hamburg ' in de definitieve vertaling.
- Als een woord meermaals wordt weer gegeven in een woordenlijst bestand, gebruikt het systeem altijd de laatste vermelding die is opgegeven. De woorden lijst mag niet meerdere vertalingen van hetzelfde woord bevatten.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [richt lijnen voor document indelingen](document-formats-naming-convention.md).
