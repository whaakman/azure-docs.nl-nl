---
title: Hoe om te leren met Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Leer hoe u om te leren met Conversatiecursist.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 8c55bb27ce5a413c5ceae22371ad61a5acf47281
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55220260"
---
# <a name="how-to-teach-with-conversation-learner"></a>Lesgeven met Conversatiecursist 

Dit document wordt uitgelegd wat signalen Conversatiecursist op de hoogte van, en wordt beschreven hoe het aanleren.  

Docenten kan worden onderverdeeld in twee afzonderlijke stappen: entiteiten extraheren en actie selecteren.

## <a name="entity-extraction"></a>Entiteiten extraheren

Op de achtergrond Conversatiecursist gebruikt [LUIS](https://www.luis.ai) voor het ophalen van de entiteit.  Als u bekend bent met LUIS, die zich voordoen is van toepassing op entiteiten extraheren in Conversatiecursist.

De entiteit extractie modellen zijn op de hoogte van de *inhoud* en *context* binnen een utterance gebruiker.  Bijvoorbeeld, als het woord "Seattle" is aangeduid als een plaats in één utterance zoals "Wat is het weer in Seattle", entiteiten extraheren is in staat dat dezelfde inhoud ("Seattle") herkennen als een plaats in een andere utterance zoals "Populatie van Seattle", zelfs als de uitingen zijn heel anders.  Daarentegen als "Francis" is erkend als een naam in ' Een vergadering plannen met Francis', en vervolgens een nieuwe naam van de eerder niet waren herkend kan worden herkend in een vergelijkbaar context, zoals 'Set up een vergadering met Robin'.  Machine learning bepaalt welke bij te wonen op inhoud, context of beide, op basis van voorbeelden van training.

Entiteiten extraheren is op dit moment alleen op de hoogte van de inhoud van de huidige utterance.  In tegenstelling tot de actieselectie is (Zie hieronder) is het niet op de hoogte van de dialoogvenstergeschiedenis van het zoals vorige systeem draait, vorige gebruiker schakelt of eerder herkende entiteiten.  Als gevolg hiervan is het gedrag van entiteiten extraheren 'gedeeld' tussen alle uitingen.  Bijvoorbeeld als de gebruiker utterance 'Ik wil Apple' 'Apple is' als entiteitstype "Fruit" in één gebruiker utterance, wordt het model van de extractie entiteit verwacht dat deze utterance ("Ik wil Apple') 'Apple' met het label"Fruit"altijd mag hebben.

Als de extractie van de entiteit is niet gedraagt zoals verwacht, moet u dit mogelijke oplossingen zijn:

- Allereerst moet proberen is het toevoegen van meer voorbeelden van training, met name voorbeelden die laten zien van de context van typische entiteit (rond woorden) of uitzonderingen
- Houd rekening met de eigenschap van een 'Verwacht een entiteit' toe te voegen aan een actie, indien van toepassing.  Zie de zelfstudie voor entiteiten verwacht voor meer informatie.
- Het is mogelijk om toe te voegen handmatige verwerking naar `EntityExtractionCallback` om uit te pakken entiteiten met behulp van code, dit de minste aanbevolen aanpak is omdat deze niet van verbeteringen profiteren in machine learning als uw systeem meer vormt krijgt.

## <a name="action-selection"></a>Actie selecteren

Actieselectie maakt gebruik van een terugkerende neural network, waarbij alle Gesprekgeschiedenis als invoer.  Actie selecteren is dus een stateful processen die op de hoogte van de vorige gebruiker uitingen entiteitswaarden en systeem uitingen.  

Sommige signalen voorkeur op een natuurlijke manier van het leerproces.  Met andere woorden, als Conversatiecursist kan een actie selecteren besluit met behulp van meer "voorkeur" signalen uitleg, wordt; Als dit niet, wordt er minder "voorkeur" signalen gebruiken.

Hier volgt een tabel met alle signalen in Conversatiecursist, en welke Apps worden gebruikt door de actie selecteren.  Merk op dat de volgorde van word in uitingen van de gebruiker wordt genegeerd.

Signaal | Voorkeur (1 = voorkeur) | Opmerkingen
--- | --- | --- 
Systeemactie in een vorige inschakelen | 1 | 
Entiteiten die aanwezig zijn in de huidige inschakelen | 1 | 
Of dit de eerste inschakelen is | 1 |
Exacte overeenkomst van woorden in de huidige gebruiker utterance | 2 | 
Net als betekenis woorden in de huidige gebruiker utterance | 3 | 
Systeemacties voorafgaand aan de vorige inschakelen | 4 |
Entiteiten die aanwezig zijn op zijn beurt voorafgaand aan de huidige inschakelen | 4 | 
Uitingen voorafgaand aan de huidige gebruiker inschakelen | 5 | 

> [!NOTE]
> Actieselectie neemt de inhoud van het systeemacties--de tekst, inhoud van de kaart, of API-naam of gedrag--alleen de identiteit van de systeemactie.  Als gevolg hiervan niet wijzigen van de inhoud van een actie wordt het gedrag van het model van de selectie actie niet wijzigen.
>
> Bovendien wordt de inhoud/waarden van entiteiten zijn niet gebruikt--alleen hun aanwezigheid/afwezigheid.

Als de actie selecteren is niet gedraagt zoals verwacht, moet u dit mogelijke oplossingen zijn:

- Voeg meer train dialoogvensters, met name de dialoogvensters die laten zien welke signalen actieselectie moet de waarbij aandacht wordt besteed aan.  Als actieselectie moet één signaal liever via een andere, geeft bijvoorbeeld voorbeelden die laten zien van de gewenste signaal dat deze zich in dezelfde staat en de andere signalen variëren.  Aantal reeksen kunnen duren voordat een handvol training dialoogvensters voor meer informatie.
- 'Vereist' en 'diskwalificeren"entiteiten met definities van de actie toevoegen.  Deze limieten wanneer acties beschikbaar zijn en kunnen nuttig zijn voor snelle bedrijfsregels en sommige gezond verstand patronen. 

## <a name="updates-to-models"></a>Updates voor modellen

Elk gewenst moment toevoegen of bewerken van een entiteit, een actie of een dialoogvenster van de trein in de gebruikersinterface, Hiermee wordt een aanvraag voor het opnieuw trainen van zowel de entiteit extractie-model en het model van de selectie actie gegenereerd.  Deze aanvraag in een wachtrij wordt geplaatst en opnieuw trainen van asynchroon wordt uitgevoerd.  Wanneer een nieuw model beschikbaar is, wordt deze of hoger vanaf dat moment gebruikt voor de selectie van de extractie en actie van de entiteit.  Dit proces opnieuw training vaak duurt ongeveer 5 seconden, maar kan niet langer als het model complex is of als de belasting van de trainingsservice is hoog.

Omdat training asynchroon wordt uitgevoerd, is het mogelijk dat u hebt wijzigingen direct worden niet doorgevoerd.  Als er sleuteltermextractie plaats of actie selecteren van een entiteit niet gedraagt is zoals verwacht op basis van wijzigingen die u hebt aangebracht in de afgelopen 5-10 seconden, kan dit de oorzaak.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Standaardwaarden en grenzen](./cl-values-and-boundaries.md)
