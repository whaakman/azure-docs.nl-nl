---
title: Gegevensopslag
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS slaat gegevens op in een Azure-gegevensarchief overeenkomt met de regio die is opgegeven door de sleutel is gecodeerd.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: diberry
ms.openlocfilehash: a1093c2a6303b453a17a52058303913de5ecfa8d
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893194"
---
# <a name="data-storage-and-removal-in-language-understanding-luis-cognitive-services"></a>De opslag van gegevens en de verwijdering van Cognitive Services Language Understanding (LUIS)
LUIS slaat gegevens op in een Azure-gegevensarchief overeenkomt met de regio die is opgegeven door de sleutel is gecodeerd. Deze gegevens worden opgeslagen voor 30 dagen. 

## <a name="export-and-delete-app"></a>Exporteren en verwijderen van app
Gebruikers hebben volledig beheer over [exporteren](luis-how-to-start-new-app.md#export-app) en [verwijderen](luis-how-to-start-new-app.md#delete-app) de app. 

## <a name="utterances"></a>Utterances

Uitingen kunnen worden opgeslagen in twee verschillende plaatsen. 

* Tijdens de **het ontwerpproces**, uitingen worden gemaakt en opgeslagen in het doel. Uitingen in intents zijn vereist voor een geslaagde LUIS-app. Zodra de app is gepubliceerd en query's op het eindpunt, de querystring van de eindpunt-aanvraag, ontvangt `log=false`, bepaalt u of het eindpunt utterance is opgeslagen. Als het eindpunt is opgeslagen, wordt het onderdeel van de actief leren uitingen gevonden in de **bouwen** sectie van de portal, in de **bekijken eindpunt uitingen** sectie. 
* Wanneer u **bekijken eindpunt uitingen**, en een utterance toevoegen aan een doel, de utterance is niet meer opgeslagen als onderdeel van de eindpunt-uitingen worden beoordeeld. Deze wordt toegevoegd aan de app intents. 

<a name="utterances-in-an-intent"></a>

### <a name="delete-example-utterances-from-an-intent"></a>Voorbeeld-uitingen vanuit een intent verwijderen
Verwijderen van de voorbeeld-uitingen die wordt gebruikt voor de training [LUIS](luis-reference-regions.md). Als u een voorbeeld-utterance uit uw LUIS-app verwijdert, wordt verwijderd uit de LUIS-webservice en is niet beschikbaar voor het exporteren.

<a name="utterances-in-review"></a>

### <a name="delete-utterances-in-review-from-active-learning"></a>Uitingen onder revisie verwijderen uit actief leren

U kunt uitingen verwijderen uit de lijst met gebruikers-uitingen die LUIS voorgesteld in de  **[voorbeeldpagina eindpunt uitingen](luis-how-to-review-endpoint-utterances.md)**. Uitingen verwijderen uit deze lijst voorkomt dat deze wordt voorgesteld, maar niet verwijderd van Logboeken.

Als u niet dat uitingen actief leren wilt, kunt u [uitschakelen actief leren](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Actief leren uit te schakelen, schakelt ook logboekregistratie.

### <a name="disable-logging-utterances"></a>Uitingen logboekregistratie uitschakelen
[Actief leren uitschakelen](luis-how-to-review-endpoint-utterances.md#disable-active-learning) schakelt logboekregistratie is.


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
