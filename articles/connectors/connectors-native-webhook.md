---
title: Op gebeurtenissen gebaseerde taken en werkstromen in Azure Logic Apps maken
description: Starten, onderbreken en hervatten van geautomatiseerde taken, processen en werkstromen op basis van gebeurtenissen die op een eindpunt plaatsvinden met behulp van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/05/2019
tags: connectors
ms.openlocfilehash: c2658df185d4836210c496d2c46a00a3541257a2
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67541365"
---
# <a name="automate-event-based-tasks-and-workflows-by-using-http-webhooks-in-azure-logic-apps"></a>Op gebeurtenissen gebaseerde taken en werkstromen automatiseren met behulp van HTTP-webhooks in Azure Logic Apps

Met [Azure Logic Apps](../logic-apps/logic-apps-overview.md) en de ingebouwde HTTP-Webhook-connector, kunt u werkstromen die wacht en uitgevoerd op basis van specifieke gebeurtenissen die op een HTTP of HTTPS-eindpunt plaatsvinden met het bouwen van logische apps automatiseren. Bijvoorbeeld, kunt u een logische app waarmee een service-eindpunt wordt gecontroleerd door een specifieke gebeurtenis wachten voordat de werkstroom wordt geactiveerd en de opgegeven acties uitvoeren, in plaats van regelmatig controleren of *polling* dat eindpunt.

Hier volgen enkele voorbeeld-werkstromen op basis van gebeurtenissen:

