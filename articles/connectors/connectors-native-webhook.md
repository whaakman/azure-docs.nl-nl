---
title: Webhook-connector voor Azure Logic Apps | Microsoft Docs
description: Het gebruik van webhookacties en triggers voor het uitvoeren van acties zoals matrix van Filter vanuit logic apps
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 71775384-6c3a-482c-a484-6624cbe4fcc7
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: fbfef291334109c6dcfcde80741874549fb7929f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-webhook-connector"></a>Aan de slag met de webhook-connector

Met de actie van de webhook en trigger, kunt u starten, onderbreken en hervatten van stromen om deze taken uitvoeren:

* Activeren van een [Azure Event Hub](https://github.com/logicappsio/EventHubAPI) wanneer een item wordt ontvangen
* Wachten op een goedkeuring voordat u doorgaat een werkstroom

Meer informatie over [het maken van aangepaste API's die ondersteuning bieden voor een webhook](../logic-apps/logic-apps-create-api-app.md).

## <a name="use-the-webhook-trigger"></a>De webhook-trigger te gebruiken

Een [ *trigger* ](connectors-overview.md) is een gebeurtenis die een werkstroom van logic app start. Een trigger webhook is op basis van gebeurtenissen en niet afhankelijk is van polling voor nieuwe items. Als de [aanvraag trigger](connectors-native-reqres.md), de logische app het moment dat het geval van een gebeurtenis wordt geactiveerd. De webhook-trigger registreert een *retouraanroep URL* naar een service en wordt die URL activeert de logische app als nodig is.

Hier volgt een voorbeeld waarin het instellen van een HTTP-trigger in de ontwerpfunctie voor Logic App. De stappen wordt ervan uitgegaan dat u al hebt geïmplementeerd of toegang hebben tot een API die volgt de [webhook abonneren en zich afmelden patroon in logic apps](../logic-apps/logic-apps-create-api-app.md#webhook-triggers). De aanroep abonneren wordt uitgevoerd wanneer een logische app wordt opgeslagen met een nieuwe webhook of overgeschakeld van uitgeschakeld in ingeschakeld. De aanroep afmelden uitgevoerd wanneer een logische app webhook trigger wordt verwijderd en opgeslagen of gewijzigd van ingeschakeld in uitgeschakeld.

**De webhook-trigger toevoegen**

1. Voeg de **HTTP-Webhook** trigger als de eerste stap in een logische app.
2. Vul de parameters voor de webhook abonneren en aanroepen opzeggen.

   Deze stap volgt hetzelfde patroon als de [HTTP-actie](connectors-native-http.md) indeling.

     ![HTTP-Trigger](./media/connectors-native-webhook/using-trigger.png)

3. Voeg ten minste één actie.
4. Klik op **opslaan** voor het publiceren van de logische app. Deze stap wordt het eindpunt abonneren met de callback-URL die nodig zijn voor het activeren van deze logische app.
5. Wanneer de service maakt een `HTTP POST` voor de callback-URL, de logische app wordt geactiveerd en bevat geen gegevens doorgegeven aan de aanvraag.

## <a name="use-the-webhook-action"></a>Gebruik de actie webhook

Een [ *actie* ](connectors-overview.md) wordt een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een logische app. Registreert een webhook-actie een *retouraanroep URL* met een service en wordt er gewacht totdat de URL wordt aangeroepen voordat het wordt hervat. De ['Goedkeuring E-mail verzenden'](connectors-create-api-office365-outlook.md) is een voorbeeld van een connector die volgt op dit patroon. U kunt dit patroon uitbreiden naar alle services die via de webhook-actie. 

Hier volgt een voorbeeld waarin het instellen van een webhook actie in de ontwerpfunctie voor Logic App. Deze stappen wordt ervan uitgegaan dat u al hebt geïmplementeerd of toegang hebben tot een API die volgt de [webhook abonneren en zich afmelden patroon dat wordt gebruikt in logic apps](../logic-apps/logic-apps-create-api-app.md#webhook-actions). De aanroep abonneren uitgevoerd wanneer een logische app de webhook-actie uitvoert. De aanroep afmelden uitgevoerd wanneer een uitvoering is geannuleerd tijdens het wachten op reactie of voordat de logic app een optreedt time-out.

**Toevoegen van een webhook-actie**

1. Kies **nieuwe stap** > **een actie toevoegen**.

2. Typ in het zoekvak 'webhook' om te zoeken de **HTTP-Webhook** in te grijpen.

    ![Selecteer de queryactie](./media/connectors-native-webhook/using-action-1.png)

3. Vul de parameters voor de webhook abonneren en aanroepen afmelden

   Deze stap volgt hetzelfde patroon als de [HTTP-actie](connectors-native-http.md) indeling.

     ![Volledige queryactie](./media/connectors-native-webhook/using-action-2.png)
   
   Tijdens runtime roept de logische app het eindpunt abonneren wanneer die stap is bereikt.

4. Klik op **opslaan** voor het publiceren van de logische app.

## <a name="technical-details"></a>Technische details

Hier vindt u meer informatie over de triggers en acties die webhook ondersteunt.

## <a name="webhook-triggers"></a>Webhook-triggers

| Actie | Beschrijving |
| --- | --- |
| HTTP-Webhook |Zich abonneren een retouraanroep URL naar een service die de URL gestart logische app naar behoefte kunt aanroepen. |

### <a name="trigger-details"></a>Details van de trigger

#### <a name="http-webhook"></a>HTTP-Webhook

Zich abonneren een retouraanroep URL naar een service die de URL gestart logische app naar behoefte kunt aanroepen.
Een * betekent verplicht veld.

| Weergavenaam | De naam van eigenschap | Beschrijving |
| --- | --- | --- |
| Abonneren methode * |Methode |HTTP-methode moet worden gebruikt voor het abonneren op aanvraag |
| Abonneren URI * |URI |HTTP-URI moet worden gebruikt voor het abonneren op aanvraag |
| Afmelden methode * |Methode |HTTP-methode moet worden gebruikt voor afmelding |
| Afmelden URI * |URI |HTTP-URI moet worden gebruikt voor afmelding |
| Hoofdtekst abonneren |Hoofdtekst |Hoofdtekst van de HTTP-aanvraag voor het abonneren |
| Headers abonneren |Headers |HTTP-aanvraagheaders voor abonneren |
| Abonneren verificatie |Verificatie |HTTP-verificatie moet worden gebruikt voor het abonneren. [Zie HTTP connector](connectors-native-http.md#authentication) voor meer informatie |
| Hoofdtekst afmelden |Hoofdtekst |Hoofdtekst van de HTTP-aanvraag voor afmelden |
| Headers afmelden |Headers |HTTP-aanvraagheaders voor afmelden |
| Verificatie afmelden |Verificatie |HTTP-verificatie moet worden gebruikt voor afmelden. [Zie HTTP connector](connectors-native-http.md#authentication) voor meer informatie |

**Uitvoerdetails**

Webhook-aanvraag

| De naam van eigenschap | Gegevenstype | Beschrijving |
| --- | --- | --- |
| Headers |object |Webhook-aanvraagheaders |
| Hoofdtekst |object |Webhook request-object |
| Statuscode |int |Statuscode van aanvraag Webhook |

## <a name="webhook-actions"></a>Webhookacties

| Actie | Beschrijving |
| --- | --- |
| HTTP-Webhook |Zich abonneren een retouraanroep URL naar een service die de URL voor het hervatten van een werkstroomstap zo nodig kunt aanroepen. |

### <a name="action-details"></a>Actiedetails

#### <a name="http-webhook"></a>HTTP-Webhook

Zich abonneren een retouraanroep URL naar een service die de URL voor het hervatten van een werkstroomstap zo nodig kunt aanroepen.
Een * betekent verplicht veld.

| Weergavenaam | De naam van eigenschap | Beschrijving |
| --- | --- | --- |
| Abonneren methode * |Methode |HTTP-methode moet worden gebruikt voor het abonneren op aanvraag |
| Abonneren URI * |URI |HTTP-URI moet worden gebruikt voor het abonneren op aanvraag |
| Afmelden methode * |Methode |HTTP-methode moet worden gebruikt voor afmelding |
| Afmelden URI * |URI |HTTP-URI moet worden gebruikt voor afmelding |
| Hoofdtekst abonneren |Hoofdtekst |Hoofdtekst van de HTTP-aanvraag voor het abonneren |
| Headers abonneren |Headers |HTTP-aanvraagheaders voor abonneren |
| Abonneren verificatie |Verificatie |HTTP-verificatie moet worden gebruikt voor het abonneren. [Zie HTTP connector](connectors-native-http.md#authentication) voor meer informatie |
| Hoofdtekst afmelden |Hoofdtekst |Hoofdtekst van de HTTP-aanvraag voor afmelden |
| Headers afmelden |Headers |HTTP-aanvraagheaders voor afmelden |
| Verificatie afmelden |Verificatie |HTTP-verificatie moet worden gebruikt voor afmelden. [Zie HTTP connector](connectors-native-http.md#authentication) voor meer informatie |

**Uitvoerdetails**

Webhook-aanvraag

| De naam van eigenschap | Gegevenstype | Beschrijving |
| --- | --- | --- |
| Headers |object |Webhook-aanvraagheaders |
| Hoofdtekst |object |Webhook request-object |
| Statuscode |int |Statuscode van aanvraag Webhook |

## <a name="next-steps"></a>Volgende stappen

* [Een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md)
* [Andere connectors zoeken](apis-list.md)