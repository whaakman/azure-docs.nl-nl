---
title: Modellering van Multitenancy in Azure Search | Microsoft Docs
description: Meer informatie over algemene ontwerppatronen voor multitenant SaaS-toepassingen tijdens het gebruik van Azure Search.
manager: jlembicz
author: LiamCavanagh
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: liamca
ms.custom: seodec2018
ms.openlocfilehash: 1da9756df4fa05b367665a5fe024528939f22578
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53313034"
---
# <a name="design-patterns-for-multitenant-saas-applications-and-azure-search"></a>Ontwerppatronen voor multitenant SaaS-toepassingen en Azure Search
Een multitenant-toepassing is die de dezelfde services en mogelijkheden biedt naar een willekeurig aantal tenants die niet kan zien of de gegevens van een andere tenant delen. Dit document bespreekt strategieën voor tenant-isolatie voor multitenant-toepassingen die zijn gebouwd met Azure Search.

## <a name="azure-search-concepts"></a>Azure Search-concepten
Als een oplossing voor search-as-a-service kunnen Azure Search ontwikkelaars uitgebreide zoekervaring toevoegen aan toepassingen zonder beheren van een infrastructuur of een expert in ophalen van gegevens is. Gegevens worden geüpload naar de service en vervolgens opgeslagen in de cloud. Met behulp van eenvoudige aanvragen aan de Azure Search-API, kunnen de gegevens vervolgens worden gewijzigd en doorzocht. Een overzicht van de service kan worden gevonden in [in dit artikel](https://aka.ms/whatisazsearch). Voordat u hierover te discussiëren ontwerppatronen, is het belangrijk om te begrijpen van enkele concepten in Azure Search.

### <a name="search-services-indexes-fields-and-documents"></a>Services, indexen, velden en documenten zoeken
Wanneer u Azure Search, een zich abonneert op een *search-service*. Als u gegevens naar Azure Search is geüpload, wordt deze opgeslagen in een *index* binnen de search-service. Er is een aantal indexen binnen één service. Voor het gebruik van de vertrouwde concepten van databases, kan de search-service worden vergeleken met een database terwijl de indexen in een service kunnen worden vergeleken met tabellen in een database.

Elke index binnen een search-service heeft een eigen schema, dat is gedefinieerd door een aantal aanpasbare *velden*. Gegevens worden toegevoegd aan een Azure Search-index in de vorm van afzonderlijke *documenten*. Elk document moet worden geüpload naar een specifieke index en van die index schema moet passen. Bij het zoeken naar gegevens met behulp van Azure Search, wordt de zoekopdracht in volledige tekst query's worden uitgegeven op basis van een bepaalde index.  Als u wilt vergelijken van deze concepten die van een database, velden kunnen worden vergeleken met de kolommen in een tabel en documenten kunnen worden vergeleken met rijen.

### <a name="scalability"></a>Schaalbaarheid
Een Azure Search-service in de standaard [prijscategorie](https://azure.microsoft.com/pricing/details/search/) kunt in twee dimensies schalen: opslag en beschikbaarheid.

* *Partities* kunnen worden toegevoegd aan het verhogen van de opslag van een service voor zoeken.
* *Replica's* kunnen worden toegevoegd aan een service voor het verhogen van de doorvoer van aanvragen die een search-service kan verwerken.

Toevoegen en verwijderen partities en replica's op kunt de capaciteit van de search-service om te groeien met de hoeveelheid gegevens en het verkeer naar de eisen van de toepassing. Opdat een search-service om een leesbewerking [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), hiervoor twee replica's. Opdat een service voor het bereiken van een lezen / schrijven [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), drie replica's is vereist.

### <a name="service-and-index-limits-in-azure-search"></a>Limieten voor service en de index in Azure Search
Er zijn een aantal verschillende [Prijscategorieën](https://azure.microsoft.com/pricing/details/search/) in Azure Search, elk van de lagen heeft verschillende [limieten en quota](search-limits-quotas-capacity.md). Sommige van deze limieten zijn op het serviceniveau, sommige zijn op de index-niveau en sommige zijn op de partitie-niveau.

|  | Basic | Standard1 | Standard2 | Standard3 | Standard3 HD |
| --- | --- | --- | --- | --- | --- |
| Maximum aantal replica's per Service |3 |12 |12 |12 |12 |
| Maximum aantal partities per Service |1 |12 |12 |12 |3 |
| Maximum aantal Zoekeenheden (replica's * partities) per Service |3 |36 |36 |36 |36 (maximaal 3 partities) |
| Maximale opslag per Service |2 GB |300 GB |1,2 TB |2,4 TB |600 GB |
| Maximale opslag per partitie |2 GB |25 GB |100 GB |200 GB |200 GB |
| Maximum aantal indexen per Service |5 |50 |200 |200 |3000 (maximaal 1000 indexen per partitie) |

#### <a name="s3-high-density"></a>S3 High Density'
In de Azure Search S3-prijscategorie is er een optie voor de modus hoge dichtheid (HD) is speciaal ontworpen voor scenario's voor meerdere tenants. In veel gevallen is het nodig voor het ondersteunen van een groot aantal kleinere tenants onder één service om te profiteren van de voordelen van eenvoud en kostenbeheersing efficiëntie.

S3 HD kunt u veel kleine indexen moeten worden verpakt onder het beheer van een service met één zoekopdracht doorzoeken door verhandeling de mogelijkheid om uit indexen met behulp van partities voor de mogelijkheid voor het hosten van meer indexen in één service te schalen.

Concrete invulling te geven, kan een S3-service hebben tussen 1 en 200 indexen die samen tot 1,4 miljard documenten kunnen hosten. Een S3 HD aan de andere kant afzonderlijke indexen alleen gaan tot wel 1 miljoen documenten wilt toestaan, maar maximaal 1000 indices per partitie (maximaal 3000 per service) met een totale documentaantal van 200 miljoen per partitie kan worden verwerkt (maximaal 600 miljoen per service).

## <a name="considerations-for-multitenant-applications"></a>Overwegingen voor multitenant-toepassingen
Multitenant-toepassingen moeten effectief met het distribueren van bronnen tussen de tenants behoud van bepaalde mate van privacy tussen de verschillende tenants. Er zijn enkele overwegingen bij het ontwerpen van de architectuur voor dergelijke toepassing:

* *Isolatie van tenants:* Ontwikkelaars van toepassingen moeten passende maatregelen om ervoor te zorgen dat er geen tenants hebben niet-geautoriseerde of toegang tot de gegevens van andere tenants ongewenste. Naast het perspectief van de privacy van gegevens vereisen tenant isolatie strategieën effectief beheer van gedeelde resources en de bescherming van de luidruchtige buren.
* *Kosten van de resource cloud:* Net als bij elke andere toepassing, moeten de software-oplossingen kosten concurrerende als onderdeel van een multitenant-toepassing blijven.
* *Eenvoudige bewerkingen:* Wanneer u een architectuur met meerdere tenants ontwikkelt, is de impact op de bewerkingen en de complexiteit van de toepassing een belangrijk aandachtspunt. Azure Search is een [SLA van 99,9%](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
* *Wereldwijde voetafdruk:* Multitenant-toepassingen mogelijk effectief fungeren tenants die zijn verdeeld over de hele wereld.
* *Schaalbaarheid:* Ontwikkelaars van toepassingen moeten rekening houden met hoe ze in overeenstemming brengen tussen een voldoende laag niveau van de complexiteit van de toepassing te onderhouden en het ontwerpen van de toepassing om te schalen met het aantal tenants en de grootte van de gegevens en werkbelasting van tenants.

Azure Search biedt een aantal grenzen die kunnen worden gebruikt voor het isoleren van de gegevens en werkbelasting van tenants.

## <a name="modeling-multitenancy-with-azure-search"></a>Multitenancy met Azure Search modelleren
In het geval van een scenario voor meerdere tenants, ontwikkelaar van de toepassing verbruikt een of meer search-services en delen van hun tenants tussen services en/of indexen. Azure Search heeft een aantal algemene patronen bij het modelleren van een scenario voor meerdere tenants:

1. *Index per tenant:* Elke tenant heeft een eigen index binnen een search-service die wordt gedeeld met andere tenants.
2. *De service per tenant:* Elke tenant heeft een eigen toegewezen Azure Search-service, biedt de hoogste niveau van scheiding van gegevens en werkbelasting.
3. *De combinatie van beide:* Grotere, meer actieve tenants zijn toegewezen services toegewezen, terwijl tenants kleinere afzonderlijke indexen in gedeelde services zijn toegewezen.

## <a name="1-index-per-tenant"></a>1. Index per tenant
![Een portrayal van de index-per-tenant-model](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

In een model met index-per-tenant nemen meerdere tenants een enkele Azure Search-service waarbij elke tenant een eigen index heeft.

Tenants bereiken gegevensisolatie omdat alle aanvragen zoeken en documentbewerkingen op het indexniveau van een in Azure Search worden uitgegeven. In het niveau van de toepassing is het bewustzijn nodig om te leiden van de verschillende tenants verkeer naar de juiste indexen bij het beheren van resources op het serviceniveau van de voor alle tenants.

Een sleutelkenmerk van de index-per-tenant-model is de mogelijkheid voor de ontwikkelaar van de toepassing naar meerdere virtuele machines gebruiken de capaciteit van een service voor zoeken tussen tenants van de toepassing. Als de tenants een ongelijke verdeling van de werkbelasting hebben, kan de optimale combinatie van tenants worden verdeeld over een zoekservice indexen voor een aantal zeer actief, resource-intensieve tenants terwijl tegelijkertijd een lange staart van minder actieve tenants. De verhouding is het niet van het model voor het afhandelen van situaties waarin elke tenant gelijktijdig zeer actief is.

De index-per-tenant-model vormt de basis voor een model voor variabele kosten, waar een hele Azure Search-service is gekocht vooraf te betalen en vervolgens gevuld met tenants. Dit kan de niet-gebruikte capaciteit moet zijn aangewezen voor proefversies en gratis accounts.

Voor toepassingen met een wereldwijde footprint altijd het model van de index-per-tenant niet de meest efficiënte. Als tenants van een toepassing worden verdeeld over de hele wereld, is het mogelijk dat een afzonderlijke service die nodig zijn voor elke regio die kosten voor elk van deze mogelijk dupliceren.

Azure Search kan de schaal van de afzonderlijke indexen en het totale aantal indexen te laten groeien. Als een juiste prijzen worden de laag gekozen, is de partities en replica's kunnen worden toegevoegd aan de hele search-service als een afzonderlijke index in de service te groot is voor wat betreft opslag of het verkeer toeneemt.

Als het totale aantal indexen te voor één service groot is een andere service om te voldoen aan de nieuwe tenants worden ingericht. Als indexen te worden verplaatst tussen search-services als nieuwe services worden toegevoegd, moet de gegevens van de index handmatig worden gekopieerd van een index in de andere als Azure Search is niet toegestaan voor een index worden verplaatst.

## <a name="2-service-per-tenant"></a>2. Service per tenant
![Een portrayal van het service-per-tenant-model](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

Elke tenant heeft in een architectuur met service-per-tenant een eigen search-service.

In dit model realiseert de toepassing het maximumniveau van isolatie voor de tenants. Elke service heeft toegewezen opslag en doorvoer voor het verwerken van de zoekopdracht, evenals een afzonderlijke API-sleutels.

Het service-per-tenant-model is een effectieve keuze voor toepassingen waarbij elke tenant heeft een groot footprint of de werkbelasting heeft slechts variabiliteit tenant tenant, zoals resources zijn niet verdeeld over verschillende tenants werkbelastingen.

Een service per tenant model biedt ook het voordeel van een voorspelbaar, vaste kostenmodel. Er is geen investeringen vooraf in een hele search-service totdat er een tenant in te vullen, maar de kosten-per-tenant hoger dan een index-per-tenant-model is.

Het service-per-tenant-model is een efficiënte keuze voor toepassingen met een wereldwijde voetafdruk. Met geografisch verspreide-tenants is het eenvoudig te hebben van elke tenant-service in de juiste regio.

De uitdagingen bij het schalen van dit patroon zich kunnen voordoen bij afzonderlijke tenants langzamerhand hun service. Azure Search ondersteunt momenteel geen upgrade van de prijscategorie van een service voor zoeken, zodat alle gegevens zelf zou moeten handmatig worden gekopieerd naar een nieuwe service.

## <a name="3-mixing-both-models"></a>3. Met een combinatie van beide modellen
Een ander patroon voor het modelleren van multitenancy is een combinatie van strategieën voor zowel de index-per-tenant en de service-per-tenant.

Door een combinatie van de twee patronen, de grootste tenants van een toepassing in beslag kunnen nemen toegewezen services terwijl de lang staart van minder actieve, kleinere tenants in beslag indexen in een gedeelde-service nemen kan. Dit model zorgt ervoor dat het grootste tenants consistent hoge prestaties van de service bij het beveiligen van de kleinere tenants van de eventuele luidruchtige buren hebben.

Implementatie van deze strategie hebben echter prognose voorspellen welke tenants moet een toegewezen service ten opzichte van een index in een gedeelde-service. Met de noodzaak voor het beheren van deze modellen multitenancy verhoogt de complexiteit van de toepassing.

## <a name="achieving-even-finer-granularity"></a>Zelfs weer specifieker bereiken
De bovenstaande ontwerppatronen als model voor meerdere tenants scenario's in Azure Search wordt ervan uitgegaan dat een uniform scope waarbij elke tenant een hele exemplaar van een toepassing is. Toepassingen kunnen echter soms verwerkt voor veel kleinere bereiken.

Als service-per-tenant en index per tenant modellen niet voldoende kleine bereiken zijn, is het mogelijk om een index voor het bereiken van een nog betere mate van granulatie te modelleren.

Als u wilt één index zich anders gedragen voor andere client-eindpunten, kan een veld worden toegevoegd aan een index waarmee wordt aangegeven dat een bepaalde waarde voor elke client mogelijk. Telkens wanneer een client Azure Search als u wilt opvragen of wijzigen van een index, roept de code van de clienttoepassing Hiermee geeft u de juiste waarde voor dat veld met behulp van Azure-Search [filter](https://msdn.microsoft.com/library/azure/dn798921.aspx) mogelijkheden bij het uitvoeren van query's.

Deze methode kan worden gebruikt om de functionaliteit van afzonderlijke gebruikersaccounts, afzonderlijke machtigingsniveaus, bereiken en zelfs verschillende toepassingen.

> [!NOTE]
> Met behulp van de hierboven beschreven aanpak voor het configureren van één index voor meerdere tenants is van invloed op de relevantie van zoekresultaten. Zoeken op relevantie scores worden berekend op een bereik van de index-niveau, niet op tenantniveau-bereik, zodat de gegevens van alle tenants zijn opgenomen in de volgorde van relevantie-scores onderliggende statistieken, zoals de frequentie van de termijn.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Azure Search is een aantrekkelijke keuze voor veel toepassingen, [voor meer informatie over de robuuste mogelijkheden van de service](https://aka.ms/whatisazsearch). Bij het evalueren van de verschillende ontwerppatronen voor multitenant-toepassingen, houd rekening met de [verschillende Prijscategorieën](https://azure.microsoft.com/pricing/details/search/) en de desbetreffende [Servicelimieten](search-limits-quotas-capacity.md) om aan te passen beste Azure Search aan toepassing werkbelastingen en architecturen van elke omvang.

Vragen over Azure Search en scenario's voor meerdere tenants kunnen worden omgeleid naar azuresearch_contact@microsoft.com.

