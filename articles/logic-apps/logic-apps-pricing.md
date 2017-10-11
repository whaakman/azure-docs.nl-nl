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
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.openlocfilehash: 63784c5e3af360b2f3f8cb330a9df8b27a85d859
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="logic-apps-pricing-model"></a>Prijsmodel voor logische apps
Logische Apps van Azure kunt u om te schalen en integratie-werkstromen worden uitgevoerd in de cloud.  Hieronder vindt u meer informatie over de facturering en prijsstelling voor Logic Apps.
## <a name="consumption-pricing"></a>Prijzen voor verbruik
Nieuw gemaakte Logic Apps gebruiken voor een plan verbruik. Met het verbruik prijsmodel van Logic Apps betaalt u alleen voor wat u gebruikt.  Logische Apps zijn niet beperkt wanneer u een plan verbruik.
Alle acties die worden uitgevoerd in een uitvoering van een logische app-exemplaar zijn gemeten.
### <a name="what-are-action-executions"></a>Wat zijn actie uitvoeringen?
Elke stap in de definitie van een logische app is een actie, waaronder de triggers, Controlestroom stappen zoals voorwaarden, scopes voor elke lussen totdat lussen, aanroepen van connectors doen en aanroepen naar systeemeigen acties.
Triggers zijn speciale handelingen die zijn ontworpen voor een nieuwe instantie van een logische app wanneer een bepaalde gebeurtenis plaatsvindt.  Er zijn diverse verschillende gedragingen voor triggers kunnen beïnvloeden hoe de logische app datalimiet geldt.
* **Polling-trigger** – deze trigger controleert voortdurend een eindpunt totdat er een bericht dat voldoet aan de criteria ontvangt voor het maken van een exemplaar van een logische app.  Het polling-interval kan worden geconfigureerd in de trigger in de ontwerpfunctie voor Logic App.  Elke aanvraag polling, telt zelfs als dit niet geen exemplaar van een logische app maken, als de uitvoering van een actie.
* **Webhook trigger** – deze trigger wordt gewacht op een client een aanvraag voor een bepaald eindpunt verzenden.  Elke aanvraag verzonden naar het eindpunt van de webhook telt als de uitvoering van een actie. De aanvraag en de HTTP-Webhook-trigger zijn beide webhook-triggers.
* **Terugkeerpatroon trigger** – deze trigger maakt een exemplaar van de logische app op basis van het terugkeerpatroon die is geconfigureerd in de trigger.  Een trigger terugkeerpatroon kan bijvoorbeeld worden geconfigureerd om te worden uitgevoerd om de drie dagen of zelfs elke minuut.

Trigger uitvoeringen zichtbaar in de resourceblade van logische Apps in de geschiedenis van de Trigger.

Alle acties die zijn uitgevoerd, of ze zijn geslaagd of mislukt als de uitvoering van een actie zijn datalimiet.  Acties die zijn overgeslagen als gevolg van een voorwaarde die niet wordt voldaan aan of acties die niet worden uitgevoerd omdat de logische app beëindigd voordat de voltooid, worden niet meegeteld als actie-uitvoeringen.

Acties die worden uitgevoerd binnen lussen worden per herhaling van de lus meegeteld.  Bijvoorbeeld één actie in een voor elke lus doorloopt een lijst met 10 items worden geteld als het aantal items in de lijst (10) vermenigvuldigd met het aantal acties in de lus (1) plus één voor de initialisatie van de lus , die in dit voorbeeld zou zijn (10 * 1) + 1 = 11 actie uitvoeringen.
Uitgeschakelde Logic Apps kunnen geen nieuwe exemplaren geïnstantieerd, en daarom tijdens uitgeschakeld, worden niet in rekening gebracht.  Worden gelet dat na het uitschakelen van een logische app even de tijd voor de exemplaren stilleggen duurt voordat volledig wordt uitgeschakeld.
### <a name="integration-account-usage"></a>Gebruik van het Account integratie
Opgenomen in gebruik op basis van verbruik is een [integratie account](logic-apps-enterprise-integration-create-integration-account.md) voor onderzoek, ontwikkeling en zodat u kunt gebruiken voor testdoeleinden de [B2B/EDI](logic-apps-enterprise-integration-b2b.md) en [XML-verwerking](logic-apps-enterprise-integration-xml.md)functies van Logic Apps zonder extra kosten. U bent kunnen maximaal één account per regio en store maximaal 10 overeenkomsten en 25 maps maken. Schema's, certificaten en partners hebben geen limieten en u zoveel naar wens kunt uploaden.

Naast het opnemen van integratieaccounts het energieverbruik, kunt u ook standaard integratieaccounts zonder deze beperkingen en met onze standaard Logic Apps SLA maken. Zie voor meer informatie [Azure-prijzen](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="app-service-plans"></a>App Service-abonnementen
Logische apps eerder hebt gemaakt die verwijzen naar een App Service-Plan blijft zich gedragen als voorheen. Afhankelijk van het plan dat is gekozen, worden beperkt na de voorgeschreven dagelijkse uitvoeringen is overschreden, maar worden gefactureerd met behulp van de actie uitvoeren meter.
EA klanten met een App Service-Plan in hun abonnement dat niet hoeft te worden expliciet gekoppeld aan de logische App, beschikken over de handige opgenomen hoeveelheden.  Bijvoorbeeld, als er een Standard-App Service-Plan in uw abonnement EA en een logische App in hetzelfde abonnement vervolgens u niet in rekening gebracht 10.000 actie uitvoeringen per dag (Zie de volgende tabel). 

App Service-abonnementen en hun dagelijkse toegestane actie uitvoeringen:
|  | Gratis en gedeeld/eenvoudige | Standard | Premium |
| --- | --- | --- | --- |
| Actie-uitvoeringen per dag |200 |10.000 |50,000 |
### <a name="convert-from-app-service-plan-pricing-to-consumption"></a>Converteren van App Service-Plan prijzen verbruik
Om te wijzigen van een logische App met een App Service-abonnement gekoppeld aan een model verbruik, verwijder de verwijzing naar de App Service-Plan in de definitie van logische Apps.  Deze wijziging kunt u doen met een aanroep van een PowerShell-cmdlet:`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`
## <a name="pricing"></a>Prijzen
Zie voor prijsinformatie, [Logic Apps prijzen](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Volgende stappen
* [Een overzicht van Logic Apps][whatis]
* [Uw eerste logische app maken][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-what-are-logic-apps.md
[create]: logic-apps-create-a-logic-app.md

