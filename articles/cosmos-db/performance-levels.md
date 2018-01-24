---
title: Buiten gebruik gesteld Azure Cosmos DB prestatieniveaus | Microsoft Docs
description: Meer informatie over de S1, S2 en S3 prestatieniveaus eerder beschikbaar zijn in Azure Cosmos DB.
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 7dc21c71-47e2-4e06-aa21-e84af52866f4
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4cab6297e45dc7a14826d7bb10cf22a8a300355f
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="retiring-the-s1-s2-and-s3-performance-levels"></a>Buiten gebruik stellen van de prestaties S1, S2 en S3

> [!IMPORTANT] 
> De S1, S2 en S3 prestatieniveaus besproken in dit artikel worden buiten gebruik gesteld en zijn niet meer beschikbaar voor de nieuwe Azure DB die Cosmos-accounts.
>

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Dit artikel biedt een overzicht van S1, S2 en S3 prestatieniveaus en wordt beschreven hoe de verzamelingen die gebruikmaken van deze prestatieniveaus laat in 2017 worden gemigreerd naar verzamelingen met één partitie. Na het lezen van dit artikel, hebt u mogelijk de volgende vragen beantwoorden:

- [Waarom worden de prestaties S1, S2 en S3 niveaus buiten gebruik gesteld?](#why-retired)
- [Hoe verzamelingen met één partitie en gepartitioneerde verzamelingen Vergelijk aan de S1 S2 S3 prestatieniveaus?](#compare)
- [Wat moet ik doen om ervoor te zorgen dat toegang tot mijn gegevens?](#uninterrupted-access)
- [Hoe wordt mijn verzameling wijzigen na de migratie?](#collection-change)
- [Hoe wordt mijn facturering wijzigen nadat ik ben gemigreerd naar verzamelingen met één partitie?](#billing-change)
- [Wat gebeurt er als ik meer dan 10 GB aan opslagruimte nodig?](#more-storage-needed)
- [Kan ik wijzigen tussen de S1, S2 en S3 prestatieniveaus vóór de geplande migratie?](#change-before)
- [Hoe weet ik wanneer de verzameling is gemigreerd?](#when-migrated)
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
|Maximale doorvoer|Onbeperkt|10K RU/s|250 RU/s|1 K RU/s|2.5 K RU/s|
|Minimaal doorvoer|2.5 K RU/s|400 RU/s|250 RU/s|1 K RU/s|2.5 K RU/s|
|Maximale opslag|Onbeperkt|10 GB|10 GB|10 GB|10 GB|
|Prijs (maandelijks)|Doorvoer: $6 / 100 RU/s<br><br>Opslag: $ 0,25/GB|Doorvoer: $6 / 100 RU/s<br><br>Opslag: $ 0,25/GB|25 USD $|50 USD $|100 USD $|

Weet u een klant EA? Zo ja, Zie [hoe ben ik beïnvloed als ik een klant EA?](#ea-customer)

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-to-do-to-ensure-uninterrupted-access-to-my-data"></a>Wat moet ik doen om ervoor te zorgen dat toegang tot mijn gegevens?

Er is niets, Cosmos DB omgaat met de migratie voor u. Als u een verzameling S1, S2 of S3 hebt, wordt de huidige verzameling worden gemigreerd naar een verzameling van één partitie laat in 2017. 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-the-migration"></a>Hoe wordt mijn verzameling wijzigen na de migratie?

Als u een verzameling S1 hebt, wordt u worden gemigreerd naar een verzameling van één partitie met 400 RU/s-doorvoer. 400 RU/s is de laagste doorvoer beschikbaar met verzamelingen met één partitie. Echter, de kosten voor 400 RU/s in een verzameling van één partitie is ongeveer hetzelfde als u met uw verzameling S1 en 250 RU/s – zijn betaalt, zodat u niet voor extra 150 RU/s. voor u beschikbaar betalen.

Als u een verzameling S2 hebt, wordt u worden gemigreerd naar een verzameling van één partitie met 1 K RU/s. Er zijn geen wijzigingen ziet u op het doorvoerniveau.

Als u een verzameling S3 hebt, wordt u worden gemigreerd naar een verzameling van één partitie met 2,5 K RU/s. Er zijn geen wijzigingen ziet u op het doorvoerniveau.

In elk van deze gevallen, nadat uw verzameling wordt gemigreerd, kunt u zich kunnen het doorvoerniveau van uw aanpassen of het omhoog en omlaag geschaald naar behoefte lage latentie om toegang te bieden aan uw gebruikers. Om de doorvoerniveau nadat uw verzameling zijn gemigreerd, gewoon uw Cosmos-DB-account in de Azure portal te openen, klikt u op schaal, kies uw verzameling en past u het doorvoerniveau van, zoals wordt weergegeven in de volgende schermafbeelding:

![Doorvoer schalen in Azure portal](./media/performance-levels/portal-scale-throughput.png)

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-im-migrated-to-the-single-partition-collections"></a>Hoe wordt mijn facturering wijzigen nadat ik ben gemigreerd naar de verzamelingen met één partitie?

Ervan uitgaande dat u hebt 10 S1 verzamelingen, 1 GB aan opslagruimte voor elk in de regio VS-Oost, en u deze 10 S1 verzamelingen migreren naar verzamelingen met één partitie 10 op 400 RU per seconde (minimaal niveau). Uw factuur wordt er als volgt uitzien als u de 10 verzamelingen met één partitie voor een volledige maand behouden:

![Hoe S1 prijzen voor 10 verzamelingen vergelijkt met 10 verzamelingen met prijzen voor een verzameling van één partitie](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a>Wat gebeurt er als ik meer dan 10 GB aan opslagruimte nodig?

Of u een verzameling met een prestatieniveau S1, S2 of S3 of een verzameling van één partitie, die allemaal 10 GB aan opslagruimte beschikbaar is, dat kunt u het hulpprogramma voor gegevensmigratie Cosmos-DB uw om gegevens te migreren naar een gepartitioneerde verzameling met vrijwel hebben onbeperkte opslag. Zie voor meer informatie over de voordelen van een gepartitioneerde verzameling [partitionering en schalen in Azure Cosmos DB](sql-api-partition-data.md). 

<a name="change-before"></a>

## <a name="can-i-change-between-the-s1-s2-and-s3-performance-levels-before-the-planned-migration"></a>Kan ik wijzigen tussen de S1, S2 en S3 prestatieniveaus vóór de geplande migratie?

Alleen bestaande accounts met S1, S2 en S3 prestaties zich te wijzigen en alter niveau prestatielagen via de portal of programmatisch. Als u vanuit S1, S3 of S3 voor een verzameling van één partitie wijzigt, kunt u niet terug naar de prestatieniveaus S1, S2 of S3.

<a name="when-migrated"></a>

## <a name="how-will-i-know-when-my-collection-has-migrated"></a>Hoe weet ik wanneer de verzameling is gemigreerd?

De migratie wordt uitgevoerd op laat in 2017. Als u een verzameling die gebruikmaakt van de S1 hebt, prestatieniveaus S2 of S3, de Cosmos-DB-team neemt contact met u per e-mail voordat de migratie plaatsvindt. Zodra de migratie voltooid is, wordt de Azure-portal weergegeven dat gebruikmaakt van uw verzameling standaardprijzen.

![Het bevestigen van uw verzameling is gemigreerd naar de Standard-prijscategorie](./media/performance-levels/portal-standard-pricing-applied.png)

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-the-s1-s2-s3-performance-levels-to-single-partition-collections-on-my-own"></a>Hoe Migreer ik van de S1, S2 S3 prestatieniveaus om verzamelingen met één partitie zelf?

U kunt migreren vanaf de prestatieniveaus S1, S2 en S3 naar verzamelingen met één partitie met de Azure portal of programmatisch. U kunt dit doen op uw eigen vóór de geplande migratie profiteren van de flexibele doorvoer beschikbare opties bij verzamelingen met één partitie of gaan we uw verzamelingen voor u laat in 2017.

**Om te migreren naar verzamelingen met één partitie met de Azure portal**

1. In de [ **Azure-portal**](https://portal.azure.com), klikt u op **Azure Cosmos DB**, selecteer vervolgens de Cosmos-DB-account te wijzigen. 
 
    Als **Azure Cosmos DB** is niet in de Snelbalk, klikt u op >, schuif naar **Databases**, selecteer **Azure Cosmos DB**, en selecteer vervolgens het account.  

2. Klik in het menu resource onder **Containers**, klikt u op **Scale**, selecteer de verzameling wijzigen van de vervolgkeuzelijst en klik vervolgens op **prijscategorie**. Accounts met behulp van vooraf gedefinieerde doorvoer hebben een prijscategorie S1, S2 of S3.  In de **Kies uw prijscategorie** pagina, klikt u op **standaard** te wijzigen naar de gebruiker gedefinieerde doorvoer en klik vervolgens op **Selecteer** uw wijziging op te slaan.

    ![Schermopname van de pagina met instellingen die toont waar u de waarde van de doorvoer te wijzigen](./media/performance-levels/change-performance-set-thoughput.png)

3. Terug in de **Scale** pagina de **prijscategorie** wordt gewijzigd naar **standaard** en de **doorvoer (RU/s)** wordt weergegeven met een standaard de waarde van 400. Stel de doorvoer tussen 400 en 10.000 [Aanvraageenheden](request-units.md)/second (RU/s). De **maandelijkse factuur geschatte** onder aan de pagina-updates automatisch naar een schatting van de maandelijkse kosten. 

    >[!IMPORTANT] 
    > Nadat u uw wijzigingen hebt opgeslagen en naar de Standard verplaatsen-prijscategorie, u kan niet worden teruggedraaid naar de prestatieniveaus S1, S2 of S3.

4. Klik op **opslaan** uw wijzigingen op te slaan.

    Als u vaststelt dat u meer doorvoer (groter dan 10.000 RU/s) of meer opslagruimte moet (groter dan 10 GB), kunt u een gepartitioneerde verzameling kunt maken. Zie voor het migreren van een verzameling van één partitie in een gepartitioneerde verzameling, [migreren van één partitie naar gepartitioneerde verzamelingen](sql-api-partition-data.md#migrating-from-single-partition).

    > [!NOTE]
    > Wijzigen van S1, S2 of S3 naar Standard kan maximaal twee minuten duren.
    > 
    > 

**Om te migreren naar verzamelingen met één partitie met de .NET SDK**

Er is een andere optie voor het wijzigen van de uw verzamelingen prestatieniveaus via de Azure Cosmos DB SDK's. Deze sectie bevat alleen de prestaties van een verzameling wijzigen niveau met behulp van de [SQL .NET API](sql-api-sdk-dotnet.md), maar het proces is vergelijkbaar voor onze andere SDK.

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
