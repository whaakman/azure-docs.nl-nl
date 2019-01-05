---
title: Buiten gebruik gestelde prestatieniveaus van Azure Cosmos DB
description: Meer informatie over de S1, S2 en S3 prestatieniveaus die eerder beschikbaar in Azure Cosmos DB.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: sngun
ms.openlocfilehash: 06fa98ae4acc2252d8866858ed0e2194ed84ff79
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54034383"
---
# <a name="retiring-the-s1-s2-and-s3-performance-levels"></a>Buiten gebruik stellen van de prestatieniveaus S1, S2 en S3

> [!IMPORTANT] 
> De prestatieniveaus S1, S2 en S3 besproken in dit artikel worden buiten gebruik gesteld en zijn niet meer beschikbaar voor nieuwe Azure Cosmos DB-accounts.
>

In dit artikel biedt een overzicht van de prestatieniveaus S1, S2 en S3 en wordt beschreven hoe de verzamelingen die gebruikmaken van deze prestatieniveaus gemigreerde met één gepartitioneerde verzamelingen kunnen worden. Na het lezen van dit artikel, zal het mogelijk om de volgende vragen te beantwoorden:

- [Waarom worden de prestatieniveaus S1, S2 en S3 zijn stopgezet?](#why-retired)
- [Hoe verzamelingen met één partitie en gepartitioneerde verzamelingen zich verhouden tot de S1, S2 en S3-prestaties?](#compare)
- [Wat moet ik doen om ervoor te zorgen ononderbroken toegang tot mijn gegevens?](#uninterrupted-access)
- [Hoe wordt mijn verzameling wijzigen na de migratie?](#collection-change)
- [Hoe wordt mijn facturering wijzigen nadat ik ben gemigreerd naar verzamelingen met één partitie?](#billing-change)
- [Wat gebeurt er als ik meer dan 10 GB aan opslagruimte nodig?](#more-storage-needed)
- [Kan ik wisselen tussen de S1, S2 en S3 prestatieniveaus vóór de geplande migratie?](#change-before)
- [Hoe Migreer ik van de S1, S2 en S3-prestatieniveaus voor verzamelingen met één partitie op mijn eigen?](#migrate-diy)
- [Hoe krijg ik als ik een EA-klant bent beïnvloed?](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-the-s1-s2-and-s3-performance-levels-being-retired"></a>Waarom zijn de prestaties S1, S2 en S3-niveaus buiten gebruik gesteld?

De prestatieniveaus S1, S2 en S3 bieden de flexibiliteit die de standaardaanbieding van de Azure Cosmos DB biedt. Met de S1, S2 en S3-prestaties, de doorvoer-en opslagcapaciteit zijn vooraf ingesteld en is niet aangeboden voor elasticiteit. Azure Cosmos DB biedt nu de mogelijkheid tot het aanpassen van de doorvoer en opslag, bieden u veel meer flexibiliteit in de mogelijkheid om te schalen wanneer uw behoeften veranderen.

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-to-the-s1-s2-s3-performance-levels"></a>Hoe verzamelingen met één partitie en gepartitioneerde verzamelingen zich verhouden tot de S1, S2 en S3-prestaties?

De volgende tabel vergelijkt de doorvoer en opslag opties die beschikbaar zijn in verzamelingen met één partitie, gepartitioneerde verzamelingen, en S1, S2, S3-prestatieniveau. Hier volgt een voorbeeld van de regio VS Oost 2:

|   |Gepartitioneerde verzameling|Verzameling met één partitie|S1|S2|S3|
|---|---|---|---|---|---|
|Maximumdoorvoer|Onbeperkt|10 K RU/s|250 RU/s|1 K RU/s|2.5 K RU/s|
|Minimale doorvoer|2.5 K RU/s|400 RU/s|250 RU/s|1 K RU/s|2.5 K RU/s|
|Maximale opslag|Onbeperkt|10 GB|10 GB|10 GB|10 GB|
|Prijs (maandelijks)|Doorvoer: $6 / 100 RU/s<br><br>Opslag: $ 0,25/GB|Doorvoer: $6 / 100 RU/s<br><br>Opslag: $ 0,25/GB|$25 USD|$50 USD|$100 USD|

Bent u een EA-klant? Als dit het geval is, raadpleegt u [hoe ben ik last van als ik een EA-klant ben?](#ea-customer)

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-to-do-to-ensure-uninterrupted-access-to-my-data"></a>Wat moet ik doen om ervoor te zorgen ononderbroken toegang tot mijn gegevens?

Als u een S1, S2 of S3-verzameling hebt, moet u de verzameling migreren naar een verzameling met één partitie programmatisch [met behulp van de .NET SDK](#migrate-diy). 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-the-migration"></a>Hoe wordt mijn verzameling wijzigen na de migratie?

Als u een S1-verzameling hebt, kunt u deze migreren naar een verzameling met één partitie met de doorvoer van 400 RU/s. 400 RU/s is de laagste doorvoer beschikbaar bij verzamelingen met één partitie. Echter, de kosten voor 400 RU/s in een verzameling met één partitie ongeveer hetzelfde is als u met de S1-verzameling en 250 RU/s: zijn betaalt, zodat u niet voor de extra 150 RU/s voor u beschikbaar betaalt.

Als u een S2-verzameling hebt, kunt u deze migreren naar een verzameling met één partitie met 1 K RU/s. Er is geen wijziging ziet u op het doorvoerniveau van uw.

Als u een S3-verzameling hebt, kunt u deze migreren naar een verzameling met één partitie met 2,5 K RU/s. Er is geen wijziging ziet u op het doorvoerniveau van uw.

In elk van deze gevallen is na de migratie van de verzameling, kunt u zich voor het aanpassen van uw doorvoerniveau of het omhoog en omlaag schalen naar behoefte met lage latentie om toegang te bieden aan uw gebruikers. 

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-i-migrated-to-the-single-partition-collections"></a>Hoe wordt mijn facturering wijzigen nadat ik gemigreerd naar de verzamelingen met één partitie?

Ervan uitgaande dat u 10 S1 verzamelingen, 1 GB aan opslagruimte voor elk daarvan in de regio VS-Oost, hebben en u deze 10 S1-verzamelingen migreren naar 10 verzamelingen met één partitie op 400 RU/sec. (het minimale niveau). Uw factuur ziet er als volgt als u de 10 verzamelingen met één partitie wilt voor een volledige maand houden:

![Hoe S1 prijzen voor 10 verzamelingen vergelijkt met 10 verzamelingen met behulp van de prijzen voor een verzameling met één partitie](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a>Wat gebeurt er als ik meer dan 10 GB aan opslagruimte nodig?

Of u hebt een verzameling met prestatieniveau S1, S2 of S3, of een verzameling met één partitie, die allemaal 10 GB aan opslagruimte beschikbaar is zijn, dat kunt u het hulpprogramma voor gegevensmigratie van Azure Cosmos DB uw gegevens te migreren naar een gepartitioneerde verzameling met vrijwel onbeperkte opslag. Zie voor meer informatie over de voordelen van een gepartitioneerde verzameling [partitioneren en schalen in Azure Cosmos DB](sql-api-partition-data.md). 

<a name="change-before"></a>

## <a name="can-i-change-between-the-s1-s2-and-s3-performance-levels-before-the-planned-migration"></a>Kan ik wisselen tussen de S1, S2 en S3 prestatieniveaus vóór de geplande migratie?

Alleen bestaande accounts met S1, S2 en S3-prestaties kunnen worden gewijzigd en niveau prestatielagen programmatisch alter [met behulp van de .NET SDK](#migrate-diy). Als u van S1-, S3- of S3 naar een verzameling met één partitie wijzigt, kunt u niet terug naar de prestatieniveaus S1, S2 of S3.

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-the-s1-s2-s3-performance-levels-to-single-partition-collections-on-my-own"></a>Hoe Migreer ik van de S1, S2 en S3-prestatieniveaus voor verzamelingen met één partitie op mijn eigen?

U kunt migreren vanaf de prestatieniveaus S1, S2 en S3 naar verzamelingen met één partitie programmatisch [met behulp van de .NET SDK](#migrate-diy). U kunt dit doen op uw eigen vóór de geplande migratie om te profiteren van de doorvoer van flexibele opties beschikbaar bij verzamelingen met één partitie.

### <a name="migrate-to-single-partition-collections-by-using-the-net-sdk"></a>Migreren naar verzamelingen met één partitie met behulp van de .NET SDK

In deze sectie heeft alleen betrekking op de prestaties van een verzameling wijzigen niveau met behulp van de [SQL .NET API](sql-api-sdk-dotnet.md), maar het proces is vergelijkbaar voor onze andere SDK's.

Hier volgt een codefragment voor het wijzigen van de verzamelingsdoorvoer naar 5000 aanvraageenheden per seconde:
    
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

Ga naar [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) extra voorbeelden bekijken en meer informatie over onze aanbieding methoden:

* [**ReadOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [**ReadOffersFeedAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [**ReplaceOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [**CreateOfferQuery**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a>Hoe krijg ik als ik een EA-klant bent beïnvloed?

EA-klanten worden beveiligd tot het einde van hun huidige contract prijs.

## <a name="next-steps"></a>Volgende stappen
De volgende bronnen voor meer informatie over prijzen en beheren van gegevens met Azure Cosmos DB:

1.  [Partitioneren van gegevens in Cosmos DB](sql-api-partition-data.md). Is het verschil tussen één partitie container en gepartitioneerde containers, evenals tips over het implementeren van een strategie voor partitionering naadloos schalen.
2.  [Prijzen van cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/). Meer informatie over de kosten van het inrichten van de doorvoer en opslag verbruikt.
3.  [Aanvraageenheden](request-units.md). Informatie over het gebruik van de doorvoer voor verschillende bewerkingstypen, bijvoorbeeld lezen, schrijven, query's.
