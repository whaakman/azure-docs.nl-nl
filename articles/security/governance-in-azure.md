---
title: Governance in Azure | Microsoft Docs
description: Meer informatie over cloud-gebaseerde computing services die omhoog en omlaag schalen kunnen om te voldoen aan de behoeften van uw toepassing of enterprise.
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
ms.openlocfilehash: 6e5b6fac25c8c7f76991a58fbcab363c6fc20f12
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44380349"
---
# <a name="governance-in-azure"></a>Governance in Azure

Azure biedt u veel beveiligingsopties en de mogelijkheid om deze te beheren zodat u kunt voldoen aan de unieke vereisten van implementaties van uw organisatie.

Azure-cloud governance verwijst naar de besluitvormingsprocessen, criteria en beleidsregels die betrokken zijn bij het plannen, architectuur, aanschaf, implementatie, bewerking en beheer van cloud computing. Azure-cloud governance biedt een geïntegreerde controle en advies benadering voor het controleren en te adviseren organisaties op hun gebruik van het Azure-platform. 

Voor het maken van een abonnement voor Azure-cloud-beheer, moet u een diepgaande blik op de mensen, processen en -technologieën nu op locatie. Vervolgens kunt samenstellen van frameworks die het gemakkelijk voor IT-ondersteuning voor consistent zakelijke behoeften en tegelijkertijd gebruikers de flexibiliteit om de functies van Azure te gebruiken.

## <a name="implementation-of-policies-processes-and-standards"></a>Implementatie van beleid, processen en standaarden 

Management heeft tot stand gebracht rollen en verantwoordelijkheden over de implementatie van het beveiligingsbeleid van de informatie en operationele continuïteit waakt binnen Azure. Azure-beheer is verantwoordelijk voor het beheren van beveiliging en bedrijfscontinuïteit in de desbetreffende teams (met inbegrip van derden). Dit vergemakkelijkt de naleving van beveiligingsbeleid, processen en standaarden.

### <a name="account-provisioning"></a>Account inrichten

Account-hiërarchie te definiëren, is een belangrijke stap te gebruiken en structuur van Azure-services binnen een bedrijf. Het is de bestuursstructuur core. Klanten met een Enterprise Agreement (EA) kunnen de omgeving in afdelingen, accounts en abonnementen onderverdelen.

![Account inrichten](./media/governance-in-azure/security-governance-in-azure-fig1.png)

Als u geen een Enterprise Agreement hebt, kunt u overwegen [Azure tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) op het abonnementsniveau van het voor het definiëren van de hiërarchie. Een Azure-abonnement is de basiseenheid voor dat alle resources bevat. Het definieert ook enkele beperkingen in Azure, zoals het aantal kernen en resources. Abonnementen kunnen bevatten [resourcegroepen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), kan die resources bevatten. [Op rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) beginselen zijn van toepassing op de drie niveaus.

Iedere onderneming komt niet overeen. Voor niet-zakelijke bedrijven kan de hiërarchie van het gebruik van Azure-tags voor flexibiliteit in hoe Azure wordt geordend. Voordat u resources in Azure implementeert, moet u een hiërarchie modelleren en begrijp de gevolgen van facturering, toegang tot bronnen en complexiteit.

### <a name="subscription-controls"></a>Besturingselementen voor abonnement

Abonnementen te bepalen hoe Resourcegebruik wordt gerapporteerd en kosten in rekening gebracht. U kunt abonnementen voor afzonderlijke facturering en betaling instellen. Een Azure-account kan meerdere abonnementen hebt. Abonnementen kunnen worden gebruikt om te bepalen van het Azure-resource-gebruik van meerdere afdelingen in een bedrijf.

Bijvoorbeeld, een bedrijf heeft IT, HR, en Marketing afdelingen, en deze afdelingen verschillende projecten worden uitgevoerd. Het bedrijf kunt basis van de facturering voor het gebruik van Azure-resources, zoals virtuele machines van elke afdeling. Het bedrijf kan vervolgens de financiën van elke afdeling bepalen.

Azure-abonnementen tot stand brengen van de drie parameters:

- De unieke abonnements-ID

- Facturering-locatie

- Instellen van de beschikbare bronnen

Deze parameters bevatten voor een persoon, een Microsoft-account-ID, een creditcardnummer en de volledige suite met Azure-services. Microsoft hanteert een verbruik van grenzen, afhankelijk van het abonnementstype.

Azure-inschrijving hiërarchieën definiëren hoe de services binnen een Enterprise Agreement worden opgebouwd. De portal voor Enterprise Agreement kan klanten toegang tot Azure-resources die zijn gekoppeld aan een Enterprise Agreement op basis van flexibele hiërarchieën die kunnen aangepast aan de behoeften van een organisatie worden delen. Het hiërarchie-patroon moet overeenkomen met het beheer en de geografische structuur ter compensatie van de bijbehorende facturerings- en -toegang van een organisatie.

De drie op hoog niveau hiërarchie-patronen worden functionele, business unit, en geografische. Afdelingen zijn een administratieve om voor te bereiden voor account groeperingen. Binnen elke afdeling kunnen de accounts worden toegewezen aan abonnementen, die een silo's voor facturering en limieten voor verschillende belangrijke in Azure (bijvoorbeeld aantal virtuele machines en storage-accounts maken).

![Besturingselementen voor abonnement](./media/governance-in-azure/security-governance-in-azure-fig2.png)


Azure-abonnementen voor organisaties met een Enterprise Agreement, Ga als volgt een hiërarchie vier niveaus:

1. Enterprise-beheerder voor inschrijving

2. Afdelingsbeheerder

3. De eigenaar van account

4. Servicebeheerder

Deze hiërarchie geldt het volgende:

- Facturering-relatie.

