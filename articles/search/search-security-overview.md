---
title: Beveiligen van gegevens en bewerkingen in Azure Search | Microsoft Docs
description: Azure Search-beveiliging is gebaseerd op naleving, versleuteling, verificatie en identiteit toegang via gebruiker en groep beveiligings-id's in Azure Search filters SOC 2.
services: search
documentationcenter: 
author: HeidiSteen
manager: cgronlun
editor: 
ms.assetid: 
ms.service: search
ms.devlang: 
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/19/2018
ms.author: heidist
ms.openlocfilehash: c3aa4883e33b1f3494f8502fe7f8b12f7d64a72f
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="security-and-controlled-access-in-azure-search"></a>Beveiliging en beperkte toegang in Azure Search

Azure Search is [SOC 2 compatibele](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports), met een uitgebreide beveiliging architectuur spanning fysieke beveiliging, gecodeerde gegevensoverdrachten, gecodeerde opslag en platform wide software beveiligingen. Azure Search accepteert alleen operationeel, geverifieerde aanvragen. U kunt eventueel per gebruiker toegangsbeheer op inhoud toevoegen. In dit artikel raakt op beveiliging op elke laag, maar richt zich voornamelijk op hoe gegevens en bewerkingen in Azure Search worden beveiligd.

![Diagram van beveiligingslagen blokkeren](media/search-security-overview/azsearch-security-diagram.png)

## <a name="physical-security"></a>Fysieke beveiliging

