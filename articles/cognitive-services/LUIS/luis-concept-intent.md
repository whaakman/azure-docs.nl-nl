---
title: Understanding intents in LUIS apps in Azure | Microsoft Docs
description: Hierin wordt beschreven wat intents in apps Language Understanding Intelligent Service (LUIS) zijn.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/04/2018
ms.author: v-geberr
ms.openlocfilehash: 11aaa6c0891d7087556a82f3a818ef865869b766
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35350084"
---
# <a name="intents-in-luis"></a>Intents in LUIS

De opzet vertegenwoordigt een taak of actie van de gebruiker wil uitvoeren. Dit is een doel of het doel uitgedrukt in een gebruiker [utterance](luis-concept-utterance.md).

Een set intents die hoort bij acties die gebruikers te nemen in uw toepassing wilt definiëren. Een app reizen definieert bijvoorbeeld verschillende intents:

Reizen app intents   |   Voorbeeld utterances   | 
------|------|
 BookFlight     |   'Het adresboek mij een vlucht Rio volgende week' <br/> 'Vliegen mij Rio op de 24th' <br/> "Ik moet een ticket vlak volgende zondag naar Rio de Janeiro"    |
 Begroeting     |   'Hallo' <br/>'Hallo' <br/>'Goede ochtend'  |
 CheckWeather | 'Wat is het weer zoals in Boston?' <br/> 'Weergeven de prognose voor deze weekend' |
 Geen         | 'Ophalen mij een cookie recept'<br>'De Lakers win?' |

Alle toepassingen worden geleverd met de vooraf gedefinieerde bedoeling '[geen](#none-intent-is-fallback-for-app)' Dit is de bedoeling terugval. 

## <a name="prebuilt-domains-provide-intents"></a>Vooraf gedefinieerde domeinen bieden intents
Naast intents die u definieert, kunt u vooraf gedefinieerde intents uit een van de vooraf gedefinieerde domeinen. Zie voor meer informatie [gebruik van vooraf gedefinieerde domeinen in LUIS apps](luis-how-to-use-prebuilt-domains.md) voor meer informatie over het aanpassen van intents van een vooraf gedefinieerde domein voor gebruik in uw app.

