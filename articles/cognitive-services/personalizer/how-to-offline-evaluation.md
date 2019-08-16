---
title: Offline-evaluatie-persoonlijker
titleSuffix: Azure Cognitive Services
description: Meer informatie over het analyseren van uw leer proces met een offline-evaluatie
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: a8a75601daf36ca21ea56a5930219d7d467f0c85
ms.sourcegitcommit: a6888fba33fc20cc6a850e436f8f1d300d03771f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69557820"
---
# <a name="how-to-analyze-your-learning-loop-with-an-offline-evaluation"></a>Uw leer proces analyseren met een offline-evaluatie


Meer informatie over het volt ooien van een offline-evaluatie en het begrijpen van de resultaten.

Met offline-evaluaties kunt u meten hoe effectief Personaler wordt vergeleken met het standaard gedrag van uw toepassing, hoe functies het meest aan persoonlijke voor keur hebben en nieuwe machine learning-instellingen automatisch detecteren.

Lees meer over [offline](concepts-offline-evaluation.md) -evaluaties voor meer informatie.


## <a name="prerequisites"></a>Vereisten

1. U moet een aangepaste lus hebben geconfigureerd
1. De Personaler-lus moet beschikken over een representatieve hoeveelheid gegevens-als een indicatieve-recommmend ten minste 50.000 gebeurtenissen in de logboeken voor zinvolle evaluatie resultaten.

U kunt eventueel ook eerder de _Learning-beleids_ bestanden hebben geëxporteerd die u in dezelfde evaluatie wilt vergelijken en testen.

## <a name="steps-to-start-a-new-offline-evaluation"></a>Stappen voor het starten van een nieuwe offline-evaluatie

1. Zoek uw resource voor de personalisatie-lus op in het Azure Portal.
1. Ga naar de sectie ' evaluatie '.
1. Klik op nieuwe evaluatie
1. Selecteer een begin-en eind datum voor de offline-evaluatie. Dit zijn datums in het verleden die het gegevens bereik opgeven dat in de evaluatie moet worden gebruikt. Deze gegevens moeten aanwezig zijn in de logboeken, zoals opgegeven in de instelling voor het [bewaren van gegevens](how-to-settings.md) .
1. U kunt desgewenst uw eigen leer beleid uploaden. 
1. Geef op of personalisatie een geoptimaliseerd leer beleid moet maken op basis van het gedrag van de gebruiker in deze tijds periode.
1. De evaluatie versie starten

## <a name="results"></a>Resultaten

Het uitvoeren van evaluaties kan veel tijd in beslag nemen, afhankelijk van de hoeveelheid gegevens die moet worden verwerkt, het aantal trainings beleid dat moet worden vergeleken en of er een optimalisatie is aangevraagd.

Zodra het is voltooid, kunt u de volgende resultaten zien:

1. Vergelijking van het leer beleid, waaronder:
    * **Online beleid**: Het huidige leer beleid dat wordt gebruikt in Personaler
    * **Basis lijn**: De standaard waarde van de toepassing (zoals bepaald door de eerste actie die in Rangings aanroepen wordt verzonden),
    * **Wille keurig beleid**: Een imaginair positie gedrag dat altijd een wille keurige keuze van de acties retourneert uit de opgegeven records.
    * **Aangepast beleid**: Er zijn extra leer beleid geüpload bij het starten van de evaluatie.
    * **Geoptimaliseerd beleid**: Als de evaluatie is gestart met de optie om een geoptimaliseerd beleid te detecteren, wordt het ook vergeleken en kunt u het downloaden of het online leer beleid maken, waarbij de huidige wordt vervangen.

1. Effectiviteit van [functies](concepts-features.md) voor acties en context.


## <a name="more-information"></a>Meer informatie

* Meer informatie [over het werken met offline](concepts-offline-evaluation.md)-evaluaties.
