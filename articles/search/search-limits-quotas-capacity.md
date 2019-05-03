---
title: Servicelimieten voor lagen en SKU's - Azure Search
description: Servicelimieten gebruikt voor het plannen van capaciteit en maximumlimieten op aanvragen en antwoorden voor Azure Search.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 419c533aabd67637efa64777387c491dd890596e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024644"
---
# <a name="service-limits-in-azure-search"></a>Servicelimieten in Azure Search
Maximale limieten voor opslag, workloads en aantallen indexen, documenten, en andere objecten afhankelijk zijn van of u [inrichten van Azure Search](search-create-service-portal.md) op **gratis**, **Basic**,  **Standard**, of **met geoptimaliseerde opslag** Prijscategorieën.

+ **Gratis** is een multitenant gedeelde service die wordt geleverd met uw Azure-abonnement.

+ **Basic** biedt speciale computerbronnen voor productieworkloads op kleinere schaal.

+ **Standard** wordt uitgevoerd op toegewezen virtuele machines met meer capaciteit voor opslag en verwerking op ieder niveau. Er is standaard beschikbaar in vier niveaus: S1, S2, S3 en S3 HD.

+ **Opslag geoptimaliseerd** wordt uitgevoerd op toegewezen virtuele machines met meer totale opslag, opslag, bandbreedte en geheugen dan **Standard**. Opslag geoptimaliseerd wordt geleverd met twee niveaus: L1 en L2

> [!NOTE]
> De Servicelagen met geoptimaliseerde opslag zijn momenteel beschikbaar als preview op kortingsprijzen voor testen en experimenteren met het doel van het verzamelen van feedback. Laatste prijzen worden later aangekondigd wanneer deze lagen algemeen beschikbaar zijn. We raden u aan die deze lagen gebruiken voor productie-Apps.

  S3 High Density (S3 HD) is ontworpen voor specifieke werkbelastingen: [multitenancy](search-modeling-multitenant-saas-applications.md) en grote hoeveelheden kleine indexen (1 miljoen documenten per index, drie duizend indexen per service). Deze laag biedt de [indexeerfunctie functie](search-indexer-overview.md). Opname van gegevens moet gebruikmaken van de push-benadering, met behulp van API-aanroepen om gegevens te pushen van bron naar een index op S3 HD. 

