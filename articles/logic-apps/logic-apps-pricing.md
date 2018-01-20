---
title: Prijzen en facturering - Azure Logic Apps | Microsoft Docs
description: Meer informatie over prijzen en facturering werking voor Azure Logic Apps.
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2017
ms.author: LADocs; klam
ms.openlocfilehash: 49a66c826a98f7160b97b516e6fd541795ae3b0e
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="logic-apps-pricing-model"></a>Prijsmodel voor logische apps
U kunt maken en uitvoeren van geautomatiseerde schaalbare integratie werkstromen in de cloud met Azure Logic Apps. Hier volgen de details over de werking facturering en prijzen voor Logic Apps.
## <a name="consumption-pricing-model"></a>Prijsmodel voor verbruik
U betaalt alleen voor wat u gebruikt met gemaakte logische apps. Nieuwe logische apps gebruiken een verbruik plannings- en prijzen model, wat betekent dat alle uitvoeringen uitgevoerd door een logische app-exemplaar zijn gemeten.
### <a name="what-are-action-executions"></a>Wat zijn actie uitvoeringen?
Elke stap in de definitie van een logische app is een actie, waaronder triggers, stroom controlestappen aanroepen naar ingebouwde bewerkingen en aanroepen naar connectors.
### <a name="triggers"></a>Triggers
Triggers zijn speciale handelingen voor het maken van een logische app-exemplaar als een bepaalde gebeurtenis gebeurt. Triggers hebben verschillende andere gedrag dat van invloed zijn op hoe de logische app datalimiet geldt.
* **Polling-trigger** – deze trigger controleert voortdurend een eindpunt totdat deze een bericht dat ontvangt voldoet aan de criteria voor het maken van een logische app-exemplaar voor het starten van de werkstroom. U kunt het pollinginterval in de trigger via de Logic App-ontwerper instellen. Elke aanvraag polling telt als een uitvoering, zelfs wanneer er geen logic app-exemplaar wordt gemaakt.
* **Webhook trigger** – deze trigger wordt gewacht op een client een aanvraag te verzenden naar een specifieke eindpunt. Elke aanvraag verzonden naar het eindpunt van de webhook telt als de uitvoering van een actie. De aanvraag en HTTP-Webhook trigger zijn bijvoorbeeld beide webhook-triggers.
* **Terugkeerpatroon trigger** – deze trigger maakt een logische app-exemplaar op basis van het terugkeerpatroon die u hebt ingesteld in de trigger. U kunt bijvoorbeeld van een terugkeerpatroon trigger die wordt uitgevoerd om de drie dagen of op een meer complexe planning instellen.

Trigger uitvoeringen vindt u in het overzichtsvenster van uw logische app in de sectie Geschiedenis van de Trigger.

### <a name="actions"></a>Acties
Ingebouwde acties, bijvoorbeeld de acties die HTTP-, Azure Functions of API Management, evenals controlestappen van stroom aanroepen als systeemeigen acties worden gemeten en hun respectieve typen hebben. Acties die aanroepen [connectors](https://docs.microsoft.com/connectors) hebben van het type 'ApiConnection'. Connectors zijn geclassificeerd als standard of enterprise-connectors en zijn datalimiet op hun respectieve [prijzen][pricing].
Alle correct zijn en succes uitgevoerde acties worden geteld en datalimiet als actie-uitvoeringen. Acties die zijn overgeslagen, omdat hangt voorwaarden of acties die niet worden uitgevoerd, omdat de logische app beëindigd voordat de voltooid, tellen niet echter als actie-uitvoeringen. Uitgeschakelde logische apps kunnen geen exemplaar maken van nieuwe exemplaren, zodat ze niet in rekening gebracht als ze zijn uitgeschakeld.

> [!NOTE]
> Nadat u een logische app hebt uitgeschakeld, is de momenteel actieve exemplaren duurt even de tijd voordat ze volledig is gestopt.

Acties die worden uitgevoerd in lussen worden per elke cyclus in de lus meegeteld. Bijvoorbeeld de één actie in een lus 'voor elk' waarmee een lijst met 10 items worden verwerkt door het aantal items in lijst (10) vermenigvuldigen met het aantal acties in de lus (1) plus één voor het starten van de lus wordt geteld. Bijvoorbeeld, dus wordt de berekening (10 * 1) + 1, wat tot 11 actie uitvoeringen leidt.

### <a name="integration-account-usage"></a>Gebruik van het Account integratie
Gebruik op basis van verbruik bevat een [integratie account](logic-apps-enterprise-integration-create-integration-account.md) waar u kunt verkennen, ontwikkelen en testen van de [B2B/EDI](logic-apps-enterprise-integration-b2b.md) en [XML-verwerking](logic-apps-enterprise-integration-xml.md) functies van Logic Apps zonder extra kosten. U kunt een van deze integratieaccounts per regio en store maximaal 10 overeenkomsten en 25 maps hebben. U kunt hebt en onbeperkte partners, schema's en certificaten uploaden.

Logic Apps biedt ook de basis en standaard integratieaccounts met ondersteunde SLA van Logic Apps. U kunt basisintegratie accounts gebruiken wanneer u wilt gebruiken alleen bericht verwerken, of fungeren als een klein bedrijfspartner die een trading partner relatie met een grotere business-entiteit heeft. Standaard integratieaccounts complexere B2B relaties ondersteuning en verhoog het aantal entiteiten die u kunt beheren. Zie voor meer informatie [Azure-prijzen](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="pricing"></a>Prijzen
Zie voor meer informatie [Logic Apps prijzen](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Volgende stappen
* [Een overzicht van Logic Apps][whatis]
* [Uw eerste logische app maken][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-overview.md
[create]: quickstart-create-first-logic-app-workflow.md

