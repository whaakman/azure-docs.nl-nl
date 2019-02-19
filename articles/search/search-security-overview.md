---
title: Beveiliging en privacy - Azure Search
description: Azure Search is compatibel met het SOC 2, HIPAA en andere certificeringen. Verbinding met en gegevens voor versleuteling, verificatie en identiteit toegang door middel van gebruiker en groep beveiligings-id's in de filters voor Azure Search.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 02/18/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: c0f824e2be0215192ca4ca1a722e814cbf299b7a
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342419"
---
# <a name="security-and-data-privacy-in-azure-search"></a>Beveiliging en privacy in Azure Search

Uitgebreide beveiligingsfuncties en -toegangsbeheer zijn ingebouwd in Azure Search om te controleren of persoonlijke inhoud op die manier blijft. In dit artikel worden de functies en normen voor naleving van beveiliging die is ingebouwd in Azure Search.

Azure Search-beveiligingsarchitectuur omvat fysieke beveiliging, versleutelde overdrachten van versleutelde opslag en naleving van standaarden voor brede, door het platform. Azure Search accepteert operationeel, alleen geverifieerde aanvragen. U kunt eventueel toegangsbeheer per gebruiker op inhoud toevoegen via beveiligingsfilters. In dit artikel is van toepassing op de beveiliging op elke laag, maar is voornamelijk gericht op hoe gegevens en bewerkingen in Azure Search worden beveiligd.

## <a name="standards-compliance-iso-27001-soc-2-hipaa"></a>Naleving van standaarden: ISO 27001, SOC 2, HIPAA

