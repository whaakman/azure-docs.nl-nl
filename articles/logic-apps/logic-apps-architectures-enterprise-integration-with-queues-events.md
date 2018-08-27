---
title: Azure Integration Services enterprise integration-referentiearchitectuur
description: Hierin wordt beschreven in de referentiearchitectuur die laat hoe u een enterprise integration-patroon implementeert zien met behulp van Logic Apps, API Management Service Bus en Event Grid.
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
ms.openlocfilehash: ffa61ebfaa58425cd2bf70d9bf78a2e71b672369
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42918480"
---
# <a name="reference-architecture-enterprise-integration-with-queues-and-events"></a>Referentie-architectuur: bedrijfsintegratie met wachtrijen en -gebeurtenissen

De volgende referentiearchitectuur toont een set bewezen procedures die u kunt toepassen op een integratietoepassing die gebruikmaakt van Azure Integration Services. De architectuur kan fungeren als basis voor veel verschillende toepassingspatronen waarvoor HTTP APIs, werkstromen en orchestration.

![Architectuurdiagram - bedrijfsintegratie met wachtrijen en -gebeurtenissen](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

*Er zijn veel mogelijke toepassingen voor de integratie van technologie. Ze variëren van een eenvoudige point-to-point-toepassing naar een volledige enterprise Azure Service Bus-toepassing. De architectuur-serie beschrijft de herbruikbare onderdelen die van toepassing zijn mogelijk voor het bouwen van een algemene integratietoepassing. Architecten moeten rekening houden met welke onderdelen ze nodig hebben om te implementeren voor hun toepassingen en infrastructuur.*
<!-- Should previous line be in Italic or asterisks must be escaped? -->

## <a name="architecture"></a>Architectuur

De architectuur *bouwt voort op* de [eenvoudige bedrijfsintegratie](logic-apps-architectures-simple-enterprise-integration.md) architectuur. [Aanbevelingen voor de eenvoudige ondernemingsstructuur](logic-apps-architectures-simple-enterprise-integration.md#recommendations) hier uw aanvraag indienen. Ze zijn weggelaten uit de [aanbevelingen](#recommendations) in dit artikel voor kort te houden. 

De architectuur heeft de volgende onderdelen:

- **Resourcegroep**. Een [resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) is een logische container voor Azure-resources.
- **Azure API Management**. [API Management](https://docs.microsoft.com/azure/api-management/) is een volledig beheerd platform dat wordt gebruikt om te publiceren, te beveiligen en te transformeren HTTP APIs.
- **Azure API Management-ontwikkelaarsportal**. Elk exemplaar van Azure API Management wordt geleverd met toegang tot de [ontwikkelaarsportal](https://docs.microsoft.com/azure/api-management/api-management-customize-styles). De ontwikkelaarsportal van API Management biedt u toegang tot documentatie en codevoorbeelden. U kunt API's testen in de portal voor ontwikkelaars.
- **Azure Logic Apps**. [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) is een serverloos platform dat wordt gebruikt voor het bouwen van enterprise-werkstroom en het integratieaccount.
- **Connectors**. Logic Apps gebruikt [connectors](https://docs.microsoft.com/azure/connectors/apis-list) verbinding maken met veel gebruikte services. Logic Apps is al honderden verschillende connectors, maar u kunt ook een aangepaste connector maken.
- **Azure Servicebus**. [Service Bus](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview) biedt veilige en betrouwbare berichten. Berichten kunnen worden gebruikt voor het loskoppelen van toepassingen en integreren met andere systemen op basis van een bericht.
- **Azure Event Grid**. [Event Grid](https://docs.microsoft.com/azure/event-grid/overview) een serverloos platform dat wordt gebruikt om te publiceren en leveren van toepassingsgebeurtenissen is.
- **IP-adres**. De Azure API Management-service heeft een vaste openbare [IP-adres](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) en de domeinnaam van een. De standaarddomeinnaam is een subdomein van azure-api.net, zoals contoso.azure-api.net, maar [aangepaste domeinen](https://docs.microsoft.com/azure/api-management/configure-custom-domain) kan ook worden geconfigureerd. Logic Apps en Service Bus moet u ook een openbaar IP-adres hebben. In deze architectuur beperken we echter toegang voor het aanroepen van Logic Apps-eindpunten alleen het IP-adres van API Management (voor beveiliging). Aanroepen naar Service Bus worden beveiligd door een shared access signature (SAS).
- **Azure DNS**. [Azure DNS](https://docs.microsoft.com/azure/dns/) is een hostingservice voor DNS-domeinen. Azure DNS biedt naamomzetting met behulp van de Microsoft Azure-infrastructuur. Door uw domeinen in Azure hosten, kunt u uw DNS-records beheren met behulp van dezelfde referenties, API's, hulpprogramma's en facturering, gebruikt u voor uw andere Azure-services. Voor het gebruik van een aangepaste domeinnaam (zoals contoso.com), DNS-records die de aangepaste domeinnaam aan het IP-adres toewijzen te maken. Zie voor meer informatie, [een aangepaste domeinnaam configureren in API Management](https://docs.microsoft.com/en-us/azure/api-management/configure-custom-domain).
- **Azure Active Directory (Azure AD)**. Gebruik [Azure AD](https://docs.microsoft.com/azure/active-directory/) of een andere id-provider voor verificatie. Azure AD-verificatie voor toegang tot API-eindpunten door door te geven biedt een [JSON Web Token voor API Management](https://docs.microsoft.com/azure/api-management/policies/authorize-request-based-on-jwt-claims) om te valideren. Azure AD kan beveiligde toegang tot de API Management-ontwikkelaarsportal (alleen Standard en Premium-lagen).

De architectuur heeft enkele patronen die essentieel voor de werking ervan zijn:

* Bestaande back-end-HTTP-API's worden gepubliceerd via de API Management-ontwikkelaarsportal. In de portal voor ontwikkelaars (ofwel intern gebruik binnen uw organisatie, externe of beide) kunnen aanroepen naar deze API's integreren in toepassingen.
* Samengestelde API's zijn gebouwd met behulp van logic apps en door het coördineren van aanroepen naar software als een service (SaaS)-systemen, Azure-services en API's die worden gepubliceerd naar API Management. [Logische apps worden ook gepubliceerd](https://docs.microsoft.com/azure/api-management/import-logic-app-as-api) via de API Management-ontwikkelaarsportal.
- Toepassingen Azure AD gebruiken om [verkrijgen van een OAuth 2.0-beveiligingstoken](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) die is vereist voor toegang tot een API.
- API Management [valideert het beveiligingstoken](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) en geeft vervolgens de aanvraag door naar de back-end-API of de logische app.
- Service Bus-wachtrijen worden gebruikt om [loskoppelen](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) activiteit van de toepassing en [pieken in de belasting vloeiende](https://docs.microsoft.com/azure/architecture/patterns/queue-based-load-leveling). Berichten die worden toegevoegd aan de wachtrijen door logic apps, de toepassingen van derden, of (niet afgebeeld) door het publiceren van de wachtrij als een HTTP-API via API Management.
- Wanneer berichten worden toegevoegd aan een Service Bus-wachtrij, wordt een gebeurtenis wordt geactiveerd. Een logische app wordt geactiveerd door de gebeurtenis. De logische app verwerkt vervolgens het bericht.
- Andere Azure-services (bijvoorbeeld Azure-blobopslag en Azure Event Hubs) ook publiceren van gebeurtenissen naar Event Grid. Deze services-trigger voor logische apps voor het ontvangen van de gebeurtenis en vervolgens de volgende acties uitvoeren.

## <a name="recommendations"></a>Aanbevelingen

Uw specifieke vereisten kunnen afwijken van de algemene architectuur die in dit artikel wordt beschreven. Gebruik de aanbevelingen in deze sectie als uitgangspunt.

### <a name="service-bus-tier"></a>Service Bus-laag

Gebruik de Service Bus Premium-laag. De Premier laag ondersteunt Event Grid-meldingen. Zie voor meer informatie, [prijzen van Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Prijzen van Event Grid

Event Grid maakt gebruik van een model zonder server. Facturering wordt berekend op basis van het aantal bewerkingen (gebeurtenis uitvoeren). Zie voor meer informatie, [prijzen van Event Grid](https://azure.microsoft.com/pricing/details/event-grid/). Er zijn momenteel geen laag overwegingen voor Event Grid.

### <a name="use-peeklock-to-consume-service-bus-messages"></a>PeekLock gebruiken om te gebruiken van Service Bus-berichten

Wanneer u een logische app gebruiken voor Service Bus-berichten maken, gebruikt u [PeekLock](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#queues) in de logische app voor toegang tot een groep van berichten. Wanneer u PeekLock gebruikt, kan de logische app stappen voor het valideren van elk bericht voor het voltooien van of het bericht wordt afgebroken kunt uitvoeren. Deze aanpak worden beschermd tegen onbedoeld bericht verlies.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Controleren op meerdere objecten bij een Event Grid-trigger wordt geactiveerd

Wanneer een Event Grid-trigger wordt geactiveerd, betekent dit gewoon dat 'ten minste één van deze dingen is gebeurd." Bijvoorbeeld, wanneer Event Grid wordt geactiveerd met een logische app op een bericht dat wordt weergegeven in een Service Bus-wachtrij, de logische app moet altijd wordt ervan uitgegaan dat er mogelijk een of meer berichten beschikbaar om te verwerken.

### <a name="region"></a>Regio

Richt API Management, Logic Apps en Service Bus in dezelfde regio voor minimale netwerklatentie. Over het algemeen kiest u de regio die zich het dichtst bij uw gebruikers.

De resourcegroep heeft ook een regio. De regio geeft aan waar de metagegevens voor de implementatie worden opgeslagen en waar de sjabloon voor de implementatie van wordt uitgevoerd. Plaats de resourcegroep en de daarbij behorende bronnen in dezelfde regio beschikbaarheid verbeteren tijdens de implementatie.

## <a name="scalability"></a>Schaalbaarheid

De Service Bus Premium-laag voor het aantal messaging-eenheden voor hogere schaalbaarheid schalen. Configuraties voor Premium-laag kunnen een, twee of vier messaging-eenheden hebben. Zie voor meer informatie over het schalen van Service Bus [aanbevolen procedures voor prestatieverbeteringen met behulp van Service Bus-berichten](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability"></a>Beschikbaarheid

De service level agreement (SLA) voor Azure API Management is momenteel 99,9% voor de lagen Basic, Standard en Premium. Premium-laag configuraties met implementatie van ten minste één eenheid in twee of meer regio's hebben een SLA van 99,95%.

De SLA voor Azure Logic Apps is momenteel 99,9%.

### <a name="disaster-recovery"></a>Herstel na noodgeval

Houd rekening met geo-noodherstel implementeren in Service Bus Premium om failover mogelijk te als er een ernstige storing optreedt. Zie voor meer informatie, [geo-noodherstel van Azure Service Bus](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability"></a>Beheerbaarheid

Maak afzonderlijke resourcegroepen voor productie, ontwikkeling en testomgevingen. Afzonderlijke resourcegroepen kunt u eenvoudiger implementaties beheren, testimplementaties verwijderen en toegangsrechten toewijzen.

Wanneer u resources aan resourcegroepen toewijst, houd rekening met de volgende factoren:

- **Levenscyclus**. In het algemeen plaats resources die dezelfde levenscyclus te in dezelfde resourcegroep bevinden hebben.
- **Toegang tot**. U kunt [op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md) (RBAC) om toe te passen van beleidsregels voor toegang tot de resources in een groep.
- **Facturering**. U kunt updatepakket kosten voor de resourcegroep bekijken.
- **Prijscategorie voor API Management**. We raden u aan de Developer-laag gebruiken voor ontwikkeling en testomgevingen. Voor Pre-productie, wordt aangeraden een replica van de productie-omgeving implementeren tests uitvoert en vervolgens naar de kosten minimaliseren wordt afgesloten.

Zie voor meer informatie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="deployment"></a>Distributie

Het is raadzaam dat u [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md) API Management, Logic Apps, Event Grid en Service Bus te implementeren. Sjablonen wordt het eenvoudiger implementaties automatiseren via PowerShell of Azure CLI.

Het is raadzaam om de plaatsen van API Management, een afzonderlijke logic apps, Event Grid-onderwerpen en Service Bus-naamruimten in hun eigen, afzonderlijke Resource Manager-sjablonen. Als u afzonderlijke sjablonen gebruikt, kunt u de resources in bronbeheersystemen kunt opslaan. Vervolgens kunt u deze sjablonen implementeren samen of afzonderlijk als onderdeel van een continue integratie/continue implementatie (CI/CD)-proces.

### <a name="diagnostics-and-monitoring"></a>Diagnostische gegevens en controle

U kunt Service Bus, zoals API Management en logische Apps bewaken met behulp van Azure Monitor. Azure Monitor biedt informatie op basis van de metrische gegevens die zijn geconfigureerd voor elke service. Azure Monitor is standaard ingeschakeld.

## <a name="security"></a>Beveiliging

Service Bus beveiligen met behulp van een SAS. U kunt [SAS verificatie](../service-bus-messaging/service-bus-sas.md) om een gebruikerstoegang naar Service Bus-bronnen met specifieke rechten te verlenen. Zie voor meer informatie, [Service Bus-verificatie en autorisatie](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Als een Service Bus-wachtrij moet worden weergegeven als een HTTP-eindpunt (voor nieuwe berichten plaatsen), moet u API Management te beveiligen door het eindpunt fronting. Het eindpunt kan vervolgens worden beveiligd met certificaten of OAuth waar nodig. Er is de eenvoudigste manier voor het beveiligen van een eindpunt met behulp van een logische app met een HTTP-trigger voor aanvraag/antwoord als een intermediair.

Event Grid beveiligt bezorging van gebeurtenissen via een code voor validatie. Als u Logic Apps gebruiken voor de gebeurtenis, wordt validatie automatisch uitgevoerd. Zie voor meer informatie, [Event Grid-beveiliging en verificatie](../event-grid/security-authentication.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [eenvoudige bedrijfsintegratie](logic-apps-architectures-simple-enterprise-integration.md).
