---
title: Azure Integration Services-referentiearchitectuur
description: Referentiearchitectuur waarin wordt getoond hoe voor het implementeren van een patroon van de enterprise-integratie met Logic Apps, API Management, Service Bus en Event Grid
author: mattfarm
manager: jonfan
editor: ''
services: logic-apps api-management
documentationcenter: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/15/2018
ms.author: LADocs; estfan
ms.openlocfilehash: 7d14bbd587b5086348026c41f6551b10809b939a
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37859639"
---
# <a name="enterprise-integration-with-queues-and-events-reference-architecture"></a>Bedrijfsintegratie met wachtrijen en -gebeurtenissen: referentie-architectuur

## <a name="overview"></a>Overzicht

Deze referentiearchitectuur toont een set bewezen procedures voor een integratietoepassing die gebruikmaakt van Azure Integration Services. Deze architectuur kan fungeren als deze basis van veel verschillende toepassingspatronen waarbij HTTP APIs, werkstroom en orchestration.

*Er zijn veel mogelijke toepassingen van technologie voor integratie van de toepassing van een eenvoudige punt-naar-punt met een volledige enterprise servicebus. Deze reeks architectuur stelt de herbruikbare onderdelen die mogelijk van toepassing voor het bouwen van een integratietoepassing algemene-architecten moeten rekening houden met welke specifieke onderdelen, moeten ze worden geïmplementeerd voor de toepassingen en infrastructuur.*