- Accountbeheer.

- Toegang tot resources via RBAC.

- Grenzen:

  - Gebruik en facturering (tariefkaart op basis van aanbieding getallen)

  - Limieten

  - Virtueel netwerk

- Bijlage naar Azure Active Directory (Azure AD). Een Azure AD-exemplaar kan worden gekoppeld met veel abonnementen zijn.

- Koppeling met een enterprise-inschrijving-account.

### <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

[RBAC](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) beschikt gedetailleerd toegangsbeheer van bronnen in Azure. Met behulp van RBAC, kunt u alleen de mate van toegang verlenen dat gebruikers moeten hun werk. Bedrijven moeten zich richten op uw werknemers de exacte machtigingen die ze nodig hebben. Te veel machtigingen beschikbaar maken van een account voor aanvallen. Te weinig machtigingen betekenen dat werknemers hun werk efficiënter kunnen niet ophalen. 

In plaats van zodat iedereen onbeperkte machtigingen in uw Azure-abonnement of resources, kunt u alleen bepaalde acties toestaan. U kunt bijvoorbeeld RBAC gebruiken om één werknemer virtuele machines in een abonnement beheren terwijl een andere werknemer SQL-databases in hetzelfde abonnement beheert.

Om toegang te verlenen, kunt u rollen toewijzen aan gebruikers, groepen of toepassingen met een bepaald bereik. Het bereik van een roltoewijzing kan een abonnement, een resourcegroep of één resource zijn. Een rol die is toegewezen aan een bovenliggend bereik ook verleent toegang aan de onderliggende objecten die hierin zijn opgenomen. Een gebruiker met toegang tot een resourcegroep waarin u kunt bijvoorbeeld alle resources die deze, zoals websites, virtuele machines en subnetten bevat te beheren. Binnen elk abonnement, kunt u maximaal 2000 roltoewijzingen.

Een rol is een verzameling van machtigingen en RBAC heeft diverse ingebouwde rollen. De volgende ingebouwde rollen toepassen op alle resourcetypen:

- **De eigenaar van** heeft volledige toegang tot alle bronnen, waaronder het recht op toegang aan anderen delegeren.

- **Inzender** kunt maken en beheren van alle typen Azure-resources, maar kan geen toegang om anderen te verlenen.

- **Lezer** alle Azure-resources kunt bekijken.

