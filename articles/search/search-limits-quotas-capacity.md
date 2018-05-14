---
title: Servicelimieten in Azure Search | Microsoft Docs
description: Servicelimieten gebruikt voor het plannen van capaciteit en bovengrenzen voor aanvragen en antwoorden voor Azure Search.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/10/2018
ms.author: heidist
ms.openlocfilehash: 9fd046efd01281de6d5b46cca37d22a48671b1b2
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="service-limits-in-azure-search"></a>Servicelimieten in Azure Search
Maximum beperkt op opslag, workloads en aantallen van indexen, documenten en andere objecten afhankelijk zijn van of u [inrichten van Azure Search](search-create-service-portal.md) op **vrije**, **Basic**, of **Standaard** Prijscategorieën.

+ **Gratis** is een gedeelde multitenant-service die wordt geleverd met uw Azure-abonnement.

+ **Basic** biedt speciale computerbronnen voor productieworkloads op kleinere schaal.

+ **Standaard** wordt uitgevoerd op specifieke computers met meer capaciteit voor opslag en verwerking op elk niveau. Standaard wordt geleverd in vier niveaus: S1, S2, S3 en S3 HD.

  S3 High-Density (S3 HD) is ontworpen voor specifieke werkbelastingen: [multi-tenancymodus](search-modeling-multitenant-saas-applications.md) en grote hoeveelheden kleine indexen (een miljoen documenten per index, 3000-indexen per service). Deze laag biedt niet de [indexeerfunctie functie](search-indexer-overview.md). Op S3-HD moet gegevensopname gebruikmaken van de push-benadering, met behulp van API-aanroepen om gegevens te pushen van bron naar index. 

