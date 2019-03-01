---
title: Prijzen en facturering - Azure Logic Apps | Microsoft Docs
description: Ontdek hoe prijzen en facturering voor Azure Logic Apps werkt
services: logic-apps
ms.service: logic-apps
ms.suite: logic-apps
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
manager: carmonm
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.topic: article
ms.date: 02/26/2019
ms.openlocfilehash: 9b5452f112c6325dafd5edbe693b90ec2a94abc0
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56990234"
---
# <a name="pricing-model-for-azure-logic-apps"></a>Prijsmodel voor Azure Logic Apps

U kunt maken en uitvoeren van de integratie van geautomatiseerde werkstromen die in de cloud schalen kunnen wanneer u Azure Logic Apps. Hier vindt u de informatie over hoe facturering en prijzen voor Logic Apps werken. 

<a name="consumption-pricing"></a>

## <a name="consumption-pricing-model"></a>Prijsmodel voor verbruik

U betaalt alleen voor wat u gebruikt voor nieuwe logische apps die worden uitgevoerd in de openbare of 'global' Logic Apps-service. Deze logische apps gebruiken een planning op basis van gebruik en het prijsmodel. In de definitie van uw logische app is elke stap een actie. Acties omvatten de trigger, elke stroom de controlestappen, ingebouwde acties en aanroepen van de connector. Logic Apps-meters alle acties die worden uitgevoerd in uw logische app.  
Zie voor meer informatie, [Logic Apps-prijzen](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="fixed-pricing"></a>

## <a name="fixed-pricing-model"></a>Vaste prijsmodel

Voor nieuwe logic-apps die worden uitgevoerd binnen een [ *integratieserviceomgeving* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), betaalt u maandelijks een vast bedrag voor ingebouwde acties en standaard-connectors. Een ISE biedt een manier voor u maken en uitvoeren van geïsoleerde logische apps die toegang hebben tot resources in een Azure-netwerk. 

De basiseenheid voor uw ISE-capaciteit, is opgelost, zodat als u meer doorvoer nodig hebt, u kunt [toevoegen van meer schaaleenheden](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#add-capacity), hetzij tijdens het maken of later. Uw ISE bevat één gratis Enterprise-connector, waaronder het aantal verbindingen als u wilt. Gebruik voor extra Enterprise connectors worden in rekening gebracht op basis van de Enterprise-verbruik prijs. 

> [!NOTE]
> Het ISE is in [ *openbare preview*](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Zie voor meer informatie, [Logic Apps-prijzen](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="triggers"></a>

## <a name="triggers"></a>Triggers

Triggers zijn speciale acties die een exemplaar van de logische app maken wanneer een bepaalde gebeurtenis plaatsvindt. Triggers fungeren op verschillende manieren die invloed hebben op hoe de logische app wordt gemeten.

* **Polling-trigger** – deze trigger controleert voortdurend een eindpunt voor berichten die voldoen aan de criteria voor het exemplaar van een logische app maken en starten van de werkstroom. Zelfs wanneer er geen exemplaar van de logische app wordt gemaakt, Logic Apps-meters elke polling-aanvraag als een uitvoering. Als u wilt opgeven voor het pollinginterval, instellen van de trigger via de ontwerper van logische App.

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Webhook-trigger** – deze trigger moet wachten voor een client een aanvraag te verzenden naar een bepaald eindpunt. Elke aanvraag verzonden naar de webhook-eindpunt telt als een actie kan worden uitgevoerd. De aanvraag- en HTTP-Webhook-trigger zijn bijvoorbeeld beide webhook-triggers.

* **Trigger met terugkeerpatroon** – deze trigger maakt een logische app-exemplaar op basis van het terugkeerpatroon die u hebt ingesteld in de trigger. U kunt bijvoorbeeld een trigger met terugkeerpatroon die elke drie dagen wordt uitgevoerd of op een meer complexe planning instellen.

## <a name="actions"></a>Acties

Logic Apps-meters ingebouwde acties als systeemeigen acties. Ingebouwde acties bijvoorbeeld aanroepen via HTTP, aanroepen van Azure Functions of API Management en beheer stromingsstappen zoals lussen en voorwaarden 
- elk met hun eigen actietype. Acties die aanroepen [connectors](https://docs.microsoft.com/connectors) hebben van het type 'ApiConnection'. Deze connectors zijn geclassificeerd als standard of enterprise-connectors, die worden gemeten op basis van hun respectieve [prijzen][pricing]. Enterprise-connectors in *Preview* worden in rekening gebracht als standaard-connectors.

Logic Apps-meters alle correct en zonder succes acties worden uitgevoerd als actie-uitvoeringen. Logic Apps niet houdt door deze acties: 

* Acties die vanwege een nog niet vervulde voorwaarden overgeslagen u
* Acties die niet worden uitgevoerd omdat de logische app is gestopt voordat u klaar bent

Uitgeschakelde logische apps worden niet kosten in rekening gebracht tijdens uitgeschakeld omdat ze nieuwe exemplaren kunnen niet maken.

> [!NOTE]
> Nadat u een logische app uitschakelen, kunnen alle exemplaren die momenteel wordt uitgevoerd even voordat ze volledig is gestopt.

Voor de acties die worden uitgevoerd binnen lussen, telt Logic Apps elke actie per cyclus in de lus. Stel bijvoorbeeld dat u hebt een lus 'voor elke' waarmee een lijst wordt verwerkt. Logische Apps meters een actie in deze lus wordt vermenigvuldigd met het nummer van de lijst met items met het aantal acties op de hoogte en voegt de actie die de lus wordt gestart. De berekening voor een lijst met 10 items is (10 * 1) + 1, wat tot 11 actie-uitvoeringen leidt.

## <a name="integration-account-usage"></a>Gebruik van integratie van het Account

Gebruik op basis van gebruik van toepassing op [integratieaccounts](logic-apps-enterprise-integration-create-integration-account.md) waar u kunt verkennen, ontwikkelen en testen van de [B2B/EDI](logic-apps-enterprise-integration-b2b.md) en [XML-verwerking](logic-apps-enterprise-integration-xml.md) functies in Logic Apps zonder extra kosten. U kunt een integratieaccount per regio hebben. Elke integratie-account kan maximaal opslaan op specifieke [aantallen artefacten](../logic-apps/logic-apps-limits-and-config.md), waaronder trading partners, overeenkomsten, kaarten, schema's, assembly's, certificaten, batchconfiguraties enzovoort.

Logic Apps biedt ook basic en standard-integratieaccounts met ondersteunde SLA van logische Apps. U kunt de basic-integratieaccounts kunt gebruiken wanneer u maar afhandeling van berichten wilt of fungeren als een kleine zakelijke partner die een trading partner relatie heeft met een grotere bedrijfsentiteit. Standard-integratieaccounts ondersteunen complexere B2B-relaties en verhoog het aantal entiteiten die u kunt beheren. Zie voor meer informatie, [Azure-prijzen](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Logic Apps][whatis]
* [Uw eerste logische app maken][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-overview.md
[create]: quickstart-create-first-logic-app-workflow.md

