---
title: Kiezen tussen Flow, Logic Apps, Functions en WebJobs | Microsoft Docs
description: Vergelijk de cloudintegratieservices van Microsoft en bepaal welke service (s) u moet gebruiken.
services: functions,app-service\logic
documentationcenter: na
author: tdykstra
manager: wpickett
tags: ''
keywords: microsoft flow, flow, logic apps, azure functions, functions, azure webjobs, webjobs, gebeurtenisverwerking, dynamische computing, serverloze architectuur
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/20/2018
ms.author: tdykstra
ms.custom: mvc
ms.openlocfilehash: 577031c58e95781dc97721acc71fb22114b1c606
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Kiezen tussen Stroom, Logische apps, Functies en WebJobs

In dit artikel worden de volgende services in Microsoft Cloud vergeleken:

* [Microsoft Flow](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure Functions](https://azure.microsoft.com/services/functions/)
* [Azure App Service WebJobs](../app-service/web-sites-create-web-jobs.md)

Al deze services kunnen integratieproblemen oplossen en bedrijfsprocessen automatiseren. Ze kunnen allemaal input, acties, voorwaarden en output definiëren. U kunt ze allemaal uitvoeren volgens een planning of na een trigger. Elke service biedt echter unieke voordelen, en in dit artikel worden de verschillen uitgelegd.

## <a name="flow-vs-logic-apps"></a>Flow vergeleken met Logic Apps

Microsoft Flow en Azure Logic Apps zijn allebei integratieservices waarbij *de configuratie op de eerste plaats komt*. Beide maken werkstromen die zijn geïntegreerd met verschillende SaaS- en bedrijfs-apps. 

Flow is gebaseerd op Logic Apps. Ze hebben dezelfde ontwerpfunctie voor werkstromen en dezelfde [connectors](../connectors/apis-list.md). 

Flow stelt kantoormedewerkers in staat om eenvoudige integraties tot stand te brengen (bijvoorbeeld een goedkeuringsproces voor een documentbibliotheek in SharePoint) zonder tussenkomst van ontwikkelaars of IT. Logic Apps daarentegen maakt het mogelijk om geavanceerde integraties te realiseren (zoals B2B-processen) waarvoor DevOps-procedures en beveiligingsprocessen vereist zijn. Het is normaal dat een werkstroom voor een bedrijfsproces na verloop van tijd toeneemt in complexiteit. U kunt daarom eerst met een stroom beginnen en deze later omzetten in een logische app als dat nodig is.

Raadpleeg de volgende tabel om te bepalen of Flow of Logic Apps het meest geschikt is voor een bepaalde integratie.

|  | Stroom | Logic Apps |
| --- | --- | --- |
| Gebruikers |Kantoorpersoneel, zakelijke gebruikers, SharePoint-beheerders |Professionele integrators en ontwikkelaars, IT-professionals |
| Scenario's |Selfservice |Geavanceerde integraties |
| Ontwerpomgeving |In browser en mobiele app, alleen UI |In browser en [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), [codeweergave](../logic-apps/logic-apps-author-definitions.md) beschikbaar |
| Beheer van toepassingslevenscyclus |Ontwerpen en testen in niet-productieomgevingen, niveau verhogen naar productie wanneer u klaar bent. |DevOps: resourcebeheer, testen, ondersteuning, automatisering en beheersbaarheid in [Azure Resource Management](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md) |
| Beheerervaring |Beleidsregels voor stroomomgevingen en preventie van gegevensverlies beheren, licenties bijhouden [https://admin.flow.microsoft.com](https://admin.flow.microsoft.com) |Resourcegroepen, verbindingen, toegangsbeheer en logboekregistratie beheren [https://portal.azure.com](https://portal.azure.com) |
| Beveiliging |Office 365-beveiliging en auditlogboeken voor naleving, preventie van gegevensverlies, [inactieve versleuteling](https://wikipedia.org/wiki/Data_at_rest#Encryption) voor gevoelige gegevens, etc. |Beveiligingsondersteuning van Azure: [Azure-beveiliging](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Security Center](https://azure.microsoft.com/services/security-center/), [auditlogboeken](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/) en meer. |

<a name="function"></a>

## <a name="functions-vs-webjobs"></a>Functions in vergelijking met Webtaken

Azure App Service WebJobs met de WebJobs SDK is net als Azure Functions een integratieservice *waarbij code op de eerste plaats komt* en die is ontworpen voor ontwikkelaars. Beide zijn gebaseerd op [Azure App Service](../app-service/app-service-web-overview.md) en ondersteunen functies zoals [integratie van broncodebeheer](../app-service/app-service-continuous-deployment.md), [verificatie](../app-service/app-service-authentication-overview.md) en [bewaking met Application Insights-integratie](functions-monitoring.md).

### <a name="webjobs-vs-the-webjobs-sdk"></a>WebJobs vergeleken met de WebJobs SDK

Met de *WebJobs*-functie van App Service kunt u een script of code uitvoeren in de context van een App Service-web-app. De *WebJobs SDK* is een framework dat is ontworpen voor WebJobs dat de code vereenvoudigt die u schrijft om te reageren op gebeurtenissen in Azure-services. U kunt bijvoorbeeld reageren op een afbeeldingsblob die in Azure Storage is gemaakt door een miniatuurafbeelding te maken. De WebJobs SDK wordt uitgevoerd als een .NET-consoletoepassing die u op een WebJob kunt implementeren. 

WebJobs en de WebJobs SDK werken samen het beste, maar u kunt WebJobs ook zonder de WebJobs SDK gebruiken en vice versa. Een WebJob kan elk programma of script uitvoeren dat in de App Service-sandbox kan worden uitgevoerd. Een WebJobs SDK-consoletoepassing kan overal waar consoletoepassingen worden uitgevoerd, zoals op on-premises servers, worden uitgevoerd.

### <a name="comparison-table"></a>Vergelijkingstabel

Azure Functions is gebaseerd op de WebJobs SDK en deelt daarom veel triggers en verbindingen met andere Azure-services. Hier volgt een aantal factoren om te overwegen bij de keuze tussen Azure Functions en WebJobs met de WebJobs SDK:

|  | Functies | WebJobs met WebJobs SDK |
| --- | --- | --- |
|[App-model zonder server](https://azure.microsoft.com/overview/serverless-computing/) met [automatisch schalen](functions-scale.md#how-the-consumption-plan-works)|✔||
|[Ontwikkelen en testen in de browser](functions-create-first-azure-function.md) |✔||
|[Betalen per gebruik](functions-scale.md#consumption-plan)|✔||
|[Integratie met Logic Apps](functions-twitter-email.md)|✔||
| Trigger-gebeurtenissen |[Timer](functions-bindings-timer.md)<br>[Azure Storage-wachtrijen en -blobs](functions-bindings-storage-blob.md)<br>[Azure Service Bus-wachtrijen en -onderwerpen](functions-bindings-service-bus.md)<br>[Azure Cosmos DB](functions-bindings-cosmosdb.md)<br>[Azure Event Hubs](functions-bindings-event-hubs.md)<br>[HTTP/WebHook (GitHub, Slack)](functions-bindings-http-webhook.md)<br>[Azure Event Grid](functions-bindings-event-grid.md)|[Timer](functions-bindings-timer.md)<br>[Azure Storage-wachtrijen en -blobs](functions-bindings-storage-blob.md)<br>[Azure Service Bus-wachtrijen en -onderwerpen](functions-bindings-service-bus.md)<br>[Azure Cosmos DB](functions-bindings-cosmosdb.md)<br>[Azure Event Hubs](functions-bindings-event-hubs.md)<br>[Bestandssysteem](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Files/FileTriggerAttribute.cs)|
| Ondersteunde talen  |C#<br>F#<br>Javascript<br>Java (preview) |C#<sup>1</sup>|
|Pakketbeheer|NPM en NuGet|NuGet<sup>2</sup>|

<sup>1</sup> WebJobs (zonder WebJobs SDK) biedt ondersteuning voor C#, JavaScript, Bash, .cmd, .bat, PowerShell, PHP, TypeScript, Python en meer. Dit is geen volledige lijst; een WebJob kan elk programma of script uitvoeren dat in de App Service-sandbox kan worden uitgevoerd.

<sup>2</sup> WebJobs (zonder de WebJobs SDK) ondersteunt NPM en NuGet.

### <a name="summary"></a>Samenvatting

Azure Functions biedt een hogere productiviteit voor ontwikkelaars, meer opties voor programmeertaal, meer opties voor ontwikkelomgeving, meer opties voor integratie met Azure services en meer prijsopties. Deze oplossing is voor de meeste scenario's de beste keuze.

Hier volgen twee scenario's waarvoor WebJobs de beste keuze kan zijn:

* U hebt meer controle nodig over de code die naar gebeurtenissen luistert, het `JobHost`-object. Functions biedt een beperkt aantal manieren om `JobHost`-gedrag aan te passen in het [host.json](functions-host-json.md)-bestand. Soms moet u acties uitvoeren die niet met een tekenreeks in een JSON-bestand kunnen worden opgegeven. U kunt bijvoorbeeld alleen met de WebJobs SDK een aangepast beleid voor opnieuw proberen voor Azure Storage configureren.
* U hebt een App Service-app waarvoor u codefragmenten wilt uitvoeren en u wilt deze gezamenlijk beheren in dezelfde DevOps-omgeving.

Voor andere scenario's waarin u codefragmenten wilt uitvoeren voor de integratie van Azure-services of services van derden, kiest u Azure Functions in plaats van WebJobs en de WebJobs SDK.

<a name="together"></a>

## <a name="flow-logic-apps-functions-and-webjobs-together"></a>Flow, Logic Apps, Functions en WebJobs samen

U hoeft niet maar één van deze services te kiezen; ze kunnen met elkaar en met externe services worden geïntegreerd.

Een stroom kan een logische app aanroepen. Een logische app kan een functie aanroepen en een functie kan een logische app aanroepen. Zie bijvoorbeeld [Een functie maken die kan worden geïntegreerd met Azure Logic Apps](functions-twitter-email.md).

De integratie tussen Flow, Logic Apps en Functions wordt alleen maar beter. U kunt iets bouwen in de ene service en gebruiken in de andere services.

## <a name="next-steps"></a>Volgende stappen

Begin met het maken van uw eerste stroom, logische app of functie-app. Klik op een van de volgende koppelingen:

* [Aan de slag met Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Uw eerste Azure-functie maken](functions-create-first-azure-function.md)

Volg deze koppelingen voor meer informatie over deze integratieservices:

* [Leveraging Azure Functions & Azure App Service for integration scenarios door Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Integrations Made Simple door Charles Lamanna](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [Logic Apps Webcast](http://aka.ms/logicappslive)
* [Veelgestelde vragen over Microsoft Flow](https://flow.microsoft.com/documentation/frequently-asked-questions/)