> [!NOTE]
> Een service is ingericht op een bepaalde laag. Categorieën voor het verkrijgen van capaciteit springen omvat het inrichten van een nieuwe service (Er is geen in-place upgrade). Zie voor meer informatie [Kies een SKU of laag](search-sku-tier.md). Zie voor meer informatie over het aanpassen van de capaciteit binnen een service die u al hebt ingericht, [schalen niveaus van de bron voor query's en workloads indexering](search-capacity-planning.md).
>

## <a name="subscription-limits"></a>Abonnementen
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="storage-limits"></a>Opslaglimieten
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

<a name="index-limits"></a>

## <a name="index-limits"></a>Limieten voor index

| Resource | Gratis | Basic&nbsp;<sup>1</sup>  | S1 | S2 | S3 | S3&nbsp;HD |
| -------- | ---- | ------------------- | --- | --- | --- | --- |
| Maximale aantal indexen |3 |5 of 15 |50 |200 |200 |1000 per partitie of 3000 per service |
| Maximum aantal velden per index |1000 |100 |1000 |1000 |1000 |1000 |
| Maximale scoreprofiel profielen per index |100 |100 |100 |100 |100 |100 |
| Maximale functies per profiel |8 |8 |8 |8 |8 |8 |

<sup>1</sup> basisservices nadat 2017 laat een hogere limiet van 15 indexen, gegevensbronnen en indexeerfuncties hebt gemaakt. Services die eerder hebt gemaakt hebben 5. Basic-lagen is de enige SKU met een lagere limiet van 100 velden per index.

## <a name="document-limits"></a>Document limieten 

In de meeste regio's Azure Search Prijscategorieën (Basic, S1, S2, S3, S3 HD) hebben onbeperkte document tellingen voor alle services gemaakt na November/December 2017. Deze sectie worden de regio's waar beperkingen gelden, en het bepalen of uw service wordt beïnvloed. 

Om te bepalen of uw service document limieten heeft, controleert u de tegel voor gebruik op de overzichtspagina van uw service. Telt het aantal documenten zijn onbeperkte of onderworpen aan een limiet op basis van de laag.

  ![Tegel gebruik](media/search-limits-quotas-capacity/portal-usage-tile.png)

### <a name="regions-and-services-having-document-limits"></a>Regio's en services die document limieten

Services limieten die zijn gemaakt voordat laat 2017 of op datacenters met behulp van clusters met lagere capaciteit voor het hosten van Azure Search-services worden uitgevoerd. Betrokken datacenters zijn in de volgende gebieden:

+ Australië - oost
+ Oost-Azië
+ Centraal-India
+ Japan - west
+ West-centraal VS

Voor services document beperkt gelden de volgende maxima:

|  Gratis | Basic | S1 | S2 | S3 | S3&nbsp;HD |
|-------|-------|----|----|----|-------|
|  10.000 |1 miljoen |15 miljoen per partitie of 180 miljoen per service |60 miljoen per partitie of 720 miljoen per service |120 miljoen per partitie of 1,4 miljard per service |1 miljoen per index of 200 miljoen per partitie |

> [!Note] 
> Voor S3 high-density services gemaakt nadat laat 2017, is het document 200 miljoen per partitie verwijderd maar het document 1 miljoen per index limiet blijft.


### <a name="document-size-limits-per-api-call"></a>Documenteer de maximale grootte per API-aanroep

Maximale grootte van het document bij het aanroepen van een Index-API is ongeveer 16 MB.

De documentgrootte van het is daadwerkelijk een limiet van de grootte van de aanvraagtekst Index API. Omdat u een batch met meerdere documenten tegelijk aan de API-Index doorgeven kunt, afhankelijk de maximale grootte in de praktijk van het aantal documenten in de batch zijn. Voor een batch met één document is de grootte van de maximale document 16 MB van JSON.

Vergeet niet uitsluiten van niet-waarop gegevens van de aanvraag om de documentgrootte van het beperken. Afbeeldingen en andere binaire gegevens zijn niet rechtstreeks waarop en mag niet worden opgeslagen in de index. Niet-query om gegevens te integreren zoekresultaten, definieert u een niet-doorzoekbare veld dat een URL-verwijzing naar de resource opgeslagen.

## <a name="indexer-limits"></a>Indexeerfunctie limieten

Basic services die zijn gemaakt na laat 2017 hebben een hogere limiet van 15 indexen, gegevensbronnen, vaardigheden en indexeerfuncties.

| Resource | Gratis&nbsp;<sup>1</sup> | Basic&nbsp;<sup>2</sup>| S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>3</sup>|
| -------- | ----------------- | ----------------- | --- | --- | --- | --- |
| Maximale aantal indexeerfuncties |3 |5 of 15|50 |200 |200 |N/A |
| Maximale aantal gegevensbronnen |3 |5 of 15 |50 |200 |200 |N/A |
| Maximale vaardigheden |3 |5 of 15 |50 |200 |200 |N/A |
| Maximale indexering belasting per aanroep |10.000 documenten |Alleen beperkt door het maximum aantal documenten |Alleen beperkt door het maximum aantal documenten |Alleen beperkt door het maximum aantal documenten |Alleen beperkt door het maximum aantal documenten |N/A |
| Maximale uitvoeringstijd | 1-3 minuten |24 uur |24 uur |24 uur |24 uur |N/A  |
| BLOB-indexeerfunctie: maximale blobgrootte, MB |16 |16 |128 |256 |256 |N/A  |
| BLOB-indexeerfunctie: maximum aantal tekens inhoud opgehaald uit een blob |32,000 |64,000 |4 miljoen |4 miljoen |4 miljoen |N/A |

<sup>1</sup> gratis services hebben een indexeerfunctie maximale uitvoeringstijd van de 3 minuten voor blob-bronnen en 1 minuut voor andere gegevensbronnen.

<sup>2</sup> basisservices nadat 2017 laat een hogere limiet van 15 indexen, gegevensbronnen en indexeerfuncties hebt gemaakt. Services die eerder hebt gemaakt hebben 5.

<sup>3</sup> S3 HD services omvatten geen indexeerfunctie ondersteuning.

## <a name="queries-per-second-qps"></a>Query's per seconde (QPS)

Maakt een schatting van QPS moeten onafhankelijk zijn ontwikkeld door elke klant. De grootte van de index en complexiteit, de grootte van de query en complexiteit en de hoeveelheid verkeer zijn primaire factoren van QPS. Er is geen manier voor een zinvolle maakt een schatting bij factoren onbekend zijn.

Er zijn geen schattingen beter voorspelbaar wanneer berekend op services die worden uitgevoerd op specifieke bronnen (lagen Basic en Standard). U kunt meer QPS nauw schatten omdat u controle over meer van de parameters hebt. Zie voor instructies over het benadering schatting [Azure Search-prestaties en optimalisatie](search-performance-optimization.md).

## <a name="api-request-limits"></a>Limieten voor API-aanvraag
* Maximum van 16 MB per aanvraag <sup>1</sup>
* Maximale URL-lengte van 8 KB
* Maximaal 1000 documenten per batch van index uploadt, worden samengevoegd of verwijderd
* Maximaal 32 velden in de component $orderby
* Maximale zoeken Termgrootte is 32.766 bytes (32 KB min 2-bytes) van UTF-8-gecodeerde tekst

<sup>1</sup> in Azure Search de hoofdtekst van een aanvraag is onderworpen aan de bovengrens van 16 MB, om een limiet van de inhoud van afzonderlijke velden of verzamelingen die anders niet worden beperkt door theoretische limieten te leggen (Zie [gegevens ondersteund typen](https://msdn.microsoft.com/library/azure/dn798938.aspx) voor meer informatie over het veld samenstelling en beperkingen).

## <a name="api-response-limits"></a>API-reactie limieten
* Maximaal 1000 documenten die worden geretourneerd per pagina met zoekresultaten
* Maximaal 100 suggesties heeft geretourneerd per voorstellen API-aanvraag

## <a name="api-key-limits"></a>Limieten voor API-sleutel
API-sleutels worden gebruikt voor verificatie van de service. Er zijn twee typen. Administratorsleutels zijn opgegeven in de aanvraagheader en lezen-schrijven toegang verlenen tot de service. Querysleutels zijn alleen-lezen, opgegeven in de URL en doorgaans verleend aan clienttoepassingen.

* Maximaal 2 administratorsleutels per service
* Maximum van 50 querysleutels per service
