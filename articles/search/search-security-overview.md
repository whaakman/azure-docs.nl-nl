---
title: Beveiligings- en privacy in Azure Search | Microsoft Docs
description: Azure Search is compatibel met SOC 2 HIPAA en andere certificeringen. Verbindings- en versleuteling, authenticatie en identiteit toegang via gebruiker en groep beveiligings-id's in Azure Search-filters.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: heidist
ms.openlocfilehash: 888f7c3ced0ef48cff222bffdbf0f278fa5f42b3
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285726"
---
# <a name="security-and-data-privacy-in-azure-search"></a>Beveiligings- en privacy in Azure Search

Uitgebreide beveiligingsfuncties en toegangsbeheer zijn ingebouwd in Azure Search om te controleren of persoonlijke inhoud op die manier blijft. In dit artikel worden de functies en standaarden compatibiliteit van beveiliging ingebouwd in Azure Search opgesomd.

Azure Search-beveiligingsarchitectuur omvat fysieke beveiliging, gecodeerde gegevensoverdrachten, gecodeerde opslag en naleving van de platform-standaarden. Azure Search accepteert alleen operationeel, geverifieerde aanvragen. U kunt eventueel per gebruiker toegangsbeheer op inhoud toevoegen via beveiligingsfilters. In dit artikel raakt op beveiliging op elke laag, maar richt zich voornamelijk op hoe gegevens en bewerkingen in Azure Search worden beveiligd.

## <a name="standards-compliance-iso-27001-soc-2-hipaa"></a>Naleving van standaarden: ISO 27001, SOC 2, HIPAA

