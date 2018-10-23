---
title: Understanding intents in LUIS-apps
titleSuffix: Azure Cognitive Services
description: Een doel vertegenwoordigt een taak of actie de gebruiker wil om uit te voeren. Het is een doel of het doel, uitgedrukt in van een gebruiker utterance. Een set intents die overeenkomt met voor acties die gebruikers te nemen in uw toepassing wilt definiëren.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 5ccfe781b3632bd7ccfc532398a00faf7a87b63f
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637208"
---
# <a name="intents-in-luis"></a>Intents in LUIS

Een doel vertegenwoordigt een taak of actie de gebruiker wil om uit te voeren. Het is een doel of het doel, uitgedrukt in van een gebruiker [utterance](luis-concept-utterance.md).

Een set intents die overeenkomt met voor acties die gebruikers te nemen in uw toepassing wilt definiëren. Bijvoorbeeld, definieert een reis-app verschillende intents:

App-intents reizen   |   Voorbeelden van utterances   | 
------|------|
 BookFlight     |   "Book me een vlucht Rio volgende week" <br/> "Vliegt me naar Rio op de 24th" <br/> "Ik moet een ticket vlak volgende zondag naar Rio de Janeiro"    |
 Begroeting     |   'Hallo' <br/>"Hallo" <br/>"Goedemorgen"  |
 CheckWeather | "Wat is het weer, zoals in Boston?" <br/> 'Toon de prognose voor dit weekend' |
 Geen         | 'Ophalen me een cookie recept'<br>"De Lakers te winnen?" |

Alle toepassingen die worden geleverd met de vooraf gedefinieerde intentie "[geen](#none-intent-is-fallback-for-app)" Dit is de terugval intentie. 

## <a name="prebuilt-domains-provide-intents"></a>Vooraf gemaakte domeinen bieden intents
Naast intents die u definieert, kunt u vooraf gedefinieerde intents uit een van de vooraf gemaakte domeinen gebruiken. Zie voor meer informatie, [vooraf gemaakte domeinen in LUIS-apps gebruiken](luis-how-to-use-prebuilt-domains.md) voor meer informatie over het aanpassen van de intenties van een vooraf gedefinieerde domein voor gebruik in uw app.

