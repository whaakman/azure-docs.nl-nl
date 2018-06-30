---
title: Beheeracties in Azure | Microsoft Docs
description: Meer informatie over computergebruik cloudservices die kunnen worden omhoog en omlaag geschaald om te voldoen aan de behoeften van uw toepassing of enterprise.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: TomSh
ms.openlocfilehash: 9c6509f25be7fe520a427e17ca1206e10f296fea
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110759"
---
# <a name="governance-in-azure"></a>Governance in Azure

Azure biedt u veel beveiligingsopties en de mogelijkheid om ze te beheren zodat u kunt voldoet aan de unieke vereisten van uw organisatie implementaties.

Azure-cloud governance verwijst naar de besluitvormingsprocessen automatiseert, criteria en beleidsregels die zijn betrokken bij de planning en architectuur, overname, bewerking, implementatie en beheer van cloudcomputing. Azure-cloud governance biedt een geïntegreerde controle en advies benadering voor het controleren en adviseren organisaties op het gebruik van de Azure-platform. 

U moet een diepgaande blik op de mensen, processen en technologieën nu duren in plaats voor het maken van een plan voor Azure-cloud toezicht. Vervolgens kunt u frameworks die eenvoudig samenstellen IT ter ondersteuning van consistent bedrijfsbehoeften en tegelijkertijd gebruikers de flexibiliteit om de functies van Azure te gebruiken.

## <a name="implementation-of-policies-processes-and-standards"></a>Implementatie van beleid, processen en standaarden 

Management heeft tot stand gebracht, rollen en verantwoordelijkheden over de implementatie van het beveiligingsbeleid van informatie en operationele continuïteit waakt in Azure. Azure management is verantwoordelijk voor het beheren van beveiliging en procedures voor bedrijfscontinuïteit binnen de respectieve teams (met inbegrip van derden). Daarnaast kunt u voldoen aan het beveiligingsbeleid, processen en standaarden.

### <a name="account-provisioning"></a>Account inrichten

Account hiërarchie definiëren, is een belangrijke stap om te gebruiken en de structuur van de Azure-services binnen een bedrijf. De bestuursstructuur core is. Klanten met een Enterprise Agreement (EA) kunnen onderverdelen in de omgeving aan de afdelingen, accounts en -abonnementen.

![Account inrichten](./media/governance-in-azure/security-governance-in-azure-fig1.png)

Als u een Enterprise Agreement hebt, kunt u overwegen [Azure labels](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) op het abonnementsniveau voor het definiëren van de hiërarchie. Een Azure-abonnement is de basiseenheid waarin alle resources. Het definieert ook diverse limieten in Azure, zoals het aantal kernen en resources. Abonnementen kunnen bevatten [resourcegroepen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), die bronnen kunnen bevatten. [Op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview) beginselen van toepassing op deze drie niveaus.

Elke enterprise verschilt. Voor bedrijven die buiten de onderneming kunt u de hiërarchie van het gebruik van Azure labels flexibiliteit in de rangschikking van Azure. Voordat u resources in Azure implementeert, moet u een hiërarchie model en wat de gevolgen van facturering, toegang tot bedrijfsbronnen en complexiteit.

### <a name="subscription-controls"></a>Abonnement-besturingselementen

Abonnementen bepalen hoe verbruik van resources is gerapporteerd en kosten in rekening gebracht. U kunt abonnementen voor afzonderlijke facturering en betaling instellen. Een Azure-account kan meerdere abonnementen hebt. Abonnementen kunnen worden gebruikt om te bepalen van het Azure brongebruik van meerdere afdelingen in een bedrijf.

Bijvoorbeeld, een bedrijf heeft IT, HR, en Marketing afdelingen en deze afdelingen verschillende projecten worden uitgevoerd. Het bedrijf kunt baseren de facturering op elke afdeling informatie over het gebruik van Azure-resources, zoals virtuele machines. Het bedrijf kan vervolgens bepalen voor de financiële gegevens van elke afdeling.

Azure-abonnementen tot stand brengen van de drie parameters:

- Unieke abonnements-ID

- Locatie van de facturering

- Aantal beschikbare resources

Voor een afzonderlijke bevatten die parameters één Microsoft-account-ID, een creditcardnummer en de volledige suite met Azure-services. Microsoft zorgt er verbruik limieten, afhankelijk van het abonnementstype.

Azure inschrijving hiërarchieën definiëren hoe de structuur van services binnen een Enterprise Agreement. De Enterprise Agreement portal kan klanten toegang tot Azure-resources die zijn gekoppeld aan een Enterprise Agreement op basis van flexibele hiërarchieën die kunnen aangepast aan de behoeften van een organisatie worden te verdelen. Het patroon van de hiërarchie moet overeenkomen met het beheer en de geografische structuur voor de bijbehorende facturerings- en -toegang van een organisatie.

