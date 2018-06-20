---
title: Azure Integration Services-referentiearchitectuur
description: Referentiearchitectuur getoond hoe voor het implementeren van een patroon van de enterprise-integratie met Logic Apps, API Management, Service Bus en gebeurtenis raster
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
ms.openlocfilehash: db3350b3c70f6e6f35949e8423334061849b7b37
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232391"
---
# <a name="enterprise-integration-with-queues-and-events-reference-architecture"></a>Integratie met wachtrijen en gebeurtenissen in de onderneming: verwijzen naar architectuur

## <a name="overview"></a>Overzicht

Deze verwijzende architectuur wordt een reeks bewezen een integratietoepassing die gebruikmaakt van Azure Integration Services. Deze architectuur kan fungeren als basis van veel verschillende groepen van toepassingen patronen vereisen HTTP APIs, werkstroom- en orkestratielaag.

*Er zijn veel mogelijke toepassingen van integratie-technologie van de toepassing van een eenvoudige punt-naar-punt naar een volledige enterprise servicebus. Deze reeks architectuur stelt de herbruikbare onderdelen voor het bouwen van een integratietoepassing – architecten moeten rekening houden met de onderdelen die ze nodig voor de toepassingen en infrastructuur hebt.*

![Architectuurdiagram - integratie met wachtrijen en gebeurtenissen in de onderneming](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

## <a name="architecture"></a>Architectuur

Deze architectuur is gebaseerd op wordt weergegeven [eenvoudige enterprise integration](logic-apps-architectures-simple-enterprise-integration.md). Deze bevat de volgende onderdelen:

- Resourcegroep. Een resourcegroep is een logische container voor Azure-resources.
- Azure API Management. Azure API Management is een volledig beheerd platform voor het publiceren, te beveiligen en te transformeren HTTP APIs.
- Azure API Management Developer-Portal. Elk exemplaar van Azure API Management wordt geleverd met een Portal voor ontwikkelaars die toegang geeft tot documentatie, codevoorbeelden en de mogelijkheid voor het testen van een API.
- Azure Logic Apps. Logic Apps is een zonder server platform voor het bouwen van enterprise-werkstroom en integratie.
- Connectors. Connectors worden gebruikt door Logic Apps verbinding maken met veelgebruikte services. Logic Apps honderden verschillende connectors al heeft, maar ze kunnen ook worden gemaakt met een aangepaste connector.
- Azure Servicebus. Service Bus biedt veilige en betrouwbare messaging. Berichten kan worden gebruikt voor ongedaan Combineer toepassingen van elkaar en integreren met andere systemen op basis van berichten.
- Azure Event raster. Raster gebeurtenis is een zonder server platform voor het publiceren en levering van toepassingsgebeurtenissen.
- IP-adres. De Azure API Management-service heeft een vaste openbare IP-adres en een domeinnaam. De domeinnaam is een subdomein van azure-api.net, zoals contoso.azure api.net. Logic Apps en Service Bus hebt ook een openbaar IP-adres; in deze architectuur beperken we echter toegang tot het aanroepen van Logic apps-eindpunten op alleen de IP-adres van API Management (voor beveiliging). Aanroepen naar Service Bus worden beveiligd door een shared access signature.
- Azure DNS. Azure DNS is een hosting service voor DNS-domeinen omzetten van namen met behulp van Microsoft Azure-infrastructuur. Door uw domeinen in Azure te hosten, kunt u uw DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering beheren als voor uw andere Azure-services. Maak DNS-records die de naam van het aangepaste domein aan het IP-adres toewijzen wilt gebruiken een aangepaste domeinnaam (zoals contoso.com). Zie voor meer informatie configureren een aangepaste domeinnaam in Azure API Management.
- Azure Active Directory (Azure AD). Gebruik Azure AD of een andere id-provider voor verificatie. Azure AD biedt verificatie met toegang tot API-eindpunten (door het doorgeven van een JSON Web Token voor API Management valideren) en toegang tot de Ontwikkelaarsportal van de API-beheer (alleen de lagen Standard en Premium) kunt beveiligen.

Deze architectuur heeft alle fundamentele patronen voor de bewerking:

1. Bestaande back-end HTTP APIs worden gepubliceerd via het API Management-Portal voor ontwikkelaars, zodat ontwikkelaars (ofwel intern gebruik binnen uw organisatie, externe of beide) aanroepen naar deze API's integreren in toepassingen.
2. Samengestelde API's zijn gebouwd met behulp van Logic Apps; organiseren aanroepen naar de SAAS-systemen, Azure-services en een API's gepubliceerd op API Management. De Logic Apps worden ook worden uitgegeven via het Ontwikkelaarsportal van API Management.
3. Toepassingen verkrijgen van een beveiligingstoken OAuth 2.0 nodig voor toegang tot een API met Azure Active Directory.
4. Azure API Management valideert het beveiligingstoken, en de aanvraag wordt doorgegeven aan de back-end API/logische App.
5. Service Bus-wachtrijen worden gebruikt voor het loskoppelen van de activiteit van de toepassing en smooth pieken in laden. Berichten die worden toegevoegd aan wachtrijen door Logic Apps, 3e toepassingen van derden of (niet weergegeven) door het publiceren van de wachtrij als een HTTP-API via API Management.
6. Een gebeurtenis wordt geactiveerd wanneer berichten worden toegevoegd aan een Service Bus-wachtrij. Een logische App bij deze gebeurtenis wordt geactiveerd en verwerkt het bericht.
7. Andere Azure-services (bijvoorbeeld Blob Storage, Event Hub) worden op dezelfde manier ook de gebeurtenissen publiceren naar gebeurtenis raster. Deze trigger Logic Apps voor het ontvangen van de gebeurtenis en verdere acties uitvoeren.

## <a name="recommendations"></a>Aanbevelingen

Uw vereisten kunnen afwijken van de architectuur die hier wordt beschreven. Gebruik de aanbevelingen in deze sectie als uitgangspunt.

### <a name="service-bus-tier"></a>Service Bus-laag

Gebruik van Service Bus premium-laag, zoals dit raster gebeurtenismeldingen momenteel ondersteunt (ondersteuning voor alle lagen verwacht). Zie [Service Bus prijzen](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Gebeurtenis raster prijzen

Gebeurtenis raster werkt met behulp van een model zonder server – facturering wordt berekend op basis van het aantal bewerkingen (gebeurtenis uitvoeren). Zie [gebeurtenis raster prijzen](https://azure.microsoft.com/pricing/details/event-grid/) voor meer informatie. Er zijn momenteel geen laag overwegingen voor gebeurtenis raster.

### <a name="use-peeklock-when-consuming-service-bus-messages"></a>Gebruik PeekLock wanneer het verbruik van Service Bus-berichten

Gebruik bij het maken van Logic Apps voor het verbruik van Service Bus-berichten [PeekLock](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#queues) in de logische App toegang tot een groep van berichten. De logische App kunt stappen voor het valideren van elk bericht voordat voltooien of opgegeven vervolgens uitvoeren. Deze aanpak wordt beschermd tegen onbedoeld bericht verloren gaan.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Controleren op meerdere objecten bij een raster voor gebeurtenis-trigger wordt geactiveerd

Raster gebeurtenistriggers uitgevoerd betekent die 'ten minste 1 van deze dingen hebben plaatsgevonden'. Bijvoorbeeld wanneer gebeurtenis raster een logische app voor een bericht wordt weergegeven in een Service Bus-wachtrij veroorzaakt, de logische app moet altijd wordt ervan uitgegaan dat er mogelijk een of meer berichten beschikbaar om te verwerken.

### <a name="region"></a>Regio

API Management, Logic Apps en Service Bus in dezelfde regio als u wilt minimaliseren netwerklatentie inrichten. In het algemeen kiest u de regio die het meest in de buurt van uw gebruikers is.

De resourcegroep heeft ook een regio die wordt opgegeven waar de metagegevens voor de implementatie is opgeslagen, en waarop de sjabloon voor de implementatie van wordt uitgevoerd. Plaats de resourcegroep en de bijbehorende resources in dezelfde regio. Dit kan de beschikbaarheid verbeteren tijdens de implementatie.

## <a name="scalability-considerations"></a>Schaalbaarheidsoverwegingen

Azure Service Bus premium-laag kunt schalen het aantal messaging-eenheden voor hogere schaalbaarheid. Premium kunt hebben 1, 2 of 4 messaging-eenheden. Zie voor verdere instructies over het schalen van Azure Service Bus [Best Practices voor verbeterde prestaties met behulp van Service Bus-berichtenservice](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability-considerations"></a>Beschikbaarheidsoverwegingen

Op het moment van schrijven is service level agreement (SLA) voor Azure API Management 99,9% voor categorieën Basic, Standard en Premium. Premium-laag configuraties met implementatie van ten minste één eenheid in twee of meer regio's hebben een SLA van 99,95%.

Op het moment van schrijven is de serviceovereenkomst (SLA) voor Azure Logic Apps 99,9%.

### <a name="disaster-recovery"></a>Herstel na noodgeval

Overweeg de implementatie van Geo-noodherstel van Service Bus premium failover inschakelen in het geval van een ernstige storing. Lees meer over [Azure Service Bus Geo-noodherstel](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability-considerations"></a>Beheerbaarheidsoverwegingen

Maken van afzonderlijke resourcegroepen voor productie, ontwikkeling en testen omgevingen. Hiermee kunt u eenvoudiger implementaties beheren, testimplementaties verwijderen en toegangsrechten verlenen.
Wanneer u resources toewijst aan resourcegroepen, moet u het volgende overwegen:

- Levenscyclus. In het algemeen kunt u resources met dezelfde levenscyclus het beste in dezelfde resourcegroep onderbrengen.
- Toegang. U kunt [toegangsbeheer op basis van rollen](../role-based-access-control/overview.md) (RBAC) toe te passen van beleidsregels voor toegang tot bronnen in een groep.
- Facturering. U kunt de samengevoegde kosten voor de resourcegroep bekijken.
- Prijscategorie voor API Management – aangeraden categorie Developer voor ontwikkel- en testomgevingen. Voor preproductie, wordt aangeraden een replica van uw productieomgeving implementeren tests uitgevoerd, en vervolgens de kosten te minimaliseren wordt afgesloten.

Zie voor meer informatie [resourcegroep](../azure-resource-manager/resource-group-overview.md) overzicht.

### <a name="deployment"></a>Implementatie

Het is raadzaam dat u [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md) Azure API Management, Logic Apps, gebeurtenis raster en Service Bus implementeren. Sjablonen kunnen gemakkelijker implementaties via PowerShell of de Azure-opdrachtregelinterface (CLI) automatiseren.

We raden u aan als Azure API Management, een afzonderlijke Logic Apps, gebeurtenis raster onderwerpen en Service Bus-naamruimten in hun eigen afzonderlijke Resource Manager-sjablonen. Hierdoor kunnen ze tijdens het opslaan in besturingselement bronsystemen voor. Deze sjablonen kunnen worden geïmplementeerd samen of afzonderlijk als onderdeel van het implementatieproces van een doorlopende integratie/continue (CI/CD).

### <a name="diagnostics-and-monitoring"></a>Diagnose en controle

Service Bus, zoals API Management en Logic Apps kunnen worden gecontroleerd met behulp van Azure-Monitor. Azure Monitor is standaard ingeschakeld en op basis van de andere metrische gegevens voor elke service geconfigureerd informatie bieden.

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Service Bus met behulp van een Shared Access Signature beveiligen. [SAS verificatie](../service-bus-messaging/service-bus-sas.md) kunt u een gebruikerstoegang tot de Service Bus-resources met specifieke rechten verlenen. Lees meer over [Service Bus-verificatie en autorisatie](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Daarnaast moet een Service Bus-wachtrij moet worden weergegeven als een HTTP-eindpunt (voor boeken van nieuwe berichten) API Management te beveiligen door het eindpunt fronting moet worden gebruikt. Dit kan vervolgens worden beveiligd met certificaten of OAuth naar gelang van toepassing. De eenvoudigste manier om dit te doen is een logische App die met een HTTP-aanvraag/antwoord-trigger als intermediaire.

Gebeurtenis raster beveiligt gebeurtenis levering via een code voor validatie. Als u LogicApps gebruiken voor de gebeurtenis, wordt dit automatisch uitgevoerd. Meer details weergeven over [gebeurtenis raster beveiligings- en verificatie](../event-grid/security-authentication.md).

## <a name="next-steps"></a>Volgende stappen

* [Eenvoudige Enterprise Integration](logic-apps-architectures-simple-enterprise-integration.md)