## <a name="return-all-intents-scores"></a>Retourneren van alle intents scores
U kunt een utterance toewijzen aan een één doel. Wanneer LUIS een utterance op het eindpunt ontvangt, wordt het een bovenste doel voor deze utterance. Als u scores voor alle intents voor de utterance wilt, kunt u opgeven `verbose=true` vlag in de queryreeks van de API [eindpunt aanroep](https://aka.ms/v1-endpoint-api-docs). 

## <a name="intent-compared-to-entity"></a>Bedoeling vergeleken met de entiteit
De bedoeling vertegenwoordigt de chatbot moet nemen voor de gebruiker en zijn gebaseerd op de hele utterance in te grijpen. De entiteit vertegenwoordigt woorden of zinnen erin de utterance. Een utterance kan slechts één boven score berekenen voor doel hebben, maar er veel entiteiten. 

<a name="how-do-intents-relate-to-entities"></a> De opzet maken wanneer de gebruiker _voornemen_ een actie in uw clienttoepassing, zoals een aanroep van de functie checkweather() wordt geactiveerd. Vervolgens maakt u een entiteit voor vereiste parameters voor de actie uitvoeren. 

|Voorbeeld van doel   | Entiteit | Entiteit in voorbeeld utterances   | 
|------------------|------------------------------|------------------------------|
| CheckWeather | {{'type': 'locatie', "entiteit": "seattle"}<br>{{'type': 'builtin.datetimeV2.date","entity': 'morgen', 'Oplossing': "2018-05-23"} | Wat het weer, zoals `Seattle` `tomorrow`? |
| CheckWeather | {{'type': 'date_range', 'entiteit': "weekend"} | De prognose voor weergeven `this weekend` | 

## <a name="custom-intents"></a>Aangepaste intents

Op dezelfde manier populariteit [utterances](luis-concept-utterance.md) komen overeen met een één doel. Utterances in uw bedoeling kunnen u elk [entiteit](luis-concept-entity-types.md) in de app omdat entiteiten geen doel-specifieke zijn. 

## <a name="prebuilt-domain-intents"></a>Vooraf gedefinieerde domein intents

[Vooraf gedefinieerde domeinen](luis-how-to-use-prebuilt-domains.md) intents met utterances hebben.  

## <a name="none-intent-is-fallback-for-app"></a>Geen opzet is terugval voor app
De **geen** bedoeling is de bedoeling van een catch all- of terugval. Wordt gebruikt om te leren LUIS utterances die niet belangrijk in het app-domein (onderwerpsgebied). De **geen** bedoeling moet tussen 10 en 20 procent van de totale utterances in de toepassing. Niet het leeg laten. 

### <a name="none-intent-helps-conversation-direction"></a>Geen opzet helpt conversatie richting
Wanneer een utterance wordt voorspeld omdat het geen opzet en geretourneerd naar de chatbot met die voorspelling de bot kunnen meer vragen stellen of geef een menu rechtstreeks door de gebruiker geldig keuzes in de chatbot. 

### <a name="no-utterances-in-none-intent-skews-predictions"></a>Er is geen utterances geen opzet Hiermee laat u overhellen voorspellingen
Als u niet alle utterances voor toevoegt de **geen** opzet, LUIS forceert een utterance die buiten het domein in een van de domein-intents. Hiermee wordt de scores voorspelling een verkeerd door onderwijs LUIS het verkeerde type voor de utterance. 

### <a name="add-utterances-to-the-none-intent"></a>Utterances toevoegen aan de opzet geen
De **geen** bedoeling is gemaakt, maar met opzet leeg gelaten. Opvullen met utterances die zich buiten uw domein. Een goede utterance voor **geen** is iets volledig buiten de app, evenals de branche van de app fungeert. Een app reizen moet bijvoorbeeld niet gebruiken een utterances voor **geen** die kunnen betrekking hebben op reis zoals facturering, voeding, horeca, lading, inflight entertainment-reserveringen. 

Wat voor soort utterances worden gegeven voor de geen opzet? Beginnen met een bepaald uw bot mag niet beantwoord dergelijke 'wat voor soort dinosaur heeft blauw tanden?' Dit is een zeer specifieke vraag ver buiten een app reizen. 

### <a name="none-is-a-required-intent"></a>Geen is een doel vereist
De **geen** doel is van een doel vereist en kan niet worden verwijderd of hernoemd.

## <a name="negative-intentions"></a>Negatieve bedoelingen 
Als u wilt bepalen negatieve en positieve bedoelingen, zoals "ik **wilt** een auto ' en ' ik **niet** wilt van een auto ', kunt u twee intents (één positieve en negatieve één) maken en toevoegen van de juiste utterances voor elke. Of u kunt een één doel maken en de twee verschillende positieve en negatieve termen als een entiteit markeren.  

## <a name="intent-balance"></a>Opzet saldo
De app domain intents moeten een evenwicht tussen utterances over elk doel hebben. Beschikt niet over een doel met 10 utterances en een ander doel met 500 utterances. Dit is niet in balans. Als u deze situatie hebt, raadpleegt u de bedoeling van 500 utterances om te zien als veel van de die kunnen worden opnieuw ingedeeld in een [patroon](luis-concept-patterns.md). 

De **geen** doel is niet opgenomen in het saldo. Die intentie moet 10% van de totale utterances in de app bevatten.

## <a name="intent-limits"></a>Opzet limieten
Bekijk [limieten](luis-boundaries.md#model-boundaries) om te begrijpen hoeveel intents kunt u toevoegen aan een model. 

### <a name="if-you-need-more-than-the-maximum-number-of-intents"></a>Als u meer dan het maximum aantal intents nodig hebt 
Overweeg eerst of uw systeem te veel intents wordt gebruikt. 

### <a name="can-multiple-intents-be-combined-into-single-intent-with-entities"></a>Meerdere intents kunnen worden gecombineerd tot één doel met entiteiten 
Intents die te veel zijn kunnen het moeilijker voor LUIS onderscheid maken tussen deze. Intents moet genoeg om vast te leggen van de belangrijkste taken die de gebruiker vraagt om, maar ze niet nodig voor het vastleggen van elk pad uw code wordt gewijzigd. Bijvoorbeeld, BookFlight en FlightCustomerService mogelijk afzonderlijke intents in een app reizen, maar BookInternationalFlight en BookDomesticFlight zijn te veel. Als uw systeem moet opnieuw worden onderscheiden, gebruikt u entiteiten of andere logica gebruikt in plaats van een intents. 

### <a name="dispatcher-model"></a>Dispatcher model
Meer informatie over het combineren van LUIS en QnA maker-apps met de [dispatch model](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps). 

### <a name="request-help-for-apps-with-significant-number-of-intents"></a>Hulp voor apps met een groot aantal intents vragen
Als het aantal intents verminderen of uw intents verdelen in meerdere apps voor u niet werkt, moet u contact op met ondersteuning. Als uw Azure-abonnement ondersteuningsservices bevat, neem dan contact op met [Azure technische ondersteuning](https://azure.microsoft.com/support/options/). 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [entiteiten](luis-concept-entity-types.md), die zijn belangrijk woorden die relevant zijn voor intents
* Meer informatie over hoe [toevoegen en beheren van intents](luis-how-to-add-intents.md) in uw app LUIS.
* Bekijk bedoeling [aanbevolen procedures](luis-concept-best-practices.md)

[LUIS]:luis-reference-regions.md#luis-website