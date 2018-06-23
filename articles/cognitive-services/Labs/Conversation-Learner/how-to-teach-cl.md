---
title: Hoe om te leren met conversatie cursist - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het leren gebruiken met de conversatie cursist.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 639fea64fc8eeb2c1f6e6240c4eb26efc68febbd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345270"
---
# <a name="how-to-teach-with-conversation-learner"></a>Hoe u met de conversatie cursist leren 

Dit document wordt uitgelegd wat signalen conversatie cursist hoogte is van en beschrijft hoe het leert.  

Onderwijs kan worden onderverdeeld in twee afzonderlijke stappen: uitpakken van de entiteit en actie selecteren.

## <a name="entity-extraction"></a>Uitpakken van de entiteit

Achter de conversatie cursist gebruikt [LUIS](https://www.luis.ai) voor het ophalen van de entiteit.  Als u bekend bent met LUIS die ondervinden geldt voor de extractie entiteit in de conversatie cursist.

De entiteit extractie modellen zich bewust bent van de *inhoud* en *context* binnen een utterance gebruiker.  Bijvoorbeeld, als het woord "Seattle" zijn aangeduid als een plaats in één utterance zoals 'Wat is het weer in Haarlem', uitpakken van de entiteit is geschikt voor het herkennen van dezelfde inhoud ("Seattle") als een plaats in een andere utterance zoals "Populatie van Seattle", zelfs als de utterances zijn heel verschillend.  Als u daarentegen als 'Francis' heeft zijn herkend als een naam in ' Een vergadering plannen met Francis', en vervolgens een nieuwe naam van de eerder verborgen in een vergelijkbare context, zoals 'Instellen een vergadering met Robin' kan worden herkend.  Machine learning afleidt wanneer aandacht aan de inhoud en/of de context op basis van training voorbeelden.

Uitpakken van de entiteit is momenteel alleen op de hoogte van de inhoud van de huidige utterance.  In tegenstelling tot een actie selecteren (Zie hieronder) is het niet op de hoogte van de dialoogvenstergeschiedenis van het zoals vorige systeem draait, vorige gebruiker schakelt of eerder herkende entiteiten.  Als gevolg hiervan is het gedrag van het uitpakken van de entiteit 'gedeeld' tussen alle utterances.  Bijvoorbeeld als de gebruiker utterance "Ik wil Apple" "Apple" gelabeld als entiteitstype "Fruit" in één gebruiker utterance heeft, wordt de extractie entiteitsmodel verwacht dat deze utterance ("Ik wil dat Apple") "Apple" aangeduid als "Fruit" altijd moet hebben.

Als het uitpakken van de entiteit is niet zoals verwacht, moet u hier mogelijke oplossingen zijn:

- Het eerste wat om te proberen is het toevoegen van meer training voorbeelden--met name de voorbeelden die onthullen typische entiteit context (omringende woorden) of uitzonderingen
- Overweeg een 'Verwacht een entiteit'-eigenschap op een gebruikersactie, indien van toepassing.  Zie de zelfstudie over entiteiten verwacht voor meer informatie.
- Het is mogelijk om toe te voegen handmatige verwerking naar `EntityExtractionCallback` om uit te pakken entiteiten met code, dit de minste aanbevolen aanpak is omdat deze niet van verbeteringen profiteren in machine learning naarmate uw systeem meer vormt krijgt.

## <a name="action-selection"></a>Actie selecteren

Actie selecteren maakt gebruik van een terugkerende neurale netwerk, waarbij als invoer alle van de geschiedenis van de conversatie wordt.  Actie selecteren is dus een stateful proces dat op de hoogte van de vorige gebruiker utterances, waarden entiteit en system utterances is.  

Sommige signalen zijn natuurlijk voorkeur van het leerproces.  Met andere woorden, als een actie selectie besluit via meer 'aanbevolen' signalen conversatie cursist uitgelegd, wordt; Als dit niet, wordt deze minder 'aanbevolen' signalen gebruiken.

Hier volgt een tabel met alle signalen in conversatie cursist en welke worden gebruikt door een actie selecteren.  Let op: word-volgorde in gebruiker utterances wordt genegeerd.

Signaal | Voorkeur (1 = voorkeur) | Opmerkingen
--- | --- | --- 
Systeem-bewerking in de vorige inschakelen | 1 | 
Entiteiten aanwezig zijn op de huidige inschakelen | 1 | 
Of dit de eerste inschakelen is | 1 |
Exacte overeenkomst met woorden in de huidige gebruiker utterance | 2 | 
Vergelijkbaar betekenis woorden in de huidige gebruiker utterance | 3 | 
Systeemacties voorafgaand aan de vorige inschakelen | 4 |
Entiteiten presenteren op zijn beurt vóór de huidige inschakelen | 4 | 
Utterances vóór de huidige gebruiker inschakelen | 5 | 

Duren actie selecteren de inhoud van het systeemacties--de tekst, inhoud van de kaart, of API-naam of gedrag--alleen de identiteit van de systeemactie.  Als gevolg hiervan niet wijzigen van de inhoud van een actie wordt het gedrag van het model van de selectie actie niet wijzigen.

Bovendien wordt de inhoud en-waarden van entiteiten zijn niet gebruikt--alleen hun aanwezigheid/afwezigheid.

Als de selectie van de actie niet zoals verwacht, moet u hier mogelijke oplossingen zijn:

- Voeg meer train dialoogvensters, met name de dialoogvensters die aangeven welke actie selecteren moet met aandacht voor signalen.  Bijvoorbeeld als actie selecteren moet één signaal liever op een andere, geven voorbeelden van het gewenste signaal wordt in dezelfde staat, en de andere signalen variëren.  Sommige reeksen kunnen duren voordat een handvol training dialoogvensters voor meer informatie.
- 'Vereist' en 'Tot uitsluiting' entiteiten in de definities van de actie toevoegen.  Deze limieten wanneer acties beschikbaar zijn en kunnen nuttig zijn voor snelle bedrijfsregels en sommige gezond verstand patronen. 

## <a name="updates-to-models"></a>Updates van modellen

Elk gewenst moment u toevoegen of bewerken van een entiteit, actie of train dialoogvenster in de gebruikersinterface, hierdoor wordt een aanvraag voor het opnieuw trainen zowel de extractie entiteitsmodel en het model van de selectie actie gegenereerd.  Deze aanvraag is een wachtrij geplaatst en opnieuw trainen asynchroon wordt uitgevoerd.  Wanneer een nieuw model beschikbaar is, wordt deze en hoger vanaf dat moment gebruikt voor de selectie van de extractie en actie van de entiteit.  Dit proces opnieuw training vaak duurt ongeveer 5 seconden, maar kan niet langer als het model is complexe of als de belasting van de service training is hoog.

Omdat training asynchroon wordt gedaan, is het mogelijk dat de bewerkingen die u hebt aangebracht onmiddellijk worden niet weergegeven.  Als de extractie of actie selectie entiteit niet zoals verwacht op basis van wijzigingen die u hebt aangebracht in de laatste 5-10 seconden, is dit mogelijk de oorzaak.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Standaardwaarden en grenzen](./cl-values-and-boundaries.md)
