---
title: Maak werkstromen op basis van gebeurtenissen of acties - Azure Logic Apps | Microsoft Docs
description: Werkstromen op basis van gebeurtenissen of acties automatiseren met behulp van webhooks en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, jehollan, LADocs
ms.assetid: 71775384-6c3a-482c-a484-6624cbe4fcc7
ms.topic: article
tags: connectors
ms.date: 07/21/2016
ms.openlocfilehash: c0985df445ae34795d5287144d4664755cc006da
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58182112"
---
# <a name="create-event-based-workflows-or-actions-by-using-webhooks-and-azure-logic-apps"></a>Maken van werkstromen op basis van gebeurtenissen of acties worden uitgevoerd met behulp van webhooks en Azure Logic Apps

Met de webhookactie en -trigger, kunt u starten, onderbreken en hervatten van stromen om uit te voeren van deze taken:

* Activeren van een [Azure Event Hub](https://github.com/logicappsio/EventHubAPI) wanneer een item wordt ontvangen
* Wachten op een goedkeuring voordat u doorgaat een werkstroom

Meer informatie over [over het maken van aangepaste API's die ondersteuning bieden voor een webhook](../logic-apps/logic-apps-create-api-app.md).

## <a name="use-the-webhook-trigger"></a>Gebruik de webhook-trigger

Een [ *trigger* ](connectors-overview.md) is een gebeurtenis waarmee een werkstroom voor logische app wordt gestart. De webhook-trigger is op basis van gebeurtenissen die niet afhankelijk van het controleren op nieuwe items. Wanneer u uw logische app opslaan met een webhook-trigger, of wanneer u uw logische app gewijzigd van uitgeschakeld naar ingeschakeld, de webhook-trigger *geabonneerd* aan de opgegeven service of het eindpunt van het registreren van een *URL voor terugbellen voor* met die service of -eindpunt. De trigger gebruikt vervolgens die URL om de logische app uitvoeren indien nodig. Net als de [aanvraagtrigger](connectors-native-reqres.md), de logische app wordt geactiveerd vlak wanneer de verwachte gebeurtenis plaatsvindt. De trigger *afmeldingen* als u de trigger verwijdert en sla uw logische app, of wanneer u uw logische app van ingeschakeld in uitgeschakeld.

Hier volgt een voorbeeld waarin wordt weergegeven over het instellen van een HTTP-trigger in de Logic App Designer. De stappen wordt ervan uitgegaan dat u al hebt geïmplementeerd, of gebruikmaken van een API die volgt op de [webhook abonneren en u afmelden voor patroon in logische apps](../logic-apps/logic-apps-create-api-app.md#webhook-triggers). 

**De webhook-trigger toevoegen**

1. Voeg de **HTTP-Webhook** trigger als de eerste stap in een logische app.
2. Vul de parameters voor de webhook abonneren en u afmelden voor aanroepen.

   Deze stap volgt hetzelfde patroon als het [HTTP-actie](connectors-native-http.md) indeling.

     ![HTTP-trigger](./media/connectors-native-webhook/using-trigger.png)

3. Voeg ten minste één actie toe.
4. Klik op **opslaan** voor het publiceren van de logische app. Deze stap wordt het eindpunt abonneren met de callback-URL nodig voor het activeren van deze logische app.
5. Wanneer de service maakt een `HTTP POST` naar de callback-URL, de logische app wordt gestart en alle gegevens doorgegeven aan de aanvraag bevat.

## <a name="use-the-webhook-action"></a>Gebruik de webhookactie

Een [ *actie* ](connectors-overview.md) is een bewerking die gedefinieerd en uitgevoerd door de werkstroom van uw logische app. Wanneer een logische app wordt uitgevoerd met een webhookactie, die actie *geabonneerd* aan de opgegeven service of het eindpunt van het registreren van een *URL voor terugbellen voor* met die service of -eindpunt. De webhookactie en wordt er gewacht totdat die service aanroepen van de URL voor de logische app hervat. De logische app afmeldingen van de service of het eindpunt in dergelijke gevallen: 

* Wanneer de webhookactie succes is voltooid
* Als de uitvoering van de logische app is geannuleerd tijdens het wachten op reactie
* Voordat u de logische app een time-out optreedt

Bijvoorbeeld, de [ **e-mail voor goedkeuring verzenden** ](connectors-create-api-office365-outlook.md) actie is een voorbeeld van webhookactie die dit patroon volgt. U kunt dit patroon uitbreiden naar een service door de webhookactie. 

Hier volgt een voorbeeld waarin wordt uitgelegd hoe u een webhookactie van de in de ontwerper van logische App instelt. Deze stappen wordt ervan uitgegaan dat u al hebt geïmplementeerd, of gebruikmaken van een API die volgt op de [webhook abonneren en u afmelden voor patroon dat wordt gebruikt in logische apps](../logic-apps/logic-apps-create-api-app.md#webhook-actions). 

**Een webhookactie toevoegen**

1. Kies **nieuwe stap** > **een actie toevoegen**.

2. In het zoekvak typt u "webhook" om de **HTTP-Webhook** actie.

    ![De actie SELECT-query](./media/connectors-native-webhook/using-action-1.png)

3. Vul de parameters voor de webhook abonneren en u afmelden voor aanroepen

   Deze stap volgt hetzelfde patroon als het [HTTP-actie](connectors-native-http.md) indeling.

     ![Actie voltooid query](./media/connectors-native-webhook/using-action-2.png)
   
   Tijdens runtime roept de logische app het eindpunt abonneren wanneer deze stap is bereikt.

4. Klik op **opslaan** voor het publiceren van de logische app.

## <a name="technical-details"></a>Technische details

Hier vindt u meer informatie over de triggers en acties die webhook ondersteunt.

## <a name="webhook-triggers"></a>Webhook-triggers

| Bewerking | Description |
| --- | --- |
| HTTP-webhook |Abonneren op een callback-URL voor een service die de URL om te worden geactiveerd logische app zo nodig kunt aanroepen. |

### <a name="trigger-details"></a>Triggerdetails

#### <a name="http-webhook"></a>HTTP-webhook

Abonneren op een callback-URL voor een service die de URL om te worden geactiveerd logische app zo nodig kunt aanroepen.
Een * betekent vereist veld.

| Weergavenaam | Naam van eigenschap | Description |
| --- | --- | --- |
| Abonneren methode * |method |HTTP-methode moet worden gebruikt voor de abonnementsaanvraag |
| Abonneren URI * |uri |HTTP-URI moet worden gebruikt voor de abonnementsaanvraag |
| Afmelden methode * |method |HTTP-methode moet worden gebruikt voor afmelding |
| Afmelden URI * |uri |HTTP-URI moet worden gebruikt voor afmelding |
| Hoofdtekst abonneren |hoofdtekst |Hoofdtekst van de HTTP-aanvraag voor abonneren |
| Headers abonneren |Headers |HTTP-aanvraagheaders voor abonneren |
| Abonneer u verificatie |verificatie |HTTP-verificatie moet worden gebruikt voor het abonneren. [Zie de HTTP-connector](connectors-native-http.md#authentication) voor meer informatie |
| Afmelden voor hoofdtekst |hoofdtekst |Hoofdtekst van de HTTP-aanvraag voor afmelden |
| Headers afmelden |Headers |HTTP-aanvraagheaders voor afmelden |
| Afmelden voor verificatie |verificatie |HTTP-verificatie moet worden gebruikt voor afmelden. [Zie de HTTP-connector](connectors-native-http.md#authentication) voor meer informatie |

**Uitvoergegevens**

Webhook-aanvraag

| Naam van eigenschap | Gegevenstype | Description |
| --- | --- | --- |
| Headers |object |Webhook-aanvraagheaders |
| Hoofdtekst |object |Webhook-request-object |
| Statuscode |int |Statuscode van de Webhook-aanvraag |

## <a name="webhook-actions"></a>Webhookacties

| Bewerking | Description |
| --- | --- |
| HTTP-webhook |Abonneren op een callback-URL voor een service die de URL om te hervatten van een werkstroomstap naar behoefte kunt aanroepen. |

### <a name="action-details"></a>Actiedetails

#### <a name="http-webhook"></a>HTTP-webhook

Abonneren op een callback-URL voor een service die de URL om te hervatten van een werkstroomstap naar behoefte kunt aanroepen.
Een * betekent vereist veld.

| Weergavenaam | Naam van eigenschap | Description |
| --- | --- | --- |
| Abonneren methode * |method |HTTP-methode moet worden gebruikt voor de abonnementsaanvraag |
| Abonneren URI * |uri |HTTP-URI moet worden gebruikt voor de abonnementsaanvraag |
| Afmelden methode * |method |HTTP-methode moet worden gebruikt voor afmelding |
| Afmelden URI * |uri |HTTP-URI moet worden gebruikt voor afmelding |
| Hoofdtekst abonneren |hoofdtekst |Hoofdtekst van de HTTP-aanvraag voor abonneren |
| Headers abonneren |Headers |HTTP-aanvraagheaders voor abonneren |
| Abonneer u verificatie |verificatie |HTTP-verificatie moet worden gebruikt voor het abonneren. [Zie de HTTP-connector](connectors-native-http.md#authentication) voor meer informatie |
| Afmelden voor hoofdtekst |hoofdtekst |Hoofdtekst van de HTTP-aanvraag voor afmelden |
| Headers afmelden |Headers |HTTP-aanvraagheaders voor afmelden |
| Afmelden voor verificatie |verificatie |HTTP-verificatie moet worden gebruikt voor afmelden. [Zie de HTTP-connector](connectors-native-http.md#authentication) voor meer informatie |

**Uitvoergegevens**

Webhook-aanvraag

| Naam van eigenschap | Gegevenstype | Description |
| --- | --- | --- |
| Headers |object |Webhook-aanvraagheaders |
| Hoofdtekst |object |Webhook-request-object |
| Statuscode |int |Statuscode van de Webhook-aanvraag |

## <a name="next-steps"></a>Volgende stappen

* [Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Andere connectors zoeken](apis-list.md)
