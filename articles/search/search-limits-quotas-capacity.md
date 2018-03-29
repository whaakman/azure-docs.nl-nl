---
title: Servicelimieten in Azure Search | Microsoft Docs
description: Servicelimieten gebruikt voor het plannen van capaciteit en bovengrenzen voor aanvragen en antwoorden voor Azure Search.
services: search
documentationcenter: ''
author: HeidiSteen
manager: jhubbard
editor: ''
tags: azure-portal
ms.assetid: 857a8606-c1bf-48f1-8758-8032bbe220ad
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 03/26/2018
ms.author: heidist
ms.openlocfilehash: fb2234e79e8deb98a94068f31a40c8f0b415d7ba
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="service-limits-in-azure-search"></a>Servicelimieten in Azure Search
Maximum beperkt op opslag, workloads en aantallen van indexen, documenten en andere objecten afhankelijk zijn van of u [inrichten van Azure Search](search-create-service-portal.md) op een **vrije**, **Basic**, of **standaard** prijscategorie.

* **Gratis** is een gedeelde multitenant-service die wordt geleverd met uw Azure-abonnement. 
* **Basic** biedt speciale computerbronnen voor productieworkloads op kleinere schaal.
* **Standaard** wordt uitgevoerd op specifieke computers met meer capaciteit voor opslag en verwerking op elk niveau. Standaard wordt geleverd in vier niveaus: S1, S2, S3 en S3 High-Density (S3 HD).

> [!NOTE]
> Een service is ingericht op een bepaalde laag. Categorieën voor het verkrijgen van capaciteit springen omvat het inrichten van een nieuwe service (Er is geen in-place upgrade). Zie voor meer informatie [Kies een SKU of laag](search-sku-tier.md). Zie voor meer informatie over het aanpassen van de capaciteit binnen een service die u al hebt ingericht, [schalen niveaus van de bron voor query's en workloads indexering](search-capacity-planning.md).
>

## <a name="subscription-limits"></a>Abonnementen
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="service-limits"></a>Servicelimieten
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

## <a name="index-limits"></a>Limieten voor index

| Resource | Gratis | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Maximum aantal velden per index |1000 |100 <sup>1</sup> |1000 |1000 |1000 |1000 |
| Maximale scoreprofiel profielen per index |100 |100 |100 |100 |100 |100 |
| Maximale functies per profiel |8 |8 |8 |8 |8 |8 |

<sup>1</sup> basisstaffel is de enige SKU met een lagere limiet van 100 velden per index.

## <a name="indexer-limits"></a>Indexeerfunctie limieten

| Resource | Gratis | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Maximale indexering belasting per aanroep |10.000 documenten |Alleen beperkt door het maximum aantal documenten |Alleen beperkt door het maximum aantal documenten |Alleen beperkt door het maximum aantal documenten |Alleen beperkt door het maximum aantal documenten |N.V.T. <sup>1</sup> |
| Maximale uitvoeringstijd | 1-3 minuten <sup>2</sup> |24 uur |24 uur |24 uur |24 uur |N.V.T. <sup>1</sup> |
| BLOB-indexeerfunctie: maximale blobgrootte, MB |16 |16 |128 |256 |256 |N.V.T. <sup>1</sup> |
| BLOB-indexeerfunctie: maximum aantal tekens inhoud opgehaald uit een blob |32,000 |64,000 |4 miljoen |4 miljoen |4 miljoen |N.V.T. <sup>1</sup> |

<sup>1</sup> S3 HD ondersteunt momenteel geen indexeerfuncties. Neem contact op met ondersteuning van Azure als u deze mogelijkheid dringend behoefte hebt.

<sup>2</sup> indexeerfunctie maximale uitvoeringstijd voor de laag gratis is 3 minuten voor blob-bronnen en 1 minuut voor andere gegevensbronnen.


## <a name="document-size-limits"></a>De maximale grootte document
| Resource | Gratis | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Grootte van afzonderlijke document per Index API |<16 MB |<16 MB |<16 MB |<16 MB |<16 MB |<16 MB |

Verwijst naar de grootte van de maximale document bij het aanroepen van een API-Index. De documentgrootte van het is daadwerkelijk een limiet van de grootte van de aanvraagtekst Index API. Omdat u een batch met meerdere documenten tegelijk aan de API-Index doorgeven kunt, wordt de maximale grootte daadwerkelijk hangt af van het aantal documenten in de batch zijn. Voor een batch met één document is de grootte van de maximale document 16 MB van JSON.

Vergeet niet uitsluiten van niet-waarop gegevens van de aanvraag om de documentgrootte van het beperken. Afbeeldingen en andere binaire gegevens zijn niet rechtstreeks waarop en mag niet worden opgeslagen in de index. Niet-query om gegevens te integreren zoekresultaten, definieert u een niet-doorzoekbare veld dat een URL-verwijzing naar de resource opgeslagen.

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
