---
title: Beheeracties in Azure | Microsoft Docs
description: Meer informatie over cloud-gebaseerde computers onder meer services als een groot aantal compute-exemplaren en services die kunnen worden geschaald omhoog en omlaag automatisch om te voldoen aan de behoeften van uw toepassing of enterprise.
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: TomSh
ms.openlocfilehash: 875b78f6af330aea3333b045ee72e9632e9aa8a6
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="governance-in-azure"></a>Beheeracties in Azure

We weten dat beveiliging taak in de cloud en hoe belangrijk is dat u tijdig en informatie over Azure-beveiliging vinden. Een van de beste redenen voor het gebruik van Azure voor uw toepassingen en services is om te profiteren van de breed scala aan mogelijkheden en hulpprogramma's voor beveiliging. Deze hulpprogramma's en mogelijkheden te maken het mogelijk om veilige oplossingen maken op de beveiligde Azure-platform.

Om beter inzicht in de matrix van toezicht besturingselementen die zijn geïmplementeerd in Microsoft Azure vanuit zowel van de klant en de Microsoft operations perspectieven, in dit artikel 'Governance in Azure', wordt geschreven naar bieden een uitgebreide blik op de Governance functies die beschikbaar zijn met Microsoft Azure.

## <a name="azure-platform"></a>Azure-platform

Azure is een openbare cloud service-platform die ondersteuning biedt voor een brede selectie van besturingssystemen, programmeertalen, frameworks, hulpprogramma's, databases en apparaten. Linux-containers kan worden uitgevoerd door de integratie van Dockers; bouwen van apps met JavaScript, Python, .NET, PHP, Java en Node.js; Build back-ends voor iOS, Android en Windows-apparaten. Openbare Azure-cloud-services ondersteunen dezelfde technologieën miljoenen ontwikkelaars en IT-professionals die al zijn afhankelijk van en vertrouwt.

Wanneer u bouwen op of IT-activa te migreren, een openbare cloud-serviceprovider u vertrouwen op de mogelijkheden voor uw toepassingen en gegevens beschermen met de services en de besturingselementen die organisatie ze bieden voor het beheren van de beveiliging van uw cloud-gebaseerde activa.

Azure infrastructuur van de faciliteit is ontworpen voor het hosten van miljoenen klanten tegelijkertijd van toepassingen en biedt een betrouwbare basis waarop bedrijven kunnen voldoen aan de beveiligingsvereisten. Bovendien biedt Azure veel mogelijkheden voor beveiliging en de mogelijkheid om ze te beheren zodat u kunt de beveiliging om te voldoen aan de unieke vereisten van uw organisatie implementaties aanpassen.

Dit document helpt u begrijpen hoe Azure Governance mogelijkheden kunnen u helpen te voldoen aan deze vereisten.

## <a name="abstract"></a>Abstracte

Microsoft Azure cloud governance biedt een geïntegreerde controle en advies benadering voor het controleren en adviseren organisaties op het gebruik van de Azure-platform. Microsoft Azure cloud governance verwijst naar de besluitvormingsprocessen automatiseert, criteria en beleidsregels die zijn betrokken bij de planning, architectuur, overname, implementatie, bewerking en beheer van een Cloud computing.

Om een plan voor Microsoft Azure cloud toezicht maakt, moet u een diepgaande blik op de mensen, processen en technologieën momenteel duren in plaats en vervolgens samen frameworks die eenvoudig IT ter ondersteuning van consistent bedrijfsbehoeften en tegelijkertijd eindgebruikers de flexibiliteit om het gebruik van de andere van krachtige functies van Microsoft Azure.

Dit artikel wordt beschreven hoe u een verhoogd niveau van beheer van uw IT-bronnen in Microsoft Azure kunt bereiken. Dit artikel vindt u informatie over de beveiliging en beheeracties functies die zijn ingebouwd in Microsoft Azure.

Hier volgen de governance problemen in dit artikel beschreven main:

- Implementatie van beleid, processen en procedures conform de instelling voor organisatiedoelstellingen.

- Beveiliging en continue naleving met de standaarden van organisatie

- Bewaking en waarschuwingen

## <a name="implementation-of-policies-processes-and-procedures"></a>Implementatie van beleid, processen en procedures 

Management heeft tot stand gebracht, rollen en verantwoordelijkheden over de uitvoering van het beveiligingsbeleid van informatie en operationele continuïteit waakt in Azure. Beheer van Microsoft Azure is verantwoordelijk voor het beheren van beveiliging en procedures voor bedrijfscontinuïteit binnen hun respectieve teams (inclusief derden) en om naleving van beveiligingsbeleid, processen en standaarden.

Hier volgen de factoren die is ontwikkeld:

- Account inrichten

- Abonnement-besturingselementen

- Rollen gebaseerd toegangsbeheer

- Resourcebeheer

- Bronnen bijhouden

- Kritieke bronnen

- API-toegang tot factureringsgegevens

- Besturingselementen voor netwerken

## <a name="account-provisioning"></a>Account inrichten

Account hiërarchie definiëren, is een belangrijke stap om te gebruiken en de structuur van Azure services binnen een bedrijf en is de bestuursstructuur core. In geval van klanten met de enterprise agreement, kunnen klanten verder onderverdelen in de omgeving aan de afdelingen, accounts en ten slotte abonnementen.

![Account inrichten](./media/governance-in-azure/security-governance-in-azure-fig1.png)

