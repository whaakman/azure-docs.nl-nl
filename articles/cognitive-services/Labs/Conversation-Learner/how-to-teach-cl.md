---
title: Leren met Conversation Learner-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het leren van Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: a18d4c31da4ffeefebd4bda9aa441fdfec062be9
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705269"
---
# <a name="how-to-teach-with-conversation-learner"></a>Lesgeven met Conversatiecursist 

In dit document wordt uitgelegd welke signalen Conversation Learner op de hoogte is, en wordt beschreven hoe het leert.  

Onderwijs kan worden opgesplitst in twee afzonderlijke stappen: entiteits extractie en actie selectie.

## <a name="entity-extraction"></a>Entiteits extractie

Onder de kaften Conversation Learner gebruikt [Luis](https://www.luis.ai) voor het uitpakken van de entiteit.  Als u bekend bent met LUIS, is deze ervaring van toepassing op de extractie van entiteiten in Conversation Learner.

De entiteit extractie modellen zijn op de hoogte van de *inhoud* en *context* binnen een gebruiker utterance.  Als bijvoorbeeld het woord ' Seattle ' is gemarkeerd als een stad in een utterance zoals ' wat is het weer in Seattle ', kan de extractie van de entiteit dezelfde inhoud ("Seattle") herkennen als een plaats in een andere utterance, zoals "populatie van Seattle", zelfs als de uitingen zijn zeer verschillend.  Als "Francis" is herkend als een naam in een vergadering plannen met Francis, dan kan een nieuwe eerder geherkende naam worden herkend in een vergelijk bare context, zoals "een vergadering met Robin instellen".  Machine learning maakt deel uit van de gegevens, context of beide, op basis van de trainings voorbeelden.

Op dit moment is het uitpakken van de entiteit alleen op de hoogte van de inhoud van de huidige utterance.  In tegens telling tot actie selectie (hieronder) is het niet op de hoogte van de dialoog geschiedenis, zoals eerdere systeem mogelijkheden, eerdere gebruikers of eerder herkende entiteiten.  Als gevolg hiervan is het gedrag van entiteits extractie "gedeeld" in alle uitingen.  Als de gebruiker bijvoorbeeld utterance ' Ik wil dat Apple ' is aangeduid als entiteits type ' fruit ' in één gebruiker utterance, zal het model voor het uitpakken van de entiteit verwachten dat deze utterance ("Ik wil dat Apple") altijd "Apple" als "fruit" moet hebben.

Als de extractie van entiteiten niet naar behoren werkt, kunt u het volgende doen:

- Het eerste wat u moet proberen om meer trainings voorbeelden toe te voegen: met name voor beelden die typische entiteits context (omringende woorden) of uitzonde ringen tonen
- Overweeg een ' verwachte entiteit ' toe te voegen aan een actie, indien van toepassing.  Zie de zelf studie over verwachte entiteiten voor meer informatie.
- Hoewel het mogelijk is om hand matige verwerking toe `EntityExtractionCallback` te voegen aan om entiteiten te extra heren met behulp van code, is dit de minst aanbevolen benadering omdat deze niet kan profiteren van verbeteringen in machine learning als uw systeem verouderd is.

## <a name="action-selection"></a>Actie selectie

Actie selectie maakt gebruik van een recurrent Neural-netwerk, dat als invoer wordt gebruikt voor alle gesprek geschiedenis.  Actie selectie is dus een stateful proces dat op de hoogte is van de vorige gebruiker uitingen, entiteits waarden en systeem uitingen.  

Sommige signalen worden voor het leer proces als voor keur in het geraden.  Met andere woorden, als Conversation Learner een actie selectie besluit met behulp van ' meer voor keur '-signalen kunt uitleggen, wordt het; Als dat niet het geval is, worden de signalen ' minder voor keur ' gebruikt.

Hier volgt een tabel met alle signalen in Conversation Learner, die worden gebruikt door actie selectie.  Houd er rekening mee dat woord volgorde in de uitingen van de gebruiker wordt genegeerd.

Melden | Voor keur (1 = meest voor keur) | Opmerkingen
--- | --- | --- 
Systeem actie in vorige zet | 1 | 
Entiteiten die momenteel aanwezig zijn in de huidige beurt | 1 | 
Of dit de eerste zet is | 1 |
Exact overeenkomende woorden in de huidige gebruiker utterance | 2 | 
Vergelijk bare woorden in de huidige gebruiker utterance | 3 | 
Systeem acties voorafgaand aan vorige turn | 4 |
Entiteiten aanwezig in beurten vóór huidige beurt | 4 | 
Gebruikers uitingen vóór huidige beurt | 5 | 

> [!NOTE]
> Actie selectie houdt geen rekening met de inhoud van systeem acties: de tekst, de kaart inhoud of de API-naam of het gedrag, alleen de identiteit van de systeem actie.  Als gevolg hiervan is het wijzigen van de inhoud van een actie niet van invloed op het gedrag van het actie selectie model.
>
> De inhoud/waarden van entiteiten worden niet gebruikt, maar alleen hun aanwezigheid/verzuim.

Als de actie selectie niet werkt zoals verwacht, kunt u dit op de volgende manieren doen:

- Voeg meer dialoog vensters voor Train toe, met name dialoog vensters die laten zien welke actie selectie voor signalen moet worden bedacht.  Als de actie selectie bijvoorbeeld de voor keur geeft aan een signaal over een andere, geeft u voor beelden op die het voorkeurs signaal in dezelfde staat weer geven en de andere signalen variëren.  Sommige reeksen kunnen een aantal trainings dialoogvensters hebben om te leren.
- Entiteiten ' vereist ' en ' diskwalificerend ' toevoegen aan actie definities.  Dit is beperkt wanneer er acties beschikbaar zijn en kan nuttig zijn om bedrijfs regels en enkele algemene gevoels patronen te maken. 

## <a name="updates-to-models"></a>Updates voor modellen

Telkens wanneer u een entiteit, actie of trein dialoog venster in de gebruikers interface toevoegt of bewerkt, genereert dit een aanvraag voor het opnieuw trainen van zowel het model voor de extractie van entiteiten als voor het actie selectie model.  Deze aanvraag wordt in een wachtrij geplaatst en opnieuw getraind wordt asynchroon uitgevoerd.  Wanneer een nieuw model beschikbaar is, wordt het vanaf dat punt gebruikt voor het uitpakken van de entiteit en de actie selectie.  Dit proces voor opnieuw trainen duurt vaak ongeveer vijf seconden, maar kan langer zijn als het model complex is of als de belasting van de trainings service hoog is.

Omdat de training asynchroon wordt uitgevoerd, is het mogelijk dat wijzigingen die u hebt aangebracht, niet meteen worden weer gegeven.  Als entiteits extractie of actie selectie niet werkt zoals verwacht, op basis van wijzigingen die u in de afgelopen 5-10 seconden hebt aangebracht, kan dit de oorzaak zijn.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Standaard waarden en grenzen](./cl-values-and-boundaries.md)