## <a name="return-all-intents-scores"></a>Retourneren van alle intents scores
U toewijzen een utterance aan een één doel. Wanneer LUIS een utterance op het eindpunt ontvangt, wordt het een belangrijkste doel voor deze utterance. Als u scores voor alle intents voor de utterance wilt, kunt u opgeven `verbose=true` vlag aan de query-tekenreeks van de API [eindpunt aanroep](https://aka.ms/v1-endpoint-api-docs). 

## <a name="intent-compared-to-entity"></a>Kunt u lezen wat ten opzichte van entiteit
De bedoeling vertegenwoordigt actie de chatbot kunt uitvoeren voor de gebruiker en zijn gebaseerd op de hele utterance. De entiteit vertegenwoordigt woorden of zinsdelen die zich in de utterance. Een utterance kan slechts één boven scoren doel hebben, maar het kan veel entiteiten hebben. 

<a name="how-do-intents-relate-to-entities"></a> Maken van een doel als van de gebruiker _voornemen_ zou een actie in een clienttoepassing, zoals een aanroep naar de functie checkweather() activeren. Vervolgens maakt u een entiteit om weer te geven van de parameters die zijn vereist voor het uitvoeren van de actie. 

|Voorbeeld van doel   | Entiteit | Entiteit in de voorbeeld-uitingen   | 
|------------------|------------------------------|------------------------------|
| CheckWeather | {"type": "locatie", "entiteit": "seattle"}<br>{"type": "builtin.datetimeV2.date","entity": 'morgen', 'Oplossing': "2018-05-23"} | Wat het weer, zoals `Seattle` `tomorrow`? |
| CheckWeather | {"type": "date_range', 'entiteit':"dit weekend"} | De weersvoorspelling voor weergeven `this weekend` | 

## <a name="custom-intents"></a>Aangepaste intents

Op dezelfde manier populariteit [uitingen](luis-concept-utterance.md) komen overeen met een één doel. Uitingen in uw bedoeling kunnen u elk [entiteit](luis-concept-entity-types.md) in de app omdat de entiteiten zijn niet specifiek voor een doel. 

## <a name="prebuilt-domain-intents"></a>Vooraf gedefinieerde domein intents

[Vooraf gemaakte domeinen](luis-how-to-use-prebuilt-domains.md) intents met uitingen hebben.  

## <a name="none-intent-is-fallback-for-app"></a>Geen intentie is terugval voor app
De **geen** bedoeling is de bedoeling van een catch-all of terugval. Het wordt gebruikt om te leren LUIS uitingen die in het app-domein (onderwerpsgebied) niet belangrijk zijn. De **geen** bedoeling moet tussen 10 en 20 procent van de totale uitingen in de toepassing. U mag deze intent niet leeglaten. 

### <a name="none-intent-helps-conversation-direction"></a>Geen intentie helpt conversatie richting
Wanneer een utterance wordt voorspeld omdat het geen intentie en geretourneerd naar de chatbot met die voorspelling de bot kunt meer vragen stellen of geef een menu voor het regelen van de gebruiker kan de geldige opties in de chatbot. 

### <a name="no-utterances-in-none-intent-skews-predictions"></a>Er is geen uitingen geen intentie Hiermee laat u overhellen voorspellingen
Als u niet alle uitingen van toevoegt de **geen** intentie, LUIS dwingt een utterance die zich buiten het domein in een van de domein-intents. Hiermee wordt de scores voorspelling scheeftrekken door onderwijzen LUIS het verkeerde type voor de utterance. 

### <a name="add-utterances-to-the-none-intent"></a>Utterances toevoegen op de intentie None
De **geen** bedoeling is gemaakt, maar opzet leeg wordt gelaten. Vul het met uitingen die zich buiten uw domein. Een goede utterance voor **geen** is iets volledig buiten de app, evenals de bedrijfstak van de app fungeert. Een reis-app moet bijvoorbeeld niet gebruiken alle uitingen voor **geen** die kan betrekking hebben op reis, zoals facturering, food, hospitality, lading, actieve entertainment-reserveringen. 

Welk type uitingen worden gegeven voor de geen intentie? Beginnen met een bepaald dat uw bot mag niet beantwoord deze 'wat voor soort dinosaur heeft blauw tanden?" Dit is een zeer specifieke vraag ver buiten een reis-app. 

### <a name="none-is-a-required-intent"></a>Geen is een vereiste doel
De **geen** kunt u lezen wat is een vereiste bedoeling en kan niet worden verwijderd of hernoemd.

## <a name="negative-intentions"></a>Negatieve bedoelingen 
Als u wilt bepalen negatief als positief bedoelingen, zoals ' ik **wilt** een auto "en" ik **niet** wilt van een auto ", kunt u twee intents (één positieve en negatieve één) maken en juiste utterances voor toevoegen elke. Of u kunt maken van een enkele kunt u lezen wat en markeren van de twee verschillende positieve en negatieve voorwaarden als een entiteit.  

## <a name="intent-balance"></a>Intentie saldo
De app-intents domein moeten een balans tussen uitingen voor elk doel hebben. Beschikt niet over een doel met 10 uitingen en een ander doel met 500 uitingen. Dit is niet met gelijke taakverdeling. Als u deze situatie hebt, raadpleegt u de bedoeling van 500 uitingen om te zien als veel van de intenties kunnen opnieuw worden gerangschikt in een [patroon](luis-concept-patterns.md). 

De **geen** doel is niet opgenomen in het saldo. Dit doel moet 10% van de totale uitingen in de app bevatten.

## <a name="intent-limits"></a>Intentie limieten
Beoordeling [limieten](luis-boundaries.md#model-boundaries) om te begrijpen hoeveel intents kunt u toevoegen aan een model. 

### <a name="if-you-need-more-than-the-maximum-number-of-intents"></a>Als u meer dan het maximum aantal intents nodig hebt 
Ten eerste kunt u overwegen of uw systeem is te veel intents. 

### <a name="can-multiple-intents-be-combined-into-single-intent-with-entities"></a>Meerdere intents kunnen worden gecombineerd in één doel met entiteiten 
Intents die te veel zijn kunnen maken voor LUIS onderscheid maken tussen deze moeilijker. Intents moet voldoende zijn om vast te leggen van de belangrijkste taken die de gebruiker wordt gevraagd, maar ze niet nodig hebben om vast te leggen van elk pad uw code wordt gevarieerd. Bijvoorbeeld, BookFlight en FlightCustomerService mogelijk afzonderlijke intents in een reis-app, maar BookInternationalFlight en BookDomesticFlight te vergelijkbaar zijn. Als uw systeem nodig heeft om deze te onderscheiden, gebruikt u entiteiten of andere logische gebruikt in plaats van een intents. 

### <a name="dispatcher-model"></a>Model van de functie voor berichtverzending
Meer informatie over het combineren van LUIS en QnA maker-apps met de [verzending model](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps). 

### <a name="request-help-for-apps-with-significant-number-of-intents"></a>Help-informatie voor apps met een groot aantal intenties Request
Neem contact op met ondersteuning als het aantal intents te verminderen of uw intenties verdelen in meerdere apps niet voor u werkt. Als uw Azure-abonnement voor ondersteuningsservices bevat, neem dan contact op met [technische ondersteuning van Azure](https://azure.microsoft.com/support/options/). 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [entiteiten](luis-concept-entity-types.md), die zijn belangrijk woorden die relevant zijn voor intents
* Meer informatie over het [toevoegen en beheren van intents](luis-how-to-add-intents.md) in uw LUIS-app.
* Controleren van de intentie [aanbevolen procedures](luis-concept-best-practices.md)