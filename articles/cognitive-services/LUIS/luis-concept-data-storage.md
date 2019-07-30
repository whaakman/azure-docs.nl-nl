---
title: Gegevens opslag-LUIS
titleSuffix: Azure Cognitive Services
description: LUIS slaat gegevens op in een Azure-gegevensarchief overeenkomt met de regio die is opgegeven door de sleutel is gecodeerd.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: e3028a9f046edb4013d8a2d6ec88d8c3cb2ca489
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639232"
---
# <a name="data-storage-and-removal-in-language-understanding-luis-cognitive-services"></a>De opslag van gegevens en de verwijdering van Cognitive Services Language Understanding (LUIS)
LUIS slaat gegevens op in een Azure-gegevensarchief overeenkomt met de regio die is opgegeven door de sleutel is gecodeerd. Deze gegevens worden opgeslagen voor 30 dagen. 

## <a name="export-and-delete-app"></a>Exporteren en verwijderen van app
Gebruikers hebben volledig beheer over [exporteren](luis-how-to-start-new-app.md#export-app) en [verwijderen](luis-how-to-start-new-app.md#delete-app) de app. 

## <a name="utterances"></a>Utterances

Uitingen kan op twee verschillende plaatsen worden opgeslagen. 

* Tijdens **het ontwerp proces**worden uitingen gemaakt en opgeslagen in de bedoeling. Uitingen in intenties zijn vereist voor een geslaagde LUIS-app. Zodra de app is gepubliceerd en er query's worden ontvangen op het eind punt, wordt de query `log=false`reeks van het eindpunt verzoek,, bepaalt of het eind punt utterance is opgeslagen. Als het eind punt wordt opgeslagen, maakt het deel uit van het actieve Learning uitingen dat wordt weer gegeven in de sectie **Build** van de portal, in de sectie beoordeling van het **eind punt uitingen** . 
* Wanneer u **endpoint uitingen bekijkt**en een utterance aan een intentie toevoegt, wordt de utterance niet meer opgeslagen als onderdeel van het eind punt uitingen dat moet worden gecontroleerd. Deze wordt toegevoegd aan de intenties van de app. 

<a name="utterances-in-an-intent"></a>

### <a name="delete-example-utterances-from-an-intent"></a>Voor beeld-uitingen verwijderen uit een intentie

Verwijderen van de voorbeeld-uitingen die wordt gebruikt voor de training [LUIS](luis-reference-regions.md). Als u een voorbeeld-utterance uit uw LUIS-app verwijdert, wordt verwijderd uit de LUIS-webservice en is niet beschikbaar voor het exporteren.

<a name="utterances-in-review"></a>

### <a name="delete-utterances-in-review-from-active-learning"></a>Uitingen verwijderen uit actief leren

U kunt uitingen verwijderen uit de lijst met gebruikers-uitingen die LUIS voorgesteld in de  **[voorbeeldpagina eindpunt uitingen](luis-how-to-review-endpoint-utterances.md)** . Uitingen verwijderen uit deze lijst voorkomt dat deze wordt voorgesteld, maar niet verwijderd van Logboeken.

Als u geen actieve Learning uitingen wilt, kunt u [actief leren uitschakelen](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Als u actief leren uitschakelt, wordt logboek registratie ook uitgeschakeld.

### <a name="disable-logging-utterances"></a>Logboek registratie uitingen uitschakelen
Als u [actief leren](luis-how-to-review-endpoint-utterances.md#disable-active-learning) uitschakelt, wordt logboek registratie uitgeschakeld.


<a name="accounts"></a>

## <a name="delete-an-account"></a>Een account verwijderen
Als u een account verwijdert, worden alle apps verwijderd, samen met hun voorbeeld uitingen en Logboeken. De gegevens worden bewaard gedurende 60 dagen voordat het account en gegevens worden definitief verwijderd.

Account verwijderen is beschikbaar via de **instellingen** pagina. Selecteer de accountnaam van uw in de bovenste juiste navigatiebalk om terug te gaan naar de **instellingen** pagina.

## <a name="data-inactivity-as-an-expired-subscription"></a>Inactiviteit van de gegevens als een verlopen abonnement
Voor de toepassing van het bewaren van gegevens en verwijderen, een inactieve LUIS-app kan op _van Microsoft en billijkheid vastgesteld_ worden behandeld als een verlopen abonnement. Een app wordt beschouwd als inactief als deze voldoet aan de volgende criteria voor de afgelopen 90 dagen: 

* Heeft **geen** oproepen naar deze.
* Is niet gewijzigd.
* Niet komt hebben een huidige sleutel toegewezen.
* Heeft een gebruiker zich aanmeldt bij deze niet.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het exporteren en verwijderen van een app](luis-how-to-start-new-app.md)
