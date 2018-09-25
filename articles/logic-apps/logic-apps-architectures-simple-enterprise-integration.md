---
title: 'Patroon voor eenvoudige enterprise integration-architectuur: Azure Integration Services'
description: De verwijzing van deze architectuur laat zien hoe u een eenvoudige enterprise integration-patroon kunt implementeren met behulp van Azure Logic Apps en Azure API Management
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: mattfarm
ms.author: mattfarm
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/15/2018
ms.openlocfilehash: 5ed6fa9f514bae3ea651edba6702714e2680091f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955942"
---
# <a name="simple-enterprise-integration-architecture"></a>Eenvoudige enterprise integration-architectuur

Dit artikel beschrijft de architectuur van een enterprise-integratie die gebruikmaakt van bewezen procedures die u op een toepassing voor gegevensintegratie toepassen kunt bij het gebruik van Azure Integration Services. U kunt deze architectuur gebruiken als basis voor veel verschillende toepassingspatronen waarvoor HTTP APIs, werkstromen en orchestration.

![Architectuurdiagram - eenvoudige bedrijfsintegratie](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

Deze reeks wordt uitgelegd dat de herbruikbare onderdelen die van toepassing zijn mogelijk voor het bouwen van een algemene integratietoepassing. Omdat technologie voor integratie veel mogelijke toepassingen heeft, variërend van eenvoudige point-to-point apps tot volledige enterprise Azure Service Bus-apps, kunt u welke onderdelen die u wilt implementeren voor uw apps en infrastructuur.

## <a name="architecture-components"></a>Architectuur van onderdelen

Deze enterprise integration-architectuur bevat de volgende onderdelen:

- **Resourcegroep**: A [resourcegroep](../azure-resource-manager/resource-group-overview.md) is een logische container voor Azure-resources.

- **Met Azure API Management**: de [API Management](https://docs.microsoft.com/azure/api-management/) -service is een volledig beheerd platform voor het publiceren, te beveiligen en te transformeren, HTTP APIs.

- **Azure API Management-ontwikkelaarsportal**: elk exemplaar van Azure API Management biedt toegang tot de [ontwikkelaarsportal](../api-management/api-management-customize-styles.md). Deze portal geeft u toegang tot documentatie en codevoorbeelden. U kunt ook API's testen in de portal voor ontwikkelaars.

- **Met Azure Logic Apps**: [Logic Apps](../logic-apps/logic-apps-overview.md) is een serverloos platform voor het bouwen van enterprise-werkstromen en integraties.

- **Connectors**: maakt gebruik van Logic Apps [connectors](../connectors/apis-list.md) voor verbinding maken met veel services gebruikt. Logic Apps biedt honderden connectors, maar u kunt ook een aangepaste connector maken.

- **IP-adres**: de Azure API Management-service heeft een vaste openbare [IP-adres](../virtual-network/virtual-network-ip-addresses-overview-arm.md) en de domeinnaam van een. De standaarddomeinnaam is een subdomein van azure-api.net, bijvoorbeeld contoso.azure-api.net, maar u kunt ook configureren [aangepaste domeinen](../api-management/configure-custom-domain.md). Logic Apps en Service Bus moet u ook een openbaar IP-adres hebben. Voor beveiliging, echter beperkt deze architectuur toegang voor het aanroepen van Logic Apps-eindpunten alleen het IP-adres van API Management. Aanroepen naar Service Bus worden beveiligd door een shared access signature (SAS).

- **Azure DNS**: [Azure DNS](https://docs.microsoft.com/azure/dns/) is een hostingservice voor DNS-domeinen. Azure DNS biedt naamomzetting met behulp van de Microsoft Azure-infrastructuur. Door uw domeinen in Azure hosten, kunt u uw DNS-records beheren met behulp van dezelfde referenties, API's, hulpprogramma's en facturering, gebruikt u voor uw andere Azure-services. Maak DNS-records die de aangepaste domeinnaam aan het IP-adres toewijzen voor het gebruik van een aangepaste domeinnaam, zoals contoso.com. Zie voor meer informatie, [een aangepaste domeinnaam configureren in API Management](../api-management/configure-custom-domain.md).

- **Azure Active Directory (Azure AD)**: U kunt [Azure AD](https://docs.microsoft.com/azure/active-directory/) of een andere id-provider voor verificatie. Azure AD-verificatie voor toegang tot API-eindpunten door door te geven biedt een [JSON Web Token voor API Management](../api-management/policies/authorize-request-based-on-jwt-claims.md) om te valideren. Voor de lagen standaard en Premium, kunt Azure AD beveiligen van toegang tot de API Management-ontwikkelaarsportal.

## <a name="patterns"></a>Patronen 

Deze architectuur maakt gebruik van sommige patronen die essentieel voor bewerking zijn:

* Samengestelde API's zijn gebouwd met behulp van logic apps, die het organiseren van aanroepen naar software als een service (SaaS)-systemen, Azure-services en API's die worden gepubliceerd naar API Management. Logische apps zijn ook [gepubliceerd via de API Management-ontwikkelaarsportal](../api-management/import-logic-app-as-api.md).

* Toepassingen gebruiken Azure AD voor [verkrijgen van een OAuth 2.0-beveiligingstoken](../api-management/api-management-howto-protect-backend-with-aad.md) die is vereist voor toegang tot een API.

* Met Azure API Management [valideert het beveiligingstoken](../api-management/api-management-howto-protect-backend-with-aad.md) en geeft vervolgens de aanvraag door naar de back-end-API of de logische app.

## <a name="recommendations"></a>Aanbevelingen

Uw specifieke vereisten kunnen afwijken van de algemene architectuur die in dit artikel wordt beschreven. Gebruik de aanbevelingen in deze sectie als uitgangspunt.

### <a name="azure-api-management-tier"></a>Azure API Management-laag

Gebruik de API Management Basic, Standard of Premium-lagen. Deze lagen bieden van een service level agreement (SLA) van productie en ondersteuning voor scaleout binnen de Azure-regio. Het aantal eenheden varieert per laag. De Premium-laag biedt ook ondersteuning voor scaleout meerdere Azure-regio's. Kies uw laag op basis van de reeks functies en het niveau van de vereiste doorvoer. Zie voor meer informatie, [API Management-prijzen](https://azure.microsoft.com/pricing/details/api-management/).

U betaalt voor alle exemplaren van API Management wanneer ze worden uitgevoerd. Als u hebt uitgebreid en hoeft dat niveau van de prestaties van de tijd, kunt u profiteren van de API Management facturering per uur en omlaag schalen.

### <a name="logic-apps-pricing"></a>Prijzen voor Logic Apps

Logic Apps gebruikt een [serverloze](../logic-apps/logic-apps-serverless-overview.md) model. Facturering wordt berekend op basis van de actie en connector kan worden uitgevoerd. Zie voor meer informatie, [prijzen voor Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/). Er zijn momenteel geen laag overwegingen voor logische Apps.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Logic Apps voor asynchrone API-aanroepen

Logische Apps werkt het beste in scenario's waarvoor geen lage latentie is vereist. Bijvoorbeeld, Logic Apps werkt het beste voor asynchrone of semi-langlopende API-aanroepen. Als lage latentie is vereist, bijvoorbeeld een aanroep waarmee een gebruikersinterface, wordt geblokkeerd implementeren van uw API of bewerking met behulp van een andere technologie. Gebruik bijvoorbeeld Azure Functions of een Web-API die u implementeert met behulp van Azure App Service. Gebruik API Management om de API aan uw consumenten API front-end.

### <a name="region"></a>Regio

Kies voor minimale netwerklatentie, dezelfde regio voor API Management, Logic Apps en Service Bus. In het algemeen kiest u de regio die zich het dichtst bij uw gebruikers.

De resourcegroep heeft ook een regio. Deze regio geeft aan waar de voor het opslaan van metagegevens voor de implementatie en waar om uit te voeren van de sjabloon voor de implementatie. De beschikbaarheid te verbeteren tijdens de implementatie, plaatst u de resourcegroep en resources in dezelfde regio.

## <a name="scalability"></a>Schaalbaarheid

Als u wilt de schaalbaarheid verhogen bij het beheren van een API Management-service, toevoegen [cachebeleidsregels](../api-management/api-management-howto-cache.md) indien van toepassing. Opslaan in cache helpt ook bij de belasting van back-endservices verminderen.

Als u wilt bieden grotere capaciteit, kunt u Azure API Management Basic, Standard en Premium-lagen in een Azure-regio uitschalen. Voor het analyseren van het gebruik van uw service, op de **metrische gegevens** in het menu het **capaciteit metriek** optie en klikt u vervolgens omhoog of omlaag schalen indien van toepassing.

Aanbevelingen voor het schalen van een API Management-service:

- Overweeg patronen in het netwerkverkeer wanneer schalen. Klanten met meer vluchtige verkeerspatronen meer capaciteit nodig hebt.

- Consistente capaciteit die groter is dan 66% kan wijzen op een nodig om omhoog te schalen.

- Consistente de opslagcapaciteit die onder 20% kan duiden op een kans om omlaag te schalen.

- Voordat u de belasting in de productieomgeving inschakelt, altijd load-test uw API Management-service met een representatieve workload.

U kunt services voor Premium-laag uitschalen over meerdere Azure-regio's. Als u implementeert met services schalen over meerdere Azure-regio's, krijgt u een hogere SLA (99,95% ten opzichte van 99,9%) en services inrichten in de buurt van gebruikers in meerdere regio's.

De Logic Apps serverloze model betekent beheerders dat hoeft te plannen voor schaalbaarheid van de service. De service wordt automatisch geschaald om te voldoen aan de vraag.

## <a name="availability"></a>Beschikbaarheid

* De service level agreement (SLA) voor Azure API Management is voor de lagen Basic, Standard en Premium, momenteel 99,9%. Voor premium-laag configuraties met een implementatie met minimaal één eenheid in twee of meer regio's, is de SLA van 99,95%.

* De SLA voor Azure Logic Apps wordt momenteel 99,9%.

### <a name="backups"></a>Back-ups

Op basis van regelmaat veranderen, [regelmatig een back-up maken van](../api-management/api-management-howto-disaster-recovery-backup-restore.md) uw Azure API Management-configuratie. Uw back-upbestanden Store in een locatie of een Azure-regio die wijkt af van waar de service zich bevindt. U kunt vervolgens een van beide opties als uw strategie voor noodherstel kiezen:

* In een gebeurtenis van het herstel na noodgevallen, inrichten van een nieuw API Management-exemplaar en de back-up herstellen naar het nieuwe exemplaar implementatiedomein van de DNS-records.

* Houd een passieve kopie van uw service in een andere Azure-regio, die worden extra kosten in rekening gebracht. Regelmatig back-ups naar dat exemplaar herstellen. Als u wilt herstellen van de service tijdens een herstel na noodgevallen gebeurtenis, moet u alleen de DNS-records implementatiedomein.

Omdat u kunt snel opnieuw maken logic apps, die zonder server, een back-up door op te slaan een kopie van de bijbehorende Azure Resource Manager-sjabloon. U kunt sjablonen opslaan in broncodebeheer, en u sjablonen kunt integreren met uw continue integratie/continue implementatie (CI/CD)-proces.

Als u een logische app via Azure API Management hebt gepubliceerd, en dat logische app wordt verplaatst naar een ander datacenter, de locatie van de app bijwerken U kunt uw API's bijwerken **back-end** eigenschap met behulp van een eenvoudige PowerShell-script.

## <a name="manageability"></a>Beheerbaarheid

Maak afzonderlijke resourcegroepen voor productie, ontwikkeling en testomgevingen. Afzonderlijke resourcegroepen kunt u eenvoudiger implementaties beheren, testimplementaties verwijderen en toegangsrechten toewijzen.

Wanneer u resources aan resourcegroepen toewijst, houd rekening met deze factoren:

* **Levenscyclus**: plaats In het algemeen resources die dezelfde levenscyclus te in dezelfde resourcegroep bevinden hebben.

* **Toegang tot**: om toe te passen beleidsregels voor toegang tot de resources in een groep, kunt u [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md).

* **Facturering**: U kunt updatepakket kosten voor de resourcegroep bekijken.

* **Prijscategorie voor API Management**: de Developer-laag gebruiken voor uw ontwikkel- en testomgevingen. U kosten kunt minimaliseren tijdens de testfase vóór productie, een replica van de productie-omgeving implementeren, Voer uw tests uit en sluit vervolgens af.

Zie voor meer informatie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="deployment"></a>Implementatie

* Voor het implementeren van API Management en Logic Apps, gebruikt u de [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md). Sjablonen eenvoudiger implementaties automatiseren met behulp van PowerShell of Azure CLI.

* API Management en een afzonderlijke logic-apps in hun eigen afzonderlijke Resource Manager-sjablonen worden geplaatst. U kunt de resources met behulp van afzonderlijke sjablonen, opslaan in bronbeheersystemen. Vervolgens kunt u deze sjablonen implementeren samen of afzonderlijk als onderdeel van een continue integratie/continue implementatie (CI/CD)-proces.

### <a name="versions"></a>Versies

Telkens wanneer u een logische app-configuratie wijzigen of een update via Resource Manager-sjabloon implementeert Azure houdt een kopie van die versie voor uw gemak en houdt u alle versies waarvoor een uitvoeringsgeschiedenis. U kunt deze versies gebruiken voor het bijhouden van wijzigingen van historische of promoveren, een versie als de huidige configuratie van de logische app. Bijvoorbeeld, kunt u effectief terugdraaien een logische app.

Azure API Management heeft deze verschillende maar aanvullende [versiebeheer concepten](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/):

* Versies die uw consumenten API bieden de mogelijkheid om te kiezen op basis van hun behoeften, bijvoorbeeld API-versie, v1, v2, beta of productie

* Revisies waarmee beheerders van de API voor het veilig wijzigingen aanbrengen in een API en vervolgens de wijzigingen implementeert voor gebruikers met optionele opmerkingen

Voor de implementatie, rekening houden met revisies van API Management als een manier om veilig aanbrengen, een wijzigingen bijhouden en deze wijzigingen naar uw API's klanten communiceren. U kunt een wijziging doorvoert in een ontwikkelingsomgeving en die wijziging in andere omgevingen implementeert met behulp van Resource Manager-sjablonen.

Hoewel u wijzigingen gebruiken kunt voor het testen van een API voordat u deze wijzigingen 'huidige' en toegankelijk is voor gebruikers, wordt deze methode wordt niet aanbevolen voor belasting of het testen van de integratie. Gebruik in plaats daarvan afzonderlijke test of omgevingen vóór productie.

### <a name="configuration-and-sensitive-information"></a>Configuratie- en gevoelige gegevens

Schakel wachtwoorden, toegangstoetsen of verbindingsreeksen nooit in broncodebeheer in. Als deze waarden vereist zijn, beveiligen en implementeren van deze waarden met behulp van de juiste technieken. 

In Logic Apps, worden als een logische app gevoelige waarden die u kunt geen binnen een verbinding maken vereist, die waarden opslaan in Azure Key Vault en ernaar te verwijzen in Resource Manager-sjabloon. Implementatie sjabloonparameters en parameterbestanden gebruiken voor elke omgeving. Zie voor meer informatie, [beveiliging van de parameters en invoer binnen een werkstroom](../logic-apps/logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

API Management geheimen beheren met behulp van objecten met de naam *benoemde waarden* of *eigenschappen*. Deze objecten waarden waartoe u toegang hebt via API Management-beleidsregels voor het veilig opslaan. Zie voor meer informatie, [geheimen in API Management beheren](../api-management/api-management-howto-properties.md).

## <a name="diagnostics-and-monitoring"></a>Diagnose en controle

U kunt [Azure Monitor](../azure-monitor/overview.md) voor operationele bewaking in beide [API Management](../api-management/api-management-howto-use-azure-monitor.md) en [Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md). Azure Monitor vindt u informatie op basis van de metrische gegevens voor elke service geconfigureerd en is standaard ingeschakeld.

Elke service heeft ook de volgende opties:

* Voor meer gedetailleerde analyse en dashboarding, kunt u Logic Apps-logboeken te verzenden [Azure Log Analytics](logic-apps-monitor-your-logic-apps-oms.md).

* U kunt voor het bewaken van DevOps, Azure Application Insights configureren voor API Management.

* Biedt ondersteuning voor API Management de [Power BI-oplossingssjabloon voor aangepaste API analytics](http://aka.ms/apimpbi). U kunt deze oplossingssjabloon gebruiken voor het maken van uw eigen analyseoplossing. Voor zakelijke gebruikers maakt Power BI rapporten beschikbaar.

## <a name="security"></a>Beveiliging

Hoewel alle aanbevolen beveiligingsprocedures niet volledig wordt beschreven in deze lijst, vindt hier u enkele beveiligingsoverwegingen die van toepassing zijn op de Azure-services geïmplementeerd in de architectuur die in dit artikel wordt beschreven:

* Gebruiken om te zorgen dat gebruikers de juiste toegangsniveaus hebben, op rollen gebaseerd toegangsbeheer (RBAC).

* Openbare API-eindpunten in API Management beveiligen met behulp van OAuth of OpenID Connect. Als u wilt beveiligen openbare API-eindpunten, een id-provider configureren en toevoegen van een beleid voor validatie van JSON Web Token (JWT).

* Verbinding maken met back-end-services van API Management met behulp van wederzijdse certificaten.

* HTTP-trigger op basis van logische apps beveiligen met het maken van een goedgekeurde IP-adres-adressen die naar de API Management-IP-adres verwijst. Een goedgekeurde IP-adres wordt voorkomen dat de logische app aanroepen vanuit het openbare internet zonder tussenkomst van eerste via API Management.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [bedrijfsintegratie met wachtrijen en -gebeurtenissen](../logic-apps/logic-apps-architectures-enterprise-integration-with-queues-events.md)