> [!NOTE]
> Een service wordt op een specifieke laag ingericht. Lagen te krijgen van de capaciteit te springen omvat het inrichten van een nieuwe service (Er is geen in-place upgrade). Zie voor meer informatie, [Kies een SKU of laag](search-sku-tier.md). Zie voor meer informatie over het aanpassen van capaciteit binnen een service die u al hebt ingericht, [resource niveaus voor query's en indexering workloads schalen](search-capacity-planning.md).
>

## <a name="subscription-limits"></a>Abonnementslimieten
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="storage-limits"></a>Maximale opslag
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

<a name="index-limits"></a>

## <a name="index-limits"></a>Index-limieten

| Resource | Gratis | Basic&nbsp;<sup>1</sup>  | S1 | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| -------- | ---- | ------------------- | --- | --- | --- | --- | --- | --- |
| Maximale aantal indexen |3 |5 of 15 |50 |200 |200 |1000 per partitie of 3000 per service |10 |10 |
| Maximale eenvoudige velden per index |1000 |100 |1000 |1000 |1000 |1000 |1000 |1000 |
| Maximum aantal complexe verzameling velden per index |40 |40 |40 |40 |40 |40 |40 |40 |
| Maximum aantal elementen voor alle complexe verzamelingen per document |3000 |3000 |3000 |3000 |3000 |3000 |3000 |3000 |
| Maximale diepte van complexe velden |10 |10 |10 |10 |10 |10 |10 |10 |
| Maximale [suggesties](https://docs.microsoft.com/rest/api/searchservice/suggesters) per index |1 |1 |1 |1 |1 |1 |1 |1 |
| Maximale [scoreprofielen](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) per index |100 |100 |100 |100 |100 |100 |100 |100 |
| Maximumaantal functies per profiel |8 |8 |8 |8 |8 |8 |8 |8 |

<sup>1</sup> eenvoudige services die zijn gemaakt vóór December 2017 hebben lagere limieten (5 in plaats van 15) over indexen. Basic-laag is de enige SKU met een lagere limiet van 100 velden per index.

<a name="document-limits"></a>

## <a name="document-limits"></a>Document-limieten 

Vanaf oktober 2018 zijn er niet langer een document-limieten voor elke nieuwe service die is gemaakt op een factureerbare laag (Basic, S1, S2, S3, S3 HD) in elke regio. Hoewel de meeste regio's hadden onbeperkt aantal documenten telt sinds November/December 2017, zijn er vijf regio's die verder in te stellen document limieten. Afhankelijk van waar en wanneer u een search-service hebt gemaakt, kunt u een service die wordt nog steeds worden beperkt door de document worden uitgevoerd.

Om te bepalen of uw service document limiet een, controleert u de tegel Usage in de overzichtspagina van uw service. Telt het aantal documenten worden onbeperkt of geldt een limiet op basis van de laag.

  ![Gebruikstegel](media/search-limits-quotas-capacity/portal-usage-tile.png)

### <a name="regions-previously-having-document-limits"></a>Regio's eerder met document-limieten

Als de portal wordt aangegeven de Documentlimiet van een dat, wordt uw service is gemaakt vóór eind 2017 of deze is gemaakt op een datacenter met lagere capaciteit-clusters voor het hosten van Azure Search-services:

+ Australië - oost
+ Azië - oost
+ India - centraal
+ Japan - west
+ US - west-centraal

Voor services beperkt het document de volgende maximale limieten gelden:

|  Gratis | Basic | S1 | S2 | S3 | S3&nbsp;HD |
|-------|-------|----|----|----|-------|
|  10.000 |1&nbsp;miljoen |15 miljoen per partitie of 180 miljoen per service |60 miljoen per partitie of 720 miljoen per service |120 miljoen per partitie of 1,4 miljard per service |1 miljoen per index of 200 miljoen per partitie |

Als uw service limiet die u blokkeren een, een nieuwe service maken en alle inhoud die service vervolgens opnieuw te publiceren. Er is geen mechanisme voor het naadloos beëindiging van uw service op de nieuwe hardware op de achtergrond.

> [!Note] 
> Voor S3 high-density-services die zijn gemaakt na laat 2017, is het document 200 miljoen per partitie verwijderd, maar het document van 1 miljoen per index limiet blijven.


### <a name="document-size-limits-per-api-call"></a>Documenteer de maximale grootte per API-aanroep

Maximale grootte van het document bij het aanroepen van een Index-API is ongeveer 16 MB.

Documentgrootte van het is eigenlijk een limiet voor de grootte van de hoofdtekst van de Index API-aanvraag. Omdat u in één keer een batch van meerdere documenten aan de API-Index doorgeven kunt, afhankelijk de maximale grootte in de praktijk zijn van het aantal documenten in de batch zijn. Voor een batch met één document is de maximale documentgrootte 16 MB van JSON.

Houd er rekening mee om te voorkomen dat documentgrootte omlaag, moeten worden uitgesloten om gegevens van de aanvraag. Afbeeldingen en andere binaire gegevens zijn niet rechtstreeks waarin u kunt zoeken en mag niet worden opgeslagen in de index. Als u wilt integreren om gegevens in de zoekresultaten, definieert u een niet-doorzoekbaar veld waarin een URL-verwijzing naar de resource.

## <a name="indexer-limits"></a>Indexeerfunctie limieten

Maximum aantal actieve keren herhaald bestaan voor het saldo en stabiliteit naar de service als geheel, maar grotere gegevenssets mogelijk meer indexering tijd dan is toegestaan door de maximale nodig hebt. Als een indexeringstaak kan niet worden voltooid binnen de maximale tijd die is toegestaan, probeert u deze volgens een schema uitvoert. De scheduler houdt van Indexstatus. Als een geplande taak voor indexering is onderbroken voor een bepaalde reden, de indexeerfunctie verder kan gaan waar het laatste bij de volgende geplande run afgebroken.


| Resource | Gratis&nbsp;<sup>1</sup> | Basic&nbsp;<sup>2</sup>| S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>3</sup>|L1 |L2 |
| -------- | ----------------- | ----------------- | --- | --- | --- | --- | --- | --- |
| Maximale aantal indexeerfuncties |3 |5 of 15|50 |200 |200 |N/A |10 |10 |
| Maximale aantal gegevensbronnen |3 |5 of 15 |50 |200 |200 |N/A |10 |10 |
| Maximale kennis en vaardigheden <sup>4</sup> |3 |5 of 15 |50 |200 |200 |N/A |10 |10 |
| Indexering maximumbelasting per aanroep |10.000 documenten |Alleen beperkt door het maximale aantal documenten |Alleen beperkt door het maximale aantal documenten |Alleen beperkt door het maximale aantal documenten |Alleen beperkt door het maximale aantal documenten |N/A |Geen limiet |Geen limiet |
| Minimale plannen | 5 minuten |5 minuten |5 minuten |5 minuten |5 minuten |5 minuten |5 minuten | 5 minuten |
| Maximale uitvoeringstijd <sup>5</sup> | 1-3 minuten |24 uur |24 uur |24 uur |24 uur |N/A  |24 uur |24 uur |
| Maximale uitvoeringstijd voor cognitief zoeken kennis en vaardigheden of blob-indexering met analyse van de afbeelding <sup>5</sup> | 3-10 minuten |2 uur |2 uur |2 uur |2 uur |N/A  |2 uur |2 uur |
| Indexeerfunctie voor BLOB: maximum blobgrootte, MB |16 |16 |128 |256 |256 |N/A  |256 |256 |
| Indexeerfunctie voor BLOB: maximum aantal tekens van de inhoud opgehaald uit een blob |32,000 |64,000 |4&nbsp;miljoen |4&nbsp;miljoen |4&nbsp;miljoen |N/A |4&nbsp;miljoen |4&nbsp;miljoen |

<sup>1</sup> gratis services hebben een maximale uitvoeringstijd van 3 minuten indexeerfunctie voor blob-bronnen en 1 minuut voor alle andere gegevensbronnen. Voor AI indexeren die aanroepen naar Cognitive Services, zijn gratis services beperkt tot 20 gratis transacties per dag, waarbij een transactie is gedefinieerd als een document dat is doorgegeven via de pijplijn verrijking.

<sup>2</sup> eenvoudige services die zijn gemaakt vóór December 2017 hebben lagere limieten (5 in plaats van 15) op Indexeerfuncties en gegevensbronnen kennis en vaardigheden.

<sup>3</sup> S3 HD-services omvatten geen ondersteuning voor indexeerfuncties.

<sup>4</sup> maximaal 30 vaardigheden per vaardigheden.

<sup>5</sup> cognitief zoeken workloads en analyse van de afbeelding in Azure blob-indexering hebben kortere tijden dan het indexeren van gewone tekst. Analyse van de afbeelding en verwerking van natuurlijke taal voor rekenintensieve en buitensporige hoeveelheden beschikbaar verwerkingskracht in beslag nemen. Uitvoeringstijd is verkleind zodat andere taken in de wachtrij een kans om uit te voeren.  

## <a name="queries-per-second-qps"></a>Query's per seconde (QPS)

Maakt een schatting QPS moeten onafhankelijk van elkaar zijn ontwikkeld door elke klant. Zijn de primaire factoren van QPS indexgrootte en complexiteit, query-grootte en complexiteit en de hoeveelheid verkeer. Er is geen manier voor een zinvolle schattingen bij deze factoren zijn onbekend.

Er zijn geen schattingen beter te voorspellen wanneer berekend op services die worden uitgevoerd op specifieke resources (Basic en Standard-lagen). U kunt meer QPS nauw schatten omdat u controle over meer van de parameters hebt. Zie voor instructies over het benadering schatting [Azure Search-prestaties en optimalisatie](search-performance-optimization.md).

Voor de lagen met geoptimaliseerde opslag, moet u verwachten dat een lagere query-doorvoer en een hogere latentie dan de standaard-laag.  De methodologie voor het schatten van de prestaties van query's die u zult ervaren is hetzelfde als de standaard-laag.

## <a name="data-limits-cognitive-search"></a>Gegevenslimieten (cognitief zoeken)

Een [cognitief zoeken pijplijn](cognitive-search-concept-intro.md) waardoor aanroepen naar een Text Analytics-resource voor [entiteit erkenning](cognitive-search-skill-entity-recognition.md), [sleutel vindt er sleuteltermextractie plaats](cognitive-search-skill-keyphrases.md), [sentimentanalyse ](cognitive-search-skill-sentiment.md), en [taaldetectie](cognitive-search-skill-language-detection.md) is onderhevig aan de limieten van gegevens. De maximale grootte van een record moet tussen de 50.000 tekens wordt gemeten door `String.Length`. Als u splitst u uw gegevens wilt voordat deze worden verzonden naar de analyzer sentiment, gebruik de [tekst splitsen vaardigheid](cognitive-search-skill-textsplit.md).

## <a name="api-request-limits"></a>API-limieten voor aanvraag
* Maximaal 16 MB per aanvraag <sup>1</sup>
* Maximale URL-lengte van 8 KB
* Maximaal 1000 documenten per batch van index wordt geüpload, worden samengevoegd of verwijderd
* Maximaal 32 velden in de component $orderby
* Maximale zoeken Termgrootte is 32.766 bytes (32 KB min 2 bytes) van UTF-8-gecodeerde tekst

<sup>1</sup> in Azure Search de hoofdtekst van een aanvraag is onderworpen aan een bovengrens van 16 MB, waarbij een praktische limiet op de inhoud van afzonderlijke velden of verzamelingen die zijn niet anders is beperkt door theoretische limieten (Zie [gegevens ondersteund typen](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) voor meer informatie over het veld samenstelling en beperkingen).

## <a name="api-response-limits"></a>API-reactie-limieten
* Maximaal 1000 documenten per pagina met zoekresultaten geretourneerd
* Maximaal 100 suggesties per voorstellen API-aanvraag geretourneerd

## <a name="api-key-limits"></a>API-sleutel limieten
API-sleutels worden gebruikt voor service-verificatie. Er zijn twee typen. Beheersleutels zijn opgegeven in de aanvraagheader en lezen-schrijven toegang verlenen tot de service. Querysleutels zijn alleen-lezen, opgegeven in de URL en doorgaans verleend aan clienttoepassingen.

* Maximaal 2 beheersleutels per service
* Maximum van 50 querysleutels per service
