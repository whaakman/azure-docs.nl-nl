---
title: Aanvraag en -antwoord acties gebruiken | Microsoft Docs
description: Overzicht van de aanvraag en antwoord-trigger en action in een Azure logic app
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 566924a4-0988-4d86-9ecd-ad22507858c0
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: 58210db585befd7ce915d4579d4d0303eb15bff3
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-request-and-response-components"></a>Aan de slag met de aanvraag en antwoord-onderdelen
Met de aanvraag- en -onderdelen in een logische app kunt u in realtime reageren op gebeurtenissen.

U kunt bijvoorbeeld:

* Reageren op een HTTP-aanvraag met gegevens van een on-premises database via een logische app.
* Een logische app vanaf een externe webhook-gebeurtenis geactiveerd.
* Een logische app met de actie aanvraag en -antwoord uit in een andere logische app aanroepen.

Om te beginnen met de aanvraag en antwoord-acties in een logische app, Zie [een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="use-the-http-request-trigger"></a>Gebruik de trigger HTTP-aanvraag
Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die is gedefinieerd in een logische app te starten. [Meer informatie over triggers](connectors-overview.md).

Hier volgt een voorbeeld van de volgorde van het instellen van een HTTP-aanvraag in de ontwerpfunctie voor Logic App.

1. De trigger toevoegen **aanvraag: wanneer een HTTP-aanvraag wordt ontvangen** in uw logische app. U kunt desgewenst een JSON-schema (met behulp van een hulpprogramma zoals [JSONSchema.net](http://jsonschema.net)) voor de aanvraagtekst. Hierdoor kan de ontwerpfunctie voor het genereren van tokens voor eigenschappen in de HTTP-aanvraag.
2. Nog een actie toevoegen zodat u de logische app kunt opslaan.
3. Na het opslaan van de logische app, kunt u de HTTP-aanvraag-URL ophalen van de aanvraag-kaart.
4. Een HTTP POST (kunt u een hulpprogramma zoals [Postman](https://www.getpostman.com/)) naar de URL activeert de logische app.

> [!NOTE]
> Als u een actie antwoord niet definieert een `202 ACCEPTED` antwoord wordt onmiddellijk geretourneerd naar de aanroeper. U kunt de antwoord-actie voor het aanpassen van een antwoord.
> 
> 

![Antwoord trigger](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a>Gebruik de actie HTTP-antwoord
De HTTP-antwoord-actie is alleen geldig wanneer u deze gebruikt in een werkstroom die wordt geactiveerd door een HTTP-aanvraag. Als u een actie antwoord niet definieert een `202 ACCEPTED` antwoord wordt onmiddellijk geretourneerd naar de aanroeper.  U kunt een antwoord actie toevoegen bij elke stap in de werkstroom. De logische app alleen houdt de binnenkomende aanvraag open voor één minuut op een reactie.  Na een minuut, als er geen antwoord is verzonden van de werkstroom (en een actie antwoord in de definitie bestaat), een `504 GATEWAY TIMEOUT` wordt geretourneerd naar de aanroeper.

Dit is het toevoegen van een HTTP-antwoord-actie:

1. Selecteer de **nieuwe stap** knop.
2. Kies **een actie toevoegen**.
3. Typ in het zoekvak actie **antwoord** voor een lijst met de actie antwoord.
   
    ![Selecteer de actie antwoord](./media/connectors-native-reqres/using-action-1.png)
4. In de parameters die vereist voor het HTTP-antwoordbericht zijn toevoegen.
   
    ![De reactie actie niet voltooien](./media/connectors-native-reqres/using-action-2.png)
5. Klik op de linkerbovenhoek van de werkbalk om op te slaan en uw logische app wordt opslaan en publiceren (activeren).

## <a name="request-trigger"></a>Aanvraag trigger
Hier volgen de details voor de trigger die ondersteuning biedt voor deze connector. Er is een trigger één aanvraag.

| Trigger | Beschrijving |
| --- | --- |
| Aanvraag |Deze gebeurtenis treedt op wanneer er een HTTP-aanvraag wordt ontvangen |

## <a name="response-action"></a>Antwoord actie
Hier volgen de details voor de actie die ondersteuning biedt voor deze connector. Er is een enkele antwoordthread-actie kan alleen worden gebruikt wanneer u gaat vergezeld van een aanvraag-trigger.

| Bewerking | Beschrijving |
| --- | --- |
| Antwoord |Retourneert een antwoord aan de gecorreleerde HTTP-aanvraag |

### <a name="trigger-and-action-details"></a>Details van de trigger en action
De volgende tabellen beschrijven de invoervelden voor de trigger en action en de bijbehorende details uitvoer.

#### <a name="request-trigger"></a>Aanvraag trigger
Hier volgt een invoerveld voor de trigger van een binnenkomende HTTP-aanvraag.

| Weergavenaam | Naam van eigenschap | Beschrijving |
| --- | --- | --- |
| JSON-Schema |schema |De JSON-schema van het hoofdgedeelte van de HTTP-aanvraag |

<br>

**Uitvoerdetails**

Hier volgen de uitvoerdetails voor de aanvraag.

| Naam van eigenschap | Gegevenstype | Beschrijving |
| --- | --- | --- |
| Kopteksten |object |Aanvraagheaders |
| Hoofdtekst |object |Request-object |

#### <a name="response-action"></a>Antwoord actie
Hier volgen de invoervelden voor de HTTP-antwoord-actie. A * houdt in dat een vereist veld.

| Weergavenaam | Naam van eigenschap | Beschrijving |
| --- | --- | --- |
| Status Code * |statusCode |De HTTP-statuscode |
| Kopteksten |headers |Een JSON-object van een antwoordheaders om op te nemen |
| Hoofdtekst |hoofdtekst |Hoofdtekst van de reactie |

## <a name="next-steps"></a>Volgende stappen
Nu uitproberen van het platform en [een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md). U kunt de beschikbare connectors in logische apps door te kijken verkennen onze [API's lijst](apis-list.md).