![Architectuurdiagram - bedrijfsintegratie met wachtrijen en -gebeurtenissen](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

## <a name="architecture"></a>Architectuur

Deze architectuur **bouwt voort op** de [eenvoudige bedrijfsintegratie](logic-apps-architectures-simple-enterprise-integration.md) architectuur. De [eenvoudige enterprise architectuur aanbevelingen](logic-apps-architectures-simple-enterprise-integration.md#recommendations) ook hier van toepassing, maar zijn weggelaten uit de [aanbevelingen](#recommendations) in dit document voor kort te houden. Deze heeft de volgende onderdelen:

- Resourcegroep. Een [resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) is een logische container voor Azure-resources.
- Azure API Management. [Met Azure API Management](https://docs.microsoft.com/azure/api-management/) is een volledig beheerd platform voor het publiceren, beveiligen en HTTP APIs transformeren.
- Portal voor ontwikkelaars van Azure API Management. Elk exemplaar van Azure API Management wordt geleverd met een [Ontwikkelaarsportal](https://docs.microsoft.com/azure/api-management/api-management-customize-styles), die toegang geeft tot documentatie, codevoorbeelden en de mogelijkheid voor het testen van een API.
- Azure Logic Apps. [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) is een serverloos platform voor het bouwen van enterprise-werkstroom en het integratieaccount.
- Connectors. [Connectors](https://docs.microsoft.com/azure/connectors/apis-list) worden gebruikt door Logic Apps verbinding maken met de meest gebruikte services. Logic Apps heeft al honderden verschillende connectors, maar ze kunnen ook worden gemaakt met behulp van een aangepaste connector.
- Azure Servicebus. [Service Bus](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview) biedt veilige en betrouwbare berichten. Berichten kunnen worden gebruikt voor het ongedaan maken Combineer toepassingen van elkaar en integreren met andere systemen op basis van een bericht.
- Azure Event Grid. [Event Grid](https://docs.microsoft.com/azure/event-grid/overview) een serverloos platform voor het publiceren en leveren van toepassingsgebeurtenissen is.
- IP-adres. De Azure API Management-service heeft een vaste openbare [IP-adres](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) en de domeinnaam van een. De domeinnaam is een subdomein van azure-api.net, zoals contoso.azure-api.net. Logic Apps en Service Bus hebt ook een openbare IP-adres. in deze architectuur beperken we echter toegang voor het aanroepen van Logic apps-eindpunten aan alleen de IP-adres van API Management (voor beveiliging). Aanroepen naar Service Bus worden beveiligd door een shared access signature.
- Azure DNS. [Azure DNS](https://docs.microsoft.com/azure/dns/) is een hostingservice voor DNS-domeinen die naamomzetting met behulp van Microsoft Azure-infrastructuur biedt. Door uw domeinen in Azure te hosten, kunt u uw DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering beheren als voor uw andere Azure-services. Maak DNS-records die de aangepaste domeinnaam aan het IP-adres toewijzen voor het gebruik van een aangepaste domeinnaam (zoals contoso.com). Zie voor meer informatie, een aangepaste domeinnaam in Azure API Management configureren.
- Azure Active Directory (Azure AD). Gebruik [Azure AD](https://docs.microsoft.com/azure/active-directory/) of een andere id-provider voor verificatie. Azure AD biedt verificatie voor toegang tot API-eindpunten (door door te geven een [JSON Web Token voor API Management](https://docs.microsoft.com/azure/api-management/policies/authorize-request-based-on-jwt-claims) valideren) en toegang tot de API Management-Ontwikkelaarsportal (alleen Standard en Premium-lagen) kunt beveiligen.

Deze architectuur heeft enkele fundamentele patronen op de werking ervan:

1. Bestaande back-end HTTP APIs worden gepubliceerd via de API Management Portal voor ontwikkelaars, zodat ontwikkelaars (ofwel intern gebruik binnen uw organisatie, externe of beide) aanroepen naar deze API's integreren in toepassingen.
2. Samengestelde API's zijn gebouwd met behulp van Logic Apps; aanroepen van SAAS-systemen organiseren, Azure-services en API's gepubliceerd op API Management. De [Logic Apps worden ook gepubliceerd](https://docs.microsoft.com/azure/api-management/import-logic-app-as-api) via de API Management-Ontwikkelaarsportal.
3. Toepassingen [verkrijgen van een OAuth 2.0-beveiligingstoken](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) die nodig zijn voor het verkrijgen van toegang tot een API met behulp van Azure Active Directory.
4. Met Azure API Management [valideert het beveiligingstoken](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad), en wordt de aanvraag doorgegeven aan de back-end API/logische App.
5. Service Bus-wachtrijen worden gebruikt om [loskoppelen](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) activiteit van de toepassing en [pieken in de belasting vloeiende](https://docs.microsoft.com/azure/architecture/patterns/queue-based-load-leveling). Berichten die worden toegevoegd aan de wachtrijen door Logic Apps, 3rd party toepassingen, of (niet afgebeeld) door het publiceren van de wachtrij als een HTTP-API via API Management.
6. Wanneer berichten worden toegevoegd aan een Service Bus-wachtrij, wordt een gebeurtenis wordt geactiveerd. Een logische App wordt geactiveerd door deze gebeurtenis en verwerkt het bericht.
7. Andere Azure-services (bijvoorbeeld Blob Storage, Event Hub) worden op dezelfde manier ook de gebeurtenissen publiceren naar Event Grid. Deze trigger voor logische Apps voor het ontvangen van de gebeurtenis en latere acties uitvoeren.

## <a name="recommendations"></a>Aanbevelingen

Uw specifieke vereisten kunnen afwijken van de algemene architectuur die hier worden beschreven. Gebruik de aanbevelingen in deze sectie als uitgangspunt.

### <a name="service-bus-tier"></a>Service Bus-laag

Service Bus premium-laag gebruiken zoals dit event grid-meldingen op dit moment ondersteunt (ondersteuning voor alle lagen wordt verwacht). Zie [prijzen van Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Prijzen van Event Grid

Event Grid werkt met behulp van een model zonder server – facturering wordt berekend op basis van aantal bewerkingen (gebeurtenis uitvoeren). Zie [prijzen van Event Grid](https://azure.microsoft.com/pricing/details/event-grid/) voor meer informatie. Er zijn momenteel geen laag overwegingen voor Event Grid.

### <a name="use-peeklock-when-consuming-service-bus-messages"></a>PeekLock gebruiken bij het gebruiken van Service Bus-berichten

Gebruik bij het maken van logische Apps gebruiken van Service Bus-berichten [PeekLock](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#queues) in de logische App voor toegang tot een groep van berichten. De logische App kunt vervolgens stappen uitvoeren om elk bericht valideren voordat deze wordt afgebroken of annuleren. Deze aanpak worden beschermd tegen onbedoeld bericht verlies.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Controleren op meerdere objecten bij een Event Grid-trigger wordt geactiveerd

Event Grid wordt geactiveerd betekent het volgende die 'ten minste 1 van deze dingen gebeurd' geactiveerd. Bijvoorbeeld, wanneer Event Grid wordt geactiveerd met een logische app op een bericht wordt weergegeven in een Service Bus-wachtrij, de logische app moet altijd wordt ervan uitgegaan dat er mogelijk een of meer berichten beschikbaar om te verwerken.

### <a name="region"></a>Regio

Richt API Management, Logic Apps en Service Bus in dezelfde regio voor minimale netwerklatentie. In het algemeen kiest u de regio die het meest in de buurt van uw gebruikers is.

De resourcegroep heeft ook een regio, dat waar de metagegevens voor de implementatie is opgeslagen aangeeft, en waar de sjabloon voor de implementatie is uitgevoerd. Plaats de resourcegroep en de bijbehorende resources in dezelfde regio. Dit kan de beschikbaarheid verbeteren tijdens de implementatie.

## <a name="scalability-considerations"></a>Schaalbaarheidsoverwegingen

Azure Service Bus premium-laag kunt scale-out het aantal messaging-eenheden voor hogere schaalbaarheid. Premium kan 1, 2 of 4 messaging-eenheden hebben. Zie voor verdere instructies over het schalen van Azure Service Bus [aanbevolen procedures voor prestatieverbeteringen met Service Bus-berichten](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability-considerations"></a>Beschikbaarheidsoverwegingen

Op het moment van schrijven is de service level agreement (SLA) voor Azure API Management 99,9% voor de lagen Basic, Standard en Premium. Premium-laag configuraties met implementatie van ten minste één eenheid in twee of meer regio's hebben een SLA van 99,95%.

Op het moment van schrijven is de service level agreement (SLA) voor Azure Logic Apps 99,9%.

### <a name="disaster-recovery"></a>Herstel na noodgeval

Overweeg de implementatie van Geo-noodherstel van Service Bus premium om in te schakelen van failover in geval van een ernstige uitval. Meer informatie over [Geo-noodherstel van Azure Service Bus](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability-considerations"></a>Beheerbaarheidsoverwegingen

Maak afzonderlijke resourcegroepen voor productie, ontwikkeling en testomgevingen. Hiermee kunt u eenvoudiger implementaties beheren, testimplementaties verwijderen en toegangsrechten verlenen.
Wanneer u resources toewijst aan resourcegroepen, moet u het volgende overwegen:

- Levenscyclus. In het algemeen kunt u resources met dezelfde levenscyclus het beste in dezelfde resourcegroep onderbrengen.
- Toegang. U kunt [Role-Based Access Control](../role-based-access-control/overview.md) (RBAC) om toe te passen van beleidsregels voor toegang tot de resources in een groep.
- Facturering. U kunt de samengevoegde kosten voor de resourcegroep bekijken.
- Prijscategorie voor API Management – wordt u aangeraden Developer-laag voor ontwikkel- en testomgevingen. Voor Pre-productie, wordt aangeraden een replica van de productie-omgeving implementeren tests uitvoert en vervolgens naar de kosten minimaliseren wordt afgesloten.

Zie voor meer informatie, [resourcegroep](../azure-resource-manager/resource-group-overview.md) overzicht.

### <a name="deployment"></a>Implementatie

Het is raadzaam dat u [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md) Azure API Management, Logic Apps, Event Grid en Service Bus te implementeren. Sjablonen wordt het eenvoudiger implementaties automatiseren via PowerShell of de Azure-opdrachtregelinterface (CLI).

Het is raadzaam om de toevoeging van Azure API Management, een afzonderlijke Logic Apps, Event Grid-onderwerpen en Service Bus-naamruimten in hun eigen afzonderlijke Resource Manager-sjablonen. Hierdoor kunnen opslaat in bronbeheersystemen. Deze sjablonen kunnen vervolgens worden geïmplementeerd samen of afzonderlijk als onderdeel van een continue integratie/continue (CI/CD)-implementatieproces.

### <a name="diagnostics-and-monitoring"></a>Diagnose en controle

Service Bus, zoals API Management en Logic Apps kan worden gecontroleerd met behulp van Azure Monitor. Azure Monitor is standaard ingeschakeld en biedt informatie op basis van de verschillende metrische gegevens voor elke service is geconfigureerd.

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Service Bus via een handtekening voor gedeelde toegang beveiligen. [SAS-verificatie](../service-bus-messaging/service-bus-sas.md) kunt u een gebruiker toegang tot Service Bus-bronnen, met specifieke rechten. Meer informatie over [Service Bus-verificatie en autorisatie](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Daarnaast moet een Service Bus-wachtrij moet worden weergegeven als een HTTP-eindpunt (voor boeken van nieuwe berichten), API Management te beveiligen door het eindpunt fronting moet worden gebruikt. Dit kan vervolgens worden beveiligd met certificaten of OAuth waar nodig. De eenvoudigste manier om dit te doen is een logische App met een verzoek/reactie-HTTP-trigger gebruikt als een intermediair.

Event Grid beveiligt bezorging van gebeurtenissen via een code voor validatie. Als u LogicApps gebruiken voor de gebeurtenis, wordt dit automatisch uitgevoerd. Zie meer informatie over [Event Grid-beveiliging en verificatie](../event-grid/security-authentication.md).

## <a name="next-steps"></a>Volgende stappen

- [Eenvoudige Enterprise Integration](logic-apps-architectures-simple-enterprise-integration.md)