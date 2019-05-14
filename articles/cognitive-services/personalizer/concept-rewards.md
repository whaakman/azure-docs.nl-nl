---
title: Beloning score - Personalizer
titleSuffix: Azure Cognitive Services
description: De score beloning geeft aan hoe goed uw persoonlijke instellingen keuze, RewardActionID, heeft geleid tot voor de gebruiker. De waarde van de score van de prijs wordt bepaald door uw zakelijke logica, op basis van metingen van het gedrag van gebruikers. De machine learning-modellen traint personalizer door het evalueren van de voordelen.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/13/2019
ms.author: edjez
ms.openlocfilehash: 536aad0fac4e833cd9a30bad2cfd10e25b0f1300
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65607075"
---
# <a name="reward-scores-indicate-success-of-personalization"></a>Beloning scores sitebeheerpunt van persoonlijke instellingen

De score beloning geeft aan hoe goed de keuze personalisatie [RewardActionID](https://docs.microsoft.com/rest/api/cognitiveservices/personalizer/events/rank#rankresponse), heeft voor de gebruiker. De waarde van de score van de prijs wordt bepaald door uw zakelijke logica, op basis van metingen van het gedrag van gebruikers.

Personalizer traint het machine learning-modellen door het evalueren van de voordelen. 

## <a name="use-reward-api-to-send-reward-score-to-personalizer"></a>Beloning-API gebruiken voor het verzenden van derden score naar Personalizer

Vouchers worden verzonden naar Personalizer door de [beloning API](https://docs.microsoft.com/rest/api/cognitiveservices/personalizer/events/reward). Een beloning is een getal tussen 1 en 1. Personalizer traint het model om de hoogst mogelijke som van de beloningen na verloop van tijd.

Vouchers worden verzonden nadat het gedrag van gebruikers is opgetreden, wat erop kan dagen later. De maximale hoeveelheid tijd Personalizer wacht totdat een gebeurtenis wordt beschouwd als geen vergoeding hebben of een beloning standaard is geconfigureerd met de [beloning wachttijd](#reward-wait-time) in Azure portal.

Als de score beloning voor een gebeurtenis is niet ontvangen binnen de **beloning wachttijd**, dan zal de **standaard beloning** wordt toegepast. Normaal gesproken de **[standaard beloning](how-to-settings.md#configure-reward-settings-for-the-feedback-loop-based-on-use-case)** is geconfigureerd om te worden van nul.

## <a name="composing-reward-scores"></a>Beloning scores samenstellen

Een beloning score moet worden berekend in uw bedrijfslogica. De score kan worden weergegeven als:

* Een enkel getal eenmaal verzonden 
* Een score verzonden onmiddellijk (zoals 0,8) en een extra score hoger verzonden (meestal 0,2).

## <a name="default-rewards"></a>Standaard-voordelen

Als er geen betaling is ontvangen binnen de [beloning wachttijd](#reward-wait-time), de duur sinds de rang aanroepen, Personalizer impliciet van toepassing is de **standaard beloning** op deze positie gebeurtenis.

## <a name="building-up-rewards-with-multiple-factors"></a>Het opbouwen van de beloningen met meerdere factoren  

Voor effectieve personalisatie, kunt u de score beloning opbouwen (een getal tussen 1 en 1) op basis van meerdere factoren. 

U kunt bijvoorbeeld deze regels voor het personaliseren van een lijst met video-inhoud toepassen:

|Gedrag van gebruikers|Gedeeltelijke waarde score|
|--|--|
|De gebruiker heeft geklikt op het eerste item.|+0.5 beloning|
|De gebruiker opent de daadwerkelijke inhoud van dat item.|+0.3 beloning|
|De gebruiker gevolgd 5 minuten van de inhoud of 30%, afhankelijk van welke periode langer is.|+ 0,2 beloning|
|||

Vervolgens kunt u de totale prijs verzenden naar de API.

## <a name="calling-the-reward-api-multiple-times"></a>Aanroepen van de API beloning meerdere keren

U kunt ook de beloning-API met behulp van de dezelfde gebeurtenis-ID, verzenden van verschillende beloning scores aanroepen. Wanneer Personalizer opgehaald die voordelen, bepaalt de definitieve prijs voor die gebeurtenis door samenvoeging van deze die zijn opgegeven in de Personalizer-instellingen.

Globalisatie-instellingen:

*  **First**: Duurt de eerste beloning score voor de gebeurtenis is ontvangen en de rest wordt genegeerd.
* **Som**: Neemt alle beloning scores die zijn verzameld voor de gebeurtenis-id in en voegt deze samen toe.

Alle voordelen van een gebeurtenis, die worden ontvangen na de **beloning wachttijd**, worden verwijderd en niet van invloed op de training van modellen.

Door toe te voegen beloning scores, kan uw uiteindelijke prijs hoger is dan 1 of lager is dan -1 zijn. Hierdoor wordt niet de service mislukt.

<!--
@edjez - is the number ignored if it is outside the acceptable range?
-->

## <a name="best-practices-for-calculating-reward-score"></a>Aanbevolen procedures voor het berekenen van derden score

* **Houd rekening met de waarde true indicatoren van geslaagde persoonlijke instellingen**: Het is eenvoudig te denken in termen van klikken, maar een goede prijs is gebaseerd op wat u wilt dat uw gebruikers *bereiken* in plaats van wat u mensen wilt *doen*.  Bijvoorbeeld, kan belonen op muisklikken leiden tot inhoud die is gevoelig clickbait selecteren.

* **Gebruik een beloning voor hoe goed de personalisatie gewerkt score**: Een suggestie film personaliseren Hopelijk resulteert in de gebruiker de film te bekijken en een hoge beoordeling wordt gegeven. Omdat de classificatie van films is waarschijnlijk afhankelijk is van groot aantal dingen (de kwaliteit van de fungeert, de stemming van de gebruiker), het is niet een signaal goede beloning voor hoe u kunt ook *de personalisatie* gewerkt. De gebruiker de eerste paar minuten van de film bekijken maar mogelijk een betere signaal van de effectiviteit van persoonlijke instellingen en het verzenden van een prijs van 1 na 5 minuten, een betere signaal worden.

* **Vouchers zijn alleen van toepassing op RewardActionID**: Personalizer geldt de voordelen voor meer informatie over de effectiviteit van de actie die is opgegeven in RewardActionID. Als u ervoor kiest om andere acties en de gebruiker klikt op deze weer te geven, moet de prijs nul zijn.

* **Houd rekening met ongewenste gevolgen**: Beloning functies maken die tot verantwoordelijk uitkomsten met leiden [ethiek en verantwoordelijk gebruik](ethics-responsible-use.md).

* **Gebruik incrementele beloningen**: Toevoegen van gedeeltelijke beloningen in kleinere gebruikersgedrag helpt Personalizer bij het bereiken van betere voordelen. Deze incrementele beloning kunt het algoritme om te weten dreigt dichter bij het gebruik van de gebruiker in het laatste gewenste gedrag.
    * Als u kunt een lijst van films, worden weergegeven als de gebruiker de muisaanwijzer op het eerste item voor een tijdje voor meer informatie, kunt u bepalen dat bepaalde gebruikersbetrokkenheid is gebeurd. Het gedrag kan met een score beloning van 0,1 worden geteld. 
    * Als de gebruiker de pagina geopend en vervolgens is afgesloten, kan de score beloning 0.2 zijn. 

## <a name="reward-wait-time"></a>Wachttijd van derden

Personalizer wordt de informatie van een positie correleren aanroepen met de beloningen in beloning aanroepen naar het model te trainen verzonden. Deze kunnen afkomstig zijn op verschillende tijdstippen. Personalizer wacht gedurende een beperkte periode, wordt gestart wanneer de positie aanroep is gebeurd, zelfs als de aanroep van de positie is gemaakt als een niet-actieve gebeurtenis en later opnieuw geactiveerd.

Als de **beloning wachttijd** is verlopen, en er is geen informatie van derden, een standaard-vergoeding wordt toegepast op deze gebeurtenis voor de training. De maximale wachttijd is zes dagen.

## <a name="best-practices-for-setting-reward-wait-time"></a>Wachttijd voor aanbevolen procedures voor het instellen van derden

Volg deze aanbevelingen voor betere resultaten.

* Controleer de beloning wachttijd zo kort mogelijk, terwijl u voldoende tijd om op te halen van feedback van gebruikers. 

<!--@Edjez - storage quota? -->

* Kies geen een duur van die korter is dan de tijd die nodig is om feedback te ontvangen. Bijvoorbeeld, als enkele van uw vouchers zijn verkrijgbaar in nadat een gebruiker 1 minuut van een video is bekeken, de lengte van het experiment moet ten minste dubbele die.

## <a name="next-steps"></a>Volgende stappen

* [Bekrachtigingen](concepts-reinforcement-learning.md) 
* [De positie API proberen](https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api/operations/Rank/console)
* [De prijs API proberen](https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api/operations/Reward)