De drie op hoog niveau hiërarchie zijn functionele, zakelijke eenheid, en geografische. Afdelingen zijn een administratieve constructie voor account groeperingen. Binnen elke afdeling kunnen de accounts worden toegewezen aan abonnementen, die silo voor facturerings- en diverse belangrijke limieten in Azure (bijvoorbeeld aantal VM's en storage-accounts maken).

![Abonnement-besturingselementen](./media/governance-in-azure/security-governance-in-azure-fig2.png)


Azure-abonnementen voor organisaties met een Enterprise Agreement, Ga als volgt een hiërarchie vier niveaus:

1. de beheerder van de Enterprise-inschrijving

2. afdeling beheerder

3. de eigenaar van account

4. Servicebeheerder

Deze hiërarchie bepaalt het volgende:

- Relatie van de facturering.

- Account-beheer.

- Toegang tot bronnen via RBAC.

- Grenzen:

  - Informatie over het gebruik en facturering (tariefkaart op basis van de aanbieding cijfers)

  - Limieten

  - Virtueel netwerk

- Koppelen aan Azure Active Directory (Azure AD). Een Azure AD-exemplaar kan worden gekoppeld aan veel abonnementen.

- De koppeling met een enterprise-inschrijving-account.

### <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

[RBAC](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) kan worden beheerd gedetailleerde toegang tot resources in Azure. Met behulp van RBAC kunt u alleen de hoeveelheid toegang verlenen dat gebruikers moeten uitvoeren van hun taken. Bedrijven moeten zich richten op uw werknemers de exacte machtigingen die ze nodig hebben. Te veel machtigingen tonen een account voor aanvallen. Te weinig machtigingen betekenen dat werknemers hun werk efficiënt kunnen niet ophalen. 

In plaats van iedereen geven onbeperkte machtigingen in uw Azure-abonnement of de bronnen, kunt u alleen bepaalde acties. U kunt bijvoorbeeld RBAC te laten een werknemer virtuele machines in een abonnement beheren terwijl een andere werknemer SQL-databases in hetzelfde abonnement beheert.

Om toegang te verlenen, kunt u rollen toewijzen aan gebruikers, groepen of toepassingen op een bepaalde scope. Het bereik van een roltoewijzing kan dit een abonnement, resourcegroep of één resource. Een rol die is toegewezen aan een bovenliggend bereik verleent toegang ook naar de onderliggende elementen erin opgenomen. Een gebruiker met toegang tot een resourcegroep kan bijvoorbeeld alle resources die deze, zoals websites, virtuele machines en subnetten bevat te beheren. U kunt maximaal 2.000 roltoewijzingen maken binnen elk abonnement.

Een rol is een verzameling machtigingen en RBAC heeft diverse ingebouwde rollen. De volgende ingebouwde rollen toepassen op alle brontypen:

- **Eigenaar** heeft volledige toegang tot alle bronnen, waaronder het recht op toegang aan anderen delegeren.

- **Inzender** kunt maken en beheren van alle soorten Azure-resources, maar kan geen toegang tot de overige verlenen.

- **Lezer** alle Azure-resources kunt weergeven.

De rest van de ingebouwde rollen in Azure toestaan van beheer van specifieke Azure-resources. De rol Inzender van de virtuele Machine kan bijvoorbeeld de gebruiker te maken en beheren van virtuele machines. Zie voor een lijst van de ingebouwde rollen en hun bewerkingen [ingebouwde RBAC-rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

RBAC ondersteunt beheerbewerkingen van de Azure-resources in de Azure portal en Azure Resource Manager-API's. In de meeste gevallen kan geen RBAC gegevensniveau bewerkingen voor Azure-resources autoriseren. Zie voor meer informatie over hoe RBAC wordt uitgebreid naar gegevensbewerkingen [begrijpen roldefinities](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-definitions).

Als de ingebouwde rollen niet voldoen aan de behoeften van uw specifieke toegang, kunt u [maakt u een aangepaste rol](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). Net als de ingebouwde rollen kunnen aangepaste rollen worden toegewezen aan gebruikers, groepen en toepassingen bij het abonnement, resourcegroep en bereik van de resource. U kunt ook aangepaste rollen maken met behulp van [Azure PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli), en de [REST-API](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

### <a name="resource-management"></a>Resourcebeheer

Azure biedt twee implementatiemodellen: [klassieke](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) en Azure Resource Manager.

In het klassieke model, wordt elke resource afzonderlijk bestaat. Er is geen manier om groepen verwante resources. U moet handmatig bij te houden welke resources gezamenlijk uw oplossing of de toepassing en vergeet niet om deze te beheren in een gecoördineerde benadering. De basiseenheid voor beheer, is het abonnement. Het is moeilijk om op te splitsen resources binnen een abonnement, wat tot het maken van een groot aantal abonnementen leidt.

Het implementatiemodel van Resource Manager omvat het concept van een [resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). Een resourcegroep is een container voor resources die een gemeenschappelijke levenscyclus delen. Alle resources voor de oplossing of alleen de resources die u wilt beheren als een groep kan bevatten. U bepaalt hoe resources worden toegewezen aan resourcegroepen op basis van wat voor uw organisatie het meest zinvol is.

Het implementatiemodel van Resource Manager biedt diverse voordelen:

- U kunt alle services voor uw oplossing als een groep implementeren, beheren en bewaken, in plaats van deze services afzonderlijk te verwerken.

- U kunt herhaaldelijk implementeren van uw oplossing gedurende de levenscyclus en erop vertrouwen dat uw resources worden geïmplementeerd in een consistente status.

- U kunt toegangsbeheer toepassen op alle resources in uw resourcegroep. Deze beleidsregels worden automatisch toegepast wanneer nieuwe resources worden toegevoegd aan de resourcegroep.

- U kunt tags toepassen op de resources om alle resources in uw abonnement op een logische manier te organiseren.

- U kunt JSON (JavaScript Object Notation) gebruiken voor het definiëren van de infrastructuur voor uw oplossing. Het JSON-bestand is in feite de Resource Manager-sjabloon.

- U kunt de afhankelijkheden tussen resources zodat deze zijn geïmplementeerd in de juiste volgorde definiëren.

![Resource Manager](./media/governance-in-azure/security-governance-in-azure-fig4.png)

Ga voor aanbevelingen over sjablonen naar [Aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

Azure Resource Manager analyseert afhankelijkheden om ervoor te zorgen dat de resources in de juiste volgorde worden gemaakt. Als één resource is afhankelijk van een waarde uit een andere bron (zoals een virtuele machine die behoefte hebben aan een opslagaccount voor schijven), u [instellen van een afhankelijkheid](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies) in de sjabloon.

U kunt de sjabloon ook gebruiken voor updates aan de infrastructuur. U kunt bijvoorbeeld een resource toevoegen aan uw oplossing en configuratieregels toevoegen voor de resources die al zijn geïmplementeerd. Als de sjabloon is opgegeven voor een resource gemaakt maar betreffende resource al bestaat, wordt in het Resource Manager een update in plaats van een nieuwe asset maken uitvoert. Resource Manager werkt de bestaande asset naar dezelfde toestand als het normaal zou als een nieuwe zijn.

Resource Manager biedt uitbreidingen voor scenario's als u meer bewerkingen, zoals het installeren van software die niet is opgenomen in de installatie nodig.

### <a name="resource-tracking"></a>Bronnen bijhouden

Als gebruikers in uw organisatie een resource aan het abonnement toevoegt, wordt het belangrijker resources koppelen aan de juiste afdeling, de klant en de omgeving. U kunt metagegevens koppelen aan resources via [labels](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags). U codes gebruiken om informatie over de bron of de eigenaar te geven. Labels kunnen u niet alleen aggregeren en resources op verschillende manieren groeperen, maar ook gebruiken die gegevens voor terugstorting.

Gebruik tags wanneer u een verzameling complexe resourcegroepen en resources en u hebt moeten deze assets op de manier die het meest zinvol om u te visualiseren. U kunt bijvoorbeeld resources die een vergelijkbare rol vervullen in uw organisatie of die deel uitmaken van dezelfde afdeling labelen.

Zonder tags kunnen kunnen gebruikers in uw organisatie maken van meerdere resources die mogelijk moeilijk te later identificeren en te beheren. U wilt verwijderen van alle resources voor een project. Als u deze resources zijn niet gemarkeerd voor het project, moet u deze handmatig zoeken. Taggen kan een belangrijke manier zijn om onnodige kosten in uw abonnement te voorkomen.

Resources hoeven zich bevinden in dezelfde resourcegroep voor het delen van een label. U kunt uw eigen tagtaxonomie om ervoor te zorgen dat alle gebruikers in uw organisatie gebruiken veelgebruikte tags in plaats van per ongeluk toepassen afwijkende tags (zoals 'Afd' in plaats van 'afdeling') maken.

Bronbeleid kunnen u voor het maken van standaardregels voor uw organisatie. U kunt beleid om ervoor te zorgen dat bronnen worden gelabeld met de juiste waarden maken.

U kunt ook getagde resources via Azure Portal weergeven. De [gebruiksrapport](https://docs.microsoft.com/azure/billing/billing-understand-your-bill) voor uw abonnement tagnamen en waarden bevat, dus u kunt uitsplitsing kosten door tags.

Zie voor meer informatie over tags [facturering tags beleid initiatief](../azure-policy/scripts/billing-tags-policy-init.md).

Voor tags gelden de volgende beperkingen:

- Elke resource of resourcegroep kan maximaal 15 tag sleutel/waarde-paren hebben. Deze beperking geldt alleen voor tags die direct worden toegepast op de resourcegroep of resource. Een resourcegroep mag veel bronnen die elk 15 tag sleutel/waarde-paren hebben.

- De tagnaam is beperkt tot 512 tekens.

- De tagwaarde is beperkt tot 256 tekens.

- Tags die zijn toegepast op de resourcegroep, worden niet overgenomen door de resources in deze resourcegroep.

Als u meer dan 15 waarden aan een resource moet koppelen, gebruikt u een JSON-tekenreeks voor de tagwaarde. De JSON-tekenreeks kan veel waarden die worden toegepast op een enkel label-sleutel bevatten.

#### <a name="tags-for-billing"></a>Codes voor facturering

Labels kunnen u uw factureringsgegevens groep. Bijvoorbeeld, als u meerdere virtuele machines voor verschillende organisaties uitvoert, gebruik labels aan het gebruik van de groep door kostenplaats. U kunt ook tags gebruiken om kosten te categoriseren door de runtimeomgeving, zoals het gebruik van facturering voor VM's worden uitgevoerd in de productieomgeving.

Vindt u informatie over tags via de [Azure brongebruik en RateCard APIs](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) of het gebruik door komma's gescheiden waarden (CSV)-bestand. Downloaden van het bestand informatie over het gebruik van de [Azure-accounts portal](https://account.windowsazure.com/) of de [EA portal](https://ea.azure.com/).

Zie voor meer informatie over de programmatische toegang tot factureringsgegevens [inzicht in uw Microsoft Azure-brongebruik](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview). Zie voor REST-API-bewerkingen, [Azure Billing REST API-verwijzing](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Wanneer u het gebruik van CSV voor services die ondersteuning bieden voor tags met facturering downloadt, wordt de labels worden weergegeven in de kolom labels.

### <a name="critical-resource-controls"></a>Besturingselementen voor kritieke bronnen

Als uw organisatie wordt basisservices toegevoegd aan het abonnement, wordt het belangrijker om ervoor te zorgen dat deze services beschikbaar om te voorkomen dat de zakelijke onderbroken zijn. [Resource vergrendelingen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) kunt u beperken van bewerkingen op waardevolle bronnen waar wijzigt of verwijdert deze zou aanzienlijke gevolgen voor uw toepassingen of cloudinfrastructuur hebben. U kunt vergrendelingen toepassen op een abonnement, resourcegroep of resource. U kunt gewoonlijk vergrendelingen toepassen op fundamentele resources, zoals virtuele netwerken, gateways en storage-accounts.

Resource vergrendelingen momenteel ondersteuning voor twee waarden: **CanNotDelete** en **ReadOnly**. **CanNotDelete** betekent dat gebruikers (met de juiste rechten) kunnen wel lezen of wijzigen van een bron, maar kan niet worden verwijderd. **Alleen-lezen** betekent dat gebruikers gemachtigde niet verwijderen of wijzigen van een resource.

Resource vergrendelingen gelden alleen voor bewerkingen die in de vlak management, die uit de bewerkingen die worden verzonden optreden bestaat naar <https://management.azure.com>. De vergrendelingen beperken niet hoe resources hun eigen functies uitvoeren. Wijzigingen in de resourcedefinitie zijn beperkt, maar de bewerkingen van resources zijn niet beperkt. Bijvoorbeeld, een **ReadOnly** vergrendeling op een SQL-database wordt voorkomen dat u de database wijzigen of verwijderen, maar voorkomt niet dat u maken, bijwerken of verwijderen van gegevens in de database.

Toepassen van **ReadOnly** kan leiden tot onverwachte resultaten optreden omdat bepaalde bewerkingen die lijkt lezen bewerkingen extra acties vereist. Bijvoorbeeld, als een **ReadOnly** vergrendeling van een opslagaccount wordt voorkomen dat alle gebruikers weergeven van de sleutels. De bewerking van de aanbieding van sleutels wordt verwerkt door een POST-aanvraag omdat de geretourneerde sleutels beschikbaar voor schrijfbewerkingen zijn.

![Besturingselementen voor kritieke bronnen](./media/governance-in-azure/security-governance-in-azure-fig5.png)

Voor een ander voorbeeld plaatsen van een **ReadOnly** vergrendelen op een Azure App Service-bron wordt voorkomen dat Visual Studio Server Explorer-bestanden voor de resource worden weergegeven omdat die interactie voor toegang voor schrijven vereist.

In tegenstelling tot rollen gebaseerd toegangsbeheer kunt u management vergrendelingen toepassen van een beperking voor alle gebruikers en rollen. Zie voor meer informatie over het instellen van machtigingen [beheren van toegang met RBAC en de Azure-portal](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).

Wanneer u een vergrendeling op een bovenliggend bereik toepast, nemen alle resources binnen dat bereik de dezelfde vergrendeling. Zelfs bronnen die u later toevoegen overnemen de vergrendeling van het bovenliggende item. De meest beperkende vergrendeling in de overname voorrang.

Als u wilt maken of verwijderen van management vergrendelingen, moet u toegang hebben tot Microsoft.Authorization/ of Microsoft.Authorization/locks/ acties. Van de ingebouwde rollen worden alleen de eigenaar en de beheerder voor gebruikerstoegang die acties verleend.

### <a name="api-access-to-billing-information"></a>API-toegang tot factureringsgegevens

Azure Billing-API's gebruikt om pull gebruiks- en gegevens bij uw voorkeur hulpprogramma's voor gegevensanalyse. Het gebruik van Azure-bronnen en RateCard APIs kunt u nauwkeurige voorspellen en beheren van uw kosten. De API's worden geïmplementeerd als een resourceprovider en het onderdeel van de API's beschikbaar gesteld door Azure Resource Manager-familie.

#### <a name="resource-usage-api-preview"></a>Resourcegebruik API (Preview)

Gebruik de Azure [Resource gebruik API](https://msdn.microsoft.com/library/azure/mt219003) om uw gegevens geschatte Azure-verbruik. De API bevat:

- **RBAC**: Configureer toegangsbeleid op de [Azure-portal](https://portal.azure.com/) of via [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot gegevens over het gebruik van het abonnement krijgen kunnen. Aanroepfuncties moeten standaard tokens van Azure Active Directory voor verificatie gebruiken. De aanroeper toevoegen aan de lezer facturering, een lezer, een eigenaar of een bijdrager rol voor toegang tot de gebruiksgegevens voor een specifieke Azure-abonnement.

- **Uurlijkse of dagelijkse aggregatie**: aanroepfuncties kunnen aangeven of ze hun Azure gebruiksgegevens in Uurlijkse of dagelijkse stappen. De standaardwaarde is dagelijks.

- **Metagegevens van het exemplaar (inclusief resourcetags)**: ophalen van instantieniveau details, zoals de volledig gekwalificeerde resource-URI (/subscriptions/ {abonnement-id} /..), groep resourcegegevens en resourcetags. Deze metagegevens kunt u deterministisch en programmatisch gebruik toewijzen door de tags, voor gebruiksvoorbeelden zoals cross in rekening gebracht.

- **Bron-metagegevens**: Resourcedetails zoals de naam van de meter, meter categorie meter subcategorie, eenheid en regio geven de aanroeper een beter inzicht in wat is verbruikt. We ook proberen om te worden uitgelijnd resource metagegevens terminologie via de Azure-portal, Azure gebruik CSV, EA facturering CSV en andere ervaringen openbare voor hulp bij het correleren van gegevens over ervaringen.

- **Gebruik voor alle typen bieden**: gebruiksgegevens beschikbaar is voor alle typen, inclusief betalen naar gebruik, MSDN, bedrag, financieel tegoed en EA bieden.

#### <a name="resource-ratecard-api"></a>Resource RateCard API

De Azure Resource RateCard API gebruiken om de lijst met beschikbare Azure-resources en informatie over de geschatte prijzen voor elk. De API bevat:

- **RBAC**: uw beleidsregels configureren op de Azure-portal of via Azure PowerShell-cmdlets om op te geven welke gebruikers of toepassingen toegang tot de gegevens RateCard krijgen kunnen. Aanroepfuncties moeten standaard tokens van Azure Active Directory voor verificatie gebruiken. De aanroeper toevoegen aan de rol Lezer, de eigenaar of bijdrager ze toegang krijgen tot de gebruiksgegevens voor een bepaald Azure-abonnement.

- **Ondersteuning voor betalen per gebruik, MSDN, bedrag en financieel tegoed aanbiedingen (maar niet EA)**: voor deze API biedt Azure-aanbieding niveau snelheid informatie. De aanroeper van deze API moet in de aanbieding informatie ophalen van de resourcedetails van de en tarieven doorgeven. EA is momenteel niet ondersteund omdat EA aanbiedingen tarieven per inschrijving hebt aangepast. 

#### <a name="scenarios"></a>Scenario's

De combinatie van de informatie over het gebruik en RateCard APIs maakt deze scenario's mogelijk:

- **Inzicht in Azure tijdens de maand te besteden aan**: gebruiken om de combinatie van het gebruik en RateCard APIs ophalen beter inzicht in uw cloud-uitgaven in de maand. U kunt elk uur en dagelijks gebruik en de kosten maakt een schatting analyseren.

- **Stel waarschuwingen**: Gebruik de informatie over het gebruik en RateCard APIs geschatte cloud verbruik en de kosten en resource of monetaire gebaseerde waarschuwingen instellen.

- **Factuur voorspellen**: Get uw geschatte gebruiks- en cloud besteden en machine learning-algoritmen om te voorspellen wat de factuur aan het einde van de factureringscyclus zou zijn van toepassing.

- **Uitvoeren van een vooraf verbruik kosten analysis-**: de RateCard-API gebruiken om te voorspellen hoeveel uw factuur voor niet het verwachte gebruik wanneer u uw workloads naar Azure verplaatst. Als u bestaande workloads in andere clouds of privéclouds hebt, kunt u ook uw gebruik met de Azure toewijzen te besteden aan de tarieven voor een betere schatting van Azure ophalen. Deze schatting biedt u de mogelijkheid om te draaien op aanbieding en tussen de verschillende aanbiedingstypen dan betalen naar gebruik, zoals bedrag en financieel tegoed vergelijken.

- **Een wat-als-analyse uitvoeren**: U kunt bepalen of het meest efficiënt werkbelastingen worden uitgevoerd in een andere regio of op een andere configuratie van de Azure-resource. Azure-resourcekosten mogelijk verschillen op basis van de Azure-regio u. U kunt ook bepalen als een ander Azure-aanbiedingtype resulteert in een betere rentabiliteit op een Azure-resource.

### <a name="networking-controls"></a>Besturingselementen voor netwerken

Toegang tot bronnen kan worden (binnen het bedrijfsnetwerk) interne of externe (via het internet). Het is gemakkelijk voor gebruikers in uw organisatie per ongeluk resources in de verkeerde positie plaatsen en blootstellen aan schadelijke toegang. Net als bij de on-premises apparaten, moeten de ondernemingen juiste besturingselementen voor het Azure-gebruikers moet ervoor zorgen dat de juiste keuzes toevoegen.

![Besturingselementen voor netwerken](./media/governance-in-azure/security-governance-in-azure-fig6.png)

Voor abonnement toezicht bieden de volgende bronnen voor core basisbesturingselement van toegang.

#### <a name="network-connectivity"></a>Netwerkverbinding

[Virtuele netwerken](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) containerobjecten voor subnetten zijn. Hoewel niet strikt noodzakelijk is, wordt een virtueel netwerk wordt vaak gebruikt voor het verbinden van toepassingen tot interne bedrijfsbronnen. De service Azure Virtual Network kunt u veilig Azure-resources met elkaar te verbinden met virtuele netwerken.

Een virtueel netwerk is een weergave van uw eigen netwerk in de cloud. Een virtueel netwerk is een logische isolatie van de Azure-cloud toegewezen aan uw abonnement. Ook kunt u virtuele netwerken koppelen aan uw on-premises netwerk.

Hieronder vindt u mogelijkheden voor virtuele netwerken van Azure:

- **Isolatie**: virtuele netwerken zijn geïsoleerd van elkaar. U kunt afzonderlijke virtuele netwerken voor ontwikkeling, testen en productie die gebruikmaken van dezelfde CIDR-adresblokken maken. Als u daarentegen, kunt u meerdere virtuele netwerken die gebruikmaken van verschillende CIDR-adresblokken en netwerken met elkaar verbinden. U kunt een virtueel netwerk segmenteren in meerdere subnetten. Azure biedt interne naamomzetting voor VM's en Azure Cloud Services-rolexemplaren die zijn verbonden met een virtueel netwerk. U kunt desgewenst een virtueel netwerk voor het gebruik van uw eigen DNS-servers, in plaats van Azure interne naamomzetting configureren.

- **Verbinding met Internet**: alle Azure virtuele machines en Cloud Services-rolexemplaren die zijn verbonden met een virtueel netwerk hebben toegang tot het internet standaard. U kunt binnenkomende toegang tot specifieke bronnen, indien nodig.

- **Azure-resource connectiviteit**: U kunt Azure-resources zoals Cloudservices en virtuele machines, verbinding maken met hetzelfde virtuele netwerk. De resources kunnen met elkaar verbinden via privé IP-adressen, zelfs als ze zich in verschillende subnetten. Azure biedt standaardroutering tussen subnetten, virtuele netwerken en on-premises netwerken, zodat u niet hoeft te configureren en beheren van routes.

- **Verbinding met het virtuele netwerk**: U kunt virtuele netwerken met elkaar verbinden. Resources die zijn verbonden met een virtueel netwerk kunnen vervolgens communiceren met een resource in een ander virtueel netwerk.

- **Lokale connectiviteit**: U kunt virtuele netwerken kunt verbinden met on-premises netwerken via persoonlijke netwerkverbindingen tussen uw netwerk en Azure of een site-naar-site virtueel particulier netwerk (VPN)-verbinding via internet.

- **Filteren van verkeer**: U kunt netwerkverkeer (binnenkomend en uitgaand) voor virtuele machines en Cloud-Services door de bron-IP-adres en poort, doel-IP-adres en poort en protocol filteren.

- **Routering**: U kunt eventueel standaard Azure routering door uw eigen routes te configureren of met behulp van BGP-routes via een netwerkgateway overschrijven.

#### <a name="network-access-controls"></a>Netwerk-toegangsbeheer

[Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (nsg's) zijn, zoals een firewall en regels voor hoe een resource "" via het netwerk praten kan opgeeft. Deze bieden controle over hoe een subnet (of de virtuele machine) verbinding met internet of andere subnetten in hetzelfde virtuele netwerk maken kan.

Een netwerkbeveiligingsgroep bevat een lijst met regels voor toestaan of weigeren van netwerkverkeer naar bronnen die zijn verbonden met virtuele netwerken in Azure. Nsg's kunnen worden gekoppeld aan subnetten, afzonderlijke virtuele machines (klassiek) of afzonderlijke netwerkinterfaces (NIC's) gekoppeld aan virtuele machines (Resource Manager).

Wanneer een NSG gekoppeld aan een subnet is, zijn de regels van toepassing op alle resources die zijn verbonden met het subnet. U kunt verkeer verder beperken door een NSG koppelen aan een VM of NIC wordt gebruikt.

## <a name="security-and-compliance-with-organizational-standards"></a>Beveiliging en organisatie-normen

Elk bedrijf heeft verschillende behoeften en verschillende voordelen van cloudoplossingen wordt benutten. Toch klanten van alle soorten het dezelfde basis twijfels hebt over de verplaatsing naar de cloud. Klanten willen van cloudproviders is:

- **Beveiligen van onze gegevens**: IT-managers bevestigen of de cloud verbeterde beveiliging en beheer bieden kan. Maar ze zijn nog steeds betrokken dat migreren naar de cloud ze gevoeliger voor hackers dan hun huidige interne oplossingen laat.

- **Onze gegevens privé houden**: cloudservices unieke privacy uitdagingen verhogen. Bedrijven zoeken naar de cloud op te slaan op de kosten van infrastructuur hun flexibiliteit verbeteren, bang ze ook te verliezen van de controle van waar hun gegevens worden opgeslagen, wie het en hoe deze wordt gebruikt.

- **Geef ons besturingselement**: zelfs als bedrijven van de cloud meer innovatieve oplossingen te implementeren profiteren, zij zich zorgen maakt over controle over hun gegevens verliezen. De recente vermeldingen van overheidsinstanties toegang tot gegevens van de klant, juridische en buitengerechtelijke wijze, zorg sommige CIO hoede hun gegevens opslaan in de cloud.

- **Bevorderen de transparantie**: zakelijke besluitvormers begrijpen het belang van beveiliging, privacy en controle. Maar ze ook de mogelijkheid wilt onafhankelijk controleren hoe de gegevens worden opgeslagen, geopend en beveiligd.

- **Behouden van compatibiliteit**: bij bedrijven uitbreiden voor het gebruik van cloudtechnologieën, de complexiteit en het bereik van standaarden en -voorschriften gaan ontwikkelen. Bedrijven moeten weten dat hun compatibiliteit normen wordt voldaan.

## <a name="security-configuration-for-monitoring-logging-and-auditing"></a>Beveiligingsconfiguratie voor bewaking, logboekregistratie en controle

Azure-abonnees kunnen hun cloudomgevingen beheren vanaf meerdere apparaten. Deze apparaten mogelijk omvatten beheerwerkstations, developer-pc's en apparaten van zelfs bevoegde eindgebruikers met taakspecifieke rechten. 

In sommige gevallen worden beheerfuncties uitgevoerd via het web gebaseerde consoles, zoals de Azure-portal. In andere gevallen is er mogelijk rechtstreekse verbindingen naar Azure vanaf on-premises systemen via VPN-verbindingen, Terminal Services, protocollen van clienttoepassingen of (programmatisch) Azure Service Management API (SMAPI). Clienteindpunten kunnen bovendien zowel in een domein die lid zijn van zichzelf staand en niet-beheerd, zoals tablets of smartphones zijn.

Deze verscheidenheid kan aanzienlijke risico's toevoegen aan een cloudimplementatie. Het kan zijn moeilijk te beheren, volgen en te controleren. Deze verscheidenheid kan ook leiden tot beveiligingsrisico's via ongereglementeerde toegang plaatsvindt tot clienteindpunten die worden gebruikt voor het beheer van cloudservices. Het gebruik van algemene of persoonlijke werkstations voor het ontwikkelen en beheren van infrastructuur brengt onvoorspelbare bedreigingen met zich mee voor bijvoorbeeld surfen op internet (denk aan waterhole-aanvallen) of e-mail (zoals social engineering en phishing).

Bewaking, logboekregistratie en controle bieden een basis voor het bijhouden en begrijpen van beheeractiviteiten. Controle van alle acties nauwgezet altijd mogelijk niet mogelijk vanwege de hoeveelheid gegevens die zijn gegenereerd. Maar controle van de effectiviteit van de management-beleidsregels wordt aanbevolen.

Azure-beveiliging governance van groepsbeleidsobjecten met Azure Active Directory Domain Services (AD DS) kunt u alle de beheerders Windows-interfaces, zoals het delen van bestanden bepalen. Neem beheerwerkstations in bewaking, logboekregistratie en controle van processen. Houd toegang en gebruik van alle beheerders en ontwikkelaars bij.

### <a name="azure-security-center"></a>Azure Security Center

[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) biedt een centrale weergave van de beveiligingsstatus van resources in abonnementen. Deze aanbevelingen die helpen te voorkomen dat resources waarmee is geknoeid. Het kunt meer gedetailleerde beleidsregels--bijvoorbeeld beleidsregels toepassen op specifieke resourcegroepen waarmee bedrijven hun houding het risico dat ze je adressering aanpassen inschakelen.

![Azure Security Center](./media/governance-in-azure/security-governance-in-azure-fig7.png)

Security Center biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen die anders onopgemerkt, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen te detecteren. Nadat u hebt ingeschakeld [beveiligingsbeleid](https://docs.microsoft.com/azure/security-center/security-center-policies) voor resources van een abonnement, analyseert Security Center de beveiliging van uw resources voor het identificeren van mogelijke beveiligingsproblemen. Informatie over uw netwerkconfiguratie is onmiddellijk beschikbaar.

Azure Security Center vertegenwoordigt een combinatie van best practice analyse- en beleidsbeheer voor alle resources binnen een Azure-abonnement. Hiermee kunt beveiliging teams en risico managers te detecteren, voorkomen van en reageren op bedreigingen, zoals deze worden automatisch verzameld en beveiligingsgegevens van uw Azure-resources, het netwerk en partneroplossingen zoals antimalwareprogramma's en firewalls analyseert.

Bovendien Azure Security Center geavanceerde analyses toegepast, met inbegrip van machine learning en gedragsanalyse. Het wereldwijde dreigingen van Microsoft-producten en services, de Microsoft Digital Crimes Unit (DCU), Microsoft Security Response Center (MSRC) gebruikt en externe feeds. U kunt toepassen [beveiliging governance](https://www.credera.com/blog/credera-site/azure-governance-part-4-other-tools-in-the-toolbox/) grotendeels op het abonnementsniveau. Of u kunt deze beperken tot specifieke vereisten en toegepast op afzonderlijke resources via beleidsdefinitie.

Ten slotte Azure Security Center analyseert de beveiligingsstatus van de resource op basis van deze beleidsregels en gebruikt deze informatie om begrijpelijke manier mee dashboards en waarschuwingen voor gebeurtenissen zoals detectie van malware of schadelijke IP-verbinding probeert te bieden. Zie voor meer informatie over het toepassen van aanbevelingen [beveiligingsaanbevelingen implementeren in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Azure Security Center bewaakt de volgende Azure-resources:

- Virtuele machines (VM's) (inclusief cloudservices)

- Virtuele netwerken

- SQL-databases

- Partneroplossingen die zijn geïntegreerd met uw Azure-abonnement, zoals web application firewall op virtuele machines en klik op de [App Service-omgeving](https://docs.microsoft.com/azure/app-service/app-service-app-service-environments-readme)

Wanneer u Security Center voor het eerst opent, worden gegevensverzameling is ingeschakeld op alle virtuele machines in uw abonnement. Het is raadzaam dat u gegevensverzameling is ingeschakeld, maar u kunt [uitschakelen](https://docs.microsoft.com/azure/security-center/security-center-faq) in het beleid Security Center.

### <a name="log-analytics"></a>Log Analytics

Azure-logboekanalyse software ontwikkelings- en service van het team informatiebeveiliging en [governance programma](https://github.com/Microsoft/azure-docs/blob/master/articles/log-analytics/log-analytics-security.md) ondersteunt de zakelijke vereisten. Het document voldoet aan regelgeving zoals beschreven op [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) en [Microsoft Trust Center naleving](https://www.microsoft.com/TrustCenter/Compliance/default.aspx). Hoe logboekanalyse beveiligingsvereisten, identificeert beveiligingsmechanismen en beheert en monitors risico's wordt er ook beschreven. Het team beoordeelt jaarlijks, beleidsregels, standaarden, procedures en richtlijnen.

Elk teamlid Log Analytics-ontwikkeling ontvangt beveiligingstraining formele toepassing. Een versiebeheersysteem beschermt u elk software-project in ontwikkeling.

Microsoft heeft een beveiliging en naleving team dat verantwoordelijk en alle services in Microsoft evalueert. De vaststelling van informatie van het team maken en ze zijn niet gekoppeld aan de technische afdelingen die Log Analytics te ontwikkelen. De beveiliging verantwoordelijken hebben hun eigen Managementketen. Het uitvoeren van onafhankelijke beoordelingen van producten en services om te zorgen voor beveiliging en naleving.

De kernfunctionaliteit van logboekanalyse wordt verstrekt door een set services die worden uitgevoerd in Azure. Elke service biedt een specifieke beheerfunctie. U kunt services combineren om verschillende beheerscenario's te bewerkstelligen.

![Azure-services voor beheer](./media/governance-in-azure/security-governance-in-azure-fig9.JPG)

Deze management-services zijn ontworpen in de cloud. Ze zijn volledig gehost in Azure, zodat ze niet implementeren en beheren van lokale bronnen veroorzaken. Configuratie is minimaal en u kunt actief en werkend zijn binnen een paar minuten.

Een agent op een Windows- of Linux-computer in uw datacenter geplaatst en gegevens met logboekanalyse wordt verzonden. Deze kan er, samen met alle andere gegevens verzameld van de cloud of on-premises services worden geanalyseerd. Azure Backup en Azure Site Recovery gebruiken om te profiteren van de cloud voor back-up en hoge beschikbaarheid voor lokale bronnen.

![Management-services op de Azure-dashboard](./media/governance-in-azure/security-governance-in-azure-fig8.png)

Runbooks in de cloud heeft geen gewoonlijk toegang tot uw lokale bronnen, maar u kunt een agent installeren op een of meer computers die als host voor runbooks in uw datacenter fungeert. Wanneer u een runbook start, kunt u opgeven of u wilt uitvoeren in de cloud of op een lokale worker.

Er zijn verschillende oplossingen beschikbaar van Microsoft en partners die u aan uw Azure-abonnement toevoegen kunt te verhogen van de waarde van uw investering in logboekanalyse. Bijvoorbeeld: Azure biedt [beheeroplossingen](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions)--voorverpakt sets van logica die een beheerscenario implementeren met behulp van een of meer management-services.

![Galerie van oplossingen in Azure](./media/governance-in-azure/security-governance-in-azure-fig10.png)

Als partner kunt u uw eigen oplossingen voor de ondersteuning van uw toepassingen en services en ze te bieden aan gebruikers via de Azure Marketplace of Quick Start-sjablonen maken.

## <a name="performance-alerting-and-monitoring"></a>Prestatiewaarschuwingen en controle

### <a name="alerting"></a>Waarschuwingen

Waarschuwingen zijn een methode voor bewaking metrische gegevens voor Azure-resource, gebeurtenissen of Logboeken. Ze een melding zodra wordt voldaan aan een voorwaarde die u hebt opgegeven.

Waarschuwingen zijn beschikbaar via de services, waaronder:

- **Azure Application Insights**: kan web-test en meetwaarde op waarschuwingen. Zie voor meer informatie [meldingen instellen in Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) en [beschikbaarheid en reactiesnelheid van een website bewaken](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability).

- **Meld u Analytics**: de routering van de activiteit en de logboeken met diagnostische gegevens met logboekanalyse is ingeschakeld. Kunt u metrische gegevens, het logboek en overige Waarschuwingstypen. Zie voor meer informatie [waarschuwingen in logboekanalyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts).

- **Monitor voor Azure**: Hiermee kunt u waarschuwingen op basis van zowel de metrische waarden als de activiteit logboekgebeurtenissen. U kunt de [REST-API van Azure-Monitor](https://msdn.microsoft.com/library/dn931943.aspx) om waarschuwingen te beheren. Zie voor meer informatie [met behulp van de Azure-portal, PowerShell of de opdrachtregelinterface voor het maken van waarschuwingen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal).

### <a name="monitoring"></a>Bewaking

Prestatieproblemen in uw cloud-app kunnen invloed hebben op uw bedrijf. Degradations kan met meerdere onderdelen onderling verbonden en releases vaak gebeuren op elk gewenst moment. En als u een app ontwikkelt, uw gebruikers problemen die u hebt niet vinden in het testen van meestal te detecteren. U moet weten over deze problemen direct en hulpprogramma's voor het opsporen en corrigeren hebben.

Er is een reeks hulpmiddelen voor het bewaken van Azure-toepassingen en services. Enkele van hun functies overlappen. Dit is gedeeltelijk vanwege de vervaging tussen ontwikkeling en de werking van een toepassing.

Hier volgen de principal-hulpprogramma's:

- **Monitor voor Azure** is een eenvoudige hulpprogramma voor het bewaken van services die worden uitgevoerd op Azure. Dit biedt u niveau van de infrastructuur gegevens over de doorvoer van een service en de omgeving. Als u uw apps in Azure en bepalen of schaal omhoog of omlaag resources beheert, kunt Azure Monitor u beginnen.

- **Application Insights** kunnen worden gebruikt voor ontwikkeling en als een productie-bewakingsoplossing. Dit proces werkt door het installeren van een pakket in uw app zodat dit u een meer interne weergave biedt van wat er gebeurt. De gegevens omvatten reactietijden van afhankelijkheden, uitzondering traceringen, momentopnamen en uitvoering profielen foutopsporing. Het biedt hulpprogramma's voor het analyseren van alle deze telemetrie kunt u fouten opsporen in een app, en ook om te begrijpen wat gebruikers doen met het. U kunt zien of een piek in de reactietijden veroorzaakt door iets in een app of een externe resourcing probleem wordt. Als u Visual Studio en de app is beschadigd, u direct naar de coderegel probleem gaan zodat u deze kunt oplossen.

- **Meld u Analytics** is voor gebruikers die moeten afstemmen van de prestaties en onderhoud plannen van toepassingen die worden uitgevoerd in de productieomgeving. Deze verzamelt en gegevens uit diverse bronnen, met een vertraging van 10 tot 15 minuten worden verzameld. Dit biedt een holistische oplossing voor IT-beheer voor Azure, on-premises en cloud-gebaseerde infrastructuur hebben van derden (zoals Amazon Web Services). Het biedt hulpprogramma's om gegevens te analyseren alle bronnen, kunt u complexe query's via alle logboeken en proactief kan waarschuwing bij de opgegeven voorwaarden. U kunt zelfs aangepaste gegevens verzamelen in de centrale opslagplaats, en vervolgens een query en die gegevens te visualiseren.

- **System Center Operations Manager** is voor het beheren en controleren van grote cloud-installaties. Bent u mogelijk al bekend bent met het als beheerprogramma voor lokale Windows Server en Hyper-V op basis van clouds, maar het kan ook integreren met en beheren van apps van Azure. Onder andere kan Application Insights worden geïnstalleerd op bestaande live apps. Als een app uitvalt, Operations Manager aangegeven (in seconden).


## <a name="next-steps"></a>Volgende stappen

- [Best practices voor het maken van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices)

- [Voorbeelden van de implementatie van Azure-abonnement governance](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-examples)

- [Microsoft Azure Government](https://docs.microsoft.com/azure/azure-government/)
