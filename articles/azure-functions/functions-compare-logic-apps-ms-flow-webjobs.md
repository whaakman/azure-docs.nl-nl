---
title: Kiezen tussen Flow, Logic Apps, Functions en WebJobs | Microsoft Docs
description: Vergelijk de cloudintegratieservices van Microsoft en bepaal welke service (s) u moet gebruiken.
services: functions,app-service\logic
documentationcenter: na
author: ggailey777
manager: wpickett
tags: 
keywords: microsoft flow, flow, logic apps, azure functions, functions, azure webjobs, webjobs, gebeurtenisverwerking, dynamische computing, serverloze architectuur
ms.assetid: e9ccf7ad-efc4-41af-b9d3-584957b1515d
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/03/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 7ffe44828735a5687008ebc5a7d8d9f017f49daa
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Kiezen tussen Stroom, Logische apps, Functies en WebJobs
In dit artikel worden de volgende services in de Microsoft-cloud met elkaar vergeleken. Dit zijn allemaal services die integratieproblemen kunnen oplossen en bedrijfsprocessen kunnen automatiseren:

* [Microsoft Flow](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure Functions](https://azure.microsoft.com/services/functions/)
* [Azure App Service WebJobs](../app-service/web-sites-create-web-jobs.md)

Al deze services zijn handig als u uiteenlopende systemen aan elkaar wilt 'lijmen'. Ze kunnen allemaal input, acties, voorwaarden en output definiëren. U kunt ze allemaal uitvoeren volgens een planning of na een trigger. Elke service heeft echter unieke voordelen en voor een juiste keuze is het dan ook onvoldoende om alleen de functies te vergelijken. De vraag is welke service het beste geschikt is voor een bepaalde situatie. Vaak is een combinatie van deze services de beste manier om snel een schaalbare integratieoplossing met alle mogelijke functionaliteit te bouwen.

<a name="flow"></a>

## <a name="flow-vs-logic-apps"></a>Flow vergeleken met Logic Apps
We kunnen Microsoft Flow en Azure Logic Apps samen bespreken omdat het allebei services zijn waarbij de *configuratie op de eerste plaats komt*. Deze services maken het eenvoudig om processen en werkstromen te bouwen en om te integreren met verschillende SaaS- en enterprise-toepassingen. 

* Flow is gebaseerd op Logic Apps
* Ze hebben dezelfde ontwerpfunctie voor werkstromen
* [Connectors](../connectors/apis-list.md) werken in beide services

Flow stelt kantoormedewerkers in staat om eenvoudige integraties tot stand te brengen (bijvoorbeeld een goedkeuringsproces voor een documentbibliotheek in SharePoint) zonder tussenkomst van ontwikkelaars of IT. Logic Apps daarentegen maakt het mogelijk om geavanceerde integraties te realiseren (zoals B2B-processen) waarvoor DevOps-procedures en beveiligingsprocessen vereist zijn. Het is normaal dat een werkstroom voor een bedrijfsproces na verloop van tijd toeneemt in complexiteit. U kunt daarom eerst met een stroom beginnen en deze later omzetten in een logische app als dat nodig is.

Raadpleeg de volgende tabel om te bepalen of Flow of Logic Apps het meest geschikt is voor een bepaalde integratie.

|  | Stroom | Logic Apps |
| --- | --- | --- |
| Doelgroep |Kantoorpersoneel, zakelijke gebruikers, SharePoint-beheerders |Professionele integrators en ontwikkelaars, IT-professionals |
| Scenario's |Selfservice |Geavanceerde integraties |
| Ontwerpomgeving |In browser en mobiele app, alleen UI |In browser en [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), [codeweergave](../logic-apps/logic-apps-author-definitions.md) beschikbaar |
| Beheer van toepassingslevenscyclus |Ontwerpen en testen in niet-productieomgevingen, niveau verhogen naar productie wanneer u klaar bent. |DevOps: resourcebeheer, testen, ondersteuning, automatisering en beheersbaarheid in [Azure Resource Management](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md) |
| Beheerervaring |Beleidsregels voor stroomomgevingen en preventie van gegevensverlies beheren, licenties bijhouden [https://admin.flow.microsoft.com](https://admin.flow.microsoft.com) |Resourcegroepen, verbindingen, toegangsbeheer en logboekregistratie beheren [https://portal.azure.com](https://portal.azure.com) |
| Beveiliging |Office 365-beveiliging en auditlogboeken voor naleving, preventie van gegevensverlies, [inactieve versleuteling](https://wikipedia.org/wiki/Data_at_rest#Encryption) voor gevoelige gegevens, etc. |Beveiligingsondersteuning van Azure: [Azure-beveiliging](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Security Center](https://azure.microsoft.com/services/security-center/), [auditlogboeken](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/) en meer. |

<a name="function"></a>

## <a name="functions-vs-webjobs"></a>Functions in vergelijking met Webtaken
We kunnen Azure Functions en Azure App Service WebJobs samen bespreken omdat het beide integratieservices zijn waarbij *code op de eerste plaats komt* en ze zijn ontworpen voor ontwikkelaars. De services maken het mogelijk om een script of bepaalde code uit te voeren in reactie op verschillende gebeurtenissen, zoals [nieuwe opslag-blobs](functions-bindings-storage.md) of [een WebHook-aanvraag](functions-bindings-http-webhook.md). Dit zijn de overeenkomsten tussen de services: 

* Beide services zijn gebaseerd op [Azure App Service](../app-service/app-service-web-overview.md) en ondersteunen functies zoals [resourcebeheer](../app-service/app-service-continuous-deployment.md), [verificatie](../app-service/app-service-authentication-overview.md) en [controle](../app-service/web-sites-monitor.md).
* Beide services zijn bedoeld voor ontwikkelaars.
* Beide services ondersteunen standaardscripts en -programmeertalen.
* Beide services ondersteunen NuGet en NPM.

Functions is de natuurlijke evolutie van WebJobs doordat deze service de beste eigenschappen van WebJobs als uitgangspunt heeft genomen voor verdere verbetering. Het gaat onder andere om deze verbeteringen: 

* Model met [serverloze apps](https://azure.microsoft.com/overview/serverless-computing/).
* Gestroomlijnde voorzieningen voor ontwikkelen, testen en uitvoeren van code, rechtstreeks in de browser.
* Ingebouwde integratie met meer Azure-services en services van derden, zoals [GitHub WebHooks](https://developer.github.com/webhooks/creating/).
* Betalen naar gebruik, u hoeft niet te betalen voor een [App Service-abonnement](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
* Automatische, [dynamische schaling](functions-scale.md).
* Voor bestaande klanten van App Service met een App Service-abonnement is het nog steeds mogelijk om gebruik te maken van resources die niet maximaal worden benut.
* Integratie met Logic Apps.

De volgende tabel geeft een overzicht van de verschillen tussen Functions en WebJobs:

|  | Functies | Webtaken |
| --- | --- | --- |
| Schalen |Zonder configuratie schalen |Schalen met App Service-abonnement |
| Prijzen |Betalen per gebruik of onderdeel van App Service-abonnement |Onderdeel van App Service-abonnement |
| Type uitvoering |Getriggerd, gepland (met timer-trigger) |Getriggerd, continu, gepland |
| Trigger-gebeurtenissen |[Timer](functions-bindings-timer.md), [Azure Cosmos DB](functions-bindings-cosmosdb.md), [Azure Event Hubs](functions-bindings-event-hubs.md), [HTTP/WebHook (GitHub, Slack)](functions-bindings-http-webhook.md), [Azure App Service Mobile Apps](functions-bindings-mobile-apps.md), [Azure Event Hubs](functions-bindings-event-hubs.md), [Azure Storage-wachtrijen en -blobs](functions-bindings-storage-blob.md), [Azure Service Bus-wachtrijen en -onderwerpen](functions-bindings-service-bus.md) |[Azure Storage-wachtrijen en -blobs](functions-bindings-storage-blob.md), [Azure Service Bus-wachtrijen en -onderwerpen](functions-bindings-service-bus.md) |
| Ontwikkeling in de browser |Ondersteund |Niet ondersteund |
| C# |Ondersteund |Ondersteund |
| F# |Ondersteund |Niet ondersteund |
| Javascript |Ondersteund |Ondersteund |
| Java |Preview | Niet ondersteund |
| Bash |Experimenteel |Ondersteund |
| Windows-scripts (.cmd en .bat) |Experimenteel |Ondersteund |
| PowerShell |Experimenteel |Ondersteund |
| PHP |Experimenteel |Ondersteund |
| Python |Experimenteel |Ondersteund |
| TypeScript |Experimenteel |Niet ondersteund |

Of u moet kiezen voor Functions of WebJobs hangt uiteindelijk af van wat u al doet met App Service. Als u een App Service-app hebt waarvoor u codefragmenten wilt uitvoeren en u deze gezamenlijk wilt beheren in dezelfde omgeving DevOps, kiest u voor WebJobs. In de volgende scenario's is het verstandig om voor Functions te kiezen.

* U wilt codefragmenten uitvoeren voor andere Azure-services of apps van derden.
* U wilt uw integratiecode afzonderlijk van uw App Service-apps beheren.
* U wilt uw codefragmenten aanroepen vanuit een logische app. 

<a name="together"></a>

## <a name="flow-logic-apps-and-functions-together"></a>Combinatie van Flow, Logic Apps en Functions
Zoals eerder gezegd, is uw situatie bepalend voor de service die het meest geschikt voor u is. 

* Voor niets meer dan optimalisatie van uw bedrijfsprocessen is Flow de aangewezen keuze.
* Als uw integratiescenario te geavanceerd is voor Flow, of als u DevOps-mogelijkheden nodig hebt, kiest u voor Logic Apps.
* Als voor een stap in uw integratiescenario aanzienlijk aangepaste transformatie of speciale code vereist is, schrijft u een functie en triggert u die functie als een actie in uw logische app.

U kunt een logische app aanroepen in een stroom. U kunt ook een functie aanroepen in een logische app, en een logische app in een functie. De integratie tussen Flow, Logic Apps en Functions wordt alleen maar beter. U kunt iets bouwen in de ene service en gebruiken in de andere services. Elke investering in deze drie technologieën is dan ook meer dan de moeite waard.

## <a name="next-steps"></a>Volgende stappen
Ontdek de mogelijkheden van deze services door uw eerste stroom, logische app, functie-app of webtaak te maken. Klik op een van de volgende koppelingen:

* [Aan de slag met Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Uw eerste Azure-functie maken](functions-create-first-azure-function.md)
* [WebJobs implementeren met Visual Studio](../app-service/websites-dotnet-deploy-webjobs.md)

Volg deze koppelingen voor meer informatie over deze integratieservices:

* [Leveraging Azure Functions & Azure App Service for integration scenarios door Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Integrations Made Simple door Charles Lamanna](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [Logic Apps Webcast](http://aka.ms/logicappslive)
* [Veelgestelde vragen over Microsoft Flow](https://flow.microsoft.com/documentation/frequently-asked-questions/)

