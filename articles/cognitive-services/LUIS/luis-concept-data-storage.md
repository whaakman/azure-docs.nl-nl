---
title: Gegevensopslag in LUIS - Azure begrijpen | Microsoft Docs
description: Meer informatie over hoe de gegevens in het Language Understanding (LUIS) worden opgeslagen
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/08/2018
ms.author: v-geberr
ms.openlocfilehash: f235c787e7d2064696e5421219a297d914b5882d
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266002"
---
# <a name="data-storage-and-removal"></a>Opslag van gegevens en verwijderen
LUIS slaat gegevens op in een Azure-gegevensarchief overeenkomt met de regio is opgegeven door de sleutel is gecodeerd. Deze gegevens worden opgeslagen voor 30 dagen. 

## <a name="export-and-delete-app"></a>Exporteren en verwijderen van app
Gebruikers hebben volledig beheer [exporteren](create-new-app.md#export-app) en [verwijderen](create-new-app.md#delete-app) de app. 

## <a name="utterances-in-an-intent"></a>Utterances in de opzet
Voorbeeld utterances gebruikt voor training verwijderen [LUIS][LUIS]. Als u een voorbeeld utterance van uw app LUIS verwijdert, wordt verwijderd uit de webservice LUIS en is niet beschikbaar voor exporteren.

## <a name="utterances-in-review"></a>Utterances gecontroleerd
U kunt utterances verwijderen uit de lijst met gebruikers utterances die LUIS wordt voorgesteld in de  **[voorbeeldpagina eindpunt utterances](label-suggested-utterances.md)**. Utterances verwijderen uit deze lijst voorkomt dat deze wordt voorgesteld, maar niet verwijderen van Logboeken.

## <a name="accounts"></a>Accounts
Als u een account verwijdert, worden alle apps verwijderd, samen met hun voorbeeld utterances en de logboeken. De gegevens worden bewaard gedurende 60 dagen voordat het account en gegevens worden definitief verwijderd.

Verwijderen van account is beschikbaar via de **instellingen** pagina. Selecteer de accountnaam van uw in de juiste bovenste navigatiebalk om te krijgen tot de **instellingen** pagina.

## <a name="data-inactivity-as-an-expired-subscription"></a>Gegevens inactiviteit als een verlopen abonnementen
Voor de toepassing van het bewaren van gegevens en verwijderen, een inactieve LUIS app kan op _ter discretie van Microsoft_ worden behandeld als een verlopen abonnement. Een app als inactief beschouwd als deze voldoet aan de volgende criteria voor de afgelopen 90 dagen: 

* Heeft **geen** oproepen naar deze.
* Is niet gewijzigd.
* Geen biedt huidige sleutel hebben om.
* Een gebruiker zich aanmeldt bij deze eerder niet.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het exporteren en het verwijderen van een app](create-new-app.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions