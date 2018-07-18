---
title: Azure Integration Services eenvoudig enterprise integration-referentiearchitectuur
description: Hierin wordt beschreven in de referentiearchitectuur die laat zien hoe u een eenvoudige enterprise integration-patroon met Azure Logic Apps en Azure API Management wilt implementeren.
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
ms.openlocfilehash: 982a5eabf8c6c3012a9b3e8fdbe2ff32ba439972
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39113589"
---
# <a name="reference-architecture-simple-enterprise-integration"></a>Referentie-architectuur: eenvoudige bedrijfsintegratie

De volgende referentiearchitectuur toont een set bewezen procedures die u kunt toepassen op een integratietoepassing die gebruikmaakt van Azure Integration Services. De architectuur kan fungeren als basis voor veel verschillende toepassingspatronen waarvoor HTTP APIs, werkstromen en orchestration.

![Architectuurdiagram - eenvoudige bedrijfsintegratie](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

*Er zijn veel mogelijke toepassingen voor de integratie van technologie. Ze variëren van een eenvoudige point-to-point-toepassing naar een volledige enterprise Azure Service Bus-toepassing. De architectuur-serie beschrijft de herbruikbare onderdelen die van toepassing zijn mogelijk voor het bouwen van een algemene integratietoepassing. Architecten moeten rekening houden met welke onderdelen ze nodig hebben om te implementeren voor hun toepassingen en infrastructuur.*

## <a name="architecture"></a>Architectuur

De architectuur heeft de volgende onderdelen:

- **Resourcegroep**. Een [resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) is een logische container voor Azure-resources.
- **Azure API Management**. [API Management](https://docs.microsoft.com/azure/api-management/) is een volledig beheerd platform dat wordt gebruikt om te publiceren, te beveiligen en te transformeren HTTP APIs.
- **Azure API Management-ontwikkelaarsportal**. Elk exemplaar van Azure API Management wordt geleverd met toegang tot de [ontwikkelaarsportal](https://docs.microsoft.com/azure/api-management/api-management-customize-styles). De ontwikkelaarsportal van API Management biedt u toegang tot documentatie en codevoorbeelden. U kunt API's testen in de portal voor ontwikkelaars.
- **Azure Logic Apps**. [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) is een serverloos platform dat wordt gebruikt voor het bouwen van enterprise-werkstroom en het integratieaccount.
- **Connectors**. Logic Apps gebruikt [connectors](https://docs.microsoft.com/azure/connectors/apis-list) verbinding maken met veel gebruikte services. Logic Apps is al honderden verschillende connectors, maar u kunt ook een aangepaste connector maken.
- **IP-adres**. De Azure API Management-service heeft een vaste openbare [IP-adres](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) en de domeinnaam van een. De domeinnaam is een subdomein van azure-api.net, zoals contoso.azure-api.net. Logic Apps en Service Bus moet u ook een openbaar IP-adres hebben. In deze architectuur beperken we echter toegang voor het aanroepen van Logic Apps-eindpunten alleen het IP-adres van API Management (voor beveiliging). Aanroepen naar Service Bus worden beveiligd door een shared access signature (SAS).
- **Azure DNS**. [Azure DNS](https://docs.microsoft.com/azure/dns/) is een hostingservice voor DNS-domeinen. Azure DNS biedt naamomzetting met behulp van de Microsoft Azure-infrastructuur. Door uw domeinen in Azure hosten, kunt u uw DNS-records beheren met behulp van dezelfde referenties, API's, hulpprogramma's en facturering, gebruikt u voor uw andere Azure-services. Maak DNS-records die de aangepaste domeinnaam aan het IP-adres toewijzen voor het gebruik van een aangepaste domeinnaam, zoals contoso.com. Zie voor meer informatie, [een aangepaste domeinnaam configureren in API Management](https://docs.microsoft.com/en-us/azure/api-management/configure-custom-domain).
- **Azure Active Directory (Azure AD)**. Gebruik [Azure AD](https://docs.microsoft.com/azure/active-directory/) of een andere id-provider voor verificatie. Azure AD-verificatie voor toegang tot API-eindpunten door door te geven biedt een [JSON Web Token voor API Management](https://docs.microsoft.com/azure/api-management/policies/authorize-request-based-on-jwt-claims) om te valideren. Azure AD kan beveiligde toegang tot de API Management-ontwikkelaarsportal (alleen Standard en Premium-lagen).

De architectuur heeft enkele patronen die essentieel voor de werking ervan zijn:

- Samengestelde API's zijn gebouwd met behulp van logic apps. Ze indelen aanroepen voor software als een service (SaaS)-systemen, Azure-services en API's die worden gepubliceerd naar API Management. [Logische apps worden ook gepubliceerd](https://docs.microsoft.com/azure/api-management/import-logic-app-as-api) via de API Management-ontwikkelaarsportal.
- Toepassingen Azure AD gebruiken om [verkrijgen van een OAuth 2.0-beveiligingstoken](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) die is vereist voor toegang tot een API.
- Met Azure API Management [valideert het beveiligingstoken](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) en geeft vervolgens de aanvraag door naar de back-end-API of de logische app.

## <a name="recommendations"></a>Aanbevelingen

Uw specifieke vereisten kunnen afwijken van de algemene architectuur die worden beschreven in dit artikel. Gebruik de aanbevelingen in deze sectie als uitgangspunt.

### <a name="azure-api-management-tier"></a>Azure API Management-laag

Gebruik de API Management Basic, Standard of Premium-lagen. De lagen bieden van een service level agreement (SLA) van productie en ondersteuning voor scale-out binnen de Azure-regio (het aantal eenheden verschilt per laag). De Premium-laag biedt ook ondersteuning voor scale-out meerdere Azure-regio's. Baseer de laag die u op het niveau van de vereiste doorvoer hebt kiest en de functie ingesteld. Zie voor meer informatie, [API Management-prijzen](https://azure.microsoft.com/pricing/details/api-management/).

U betaalt voor alle exemplaren van API Management wanneer ze worden uitgevoerd. Als u hebt uitgebreid en hoeft dat niveau van de prestaties van de tijd, kunt u profiteren van de API Management facturering per uur en omlaag schalen.

### <a name="logic-apps-pricing"></a>Prijzen voor Logic Apps

Logic Apps gebruikt een [serverloze](logic-apps-serverless-overview.md) model. Facturering wordt berekend op basis van de actie en connector kan worden uitgevoerd. Zie voor meer informatie, [prijzen voor Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/). Er zijn momenteel geen laag overwegingen voor logische Apps.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Logic Apps voor asynchrone API-aanroepen

Logische Apps werkt het beste in scenario's waarvoor geen lage latentie is vereist. Bijvoorbeeld, het beste werkt voor asynchrone of semi-langlopende API-aanroepen. Als met lage latentie is vereist (bijvoorbeeld een aanroep waarmee een gebruikersinterface wordt geblokkeerd), raden wij die API of bewerking implementeren met behulp van een andere technologie. Gebruik bijvoorbeeld Azure Functions of een Web-API die u implementeert met behulp van Azure App Service. Nog steeds wordt aangeraden dat u de consumenten API naar API front-met behulp van API Management.

### <a name="region"></a>Regio

Richt API Management en logische Apps in dezelfde regio voor minimale netwerklatentie. Over het algemeen kiest u de regio die zich het dichtst bij uw gebruikers.

De resourcegroep heeft ook een regio. De regio geeft aan waar de metagegevens voor de implementatie worden opgeslagen en waar de sjabloon voor de implementatie van wordt uitgevoerd. Plaats de resourcegroep en de daarbij behorende bronnen in dezelfde regio beschikbaarheid verbeteren tijdens de implementatie.

## <a name="scalability"></a>Schaalbaarheid

API Management-beheerders toe te voegen [cachebeleidsregels](../api-management/api-management-howto-cache.md) eventueel verhogen van de schaalbaarheid van de service. Opslaan in cache helpt ook bij de belasting van back-endservices verminderen.

Azure API Management Basic, Standard en Premium-lagen kunnen worden uitgebreid in een Azure-regio te bieden grotere capaciteit. Beheerders kunnen gebruikmaken van de **capaciteit metriek** optie in de **metrische gegevens** menu om te analyseren van het gebruik van hun service en klikt u vervolgens omhoog of omlaag schalen indien van toepassing.

Aanbevelingen voor het schalen van een API Management-service:

- Schaalbehoeften rekening te houden met de patronen in het netwerkverkeer. Klanten met meer vluchtige verkeerspatronen hebben groter verhoogde capaciteit nodig.
- Consistente capaciteit dan 66% kan wijzen op een nodig om omhoog te schalen.
- Consistente capaciteit dan 20% kan wijzen op een kans om omlaag te schalen.
- Het is altijd aanbevolen om load-test uw API Management-service met een representatieve workload voordat u de belasting in de productieomgeving inschakelt.

Services voor Premium-laag kunnen worden uitgebreid over meerdere Azure-regio's. Klanten die implementeren door services schalen over meerdere Azure-regio's krijgen een hogere SLA (99,95% ten opzichte van 99,9%) en services in de buurt van gebruikers in meerdere regio's kunnen inrichten.

De Logic Apps serverloze model betekent beheerders dat hoeft te plannen voor schaalbaarheid van de service. De service wordt automatisch geschaald om te voldoen aan de vraag.

## <a name="availability"></a>Beschikbaarheid

De SLA voor Azure API Management is momenteel 99,9% voor de lagen Basic, Standard en Premium. Premium-laag configuraties met implementatie van ten minste één eenheid in twee of meer regio's hebben een SLA van 99,95%.

De SLA voor Azure Logic Apps is momenteel 99,9%.

### <a name="backups"></a>Back-ups

Uw Azure API Management-configuratie moet [regelmatig back-ups van](../api-management/api-management-howto-disaster-recovery-backup-restore.md) (op basis van regelmaat van wijzigen). Back-bestanden moeten worden opgeslagen in een locatie of een Azure-regio die wijkt af van waar de service zich bevindt. Klanten kunnen vervolgens een van twee opties voor hun strategie voor noodherstel kiezen:

- Een nieuw API Management-exemplaar is ingericht in een gebeurtenis van het herstel na noodgevallen, de back-up wordt hersteld naar het nieuwe exemplaar en DNS-records zijn repointed.
- Klanten houden een passieve kopie van de service in een andere Azure-regio (aanvullende kosten). Back-ups worden regelmatig teruggezet naar de kopie. In een gebeurtenis van het herstel na noodgevallen moeten alleen de DNS-records worden repointed voor het herstellen van de service.

Omdat logic apps kunnen snel opnieuw worden gemaakt en zonder server worden, worden ze ondersteund door een kopie van de bijbehorende Azure Resource Manager-sjabloon opslaat. Sjablonen kunnen worden opgeslagen in broncodebeheer en deze kunnen worden geïntegreerd met het proces voor continue integratie/continue implementatie (CI/CD) van een klant.

Als een logische app die is gepubliceerd via API Management wordt verplaatst naar een ander datacenter, werkt u de locatie van de app. Voor het bijwerken van de app-locatie, gebruikt u een eenvoudige PowerShell-script om bij te werken de **back-end** eigenschap van de API.

## <a name="manageability"></a>Beheerbaarheid

Maak afzonderlijke resourcegroepen voor productie, ontwikkeling en testomgevingen. Afzonderlijke resourcegroepen kunt u eenvoudiger implementaties beheren, testimplementaties verwijderen en toegangsrechten toewijzen.

Wanneer u resources aan resourcegroepen toewijst, houd rekening met de volgende factoren:

- **Levenscyclus**. In het algemeen plaats resources die dezelfde levenscyclus te in dezelfde resourcegroep bevinden hebben.
- **Toegang tot**. U kunt [op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md) (RBAC) om toe te passen van beleidsregels voor toegang tot de resources in een groep.
- **Facturering**. U kunt updatepakket kosten voor de resourcegroep bekijken.
- **Prijscategorie voor API Management**. We raden u aan de Developer-laag gebruiken voor ontwikkeling en testomgevingen. Voor Pre-productie, wordt aangeraden een replica van de productie-omgeving implementeren tests uitvoert en vervolgens naar de kosten minimaliseren wordt afgesloten.

Zie voor meer informatie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="deployment"></a>Implementatie

Het is raadzaam dat u [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md) API Management en logische Apps te implementeren. Sjablonen wordt het eenvoudiger implementaties automatiseren via PowerShell of Azure CLI.

Het is raadzaam om de toevoeging van Azure API Management en een afzonderlijke logische apps in hun eigen, afzonderlijke Resource Manager-sjablonen. Als u afzonderlijke sjablonen gebruikt, kunt u de resources in bronbeheersystemen kunt opslaan. U kunt ook de resources implementeren samen of afzonderlijk als onderdeel van een CI/CD-implementatieproces.

### <a name="versions"></a>Versies

Telkens wanneer u een configuratie wijzigen in een logische app (of een update via Resource Manager-sjabloon implementeren), een kopie van deze versie wordt opgeslagen voor uw gemak (alle versies waarvoor een uitvoeringsgeschiedenis bewaard). U kunt deze versies historische wijzigingen bijhouden en een versie om te worden van de huidige configuratie van de logische app promoten. Bijvoorbeeld, kunt u effectief terugdraaien een logische app.

API Management heeft twee verschillende (maar gratis) [versiebeheer concepten](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/):

- De versies die worden gebruikt voor uw consumenten API met een keuze uit de API die ze kunnen gebruiken op basis van hun behoeften (bijvoorbeeld v1, v2 of bèta, productie).
- Wijzigingen die de API-beheerders kunnen veilig wijzigingen aanbrengen aan een API en vervolgens de wijzigingen implementeert voor gebruikers met optionele opmerkingen.

Het is een goed idee om te overwegen revisies in API Management als een manier om veilig wijzigingen aanbrengen, een wijzigingen bijhouden en consumenten API op de hoogte van deze wijzigingen aanbrengen in de context van de implementatie. Een revisie kan worden gemaakt in een ontwikkelingsomgeving en tussen omgevingen met behulp van Resource Manager-sjablonen worden geïmplementeerd.

Hoewel revisies kunt u een API testen voordat u het 'huidige' en toegankelijk is voor gebruikers, aanbevolen niet via dit mechanisme voor belasting of het testen van de integratie. Gebruik in plaats daarvan afzonderlijke test of omgevingen vóór productie.

### <a name="configuration"></a>Configuratie

Nooit controleren in de wachtwoorden, toegangssleutels of verbindingsreeksen naar broncodebeheer. Als deze waarden vereist zijn, moet u de juiste techniek gebruiken om te implementeren en beveiligen van deze waarden. 

In Logic Apps, moeten alle gevoelige waarden die nodig zijn in de logische app (die in de vorm van een verbinding kan niet worden gemaakt) worden opgeslagen in Azure Key Vault en waarnaar wordt verwezen op basis van een Resource Manager-sjabloon. Ook raadzaam implementatieparameters sjabloon en parameterbestanden gebruiken voor elke omgeving. Zie voor meer informatie, [beveiliging van de parameters en invoer binnen een werkstroom](logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

In API Management, geheimen worden beheerd met behulp van objecten met de naam *benoemde waarden* of *eigenschappen*. De objecten opslaan veilig van waarden die kunnen worden geopend in API Management-beleidsregels. Zie voor meer informatie, [geheimen in API Management beheren](../api-management/api-management-howto-properties.md).

### <a name="diagnostics-and-monitoring"></a>Diagnose en controle

[API Management](../api-management/api-management-howto-use-azure-monitor.md) en [Logic Apps](logic-apps-monitor-your-logic-apps.md) ondersteunen beide operationele bewaking via [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure Monitor biedt informatie op basis van de metrische gegevens die zijn geconfigureerd voor elke service. Azure Monitor is standaard ingeschakeld.

Deze opties zijn ook beschikbaar voor elke service:

- Logic Apps-logboeken kunnen worden verzonden naar [Azure Log Analytics](logic-apps-monitor-your-logic-apps-oms.md) voor uitgebreidere analyse en dashboarding.
- API Management biedt ondersteuning voor configureren van Azure Application Insights voor DevOps bewaking.
- Biedt ondersteuning voor API Management de [Power BI-oplossingssjabloon voor aangepaste API analytics](http://aka.ms/apimpbi). Klanten kunnen de oplossingssjabloon gebruiken om hun eigen aangepaste analytics-oplossing te maken. Rapporten zijn beschikbaar in Power BI voor zakelijke gebruikers.

## <a name="security"></a>Beveiliging

Deze sectie vindt u beveiligingsoverwegingen die specifiek voor de Azure-services die in dit artikel worden beschreven zijn, en die zijn geïmplementeerd in de architectuur, zoals wordt beschreven. Het is geen volledige lijst met aanbevolen procedures voor beveiliging.

- Op rollen gebaseerd toegangsbeheer (RBAC) gebruiken om te controleren of de juiste toegangsniveaus voor gebruikers.
- Beveilig openbare API-eindpunten in API Management met behulp van OAuth/OpenID Connect. Als u wilt beveiligen openbare API-eindpunten, configureren van een id-provider en het toevoegen van een beleid voor validatie van JSON Web Token (JWT).
- Verbinding maken met back-end-services van API Management met behulp van wederzijdse certificaten.
- HTTP-trigger op basis van logische apps beveiligen met het maken van een goedgekeurde IP-adres-adressen die naar de API Management-IP-adres verwijst. Een goedgekeurde IP-adres wordt voorkomen dat de logische app aanroepen vanuit het openbare internet zonder tussenkomst van eerste via API Management.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [bedrijfsintegratie met wachtrijen en -gebeurtenissen](logic-apps-architectures-enterprise-integration-with-queues-events.md).
