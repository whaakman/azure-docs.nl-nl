---
title: Buiten gebruik gesteld Azure Cosmos DB prestatieniveaus | Microsoft Docs
description: Meer informatie over de S1, S2 en S3 prestatieniveaus eerder beschikbaar zijn in Azure Cosmos DB.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: sngun
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d1bb7551e6dfb6c42853ab95096f17f5285c69c1
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796645"
---
# <a name="retiring-the-s1-s2-and-s3-performance-levels"></a>Buiten gebruik stellen van de prestaties S1, S2 en S3

> [!IMPORTANT] 
> De S1, S2 en S3 prestatieniveaus besproken in dit artikel worden buiten gebruik gesteld en zijn niet meer beschikbaar voor de nieuwe Azure DB die Cosmos-accounts.
>

Dit artikel biedt een overzicht van S1, S2 en S3 prestatieniveaus en wordt beschreven hoe de verzamelingen die gebruikmaken van deze prestatieniveaus gemigreerde met één gepartitioneerde verzamelingen kunnen worden. Na het lezen van dit artikel, hebt u mogelijk de volgende vragen beantwoorden:

- [Waarom worden de prestaties S1, S2 en S3 worden buiten gebruik gesteld?](#why-retired)
- [Hoe verzamelingen met één partitie en gepartitioneerde verzamelingen Vergelijk aan de S1 S2 S3 prestatieniveaus?](#compare)
- [Wat moet ik doen om ervoor te zorgen dat toegang tot mijn gegevens?](#uninterrupted-access)
- [Hoe wordt mijn verzameling wijzigen na de migratie?](#collection-change)
- [Hoe wordt mijn facturering wijzigen nadat ik ben gemigreerd naar verzamelingen met één partitie?](#billing-change)
- [Wat gebeurt er als ik meer dan 10 GB aan opslagruimte nodig?](#more-storage-needed)
- [Kan ik wijzigen tussen de S1, S2 en S3 prestatieniveaus vóór de geplande migratie?](#change-before)
- [Hoe Migreer ik van de S1, S2 S3 prestatieniveaus om verzamelingen met één partitie zelf?](#migrate-diy)
- [Hoe ben ik als ik een klant EA beïnvloed?](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-the-s1-s2-and-s3-performance-levels-being-retired"></a>Waarom worden de prestaties S1, S2 en S3 niveaus buiten gebruik gesteld?

De prestaties S1, S2 en S3 bieden niet de flexibiliteit die de standaard Azure Cosmos DB aanbieding biedt. Met de S1, S2, S3 prestatieniveaus, de doorvoer en de opslagcapaciteit vooraf zijn ingesteld en is niet aangeboden elasticiteit. Azure Cosmos DB biedt nu de mogelijkheid voor het aanpassen van de doorvoer en opslag, biedt veel meer flexibiliteit in de mogelijkheid om te worden geschaald, zoals het wijzigen van uw behoeften.

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-to-the-s1-s2-s3-performance-levels"></a>Hoe verzamelingen met één partitie en gepartitioneerde verzamelingen Vergelijk aan de S1 S2 S3 prestatieniveaus?

De volgende tabel vergelijkt de doorvoer en opslag beschikbare opties in verzamelingen met één partitie, gepartitioneerde verzamelingen en S1, S2, S3 prestatieniveaus. Hier volgt een voorbeeld voor de regio VS Oost 2:

|   |Gepartitioneerde verzameling|Verzameling van één partitie|S1|S2|S3|
|---|---|---|---|---|---|
|Maximale doorvoer|Onbeperkt|10 K RU/s|250 RU/s|1 K RU/s|2.5 K RU/s|
|Minimale doorvoer|2.5 K RU/s|400 RU/s|250 RU/s|1 K RU/s|2.5 K RU/s|
|Maximale opslag|Onbeperkt|10 GB|10 GB|10 GB|10 GB|
|Prijs (maandelijks)|Doorvoer: $6 / 100 RU/s<br><br>Opslag: $ 0,25/GB|Doorvoer: $6 / 100 RU/s<br><br>Opslag: $ 0,25/GB|25 USD $|50 USD $|100 USD $|

Weet u een klant EA? Zo ja, Zie [hoe ben ik beïnvloed als ik een klant EA?](#ea-customer)

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-to-do-to-ensure-uninterrupted-access-to-my-data"></a>Wat moet ik doen om ervoor te zorgen dat toegang tot mijn gegevens?

Als u een verzameling S1, S2 of S3 hebt, moet u de verzameling migreren naar een verzameling van één partitie programmatisch [met behulp van de .NET SDK](#migrate-diy). 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-the-migration"></a>Hoe wordt mijn verzameling wijzigen na de migratie?

Als u een verzameling S1 hebt, kunt u ze kunt migreren naar een verzameling van één partitie met 400 RU/s-doorvoer. 400 RU/s is de laagste doorvoer beschikbaar met verzamelingen met één partitie. Echter, de kosten voor 400 RU/s in een verzameling van één partitie is ongeveer hetzelfde als u met uw verzameling S1 en 250 RU/s – zijn betaalt, zodat u niet voor extra 150 RU/s. voor u beschikbaar betalen.

Als u een verzameling S2 hebt, kunt u ze kunt migreren naar een verzameling van één partitie met 1 K RU/s. Er zijn geen wijzigingen ziet u op het doorvoerniveau.

Als u een verzameling S3 hebt, kunt u ze kunt migreren naar een verzameling van één partitie met 2,5 K RU/s. Er zijn geen wijzigingen ziet u op het doorvoerniveau.

In elk van deze gevallen, nadat u de verzameling migreert, kunt u zich kunnen het doorvoerniveau van uw aanpassen of het omhoog en omlaag geschaald naar behoefte lage latentie om toegang te bieden aan uw gebruikers. 

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-i-migrated-to-the-single-partition-collections"></a>Hoe wordt mijn facturering wijzigen nadat ik gemigreerd naar de verzamelingen met één partitie?

Ervan uitgaande dat u hebt 10 S1 verzamelingen, 1 GB aan opslagruimte voor elk in de regio VS-Oost, en u deze 10 S1 verzamelingen migreren naar verzamelingen met één partitie 10 op 400 RU per seconde (minimaal niveau). Uw factuur wordt er als volgt uitzien als u de 10 verzamelingen met één partitie voor een volledige maand behouden:

![Hoe S1 prijzen voor 10 verzamelingen vergelijkt met 10 verzamelingen met prijzen voor een verzameling van één partitie](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a>Wat gebeurt er als ik meer dan 10 GB aan opslagruimte nodig?

Of u een verzameling met prestatieniveau S1, S2 of S3 of een verzameling van één partitie, die allemaal 10 GB aan opslagruimte beschikbaar is, dat kunt u het hulpprogramma voor migratie van Azure Cosmos DB gegevens voor het migreren van uw gegevens naar een gepartitioneerde verzameling met vrijwel hebben onbeperkte opslag. Zie voor meer informatie over de voordelen van een gepartitioneerde verzameling [partitionering en schalen in Azure Cosmos DB](sql-api-partition-data.md). 

<a name="change-before"></a>

## <a name="can-i-change-between-the-s1-s2-and-s3-performance-levels-before-the-planned-migration"></a>Kan ik wijzigen tussen de S1, S2 en S3 prestatieniveaus vóór de geplande migratie?

Alleen bestaande accounts met S1, S2 en S3 prestaties kunnen worden gewijzigd en niveau prestatielagen programmatisch alter [met behulp van de .NET SDK](#migrate-diy). Als u vanuit S1, S3 of S3 voor een verzameling van één partitie wijzigt, kunt u niet terug naar de prestatieniveaus S1, S2 of S3.

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-the-s1-s2-s3-performance-levels-to-single-partition-collections-on-my-own"></a>Hoe Migreer ik van de S1, S2 S3 prestatieniveaus om verzamelingen met één partitie zelf?

U kunt migreren vanaf de prestatieniveaus S1, S2 en S3 naar verzamelingen met één partitie programmatisch [met behulp van de .NET SDK](#migrate-diy). U kunt dit doen op uw eigen vóór de geplande migratie profiteren van de flexibele doorvoer beschikbare opties bij verzamelingen met één partitie.

### <a name="migrate-to-single-partition-collections-by-using-the-net-sdk"></a>Migreren naar verzamelingen met één partitie met behulp van de .NET SDK

Deze sectie bevat alleen de prestaties van een verzameling wijzigen niveau met behulp van de [SQL .NET API](sql-api-sdk-dotnet.md), maar het proces is vergelijkbaar voor onze andere SDK.

Hier volgt een codefragment voor het wijzigen van de verzameling doorvoer in 5000 aanvraageenheden per seconde:
    
```csharp
    //Fetch the resource to be updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set the throughput to 5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes to the database by replacing the original resource
    await client.ReplaceOfferAsync(offer);
```

Ga naar [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) aanvullende voorbeelden gegeven weergeven en meer informatie over onze aanbieding methoden:

* [**ReadOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [**ReadOffersFeedAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [**ReplaceOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [**CreateOfferQuery**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a>Hoe ben ik als ik een klant EA beïnvloed?

EA klanten, prijs tot het einde van hun huidige contract beschermd worden.

## <a name="next-steps"></a>Volgende stappen
Lees deze informatiebronnen voor meer informatie over prijzen en -gegevens met Azure Cosmos DB beheren:

1.  [Partitioneren van gegevens in Cosmos DB](sql-api-partition-data.md). Het verschil tussen één partitie container en gepartitioneerde containers, evenals tips voor het implementeren van een strategie voor partitionering probleemloos schalen.
2.  [Prijzen voor cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/). Meer informatie over de kosten van de inrichting van de doorvoer en opslag gebruiken.
3.  [Aanvraageenheden](request-units.md). Het verbruik van doorvoer voor een andere bewerkingstypen, bijvoorbeeld lezen, schrijven, Query begrijpen.
