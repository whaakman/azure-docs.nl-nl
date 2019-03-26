---
title: Prijzen en facturering - Azure Logic Apps | Microsoft Docs
description: Ontdek hoe prijzen en facturering voor Azure Logic Apps werkt
services: logic-apps
ms.service: logic-apps
ms.suite: logic-apps
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.topic: article
ms.date: 03/25/2019
ms.openlocfilehash: 4e5b63642b5090b50d29d6d3adc49ea683b90faa
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58437710"
---
# <a name="pricing-model-for-azure-logic-apps"></a>Prijsmodel voor Azure Logic Apps

[Met Azure Logic Apps](../logic-apps/logic-apps-overview.md) kunt u maken en uitvoeren van integratie van geautomatiseerde werkstromen die kunnen worden geschaald in de cloud. In dit artikel wordt beschreven hoe facturering en prijzen voor Azure Logic Apps werken. Zie voor gedetailleerde informatie over prijzen [prijzen voor Azure Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="consumption-pricing"></a>

## <a name="consumption-pricing-model"></a>Prijsmodel voor verbruik

U betaalt alleen voor wat u gebruikt voor nieuwe logische apps die worden uitgevoerd in de openbare of 'global' Azure Logic Apps-service. Deze logische apps gebruiken een planning op basis van gebruik en het prijsmodel. In de definitie van uw logische app is elke stap een actie. Bijvoorbeeld: acties omvatten: 

* Triggers speciale handelingen zijn. Alle logische apps is een trigger als de eerste stap vereist.
* 'Ingebouwde' of door de systeemeigen acties zoals het HTTP-aanroepen naar Azure Functions en API Management, enzovoort
* Aanroepen naar connectors zoals Outlook 365, Dropbox, enzovoort
* Beheren van de stromingsstappen, zoals lussen, voorwaardelijke instructies, enzovoort

Met Azure Logic Apps-meters de acties die worden uitgevoerd in uw logische app. Meer informatie over de werking van facturering voor [triggers](#triggers) en [acties](#actions).

<a name="fixed-pricing"></a>

## <a name="fixed-pricing-model"></a>Vaste prijsmodel

Voor nieuwe logic-apps die worden uitgevoerd binnen een [ *integratieserviceomgeving* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), betaalt u maandelijks een vast bedrag voor ingebouwde acties en standaard-connectors. Een ISE biedt een manier voor u maken en uitvoeren van geïsoleerde logische apps die toegang hebben tot resources in een Azure-netwerk.

> [!NOTE]
> Het ISE is in [ *openbare preview*](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Zie voor gedetailleerde informatie over prijzen [prijzen voor Azure Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps).

De basiseenheid voor uw ISE-capaciteit, is opgelost, zodat als u meer doorvoer nodig hebt, u kunt [toevoegen van meer schaaleenheden](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#add-capacity), hetzij tijdens het maken of later. Uw ISE bevat één gratis Enterprise-connector, waaronder het aantal verbindingen als u wilt. Voor extra Enterprise-connectors wordt verrekend op basis van de prijs van de Enterprise-resourceverbruik.

<a name="connectors"></a>

## <a name="connectors"></a>Connectors

Azure Logic Apps-connectors u helpen uw logische app toegang tot apps, services en systemen in de cloud of on-premises met [triggers](#triggers), [acties](#actions), of beide. Connectors zijn geclassificeerd als Standard of Enterprise. Zie voor een overzicht van deze connectors, [Connectors voor Azure Logic Apps](../connectors/apis-list.md). De volgende secties bevatten meer informatie over hoe de facturering voor triggers en acties werken.

<a name="triggers"></a>

## <a name="triggers"></a>Triggers

Triggers zijn speciale acties die een exemplaar van de logische app maken wanneer een bepaalde gebeurtenis plaatsvindt. Triggers fungeren op verschillende manieren die invloed hebben op hoe de logische app wordt gemeten. Hier volgen de verschillende soorten triggers die aanwezig zijn in Azure Logic Apps:

* **Polling-trigger**: Deze trigger controleert voortdurend een eindpunt voor berichten die voldoen aan de criteria voor het exemplaar van een logische app maken en starten van de werkstroom. Zelfs wanneer er geen exemplaar van de logische app wordt gemaakt, Logic Apps-meters elke polling-aanvraag als een uitvoering. Als u wilt opgeven voor het pollinginterval, instellen van de trigger via de ontwerper van logische App.

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Webhook-trigger**: Deze trigger moet wachten voor een client een aanvraag te verzenden naar een bepaald eindpunt. Elke aanvraag verzonden naar de webhook-eindpunt telt als een actie kan worden uitgevoerd. De aanvraag- en HTTP-Webhook-trigger zijn bijvoorbeeld beide webhook-triggers.

* **Trigger met terugkeerpatroon**: Deze trigger maakt een logische app-exemplaar op basis van het terugkeerpatroon die u hebt ingesteld in de trigger. U kunt bijvoorbeeld een trigger met terugkeerpatroon die elke drie dagen wordt uitgevoerd of op een meer complexe planning instellen.

<a name="actions"></a>

## <a name="actions"></a>Acties

Met Azure Logic Apps-meters 'ingebouwde' acties, zoals HTTP, als systeemeigen acties. Bijvoorbeeld, ingebouwde acties opnemen van HTTP-aanroepen, aanroepen van Azure Functions of de beheer-API, en stromingsstappen zoals voorwaarden, lussen, beheren en switch-instructies. Elke actie heeft een eigen actietype. Bijvoorbeeld, de acties die aanroepen [connectors](https://docs.microsoft.com/connectors) hebben van het type 'ApiConnection'. Deze connectors zijn geclassificeerd als Standard of Enterprise-connectors worden gemeten op basis van hun respectieve [prijzen](https://azure.microsoft.com/pricing/details/logic-apps). Enterprise-connectors in *Preview* worden in rekening gebracht als standaard-connectors.

Met Azure Logic Apps-meters alle geslaagde en mislukte acties als uitvoeringen. Logic Apps niet echter houdt door deze acties:

* Acties die vanwege een nog niet vervulde voorwaarden overgeslagen u
* Acties die niet worden uitgevoerd omdat de logische app is gestopt voordat u klaar bent

Azure Logic Apps telt voor acties die worden uitgevoerd binnen lussen, elke actie voor elke cyclus in de lus. Stel bijvoorbeeld dat u hebt een lus 'voor elke' waarmee een lijst wordt verwerkt. Logische Apps meters een actie in deze lus wordt vermenigvuldigd met het nummer van de lijst met items met het aantal acties op de hoogte en voegt de actie die de lus wordt gestart. De berekening voor een lijst met 10 items is dus (10 * 1) + 1, wat tot 11 actie-uitvoeringen leidt.

## <a name="disabled-logic-apps"></a>Uitgeschakelde logische apps

Uitgeschakeld logic apps niet in rekening gebracht omdat ze kunnen geen nieuwe exemplaren maken terwijl ze zijn uitgeschakeld.
Nadat u een logische app uitschakelen, kunnen alle exemplaren die momenteel wordt uitgevoerd even voordat ze volledig is gestopt.

## <a name="integration-accounts"></a>Integratieaccounts

Verbruiksprijzen is van toepassing op [integratieaccounts](logic-apps-enterprise-integration-create-integration-account.md) waar u kunt verkennen, ontwikkelen en testen van de [B2B en EDI](logic-apps-enterprise-integration-b2b.md) en [XML-verwerking](logic-apps-enterprise-integration-xml.md) functies in Azure Logic Apps zonder extra kosten.
U kunt een integratie-account hebben in elke Azure-regio. Elke integratie-account kan maximaal opslaan op specifieke [aantallen artefacten](../logic-apps/logic-apps-limits-and-config.md), waaronder trading partners, overeenkomsten, kaarten, schema's, assembly's, certificaten, batchconfiguraties enzovoort.

Met Azure Logic Apps biedt ook Basic en Standard-integratieaccounts met ondersteunde SLA van logische Apps. Hier zijn manieren kunt u kiezen of een Basic- of Standard-integratie-account te gebruiken:

* Basic-integratieaccounts gebruiken wanneer u maar afhandeling van berichten wilt of fungeren als een kleine zakelijke partner die een trading partner relatie heeft met een grotere bedrijfsentiteit.

* Standard-integratieaccounts gebruiken wanneer u complexere B2B-relaties en wilt verhogen van het aantal entiteiten die u kunt beheren.

Zie voor gedetailleerde informatie over prijzen [prijzen voor Azure Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="data-retention"></a>

## <a name="data-retention"></a>Bewaartijd van gegevens

Alle invoer en uitvoer die zijn opgeslagen in de uitvoeringsgeschiedenis van uw logische app in rekening gebracht op basis van een logische app [uitvoeren bewaarperiode](logic-apps-limits-and-config.md#run-duration-retention-limits). Zie voor gedetailleerde informatie over prijzen [prijzen voor Azure Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps).

Voor hulp bij het controleren van het opslagverbruik van uw logische app, kunt u het volgende doen:

* Het aantal eenheden voor opslag in GB die uw logische app maakt gebruik van elke maand weergeven.
* De grootte voor de invoer en uitvoer van een specifieke actie in de uitvoeringsgeschiedenis van uw logische app weergeven.

<a name="storage-consumption"></a>

### <a name="view-logic-app-storage-consumption"></a>Gebruik van de weergave logic app-opslag

1. In de Azure-portal, zoeken en openen van uw logische app.

1. Uw logische app in het menu onder **bewaking**, selecteer **metrische gegevens**.

1. In het rechterdeelvenster onder **grafiektitel**, uit de **Metric** in de lijst met **gebruik facturering voor Storage verbruik uitvoeringen**.

   Deze metrische gegevens kunt u het aantal verbruikseenheden voor opslag in GB per maand die worden gefactureerd.

<a name="input-output-sizes"></a>

### <a name="view-action-input-and-output-sizes"></a>Weergeven van actie-invoer en uitvoer grootten

1. In de Azure-portal, zoeken en openen van uw logische app.

1. Selecteer in het menu van uw logische app **overzicht**.

1. In het rechterdeelvenster onder **geschiedenis van uitvoeringen**, selecteert u de uitvoering die de invoer heeft en levert u wilt controleren.

1. Onder **logische app**, kiest u **Details uitvoering van**.

1. In de **details uitvoering van de logische app** deelvenster in de tabel acties, die een lijst met de status en de duur van elke actie, selecteert u de actie die u wilt weergeven.

1. In de **actie voor logische app** deelvenster, zoek de grootten voor de invoer en uitvoer van deze actie worden respectievelijk onder weergegeven **invoerkoppeling** en **uitvoerkoppeling**.

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Azure Logic Apps](logic-apps-overview.md)
* [Een logische app maken](quickstart-create-first-logic-app-workflow.md)