Azure Search is gecertificeerd voor de volgende standaarden als [aangekondigd in juni 2018](https://azure.microsoft.com/blog/azure-search-is-now-certified-for-several-levels-of-compliance/):

+ [ISO 27001:2013](https://www.iso.org/isoiec-27001-information-security.html) 
+ [SOC 2 Type 2-naleving](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) voor het volledige rapport, gaat u naar [Azure- en Azure Government SOC 2 Type II rapport](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports). 
+ [Health Insurance Portability and Accountability Act (HIPAA)](https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act)
+ [GxP (21 CFR Part 11)](https://en.wikipedia.org/wiki/Title_21_CFR_Part_11)
+ [HITRUST](https://en.wikipedia.org/wiki/HITRUST)
+ [PCI DSS Level 1](https://en.wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard)
+ [Australië IRAP voor niet-geclassificeerde DLM](https://asd.gov.au/infosec/irap/certified_clouds.htm)

Naleving van standaarden geldt voor de algemeen beschikbare functies. Preview-functies zijn gecertificeerd wanneer ze worden overgezet naar algemene beschikbaarheid, en mogen niet worden gebruikt in oplossingen met strenge normen vereisten. Nalevingscertificering wordt gedocumenteerd in [overzicht van Microsoft Azure-naleving](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) en de [Trust Center](https://www.microsoft.com/en-us/trustcenter). 

## <a name="encrypted-transmission-and-storage"></a>Versleutelde overdracht en opslag

Versleuteling is een uitbreiding gedurende de hele pijplijn voor indexering: van verbindingen, door middel van overdracht, en geïndexeerde gegevens die zijn opgeslagen in Azure Search.

| Beveiligingslaag | Description |
|----------------|-------------|
| Versleuteling tijdens overdracht <br>(HTTPS/SSL/TLS) | Azure Search luistert naar HTTPS-poort 443. Verbindingen met Azure-services zijn op het platform versleuteld. <br/><br/>Alle client-naar-service Azure Search interacties zijn SSL/TLS 1.2 die geschikt.  Zorg dat u ondersteuning voor TLSv1.2 gebruiken voor SSL-verbindingen met uw service.|
| Versleuteling 'at rest' | Versleuteling is volledig internalized in het indexeringsproces worden geautomatiseerd, met geen merkbare impact heeft op de indexering van tijd tot voltooiing of de indexgrootte. Dit gebeurt automatisch op alle indexeren, met inbegrip van incrementele updates naar een index die niet volledig is versleuteld (die zijn gemaakt vóór januari 2018).<br><br>Intern, versleuteling is gebaseerd op [Azure Storage-Serviceversleuteling](https://docs.microsoft.com/azure/storage/common/storage-service-encryption), met behulp van 256-bits [AES-versleuteling](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard).|

Versleuteling is intern voor Azure Search met certificaten en versleutelingssleutels intern beheerd door Microsoft en universeel toegepast. U kan geen versleuteling inschakelen of uitschakelen, beheren of vervangen door uw eigen sleutels of weergeven van instellingen voor codering in de portal of via een programma. 

Versleuteling-at-rest in januari 24 mei 2018 is aangekondigd en is van toepassing op alle service-lagen, inclusief gedeelde (gratis) services, in alle regio's. Voor volledige versleuteling moeten indexen die zijn gemaakt vóór die datum worden verwijderd en opnieuw worden opgebouwd voor versleuteling moet plaatsvinden. Anders worden alleen nieuwe gegevens die zijn toegevoegd na januari 24 is versleuteld.

## <a name="azure-wide-user-access-controls"></a>Toegangsbeheer voor brede, door het Azure-gebruiker

Verschillende beveiligingsmechanismen beschikbaar Azure hele zijn en dus automatisch beschikbaar voor de Azure Search-resources die u maakt.

+ [De vergrendelingen bij het abonnement of de resource om te voorkomen dat wordt verwijderd](../azure-resource-manager/resource-group-lock-resources.md)
+ [Op rollen gebaseerd toegangsbeheer (RBAC) om toegang tot de gegevens en beheerbewerkingen te beheren](../role-based-access-control/overview.md)

Alle Azure-services ondersteunen op rollen gebaseerd toegangsbeheer (RBAC) voor het instellen van de toegangsniveaus consistent op alle services. Bijvoorbeeld is weergeven van gevoelige gegevens, zoals de Administrator-code beperkt tot de rollen eigenaar en Inzender, terwijl voor het weergeven van de status van service beschikbaar voor leden van een rol is. RBAC biedt rollen eigenaar, Inzender en lezer. Standaard zijn alle servicebeheerders leden van de rol van eigenaar.

## <a name="service-access-and-authentication"></a>Toegang tot de service en -verificatie

Terwijl Azure Search de beveiligingsmaatregelen van het Azure-platform neemt, biedt het ook een eigen verificatie op basis van een sleutel. Een api-sleutel is een tekenreeks die bestaat uit een willekeurig gegenereerde cijfers en letters bevatten. Het type sleutel (admin of query) bepaalt het niveau van toegang. Indienen van een geldige sleutel wordt beschouwd als bewijs van de aanvraag afkomstig is van een vertrouwde entiteit. 

Er zijn twee niveaus van toegang tot uw search-service ingeschakeld door twee typen sleutels:

* Beheerderstoegang (geldt voor elke bewerking die lezen / schrijven op basis van de service)
* Query-toegang (geldig voor alleen-lezen bewerkingen, zoals query's op een index)

*Beheersleutels* worden gemaakt wanneer de service is geïmplementeerd. Er zijn twee beheersleutels aangewezen als *primaire* en *secundaire* dat ze rechtstreeks, maar in feite ze zijn verwisselbaar. Elke service heeft twee beheersleutels zodat u een afwisselen kunt zonder te verliezen toegang tot uw service. Kunt u een van beide administratorsleutel opnieuw genereren, maar u kunt toevoegen aan het aantal totale beheerder. Er is een maximum van twee beheersleutels per search-service.

*Querysleutels* zo nodig worden gemaakt en zijn ontworpen voor clienttoepassingen die rechtstreeks aanroepen van de zoekopdracht. U kunt maximaal 50 querysleutels maken. In de toepassingscode geeft u de URL zoeken en een query-api-sleutel voor alleen-lezen toegang tot de service. Code van uw toepassing geeft ook de index die wordt gebruikt door uw toepassing. Het eindpunt een api-sleutel voor alleen-lezentoegang en een doelindex definieert samen het bereik en het toegangsniveau van de verbinding van uw clienttoepassing.

Verificatie is vereist voor elke aanvraag, waarbij elke aanvraag is samengesteld uit een verplichte sleutel, een bewerking, en een object. Bij het aan elkaar zijn gekoppeld, zijn de twee machtigingsniveaus (volledig of alleen-lezen) plus de context (bijvoorbeeld een querybewerking op een index) voldoende zijn voor het ontwikkelen van zeer uitgebreide beveiliging voor servicebewerkingen. Zie voor meer informatie over sleutels, [maken en beheren van api-sleutels](search-security-api-keys.md).

## <a name="index-access"></a>Index toegang

In Azure Search is een afzonderlijke index niet een beveiligbaar object. In plaats daarvan wordt toegang tot een index op de servicelaag (lezen of schrijven toegang), samen met de context van een bewerking bepaald.

Voor toegang door eindgebruikers, kunt u structureren queryaanvragen verbinding maken met behulp van een querysleutel, die een aanvraag indient, alleen-lezen en de specifieke index die wordt gebruikt door uw app bevatten. In de queryaanvraag van een is er geen concept van het toevoegen van indexen of tegelijkertijd meerdere indexen te openen, zodat alle aanvragen één index doel per definitie. Als zodanig definieert constructie van de queryaanvraag zelf (een sleutel plus een index met één doel) de beveiligingsgrens.

Beheerders en ontwikkelaars toegang tot indexen is niet-gedifferentieerde: moet schrijftoegang tot het maken, verwijderen en bijwerken van objecten die worden beheerd door de service. Iedereen met een Administrator-code naar uw service kan lezen, wijzigen of verwijderen van een index in dezelfde service. Voor de bescherming tegen onbedoelde of schadelijke verwijderen van indexen is uw interne bronbeheer voor code-elementen de genoegdoening voor omgekeerd of een ongewenste index verwijderd of gewijzigd. Azure Search heeft failover binnen het cluster om beschikbaarheid te garanderen, maar deze niet opslaan of uw eigen code die wordt gebruikt om te maken of laden van indexen worden uitgevoerd.

Voor multitenancy-oplossingen vereisen beveiligingsgrenzen op het indexniveau van de, bevatten dergelijke oplossingen doorgaans een middelste laag, die klanten gebruiken voor het afhandelen van index isolatie. Zie voor meer informatie over de multitenant-use-case [ontwerppatronen voor multitenant SaaS-toepassingen en Azure Search](search-modeling-multitenant-saas-applications.md).

## <a name="admin-access-from-client-apps"></a>Beheerderstoegang van client-apps

De Azure Search Management REST API is een uitbreiding van de Azure Resource Manager en de afhankelijkheden ervan deelt. Als zodanig is Active Directory een vereiste voor het beheer van de service van Azure Search. Alle Beheeraanvragen vanuit clientcode moeten worden geverifieerd met behulp van Azure Active Directory voordat de aanvraag heeft bereikt van het Resource Manager.

Aanvragen voor gegevens voor het eindpunt van een Azure Search-service, zoals Index maken (Azure Search Service REST API) of documenten zoeken (Azure Search Service REST API), een api-sleutel in de aanvraagheader gebruiken.

Als de code van uw toepassing verwerkt servicebewerkingen voor beheer, evenals gegevensbewerkingen op zoekindexen of documenten, twee manieren van verificatie in uw code implementeren: de toegangssleutel van systeemeigen naar Azure Search en de Active Directory-verificatie methodologie die door Resource Manager. 

Zie voor meer informatie over het structureren van een aanvraag in Azure Search [Azure Search Service REST](https://docs.microsoft.com/rest/api/searchservice/). Zie voor meer informatie over de verificatievereisten voor Resource Manager, [gebruik Resource Manager verificatie-API voor toegang tot abonnementen](../azure-resource-manager/resource-manager-api-authentication.md).

## <a name="user-access-to-index-content"></a>Gebruikerstoegang tot inhoud indexeren

Standaard wordt toegang tot een index voor gebruikers bepaald door de toegangssleutel voor de queryaanvraag. De meeste ontwikkelaars maken en toewijzen van [ *querysleutels* ](search-security-api-keys.md) voor client-side zoekaanvragen. Een querysleutel verleent leestoegang tot alle inhoud in de index.

Als u gedetailleerde vereist, per gebruiker controle over inhoud, u kunt beveiligingsfilters voortbouwen op uw query's, documenten die zijn gekoppeld aan een bepaalde beveiligings-id geretourneerd. In plaats van vooraf gedefinieerde rollen roltoewijzingen, op basis van de identiteit wordt geïmplementeerd als een *filter* dat Trim van documenten en inhoud zoekresultaten op basis van identiteiten. De volgende tabel beschrijft de twee benaderingen voor trimming zoekresultaten van niet-geautoriseerde inhoud.

| Methode | Description |
|----------|-------------|
|[Security trimming wordt geregeld op basis van id-filters](search-security-trimming-for-azure-search.md)  | De algemene werkstroom voor het implementeren van toegangsbeheer voor gebruiker identiteit-documenten. Het bevat informatie over toe te voegen beveiligings-id's naar een index en vervolgens wordt uitgelegd filteren op basis van de resultaten van niet-toegestane inhoud trim voor dat veld. |
|[Security trimming wordt geregeld op basis van Azure Active Directory-identiteiten](search-security-trimming-for-azure-search-with-aad.md)  | In dit artikel gaat verder in op het vorige artikel, bieden stappen voor het ophalen van identiteiten van Azure Active Directory (AAD), een van de [gratis services](https://azure.microsoft.com/free/) in het Azure-cloud-platform. |

## <a name="table-permissioned-operations"></a>Tabel: Permissioned bewerkingen

De volgende tabel geeft een overzicht van de bewerkingen die zijn toegestaan in Azure Search en welke sleutel wordt ontgrendeld toegang tot een bepaalde bewerking.

| Bewerking | Machtigingen |
|-----------|-------------------------|
| Een service maken | De houder van de Azure-abonnement|
| Een service schaalt | Administrator-code, RBAC-eigenaar of bijdrager van de resource  |
| Een service verwijderen | Administrator-code, RBAC-eigenaar of bijdrager van de resource |
| Maken, wijzigen en verwijderen van objecten op de service: <br>Indexen en onderdelen (inclusief analyzer definities, scoreprofielen, CORS-opties), indexeerfuncties, gegevensbronnen, synoniemen, suggesties. | Administrator-code, RBAC-eigenaar of bijdrager van de resource  |
| Query uitvoeren in een index | Sleutel voor de beheerder of query (RBAC niet van toepassing) |
| Query uitvoeren op systeeminformatie, zoals statistieken, tellingen en lijsten van objecten retourneren. | Administrator-code, RBAC voor de resource (eigenaar, Inzender, lezer) |
| Beheersleutels beheren | Beheersleutel RBAC-eigenaar of bijdrager van de bron. |
| Querysleutels beheren |  Beheersleutel RBAC-eigenaar of bijdrager van de bron.  |

## <a name="physical-security"></a>Fysieke beveiliging

Microsoft-datacenters, toonaangevende fysieke beveiliging en compatibel zijn met een uitgebreide portfolio van normen en reglementen. Voor meer informatie gaat u naar de [wereldwijde datacenters](https://www.microsoft.com/cloud-platform/global-datacenters) pagina of bekijk een korte video op gegevens die security center.

> [!VIDEO https://www.youtube.com/embed/r1cyTL8JqRg]


## <a name="see-also"></a>Zie ook

+ [Ophalen .NET (ziet u een index te maken met behulp van een administratorsleutel) is gestart](search-create-index-dotnet.md)
+ [Ophalen REST (ziet u een index te maken met behulp van een administratorsleutel) is gestart](search-create-index-rest-api.md)
+ [Identiteit gebaseerd toegangsbeheer met Azure Search-filters](search-security-trimming-for-azure-search.md)
+ [Active Directory-identiteit gebaseerd toegangsbeheer met Azure Search-filters](search-security-trimming-for-azure-search-with-aad.md)
+ [Filters in Azure Search](search-filters.md)