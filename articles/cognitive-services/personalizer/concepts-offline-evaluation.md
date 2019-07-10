---
title: Offline evaluatie - Personalizer
titleSuffix: Azure Cognitive Services
description: Feedback-lus maken in deze C# Quick Start met de Personalizer-service.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 3fdedd1af9b683b221dfa4aebad7a30559b7abff
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722488"
---
# <a name="offline-evaluation"></a>Offline-evaluatie

Offline evaluatie is een methode die u kunt testen en de effectiviteit van de Service Personalizer evalueren zonder te wijzigen van uw code of die betrekking hebben op de gebruikerservaring. Offline evaluatie maakt gebruik van verleden gegevens verzonden van uw toepassing om de positie-API, om te vergelijken hoe verschillende posities hebt uitgevoerd.

Offline evaluatie wordt uitgevoerd op een bepaalde periode. Het bereik te als laat de huidige tijd voltooien. Het begin van het bereik mag niet langer dan het aantal dagen dat is opgegeven voor [Gegevensretentie](how-to-settings.md).

Offline evaluatie kunt u de volgende vragen beantwoorden:

* Hoe effectief worden Personalizer posities voor geslaagde persoonlijke instellingen?
    * Wat zijn de gemiddelde beloningen bereikt door de Personalizer online machine learning-beleid?
    * Hoe verhoudt Personalizer zich aan de effectiviteit van wat de toepassing zou hebben gedaan standaard?
    * Wat hem de vergelijkende effectiviteit van een willekeurige keuze voor persoonlijke instellingen?
    * Wat hem de vergelijkende effectiviteit van verschillende learning beleid handmatig opgegeven?
* Welke functies van de context meer of minder bijdraagt aan geslaagde persoonlijke instellingen?
* Welke functies van de acties meer of minder bijdraagt aan geslaagde persoonlijke instellingen?

Bovendien kan Offline evaluatie worden gebruikt voor het detecteren van meer geoptimaliseerd leren werken met beleidsregels die Personalizer gebruiken kunt om de resultaten in de toekomst te verbeteren.

Offline evaluaties bieden richtlijnen over het percentage van de gebeurtenissen die moeten worden gebruikt voor verkenning.

## <a name="prerequisites-for-offline-evaluation"></a>Vereisten voor de offline-evaluatie

De volgende zijn belangrijke overwegingen voor de evaluatie van de representatieve offline:

* Onvoldoende gegevens hebben. Het aanbevolen minimum is ten minste 50.000 gebeurtenissen.
* Gegevens verzamelen uit perioden met representatieve gebruikersgedrag en verkeer.

## <a name="discovering-the-optimized-learning-policy"></a>Het beleid voor geoptimaliseerde learning detecteren

Personalizer kunt u de offline evaluatieproces gebruiken voor het automatisch detecteren van een meer optimale learning-beleid.

Na het uitvoeren van de offline evaluatie, ziet u de vergelijkende effectiviteit van Personalizer met dit nieuwe beleid vergeleken met het huidige online-beleid. U kunt vervolgens dat beleid learning zodat het in Personalizer onmiddellijk van kracht toepassen of downloaden voor toekomstig gebruik of het gebruik.

## <a name="understanding-the-relevance-of-offline-evaluation-results"></a>Informatie over de relevantie van offline evaluatieresultaten

Wanneer u een offline evaluatie uitvoert, is het belangrijk om te analyseren _vertrouwen grenzen_ van de resultaten. Als ze breed zijn, betekent dat uw toepassing is niet voldoende gegevens voor de schattingen beloning nauwkeurige of aanzienlijke ontvangen. Als het systeem meer gegevens verzameld en u offline evaluaties gedurende langere tijd worden uitgevoerd, worden de intervallen vertrouwen smaller.

## <a name="how-offline-evaluations-are-done"></a>Hoe offline evaluaties worden uitgevoerd

Offline evaluaties worden gedaan met behulp van een methode met de naam **Counterfactual evaluatie**. 

Personalizer is gebouwd op de veronderstelling dat gebruikers gedrag (en dus beloningen) zijn niet mogelijk om te voorspellen retroactief (Personalizer niet weten wat zou hebben als de gebruiker had is aangetoond dat er iets anders zijn dan wat er gebeurd), en alleen voor de informatie uit gemeten voordelen. 

Dit is het algemene proces gebruikt voor de evaluatieversies:

```
[For a given _learning policy), such as the online learning policy, uploaded learning policies, or optimized candidate policies]:
{
    Initialize a virtual instance of Personalizer with that policy and a blank model;

    [For every chronological event in the logs]
    {
        - Perform a Rank call
    
        - Compare the reward of the results against the logged user behavior.
            - If they match, train the model on the observed reward in the logs.
            - If they don't match, then what the user would have done is unknown, so the event is discarded and not used for training or measurement.
        
    }

    Add up the rewards and statistics that were predicted, do some aggregation to aid visualizations, and save the results.
}
```

De offline evaluatie maakt alleen gebruik van waargenomen gebruikersgedrag. Dit proces wordt verwijderd van grote hoeveelheden gegevens, met name als uw toepassing aanroepen met een groot aantal acties gerangschikt.


## <a name="evaluation-of-features"></a>Evaluatie van functies

Offline evaluaties krijgt u informatie over welk deel van de specifieke functies voor acties of context worden wegen voor hogere beloningen. De informatie wordt berekend op basis van de evaluatie op basis van de ingestelde periode en de gegevens en kan variëren met de tijd.

U wordt aangeraden functie evaluaties kijken en waarin wordt gevraagd:

* Welke functies andere, aanvullende, kan uw toepassing of het systeem biedt langs de regels die effectiever zijn?
* Welke functies kan worden verwijderd vanwege onvoldoende effectiviteit? Lage effectiviteit functies toevoegen _ruis_ in de machine learning.
* Zijn er geen functies die per ongeluk opgenomen zijn? Voorbeelden hiervan zijn: persoonlijk identificeerbare informatie (PII), dubbele id's, enzovoort.
* Zijn er geen ongewenste functies die al dan niet mogen worden gebruikt voor het aanpassen vanwege regelgeving of verantwoordelijk overwegingen gebruiken? Zijn er functies die proxy kunnen (dat wil zeggen, nauw spiegelen of correleren met) ongewenste functies?


## <a name="next-steps"></a>Volgende stappen

[Personalizer configureren](how-to-settings.md)
