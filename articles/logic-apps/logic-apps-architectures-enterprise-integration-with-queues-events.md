---
title: Enterprise integration-architectuurpatroon - Azure Integration Services
description: De verwijzing van deze architectuur laat zien hoe u een patroon van de enterprise-integratie met Azure Logic Apps, Azure API Management, Azure Service Bus en Azure Event Grid kunt implementeren
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: mattfarm
ms.author: mattfarm
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/15/2018
ms.openlocfilehash: 5b58f4b71d8d9f3ca91d8cefc6215073fd836765
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093663"
---
# <a name="enterprise-integration-architecture-with-queues-and-events"></a>Architectuur van de Enterprise-integratie met wachtrijen en -gebeurtenissen

Dit artikel beschrijft de architectuur van een enterprise-integratie die gebruikmaakt van bewezen procedures die u op een toepassing voor gegevensintegratie toepassen kunt bij het gebruik van Azure Integration Services. U kunt deze architectuur gebruiken als basis voor veel verschillende toepassingspatronen waarvoor HTTP APIs, werkstromen en orchestration.

![Architectuurdiagram - bedrijfsintegratie met wachtrijen en -gebeurtenissen](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

Deze reeks wordt uitgelegd dat de herbruikbare onderdelen die van toepassing zijn mogelijk voor het bouwen van een algemene integratietoepassing. Omdat technologie voor integratie veel mogelijke toepassingen heeft, variërend van eenvoudige point-to-point apps tot volledige enterprise Azure Service Bus-apps, kunt u welke onderdelen die u wilt implementeren voor uw apps en infrastructuur.

## <a name="architecture-components"></a>Architectuur van onderdelen

Deze architectuur is gebaseerd op de architectuur die worden beschreven in het artikel [architectuur verwijzing: eenvoudige bedrijfsintegratie](/azure/architecture/reference-architectures/enterprise-integration/simple-enterprise-integration). Die architectuur van [aanbevelingen](/azure/architecture/reference-architectures/enterprise-integration/simple-enterprise-integration#recommendations) zijn hier ook van toepassing, maar beknopt alternatief in dit artikel worden weggelaten de aanbevelingen van de [aanbevelingen](#recommendations) sectie. Deze enterprise integration-architectuur bevat de volgende onderdelen:

- **Resourcegroep**: A [resourcegroep](../azure-resource-manager/resource-group-overview.md) is een logische container voor Azure-resources.

- **Met Azure API Management**: de [API Management](https://docs.microsoft.com/azure/api-management/) -service is een volledig beheerd platform voor het publiceren, te beveiligen en te transformeren, HTTP APIs.

- **Azure API Management-ontwikkelaarsportal**: elk exemplaar van Azure API Management biedt toegang tot de [ontwikkelaarsportal](../api-management/api-management-customize-styles.md). Deze portal geeft u toegang tot documentatie en codevoorbeelden. U kunt ook API's testen in de portal voor ontwikkelaars.

- **Met Azure Logic Apps**: [Logic Apps](../logic-apps/logic-apps-overview.md) is een serverloos platform voor het bouwen van enterprise-werkstromen en integraties.

- **Connectors**: maakt gebruik van Logic Apps [connectors](../connectors/apis-list.md) voor verbinding maken met veel services gebruikt. Logic Apps biedt honderden connectors, maar u kunt ook een aangepaste connector maken.

- **Azure Service Bus**: [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) biedt veilige en betrouwbare berichten. U kunt gebruiken voor ontkoppeling van toepassingen en het integreren met andere systemen op basis van een bericht-berichten.

- **Azure Event Grid**: [Event Grid](../event-grid/overview.md) een serverloos platform voor het publiceren en leveren van toepassingsgebeurtenissen is.

- **IP-adres**: de Azure API Management-service heeft een vaste openbare [IP-adres](../virtual-network/virtual-network-ip-addresses-overview-arm.md) en de domeinnaam van een. De standaarddomeinnaam is een subdomein van azure-api.net, bijvoorbeeld contoso.azure-api.net, maar u kunt ook configureren [aangepaste domeinen](../api-management/configure-custom-domain.md). Logic Apps en Service Bus moet u ook een openbaar IP-adres hebben. Voor beveiliging, echter beperkt deze architectuur toegang voor het aanroepen van Logic Apps-eindpunten alleen het IP-adres van API Management. Aanroepen naar Service Bus worden beveiligd door een shared access signature (SAS).

- **Azure DNS**: [Azure DNS](https://docs.microsoft.com/azure/dns/) is een hostingservice voor DNS-domeinen. Azure DNS biedt naamomzetting met behulp van de Microsoft Azure-infrastructuur. Door uw domeinen in Azure hosten, kunt u uw DNS-records beheren met behulp van dezelfde referenties, API's, hulpprogramma's en facturering, gebruikt u voor uw andere Azure-services. Maak DNS-records die de aangepaste domeinnaam aan het IP-adres toewijzen voor het gebruik van een aangepaste domeinnaam, zoals contoso.com. Zie voor meer informatie, [een aangepaste domeinnaam configureren in API Management](../api-management/configure-custom-domain.md).

- **Azure Active Directory (Azure AD)**: U kunt [Azure AD](https://docs.microsoft.com/azure/active-directory/) of een andere id-provider voor verificatie. Azure AD-verificatie voor toegang tot API-eindpunten door door te geven biedt een [JSON Web Token voor API Management](../api-management/policies/authorize-request-based-on-jwt-claims.md) om te valideren. Voor de lagen standaard en Premium, kunt Azure AD beveiligen van toegang tot de API Management-ontwikkelaarsportal.

## <a name="patterns"></a>Patronen 

Deze architectuur maakt gebruik van sommige patronen die essentieel voor bewerking zijn:

* Bestaande back-end-HTTP-API's worden gepubliceerd via de API Management-ontwikkelaarsportal. In de portal voor ontwikkelaars, die zijn intern gebruik binnen uw organisatie en/of externe  
kan aanroepen naar deze API's integreren in toepassingen.

* Samengestelde API's zijn gebouwd met behulp van logic apps, die het organiseren van aanroepen naar software als een service (SaaS)-systemen, Azure-services en API's die worden gepubliceerd naar API Management. Logische apps zijn ook [gepubliceerd via de API Management-ontwikkelaarsportal](../api-management/import-logic-app-as-api.md).

* Toepassingen gebruiken Azure AD voor [verkrijgen van een OAuth 2.0-beveiligingstoken](../api-management/api-management-howto-protect-backend-with-aad.md) die is vereist voor toegang tot een API.

* Met Azure API Management [valideert het beveiligingstoken](../api-management/api-management-howto-protect-backend-with-aad.md) en geeft vervolgens de aanvraag door naar de back-end-API of de logische app.

* Azure Service Bus-wachtrijen worden gebruikt voor [ontkoppeling](../service-bus-messaging/service-bus-messaging-overview.md) activiteit van de toepassing en voor [pieken in de load-aanpassing](https://docs.microsoft.com/azure/architecture/patterns/queue-based-load-leveling). Berichten die worden toegevoegd aan de wachtrijen door logic apps, apps van derden, of (niet weergegeven) door het publiceren van de wachtrij als een HTTP-API via API Management.

* Wanneer berichten worden toegevoegd aan een Service Bus-wachtrij, wordt een gebeurtenis wordt geactiveerd. De gebeurtenis wordt geactiveerd voor een logische app, die het bericht vervolgens verwerkt.

* Andere Azure-services zoals Azure Blob Storage en Azure Event Hubs, worden ook gebeurtenissen publiceren naar Event Grid. Deze services-trigger voor logische apps voor het ontvangen van de gebeurtenis en vervolgens de volgende acties uitvoeren.

## <a name="recommendations"></a>Aanbevelingen

Uw specifieke vereisten kunnen afwijken van de algemene architectuur die in dit artikel wordt beschreven. Gebruik de aanbevelingen in deze sectie als uitgangspunt.

### <a name="service-bus-tier"></a>Service Bus-laag

Gebruik de Service Bus Premium-laag, die ondersteuning biedt voor Event Grid-meldingen. Zie voor meer informatie, [prijzen van Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Prijzen van Event Grid

Event Grid maakt gebruik van een model zonder server. Facturering wordt berekend op basis van het aantal bewerkingen (gebeurtenis uitvoeringen). Zie voor meer informatie, [prijzen van Event Grid](https://azure.microsoft.com/pricing/details/event-grid/). Er zijn momenteel geen laag overwegingen voor Event Grid.

### <a name="use-peeklock-to-consume-service-bus-messages"></a>PeekLock gebruiken om te gebruiken van Service Bus-berichten

Wanneer u een logische app gebruiken voor Service Bus-berichten maakt, hebt u uw logische app gebruiken [PeekLock](../service-bus-messaging/service-bus-messaging-overview.md#queues) voor toegang tot een groep van berichten. Wanneer u PeekLock gebruikt, kan de logische app stappen voor het valideren van elk bericht voor het voltooien van of het bericht wordt afgebroken kunt uitvoeren. Deze aanpak worden beschermd tegen onbedoeld bericht verlies.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Controleren op meerdere objecten bij een Event Grid-trigger wordt geactiveerd

Wanneer een Event Grid-trigger wordt geactiveerd, is deze actie dat 'ten minste één van deze dingen is gebeurd." Bijvoorbeeld, wanneer Event Grid wordt geactiveerd met een logische app op een bericht dat wordt weergegeven in een Service Bus-wachtrij, de logische app moet altijd wordt ervan uitgegaan dat er mogelijk een of meer berichten beschikbaar om te verwerken.

### <a name="region"></a>Regio

Kies voor minimale netwerklatentie, dezelfde regio voor API Management, Logic Apps en Service Bus. In het algemeen kiest u de regio die zich het dichtst bij uw gebruikers.

De resourcegroep heeft ook een regio. Deze regio geeft aan waar de voor het opslaan van metagegevens voor de implementatie en waar om uit te voeren van de sjabloon voor de implementatie. De beschikbaarheid te verbeteren tijdens de implementatie, plaatst u de resourcegroep en resources in dezelfde regio.

## <a name="scalability"></a>Schaalbaarheid

Als u wilt bereiken hogere schaalbaarheid, schalen de Service Bus Premium-laag het aantal messaging-eenheden. Configuraties voor Premium-laag kunnen een, twee of vier messaging-eenheden hebben. Zie voor meer informatie over het schalen van Service Bus [aanbevolen procedures voor prestatieverbeteringen met behulp van Service Bus-berichten](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability"></a>Beschikbaarheid

* De service level agreement (SLA) voor Azure API Management is voor de lagen Basic, Standard en Premium, momenteel 99,9%. Voor premium-laag configuraties met een implementatie met minimaal één eenheid in twee of meer regio's, is de SLA van 99,95%.

* De SLA voor Azure Logic Apps wordt momenteel 99,9%.

### <a name="disaster-recovery"></a>Herstel na noodgeval

Om in te schakelen failover als er een ernstige storing optreedt, houd rekening met geo-noodherstel implementeren in Service Bus Premium. Zie voor meer informatie, [geo-noodherstel van Azure Service Bus](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability"></a>Beheerbaarheid

Maak afzonderlijke resourcegroepen voor productie, ontwikkeling en testomgevingen. Afzonderlijke resourcegroepen kunt u eenvoudiger implementaties beheren, testimplementaties verwijderen en toegangsrechten toewijzen.

Wanneer u resources aan resourcegroepen toewijst, houd rekening met deze factoren:

* **Levenscyclus**: plaats In het algemeen resources die dezelfde levenscyclus te in dezelfde resourcegroep bevinden hebben.

* **Toegang tot**: om toe te passen beleidsregels voor toegang tot de resources in een groep, kunt u [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md).

* **Facturering**: U kunt updatepakket kosten voor de resourcegroep bekijken.

* **Prijscategorie voor API Management**: de Developer-laag gebruiken voor uw ontwikkel- en testomgevingen. U kosten kunt minimaliseren tijdens de testfase vóór productie, een replica van de productie-omgeving implementeren, Voer uw tests uit en sluit vervolgens af.

Zie voor meer informatie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="deployment"></a>Implementatie

* Voor het implementeren van API Management, Logic Apps, Event Grid en Service Bus, gebruikt u de [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md). Sjablonen eenvoudiger implementaties automatiseren met behulp van PowerShell of Azure CLI.

* API Management, een afzonderlijke logic apps, Event Grid-onderwerpen en Service Bus-naamruimten in hun eigen afzonderlijke Resource Manager-sjablonen te plaatsen. U kunt de resources met behulp van afzonderlijke sjablonen, opslaan in bronbeheersystemen. Vervolgens kunt u deze sjablonen implementeren samen of afzonderlijk als onderdeel van een continue integratie/continue implementatie (CI/CD)-proces.

## <a name="diagnostics-and-monitoring"></a>Diagnose en controle

U kunt Service Bus, zoals API Management en logische Apps bewaken met behulp van Azure Monitor is standaard ingeschakeld. Azure Monitor biedt informatie op basis van de metrische gegevens die zijn geconfigureerd voor elke service. 

## <a name="security"></a>Beveiliging

Gebruik voor het beveiligen van Service Bus, handtekening voor gedeelde toegang (SAS). U kunt bijvoorbeeld een gebruikerstoegang tot Service Bus-bronnen met specifieke rechten verlenen met behulp van [SAS verificatie](../service-bus-messaging/service-bus-sas.md). Zie voor meer informatie, [Service Bus-verificatie en autorisatie](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Als u nodig hebt om beschikbaar te stellen van een Service Bus-wachtrij als een HTTP-eindpunt, bijvoorbeeld nieuwe berichten wilt posten gebruik API Management voor het beveiligen van de wachtrij door fronting van het eindpunt. Vervolgens kunt u het eindpunt met de certificaten of OAuth-verificatie als de juiste beveiligen. De eenvoudigste manier kunt u een eindpunt beveiligen is een logische app met een HTTP-aanvraag/antwoord-trigger gebruikt als een intermediair.

De Event Grid-service beveiligt bezorging van gebeurtenissen via een code voor validatie. Als u Logic Apps gebruiken voor de gebeurtenis, wordt validatie automatisch uitgevoerd. Zie voor meer informatie, [Event Grid-beveiliging en verificatie](../event-grid/security-authentication.md).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [eenvoudige bedrijfsintegratie](/azure/architecture/reference-architectures/enterprise-integration/simple-enterprise-integration)