Als u een enterprise agreement niet hebt, kunt u overwegen [Azure labels](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) op abonnementsniveau hiërarchie definiëren. Een Azure-abonnement is de basiseenheid waarbij alle resources zijn opgenomen. Het definieert ook diverse limieten in Azure, zoals het aantal kernen, bronnen, enzovoort. Abonnementen kunnen bevatten [resourcegroepen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), die bronnen kunnen bevatten. [RBAC](https://docs.microsoft.com/azure/api-management/api-management-role-based-access-control) beginselen van toepassing op deze drie niveaus.

Elke enterprise verschilt en de hiërarchie met gebruik van Azure labels in geval van een niet-zakelijke klanten kunt u aanzienlijke flexibiliteit voor hoe Azure is ingedeeld binnen het bedrijf. Voordat u resources in Microsoft Azure implementeert, moet u hiërarchie model en het effect van facturering, toegang tot bedrijfsbronnen en complexiteit.

## <a name="subscription-controls"></a>Abonnement-besturingselementen

Abonnement bepaalt hoe verbruik van resources is gerapporteerd en kosten in rekening gebracht. Abonnementen kunnen worden ingesteld voor afzonderlijke facturering en betaling. We kunnen meerdere abonnementen hebben als genoemde eerdere in een Azure-account. Abonnementen kunnen worden gebruikt om te bepalen van het Azure brongebruik van meerdere afdelingen in een bedrijf.

Bijvoorbeeld, als een bedrijf heeft IT, HR en Marketing afdelingen en deze afdelingen hebben verschillende projecten uitgevoerd. Op basis van het gebruik van Azure-resources zoals virtuele machines door elke afdeling, kunnen ze worden gefactureerd dienovereenkomstig. Dit kunnen we de financiën van elke afdeling bepalen.

Azure-abonnementen tot stand brengen van de drie parameters:

- een unieke abonnements-ID

- een locatie facturering

- Aantal beschikbare resources

Voor een afzonderlijke die geldt voor een Microsoft-account-ID, een creditcardnummer en de volledige suite met Azure-services--Hoewel Microsoft verbruik limieten, afhankelijk van het abonnementstype worden afgedwongen.

Azure inschrijving hiërarchieën definiëren hoe de structuur van services binnen een Enterprise Agreement. De Enterprise Portal kunnen klanten toegang tot Azure-resources die zijn gekoppeld aan een Enterprise Agreement op basis van flexibele hiërarchieën aanpasbare om unieke behoeften van uw organisatie te verdelen. Het patroon van de hiërarchie moet overeenkomen met het beheer en de geografische structuur van een organisatie, zodat de bijbehorende facturerings- en -toegang nauwkeurig kan worden gehouden voor.

De drie op hoog niveau zijn functionele, zakelijke eenheid en afdelingen geografische, met als een administratieve constructie voor account groeperingen. Binnen elke afdeling kunnen de accounts worden toegewezen aan abonnementen, die silo voor facturerings- en diverse belangrijke limieten maken in Azure (bijv, aantal virtuele machines, opslagaccounts, enz.).

![Abonnement-besturingselementen](./media/governance-in-azure/security-governance-in-azure-fig2.png)


Azure-abonnementen voor organisaties met een Enterprise Agreement, Ga als volgt een hiërarchie vier niveaus:

- de beheerder van de Enterprise-inschrijving

- afdeling beheerder

- de eigenaar van account

- Servicebeheerder

Deze hiërarchie bepaalt het volgende:

- Facturering relatie

- Beheer

- Rollen gebaseerd toegangsbeheer (RBAC) tot artefacten

- Grenzen/limieten

- Grenzen

  - Informatie over het gebruik en facturering (tariefkaart op basis van de aanbieding cijfers)

  - Limieten

  - Virtual Network

- Gekoppeld aan 1 AAD (1 AAD worden gekoppeld aan veel abonnementen)

- Dat is gekoppeld aan een account enterprise-inschrijving

## <a name="role-based-access-controls"></a>Toegangsbeheer op basis van rollen

Wanneer Azure in eerste instantie is uitgebracht, toegangsbeheer om een abonnement te basic zijn: Administrator of CO-beheerder. Toegang tot een abonnement in het klassieke model geïmpliceerde toegang tot alle bronnen in de portal. Dit gebrek aan nauwkeuriger beheer leidde tot de komst van abonnementen tot een redelijke toegangsbeheer bieden voor een Azure-inschrijving.

![Toegangsbeheer op basis van rollen](./media/governance-in-azure/security-governance-in-azure-fig3.png)

De verspreiding van abonnementen is niet meer nodig. Met op rollen gebaseerde toegangsbeheer, kunt u gebruikers toewijzen aan standaardrollen (zoals 'Lezer' en 'schrijver' voorkomende rollen). U kunt ook aangepaste rollen definiëren.

[Azure op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) kunt Geavanceerd toegangsbeheer voor Azure. Met RBAC kunt u alleen de toegangsrechten aan gebruikers verlenen die ze nodig hebben om hun taken uit te voeren. Beveiliging gerichte bedrijven moeten zich richten op uw werknemers de exacte machtigingen die ze nodig hebben. Te veel machtigingen tonen een account voor aanvallen. Te weinig machtigingen betekenen dat werknemers hun werk efficiënt kunnen niet ophalen. Azure op rollen gebaseerde toegangsbeheer (RBAC) kunt u dit probleem oplossen door het aanbieden van Geavanceerd toegangsbeheer voor Azure. RBAC helpt u bij het scheiden van taken binnen uw team en de hoeveelheid toegang verlenen aan gebruikers die ze nodig hebben voor het uitvoeren van hun taken. In plaats van iedereen geven onbeperkte machtigingen in uw Azure-abonnement of de bronnen, kunt u alleen bepaalde acties.

Bijvoorbeeld, gebruikmaken van RBAC te laten een werknemer virtuele machines in een abonnement beheren terwijl een andere SQL-databases binnen hetzelfde abonnement kunt beheren.

Azure RBAC heeft drie elementaire functies die van toepassing op alle brontypen:

- **Eigenaar** heeft volledige toegang tot alle bronnen, waaronder het recht op toegang aan anderen delegeren.

- **Inzender** kunt maken en beheren van alle soorten Azure-resources, maar kan geen toegang tot de overige verlenen.

- **Lezer** bestaande Azure-resources kunt weergeven.

De rest van de RBAC-rollen in Azure toestaan van beheer van specifieke Azure-resources. De rol Inzender van de virtuele Machine kan bijvoorbeeld de gebruiker te maken en beheren van virtuele machines. Het biedt ze geen toegang geven tot het virtuele netwerk of het subnet dat de virtuele machine verbinding met maakt.

[Ingebouwde RBAC-rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) vindt u de rollen die beschikbaar zijn in Azure. De Hiermee geeft u de bewerkingen en het bereik dat elke ingebouwde rol aan gebruikers verleent.

Toegang verlenen door de juiste RBAC-rol toewijzen aan gebruikers, groepen en toepassingen op een bepaalde scope. Het bereik van een roltoewijzing kan dit een abonnement, resourcegroep of één resource. Een rol die is toegewezen aan een bovenliggend bereik verleent toegang ook naar de onderliggende elementen erin opgenomen.

Een gebruiker met toegang tot een resourcegroep kan bijvoorbeeld alle resources, zoals websites, virtuele machines en subnetten bevat te beheren.

Azure RBAC ondersteunt alleen beheerbewerkingen van de Azure-resources in de Azure-portal en Azure Resource Manager-API's. Deze kan niet toestaan dat alle gegevens niveau bewerkingen voor Azure-resources. Bijvoorbeeld autoriseren iemand voor het beheren van Accounts voor opslag, maar niet aan de blobs of tabellen binnen een Opslagaccount niet. Op deze manier kan een SQL-database worden beheerd, maar niet de tabellen in het.

Zie [Wat is op rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) als u meer informatie wilt over het beheren van toegang met RBAC.

U kunt ook [maakt u een aangepaste rol](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) in gebaseerd toegangsbeheer (RBAC) als geen van de ingebouwde rollen aan uw specifieke toegang nodig heeft. Aangepaste rollen kunnen worden gemaakt met [Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell), [Azure-opdrachtregelinterface (CLI)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-azure-cli), en de [REST-API](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-rest). Net als de ingebouwde rollen kunnen aangepaste rollen worden toegewezen aan gebruikers, groepen en toepassingen bij het abonnement, resourcegroep en resource bereiken.

Binnen elk abonnement geldt een limiet van 2000 roltoewijzingen.

## <a name="resource-management"></a>Resourcebeheer

Door Azure geleverde oorspronkelijk alleen het klassieke implementatiemodel. In dit model bestonden elke resource afzonderlijk; Er is geen enkele manier verwante resources groeperen. In plaats daarvan moest u handmatig bijhouden welke resources bestaat uit uw oplossing of de toepassing en vergeet niet om deze te beheren in een gecoördineerde benadering.

Voor het implementeren van een oplossing, moest u elke resource afzonderlijk via de Azure portal maken of een script maken dat alle bronnen in de juiste volgorde geïmplementeerd. Als u wilt verwijderen van een oplossing, moest u elke resource afzonderlijk verwijderen. U kan niet eenvoudig toepassen en bijwerken van beleid voor toegangsbeheer voor verwante resources. Ten slotte kunt u niet toepassen tags aan resources label met voorwaarden die u helpen uw resources bewaken en beheren van facturering.

In 2014 geïntroduceerd Azure Resource Manager, dat het concept van een resourcegroep wordt toegevoegd. Een resourcegroep is een container voor resources die een gemeenschappelijk levenscyclus delen. Het implementatiemodel van Resource Manager biedt diverse voordelen:

- U kunt implementeren, beheren en bewaken van de services voor uw oplossing als een groep in plaats van afzonderlijk afhandeling van deze services.

- U kunt herhaaldelijk implementeren van uw oplossing gedurende de levenscyclus en erop vertrouwen dat uw resources worden geïmplementeerd in een consistente status.

- U kunt toegangsbeheer toepassen op alle resources in de resourcegroep en deze beleidsregels worden automatisch toegepast wanneer nieuwe resources worden toegevoegd aan de resourcegroep.

- U kunt tags toepassen op de resources om alle resources in uw abonnement op een logische manier te organiseren.

- U kunt notatie JSON (JavaScript Object) gebruiken voor het definiëren van de infrastructuur voor uw oplossing. Het JSON-bestand staat bekend als Resource Manager-sjabloon.

- U kunt de afhankelijkheden tussen resources zo definiëren dat deze in de juiste volgorde worden geïmplementeerd.

![Resourcebeheer](./media/governance-in-azure/security-governance-in-azure-fig4.png)

Resource Manager kunt u resources in zinvolle groepen voor beheer, facturerings- of natuurlijke affiniteit plaatsen. Zoals eerder vermeld, wordt Azure heeft twee implementatiemodellen. In de eerdere [model Klassiek](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model), de basiseenheid voor beheer van het abonnement is. Was het moeilijk om op te splitsen resources binnen een abonnement, wat geleid tot het maken van een groot aantal abonnementen heeft. Met het Resource Manager-model hebt gezien de introductie van resourcegroepen.

Een resourcegroep is een container met verwante resources voor een Azure-oplossing. [De resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) kunnen zijn voor alle resources voor de oplossing of alleen de resources die u wilt beheren als groep. U bepaalt hoe resources worden toegewezen aan resourcegroepen op basis van wat voor uw organisatie het meest zinvol is.

Ga voor aanbevelingen over sjablonen naar [Aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

Azure Resource Manager analyseert afhankelijkheden om ervoor te zorgen dat de resources in de juiste volgorde worden gemaakt. Als een resource afhankelijk is van een waarde uit een andere resource (zoals een virtuele machine die een opslagaccount nodig heeft voor schijven), stelt u een afhankelijkheid in.

>[!Note]
>Zie voor meer informatie [Afhankelijkheden definiëren in Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies).

U kunt de sjabloon ook gebruiken voor updates aan de infrastructuur. U kunt bijvoorbeeld een resource toevoegen aan uw oplossing en configuratieregels toevoegen voor de resources die al zijn geïmplementeerd. Als de sjabloon specificeert dat een resource wordt gemaakt maar die resource al bestaat, wordt er geen nieuwe asset gemaakt. In plaats daarvan voert Azure Resource Manager een update uit. Azure Resource Manager werkt de bestaande asset bij naar dezelfde toestand als een nieuwe asset.

Resource Manager biedt uitbreidingen voor scenario's als u extra bewerkingen zoals het installeren van software die niet is opgenomen in de installatie nodig.

## <a name="resource-tracking"></a>Bronnen bijhouden

Als gebruikers in uw organisatie een resource aan het abonnement toevoegt, wordt het steeds belangrijker om resources koppelen aan de juiste afdeling, de klant en de omgeving. U kunt metagegevens koppelen aan resources via labels. U gebruikt [labels](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) informatie bieden over de bron of de eigenaar. Labels kunnen u niet alleen aggregeren en resources op verschillende manieren groeperen, maar die gegevens gebruiken voor de doeleinden van doorberekening.

Gebruik tags wanneer u een verzameling complexe resourcegroepen en resources hebt en u deze assets moet visualiseren op een manier die relevant voor u is. U kunt bijvoorbeeld resources taggen die een vergelijkbare rol hebben in uw organisatie of bij dezelfde afdeling horen.

Zonder tags kunnen gebruikers in uw organisatie meerdere resources maken die later mogelijk moeilijk zijn te identificeren en te beheren. Mogelijk wilt u alle resources voor een project verwijderen. Als u deze resources zijn niet gemarkeerd voor het project, moet u deze handmatig zoeken. Taggen kan een belangrijke manier zijn om onnodige kosten in uw abonnement te voorkomen.

Resources hoeven niet tot dezelfde resourcegroep te behoren om een tag te kunnen delen. U kunt uw eigen tagtaxonomie maken om ervoor te zorgen dat alle gebruikers in uw organisatie gebruikmaken van veelgebruikte tags en niet per ongeluk afwijkende tags toepassen (zoals 'afd' in plaats van 'afdeling').

Bronbeleid kunnen u voor het maken van standaardregels voor uw organisatie. U kunt beleidsregels die zorg ervoor dat resources worden gemarkeerd met de juiste waarden maken.

> [!Note]
> Zie voor meer informatie [facturering tags beleid initiatief](../azure-policy/scripts/billing-tags-policy-init.md).

U kunt ook getagde resources via Azure Portal weergeven.

Het [gebruiksrapport](https://docs.microsoft.com/azure/billing/billing-understand-your-bill) voor uw abonnement bevat tagnamen en -waarden, zodat u de kosten op basis van tags kunt opdelen.

> [!Note]
> Zie voor meer informatie over tags [facturering tags beleid initiatief](../azure-policy/scripts/billing-tags-policy-init.md).

Voor tags gelden de volgende beperkingen:

- Elke resource of resourcegroep kan maximaal 15 tag sleutel/waarde-paren hebben. Deze beperking geldt alleen voor labels direct worden toegepast op de resourcegroep of resource. Een resourcegroep mag veel bronnen die elk 15 tag sleutel/waarde-paren hebben.

- De tagnaam is beperkt tot 512 tekens.

- De tagwaarde is beperkt tot 256 tekens.

- Tags die zijn toegepast op de resourcegroep, worden niet overgenomen door de resources in deze resourcegroep.

Als u meer dan 15 waarden aan een resource moet koppelen, gebruikt u een JSON-tekenreeks voor de tagwaarde. De JSON-tekenreeks kan veel waarden die worden toegepast op een enkel label-sleutel bevatten.

### <a name="tags-and-billing"></a>Labels en facturering

Labels kunnen u uw factureringsgegevens groep. Bijvoorbeeld, als u meerdere virtuele machines voor verschillende organisaties uitvoert, gebruiken de codes voor een groep gebruik door kostenplaats. U kunt ook codes gebruiken om kosten te categoriseren door de runtimeomgeving. zoals het gebruik van facturering voor virtuele machines die worden uitgevoerd in de productieomgeving.

Vindt u informatie over tags via de [Azure brongebruik en RateCard APIs](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) of het gebruik door komma's gescheiden waarden (CSV)-bestand. Downloaden van het bestand informatie over het gebruik van de [Azure-accounts portal](https://account.windowsazure.com/) of [EA portal](https://ea.azure.com/).

>[!Note]
> Zie voor meer informatie over de programmatische toegang tot factureringsgegevens [inzicht in uw Microsoft Azure-brongebruik](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview). Zie voor REST-API-bewerkingen, [Azure Billing REST API-verwijzing](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Wanneer u het gebruik van CSV voor services die ondersteuning bieden voor tags met facturering downloadt, wordt de labels worden weergegeven in de kolom labels.

## <a name="critical-resource-controls"></a>Besturingselementen voor kritieke bronnen

Als uw organisatie wordt basisservices toegevoegd aan het abonnement, wordt het steeds belangrijker om ervoor te zorgen dat deze services beschikbaar om te voorkomen dat de zakelijke onderbroken zijn. [Resource vergrendelingen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) kunt u beperken van bewerkingen op waardevolle bronnen waar wijzigt of verwijdert deze zou aanzienlijke gevolgen voor uw toepassingen of cloudinfrastructuur hebben. U kunt vergrendelingen toepassen op een abonnement, resourcegroep of resource. U kunt gewoonlijk vergrendelingen toepassen op fundamentele resources, zoals virtuele netwerken, gateways en storage-accounts.

Resource vergrendelingen momenteel ondersteuning voor twee waarden: CanNotDelete en alleen-lezen. CanNotDelete betekent dat gebruikers (met de juiste rechten) kunnen wel lezen of wijzigen van een bron, maar kan niet worden verwijderd. Alleen-lezen betekent dat gemachtigde gebruikers niet verwijderen of wijzigen van een resource.

Vergrendelingen van Resource Manager alleen van toepassing op bewerkingen die in de vlak management, die uit de bewerkingen die worden verzonden optreden bestaat naar <https://management.azure.com>. De vergrendelingen beperken niet hoe resources hun eigen functies uitvoeren. Wijzigingen in de resourcedefinitie zijn beperkt, maar de bewerkingen van resources zijn niet beperkt. Bijvoorbeeld, een vergrendeling van het kenmerk alleen-lezen voor een SQL-Database wordt voorkomen dat u de database wijzigen of verwijderen, maar dit voorkomt niet dat u maken, bijwerken of verwijderen van gegevens in de database.

Toepassen van **ReadOnly** kan leiden tot onverwachte resultaten optreden omdat bepaalde bewerkingen die lijkt lezen bewerkingen extra acties vereist. Bijvoorbeeld, als een **ReadOnly** vergrendeling van een opslagaccount wordt voorkomen dat alle gebruikers weergeven van de sleutels. De lijst met sleutels opnieuw wordt verwerkt door een POST-aanvraag omdat de geretourneerde sleutels zijn beschikbaar voor schrijfbewerkingen.

![Besturingselementen voor kritieke bronnen](./media/governance-in-azure/security-governance-in-azure-fig5.png)

Voor een ander voorbeeld plaatsen van een vergrendeling van het kenmerk alleen-lezen op een App Service-resource voorkomt u dat Visual Studio Server Explorer-bestanden voor de resource worden weergegeven omdat die interactie voor toegang voor schrijven vereist.

In tegenstelling tot rollen gebaseerd toegangsbeheer kunt u management vergrendelingen toepassen van een beperking voor alle gebruikers en rollen. Zie voor meer informatie over het instellen van machtigingen voor gebruikers en rollen [toegangsbeheer op basis van rollen in Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

Wanneer u een vergrendeling op een bovenliggend bereik toepast, nemen alle resources binnen dat bereik de dezelfde vergrendeling. Zelfs resources die u later toevoegen overnemen de vergrendeling van het bovenliggende item. De meest beperkende vergrendeling in de overname voorrang.

Als u wilt maken of verwijderen van management vergrendelingen, u moet toegang hebben tot Microsoft.Authorization/ _of Microsoft.Authorization/locks/_ acties. Van de ingebouwde rollen alleen **eigenaar** en **beheerder voor gebruikerstoegang** deze acties worden verleend.

## <a name="api-access-to-billing-information"></a>API-toegang tot factureringsgegevens

Azure Billing-API's gebruikt om pull gebruiks- en gegevens bij uw voorkeur hulpprogramma's voor gegevensanalyse. Het gebruik van Azure-bronnen en RateCard APIs kunt u nauwkeurige voorspellen en beheren van uw kosten. De API's worden geïmplementeerd als een Resource Provider en een deel van de reeks API's beschikbaar gesteld door de Azure Resource Manager.

### <a name="azure-resource-usage-api-preview"></a>Azure-resource gebruiks-API (Preview)

Gebruik de Azure [Resource gebruik API](https://msdn.microsoft.com/library/azure/mt219003) om uw gegevens geschatte Azure-verbruik. De API bevat:

- **Azure op rollen gebaseerd toegangsbeheer** -configureren toegangsbeleid op de [Azure-portal](https://portal.azure.com/) of via [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot krijgen kunnen de gebruiksgegevens van abonnement. Aanroepfuncties moeten standaard tokens van Azure Active Directory voor verificatie gebruiken. De aanroeper toevoegen aan de lezer facturering, een lezer, een eigenaar of een bijdrager rol voor toegang tot de gebruiksgegevens voor een specifieke Azure-abonnement.

- **Elk uur of dagelijkse samenvoegingen** - aanroepfuncties kunnen opgeven of ze hun Azure gebruiksgegevens wilt elk uur tijdsintervallen of dagelijks tijdsintervallen. De standaardwaarde is dagelijks.

- **Metagegevens van het exemplaar (inclusief resourcetags)** – ophalen op exemplaarniveau details, zoals de volledig gekwalificeerde resource-uri (/subscriptions/ {abonnement-id} /..), wordt de informatie over de resource en resourcetags. Deze metagegevens kunt u deterministisch en programmatisch gebruik toewijzen door de labels voor use cases als cross in rekening gebracht.

- **Bron-metagegevens** -Resourcedetails zoals de naam van de meter, meter categorie meter subcategorie, eenheid en regio geven de aanroeper een beter inzicht in wat is verbruikt. We ook proberen om te worden uitgelijnd resource metagegevens terminologie via de Azure-portal, Azure gebruik CSV, EA facturering CSV en andere ervaringen openbare waarmee u correleren van gegevens over ervaringen.

- **Gebruik voor alle typen bieden** – gebruiksgegevens beschikbaar is voor alle typen zoals betalen naar gebruik, MSDN, bedrag, financieel tegoed en EA bieden.

**Azure-resource RateCard API (Preview)**

De Azure Resource RateCard API gebruiken om de lijst met beschikbare Azure-resources en informatie over de geschatte prijzen voor elk. De API bevat:

- **Azure op rollen gebaseerd toegangsbeheer** : uw beleidsregels configureren op de Azure-portal of via Azure PowerShell-cmdlets om op te geven welke gebruikers of toepassingen toegang kunnen krijgen tot de gegevens RateCard. Aanroepfuncties moeten standaard tokens van Azure Active Directory voor verificatie gebruiken. De aanroeper toevoegen aan de lezer, de eigenaar of bijdrager rol voor toegang tot de gebruiksgegevens voor een bepaald Azure-abonnement.

- **Ondersteuning voor betalen per gebruik, MSDN, bedrag en financieel tegoed (EA niet ondersteund) biedt** -voor deze API biedt Azure-aanbieding niveau snelheid informatie. De aanroeper van deze API moet in de aanbieding informatie ophalen van de resourcedetails van de en tarieven doorgeven. We zijn momenteel niet mogelijk om de EA omdat EA aanbiedingen tarieven per inschrijving hebt aangepast. Hier volgen enkele van de scenario's die zijn aangebracht met de combinatie van het gebruik en de APIs RateCard mogelijk:

- **Azure besteden tijdens de maand** - gebruiken om de combinatie van het gebruik en RateCard APIs ophalen beter inzicht in uw cloud-uitgaven in de maand. U kunt de uur- en buckets van gebruiks- en kosten maakt een schatting analyseren.

- **Stel waarschuwingen** – het gebruik en de APIs RateCard gebruiken voor het ophalen van de geschatte cloud verbruik en de kosten en resource of monetaire gebaseerde waarschuwingen instellen.

- **Factuur voorspellen** – Get uw geschatte gebruiks- en cloud besteden en machine learning-algoritmen om te voorspellen wat de factuur aan het einde van de factureringscyclus zou zijn van toepassing.

- **Vooraf verbruik kosten analysis-** – de RateCard-API gebruiken om te voorspellen hoeveel uw factuur voor niet het verwachte gebruik wanneer u uw workloads naar Azure verplaatst. Als u bestaande workloads in andere clouds of privéclouds hebt, kunt u ook uw gebruik met de Azure toewijzen te besteden aan de tarieven voor een betere schatting van Azure ophalen. Deze schatting biedt u de mogelijkheid om te draaien op aanbieding en tussen de verschillende aanbiedingstypen dan betalen naar gebruik, zoals bedrag en financieel tegoed vergelijken. De API biedt u de mogelijkheid om te zien kostenverschillen per regio ook en kunt u een wat-als-kostenanalyse waarmee u beslissingen voor de implementatie uitvoeren.

- **Wat-als-analyse** -u kunt bepalen of het meest efficiënt werkbelastingen worden uitgevoerd in een andere regio of op een andere configuratie van de Azure-resource. Azure-resourcekosten kunnen verschillen op basis van de Azure-regio u.

- U kunt ook bepalen als een ander Azure-aanbiedingtype resulteert in een betere rentabiliteit op een Azure-resource.

## <a name="networking-controls"></a>Besturingselementen voor netwerken

Toegang tot bronnen kan worden (binnen het bedrijfsnetwerk) interne of externe (via het internet). Het is gemakkelijk voor gebruikers in uw organisatie per ongeluk resources in de verkeerde positie plaatsen en blootstellen aan schadelijke toegang. Als met on-premises / apparaten, ondernemingen moeten juiste besturingselementen toevoegen om ervoor te zorgen dat het Azure-gebruikers de juiste keuzes aanbrengen.

![Besturingselementen voor netwerken](./media/governance-in-azure/security-governance-in-azure-fig6.png)

We identificeren belangrijkste resources die elementaire controle van toegang bieden voor abonnement-toezicht. De belangrijkste resources bestaan uit:

### <a name="network-connectivity"></a>Netwerkverbinding

[Virtuele netwerken](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) containerobjecten voor subnetten zijn. Hoewel niet strikt noodzakelijk is het vaak gebruikt bij het verbinden van toepassingen tot interne bedrijfsbronnen. De service Azure Virtual Network kunt u veilig Azure-resources met elkaar te verbinden met virtuele netwerken (vnet's).

Een VNet is een weergave van uw eigen netwerk in de cloud. Een VNet is een logische isolatie van de Azure-cloud toegewezen aan uw abonnement. U kunt de VNets ook verbinding maken met uw on-premises netwerk.

Hieronder vindt u mogelijkheden voor virtuele netwerken van Azure:

- **Isolatie**: VNets die zijn geïsoleerd van elkaar. U kunt afzonderlijke VNets voor ontwikkeling, testen en productie die gebruikmaken van dezelfde CIDR-adresblokken maken. Als u daarentegen, kunt u meerdere VNets die gebruikmaken van verschillende CIDR-adresblokken en netwerken met elkaar verbinden. U kunt een VNet segmenteren in meerdere subnetten. Azure biedt interne naamomzetting voor VM's en Cloudservices rolinstanties zijn verbonden met een VNet. U kunt desgewenst een VNet voor het gebruik van uw eigen DNS-servers, in plaats van Azure interne naamomzetting configureren.

- **Verbinding met Internet**: alle Azure virtuele Machines (VM) en Cloud Services-rolexemplaren verbonden met een VNet hebben toegang tot het Internet standaard. U kunt binnenkomende toegang tot specifieke bronnen, indien nodig.

- **Azure-resource connectiviteit**: Azure-resources zoals Cloudservices en virtuele machines kunnen worden verbonden met hetzelfde VNet. De resources kunnen verbinden met elkaar met behulp van privé IP-adressen, zelfs als ze zich in verschillende subnetten. Azure biedt standaardroutering tussen subnetten, VNets en on-premises netwerken, zodat u niet hoeft te configureren en beheren van routes.

- **VNet-connectiviteit**: vnet's kunnen worden verbonden met elkaar, het inschakelen van de resources die zijn verbonden met een VNet om te communiceren met alle bronnen op eventuele andere VNet.

- **Lokale connectiviteit**: vnet's kunnen worden verbonden met on-premises netwerken via persoonlijke netwerkverbindingen tussen uw netwerk en Azure of een site-naar-site VPN-verbinding via Internet.

- **Filteren van verkeer**: virtuele machine en Cloud Services-rol exemplaren netwerkverkeer kan worden gefilterd binnenkomend en uitgaand op bron-IP-adres en poort, doel-IP-adres en poort en protocol.

- **Routering**: U kunt eventueel Azure standaard routering met BGP-routes via een netwerkgateway of configureren van uw eigen routes overschrijven.

## <a name="network-access-controls"></a>Netwerk-toegangsbeheer

[Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) zijn vergelijkbaar met een firewall en regels voor hoe een resource "praten kan" opgeeft via het netwerk. Ze bieden gedetailleerde controle over hoe / als een subnet (of de virtuele machine) verbinding met Internet of andere subnetten in hetzelfde virtuele netwerk maken kunt.

Een netwerkbeveiligingsgroep (Network Security Group, NSG) bevat een lijst met beveiligingsregels waarmee netwerkverkeer naar resources die zijn verbonden met virtuele Azure-netwerken (VNet) wordt toegestaan of geweigerd. NSG's kunnen worden gekoppeld aan subnetten, afzonderlijke virtuele machines (klassiek) of afzonderlijke netwerkinterfaces (NIC) die zijn gekoppeld aan VM’s (Resource Manager).

Wanneer een NSG is gekoppeld aan een subnet, zijn de regels van toepassing op alle resources die zijn verbonden met het subnet. Verkeer kan verder worden beperkt door ook een NSG te koppelen aan een VM of NIC.

## <a name="security-and-continuous-compliance-with-organizational-standards"></a>Beveiliging en continue naleving met organisatie-standaarden

Elk bedrijf heeft verschillende behoeften en elk bedrijf wordt benutten van verschillende voordelen van cloudoplossingen. Toch klanten van alle soorten het dezelfde basis twijfels hebt over de verplaatsing naar de cloud. Ze willen dat de gegevens beveiligd en privé, alle behoud transparantie en naleving worden gehouden en ze willen beheer van de bijbehorende gegevens behouden.

Klanten willen van cloudproviders is:

- **Beveiligen van onze gegevens** terwijl zijn bevestigd of de cloud kan bieden betere beveiliging van gegevens en beheer, IT-managers nog steeds betreft dat migreren naar de cloud ze gevoeliger voor hackers dan de huidige intern laat oplossingen.

- **Onze gegevens behouden** persoonlijke Cloud-services verhogen unieke privacy uitdagingen voor bedrijven. Bedrijven zoeken naar de cloud op te slaan op de kosten van infrastructuur hun flexibiliteit verbeteren, bang ze ook te verliezen van de controle van waar hun gegevens worden opgeslagen, wie het en hoe deze wordt gebruikt.

- **Geef ons besturingselement** zelfs als ze van de cloud meer innovatieve oplossingen te implementeren profiteren, bedrijven zijn zeer betrokken zijn bij de controle over hun gegevens verliezen. De recente vermeldingen van overheidsinstanties toegang tot gegevens van de klant, juridische en extra juridische wijze, zorg sommige CIO hoede hun gegevens opslaan in de cloud.

- **Bevorderen de transparantie** terwijl de beveiliging, privacy en besturingselement belangrijk voor zakelijke besluitvormers zijn, ze ook de mogelijkheid onafhankelijk controleren hoe de gegevens worden opgeslagen, geopend en beveiligde wilt.

- **Behouden van compatibiliteit** bij bedrijven uitbreiden voor het gebruik van cloudtechnologieën, de complexiteit en het bereik van standaarden en -voorschriften gaan ontwikkelen. Bedrijven moeten weten dat hun compatibiliteit normen wordt voldaan en dat naleving wordt aangepast als de wijziging van de voorschriften gedurende een bepaalde periode.

## <a name="security-configuration-monitoring-and-alerting"></a>Configuratie van beveiliging, controle en waarschuwingen

Azure-abonnees kunnen hun cloudomgevingen beheren vanaf meerdere apparaten, waaronder beheerwerkstations, de pc's van ontwikkelaars en zelfs apparaten van bevoegde eindgebruikers met taakspecifieke rechten. In sommige gevallen worden beheerfuncties uitgevoerd via het web gebaseerde consoles, zoals de Azure-portal. In andere gevallen zijn er mogelijk rechtstreekse verbindingen naar Azure vanaf on-premises systemen via virtuele particuliere netwerken (VPN), Terminal Services, protocollen van clienttoepassingen of de Azure Service Management API (SMAPI) (via een programma). Clienteindpunten kunnen bovendien zowel in een domein zijn samengevoegd als op zichzelf staand en niet-beheerd zijn, zoals tablets en smartphones.

Hoewel meerdere mogelijkheden voor toegang en beheer uitgebreide opties bieden, kan deze verscheidenheid voor een cloudimplementatie aanzienlijke risico's met zich meebrengen. Het beheren, volgen en controleren van beheeracties kan hierdoor worden bemoeilijkt. Deze verscheidenheid kan ook leiden tot beveiligingsrisico's, omdat er ongereglementeerde toegang plaatsvindt tot clienteindpunten die worden gebruikt voor het beheer van cloudservices. Het gebruik van algemene of persoonlijke werkstations voor het ontwikkelen en beheren van infrastructuur brengt onvoorspelbare bedreigingen met zich mee voor bijvoorbeeld surfen op internet (denk aan waterhole-aanvallen) of e-mail (zoals social engineering en phishing).

Bewaking, logboekregistratie en controle bieden een basis voor het bijhouden en het begrijpen van beheeractiviteiten, maar vanwege de grote hoeveelheid gegevens die wordt gegenereerd, is het niet altijd haalbaar om alle acties nauwgezet te controleren. Het controleren van de effectiviteit van de beleidsregels voor beheer is echter een aanbevolen procedure.

Azure-beveiliging Governance van AD DS-groepsbeleidsobjecten waarmee alle de beheerders Windows-interfaces, zoals het delen van bestanden. Voer voor beheerwerkstations controle-, bewakings- en logboekregistratieprocessen uit. Houd toegang en gebruik van alle beheerders en ontwikkelaars bij.

### <a name="azure-security-center"></a>Azure security center

De [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) biedt een centrale weergave van de beveiligingsstatus van resources in de abonnementen en aanbevelingen die helpen te voorkomen dat resources waarmee is geknoeid. Ze kan meer gedetailleerd beleid (bijvoorbeeld toepassen beleid aan specifieke resourcegroepen waarmee bedrijven hun houding het risico dat ze adresseert aanpassen) inschakelen.

![Azure Security Center](./media/governance-in-azure/security-governance-in-azure-fig7.png)

Security Center biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen die anders onopgemerkt, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen te detecteren. Nadat u hebt ingeschakeld [beveiligingsbeleid](https://docs.microsoft.com/azure/security-center/security-center-policies) voor resources van een abonnement, analyseert Security Center de beveiliging van uw resources voor het identificeren van mogelijke beveiligingsproblemen. Informatie over uw netwerkconfiguratie is onmiddellijk beschikbaar.

Azure Security Center vertegenwoordigt een combinatie van best practice analyse- en beleidsbeheer voor alle resources binnen een Azure-abonnement. Met dit hulpprogramma krachtige en eenvoudig te gebruiken kunt beveiliging teams en risico managers te detecteren, voorkomen van en reageren op bedreigingen, zoals deze worden automatisch verzameld en beveiligingsgegevens van uw Azure-resources, het netwerk en partneroplossingen zoals antimalwareprogramma analyseert programma's en firewalls.

Bovendien Azure Security Center geavanceerde analyses toegepast, met inbegrip van machine learning en gedragsanalyse tijdens het gebruik van Microsoft-producten en services, de Microsoft Digital Crimes Unit (DCU), Microsoft over wereldwijde dreigingen Security Response Center (MSRC) en externe feeds. [Beveiliging governance](https://www.credera.com/blog/credera-site/azure-governance-part-4-other-tools-in-the-toolbox/) kan worden grote schaal wordt toegepast op het abonnementsniveau of domeinpartitie aan specifieke, gedetailleerde vereisten die worden toegepast op afzonderlijke resources via beleidsdefinitie.

Ten slotte Azure Security Center analyseert de beveiligingsstatus van de resource op basis van deze beleidsregels en gebruikt dit om dashboards begrijpelijke manier mee en waarschuwingen voor gebeurtenissen zoals detectie van malware of schadelijke IP-verbinding probeert te bieden.

>[!Note]
> Zie [Beveiligingsaanbevelingen implementeren in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-recommendations) voor meer informatie over het toepassen van aanbevelingen.

Security Center verzamelt gegevens van uw virtuele machines naar hun beveiligingsstatus beoordelen, aanbevelingen voor beveiliging bieden en een melding op bedreigingen. Wanneer u Security Center voor het eerst opent, worden gegevensverzameling is ingeschakeld op alle virtuele machines in uw abonnement. Verzamelen van gegevens wordt aanbevolen, maar u kunt opt-out door [uitschakelen van gegevensverzameling](https://docs.microsoft.com/azure/security-center/security-center-faq) in het beleid Security Center.

Ten slotte is Azure Security Center een open platform waarmee Microsoft-partners en onafhankelijke softwareleveranciers te maken van de software die in Azure Security Center voor het verbeteren van de mogelijkheden ervan wordt geplaatst.

Azure Security Center bewaakt de volgende Azure-resources:

- Virtuele machines (VM's) (inclusief Cloudservices)

- Virtuele netwerken van Azure

- Azure SQL-service

- Partneroplossingen die zijn geïntegreerd met uw Azure-abonnement zoals web application firewall op virtuele machines en klik op [App Service-omgeving](https://docs.microsoft.com/azure/app-service/app-service-app-service-environments-readme).

### <a name="operations-management-suite"></a>Operations Management Suite

Informatiebeveiliging OMS software ontwikkelings- en service van het team en [governance programma](https://github.com/Microsoft/azure-docs/blob/master/articles/log-analytics/log-analytics-security.md) ondersteunt de zakelijke vereisten en voldoet aan regelgeving zoals beschreven op [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) en [Microsoft Trust Center naleving](https://www.microsoft.com/TrustCenter/Compliance/default.aspx). Hoe OMS beveiligingsvereisten tot stand brengen, identificeert beveiligingsmechanismen beheert en bewaakt de risico's worden er ook beschreven. Jaarlijks, wij controleren beleidsregels, standaarden, procedures en richtlijnen.

Elk teamlid OMS-ontwikkeling ontvangt beveiligingstraining formele toepassing. We gebruiken een versiebeheersysteem intern voor softwareontwikkeling. Elk software-project wordt beveiligd door het versiebeheersysteem.

Microsoft heeft een beveiliging en naleving team dat verantwoordelijk en alle services in Microsoft evalueert. De vaststelling van informatie van het team maken en ze niet zijn gekoppeld aan de technische afdelingen die OMS ontwikkelen. De beveiliging verantwoordelijken hebben hun eigen Managementketen en uitvoering van een onafhankelijke beoordelingen van producten en services om te controleren of de beveiliging en naleving.

Operations Management Suite (ook wel bekend als OMS) is een verzameling beheerservices die volledig op de cloud zijn gericht. In plaats van te implementeren en beheren van lokale bronnen, worden OMS onderdelen volledig gehost in Azure. De benodigde configuratie is minimaal en u kunt alles letterlijk binnen een paar minuten in werking zetten.

![Operations Manager-Suite](./media/governance-in-azure/security-governance-in-azure-fig8.png)

Hoewel OMS-services worden uitgevoerd in de cloud, betekent dat niet dat ze uw on-premises omgeving niet effectief kunnen beheren.

Het plaatsen van een agent op elke Windows of Linux-computer in uw datacenter, en worden gegevens verzonden met logboekanalyse waar deze samen met alle andere gegevens verzameld uit de cloud of op de lokale services kan worden geanalyseerd. Azure Backup en Azure Site Recovery gebruikmaken van de cloud voor back-up en hoge beschikbaarheid voor op de lokale bronnen gebruiken.

Runbooks in de cloud hebben doorgaans geen toegang tot uw on-premises resources, maar u kunt ook een agent installeren op een of meer computers die vervolgens runbooks hosten in uw datacenter. Wanneer u een runbook start, kunt u eenvoudig aangeven of u het wilt uitvoeren in de cloud of op een lokale werkrol.

De kernfunctionaliteit van OMS wordt geleverd door een reeks services die in Azure worden uitgevoerd. Elke service biedt een specifieke beheerfunctie. U kunt services combineren om verschillende beheerscenario's te bewerkstelligen.

![Operations Manager-Suite](./media/governance-in-azure/security-governance-in-azure-fig9.JPG)

Azure-bewerking manager biedt de functionaliteiten door oplossingen voor het beheer. [Oplossingen voor](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions) voorverpakte sets van logica die een beheerscenario gebruik te maken van een of meer OMS-services te implementeren.

![Azure-bewerking beheren](./media/governance-in-azure/security-governance-in-azure-fig10.png)

Er zijn verschillende oplossingen beschikbaar van Microsoft en partners die u gemakkelijk kunt toevoegen aan uw Azure-abonnement om de waarde van uw investering in OMS te vergroten.

Als partner kunt u uw eigen oplossingen voor de ondersteuning van uw toepassingen en services en ze te bieden aan gebruikers via de snel starten-sjablonen of Azure Marketplace.

## <a name="performance-alerting-and-monitoring"></a>Prestatiewaarschuwingen en controle

### <a name="alerting"></a>Waarschuwingen

Waarschuwingen zijn een methode voor het bewaken van metrische gegevens voor Azure-resources, gebeurtenissen of Logboeken en wordt gewaarschuwd wanneer een voorwaarde die u opgeeft wordt voldaan.

**Waarschuwingen in verschillende Azure-services**

Waarschuwingen zijn beschikbaar via andere services, waaronder:

- Application Insights: Kunt WebTest en metrische waarschuwingen.

>[!Note]
> Zie [meldingen instellen in Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) en [beschikbaarheid en reactiesnelheid van een website bewaken](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability).

- Log Analytics (Operations Management Suite): De routering van de activiteit en de logboeken met diagnostische gegevens met logboekanalyse is ingeschakeld. Operations Management Suite kunt metrische gegevens, het logboek en overige Waarschuwingstypen.

>[!Note]
> Zie voor meer informatie, waarschuwingen in [logboekanalyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts).

- Azure bewaken: Hiermee schakelt u waarschuwingen op basis van zowel de metrische waarden als de activiteit logboekgebeurtenissen. U kunt de [REST-API van Azure-Monitor](https://msdn.microsoft.com/library/dn931943.aspx) om waarschuwingen te beheren.

>[!Note]
> Zie voor meer informatie [met behulp van de Azure-portal, PowerShell of de opdrachtregelinterface voor het maken van waarschuwingen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal).

### <a name="monitoring"></a>Bewaking

Prestatieproblemen in uw cloud-app kunnen invloed hebben op uw bedrijf. Degradations kan met meerdere onderdelen onderling verbonden en releases vaak gebeuren op elk gewenst moment. En als u een app ontwikkelt, uw gebruikers problemen die u hebt niet vinden in het testen van meestal te detecteren. U moet weten over deze problemen direct en hulpprogramma's voor het opsporen en oplossen van de problemen hebben. Microsoft Azure is een reeks hulpmiddelen om deze problemen te identificeren.

**Hoe bewaak ik mijn Azure-cloud-apps?**

Er is een reeks hulpmiddelen voor het bewaken van Azure-toepassingen en services. Enkele van hun functies overlappen. Dit is gedeeltelijk omwille van de historische en deels vervaging tussen ontwikkeling en de werking van een toepassing.

Hier volgen de principal-hulpprogramma's:

- **Monitor voor Azure** is basic hulpprogramma voor het bewaken van services die worden uitgevoerd op Azure. Dit biedt u niveau van de infrastructuur gegevens over de doorvoer van een service en de omgeving. Als u uw apps in Azure beheert, krijgt bepalen of schaal omhoog of omlaag resources, Azure Monitor u dan gebruik om te starten.

- **Application Insights** kunnen worden gebruikt voor ontwikkeling en als een productie-bewakingsoplossing. Deze werkt met een pakket in uw app installeert en biedt dus een meer interne weergave van wat er gebeurt. De gegevens omvatten reactietijden van afhankelijkheden, uitzondering traceringen, foutopsporing van momentopnamen, profielen kan worden uitgevoerd. Het biedt krachtige smart tools voor het analyseren van alle deze telemetrie kunt u fouten opsporen in een app, en ook om te begrijpen wat gebruikers doen met het. U kunt zien of een piek in de reactietijden veroorzaakt door iets in een app, of een bepaalde externe resourcing probleem wordt. Als u Visual Studio gebruiken en de app beschadigd is, kunt u geleid rechts op de regel (s) probleem van code zodat u deze kunt oplossen.

- **Meld u Analytics** is voor gebruikers die moeten afstemmen van de prestaties en onderhoud plannen van toepassingen die worden uitgevoerd in de productieomgeving. In Azure is gebaseerd. Deze verzamelt en gegevens uit diverse bronnen, maar met een vertraging van 10 tot 15 minuten worden verzameld. Dit biedt een holistische oplossing voor IT-beheer voor Azure, on-premises en cloud-gebaseerde infrastructuur hebben van derden (zoals Amazon Web Services). Het biedt uitgebreidere hulpprogramma's om gegevens te analyseren in meer bronnen, kunt u complexe query's via alle logboeken en proactief waarschuwt van de opgegeven voorwaarden. U kunt ook aangepaste gegevens verzamelen in de centrale opslagplaats zodat kunnen opvragen en visualiseren van deze.

- **System Center Operations Manager (SCOM)** is voor het beheren en controleren van grote cloud-installaties. U mogelijk al bekend bent met het beheerprogramma voor on-premises Windows Server en op basis van Hyper-V-clouds, maar het kan ook integreren met en beheren van apps van Azure. Onder andere kan Application Insights worden geïnstalleerd op bestaande live apps. Als een app uitgeschakeld wordt, krijgt u in seconden.


## <a name="next-steps"></a>Volgende stappen

- [Aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

- [Voorbeelden van de implementatie van Azure-abonnement governance](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-examples).

- [Microsoft Azure Government](https://docs.microsoft.com/azure/azure-government/).