* Wachten op een item te zijn van een [Azure Event Hub](https://github.com/logicappsio/EventHubAPI) voordat u activeert een logische app uitvoeren.
* Wachten op een goedkeuring voordat u doorgaat een werkstroom.

## <a name="how-do-webhooks-work"></a>De werking van webhooks

Een HTTP-webhook-trigger is op basis van gebeurtenissen die niet afhankelijk is controleren of het polling-regelmatig nieuwe items. Wanneer u een logische app die begint met een webhook-trigger, of wanneer u uw logische app gewijzigd van uitgeschakeld in ingeschakeld, de webhook-trigger opslaat *geabonneerd* aan een bepaalde service of het eindpunt van het registreren van een *URL voor terugbellen voor* met die service of -eindpunt. De trigger wordt vervolgens gewacht voor die service of het eindpunt voor het aanroepen van de URL, die de logische app wordt gestart. Vergelijkbaar met de [aanvraagtrigger](connectors-native-reqres.md), de logische app wordt geactiveerd vlak wanneer de opgegeven gebeurtenis plaatsvindt. De trigger *afmeldingen* van de service of het eindpunt als u de trigger verwijdert en sla uw logische app, of wanneer u uw logische app van ingeschakeld in uitgeschakeld.

Een HTTP-webhook-bewerking is ook op basis van gebeurtenissen en *geabonneerd* aan een bepaalde service of het eindpunt van het registreren van een *URL voor terugbellen* met die service of -eindpunt. Door de webhookactie wordt de logic app-werkstroom wordt onderbroken en wordt er gewacht tot de service of het eindpunt aanroepen van de URL voor de logische app hervat. De actie logische app *afmeldingen* van de service of het eindpunt in dergelijke gevallen:

* Wanneer de webhookactie succes is voltooid
* Als de uitvoering van de logische app is geannuleerd tijdens het wachten op reactie
* Voordat u de logische app een time-out optreedt

Bijvoorbeeld, de Office 365 Outlook-connector van [ **e-mail voor goedkeuring verzenden** ](connectors-create-api-office365-outlook.md) actie is een voorbeeld van webhookactie die dit patroon volgt. U kunt dit patroon uitbreiden naar een service met behulp van de webhookactie.

Zie de volgende onderwerpen voor meer informatie:

* [Parameters voor HTTP-Webhook-trigger](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger)
* [Webhooks en abonnementen](../logic-apps/logic-apps-workflow-actions-triggers.md#webhooks-and-subscriptions)
* [Aangepaste API's die ondersteuning bieden voor een webhook maken](../logic-apps/logic-apps-create-api-app.md)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, [registreer u dan nu voor een gratis Azure-account](https://azure.microsoft.com/free/).

* De URL voor een reeds geïmplementeerde eindpunt of API die ondersteuning biedt voor de webhook abonneren en u afmelden voor patroon voor [webhook-triggers in logische apps](../logic-apps/logic-apps-create-api-app.md#webhook-triggers) of [webhookacties in logische apps](../logic-apps/logic-apps-create-api-app.md#webhook-actions) indien van toepassing

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md). Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

* De logische app waar u wilt wachten op specifieke gebeurtenissen op de doel-eindpunt. Om te beginnen met de HTTP-Webhook-trigger, [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Start uw logische app met een trigger die u wilt voor het gebruik van de HTTP-Webhook-actie. In dit voorbeeld wordt de HTTP-trigger gebruikt als de eerste stap.

## <a name="add-an-http-webhook-trigger"></a>Een HTTP-Webhook-trigger toevoegen

Deze ingebouwde trigger een retouraanroep-URL met de opgegeven service registreert en wacht voor die service een HTTP POST-aanvraag verzenden naar deze URL. Als deze gebeurtenis plaatsvindt, wordt de trigger wordt geactiveerd en wordt de logische app onmiddellijk uitgevoerd.

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Open uw lege, logische app in Logic App Designer.

1. De ontwerpfunctie in het zoekvak, typ 'http-webhook' als filter. Uit de **Triggers** in de lijst met de **HTTP-Webhook** trigger.

   ![HTTP-Webhook-trigger selecteren](./media/connectors-native-webhook/select-http-webhook-trigger.png)

   In dit voorbeeld wijzigt de naam van de trigger "Http-Webhook-trigger" zodat de stap een meer beschrijvende naam heeft. Ook in het voorbeeld wordt later een HTTP-Webhook-actie, en beide namen moeten uniek.

1. Geef de waarden voor de [HTTP-Webhook-trigger parameters](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger) dat u wilt gebruiken voor het abonneren en u afmelden voor aanroepen, bijvoorbeeld:

   ![HTTP-Webhook-trigger parameters invoeren](./media/connectors-native-webhook/http-webhook-trigger-parameters.png)

1. Als u wilt toevoegen van andere beschikbare parameters, opent u de **toevoegen van nieuwe parameter** lijst en selecteer de parameters die u wilt.

   Zie voor meer informatie over verificatie die beschikbaar zijn voor HTTP-Webhook [verifiëren HTTP-triggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

1. Doorgaan met het ontwikkelen van uw logische app-werkstroom met acties die worden uitgevoerd wanneer de trigger wordt geactiveerd.

1. Wanneer u klaar bent klaar, vergeet niet om op te slaan, uw logische app. Selecteer op de werkbalk van de ontwerper **opslaan**.

   Opslaan van uw logische app roept het eindpunt abonneren en registreert de callback-URL voor het activeren van deze logische app.

1. Nu, wanneer de doelservice verzendt een `HTTP POST` aanvraag naar de callback-URL, de logische app wordt geactiveerd, en alle gegevens die de aanvraag wordt doorgegeven.

## <a name="add-an-http-webhook-action"></a>Een HTTP-Webhook-actie toevoegen

Deze ingebouwde bewerking wordt een URL voor terugbellen geregistreerd bij de opgegeven service, de logic app-werkstroom wordt onderbroken en wacht voor die service een HTTP POST-aanvraag verzenden naar deze URL. Als deze gebeurtenis plaatsvindt, wordt de actie hervat de logische app uitgevoerd.

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Open uw logische app in Logic App Designer.

   In dit voorbeeld wordt de HTTP-Webhook-trigger gebruikt als de eerste stap.

1. Selecteer onder de stap waarin u wilt toevoegen van de HTTP-Webhook-actie, **nieuwe stap**.

   Als u wilt toevoegen een actie tussen fasen, de aanwijzer over de pijl tussen fasen. Selecteer het plusteken ( **+** ) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

1. De ontwerpfunctie in het zoekvak, typ 'http-webhook' als filter. Uit de **acties** in de lijst met de **HTTP-Webhook** actie.

   ![HTTP-Webhook-actie selecteren](./media/connectors-native-webhook/select-http-webhook-action.png)

   Dit voorbeeld wijzigt de actie "HTTP-Webhook action" zodat de stap een meer beschrijvende naam heeft.

1. Geef de waarden voor de HTTP-Webhook actieparameters die vergelijkbaar met zijn de [HTTP-Webhook-trigger parameters](../logic-apps/logic-apps-workflow-actions-triggers.md##http-webhook-trigger) dat u wilt gebruiken voor het abonneren en u afmelden voor aanroepen, bijvoorbeeld:

   ![HTTP-Webhook actieparameters opgeven](./media/connectors-native-webhook/http-webhook-action-parameters.png)

   De logische app roept tijdens runtime, het eindpunt abonneren wanneer deze actie wordt uitgevoerd. Vervolgens wordt de werkstroom wordt onderbroken en wacht tot de doelservice voor het verzenden van uw logische app een `HTTP POST` aanvraag voor de callback-URL. Als de actie voltooid is, wordt de actie van het eindpunt afmeldingen en uw logische app wordt hervat waarop de werkstroom wordt uitgevoerd.

1. Als u wilt toevoegen van andere beschikbare parameters, opent u de **toevoegen van nieuwe parameter** lijst en selecteer de parameters die u wilt.

   Zie voor meer informatie over verificatie die beschikbaar zijn voor HTTP-Webhook [verifiëren HTTP-triggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

1. Wanneer u klaar bent, moet u uw logische app opslaan. Selecteer op de werkbalk van de ontwerper **opslaan**.

## <a name="connector-reference"></a>Connector-verwijzing

Zie voor meer informatie over de trigger en actie-parameters, die vergelijkbaar met elkaar zijn, [HTTP-Webhook parameters](../logic-apps/logic-apps-workflow-actions-triggers.md##http-webhook-trigger).

### <a name="output-details"></a>Uitvoergegevens

Hier volgt meer informatie over de uitvoer van een HTTP-Webhook-trigger of actie, dat deze informatie wordt geretourneerd:

| Naam van eigenschap | Type | Description |
|---------------|------|-------------|
| Headers | object | De headers van de aanvraag |
| De hoofdtekst | object | JSON-object | Het object met de inhoud van de hoofdtekst van de aanvraag |
| Statuscode | int | De statuscode van de aanvraag |
|||

| Statuscode | Description |
|-------------|-------------|
| 200 | OK |
| 202 | Geaccepteerd |
| 400 | Ongeldig verzoek |
| 401 | Niet geautoriseerd |
| 403 | Verboden |
| 404 | Niet gevonden |
| 500 | Interne serverfout. Er is een onbekende fout opgetreden. |
|||

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
