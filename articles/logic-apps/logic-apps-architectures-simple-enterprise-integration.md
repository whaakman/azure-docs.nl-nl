---
title: Azure integratieservices - eenvoudige Enterprise Integration
description: Referentiearchitectuur waarin wordt getoond hoe een eenvoudige enterprise integration-patroon met Azure Logic Apps en Azure API Management implementeren
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
ms.openlocfilehash: df86ca5aed405ab5eda05c1dd207f0b6656bfd96
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37860194"
---
# <a name="simple-enterprise-integration---reference-architecture"></a>Eenvoudige Enterprise Integration - referentiearchitectuur

## <a name="overview"></a>Overzicht

Deze referentiearchitectuur toont een set bewezen procedures voor een integratietoepassing die gebruikmaakt van Azure Integration Services. Deze architectuur kan fungeren als deze basis van veel verschillende toepassingspatronen waarbij HTTP APIs, werkstroom en orchestration.

*Er zijn veel mogelijke toepassingen van technologie voor integratie van de toepassing van een eenvoudige punt-naar-punt met een volledige enterprise servicebus. Deze reeks architectuur stelt de herbruikbare onderdelen die mogelijk van toepassing voor het bouwen van een integratietoepassing algemene-architecten moeten rekening houden met welke specifieke onderdelen, moeten ze worden geïmplementeerd voor de toepassingen en infrastructuur.*

   ![Architectuurdiagram - eenvoudige bedrijfsintegratie](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

## <a name="architecture"></a>Architectuur

De architectuur heeft de volgende onderdelen:

- Resourcegroep. Een [resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) is een logische container voor Azure-resources.
- Azure API Management. [Met Azure API Management](https://docs.microsoft.com/azure/api-management/) is een volledig beheerd platform voor het publiceren, beveiligen en HTTP APIs transformeren.
- Portal voor ontwikkelaars van Azure API Management. Elk exemplaar van Azure API Management wordt geleverd met een [Ontwikkelaarsportal](https://docs.microsoft.com/azure/api-management/api-management-customize-styles), die toegang geeft tot documentatie, codevoorbeelden en de mogelijkheid voor het testen van een API.
- Azure Logic Apps. [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) is een serverloos platform voor het bouwen van enterprise-werkstroom en het integratieaccount.
- Connectors. [Connectors](https://docs.microsoft.com/azure/connectors/apis-list) worden gebruikt door Logic Apps verbinding maken met de meest gebruikte services. Logic Apps heeft al honderden verschillende connectors, maar ze kunnen ook worden gemaakt met behulp van een aangepaste connector.
- IP-adres. De Azure API Management-service heeft een vaste openbare [IP-adres](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) en de domeinnaam van een. De domeinnaam is een subdomein van azure-api.net, zoals contoso.azure-api.net. Logic Apps en Service Bus hebt ook een openbare IP-adres. in deze architectuur beperken we echter toegang voor het aanroepen van Logic apps-eindpunten aan alleen de IP-adres van API Management (voor beveiliging). Aanroepen naar Service Bus worden beveiligd door een shared access signature.
- Azure DNS. [Azure DNS](https://docs.microsoft.com/azure/dns/) is een hostingservice voor DNS-domeinen die naamomzetting met behulp van Microsoft Azure-infrastructuur biedt. Door uw domeinen in Azure te hosten, kunt u uw DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering beheren als voor uw andere Azure-services. Maak DNS-records die de aangepaste domeinnaam aan het IP-adres toewijzen voor het gebruik van een aangepaste domeinnaam (zoals contoso.com). Zie voor meer informatie, een aangepaste domeinnaam in Azure API Management configureren.
- Azure Active Directory (Azure AD). Gebruik [Azure AD](https://docs.microsoft.com/azure/active-directory/) of een andere id-provider voor verificatie. Azure AD biedt verificatie voor toegang tot API-eindpunten (door door te geven een [JSON Web Token voor API Management](https://docs.microsoft.com/azure/api-management/policies/authorize-request-based-on-jwt-claims) valideren) en toegang tot de API Management-Ontwikkelaarsportal (alleen Standard en Premium-lagen) kunt beveiligen.

Deze architectuur heeft enkele fundamentele patronen op de werking ervan:

2. Samengestelde API's zijn gebouwd met behulp van Logic Apps; aanroepen van SAAS-systemen organiseren, Azure-services en API's gepubliceerd op API Management. De [Logic Apps worden ook gepubliceerd](https://docs.microsoft.com/azure/api-management/import-logic-app-as-api) via de API Management-Ontwikkelaarsportal.
3. Toepassingen [verkrijgen van een OAuth 2.0-beveiligingstoken](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) die nodig zijn voor het verkrijgen van toegang tot een API met behulp van Azure Active Directory.
4. Met Azure API Management [valideert het beveiligingstoken](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad), en wordt de aanvraag doorgegeven aan de back-end API/logische App.

## <a name="recommendations"></a>Aanbevelingen

Uw specifieke vereisten kunnen afwijken van de algemene architectuur die hier worden beschreven. Gebruik de aanbevelingen in deze sectie als uitgangspunt.

### <a name="azure-api-management-tier"></a>Azure API Management-laag

Gebruik de Basic, Standard of Premium-lagen omdat ze een productie-SLA bieden en ondersteuning voor scale-out in de Azure-regio (aantal eenheden verschilt per laag). Premium-laag biedt ook ondersteuning voor scale-out meerdere Azure-regio's. Baseer de laag die u hebt gekozen op het niveau van de vereiste doorvoer hebt en functie ingesteld. Zie voor meer informatie, [API Management-prijzen](https://azure.microsoft.com/pricing/details/api-management/).

U betaalt voor alle exemplaren van API Management wanneer ze worden uitgevoerd. Als u hebt uitgebreid en hoeft dat niveau van de prestaties van de tijd, kunt u profiteren van de API Management van per uur facturering en schaal omlaag.

### <a name="logic-apps-pricing"></a>Prijzen voor Logic Apps

Logische Apps werkt ook als een [serverloze](logic-apps-serverless-overview.md) -model, facturering wordt berekend op basis van de actie en connector kan worden uitgevoerd. Zie [prijzen voor Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/) voor meer informatie. Er zijn momenteel geen laag overwegingen voor logische Apps.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Logic Apps voor asynchrone API-aanroepen

Logische Apps werkt het beste in scenario's waarvoor lage latentie: bijvoorbeeld asynchrone niet of halfautomatische lange uitgevoerde API-aanroepen. Als met lage latentie vereist is (bijvoorbeeld een aanroep waarmee een gebruikersinterface wordt geblokkeerd) het beste uitvoering van die API of bewerking met een andere technologie, zoals Azure Functions of geïmplementeerd met behulp van App Service-Web-API. Nog steeds het beste deze API fronted met behulp van API Management API consumenten.

### <a name="region"></a>Regio

Richt API Management en logische Apps in dezelfde regio voor minimale netwerklatentie. In het algemeen kiest u de regio die het meest in de buurt van uw gebruikers is.

De resourcegroep heeft ook een regio, dat waar de metagegevens voor de implementatie is opgeslagen aangeeft, en waar de sjabloon voor de implementatie is uitgevoerd. Plaats de resourcegroep en de bijbehorende resources in dezelfde regio. Dit kan de beschikbaarheid verbeteren tijdens de implementatie.

## <a name="scalability-considerations"></a>Schaalbaarheidsoverwegingen

API Management-beheerders toe te voegen [cachebeleidsregels](../api-management/api-management-howto-cache.md) indien van toepassing op de schaalbaarheid van de service vergroten en de belasting van hun back-endservices verminderen.

Azure API Management Basic, Standard en Premium-lagen kunnen worden uitgebreid met in een Azure-regio te bieden grotere capaciteit. Beheerders kunnen de capaciteit metrische gegevens in het menu van de metrische gegevens gebruiken voor het analyseren van het gebruik van hun service en omhoog of omlaag schalen indien van toepassing.

Aanbevelingen voor het schalen van een API Management-service:

- Schaalbehoeften rekening te houden met verkeerspatronen: klanten met meer vluchtige verkeerspatronen hebt groter verhoogde capaciteit nodig.
- Consistente capaciteit dan 66% kan duiden op een nodig om omhoog te schalen.
- Consistente capaciteit dan 20% kan duiden op een kans om omlaag te schalen.
- Is het altijd beter om te laden uw API Management-service met een representatieve belasting testen voordat u inschakelt in de productieomgeving.

Services voor Premium-laag kunnen worden uitgebreid over meerdere Azure-regio's. Klanten die op deze manier krijgt een hogere SLA (99,95% in plaats van 99,9%) en services buurt bevindt van gebruikers in meerdere regio's kunnen inrichten.

Logic Apps serverloze model betekent dat beheerders hoeft niet te maken van extra aandacht voor schaalbaarheid van de service. de service automatisch wordt geschaald op basis van vraag.

## <a name="availability-considerations"></a>Beschikbaarheidsoverwegingen

Op het moment van schrijven is de service level agreement (SLA) voor Azure API Management 99,9% voor de lagen Basic, Standard en Premium. Premium-laag configuraties met implementatie van ten minste één eenheid in twee of meer regio's hebben een SLA van 99,95%.

Op het moment van schrijven is de service level agreement (SLA) voor Azure Logic Apps 99,9%.

### <a name="backups"></a>Back-ups

De configuratie van Azure API Management moet [regelmatig back-ups van](../api-management/api-management-howto-disaster-recovery-backup-restore.md) (op de juiste wijze op basis van regelmaat van wijzigingen), en de back-upbestanden die zijn opgeslagen in een locatie of een Azure-regio is anders dan bij de waar de service zich bevindt. Klanten kunnen ervoor kiezen een van twee opties voor de DR-strategie:

1. In een DR-gebeurtenis, een nieuw API Management-exemplaar is ingericht en de back-up wordt hersteld naar het DNS-records zijn repointed.
2. Klanten behouden een passieve kopie van de service in een andere Azure-regio (aanvullende kosten) back-ups regelmatig worden teruggezet naar het. In een gebeurtenis voor herstel na Noodgevallen, moeten alleen de DNS-records worden repointed voor het herstellen van de service.

Logic Apps kunnen zeer snel worden gemaakt en zijn zonder server, worden ze ondersteund door een kopie van de bijbehorende Azure Resource Manager-sjabloon opslaat. Deze kunnen worden opgeslagen naar bron besturingselement/geïntegreerd in een klanten continue integratie/continue implementatie (CI/CD)-proces.

Logische Apps die zijn gepubliceerd via API Management moeten hun locaties bijgewerkt moeten ze worden verplaatst naar een ander datacenter. Dit kan worden bereikt via een eenvoudige PowerShell-script voor het bijwerken van de back-end-eigenschap van de API.

## <a name="manageability-considerations"></a>Beheerbaarheidsoverwegingen

Maak afzonderlijke resourcegroepen voor productie, ontwikkeling en testomgevingen. Hiermee kunt u eenvoudiger implementaties beheren, testimplementaties verwijderen en toegangsrechten verlenen.
Wanneer u resources toewijst aan resourcegroepen, moet u het volgende overwegen:

- Levenscyclus. In het algemeen kunt u resources met dezelfde levenscyclus het beste in dezelfde resourcegroep onderbrengen.
- Toegang. U kunt [Role-Based Access Control](../role-based-access-control/overview.md) (RBAC) om toe te passen van beleidsregels voor toegang tot de resources in een groep.
- Facturering. U kunt de samengevoegde kosten voor de resourcegroep bekijken.
- Prijscategorie voor API Management – wordt u aangeraden Developer-laag voor ontwikkel- en testomgevingen. Voor Pre-productie, wordt aangeraden een replica van de productie-omgeving implementeren tests uitvoert en vervolgens naar de kosten minimaliseren wordt afgesloten.

Zie voor meer informatie, [resourcegroep](../azure-resource-manager/resource-group-overview.md) overzicht.

### <a name="deployment"></a>Implementatie

Het is raadzaam dat u [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md) om zowel Azure API Management en Azure Logic Apps te implementeren. Sjablonen wordt het eenvoudiger implementaties automatiseren via PowerShell of de Azure-opdrachtregelinterface (CLI).

Het is raadzaam om de toevoeging van Azure API Management en een afzonderlijke logische Apps in hun eigen afzonderlijke Resource Manager-sjablonen. Hierdoor kunnen opslaat in bronbeheersystemen. Deze sjablonen kunnen vervolgens worden geïmplementeerd samen of afzonderlijk als onderdeel van een continue integratie/continue (CI/CD)-implementatieproces.

### <a name="versions"></a>Versies

Telkens wanneer u een configuratie wijzigen in een logische App (of een update via Resource Manager-sjabloon implementeren), een kopie van deze versie wordt opgeslagen voor uw gemak (alle versies waarvoor een uitvoeringsgeschiedenis worden behouden). U kunt deze versies historische wijzigingen bijhouden en promoot het ook een versie om te worden van de huidige configuratie van de logische app; doen dit het geval is betekent dat u kunt effectief terugdraai-een logische App, bijvoorbeeld.

API Management heeft twee verschillende (maar gratis) [versiebeheer concepten](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/):

- Versies die worden gebruikt voor uw consumenten API met een keuze uit de API die ze in beslag kunnen nemen op basis van hun behoeften (bijv. v1, v2 of bèta, productie).
- Revisies zodat de beheerder van de API veilig wijzigingen aanbrengen aan een API en implementeren voor gebruikers met optionele opmerkingen.

In de context van implementatie – moeten API Management revisies worden beschouwd als een manier om veilig wijzigingen aanbrengen, een wijzigingen bijhouden en consumenten API op de hoogte van deze wijzigingen aanbrengen. Een revisie worden gemaakt in een ontwikkelingsomgeving en geïmplementeerd tussen omgevingen met behulp van Resource Manager-sjablonen.

Hoewel revisies kunnen worden gebruikt voor het testen van een API voordat deze is gemaakt 'huidige' en toegankelijk worden gemaakt voor gebruikers, raden we niet via dit mechanisme voor belasting of het testen van de integratie: afzonderlijke test- of Pre-productie-omgevingen in plaats daarvan moeten worden gebruikt.

### <a name="configuration"></a>Configuratie

Nooit controleren wachtwoorden, toegangssleutels of verbindingsreeksen in om te bepalen van de bron. Als dat nodig is, gebruikt u de juiste methode te implementeren en te beveiligen van deze waarden. 

In Logic Apps, moeten alle gevoelige waarden die nodig zijn in de logische app (die in de vorm van een verbinding kan niet worden gemaakt) worden opgeslagen in Azure Key Vault en waarnaar wordt verwezen op basis van een Resource Manager-sjabloon. Raadzaam om ook met behulp van de sjabloonparameters implementatie samen met de parameterbestanden voor elke omgeving. Meer informatie over [beveiliging van de parameters en invoer binnen een werkstroom](logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

Geheimen worden beheerd met behulp van objecten met de naam waarden/eigenschappen aangeroepen in API Management. Deze waarden die toegankelijk zijn voor het veilig opslaan in API Management-beleidsregels. Zie hoe u [geheimen in API Management beheren](../api-management/api-management-howto-properties.md).

### <a name="diagnostics-and-monitoring"></a>Diagnose en controle

Beide [API Management](../api-management/api-management-howto-use-azure-monitor.md) en [Logic Apps](logic-apps-monitor-your-logic-apps.md) bieden ondersteuning voor operationele bewaking via [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure Monitor is standaard ingeschakeld en biedt informatie op basis van de verschillende metrische gegevens voor elke service is geconfigureerd.

Er zijn bovendien nog meer opties voor elke service:

- Logic Apps-logboeken kunnen worden verzonden naar [Log Analytics](logic-apps-monitor-your-logic-apps-oms.md) voor uitgebreidere analyse en dashboarding.
- API Management biedt ondersteuning voor configuratie van Application Insights voor het bewaken van Dev Ops.
- Biedt ondersteuning voor API Management de [Power BI-oplossingssjabloon voor aangepaste API analytics](http://aka.ms/apimpbi). Deze oplossingssjabloon kan klanten hun eigen aangepaste analyseoplossing maken met rapporten die beschikbaar zijn in Power BI voor zakelijke gebruikers.

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Deze sectie vindt u beveiligingsoverwegingen die specifiek voor de Azure-services die worden beschreven in dit artikel wordt geïmplementeerd in de architectuur zijn, zoals wordt beschreven. Het is geen volledige lijst met aanbevolen procedures voor beveiliging.

- Op rollen gebaseerd toegangsbeheer (RBAC) gebruiken om te controleren of de juiste toegangsniveaus voor gebruikers.
- Beveiligde openbare API-eindpunten in API Management met behulp van OAuth/Open IDConnect. Dit doen door een id-provider configureren en het toevoegen van een beleid voor JWT-validatie.
- Verbinding maken met back-endservices van API Management met behulp van certificaten voor wederzijdse
- HTTP-trigger op basis van logische Apps beveiligen met het maken van een IP-adres-whitelist die verwijst naar het IP-adres van de API Management. Dit voorkomt dat de logische app aanroepen vanuit het openbare internet zonder tussenkomst van eerste via API Management.

Deze verwijzende architectuur hebt u geleerd over het bouwen van een eenvoudige enterprise integratieplatform met behulp van Azure Integration Services.

## <a name="next-steps"></a>Volgende stappen

- [Bedrijfsintegratie met wachtrijen en -gebeurtenissen](logic-apps-architectures-enterprise-integration-with-queues-events.md)
