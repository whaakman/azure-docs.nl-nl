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
ms.date: 09/24/2018
ms.openlocfilehash: 5f9147035c07bbe4fb3f38b74025015e70dd87b3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47159553"
---
# <a name="pricing-model-for-azure-logic-apps"></a>Prijsmodel voor Azure Logic Apps

U kunt maken en uitvoeren van werkstromen voor geautomatiseerde, schaalbare gegevensintegratie in de cloud met Azure Logic Apps. Hier vindt u de informatie over hoe facturering en prijzen voor Logic Apps werken. 

<a name="consumption-pricing"></a>

## <a name="consumption-pricing-model"></a>Prijsmodel voor verbruik

U betaalt alleen voor wat u gebruikt voor nieuwe logische apps die u maakt met behulp van de openbare of 'global' Logic Apps-service. Deze logische apps gebruiken een planning op basis van gebruik en het prijsmodel, wat betekent dat alle uitvoeringsacties uitgevoerd door een logische app, worden gemeten. Elke stap in de definitie van een logische app is een actie, waaronder triggers, besturingselement stromingsstappen aanroepen naar ingebouwde acties en connectors-aanroepen. Zie voor meer informatie, [Logic Apps-prijzen](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="fixed-pricing"></a>

## <a name="fixed-pricing-model"></a>Vaste prijsmodel

> [!NOTE]
> De integratieserviceomgeving is in *privépreview*. Om te vragen tot, [maken van uw aanvraag voor deelname aan hier](https://aka.ms/iseprivatepreview).

Voor nieuwe logic-apps die u met maakt een [ *integratieserviceomgeving* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), Logic Apps-exemplaar dat toegewezen resources gebruikt die een particulier is geïsoleerd, betaalt u een vaste maandelijkse prijs voor ingebouwde acties en ISE etiket standaardconnectors. Uw ISE bevat een Enterprise-connector zonder kosten, terwijl andere Enterprise-connectors worden in rekening gebracht op basis van de Enterprise-verbruik prijs. Zie voor meer informatie, [Logic Apps-prijzen](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="triggers"></a>

## <a name="triggers"></a>Triggers

Triggers zijn speciale acties die een exemplaar van de logische app maken wanneer een bepaalde gebeurtenis plaatsvindt. Triggers fungeren op verschillende manieren die invloed hebben op hoe de logische app wordt gemeten.

* **Polling-trigger** – deze trigger controleert voortdurend een eindpunt voor berichten die voldoen aan de criteria voor het exemplaar van een logische app maken en starten van de werkstroom. Elke aanvraag polling telt als een uitvoering en wordt gemeten, zelfs als er geen logische app-exemplaar is gemaakt. Als u wilt opgeven voor het pollinginterval, instellen van de trigger via de ontwerper van logische App.

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Webhook-trigger** – deze trigger moet wachten voor een client een aanvraag te verzenden naar een bepaald eindpunt. Elke aanvraag verzonden naar de webhook-eindpunt telt als een actie kan worden uitgevoerd. De aanvraag- en HTTP-Webhook-trigger zijn bijvoorbeeld beide webhook-triggers.

* **Trigger met terugkeerpatroon** – deze trigger maakt een logische app-exemplaar op basis van het terugkeerpatroon die u hebt ingesteld in de trigger. U kunt bijvoorbeeld een trigger met terugkeerpatroon die elke drie dagen wordt uitgevoerd of op een meer complexe planning instellen.

Uitvoeringen door een trigger vindt u in het overzichtsvenster van uw logische app in de sectie Geschiedenis van de Trigger.

## <a name="actions"></a>Acties

Ingebouwde acties, zoals de acties die HTTP-, Azure Functions of API Management en bepalen ook stromingsstappen worden gemeten als systeemeigen acties waarvoor hun respectieve typen. Acties die aanroepen [connectors](https://docs.microsoft.com/connectors) hebben van het type 'ApiConnection'. Deze connectors zijn geclassificeerd als standard of enterprise-connectors, die worden gemeten op basis van hun respectieve [prijzen][pricing]. Enterprise-connectors in *Preview* worden in rekening gebracht als standaard-connectors.

Alle correct en zonder succes uitgevoerd acties zijn geteld en als actie-uitvoeringen gemeten. Acties die zijn overgeslagen, omdat het nog niet vervulde voorwaarden of acties die niet worden uitgevoerd, omdat de logische app wordt beëindigd voordat de voltooid, wordt echter niet gerekend als actie-uitvoeringen. Uitgeschakelde logic apps kunnen geen exemplaar maken van nieuwe exemplaren, zodat ze niet in rekening gebracht als ze zijn uitgeschakeld.

> [!NOTE]
> Nadat u een logische app uitschakelen, kunnen alle exemplaren die momenteel wordt uitgevoerd even voordat ze volledig is gestopt.

Acties die worden uitgevoerd binnen lussen worden geteld per elke cyclus in de lus. Bijvoorbeeld, de één actie in een lus 'voor elke' die een lijst met 10 items verwerkt door het aantal items in lijst (10) vermenigvuldigen met het aantal acties op de hoogte (1) plus één voor het starten van de lus wordt geteld. In dit voorbeeld de berekening is dus (10 * 1) + 1, wat tot 11 actie-uitvoeringen leidt.

## <a name="integration-account-usage"></a>Gebruik van integratie van het Account

Gebruik op basis van gebruik bevat een [integratieaccount](logic-apps-enterprise-integration-create-integration-account.md) waar u kunt verkennen, ontwikkelen en testen van de [B2B/EDI](logic-apps-enterprise-integration-b2b.md) en [XML-verwerking](logic-apps-enterprise-integration-xml.md) functies in Logic Apps zonder extra kosten. U kunt een integratieaccount per regio en store tot specifieke hebben [aantallen artefacten](../logic-apps/logic-apps-limits-and-config.md), zoals EDI-handelspartners en -overeenkomsten, kaarten, schema's, assembly's, certificaten en batchconfiguraties.

Logic Apps biedt ook basic en standard-integratieaccounts met ondersteunde SLA van logische Apps. U kunt de basic-integratieaccounts kunt gebruiken als u wilt gebruiken alleen bericht verwerken, of fungeren als een kleine zakelijke partner die een trading partner relatie heeft met een grotere bedrijfsentiteit. Standard-integratieaccounts ondersteunen complexere B2B-relaties en verhoog het aantal entiteiten die u kunt beheren. Zie voor meer informatie, [Azure-prijzen](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Logic Apps][whatis]
* [Uw eerste logische app maken][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-overview.md
[create]: quickstart-create-first-logic-app-workflow.md

