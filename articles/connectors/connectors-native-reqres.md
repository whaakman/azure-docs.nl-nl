---
title: Aanvraag- en responsacties gebruiken | Microsoft Docs
description: Overzicht van de aanvraag en respons trigger en een actie in een logische app van Azure
services: ''
documentationcenter: ''
author: jeffhollan
manager: erikre
editor: ''
tags: connectors
ms.assetid: 566924a4-0988-4d86-9ecd-ad22507858c0
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: 2479db2abcb578eb380655346582392770606b39
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57552177"
---
# <a name="get-started-with-the-request-and-response-components"></a>Aan de slag met de aanvraag- en -onderdelen
Met de aanvraag- en -onderdelen in een logische app, kunt u in realtime reageren op gebeurtenissen.

U kunt bijvoorbeeld:

* Reageren op een HTTP-aanvraag met gegevens van een on-premises database via een logische app.
* Een logische app vanaf een externe webhookgebeurtenis activeren.
* Een logische app met een actie voor het aanvragen en reacties uit in een andere logische app aanroepen.

Om te beginnen met de aanvraag en antwoord-acties in een logische app, Zie [maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="use-the-http-request-trigger"></a>Gebruik de trigger HTTP-aanvraag
Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die is gedefinieerd in een logische app te starten. [Meer informatie over triggers](connectors-overview.md).

Hier volgt een van de voorbeeldreeks over het instellen van een HTTP-aanvraag in Logic App Designer.

1. Toevoegen van de trigger **aanvraag: wanneer een HTTP-aanvraag wordt ontvangen** in uw logische app. U kunt desgewenst een JSON-schema (met behulp van een hulpprogramma zoals [JSONSchema.net](https://jsonschema.net)) voor de hoofdtekst van de aanvraag. Hiermee wordt de ontwerper voor het genereren van tokens voor eigenschappen in de HTTP-aanvraag.
2. Nog een actie toevoegen zodat u de logische app kunt opslaan.
3. Na het opslaan van de logische app, krijgt u de HTTP-aanvraag-URL van de aanvraag-kaart.
4. Een HTTP POST (u kunt een hulpprogramma zoals [Postman](https://www.getpostman.com/)) naar de URL activeert u de logische app.

> [!NOTE]
> Als u een reactie niet definieert een `202 ACCEPTED` antwoord wordt onmiddellijk geretourneerd voor de oproepende functie. U kunt de reactie voor het aanpassen van een antwoord.
> 
> 

![Antwoord-trigger](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a>Gebruik de actie van de HTTP-antwoord
De HTTP-antwoord-actie is alleen geldig wanneer u deze gebruikt in een werkstroom die wordt geactiveerd door een HTTP-aanvraag. Als u een reactie niet definieert een `202 ACCEPTED` antwoord wordt onmiddellijk geretourneerd voor de oproepende functie.  U kunt een reactie op een stap in de werkstroom toevoegen. De logische app alleen houdt de inkomende aanvraag openen voor één minuut voor een reactie.  Na een minuut, als er geen antwoord is verzonden vanaf de werkstroom (en een reactie in het definitie bestaat), een `504 GATEWAY TIMEOUT` wordt geretourneerd naar de oproepende functie.

Dit is het toevoegen van een HTTP-antwoord-actie:

1. Selecteer de **nieuwe stap** knop.
2. Kies **een actie toevoegen**.
3. Typ in het zoekvak actie **antwoord** om de reactie is weer te geven.
   
    ![Selecteer de reactie](./media/connectors-native-reqres/using-action-1.png)
4. Voeg parameters die vereist voor de HTTP-antwoordbericht zijn.
   
    ![De reactie is voltooid](./media/connectors-native-reqres/using-action-2.png)
5. Klik op de linkerbovenhoek van de werkbalk om op te slaan en de logische app wordt zowel opslaan en publiceren (activeren).

## <a name="request-trigger"></a>Trigger voor de aanvraag
Hier volgen de details voor de trigger die ondersteuning biedt voor deze connector. Er is een trigger één aanvraag.

| Trigger | Description |
| --- | --- |
| Aanvraag |Treedt op wanneer een HTTP-aanvraag wordt ontvangen |

## <a name="response-action"></a>Reactie
Hier volgen de details voor de actie die ondersteuning biedt voor deze connector. Er is een één-reactie die alleen kan worden gebruikt wanneer deze is voorzien van een aanvraag als trigger.

| Bewerking | Description |
| --- | --- |
| Antwoord |Retourneert een antwoord naar de gecorreleerde HTTP-aanvraag |

### <a name="trigger-and-action-details"></a>Details van de trigger en actie
De volgende tabellen beschrijven de invoervelden voor de trigger en de actie en de bijbehorende details van de uitvoer.

#### <a name="request-trigger"></a>Trigger voor de aanvraag
Hier volgt een invoerveld voor de trigger van een binnenkomende HTTP-aanvraag.

| Weergavenaam | Naam van eigenschap | Description |
| --- | --- | --- |
| JSON-Schema |schema |De JSON-schema van de hoofdtekst van de HTTP-aanvraag |

<br>

**Uitvoergegevens**

Hier volgen de uitvoerdetails van de voor de aanvraag.

| Naam van eigenschap | Gegevenstype | Description |
| --- | --- | --- |
| Headers |object |Aanvraagheaders |
| Hoofdtekst |object |Request-object |

#### <a name="response-action"></a>Reactie
Hier volgen de invoervelden voor de actie van de HTTP-antwoord. Een * betekent dat het een verplicht veld.

| Weergavenaam | Naam van eigenschap | Description |
| --- | --- | --- |
| Status Code * |statusCode |De HTTP-statuscode |
| Headers |Headers |Een JSON-object van een antwoordheaders om op te nemen |
| Hoofdtekst |hoofdtekst |De hoofdtekst van reactie |

## <a name="next-steps"></a>Volgende stappen
Nu uitproberen van het platform en [maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). U kunt de beschikbare connectors in logic apps verkennen door te kijken onze [lijst van API's](apis-list.md).

