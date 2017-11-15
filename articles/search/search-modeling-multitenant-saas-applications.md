---
title: Modeling Multitenancy in Azure Search | Microsoft Docs
description: Meer informatie over algemene ontwerppatronen voor multitenant SaaS-toepassingen tijdens het gebruik van Azure Search.
services: search
manager: jhubbard
author: ashmaka
documentationcenter: 
ms.assetid: 72e9696a-553b-47dc-9e05-a82db0ebf094
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/09/2017
ms.author: ashmaka
ms.openlocfilehash: 622ae64e118dd2498aff0bf2e9f6c1dbfb0ab045
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="design-patterns-for-multitenant-saas-applications-and-azure-search"></a>Ontwerppatronen voor multitenant SaaS-toepassingen en Azure Search
Een multitenant-toepassing is dat de services en de mogelijkheden biedt voor een willekeurig aantal tenants die niet zien of delen van de gegevens van een andere tenant. Dit document wordt besproken tenant isolatie strategieën voor multitenant toepassingen die zijn gebouwd met Azure Search.

## <a name="azure-search-concepts"></a>Azure Search-concepten
Als een oplossing zoeken as a service kunnen Azure Search ontwikkelaars een uitgebreide zoekopdracht ervaringen toevoegen aan toepassingen zonder eventuele infrastructuurbeheer of u een expert in informatie ophalen. Gegevens worden geüpload naar de service en klik vervolgens in de cloud worden opgeslagen. Eenvoudige aanvragen tot de Azure Search API gebruikt, kunnen de gegevens vervolgens worden gewijzigd en doorzocht. Een overzicht van de service kunt u vinden in [in dit artikel](http://aka.ms/whatisazsearch). Voordat u ontwerppatronen, is het belangrijk te weten van enkele concepten die in Azure Search.

### <a name="search-services-indexes-fields-and-documents"></a>Search-services, indexen, velden en -documenten
Wanneer u Azure Search, een lid van een *zoekservice*. Als gegevens worden geüpload naar Azure Search, wordt opgeslagen in een *index* binnen de search-service. Er is een aantal indexen binnen een enkele service. Voor het gebruik van de vertrouwde concepten van databases kan de zoekservice worden vergeleken met een database terwijl de indexen in een service kunnen worden vergeleken met tabellen in een database.

Elke index binnen een search-service heeft een eigen schema, die wordt gedefinieerd door een aantal aanpasbare *velden*. Gegevens aan worden toegevoegd aan een Azure Search-index in de vorm van afzonderlijke *documenten*. Elk document moet worden geüpload naar een specifieke index en die index schema moet passen. Bij het zoeken van gegevens met behulp van Azure Search, wordt de zoekopdracht in volledige tekst query's worden uitgegeven op basis van een bepaalde index.  Als u wilt vergelijken deze begrippen aan die van een database, velden kunnen worden vergeleken met de kolommen in een tabel en documenten kunnen worden vergeleken met de rijen.

### <a name="scalability"></a>Schaalbaarheid
Een Azure Search-service in de standaard [prijscategorie](https://azure.microsoft.com/pricing/details/search/) in twee dimensies kunt schalen: opslag en beschikbaarheid.

* *Partities* kunnen worden toegevoegd aan het verhogen van de opslag van een service voor zoeken.
* *Replica's* kunnen worden toegevoegd aan een service te verhogen van de doorvoer van aanvragen dat een search-service kan verwerken.

Toevoegen en verwijderen partities en replica's op kunt de capaciteit van de search-service kan worden uitgebreid met de hoeveelheid gegevens en verkeer van de eisen van de toepassing. Om een search-service om te lezen bereiken [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), hiervoor twee replica's. Om een service voor een alleen-lezen [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), drie replica's is vereist.

### <a name="service-and-index-limits-in-azure-search"></a>In Azure Search-service en de index limieten
Er zijn slechts enkele andere [Prijscategorieën](https://azure.microsoft.com/pricing/details/search/) in Azure Search elk van de lagen heeft verschillende [limieten en quota's](search-limits-quotas-capacity.md). Sommige van deze beperkingen zijn op het serviceniveau, sommige zijn op niveau van de index en sommige zijn op de partitie-niveau.

|  | Basic | Standard1 | Standard2 | Standard3 | HD Standard3 |
| --- | --- | --- | --- | --- | --- |
| Maximum aantal replica's per Service |3 |12 |12 |12 |12 |
| Maximum aantal partities per Service |1 |12 |12 |12 |3 |
| Maximum aantal Search-eenheden (replica's * partities) per Service |3 |36 |36 |36 |36 (maximaal 3 partities) |
| Maximum aantal documenten per Service |1 miljoen |180 miljoen |720 miljoen |1.4 miljard |600 miljoen |
| Maximale opslag per Service |2 GB |300 GB |1,2 TB |2,4 TB |600 GB |
| Maximum aantal documenten per partitie |1 miljoen |15 miljoen |60 miljoen |120 miljoen |200 miljoen |
| Maximale opslag per partitie |2 GB |25 GB |100 GB |200 GB |200 GB |
| Maximum aantal indexen per Service |5 |50 |200 |200 |3000 (maximaal 1000 indexen/partitie) |

#### <a name="s3-high-density"></a>S3 High-density '
Er is een optie voor de modus hoge dichtheid (HD) is speciaal ontworpen voor multitenant scenario's in Azure-Search S3 prijscategorie. In veel gevallen is het nodig zijn ter ondersteuning van een groot aantal onder een service te profiteren van de voordelen van eenvoud en kostenbesparing kleinere tenants.

S3 HD kunt u veel kleine indexen moeten worden verpakt onder het beheer van een enkele search-service door de mogelijkheid moet worden uitgebreid met behulp van partities voor de mogelijkheid voor het hosten van meer indexen in een enkele service indexen handel.

Concrete invulling te geven, kan een service S3 hebben tussen 1 en 200 indexen die samen tot 1,4 miljard documenten kunnen hosten. Een HD S3 aan de andere kant afzonderlijke indexen alleen gaan 1 miljoen documenten wilt toestaan, maar deze kan omgaan met maximaal 1000 indexen per partitie (maximaal 3000 per service) met een telling van de totale document van 200 miljoen per partitie (maximaal 600 miljoen per service).

## <a name="considerations-for-multitenant-applications"></a>Overwegingen voor multitenant-toepassingen
Multitenant toepassingen moeten effectief met het distribueren van bronnen tussen de tenants behoud een zekere mate van privacy tussen de verschillende tenants. Er zijn enkele overwegingen bij het ontwerpen van de architectuur voor een dergelijke toepassing:

* *Tenantisolatie:* Toepassingsontwikkelaars hoeven te passende maatregelen te nemen om ervoor te zorgen dat geen tenants hebt geverifieerd of toegang tot de gegevens van andere tenants ongewenste. Afgezien van het perspectief van de privacy van gegevens vereisen tenant isolatie strategieën effectief beheer van gedeelde bronnen en bescherming tegen ruis neighbors.
* *Kosten van de resource cloud:* als met een andere toepassing softwareoplossingen kosten concurrerende als onderdeel van een multitenant-toepassing moeten blijven.
* *Gebruiksgemak Operations:* bij het ontwikkelen van een multitenant-architectuur, de impact op de bedrijfsactiviteiten en complexiteit van de toepassing is een belangrijk aandachtspunt. Azure Search is een [SLA van 99,9%](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
* *Globale footprint:* Multitenant toepassingen mogelijk effectief dienen tenants die zijn verdeeld over de hele wereld.
* *Schaalbaarheid:* toepassingsontwikkelaars moeten rekening houden met hoe ze in overeenstemming brengen tussen het onderhouden van een voldoende laag niveau van complexiteit bij de toepassing en het ontwerpen van de toepassing om te schalen met het aantal tenants en de grootte van gegevens van tenants en werkbelasting.

Azure Search biedt een aantal grenzen die kunnen worden gebruikt voor het isoleren van de gegevens en de werkbelasting van tenants.

## <a name="modeling-multitenancy-with-azure-search"></a>Multitenancy modellering met Azure Search
In het geval van een multitenant scenario ontwikkelaar van de toepassing maakt gebruik van een of meer search-services en delen van hun tenants tussen services, indexen of beide. Azure Search heeft enkele algemene patronen bij het modelleren van een multitenant scenario:

1. *Index per tenant:* elke tenant heeft zijn eigen index binnen een search-service die wordt gedeeld met andere tenants.
2. *Service per tenant:* elke tenant heeft zijn eigen toegewezen Azure Search-service offering hoogste niveau van de scheiding van gegevens en werkbelasting.
3. *De combinatie van beide:* tenants grotere, meer actief zijn speciale services toegewezen terwijl kleinere tenants afzonderlijke indexen binnen gedeelde services zijn toegewezen.

## <a name="1-index-per-tenant"></a>1. Index per tenant
![Een portrayal van het model index per tenant](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

In een model index per tenant innemen meerdere tenants één Azure Search-service waarbij elke tenant hun eigen index heeft.

Tenants bereiken gegevensisolatie, omdat alle aanvragen zoeken en document bewerkingen op het indexniveau van een in Azure Search worden uitgegeven. In de toepassingslaag is de noodzaak afhankelijkheid van de verschillende tenants verkeer omleiden naar de juiste indexen tijdens ook het beheren van bronnen op het serviceniveau van de in alle tenants.

Een kenmerk key van het model index per tenant is de mogelijkheid voor de ontwikkelaar van de toepassing aan de capaciteit van een zoekservice tussen de toepassing tenants oversubscribe. Als de tenants een ongelijke verdeling van werkbelasting, kan de optimale combinatie van tenants worden verdeeld over een zoekservice indexen aangepast aan een aantal zeer actieve, bronintensieve tenants tijdens tegelijkertijd afhandeling van minder lang staart actieve tenants. De verhouding is het niet van het model voor het afhandelen van situaties waarbij elke tenant gelijktijdig maximaal actief is.

Het model index per tenant vormt de basis voor een model variabele kosten, waar een volledige Azure Search-service is gekocht vooraf en vervolgens gevuld met tenants. Hierdoor ongebruikte capaciteit aan te wijzen voor proefabonnementen en gratis accounts.

Voor toepassingen met een globale footprint, het model index per tenant niet mogelijk het meest efficiënt. Als een toepassing tenants zijn verdeeld over de hele wereld, is het mogelijk dat een afzonderlijke service nodig is voor elke regio die mogelijk kosten op elk van deze dupliceren.

Azure Search kunt voor de schaal van de afzonderlijke indexen en het totale aantal indexen groeien. Als een juiste prijzen laag hebt gekozen, partities en replica's kunnen worden toegevoegd aan de hele search-service wanneer een afzonderlijke index in de service te groot is in termen van opslag- of -verkeer.

Als het totale aantal indexen te voor één service groot is een andere service worden ingericht voor het opvangen van de nieuwe tenants. Als indexen worden verplaatst tussen de search-services moeten als nieuwe services worden toegevoegd, moet de gegevens van de index handmatig worden gekopieerd van een index naar de andere zoals Azure Search is niet toegestaan voor een index worden verplaatst.

## <a name="2-service-per-tenant"></a>2. Service per tenant
![Een portrayal van het model service per tenant](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

In een architectuur service per tenant heeft elke tenant een eigen search-service.

In dit model bereikt de toepassing het maximumniveau van isolatie voor de tenants. Elke service heeft toegewezen opslag en doorvoer voor het verwerken van zoekaanvraag, evenals een afzonderlijke API-sleutels.

Voor toepassingen waarbij elke tenant heeft een groot footprint of de werkbelasting variëren tenant tenant, is het model service per tenant een daadwerkelijke keuze als bronnen worden niet gedeeld door verschillende tenants werkbelastingen.

Een service per tenant model biedt ook het voordeel van een kostprijsmodel voorspelbare, vaste. Er is geen investeringen in een hele search-service tot er een tenant te vullen, is maar de kosten per tenant hoger dan het model van een index per tenant is.

Het model service per tenant is een efficiënte keuze voor toepassingen met een globale footprint. Met geografisch verspreide tenants is het gemakkelijk om elke tenant-service in de juiste regio.

De uitdagingen bij het schalen van dit patroon zich mogelijk aandienen wanneer individuele tenants langzamerhand hun service. Azure Search ondersteunt momenteel geen upgrade de prijscategorie van een service voor zoeken, zodat alle gegevens zou moeten handmatig worden gekopieerd naar een nieuwe service.

## <a name="3-mixing-both-models"></a>3. De combinatie van beide modellen
Een ander patroon voor het modelleren van multitenancy is de combinatie van strategieën voor zowel de index per tenant en de service per tenant.

Door een combinatie van de twee patronen, een toepassing grootste tenants in beslag kunnen nemen toegewezen services terwijl de lang staart minder actieve, kleinere tenants in beslag indexen in een gedeelde service nemen kan. Dit model zorgt ervoor dat de grootste tenants consistent hoge prestaties van de service bij het beveiligen van de kleinere tenants vanaf elke ruis neighbors.

Implementeren van deze strategie afhankelijk prognose voorspellen welke tenants wordt een specifieke service ten opzichte van een index in een gedeelde service nodig is. Met de noodzaak voor het beheren van zowel deze multitenancy modellen verhoogt de complexiteit van de toepassing.

## <a name="achieving-even-finer-granularity"></a>Achieving zelfs weer specifieker
De bovenstaande ontwerppatronen als model voor multitenant scenario's in Azure Search wordt ervan uitgegaan dat een uniform bereik, waarbij elke tenant een hele exemplaar van een toepassing is. Toepassingen kunnen echter soms verwerken veel kleinere bereiken.

Als service per tenant en index per tenant modellen niet voldoende kleine bereiken zijn, is het mogelijk om een index om te zorgen voor een zelfs fijner mate van granulatie te modelleren.

Als u een enkele index zich anders gedragen voor andere client-eindpunten, kan een veld worden toegevoegd aan een index waarmee wordt aangegeven dat een bepaalde waarde voor elke mogelijke client. Telkens wanneer een client roept Azure Search opvragen of wijzigen van een index met de code van de clienttoepassing wordt de juiste waarde voor dat veld met behulp van Azure-Search [filter](https://msdn.microsoft.com/library/azure/dn798921.aspx) mogelijkheden op het moment dat de query.

Deze methode kan worden gebruikt als u de functionaliteit van afzonderlijke gebruikersaccounts, aparte machtigingsniveaus en zelfs verschillende toepassingen.

> [!NOTE]
> Met de hierboven beschreven benadering voor het configureren van een enkele index voor meerdere tenants van invloed op de relevantie van zoekresultaten. Zoeken relevantie scores zijn berekend in een bereik van de index-niveau, niet op tenantniveau-bereik, dus alle huurders gegevens zijn opgenomen in de relevantie-scores onderliggende statistieken zoals term frequentie.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Azure Search is een aantrekkelijke keuze voor veel toepassingen [voor meer informatie over de krachtige mogelijkheden van de service](http://aka.ms/whatisazsearch). Overweeg bij het evalueren van de verschillende ontwerppatronen voor multitenant-toepassingen, de [verschillende Prijscategorieën](https://azure.microsoft.com/pricing/details/search/) en de bijbehorende [Servicelimieten](search-limits-quotas-capacity.md) om aan te passen beste Azure Search aanpassen aan de toepassing werkbelastingen en -architecturen van elke grootte.

Vragen over Azure Search en multitenant scenario's kunnen worden omgeleid naar azuresearch_contact@microsoft.com.

