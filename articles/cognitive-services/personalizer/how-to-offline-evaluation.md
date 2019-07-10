---
title: Offline evaluatie - Personalizer
titleSuffix: Azure Cognitive Services
description: Informatie over het analyseren van uw lus learning in een offline-evaluatie
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: b719e6e693471415350007a4f4fabed917b8e12d
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722324"
---
# <a name="how-to-analyze-your-learning-loop-with-an-offline-evaluation"></a>Over het analyseren van uw lus learning in een offline-evaluatie


Leer hoe u een offline-evaluatie voltooid en de resultaten te begrijpen.

Offline evaluaties kunnen u meten hoe effectief Personalizer wordt vergeleken met het standaardgedrag van uw toepassing, informatie over welke functies zijn die bijdragen aan de meeste tot persoonlijke instellingen en ontdek nieuwe machine learning-instellingen automatisch.

Meer informatie over [Offline evaluaties](concepts-offline-evaluation.md) voor meer informatie.


## <a name="prerequisites"></a>Vereisten

1. Hebt u een lus Personalizer is geconfigureerd
1. De lus Personalizer moet ten minste 50.000 gebeurtenissen in de logboeken voor de van betekenisvolle evaluatieresultaten hebben.

(Optioneel) u mogelijk ook eerder geëxporteerd _leren werken met beleid_ bestanden die u kunt vergelijken en testen in de dezelfde evaluatie.

## <a name="steps-to-start-a-new-offline-evaluation"></a>Stappen voor het starten van een nieuwe evaluatie van het Offline

1. Zoek de resource van uw persoonlijke instellingen lus in Azure portal.
1. Ga naar de sectie 'Beoordeling'.
1. Klik op nieuwe evaluatie
1. Selecteer een begin- en datum voor de offline evaluatie. Dit zijn de datums in het verleden, geef het bereik van gegevens voor gebruik in de evaluatie. Deze gegevens moet aanwezig zijn in de logboeken, zoals opgegeven in de [Gegevensretentie](how-to-settings.md) instelling.
1. U kunt desgewenst uw eigen beleid learning uploaden. 
1. Geef op of Personalizer moet worden gebruikt voor het maken van een geoptimaliseerde Learning-beleid op basis van het gedrag van gebruikers in deze periode is waargenomen.
1. De evaluatie starten

## <a name="results"></a>Resultaten

Evaluaties kunnen lang duren om uit te voeren, afhankelijk van de hoeveelheid gegevens te verwerken en te leren werken met beleid om te vergelijken, aantal en of een optimalisatie is aangevraagd.

Als voltooid, ziet u de volgende resultaten:

1. Vergelijkingen van Learning beleid, met inbegrip van:
    * **Online-beleid**: De huidige Learning beleid gebruikt in Personalizer
    * **Basislijn**: Van de toepassing standaard (zoals bepaald door de eerste actie in de rang aanroepen verzonden)
    * **Beleid voor willekeurige**: Een denkbeeldige positie gedrag retourneert die altijd willekeurige keuze van de acties van de opgegeven namen.
    * **Aangepaste beleidsregels**: Extra Learning beleidsregels geüpload bij het starten van de evaluatie.
    * **Beleid geoptimaliseerd**: Als de evaluatie is gestart met de optie voor het detecteren van een geoptimaliseerde beleid, wordt ook worden vergeleken en kunt u zich aan het downloaden of wijzig deze in het beleid voor online leren, vervangen de huidige versie.

1. Effectiviteit van [functies](concepts-features.md) voor acties en Context.


## <a name="more-information"></a>Meer informatie

* Informatie over [hoe offline evaluaties werken](concepts-offline-evaluation.md).