Een gedeeltelijke lijst met standaarden naleving omvat SOC 2 Type 2 en HIPAA voor functies algemeen beschikbaar. Preview-functies zijn gecertificeerd als onderdeel van de algemene beschikbaarheid en mogen niet worden gebruikt in oplossingen met specifieke normen gehanteerd vereisten. Naleving certificeringsinstantie wordt beschreven in [overzicht van Microsoft Azure-naleving](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) en de [Vertrouwenscentrum](https://www.microsoft.com/en-us/trustcenter). 

Certificeringsinstantie voor de volgende normen is [aangekondigd in juni 2018](https://azure.microsoft.com/blog/azure-search-is-now-certified-for-several-levels-of-compliance/).

+ [ISO 27001: 2013](https://www.iso.org/isoiec-27001-information-security.html) 
+ [SOC 2 Type 2-naleving](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) voor het volledige rapport, gaat u naar [Azure- en Azure Government SOC 2 II rapport van het Type](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports). 
+ [Health Insurance Portability and Accountability Act (HIPAA)](https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act)
+ [GxP (21 CFR onderdeel 11)](https://en.wikipedia.org/wiki/Title_21_CFR_Part_11)
+ [HITRUST](https://en.wikipedia.org/wiki/HITRUST)
+ [PCI DSS-niveau 1](https://en.wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard)
+ [Australië IRAP geclassificeerde DLM](https://asd.gov.au/infosec/irap/certified_clouds.htm)

## <a name="encrypted-transmission-and-storage"></a>Versleutelde overdracht en opslag

Versleuteling breidt gedurende de volledige indexering pijplijn: uit via de verzending en naar beneden op geïndexeerde gegevens die zijn opgeslagen in Azure Search-verbindingen.

| Beveiligingsniveau | Beschrijving |
|----------------|-------------|
| Codering in transit | Azure Search luistert naar HTTPS-poort 443. Via het platform worden verbindingen met de Azure-services versleuteld. |
| Versleuteling 'at rest' | Versleuteling is volledig internalized tijdens het indexeren, zonder merkbare impact op de indexering van tijd voor voltooiing of de indexgrootte van de. Dit gebeurt automatisch op alle indexeren, inclusief op incrementele updates voor een index is niet volledig versleuteld (gemaakt voordat januari 2018).<br><br>Intern versleuteling is gebaseerd op [Azure Storage-Service: versleuteling](https://docs.microsoft.com/azure/storage/common/storage-service-encryption), met behulp van 256-bits [AES-versleuteling](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard).|

Versleuteling is interne naar Azure Search met certificaten en versleutelingssleutels intern beheerd door Microsoft en universeel toegepast. U kan geen versleuteling inschakelen of uitschakelen, beheren of vervangen door uw eigen sleutels of versleutelingsinstellingen voor in de portal of programmatisch weergeven. 

Codering in rust in 24 januari 2018 is aangekondigd en geldt voor alle Servicelagen, inclusief gedeelde (gratis) services, in alle regio's. Voor volledige versleuteling worden indexen gemaakt vóór deze datum verwijderd en opnieuw opgebouwd om versleuteling om te worden uitgevoerd. Anders worden alleen nieuwe gegevens toegevoegd na 24 januari versleuteld.

## <a name="azure-wide-user-access-controls"></a>Toegang van Azure wide gebruikersbesturingselementen

Verschillende beveiligingsmechanismen beschikbaar Azure hele zijn en dus automatisch beschikbaar zijn voor de Azure Search-resources die u maakt.

+ [De vergrendelingen bij het abonnement of de resource-niveau om te voorkomen dat de verwijdering](../azure-resource-manager/resource-group-lock-resources.md)
+ [Op rollen gebaseerde toegangsbeheer (RBAC) om de toegang tot informatie en administratieve bewerkingen beheren](../role-based-access-control/overview.md)

Alle Azure-services ondersteunen op rollen gebaseerde toegangsbeheer (RBAC) voor het instellen van toegangsniveaus consistent op alle services. Bijvoorbeeld is weergeven van gevoelige gegevens, zoals de beheersleutel beperkt tot de functies van eigenaar en Inzender, terwijl de servicestatus van de weergeven beschikbaar voor leden van een rol is. RBAC biedt rollen eigenaar, bijdrager en Reader. Standaard zijn alle servicebeheerders leden van de rol van eigenaar.

## <a name="service-access-and-authentication"></a>Toegang tot service en -verificatie

Terwijl Azure Search neemt over van de beveiliging van de beveiliging van de Azure-platform, bevat het ook de eigen verificatie op basis van sleutels. Een api-sleutel is een tekenreeks van willekeurige getallen en letters bestaan. Het type van de sleutel (admin of query) bepaalt het niveau van toegang. Verzenden van een geldige sleutel wordt beschouwd als bewijs van de aanvraag afkomstig is van een vertrouwde entiteit. Twee soorten sleutels worden gebruikt voor toegang tot uw search-service:

* Beheerder (geldig voor een bewerking lezen-schrijven tegen de service)
* Query (geldig voor alleen-lezen bewerkingen, zoals een query uitgevoerd naar een index)

Administratorsleutels worden gemaakt wanneer de service is ingericht. Er zijn twee administratorsleutels aangewezen als *primaire* en *secundaire* te houden rechtstreeks, maar in feite ze uitwisselbaar zijn. Elke service heeft twee administratorsleutels zodat u een overschakelen kunt zonder verlies van toegang tot uw service. Kunt u beide administratorsleutel opnieuw genereren, maar u kunt toevoegen aan het aantal totale-beheerder. Er is een maximum van twee administratorsleutels per zoekservice.

Querysleutels nodig worden gemaakt en zijn ontworpen voor clienttoepassingen die rechtstreeks zoeken aanroepen. U kunt maximaal 50 querysleutels maken. In de toepassingscode geeft u de URL zoeken en een query api-sleutel voor alleen-lezen toegang tot de service. Uw toepassingscode geeft ook de index die is gebruikt door de toepassing. Het eindpunt een api-sleutel voor alleen-lezen toegang en een doelindex definiëren samen het bereik en het toegangsniveau van de verbinding van uw clienttoepassing.

Verificatie is vereist voor elke aanvraag, waarbij elke aanvraag uit een verplichte sleutel, een bewerking, en een object bestaat. Wanneer aan elkaar zijn gekoppeld, zijn de twee machtigingsniveaus (volledig of alleen-lezen) plus de context (bijvoorbeeld een querybewerking op een index) voldoende voor de beveiliging van de volledige spectrum van servicebewerkingen. Zie voor meer informatie over sleutels [maken en beheren van de api-sleutels](search-security-api-keys.md).

## <a name="index-access"></a>Index toegang

In Azure Search is een afzonderlijke index niet een beveiligbaar object. Toegang tot een index wordt in plaats daarvan op de servicelaag (Lees- of schrijftoegang), samen met de context van een bewerking bepaald.

Voor toegang door eindgebruikers, kunt u structureren queryaanvragen verbinding maken via een querysleutel, dat een aanvraag doet alleen-lezen en de specifieke index die wordt gebruikt door uw app opgenomen. In een queryaanvraag is er geen concept van het lidmaatschap van de indexen of meerdere indexen tegelijkertijd toegang tot zodat alle aanvragen gericht op een enkele index per definitie. Als zodanig definieert constructie van de aanvraag zelf (een sleutel plus een index met één doel) de beveiligingsgrens.

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
| Querysleutels beheren |  Beheersleutel RBAC eigenaar of bijdrager zijn op de resource.  |

## <a name="physical-security"></a>Fysieke beveiliging

Microsoft-datacenters toonaangevende fysieke beveiliging bieden en compatibel zijn met een uitgebreide portfolio van standaarden en -voorschriften. Voor meer informatie gaat u naar de [globale datacenters](https://www.microsoft.com/cloud-platform/global-datacenters) pagina of een korte video op gegevens security center bekijken.

> [!VIDEO https://www.youtube.com/embed/r1cyTL8JqRg]


## <a name="see-also"></a>Zie ook

+ [Ophalen van .NET (demonstreert een index te maken met behulp van een administratorsleutel) is gestart](search-create-index-dotnet.md)
+ [Ophalen van REST (demonstreert een index te maken met behulp van een administratorsleutel) is gestart](search-create-index-rest-api.md)
+ [Op basis van identiteit toegangsbeheer met Azure Search-filters](search-security-trimming-for-azure-search.md)
+ [Active Directory-identiteit gebaseerd toegangsbeheer met Azure Search-filters](search-security-trimming-for-azure-search-with-aad.md)
+ [Filters in Azure Search](search-filters.md)