De rest van de ingebouwde rollen in Azure kunt beheer van specifieke Azure-resources. De rol Inzender voor virtuele machines kan bijvoorbeeld de gebruiker te maken en beheren van virtuele machines. Zie voor een lijst van de ingebouwde rollen en hun activiteiten, [ingebouwde RBAC-rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

RBAC biedt ondersteuning voor beheerbewerkingen van de Azure-resources in Azure portal en Azure Resource Manager-API's. In de meeste gevallen kan geen RBAC-niveau bewerkingen voor Azure-resources toestaan. Zie voor meer informatie over hoe RBAC is bedoeld voor gegevensbewerkingen [begrijpen roldefinities](https://docs.microsoft.com/azure/role-based-access-control/role-definitions).

Als de ingebouwde rollen niet voldoen aan uw specifieke behoeften, kunt u [maakt u een aangepaste rol](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). Net als de ingebouwde rollen kunnen aangepaste rollen worden toegewezen aan gebruikers, groepen en toepassingen op het abonnement, resourcegroep en bereik van de resource. U kunt ook aangepaste rollen maken met behulp van [Azure PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli), en de [REST-API](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

### <a name="resource-management"></a>Resourcebeheer

Azure biedt twee implementatiemodellen: [klassieke](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) en Azure Resource Manager.

In het klassieke model, wordt elke resource afzonderlijk bestaat. Er is geen manier om te groeperen van gerelateerde resources. U moet handmatig bij te houden welke resources gezamenlijk uw oplossing of de toepassing en vergeet niet dat ze op een gecoördineerde manier beheren. De basiseenheid voor beheer van is het abonnement. Het is moeilijk om op te splitsen resources binnen een abonnement, die tot het maken van een groot aantal abonnementen leiden.

Het implementatiemodel van Resource Manager bevat het concept van een [resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). Een resourcegroep is een container voor resources die een gemeenschappelijke levenscyclus delen. Alle resources voor de oplossing of alleen de resources die u wilt beheren als een groep kan bevatten. U bepaalt hoe resources worden toegewezen aan resourcegroepen op basis van wat voor uw organisatie het meest zinvol is.

Het implementatiemodel van Resource Manager biedt diverse voordelen:

- U kunt alle services voor uw oplossing als een groep implementeren, beheren en bewaken, in plaats van deze services afzonderlijk te verwerken.

- U kunt herhaaldelijk implementeren van uw oplossing gedurende de levenscyclus en vertrouwen dat uw resources worden geïmplementeerd in een consistente status hebben.

- U kunt toegangsbeheer toepassen op alle resources in uw resourcegroep. Deze beleidsregels worden automatisch toegepast wanneer nieuwe resources worden toegevoegd aan de resourcegroep.

- U kunt tags toepassen op de resources om alle resources in uw abonnement op een logische manier te organiseren.

- U kunt JSON (JavaScript Object Notation) gebruiken voor het definiëren van de infrastructuur voor uw oplossing. Het JSON-bestand is in feite de Resource Manager-sjabloon.

- U kunt de afhankelijkheden tussen resources, zodat deze zijn geïmplementeerd in de juiste volgorde definiëren.

![Resource Manager](./media/governance-in-azure/security-governance-in-azure-fig4.png)

Ga voor aanbevelingen over sjablonen naar [Aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

Azure Resource Manager analyseert afhankelijkheden om ervoor te zorgen dat resources in de juiste volgorde worden gemaakt. Als een resource, afhankelijk van een waarde uit een andere resource (zoals een virtuele machine die een opslagaccount nodig voor schijven), u [instellen van een afhankelijkheid](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies) in de sjabloon.

U kunt de sjabloon ook gebruiken voor updates aan de infrastructuur. U kunt bijvoorbeeld een resource toevoegen aan uw oplossing en configuratieregels toevoegen voor de resources die al zijn geïmplementeerd. Als de sjabloon, Hiermee geeft u het maken van een resource, maar die resource al bestaat, wordt in het Resource Manager een update in plaats van een nieuwe asset gemaakt uitgevoerd. Resource Manager werkt de bestaande asset naar dezelfde toestand als het nieuwe normaal zou zijn.

Resource Manager biedt uitbreidingen voor scenario's wanneer u meer bewerkingen, zoals het installeren van software die niet is opgenomen in de instelling nodig hebt.

### <a name="resource-tracking"></a>Bronnen bijhouden

Als gebruikers in uw organisatie een resource aan het abonnement toevoegt, wordt het belangrijker resources koppelen aan de juiste afdeling, de klant en de omgeving. U kunt metagegevens toevoegen aan resources via [tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags). U kunt tags gebruiken voor meer informatie over de resource of de eigenaar. Met tags kunt u niet alleen aggregeren en resources op verschillende manieren groeperen, maar ook gebruiken die gegevens voor terugstorting.

Gebruik tags wanneer u een verzameling complexe resourcegroepen en resources en u moeten voor het visualiseren van deze assets op de manier die het meest zinvol voor u. Bijvoorbeeld, kunt u toevoegen aan resources die een vergelijkbare rol bedienen in uw organisatie of die deel uitmaken van dezelfde afdeling.

Zonder tags kunnen kunnen gebruikers in uw organisatie maken van meerdere resources die mogelijk moeilijk te later identificeren en te beheren. Bijvoorbeeld, als u wilt verwijderen van alle resources voor een project. Als deze resources zijn niet gemarkeerd voor het project, moet u deze handmatig zoeken. Taggen kan een belangrijke manier zijn om onnodige kosten in uw abonnement te voorkomen.

Resources hoeft te bevinden zich in dezelfde resourcegroep voor het delen van een tag. U kunt uw eigen tagtaxonomie om ervoor te zorgen dat alle gebruikers in uw organisatie veelgebruikte tags in plaats van per ongeluk toepassen afwijkende tags (zoals 'Afd' in plaats van 'afdeling') gebruiken.

Resourcebeleid kunnen u maken van standaardregels voor uw organisatie. U kunt beleid om ervoor te zorgen dat resources worden gemarkeerd met de juiste waarden maken.

U kunt ook getagde resources via Azure Portal weergeven. De [gebruiksrapport](https://docs.microsoft.com/azure/billing/billing-understand-your-bill) voor uw abonnement tagnamen en -waarden bevat, dus u kunt lichten kosten op labels.

Zie voor meer informatie over tags [factureringstagbeleidinitiatief](../azure-policy/scripts/billing-tags-policy-init.md).

Voor tags gelden de volgende beperkingen:

- Elke resource of resourcegroep kan maximaal 15 tags sleutel/waarde-paren hebben. Deze beperking geldt alleen voor tags die direct worden toegepast op de resourcegroep of resource. Een resourcegroep kan veel bronnen die elk 15 tag sleutel/waarde-paren hebben bevatten.

- De tagnaam is beperkt tot 512 tekens.

- De tagwaarde is beperkt tot 256 tekens.

- Tags die zijn toegepast op de resourcegroep, worden niet overgenomen door de resources in deze resourcegroep.

Als u meer dan 15 waarden aan een resource moet koppelen, gebruikt u een JSON-tekenreeks voor de tagwaarde. De JSON-tekenreeks kan veel waarden die worden toegepast op een enkele tag-sleutel bevatten.

#### <a name="tags-for-billing"></a>Tags voor facturering

Met tags kunt u de groep van uw factureringsgegevens. Bijvoorbeeld, als u meerdere VM's voor verschillende organisaties uitvoert, gebruikt u tags aan het gebruik van de groep door kostenplaats. U kunt ook tags gebruiken voor het categoriseren van kosten door de runtimeomgeving, zoals het gebruik van de facturering voor virtuele machines die worden uitgevoerd in de productie-omgeving.

U kunt informatie over tags via ophalen de [gebruik van Azure-resources en RateCard APIs](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) of het gebruik door komma's gescheiden waarden (CSV)-bestand. Downloaden van het gebruiksbestand van de [portal van Azure-accounts](https://account.windowsazure.com/) of de [EA-portal](https://ea.azure.com/).

Zie voor meer informatie over programmatische toegang tot factureringsgegevens [inzicht in het gebruik van de Microsoft Azure-resources](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview). Zie voor REST API-bewerkingen, [Azure Billing REST API-verwijzing](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Wanneer u het gebruik van CSV voor services die ondersteuning bieden voor tags met facturering downloadt, wordt de tags in de kolom labels worden weergegeven.

### <a name="critical-resource-controls"></a>Besturingselementen voor kritieke bronnen

Als de kernservices van uw organisatie aan het abonnement toevoegt, wordt het belangrijker om ervoor te zorgen dat deze services zijn beschikbaar om te voorkomen dat zakelijke wordt onderbroken. [Resourcevergrendelingen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) kunt u beperken van bewerkingen op waardevolle bronnen waar wijzigt of verwijdert ze een aanzienlijke invloed op uw toepassingen of cloudinfrastructuur zou hebben. U kunt vergrendelingen toepassen op een abonnement, resourcegroep of resource. Normaal gesproken toepassen u vergrendelingen op de fundamentele resources zoals virtuele netwerken, gateways en storage-accounts.

Resourcevergrendelingen ondersteunt momenteel twee waarden: **CanNotDelete** en **ReadOnly**. **CanNotDelete** betekent dat gebruikers (met de juiste rechten) kunnen nog steeds lezen of wijzigen van een resource, maar deze niet verwijderen. **Alleen-lezen** betekent dat de gemachtigde gebruikers kan verwijderen of wijzigen van een resource.

Resourcevergrendelingen gelden alleen voor bewerkingen die in de beheerlaag, die uit bewerkingen die worden verzonden optreden bestaat naar <https://management.azure.com>. De vergrendelingen beperken niet hoe resources hun eigen functies uitvoeren. Wijzigingen van resources zijn beperkt, maar de bewerkingen van resources zijn niet beperkt. Bijvoorbeeld, een **ReadOnly** vergrendeling op een SQL-database wordt voorkomen dat u verwijderen of aanpassen van de database, maar voorkomt niet dat u uit het maken, bijwerken of verwijderen van gegevens in de database.

Toepassen van **ReadOnly** kan leiden tot onverwachte resultaten omdat bepaalde bewerkingen die kunnen worden gelezen bewerkingen extra acties vereist. Bijvoorbeeld, als u plaatst een **ReadOnly** vergrendeling op een storage-account wordt voorkomen dat alle gebruikers van de aanbieding van de sleutels. De bewerking van de aanbieding van sleutels wordt verwerkt door een POST-aanvraag, omdat de geretourneerde sleutels beschikbaar voor schrijfbewerkingen zijn.

![Besturingselementen voor kritieke bronnen](./media/governance-in-azure/security-governance-in-azure-fig5.png)

Voor een ander voorbeeld: als u plaatst een **alleen-lezen** vergrendeling op een Azure App Service-resource voorkomt dat Visual Studio Server Explorer weergeven van bestanden voor de resource omdat die interactie schrijftoegang is vereist.

In tegenstelling tot de op rollen gebaseerd toegangsbeheer gebruikt u beheervergrendelingen voor het toepassen van een beperking voor alle gebruikers en rollen. Zie voor meer informatie over het instellen van machtigingen, [toegang met RBAC en de Azure-portal beheren](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).

Wanneer u een vergrendeling op een overkoepelend bereik toepast, nemen alle resources binnen dat bereik de dezelfde vergrendeling. Zelfs resources die u later toevoegen overnemen de vergrendeling van het bovenliggende item. De meest beperkende vergrendeling in de overname voorrang.

Als u wilt maken of verwijderen van beheervergrendelingen, moet u toegang hebben tot Microsoft.Authorization/ of Microsoft.Authorization/locks/ acties. Van de ingebouwde rollen, worden alleen de eigenaar en beheerder van gebruikerstoegang personen deze acties verleend.

### <a name="api-access-to-billing-information"></a>API-toegang tot factureringsgegevens

Gebruik Azure facturering API's voor pull-gebruik en de resource-gegevens in uw favoriete hulpprogramma's voor gegevensanalyse. Het gebruik van Azure-resources en RateCard APIs kunt u nauwkeurig kunnen voorspellen en uw kosten beheren. De API's worden geïmplementeerd als een resourceprovider en een deel van de API's beschikbaar gemaakt door Azure Resource Manager-familie.

#### <a name="resource-usage-api-preview"></a>Resourcegebruik-API (Preview)

Gebruik de Azure [Resource Usage API](https://msdn.microsoft.com/library/azure/mt219003) om uw geschatte Azure-verbruik-gegevens. De API omvat:

- **RBAC**: configureren van beleid voor gegevenstoegang op de [Azure-portal](https://portal.azure.com/) of via [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) om op te geven welke gebruikers of toepassingen toegang tot gegevens over het gebruik van het abonnement krijgen kunnen. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de factureren voor lezer, lezer, eigenaar, Inzender rol of toegang krijgen tot gegevens over gebruik voor een specifieke Azure-abonnement.

- **Uurlijkse of dagelijkse aggregatie**: aanroepers kunnen aangeven of ze hun gebruik van Azure-gegevens in stappen van per uur of dagelijks. De standaardwaarde is dagelijks.

- **Metagegevens van het exemplaar (met inbegrip van resourcetags)**: ophalen van gegevens, zoals de volledig gekwalificeerde resource-URI op exemplaarniveau (/subscriptions/ {abonnement-id} /..), informatie over resource en resourcetags. Deze metagegevens kunt u via een programma heel deterministisch en gebruik toewijzen door de tags, voor de use cases als het cross-kosten in rekening gebracht.

- **De resource metagegevens**: details van de Resource, zoals de meternaam van de, metercategorie, subcategorie van de meter, eenheid en regio geven de aanroeper een beter begrip van wat is verbruikt. We werken ook om te worden uitgelijnd resource metagegevens terminologie voor Azure portal, Azure-gebruik CSV, EA facturering CSV en andere openbare-ervaringen, kunt u gegevens correleren met ervaringen.

- **Gebruik voor alle typen bieden**: gebruiksgegevens is beschikbaar voor alle typen, waaronder betalen per gebruik, MSDN, monetaire toezegging, financieel tegoed en EA bieden.

#### <a name="resource-ratecard-api"></a>Resource RateCard API

Gebruik RateCard API van Azure Resource om de lijst met beschikbare Azure-resources en de geschatte informatie over de prijzen voor elk. De API omvat:

- **RBAC**: uw toegangsbeleid configureren in Azure portal of via Azure PowerShell-cmdlets om op te geven welke gebruikers of toepassingen toegang tot de RateCard-gegevens krijgen kunnen. Aanroepers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De oproepende functie toevoegen aan de rol Lezer, de eigenaar of Inzender voor toegang tot gegevens over gebruik voor een bepaald Azure-abonnement.

- **Ondersteuning voor betalen per gebruik, MSDN, monetaire toezegging en financieel tegoed aanbiedingen (maar niet EA)**: deze API biedt Azure-aanbieding op serverniveau snelheid informatie. De aanroeper van deze API moet doorgeven in de informatie over de aanbieding om details van de resource en tarieven te verkrijgen. EA is momenteel niet ondersteund omdat EA-aanbiedingen tarieven per inschrijving hebt aangepast. 

#### <a name="scenarios"></a>Scenario's

De combinatie van de gebruiks- en RateCard APIs maakt deze scenario's mogelijk:

- **Inzicht in Azure gedurende de maand besteden**: Gebruik de combinatie van het gebruik en RateCard APIs beter inzicht krijgt in uw cloud uitgaven gedurende de maand. U kunt elk uur en dagelijkse gebruik en kosten schattingen analyseren.

- **Waarschuwingen instellen**: Gebruik de gebruiks- en RateCard APIs geschatte cloudverbruik en kosten in rekening gebracht en waarschuwingen op basis van een resource of monetaire op basis van instellen.

- **Factuur voorspellen**: Get uw geschatte verbruik en de cloud besteden en machine learning-algoritmen om te voorspellen wat de factuur is aan het einde van de factureringscyclus van toepassing.

- **Uitvoeren van de analyse van een vooraf verbruikskosten**: de RateCard API gebruiken om te voorspellen hoeveel uw factuur zou zijn voor uw verwachte gebruik wanneer u uw werkbelastingen naar Azure verplaatsen. Als u bestaande workloads in andere clouds en privéclouds hebt, kunt u ook uw gebruik met de Azure toewijzen tarieven voor een betere schatting van de Azure-uitgaven. Deze schatting biedt u de mogelijkheid om te draaien op een aanbieding en vergelijken tussen de verschillende typen dan betalen per gebruik, zoals monetaire toezegging en financieel tegoed.

- **Een what-if-analyse uitvoeren**: U kunt bepalen of deze rendabeler voor het uitvoeren van workloads in een andere regio of op een andere configuratie van de Azure-resource is. Kosten voor Azure-resource verschillen op basis van de Azure-regio u. U kunt ook bepalen als een ander Azure-aanbiedingtype resulteert in een betere rentabiliteit op een Azure-resource.

### <a name="networking-controls"></a>Besturingselementen voor netwerken

Toegang tot resources kan worden intern (binnen het bedrijfsnetwerk) of extern (via het internet). Het is eenvoudig voor gebruikers in uw organisatie plaats per ongeluk resources in de verkeerde plaats en opent u deze mogelijk schadelijke toegang krijgen tot. Net als bij on-premises apparaten, moeten ondernemingen juiste controles om ervoor te zorgen dat gebruikers van Azure de juiste beslissingen toevoegen.

![Besturingselementen voor netwerken](./media/governance-in-azure/security-governance-in-azure-fig6.png)

Voor abonnementsgovernance bieden de volgende kernbronnen basic controle van toegang.

#### <a name="network-connectivity"></a>Verbinding met het netwerk

[Virtuele netwerken](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) zijn containerobjecten voor subnetten. Hoewel niet strikt noodzakelijk is, wordt een virtueel netwerk wordt vaak gebruikt voor het verbinden van toepassingen tot interne bedrijfsbronnen. De service Azure Virtual Network kunt u Azure-resources veilig met elkaar verbinden met virtuele netwerken.

Een virtueel netwerk is een weergave van uw eigen netwerk in de cloud. Een virtueel netwerk is een logische isolatie van de Azure-cloud toegewezen aan uw abonnement. U kunt ook virtuele netwerken verbinden met uw on-premises netwerk.

Hieronder vindt u mogelijkheden voor virtuele Azure-netwerken:

- **Isolatie**: virtuele netwerken zijn geïsoleerd van elkaar. U kunt afzonderlijke virtuele netwerken voor ontwikkeling, testen en productie die gebruikmaken van dezelfde CIDR-adresblokken maken. Daarentegen, kunt u meerdere virtuele netwerken die gebruikmaken van verschillende CIDR-adresblokken en netwerken met elkaar verbinden. U kunt een virtueel netwerk segmenteren in meerdere subnetten. Azure biedt interne naamomzetting voor virtuele machines en Azure Cloud Services-rolexemplaren die zijn verbonden met een virtueel netwerk. U kunt eventueel een virtueel netwerk voor het gebruik van uw eigen DNS-servers, in plaats van Azure interne naamomzetting configureren.

- **Verbinding met Internet**: alle Azure virtual machines en rolinstanties van Cloudservices die zijn verbonden met een virtueel netwerk hebben toegang tot het internet, standaard. U kunt ook binnenkomende toegang tot bepaalde resources inschakelen indien nodig.

- **Azure-resource connectiviteit**: U kunt Azure-resources, zoals Cloudservices en virtuele machines, verbinding maken met hetzelfde virtuele netwerk. De resources kunnen met elkaar verbinden via IP-adressen, zelfs als ze zich in verschillende subnetten. Azure biedt standaardroutering tussen subnetten, virtuele netwerken en on-premises netwerken, zodat u niet hoeven te configureren en beheren van routes.

- **Verbinding met het virtuele netwerk**: U kunt virtuele netwerken met elkaar verbinden. Resources die zijn verbonden met een virtueel netwerk kunnen vervolgens communiceren met een resource in een ander virtueel netwerk.

- **On-premises connectiviteit**: U kunt virtuele netwerken verbinden met on-premises netwerken via VPN-verbindingen tussen uw netwerk en Azure, of de verbinding van een site-naar-site virtueel particulier netwerk (VPN) via internet.

- **Verkeer filteren**: U kunt netwerkverkeer filteren (binnenkomend en uitgaand) voor virtuele machines en Cloudservices op bron-IP-adres en poort, doel-IP-adres en poort en protocol.

- **Routering**: U kunt eventueel standaard Azure routering door uw eigen routes te configureren, of met behulp van BGP-routes via een netwerkgateway overschrijven.

#### <a name="network-access-controls"></a>Netwerktoegangsbeheer

[Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (nsg's) zijn, zoals een firewall, en geef regels op voor hoe een resource "" via het netwerk praten kan. Beschikt over de controle over hoe een subnet (of een virtuele machine) met het internet of een andere subnetten in hetzelfde virtuele netwerk verbinden kan.

Een netwerkbeveiligingsgroep bevat een lijst met regels voor toestaan of weigeren van netwerkverkeer tot resources die zijn verbonden met virtuele netwerken van Azure. Nsg's kunnen worden gekoppeld aan subnetten, afzonderlijke virtuele machines (klassiek) of afzonderlijke netwerkinterfaces (NIC's) die zijn gekoppeld aan virtuele machines (Resource Manager).

Als een NSG gekoppeld aan een subnet is, zijn de regels van toepassing op alle resources die zijn verbonden met het subnet. U kunt verkeer verder beperken door een NSG koppelen aan een VM of NIC.

## <a name="security-and-compliance-with-organizational-standards"></a>Beveiliging en naleving van standaarden van de organisatie

Elk bedrijf heeft verschillende behoeften en duidelijke voordelen van cloudoplossingen wordt vruchten. Klanten van alle soorten hebben nog steeds de dezelfde basic bezorgdheid over het verplaatsen naar de cloud. Klanten willen van cloudproviders is:

- **Beveiliging van onze gegevens**: IT-professionals erkent dat de cloud verbeterde gegevensbeveiliging en -beheer biedt. Maar ze zijn nog steeds betrokken dat migreren naar de cloud ze gevoeliger voor hackers dan hun huidige interne oplossingen laat.

- **Onze gegevens privé houden**: cloudservices verhogen unieke privacy-uitdaging vormen. Bedrijven zoeken naar de cloud en Bespaar op infrastructuurkosten hun flexibiliteit verbeteren, worden ze ook zorgen over het beheer van waar hun gegevens worden opgeslagen en hoe deze wordt gebruikt die toegang heeft tot het verlies.

- **Geef ons besturingselement**:, zelfs als bedrijven van de cloud meer innovatieve oplossingen implementeren profiteren, ze zich zorgen maakt over het beheer van hun gegevens verliezen. De recente openbaarmakingen van government-die toegang hebben tot klantgegevens, juridische en buitengerechtelijke manier, moeten u sommige CIO's altijd huiverig tegenover slaan hun gegevens in de cloud staan.

- **Transparantie verhogen**: zakelijke besluitvormers begrijpen het belang van beveiliging, privacy en controle. Maar ze ook wilt kunnen onafhankelijk van elkaar controleren hoe hun gegevens worden opgeslagen, geopend en beveiligd.

- **Zorgen voor naleving**: als bedrijven hun gebruik van cloud-technologieën wilt uitbreiden, de complexiteit en het bereik van normen en reglementen blijven zich ontwikkelen. Bedrijven moeten weten dat hun standaarden voor compliance wordt voldaan.

## <a name="security-configuration-for-monitoring-logging-and-auditing"></a>Beveiligingsconfiguratie voor bewaking, logboekregistratie en controle

Azure-abonnees kunnen hun cloudomgevingen beheren vanaf meerdere apparaten. Deze apparaten advies inwinnen bij beheerwerkstations, pc's van ontwikkelaars en apparaten, zelfs bevoegde eindgebruikers met taakspecifieke rechten. 

In sommige gevallen worden beheerfuncties uitgevoerd via het web gebaseerde consoles, zoals de Azure-portal. In andere gevallen wordt er mogelijk rechtstreekse verbindingen naar Azure vanaf on-premises systemen via VPN-verbindingen, Terminal Services, protocollen van clienttoepassingen of (programmatisch) de Azure Service Management API (SMAPI). Clienteindpunten kunnen bovendien zowel in een domein gekoppeld of en niet-beheerd, zoals tablets of smartphones zijn.

Deze verscheidenheid kan aanzienlijke risico's toevoegen aan een cloudimplementatie. Kan het moeilijk zijn om te beheren, volgen en controleren van beheeracties. Deze verscheidenheid kan ook leiden tot beveiligingsrisico's ongereglementeerde toegang plaatsvindt tot clienteindpunten die worden gebruikt voor het beheren van cloudservices. Het gebruik van algemene of persoonlijke werkstations voor het ontwikkelen en beheren van infrastructuur brengt onvoorspelbare bedreigingen met zich mee voor bijvoorbeeld surfen op internet (denk aan waterhole-aanvallen) of e-mail (zoals social engineering en phishing).

Bewaking, logboekregistratie en controle bieden een basis voor het bijhouden en begrijpen van beheeractiviteiten. Alle acties nauwgezet controle mogelijk niet altijd haalbaar vanwege de grote hoeveelheid gegevens die zijn gegenereerd. Maar de effectiviteit van de management-beleidsregels controle is een aanbevolen procedure.

Azure-beveiliging-beheer van groepsbeleidsobjecten van Azure Active Directory Domain Services (AD DS) kunt u bepalen van alle de beheerders Windows-interfaces, zoals het delen van bestanden. Neem beheerwerkstations in bewaking, logboekregistratie en controle van processen. Houd toegang en gebruik van alle beheerders en ontwikkelaars bij.

### <a name="azure-security-center"></a>Azure Security Center

[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) biedt een centraal overzicht van de beveiligingsstatus van resources in abonnementen. Het biedt aanbevelingen die helpen te voorkomen dat verdachte resources. Deze kunt meer gedetailleerde beleidsregels--bijvoorbeeld beleidsregels toepassen op specifieke resourcegroepen waarmee bedrijven hun houding het risico dat ze bent adressering op maat kunt inschakelen.

![Azure Security Center](./media/governance-in-azure/security-governance-in-azure-fig7.png)

Security Center biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen die anders onopgemerkt, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen te detecteren. Nadat u hebt ingeschakeld [beveiligingsbeleid](https://docs.microsoft.com/azure/security-center/security-center-policies) voor resources van een abonnement, analyseert Security Center de beveiliging van uw resources om mogelijke beveiligingsproblemen te identificeren. Informatie over uw netwerkconfiguratie is onmiddellijk beschikbaar.

Azure Security Center is een combinatie van best practices analyse en beveiliging beleid-projectbeheer voor alle resources in een Azure-abonnement. Hiermee kunnen beveiligingsteams en officieren van risico's te voorkomen, detecteren en direct reageren op bedreigingen, zoals het automatisch verzamelt en beveiligingsgegevens van uw Azure-resources, het netwerk en partneroplossingen zoals antimalwareprogramma's en firewalls analyseert.

Azure Security Center geldt bovendien geavanceerde analyses, waaronder machine learning en gedragsanalyse. Het maakt gebruik van wereldwijde dreigingsinformatie uit Microsoft-producten en services, de Microsoft Digital Crimes Unit (DCU), de Microsoft Security Response Center (MSRC) en externe feeds. U kunt toepassen [security governance](https://www.credera.com/blog/credera-site/azure-governance-part-4-other-tools-in-the-toolbox/) ruim op het abonnementsniveau. Of u kunt dit beperken tot specifieke vereisten en deze toepassen op afzonderlijke resources via de beleidsdefinitie.

Ten slotte Azure Security Center analyseert de beveiligingsstatus van de resource op basis van deze beleidsregels en gebruikt deze gegevens inzichtelijke dashboards en waarschuwingen voor gebeurtenissen, zoals de detectie van malware of schadelijke IP-verbinding probeert te geven. Zie voor meer informatie over het toepassen van aanbevelingen [beveiligingsaanbevelingen implementeren in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

De volgende Azure-resources worden bewaakt door Azure Security Center:

- Virtuele machines (VM's) (met inbegrip van cloudservices)

- Virtuele netwerken

- SQL-databases

- Partneroplossingen die zijn geïntegreerd met uw Azure-abonnement, zoals een firewall voor webtoepassingen op virtuele machines en klik op de [App Service-omgeving](https://docs.microsoft.com/azure/app-service/app-service-app-service-environments-readme)

Wanneer u Security Center voor het eerst opent, wordt gegevensverzameling ingeschakeld op alle virtuele machines in uw abonnement. We raden aan dat u gegevensverzameling zijn ingeschakeld, maar u kunt [uitschakelen](https://docs.microsoft.com/azure/security-center/security-center-faq) in het Security Center-beleid.

### <a name="log-analytics"></a>Log Analytics

Azure Log Analytics-software ontwikkel- en service van het team voor informatiebeveiliging en [governance programma](https://github.com/Microsoft/azure-docs/blob/master/articles/log-analytics/log-analytics-security.md) biedt ondersteuning voor de zakelijke vereisten. Voldoet aan de wetten en voorschriften zoals beschreven op [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) en [Microsoft Trust Center naleving](https://www.microsoft.com/TrustCenter/Compliance/default.aspx). Hoe Log Analytics wordt tot stand gebracht beveiligingsvereisten, identificeert beveiligingsmaatregelen en beheert en monitors risico's wordt er ook beschreven. Jaarlijks, controleert het team beleidsregels, standaarden, procedures en richtlijnen.

Elk teamlid voor Log Analytics-ontwikkeling ontvangt beveiligingstraining formele aanvraag. Een versiebeheersysteem beschermt elk softwareproject in ontwikkeling.

Microsoft heeft een beveiliging en naleving team dat verantwoordelijk is voor en alle services in Microsoft beoordeelt. Information security officers maken van het team en ze zijn niet gekoppeld aan de technische afdelingen die Log Analytics te ontwikkelen. De security officers hebben hun eigen Managementketen. Ze voeren onafhankelijke evaluaties van producten en services om te zorgen voor beveiliging en naleving.

De kernfunctionaliteit van Log Analytics wordt geboden door een set met services die worden uitgevoerd in Azure. Elke service biedt een specifieke beheerfunctie. U kunt services combineren om verschillende beheerscenario's te bewerkstelligen.

![Azure-services voor beheer](./media/governance-in-azure/security-governance-in-azure-fig9.JPG)

Deze management-services zijn ontworpen in de cloud. Ze zijn volledig gehost in Azure, zodat ze hebben geen betrekking op implementeren en beheren van on-premises bronnen. Configuratie is minimaal en u kunt actief en werkend zijn binnen een paar minuten.

Een agent op een Windows- of Linux-computer in uw datacenter te plaatsen en verzendt deze gegevens naar Log Analytics. Er is, kunnen ze worden geanalyseerd samen met alle overige gegevens die worden verzameld van de cloud of on-premises services. Azure Backup en Azure Site Recovery gebruiken om te profiteren van de cloud voor back-up en hoge beschikbaarheid voor on-premises bronnen.

![Management-services op het Azure-dashboard](./media/governance-in-azure/security-governance-in-azure-fig8.png)

Runbooks in de cloud geen doorgaans toegang tot uw on-premises resources, maar u kunt een agent installeren op een of meer computers die runbooks in uw datacenter hosten vervolgens. Wanneer u een runbook start, kunt u opgeven of u wilt dat deze wordt uitgevoerd in de cloud of op een lokale werkrol.

Er zijn verschillende oplossingen beschikbaar van Microsoft en partners die u aan uw Azure-abonnement toevoegen kunt te verhogen van de waarde van uw investering in Log Analytics. Bijvoorbeeld, Azure biedt [beheeroplossingen](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions)--voorverpakt sets met logica die een scenario voor het beheer implementeren met behulp van een of meer management-services.

![Galerie van oplossingen in Azure](./media/governance-in-azure/security-governance-in-azure-fig10.png)

Als partner kunt u uw eigen oplossingen voor het ondersteunen van uw toepassingen en services en ze aanbieden aan gebruikers via de Azure Marketplace of Quickstart-sjablonen maken.

## <a name="performance-alerting-and-monitoring"></a>Van prestatiewaarschuwingen en bewaking

### <a name="alerting"></a>Waarschuwingen

Waarschuwingen zijn een methode voor bewaking metrische gegevens voor Azure-resource, evenementen of Logboeken. Ze een melding wanneer wordt voldaan aan een voorwaarde die u hebt opgegeven.

Waarschuwingen zijn beschikbaar voor de services, waaronder:

- **Azure Application Insights**: kunnen web-test- en metrische waarschuwingen. Zie voor meer informatie, [meldingen instellen in Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) en [beschikbaarheid en reactiesnelheid van een website bewaken](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability).

- **Log Analytics**: de routering van activiteiten en diagnostische logboeken naar Log Analytics is ingeschakeld. Hierdoor kan metrische gegevens, logboeken en andere typen waarschuwingen. Zie voor meer informatie, [waarschuwingen in Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts).

- **Azure Monitor**: Hiermee kunt u waarschuwingen op basis van metrische waarden en gebeurtenissen in het activiteitenlogboek. U kunt de [Azure Monitor REST API](https://msdn.microsoft.com/library/dn931943.aspx) om waarschuwingen te beheren. Zie voor meer informatie, [met behulp van de Azure-portal, PowerShell of de opdrachtregelinterface om waarschuwingen te maken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal).

### <a name="monitoring"></a>Bewaking

Prestatieproblemen in uw cloud-app kunnen invloed hebben op uw bedrijf. Degradations kan met meerdere onderdelen onderling verbonden en regelmatige versies, op elk gewenst moment gebeuren. En als u een app ontwikkelt, uw gebruikers gewoonlijk detecteren van problemen die u niet hebt gevonden in de testfase. U moet weten over deze problemen onmiddellijk en hulpprogramma's voor het opsporen en corrigeren.

Er is een scala aan hulpprogramma's voor het bewaken van Azure-toepassingen en services. Enkele van hun functies overlappen. Dit komt deels door de vervaging tussen ontwikkel- en werking van een toepassing.

Hier volgen de belangrijkste hulpprogramma's:

- **Azure Monitor** is een eenvoudige hulpprogramma voor het bewaken van services die worden uitgevoerd op Azure. Dit biedt u de gegevens op infrastructuurniveau over de doorvoer van een service en de omringende omgeving. Als u al uw apps in Azure en bepalen of schaal omhoog of omlaag resources beheert, kunt Azure Monitor u beginnen.

- **Application Insights** kan worden gebruikt voor ontwikkeling en als een productie-oplossing voor de controle. Dit gebeurt door het installeren van een pakket in uw app, zodat het biedt u een meer interne weergave van wat er gebeurt. De gegevens omvatten de reactietijden van afhankelijkheden, traceringen van de uitzondering, foutopsporing, momentopnamen en -profielen kan worden uitgevoerd. Het biedt hulpprogramma's voor het analyseren van alle deze telemetrie om op te sporen van een app zowel om te begrijpen wat gebruikers doen met deze. U kunt zien of een piek in de reactietijden veroorzaakt door iets in een app of een probleem opgetreden tijdens externe resourcing wordt. Als u Visual Studio en de app is beschadigd, wordt u rechtstreeks naar de coderegel probleem gaat, zodat u deze kunt oplossen.

- **Log Analytics** is bedoeld voor degenen die moet afstemmen van prestaties en onderhoud plannen op toepassingen die worden uitgevoerd in de productieomgeving. Het verzamelt en verzamelt gegevens uit verschillende bronnen, met een vertraging van 10 tot 15 minuten. Het biedt een holistische oplossing voor IT-beheer voor Azure, on-premises en cloudgebaseerde infrastructuur hebben van derden (zoals Amazon Web Services). Het biedt hulpprogramma's voor het analyseren van gegevens in bronnen, kunnen complexe query's over alle logboeken en kunt proactief waarschuwen van de opgegeven voorwaarden. U kunt zelfs aangepaste gegevens in de centrale opslagplaats, verzamelen en vervolgens query's uitvoeren en die gegevens visualiseren.

- **System Center Operations Manager** is voor het beheren en controleren van grote cloud-installaties. Kunt u mogelijk al bekend bent met het als beheerprogramma voor on-premises Windows Server en Hyper-V op basis van clouds, maar ook kunnen integreren en beheren van apps in Azure. Onder andere Installeer het Application Insights op bestaande live apps. Als een app uitgeschakeld wordt, wordt u door Operations Manager in seconden aan uitgelegd.


## <a name="next-steps"></a>Volgende stappen

- [Best practices voor het maken van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices)

- [Voorbeelden van de implementatie van Azure-abonnementsgovernance](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-examples)

- [Microsoft Azure Government](https://docs.microsoft.com/azure/azure-government/)
