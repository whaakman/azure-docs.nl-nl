---
title: Integratie van Azure Services - integratie van eenvoudige Enterprise
description: Referentiearchitectuur getoond hoe voor het implementeren van een patroon enterprise eenvoudige integratie met Azure Logic Apps en Azure API Management
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
ms.openlocfilehash: 17f591a470bf65d3c9365bca9c5ae2d5a6c9574f
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232319"
---
# <a name="simple-enterprise-integration---reference-architecture"></a>Eenvoudige Enterprise Integration - referentiearchitectuur

## <a name="overview"></a>Overzicht

Deze verwijzende architectuur wordt een reeks bewezen een integratietoepassing die gebruikmaakt van Azure Integration Services. Deze architectuur kan fungeren als basis van veel verschillende groepen van toepassingen patronen vereisen HTTP APIs, werkstroom- en orkestratielaag.

*Er zijn veel mogelijke toepassingen van integratie-technologie van de toepassing van een eenvoudige punt-naar-punt naar een volledige enterprise servicebus. Deze reeks architectuur stelt de herbruikbare onderdelen voor het bouwen van een integratietoepassing – architecten moeten rekening houden met de onderdelen die ze nodig voor de toepassingen en infrastructuur hebt.*

   ![Architectuurdiagram - eenvoudige enterprise-integratie](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

## <a name="architecture"></a>Architectuur

De architectuur heeft de volgende onderdelen:

- Resourcegroep. Een resourcegroep is een logische container voor Azure-resources.
- Azure API Management. Azure API Management is een volledig beheerd platform voor het publiceren, te beveiligen en te transformeren HTTP APIs.
- Azure API Management Developer-Portal. Elk exemplaar van Azure API Management wordt geleverd met een Portal voor ontwikkelaars die toegang geeft tot documentatie, codevoorbeelden en de mogelijkheid voor het testen van een API.
- Azure Logic Apps. Logic Apps is een zonder server platform voor het bouwen van enterprise-werkstroom en integratie.
- Connectors. Connectors worden gebruikt door Logic Apps verbinding maken met veelgebruikte services. Logic Apps honderden verschillende connectors al heeft, maar ze kunnen ook worden gemaakt met een aangepaste connector.
- IP-adres. De Azure API Management-service heeft een vaste openbare IP-adres en een domeinnaam. De domeinnaam is een subdomein van azure-api.net, zoals contoso. Azure-api.net. Logic Apps heeft ook een openbaar IP-adres; in deze architectuur beperken we echter toegang tot het aanroepen van Logic apps-eindpunten op alleen de IP-adres van API Management (voor beveiliging).
- Azure DNS. Azure DNS is een hosting service voor DNS-domeinen omzetten van namen met behulp van Microsoft Azure-infrastructuur. Door uw domeinen in Azure te hosten, kunt u uw DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering beheren als voor uw andere Azure-services. Maak DNS-records die de naam van het aangepaste domein aan het IP-adres toewijzen wilt gebruiken een aangepaste domeinnaam (zoals contoso.com). Zie voor meer informatie configureren een aangepaste domeinnaam in Azure API Management.
- Azure Active Directory (Azure AD). Gebruik Azure AD of een andere id-provider voor verificatie. Azure AD biedt verificatie met toegang tot API-eindpunten (door het doorgeven van een JSON Web Token voor API Management valideren) en toegang tot de Ontwikkelaarsportal van de API-beheer (alleen de lagen Standard en Premium) kunt beveiligen.

Deze architectuur heeft alle fundamentele patronen voor de bewerking:

1. Bestaande back-end HTTP APIs worden gepubliceerd via het API Management-Portal voor ontwikkelaars, zodat ontwikkelaars (ofwel intern gebruik binnen uw organisatie, externe of beide) aanroepen naar deze API's integreren in toepassingen.
2. Samengestelde API's zijn gebouwd met behulp van Logic Apps; organiseren aanroepen naar de SAAS-systemen, Azure-services en een API's gepubliceerd op API Management. De Logic Apps worden ook worden uitgegeven via het Ontwikkelaarsportal van API Management.
3. Toepassingen verkrijgen van een beveiligingstoken OAuth 2.0 nodig voor toegang tot een API met Azure Active Directory.
4. Azure API Management valideert het beveiligingstoken, en de aanvraag wordt doorgegeven aan de back-end API/logische App.

## <a name="recommendations"></a>Aanbevelingen

Uw vereisten kunnen afwijken van de architectuur die hier wordt beschreven. Gebruik de aanbevelingen in deze sectie als uitgangspunt.

### <a name="azure-api-management-tier"></a>Azure API Management-laag

Gebruik het Basic, Standard of Premium lagen omdat ze een productie-SLA bieden en ondersteuning voor scale-out in de Azure-regio (aantal eenheden verschilt per laag). Premium-laag ondersteunt scale-out ook over meerdere Azure-regio's. Basis van de laag die u hebt gekozen op het niveau van de doorvoer die is vereist en de functieset. Zie voor meer informatie [API Management-prijzen](https://azure.microsoft.com/pricing/details/api-management/).

Er worden in rekening gebracht voor alle exemplaren van API Management wanneer ze worden uitgevoerd. Als u hebt uitgebreid en hoeft niet dat niveau van de prestaties van de tijd, overweeg gebruik te maken van API Management van elk uur facturering en schaal omlaag.

### <a name="logic-apps-pricing"></a>Logische Apps prijzen

Logic Apps werkt als een [zonder server](logic-apps-serverless-overview.md) model – facturering wordt berekend op basis van de actie en de connector kan worden uitgevoerd. Zie [Logic Apps prijzen](https://azure.microsoft.com/pricing/details/logic-apps/) voor meer informatie. Er zijn momenteel geen laag overwegingen voor Logic Apps.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Logic Apps voor asynchrone API-aanroepen

Logic Apps werkt het beste in scenario's die een lage latentie: bijvoorbeeld asynchrone niet vereisen of halfautomatische lange uitgevoerd API-aanroepen. Als een lage latentie is vereist (bijvoorbeeld een aanroep die een gebruikersinterface blokkeert) wordt aangeraden implementeren die API of opnieuw met een andere technologie, zoals Azure Functions of geïmplementeerd met behulp van App Service-Web-API. Nog steeds het beste deze API fronted met behulp van API Management API consumenten.

### <a name="region"></a>Regio

API Management en Logic Apps in dezelfde regio als u wilt minimaliseren netwerklatentie inrichten. In het algemeen kiest u de regio die het meest in de buurt van uw gebruikers is.

De resourcegroep heeft ook een regio die wordt opgegeven waar de metagegevens voor de implementatie is opgeslagen, en waarop de sjabloon voor de implementatie van wordt uitgevoerd. Plaats de resourcegroep en de bijbehorende resources in dezelfde regio. Dit kan de beschikbaarheid verbeteren tijdens de implementatie.

## <a name="scalability-considerations"></a>Schaalbaarheidsoverwegingen

API Management-beheerders toe te voegen [cachebeleidsregels](../api-management/api-management-howto-cache.md) eventueel verhogen van de schaalbaarheid van de service en de belasting van de back-end-services te verminderen.

Azure API Management Basic, Standard en Premium-laag kunnen worden uitgebreid met in een Azure-regio op het aanbod van grotere capaciteit. Beheerders kunnen de metriek capaciteit in het menu metrische gegevens gebruiken voor het gebruik van hun service analyseren en opschalen of terugschroeven zo nodig.

Aanbevelingen voor het schalen van een API Management-service:

- Schaal moet rekening houden met verkeerspatronen – klanten met meer vluchtige verkeerspatronen hebben groter noodzaak van verhoogde capaciteit.
- Consistente capaciteit dan 66% kan erop wijzen moet worden uitgebreid.
- Consistente capaciteit minder dan 20% kan duiden op een kans om omlaag schalen.
- Altijd verdient het laden van uw API Management-service met een representatieve belasting te testen voordat u inschakelen in productie.

Services voor Premium-laag kunnen worden uitgebreid over meerdere Azure-regio's. Klanten implementeren op deze manier krijgt een hogere SLA (99,95% in plaats van 99,9%) en services in de buurt bevindt gebruikers in meerdere regio's kunnen inrichten.

Logische-Apps zonder server model betekent dat beheerders hoeven niet te extra overweging voor schaalbaarheid van de service. de service wordt automatisch geschaald om vraag te voldoen.

## <a name="availability-considerations"></a>Beschikbaarheidsoverwegingen

Op het moment van schrijven is service level agreement (SLA) voor Azure API Management 99,9% voor categorieën Basic, Standard en Premium. Premium-laag configuraties met implementatie van ten minste één eenheid in twee of meer regio's hebben een SLA van 99,95%.

Op het moment van schrijven is de serviceovereenkomst (SLA) voor Azure Logic Apps 99,9%.

### <a name="backups"></a>Back-ups

De configuratie van Azure API Management moet [regelmatig een back-up gemaakt](../api-management/api-management-howto-disaster-recovery-backup-restore.md) (op de juiste wijze op basis van regelmaat van wijziging), en de back-upbestanden die zijn opgeslagen in een locatie of een Azure-regio die verschillen van waar de service zich bevindt. Klanten kunnen Kies een van twee opties voor hun strategie voor Noodherstel:

1. Een nieuw exemplaar van API Management is ingericht, de back-up is hersteld en DNS-records zijn repointed in een DR-gebeurtenis.
2. Klanten houden een passieve kopie van de service in een andere Azure-regio (steeds extra kosten) back-ups regelmatig worden teruggezet naar het. In een DR-gebeurtenis moeten alleen DNS-records worden repointed voor het herstellen van de service.

Logic Apps kunnen zeer snel opnieuw gemaakt en zijn zonder server, worden ze ondersteund door een kopie van de gekoppelde Azure Resource Manager-sjabloon opslaan. Deze kunnen worden opgeslagen met source control/geïntegreerd in een klanten continue integratie/continue implementatie (CI/CD)-proces.

Logic Apps die zijn gepubliceerd via API Management moet de locaties bijgewerkt moeten ze verplaatsen naar een andere Datacenter. Dit kan worden uitgevoerd via een eenvoudige PowerShell-script voor het bijwerken van de back-end-eigenschap van de API.

## <a name="manageability-considerations"></a>Beheerbaarheidsoverwegingen

Maken van afzonderlijke resourcegroepen voor productie, ontwikkeling en testen omgevingen. Hiermee kunt u eenvoudiger implementaties beheren, testimplementaties verwijderen en toegangsrechten verlenen.
Wanneer u resources toewijst aan resourcegroepen, moet u het volgende overwegen:

- Levenscyclus. In het algemeen kunt u resources met dezelfde levenscyclus het beste in dezelfde resourcegroep onderbrengen.
- Toegang. U kunt [toegangsbeheer op basis van rollen](../role-based-access-control/overview.md) (RBAC) toe te passen van beleidsregels voor toegang tot bronnen in een groep.
- Facturering. U kunt de samengevoegde kosten voor de resourcegroep bekijken.
- Prijscategorie voor API Management – aangeraden categorie Developer voor ontwikkel- en testomgevingen. Voor preproductie, wordt aangeraden een replica van uw productieomgeving implementeren tests uitgevoerd, en vervolgens de kosten te minimaliseren wordt afgesloten.

Zie voor meer informatie [resourcegroep](../azure-resource-manager/resource-group-overview.md) overzicht.

### <a name="deployment"></a>Implementatie

Het is raadzaam dat u [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md) om zowel Azure API Management en Azure Logic Apps te implementeren. Sjablonen kunnen gemakkelijker implementaties via PowerShell of de Azure-opdrachtregelinterface (CLI) automatiseren.

We raden u aan als Azure API Management en een afzonderlijke Logic Apps in hun eigen afzonderlijke Resource Manager-sjablonen. Hierdoor kunnen ze opslaan in besturingselement bronsystemen. Deze sjablonen kunnen worden geïmplementeerd samen of afzonderlijk als onderdeel van het implementatieproces van een doorlopende integratie/continue (CI/CD).

### <a name="versions"></a>Versies

Telkens wanneer u een configuratie wijzigen in een logische App (of een update via Resource Manager-sjabloon implementeren), een kopie van die versie wordt bewaard voor uw gemak (alle versies waarvoor een uitvoeringsgeschiedenis worden behouden). U kunt deze versies historische wijzigingen bijhouden en ook Promoveer een versie om te worden van de huidige configuratie van de logische app; tijdens het doorzoeken van geval betekent dat u kunt effectief terugdraaien een logische App, bijvoorbeeld.

API Management heeft twee verschillende (maar gratis) [versioning concepten](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/):

- Versies die worden gebruikt om toegang te bieden uw consumenten API de keuze van de API ze kunnen verbruiken op basis van hun behoeften (bijvoorbeeld v1, v2 of bèta, productie).
- Revisies API beheerders veilig wijzigingen aanbrengen aan een API en implementeren voor gebruikers met optionele commentaar toestaan.

Een wijzigingen bijhouden in de context van implementatie – API Management revisies moeten worden beschouwd als een manier om veilig, wijzigingen aanbrengen en API consumenten bewust zijn van deze wijzigingen aanbrengen. Een revisie kan worden gemaakt in een ontwikkelingsomgeving en geïmplementeerd tussen andere omgevingen met behulp van Resource Manager-sjablonen.

Terwijl revisies kunnen worden gebruikt voor een API te testen voordat deze is gemaakt 'huidige' en voor gebruikers toegankelijk gemaakt, wordt niet aangeraden via dit mechanisme voor het laden of het testen van de integratie – afzonderlijke test of omgevingen vóór productie in plaats daarvan moeten worden gebruikt.

### <a name="configuration"></a>Configuratie

Nooit controleren wachtwoorden, toegangstoetsen of verbindingsreeksen in met resourcebeheer. Als ze nodig zijn, moet u de juiste techniek gebruiken om te implementeren en deze waarden te beveiligen. 

In Logic Apps moeten gevoelige waarden nodig in de logische app (die in de vorm van een verbinding kan niet worden gemaakt) worden opgeslagen in Azure Sleutelkluis en bedoelde van Resource Manager-sjabloon. We raden ook met behulp van de sjabloonparameters implementatie samen met de parameterbestanden voor elke omgeving. Meer hulp bij het [beveiligen parameters en invoer binnen een werkstroom](logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

Geheimen worden beheerd met behulp van objecten die worden aangeroepen met de naam/eigenschappen van waarden in API Management. Deze waarden die toegankelijk zijn voor het veilig opslaan in API Management-beleidsregels. Zie hoe [geheimen in API Management beheren](../api-management/api-management-howto-properties.md).

### <a name="diagnostics-and-monitoring"></a>Diagnose en controle

Beide [API Management](../api-management/api-management-howto-use-azure-monitor.md) en [Logic Apps](logic-apps-monitor-your-logic-apps.md) operationele bewaken via ondersteunen [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure Monitor is standaard ingeschakeld en op basis van de andere metrische gegevens voor elke service geconfigureerd informatie bieden.

Bovendien zijn er meer opties voor elke service:

- Logic Apps logboeken kunnen worden verzonden naar [logboekanalyse](logic-apps-monitor-your-logic-apps-oms.md) voor diepgaande analyse en dashboarding.
- API Management ondersteunt het configureren van Application Insights voor Dev Ops bewaking.
- API Management ondersteunt de [Power BI-oplossingssjabloon voor aangepaste API analytics](http://aka.ms/apimpbi). Deze oplossingssjabloon kan klanten hun eigen aangepaste analytics-oplossing maken met rapporten die beschikbaar zijn in Power BI voor zakelijke gebruikers.

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Deze sectie vindt beveiligingsoverwegingen die specifiek voor de Azure-services die worden beschreven in dit artikel wordt geïmplementeerd in de architectuur zijn, zoals wordt beschreven. Het is geen volledige lijst met aanbevolen procedures voor beveiliging.

- Op rollen gebaseerde toegangsbeheer (RBAC) gebruiken om te controleren of de juiste toegangsniveaus voor gebruikers.
- Beveiligde openbare API-eindpunten in API Management met behulp van OAuth/Open IDConnect. Dit doen door een id-provider configureren en het toevoegen van een beleid voor validatie van JWT.
- Verbinding maken met back-end-services van API Management-gebruik van wederzijdse certificaten
- HTTP-trigger gebaseerde Logic Apps beveiligen door het maken van een IP-adres goedgekeurde IP-adressen die verwijst naar de IP-adres van API Management. Dit voorkomt dat het aanroepen van de logische app via openbaar internet zonder eerste gaan via API Management.

Deze verwijzende architectuur hebt u geleerd hoe u een eenvoudige enterprise integration-platform met Azure Integration Services.

## <a name="next-steps"></a>Volgende stappen

* [Integratie met wachtrijen en gebeurtenissen in de onderneming](logic-apps-architectures-enterprise-integration-with-queues-events.md)