Microsoft-datacenters toonaangevende fysieke beveiliging bieden en compatibel zijn met een uitgebreide portfolio van standaarden en -voorschriften. Voor meer informatie gaat u naar de [globale datacenters](https://www.microsoft.com/cloud-platform/global-datacenters) pagina of een korte video op gegevens security center bekijken.

> [!VIDEO https://www.youtube.com/embed/r1cyTL8JqRg]

## <a name="encrypted-transmission-and-storage"></a>Versleutelde overdracht en opslag

Versleuteling breidt gedurende de volledige indexering pijplijn: uit via de verzending en naar beneden op geïndexeerde gegevens die zijn opgeslagen in Azure Search-verbindingen.

| Beveiligingsniveau | Beschrijving |
|----------------|-------------|
| Codering in transit | Azure Search luistert naar HTTPS-poort 443. Via het platform worden verbindingen met de Azure-services versleuteld. |
| Versleuteling 'at rest' | Versleuteling is volledig internalized tijdens het indexeren, zonder merkbare impact op de indexering van tijd voor voltooiing of de indexgrootte van de. Dit gebeurt automatisch op alle indexeren, inclusief op incrementele updates voor een index is niet volledig versleuteld (gemaakt voordat januari 2018).<br><br>Intern versleuteling is gebaseerd op [Azure Storage-Service: versleuteling](https://docs.microsoft.com/azure/storage/common/storage-service-encryption), met behulp van 256-bits [AES-versleuteling](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard).|
| [SOC 2 naleving](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) | Alle zoekservices zijn volledig compatibel zijn, AICPA SOC 2 in alle datacenters met Azure Search. Als u wilt het volledige rapport bekijken, gaat u naar [Azure- en Azure Government SOC 2 II rapport van het Type](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports). |

Versleuteling is interne naar Azure Search met certificaten en versleutelingssleutels intern beheerd door Microsoft en universeel toegepast. U kan geen versleuteling inschakelen of uitschakelen, beheren of vervangen door uw eigen sleutels of versleutelingsinstellingen voor in de portal of programmatisch weergeven. 

Codering in rust in 24 januari 2018 is aangekondigd en geldt voor alle Servicelagen, inclusief gedeelde (gratis) services, in alle regio's. Voor volledige versleuteling worden indexen gemaakt vóór deze datum verwijderd en opnieuw opgebouwd om versleuteling om te worden uitgevoerd. Anders worden alleen nieuwe gegevens toegevoegd na 24 januari versleuteld.

## <a name="azure-wide-logical-security"></a>Logische Azure beveiligingstoegang

Verschillende beveiligingsmechanismen zijn beschikbaar via de Azure-Stack en dus automatisch beschikbaar zijn voor de Azure Search-resources die u maakt.

+ [De vergrendelingen bij het abonnement of de resource-niveau om te voorkomen dat de verwijdering](../azure-resource-manager/resource-group-lock-resources.md)
+ [Op rollen gebaseerde toegangsbeheer (RBAC) om de toegang tot informatie en administratieve bewerkingen beheren](../active-directory/role-based-access-control-what-is.md)

Alle Azure-services ondersteunen op rollen gebaseerde toegangsbeheer (RBAC) voor het instellen van toegangsniveaus consistent op alle services. Bijvoorbeeld is weergeven van gevoelige gegevens, zoals de beheersleutel beperkt tot de functies van eigenaar en Inzender, terwijl de servicestatus van de weergeven beschikbaar voor leden van een rol is. RBAC biedt rollen eigenaar, bijdrager en Reader. Standaard zijn alle servicebeheerders leden van de rol van eigenaar.

## <a name="service-access-and-authentication"></a>Toegang tot service en -verificatie

Terwijl Azure Search neemt over van de beveiliging van de beveiliging van de Azure-platform, bevat het ook de eigen verificatie op basis van sleutels. Het type van de sleutel (admin of query) bepaalt het niveau van toegang. Verzenden van een geldige sleutel wordt beschouwd als bewijs van de aanvraag afkomstig is van een vertrouwde entiteit. 

Verificatie is vereist voor elke aanvraag, waarbij elke aanvraag uit een verplichte sleutel, een bewerking, en een object bestaat. Wanneer aan elkaar zijn gekoppeld, zijn de twee machtigingsniveaus (volledig of alleen-lezen) plus de context voldoende voor de beveiliging van de volledige spectrum van servicebewerkingen. 

|Sleutel|Beschrijving|Limieten|  
|---------|-----------------|------------|  
|Beheer|De volledige rechten verleent voor alle bewerkingen, inclusief de mogelijkheid voor het beheren van de service maken en verwijderen van indexen, Indexeerfuncties en gegevensbronnen.<br /><br /> Twee admin **api-sleutels**waarnaar wordt verwezen als *primaire* en *secundaire* sleutels in de portal worden gegenereerd als de service wordt gemaakt en afzonderlijk kan worden hersteld op aanvraag . Met twee sleutels, kunt u één sleutel overschakelen tijdens het gebruik van de tweede sleutel voor permanente toegang tot de service.<br /><br /> Administratorsleutels alleen in de HTTP-aanvraagheaders opgegeven. U kunt een admin api-sleutel in een URL plaatsen.|Maximaal 2 per service|  
|Query’s uitvoeren|Alleen-lezen toegang verleent tot indexen en documenten en worden doorgaans verleend aan clienttoepassingen die zoekaanvragen.<br /><br /> Querysleutels worden op verzoek gemaakt. U kunt ze handmatig maken in de portal of programmatisch via de [Management REST API](https://docs.microsoft.com/rest/api/searchmanagement/).<br /><br /> Querysleutels kunnen worden opgegeven in de header van een HTTP-aanvraag voor het zoeken, suggestie of zoekbewerking. U kunt ook een querysleutel doorgeven als een parameter van een URL. Afhankelijk van hoe uw clienttoepassing formuleert voor de aanvraag, is het mogelijk dat het eenvoudiger om door te geven van de sleutel als een queryparameter:<br /><br /> `GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2016-09-01&api-key=A8DA81E03F809FE166ADDB183E9ED84D`|50 per service|  

 Visueel, bestaat er geen onderscheid tussen een beheersleutel en querysleutel. Beide sleutels zijn tekenreeksen die bestaat uit 32 willekeurig gegenereerd alfanumerieke tekens. Als u het bijhouden van welk type sleutel dat is opgegeven in uw toepassing verliest, kunt u [controleren de sleutelwaarden in de portal](https://portal.azure.com) of gebruik de [REST-API](https://docs.microsoft.com/rest/api/searchmanagement/) om de waarde en sleuteltype te retourneren.  

> [!NOTE]  
>  Dit wordt beschouwd als een slechte beveiligingsprocedure om door te geven gevoelige gegevens, zoals een `api-key` in de aanvraag-URI. Om deze reden accepteert Azure Search alleen een querysleutel als een `api-key` in de query string en u moet voorkomen in dat geval tenzij de inhoud van de index moeten openbaar zijn. Als in het algemeen wordt aangeraden doorgeven uw `api-key` als een aanvraag-header.  

### <a name="how-to-find-the-access-keys-for-your-service"></a>Het zoeken van de toegangssleutels voor uw service

U kunt verkrijgen toegangstoetsen in de portal of via de [Management REST API](https://docs.microsoft.com/rest/api/searchmanagement/). Zie voor meer informatie [sleutels beheren](search-manage.md#manage-api-keys).

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Lijst met de [services zoeken](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) voor uw abonnement.
3. Selecteer de service en vinden op de servicepagina **instellingen** >**sleutels** admin en query sleutels weergeven.

![Portal-pagina, instellingen, sleutels sectie](media/search-security-overview/settings-keys.png)

## <a name="index-access"></a>Index toegang

In Azure Search is een afzonderlijke index niet een beveiligbaar object. Toegang tot een index wordt in plaats daarvan op de servicelaag (Lees- of schrijftoegang), samen met de context van een bewerking bepaald.

In het geval van toegang door eindgebruikers, kunt u queryaanvragen structureren in uw toepassing verbinding maken via een querysleutel, dat een aanvraag doet alleen-lezen en de specifieke index die wordt gebruikt door uw app opgenomen. In een queryaanvraag is er geen concept van het lidmaatschap van de indexen of meerdere indexen tegelijkertijd toegang tot zodat alle aanvragen gericht op een enkele index per definitie. Als zodanig in de structuur van de aanvraag zelf (een sleutel plus een index met één doel) de beveiligingsgrens wordt gedefinieerd.

Beheerders en ontwikkelaars toegang tot indexen is niet gedifferentieerde: moeten beide schrijftoegang maken, verwijderen en bijwerken van objecten die worden beheerd door de service. Iedereen met een beheersleutel met uw service kan lezen, wijzigen of verwijderen van een index in dezelfde service. Voor bescherming tegen het onbedoeld of schadelijke verwijderen van indexen is uw interne bronbeheer voor bedrijfsmiddelen die met code de remedy voor omgekeerd of een ongewenste index verwijderd of gewijzigd. Azure Search heeft failover binnen het cluster om beschikbaarheid te garanderen, maar deze niet opgeslagen of uw eigen code gebruikt voor het maken of laden indexen uitvoeren.

Voor multitenancy oplossingen vereisen beveiligingsgrenzen op het indexniveau van de bevatten dergelijke oplossingen doorgaans een middelste laag, welke klanten gebruik voor de verwerking van de index isolatie. Zie voor meer informatie over het multitenant gebruiksvoorbeeld [ontwerppatronen voor multitenant SaaS-toepassingen en Azure Search](search-modeling-multitenant-saas-applications.md).

## <a name="admin-access-from-client-apps"></a>Beheerderstoegang van client-apps

De Azure Search Management REST API is een uitbreiding van de Azure Resource Manager en de afhankelijkheden ervan deelt. Als zodanig is Active Directory een vereiste voor het servicebeheer van Azure Search. Alle administratieve aanvragen van clientcode moeten worden geverifieerd met behulp van Azure Active Directory voordat de aanvraag de Resource Manager is bereikt.

Aanvragen voor gegevens voor het eindpunt van een Azure Search-service, zoals Create Index (Azure Search Service REST-API) of documenten zoeken (Azure Search Service REST-API), gebruik api-sleutel in de aanvraagheader.

Als uw toepassingscode beheer servicebewerkingen, evenals gegevensbewerkingen op zoekindexen of documenten verwerkt, twee manieren implementeren in uw code: de toegangssleutel van systeemeigen naar Azure Search en de verificatie van Active Directory methodologie door Resource Manager vereist. 

Zie voor meer informatie over het structureren van een aanvraag in Azure Search [Azure Search Service REST](https://docs.microsoft.com/rest/api/searchservice/). Zie voor meer informatie over de verificatievereisten voor Resource Manager [gebruik Resource Manager authenticatie-API aan abonnementen, toegang](../azure-resource-manager/resource-manager-api-authentication.md).

## <a name="user-access-to-index-content"></a>Gebruikerstoegang tot inhoud van de index

Per gebruiker toegang tot de inhoud van een index wordt geïmplementeerd via beveiligingsfilters op uw query's, documenten die zijn gekoppeld aan een bepaalde beveiligings-id retourneren. In plaats van vooraf gedefinieerde rollen en roltoewijzingen is toegangsbeheer op basis van identiteit geïmplementeerd als een filter dat wil zeggen van documenten en inhoud op basis van identiteiten zoekresultaten. De volgende tabel beschrijft de twee methoden voor bijsnijden zoekresultaten van niet-geautoriseerde inhoud.

| Benadering | Beschrijving |
|----------|-------------|
|[Hoe worden ingekort op basis van identiteit filters](search-security-trimming-for-azure-search.md)  | De algemene werkstroom voor het implementeren van toegangsbeheer voor gebruikers-id en documenten. Het bevat informatie over beveiligings-id toe te voegen aan een index en vervolgens wordt uitgelegd filtering op voor de resultaten van niet-toegestane inhoud trim voor dat veld. |
|[Hoe worden ingekort op basis van Azure Active Directory-id 's](search-security-trimming-for-azure-search-with-aad.md)  | In dit artikel wordt uitgebreid in het vorige artikel stappen die voor het ophalen van id's van Azure Active Directory (AAD), een van de [gratis services](https://azure.microsoft.com/free/) in het Azure-cloud-platform. |

## <a name="table-permissioned-operations"></a>Tabel: Permissioned operations

De volgende tabel geeft een overzicht van de bewerkingen die zijn toegestaan in Azure Search en welke sleutel Hiermee ontgrendelt u de toegang tot een bepaalde bewerking.

| Bewerking | Machtigingen |
|-----------|-------------------------|
| Een service maken | Azure-abonnement houder|
| Een service schaalt | Beheersleutel RBAC eigenaar of bijdrager zijn op de bron  |
| Een service verwijderen | Beheersleutel RBAC eigenaar of bijdrager zijn op de bron |
| Maken, wijzigen en verwijderen van objecten op de service: <br>Indexen en onderdelen (inclusief analyzer definities, scoreprofiel profielen, CORS opties), indexeerfuncties, gegevensbronnen, synoniemen, suggestiefunctie. | Beheersleutel RBAC eigenaar of bijdrager zijn op de bron  |
| Query uitvoeren in een index | Beheerder of de query-sleutel (RBAC niet van toepassing) |
| Systeemgegevens opvragen, zoals het retourneren van statistieken, aantallen en een lijst met objecten. | Beheersleutel, RBAC voor de bron (eigenaar, bijdrager, lezer) |
| Beheerder sleutels beheren | Beheersleutel RBAC eigenaar of bijdrager zijn op de resource. |
| Querysleutels beheren |  Beheersleutel RBAC eigenaar of bijdrager zijn op de resource. RBAC lezer kunt querysleutels weergeven. |


## <a name="see-also"></a>Zie ook

+ [Ophalen van .NET (demonstreert een index te maken met behulp van een administratorsleutel) is gestart](search-create-index-dotnet.md)
+ [Ophalen van REST (demonstreert een index te maken met behulp van een administratorsleutel) is gestart](search-create-index-rest-api.md)
+ [Op basis van identiteit toegangsbeheer met Azure Search-filters](search-security-trimming-for-azure-search.md)
+ [Active Directory-identiteit gebaseerd toegangsbeheer met Azure Search-filters](search-security-trimming-for-azure-search-with-aad.md)
+ [Filters in Azure Search](search-filters.md)