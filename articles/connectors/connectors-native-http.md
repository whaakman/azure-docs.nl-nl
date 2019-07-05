---
title: Verbinding maken met HTTP of HTTPS-eindpunten van Azure Logic Apps
description: HTTP of HTTPS-eindpunten in geautomatiseerde taken, processen en werkstromen met behulp van Azure Logic Apps bewaken
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/05/2019
tags: connectors
ms.openlocfilehash: fa5fd3ef8b144826468f56ea2a14be592cef5dc1
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67541252"
---
# <a name="call-http-or-https-endpoints-by-using-azure-logic-apps"></a>HTTP of HTTPS-eindpunten worden aangeroepen met behulp van Azure Logic Apps

Met [Azure Logic Apps](../logic-apps/logic-apps-overview.md) en de ingebouwde HTTP-connector, kunt u Automatiseer werkstromen die regelmatig alle HTTP en HTTPS-eindpunten aanroepen door logic apps te bouwen. U kunt bijvoorbeeld het service-eindpunt voor uw website controleren door te controleren dat eindpunt op een opgegeven schema. Als een bepaalde gebeurtenis plaatsvindt op dat eindpunt, zoals uw website uitvalt, wordt de gebeurtenis wordt de werkstroom van uw logische app geactiveerd en de opgegeven acties worden uitgevoerd.

Om te controleren of *poll* een eindpunt op een vaste planning, kunt u de HTTP-trigger gebruiken als de eerste stap in uw werkstroom. Op elke check-, verzendt de trigger een oproep of *aanvraag* naar het eindpunt. Reactie van het eindpunt wordt bepaald of de werkstroom van uw logische app wordt uitgevoerd. De trigger wordt doorgegeven aan de inhoud uit het antwoord op de acties in uw logische app.

U kunt de HTTP-actie als een andere stap in uw werkstroom voor het aanroepen van het eindpunt als u wilt gebruiken. Reactie van het eindpunt bepaalt hoe uw resterende werkstroomacties worden uitgevoerd.

Op basis van het eindpunt van de doel-capaciteit, de HTTP-connector biedt ondersteuning voor Transport Layer Security (TLS) versie 1.0, 1.1 en 1.2. Logic Apps onderhandelt met het eindpunt van de vergelijking met de hoogste ondersteunde versie mogelijk. Dus als het eindpunt 1.2 ondersteunt, de connector gebruikt bijvoorbeeld 1.2 eerst. Anders wordt de connector maakt gebruik van de volgende hoogste ondersteunde versie.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, [registreer u dan nu voor een gratis Azure-account](https://azure.microsoft.com/free/).

* De URL voor de doel-eindpunt dat u wilt aanroepen

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md). Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

* De logische app van waar u aan te roepen van het doel-eindpunt. Te beginnen met de HTTP-trigger [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Start uw logische app met een trigger die u wilt voor het gebruik van de HTTP-actie. In dit voorbeeld wordt de HTTP-trigger gebruikt als de eerste stap.

## <a name="add-an-http-trigger"></a>Een HTTP-trigger toevoegen

Deze ingebouwde trigger een HTTP-aanroep naar de opgegeven URL voor een eindpunt en een antwoord retourneert.

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Open uw lege, logische app in Logic App Designer.

1. Typ 'http' als filter in het zoekvak in de ontwerpfunctie. Uit de **Triggers** in de lijst met de **HTTP** trigger.

   ![HTTP-trigger selecteren](./media/connectors-native-http/select-http-trigger.png)

   In dit voorbeeld wijzigt de naam van de trigger "HTTP-trigger" zodat de stap een meer beschrijvende naam heeft. Ook in het voorbeeld wordt later een HTTP-actie, en beide namen moeten uniek.

1. Geef de waarden voor de [HTTP-trigger parameters](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger) die u wilt opnemen in de aanroep naar de doel-eindpunt. Het terugkeerpatroon voor hoe vaak u wilt dat de trigger instellen om te controleren of de doel-eindpunt.

   ![HTTP-trigger parameters invoeren](./media/connectors-native-http/http-trigger-parameters.png)

   Zie voor meer informatie over verificatie die beschikbaar zijn voor HTTP, [verifiëren HTTP-triggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

1. Als u wilt toevoegen van andere beschikbare parameters, opent u de **toevoegen van nieuwe parameter** lijst en selecteer de parameters die u wilt.

1. Doorgaan met het ontwikkelen van uw logische app-werkstroom met acties die worden uitgevoerd wanneer de trigger wordt geactiveerd.

1. Wanneer u klaar bent klaar, vergeet niet om op te slaan, uw logische app. Selecteer op de werkbalk van de ontwerper **opslaan**.

## <a name="add-an-http-action"></a>Een HTTP-actie toevoegen

Deze ingebouwde actie maakt een HTTP-aanroep naar de opgegeven URL voor een eindpunt en een antwoord retourneert.

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Open uw logische app in Logic App Designer.

   In dit voorbeeld wordt de HTTP-trigger gebruikt als de eerste stap.

1. Selecteer onder de stap waarin u wilt toevoegen van de HTTP-actie, **nieuwe stap**.

   Als u wilt toevoegen een actie tussen fasen, de aanwijzer over de pijl tussen fasen. Selecteer het plusteken ( **+** ) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

1. Typ 'http' als filter in het zoekvak in de ontwerpfunctie. Uit de **acties** in de lijst met de **HTTP** actie.

   ![HTTP-actie selecteren](./media/connectors-native-http/select-http-action.png)

   Dit voorbeeld wijzigt de actie 'Http-actie' zodat de stap een meer beschrijvende naam heeft.

1. Geef de waarden voor de [HTTP actieparameters](../logic-apps/logic-apps-workflow-actions-triggers.md##http-action) die u wilt opnemen in de aanroep naar de doel-eindpunt.

   ![HTTP-actieparameters invoeren](./media/connectors-native-http/http-action-parameters.png)

   Zie voor meer informatie over verificatie die beschikbaar zijn voor HTTP, [verifiëren HTTP-triggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

1. Als u wilt toevoegen van andere beschikbare parameters, opent u de **toevoegen van nieuwe parameter** lijst en selecteer de parameters die u wilt.

1. Wanneer u klaar bent, moet u uw logische app opslaan. Selecteer op de werkbalk van de ontwerper **opslaan**.

## <a name="connector-reference"></a>Connector-verwijzing

Zie voor meer informatie over parameters trigger en actie in deze secties:

* [HTTP-trigger parameters](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger)
* [HTTP-actieparameters](../logic-apps/logic-apps-workflow-actions-triggers.md##http-action)

### <a name="output-details"></a>Uitvoergegevens

Hier volgt meer informatie over de uitvoer van een HTTP-trigger of actie, dat deze informatie wordt geretourneerd:

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
