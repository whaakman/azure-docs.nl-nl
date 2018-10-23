---
title: Gegevensopslag in LUIS - Language Understanding
titleSuffix: Azure Cognitive Services
description: Meer informatie over hoe de gegevens in Language Understanding (LUIS) worden opgeslagen. LUIS slaat gegevens op in een Azure-gegevensarchief overeenkomt met de regio die is opgegeven door de sleutel is gecodeerd.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: f876c4e279e723120794c550392512f5672ef91e
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637642"
---
# <a name="data-storage-and-removal-in-language-understanding-luis-cognitive-services"></a>De opslag van gegevens en de verwijdering van Cognitive Services Language Understanding (LUIS)
LUIS slaat gegevens op in een Azure-gegevensarchief overeenkomt met de regio die is opgegeven door de sleutel is gecodeerd. Deze gegevens worden opgeslagen voor 30 dagen. 

## <a name="export-and-delete-app"></a>Exporteren en verwijderen van app
Gebruikers hebben volledig beheer over [exporteren](luis-how-to-start-new-app.md#export-app) en [verwijderen](luis-how-to-start-new-app.md#delete-app) de app. 

## <a name="utterances-in-an-intent"></a>Uitingen in opzet
Verwijderen van de voorbeeld-uitingen die wordt gebruikt voor de training [LUIS](luis-reference-regions.md). Als u een voorbeeld-utterance uit uw LUIS-app verwijdert, wordt verwijderd uit de LUIS-webservice en is niet beschikbaar voor het exporteren.

## <a name="utterances-in-review"></a>Uitingen onder revisie
U kunt uitingen verwijderen uit de lijst met gebruikers-uitingen die LUIS voorgesteld in de  **[voorbeeldpagina eindpunt uitingen](luis-how-to-review-endoint-utt.md)**. Uitingen verwijderen uit deze lijst voorkomt dat deze wordt voorgesteld, maar niet verwijderd van Logboeken.

## <a name="accounts"></a>Accounts
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