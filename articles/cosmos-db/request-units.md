---
title: Aanvraageenheden & schatten van de doorvoer - Azure Cosmos DB | Microsoft Docs
description: Meer informatie over het begrijpen en vereisten voor aanvraag-eenheid in Azure Cosmos DB schatten opgeven.
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: d0a3c310-eb63-4e45-8122-b7724095c32f
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: mimig
ms.openlocfilehash: c7aadb4e535ed221f882f251324b6d4e633c2d5e
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="request-units-in-azure-cosmos-db"></a>Aanvraageenheden in Azure Cosmos DB
Nu beschikbaar: Azure Cosmos DB [aanvraag eenheid Rekenmachine](https://www.documentdb.com/capacityplanner). Meer informatie [schatting van de doorvoer moet](request-units.md#estimating-throughput-needs).

![Doorvoer Rekenmachine][5]

## <a name="introduction"></a>Inleiding
[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) van Microsoft wereldwijd gedistribueerde database voor meerdere model is. Met Azure Cosmos DB er geen virtuele machines te verhuren, software implementeren of databases bewaken. Azure Cosmos DB wordt beheerd en continu bewaakt door Microsoft bovenste engineers leveren world klasse beschikbaarheid, prestaties en beveiliging. U toegang hebt tot uw gegevens met behulp van de API's van uw keuze, zoals de [SQL-API](documentdb-introduction.md), [MongoDB API](mongodb-introduction.md), [tabel API](table-introduction.md), en Gremlin via de [Graph API](graph-introduction.md) - alle systeemeigen worden ondersteund. De valuta van Azure DB die Cosmos is de aanvraag Unit (ru/s). Met RUs hoeft u niet reserveren lezen/schrijven capaciteiten of inrichten CPU, geheugen en IOPS.

Azure Cosmos DB ondersteunt een aantal API's met verschillende bewerkingen, variërend van eenvoudige leest en schrijft naar grafiek complexe query's. Omdat niet alle aanvragen gelijk zijn, zijn ze een genormaliseerde hoeveelheid toegewezen **aanvraageenheden** op basis van de hoeveelheid berekeningen voor het uitvoeren van de aanvraag. Het aantal aanvraageenheden voor een bewerking is deterministisch en kunt u het aantal aanvraageenheden verbruikt door elke bewerking in Azure Cosmos DB via een antwoordheader bijhouden. 

Om te bieden voorspelbare prestaties, die u wilt reserveren doorvoer in eenheden van 100 RU/seconde. 

Na het lezen van dit artikel, hebt u mogelijk de volgende vragen beantwoorden:  

* Wat zijn aanvraageenheden en kosten aanvragen?
* Hoe geef ik aanvraag eenheid capaciteit voor een verzameling?
* Hoe u schat dat mijn toepassing aanvraag heeft?
* Wat gebeurt er als ik aanvraag eenheid capaciteit voor een verzameling overschrijdt?

Als Azure Cosmos DB een meerdere model-database is, is het belangrijk te weten dat in dit artikel naar een verzameling/document voor een document een grafiek/knooppunt voor de API van een grafiek en een Tabelentiteit voor de tabel-API-API verwijst. Dit artikel verwijst naar het concept van een verzameling, de grafiek of de tabel als een container en een document, een knooppunt of een entiteit als een item.

## <a name="request-units-and-request-charges"></a>Aanvraageenheden en kosten van de aanvraag
Azure Cosmos DB biedt snelle en voorspelbare prestaties door *reserveren* bronnen om te voldoen aan de behoeften van uw toepassing doorvoer.  Omdat de toepassing laden en patronen wijzigen na verloop van tijd toegang, wordt Azure Cosmos DB kunt u eenvoudig vergroten of verkleinen van de hoeveelheid gereserveerde doorvoer die beschikbaar zijn voor uw toepassing.

Met Azure Cosmos DB is gereserveerde doorvoer opgegeven in termen van aanvraageenheden per seconde verwerkt. U kunt zien aanvraageenheden als valuta doorvoer, waarbij u *reserveren* een hoeveelheid gegarandeerde aanvraageenheden die beschikbaar zijn voor uw toepassing op basis van per seconde.  Elke bewerking in Azure Cosmos DB - schrijven, uitvoeren van een query, het bijwerken van een document - verbruikt CPU, geheugen en IOPS.  Dat wil zeggen, elke bewerking leidt ertoe dat een *aanvragen kosten*, die wordt uitgedrukt in *aanvraageenheden*.  Informatie over de factoren die van invloed zijn op aanvraag eenheid kosten, samen met de vereisten van de doorvoer van uw toepassing, kunt u uw toepassing uitvoeren als de kosten-effectief mogelijk. De queryverkenner is ook een fantastische hulpprogramma voor het testen van de kern van een query.

Het is raadzaam om aan de slag door het bekijken van de volgende video, waarbij Aravind Ramachandran wordt uitgelegd aanvraageenheden en voorspelbare prestaties met Azure Cosmos DB.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Predictable-Performance-with-DocumentDB/player]
> 
> 

## <a name="specifying-request-unit-capacity-in-azure-cosmos-db"></a>Capaciteit van de aanvraag-eenheid opgeven in Azure Cosmos-DB
Bij het starten van een nieuwe verzameling, de tabel of de grafiek, u het aantal aanvraageenheden per seconde (ru/s per seconde) die u wilt gereserveerd. Op basis van de ingerichte doorvoer, Azure Cosmos DB toegewezen fysieke partities voor het hosten van uw verzameling en splitsingen/rebalances gegevens meerdere partities wanneer deze groeit.

Azure DB Cosmos-containers kunnen worden gemaakt als vast of onbeperkt. Containers met vaste grootte hebben een maximale limiet van 10 GB en doorvoer van 10.000 RU/s. Een container voor onbeperkte maken moet u een minimale doorvoer van 1000 RU/s en een [partitiesleutel](partition-data.md). Aangezien uw gegevens hebben mogelijk om te worden verdeeld over meerdere partities, is het nodig om het kiezen van een partitiesleutel met een hoge kardinaliteit (100 tot miljoenen afzonderlijke waarden). Selecteren van een partitiesleutel met veel afzonderlijke waarden zorgt u ervoor dat uw tabel-verzameling/grafiek en aanvragen kunnen worden geschaald op uniforme wijze door Azure Cosmos DB. 

> [!NOTE]
> Een partitiesleutel is een grens van een logische en niet een fysieke. Daarom hoeft u niet wilt beperken het aantal afzonderlijke partitie sleutelwaarden. In feite is het beter om meer afzonderlijke partitie sleutelwaarden dan minder Azure Cosmos DB beschikt over meer opties voor taakverdeling.

Hier volgt een codefragment voor het maken van een verzameling met 3000 aanvraageenheden per tweede met de .NET SDK:

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

Azure Cosmos-database is van invloed op een model reservering op doorvoer. Dat wil zeggen, u wordt gefactureerd voor de hoeveelheid doorvoer *gereserveerde*, ongeacht hoeveel waarop doorvoer is actief *gebruikt*. Als uw toepassing de belasting, gegevens en gebruiksgegevens patronen wijzigen kunt u eenvoudig de schaal omhoog en omlaag de hoeveelheid gereserveerd RUs via SDK's of met behulp van de [Azure Portal](https://portal.azure.com).

Elke verzameling, de tabel/het grafiek zijn toegewezen aan een `Offer` resource in Azure Cosmos DB met metagegevens over de ingerichte doorvoer. U kunt de toegewezen doorvoer wijzigen door op de overeenkomende resource in de aanbieding voor een container te zoeken en vervolgens bijgewerkt met de nieuwe waarde voor de doorvoer. Hier volgt een codefragment voor het wijzigen van de doorvoer van een verzameling in 5000 aanvraageenheden per tweede met de .NET SDK:

```csharp
// Fetch the resource to be updated
Offer offer = client.CreateOfferQuery()
                .Where(r => r.ResourceLink == collection.SelfLink)    
                .AsEnumerable()
                .SingleOrDefault();

// Set the throughput to 5000 request units per second
offer = new OfferV2(offer, 5000);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

Er zijn geen gevolgen voor de beschikbaarheid van de container wanneer u de doorvoer wijzigt. De nieuwe gereserveerde doorvoer is doorgaans effectieve binnen enkele seconden op de toepassing van de nieuwe doorvoer.

## <a name="request-unit-considerations"></a>Overwegingen voor aanvraag-eenheid
Bij het bepalen van het aantal aanvraageenheden moet worden gereserveerd voor uw Azure DB die Cosmos-container, is het belangrijk dat u rekening met de volgende variabelen:

* **De grootte van item**. Als de grootte van de eenheden gebruikt om te lezen of schrijven verhoogt ook de gegevens toeneemt.
* **Aantal eigenschappen item**. Ervan uitgaande dat standaard indexeren van alle eigenschappen, de eenheden verbruikt voor het schrijven van een verhoging van het knooppunt-document/entiteit als de eigenschap aantal toeneemt.
* **Gegevensconsistentie**. Wanneer u gegevens consistentieniveaus van sterke of gebonden veroudering, worden extra eenheden verbruikt voor het lezen van de items.
* **Geïndexeerde eigenschappen**. Een index-beleid op elke container bepaalt welke eigenschappen standaard worden geïndexeerd. Door het aantal geïndexeerde eigenschappen te beperken of doordat de vertraagde indexeren, kunt u uw aanvraag eenheidsverbruik verminderen.
* **Document indexeren**. Elk item wordt standaard automatisch geïndexeerd. U minder aanvraageenheden gebruiken als u ervoor kiest niet te indexeren aantal objecten.
* **Query uitvoeren op patronen**. De complexiteit van een query heeft gevolgen voor het aantal eenheden van de aanvraag voor een bewerking worden verbruikt. Het aantal predicaten, aard van de predikaten, projecties, aantal UDF's en de grootte van de bron-gegevensset alle invloed hebben op de kosten van querybewerkingen.
* **Gebruik een script**.  Net als bij query's, opgeslagen procedures en triggers in beslag nemen aanvraageenheden op basis van de complexiteit van de bewerkingen die worden uitgevoerd. Inspecteer de aanvraagheader kosten om beter te begrijpen hoe elke bewerking aanvraag eenheid capaciteit is verbruikt tijdens het ontwikkelen van uw toepassing.

## <a name="estimating-throughput-needs"></a>Schatten van doorvoerbehoeften
Een aanvraag-eenheid is een genormaliseerde meting van de kosten voor aanvraagverwerking. Een enkele aanvraag eenheid vertegenwoordigt de verwerkingscapaciteit vereist om te lezen (via self link- of -id) van een enkele 1 KB item dat bestaat uit 10 unieke eigenschapswaarden (met uitzondering van Systeemeigenschappen). Een aanvraag wilt maken (invoegen), vervangen of verwijderen van hetzelfde artikel verbruikt meer verwerken van de service en zodoende meer aanvraageenheden.   

> [!NOTE]
> De basislijn van de eenheid voor een 1 KB-artikel 1 aanvraag komt overeen met een eenvoudige GET door self link of id van het item.
> 
> 

Hier is bijvoorbeeld een tabel waarin het aantal aanvraageenheden om in te richten op drie verschillende item grootten (1 KB, 4 KB en 64 KB) en op twee verschillende prestatieniveaus (500 leesbewerkingen per seconde + 100 schrijfbewerkingen per seconde en 500 leesbewerkingen per seconde + 500 schrijfbewerkingen per seconde). De consistentie van de gegevens op sessie is geconfigureerd en het indexeringsbeleid is ingesteld op None.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>De grootte van artikel</strong></p></td>
            <td valign="top"><p><strong>Leesbewerkingen per seconde</strong></p></td>
            <td valign="top"><p><strong>Schrijfbewerkingen per seconde</strong></p></td>
            <td valign="top"><p><strong>Aanvraageenheden</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1) + (100 * 5) = 1000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1) + (500 * 5) = 3.000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1.3) + (100 * 7) = 1,350 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1.3) + (500 * 7) = 4,150 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 kB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 10) + (100 * 48) = 9,800 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 kB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 10) + (500 * 48) = 29.000 RU/s</p></td>
        </tr>
    </tbody>
</table>

### <a name="use-the-request-unit-calculator"></a>De rekenhulp voor aanvraag-eenheid
Om te helpen klanten hun schattingen doorvoer stemmen, er is een webgebaseerde [aanvraag eenheid Rekenmachine](https://www.documentdb.com/capacityplanner) om te schatten van de vereisten van de aanvraag-eenheid voor normale bewerkingen, inclusief:

* Item maakt (schrijft)
* Item leest
* Item wordt verwijderd
* Item updates

Het hulpprogramma biedt tevens ondersteuning voor het schatten van opslagbehoeften op basis van de voorbeelditems die u opgeeft.

Met het hulpprogramma is eenvoudig:

1. Upload een of meer representatieve items.
   
    ![Items uploaden naar de Rekenmachine-eenheid van aanvraag][2]
2. Voer het totale aantal items die u verwacht op te slaan voor een schatting van de vereisten voor gegevensopslag.
3. Voer het aantal items maken, lezen, bijwerken en verwijderen van bewerkingen die u nodig (op basis van het per seconde hebt). Upload een kopie van de voorbeeld-item uit stap 1 bovenstaande typische veld updates bevat voor een schatting van de kosten van de aanvraag-eenheid van item update-bewerkingen.  Bijvoorbeeld als item updates worden meestal twee eigenschappen met de naam lastLogin en userVisits wijzigt, klikt u vervolgens gewoon kopieert de voorbeeld-item, werk de waarden voor deze twee eigenschappen en upload het gekopieerde item.
   
    ![Doorvoer vereisten invoeren in de aanvraag eenheid Rekenmachine][3]
4. Klik op berekenen en bekijk de resultaten.
   
    ![Eenheid Rekenmachine resultaten aanvragen][4]

> [!NOTE]
> Als u itemtypen die aanzienlijk verschillen in termen van de grootte en het aantal geïndexeerde eigenschappen hebt wordt, uploadt u een voorbeeld van elke *type* van typische punt voor het hulpprogramma en de resultaten vervolgens te berekenen.
> 
> 

### <a name="use-the-azure-cosmos-db-request-charge-response-header"></a>Gebruik de antwoordheader van Azure DB die Cosmos aanvraag kosten
Elke reactie van de service Azure Cosmos DB bestaat uit een aangepaste header (`x-ms-request-charge`) die de aanvraageenheden dat is gebruikt voor de aanvraag bevat. Deze header is ook toegankelijk via de Azure Cosmos DB SDK's. In de .NET SDK is RequestCharge een eigenschap van het object ResourceResponse.  Azure Cosmos DB Query Explorer in de Azure portal biedt voor query's, kosten aanvraaggegevens voor uitgevoerde query's.

![RU kosten in de Query Explorer onderzoeken][1]

Met deze in gedachten, één methode voor het schatten van de hoeveelheid gereserveerde doorvoer vereist door uw toepassing is voor het vastleggen van de aanvraag eenheid kosten die zijn gekoppeld aan met het normale bewerkingen uitvoeren in een representatieve item gebruikt door de toepassing en vervolgens het schatten van het aantal bewerkingen wilt u uitvoeren per seconde.  Zorg ervoor dat voor het meten en zijn typische query's en ook gebruik van Azure DB die Cosmos-script.

> [!NOTE]
> Als er itemtypen die aanzienlijk verschilt en grootte en het aantal geïndexeerde eigenschappen, en noteer vervolgens de betreffende bewerking aanvraag eenheid kosten die zijn gekoppeld aan elk *type* van typische item.
> 
> 

Bijvoorbeeld:

1. Noteer de aanvraag eenheid kosten voor het maken van (invoegen) een typische item. 
2. Noteer de aanvraag eenheid kosten voor het lezen van een typische item.
3. Noteer de aanvraag eenheid kosten van het bijwerken van een typische item.
4. Noteer de kosten van de aanvraag-eenheid van item gebruikelijke, algemene query's.
5. De aanvraag eenheid kosten van alle aangepaste scripts (opgeslagen procedures, triggers, gebruiker gedefinieerde functies) gebruikt door de toepassing registreren
6. Berekenen van de vereiste aanvraageenheden gezien het geschatte aantal bewerkingen die u verwacht te per seconde worden uitgevoerd.

## <a id="GetLastRequestStatistics"></a>API voor van MongoDB GetLastRequestStatistics opdracht gebruiken
API voor MongoDB biedt ondersteuning voor een aangepaste opdracht *getLastRequestStatistics*, voor het ophalen van de kosten van de aanvraag voor opgegeven bewerkingen.

Bijvoorbeeld, in de Mongo-Shell uitvoeren van de bewerking die u wilt controleren of de kosten van de aanvraag voor.
```
> db.sample.find()
```

Voer vervolgens de opdracht *getLastRequestStatistics*.
```
> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "OP_QUERY",
    "RequestCharge": 2.48,
    "RequestDurationInMilliSeconds" : 4.0048
}
```

Met deze in gedachten, één methode voor het schatten van de hoeveelheid gereserveerde doorvoer vereist door uw toepassing is voor het vastleggen van de aanvraag eenheid kosten die zijn gekoppeld aan met het normale bewerkingen uitvoeren in een representatieve item gebruikt door de toepassing en vervolgens het schatten van het aantal bewerkingen wilt u uitvoeren per seconde.

> [!NOTE]
> Als er itemtypen die aanzienlijk in termen van de grootte en het aantal geïndexeerde eigenschappen verschillen wordt, en noteer vervolgens de betreffende bewerking aanvraag eenheid kosten die zijn gekoppeld aan elk *type* van typische item.
> 
> 

## <a name="use-api-for-mongodbs-portal-metrics"></a>API gebruiken voor de portal metrieken van MongoDB
De eenvoudigste manier om een goede schatting van de aanvraag eenheid kosten voor uw API voor ophalen MongoDB-database is het gebruik van de [Azure-portal](https://portal.azure.com) metrische gegevens. Met de *aantal aanvragen* en *aanvraag kosten* grafieken, krijgt u een schatting van het aantal aanvraageenheden elke bewerking verbruikt en het aantal aanvraageenheden die ze gebruiken ten opzichte van elkaar.

![API voor MongoDB portal metrische gegevens][6]

## <a name="a-request-unit-estimation-example"></a>Een voorbeeld van een aanvraag eenheid schatting
Houd rekening met het volgende ~ 1 KB-document:

```json
{
 "id": "08259",
  "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
  "tags": [
    {
      "name": "cereals ready-to-eat"
    },
    {
      "name": "kellogg"
    },
    {
      "name": "kellogg's crispix"
    }
  ],
  "version": 1,
  "commonName": "Includes USDA Commodity B855",
  "manufacturerName": "Kellogg, Co.",
  "isFromSurvey": false,
  "foodGroup": "Breakfast Cereals",
  "nutrients": [
    {
      "id": "262",
      "description": "Caffeine",
      "nutritionValue": 0,
      "units": "mg"
    },
    {
      "id": "307",
      "description": "Sodium, Na",
      "nutritionValue": 611,
      "units": "mg"
    },
    {
      "id": "309",
      "description": "Zinc, Zn",
      "nutritionValue": 5.2,
      "units": "mg"
    }
  ],
  "servings": [
    {
      "amount": 1,
      "description": "cup (1 NLEA serving)",
      "weightInGrams": 29
    }
  ]
}
```

> [!NOTE]
> Documenten worden minified in Azure Cosmos DB, zodat het systeem berekend grootte van het document dat hierboven is iets minder dan 1 KB.
> 
> 

De volgende tabel bevat een benadering aanvraag eenheid kosten voor normale bewerkingen op dit object (de geschatte aanvraag eenheid kosten wordt ervan uitgegaan dat het niveau van de consistentie account is ingesteld op 'Sessie' en alle items worden automatisch geïndexeerd):

| Bewerking | Aanvraag eenheid kosten |
| --- | --- |
| Item maken |~ 15 RU |
| Item lezen |~ 1 RU |
| Query-item met id |~2.5 RU |

In deze tabel ziet een benadering aanvraag bovendien eenheid kosten voor typische query's in de toepassing gebruikt:

| Query’s uitvoeren | Aanvraag eenheid kosten | het aantal geretourneerde artikelen |
| --- | --- | --- |
| Selecteer voeding door-id |~2.5 RU |1 |
| Selecteer levensmiddelen op fabrikant |~ 7 RU |7 |
| Selecteer door Voedingsgroep en de volgorde op basis van gewicht |~ 70 RU |100 |
| Bovenste 10 levensmiddelen in een Voedingsgroep selecteren |~ 10 RU |10 |

> [!NOTE]
> RU kosten variëren, afhankelijk van het aantal items geretourneerd.
> 
> 

Met deze informatie kunt u schat de RU-vereisten voor deze toepassing gezien het aantal bewerkingen en query's die u verwacht dat per seconde:

| Bewerking/Query | Het geschatte aantal per seconde | Vereiste RUs |
| --- | --- | --- |
| Item maken |10 |150 |
| Item lezen |100 |100 |
| Selecteer levensmiddelen op fabrikant |25 |175 |
| Selecteer door de Voedingsgroep |10 |700 |
| Selecteer top 10 |15 |150 totaal |

In dit geval verwacht u dat de vereiste van een gemiddelde doorvoersnelheid van 1,275 RU/s.  Afronden naar de dichtstbijzijnde 100, zou u 1.300 RU/s voor de verzameling van deze toepassing inrichten.

## <a id="RequestRateTooLarge"></a>Overschrijding van gereserveerde doorvoer grenzen in Azure Cosmos-DB
Intrekken dat verzoek eenheidsverbruik wordt beoordeeld als een percentage per seconde als het budget leeg is. Voor toepassingen die groter zijn dan de frequentie van de eenheid ingerichte aanvraag voor een container, worden aanvragen voor deze verzameling beperkt tot de snelheid onder het niveau van de gereserveerde zakt. Wanneer er een vertraging optreedt, de server optie preventief eindigt de aanvraag met RequestRateTooLargeException (HTTP-statuscode 429) en retourneert de x-ms-opnieuw-na-ms-header die aangeeft van de hoeveelheid tijd in milliseconden, dat de gebruiker alvorens nogmaals te proberen wachten moet de de aanvraag.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Als u gebruikmaakt van de Client-SDK voor .NET en LINQ-query's en vervolgens de meeste gevallen u nooit beschikt over te gaan met deze uitzondering als de huidige versie van de Client-SDK voor .NET impliciet dit antwoord dat, respecteert de server opgegeven probeer het opnieuw nadat de header en probeert de aanvraag opnieuw. Tenzij uw account wordt door meerdere clients tegelijkertijd geopend, wordt de volgende poging slaagt.

Als er meer dan één client cumulatief werken boven het percentage aanvragen het standaardgedrag voor opnieuw proberen niet toereikend zijn en de client genereert een DocumentClientException met statuscode 429 tot de toepassing. In dergelijke gevallen overweegt u verwerken gedrag voor het opnieuw en logica in uw toepassing fout routines voor het afhandelen of een uitbreiding van de gereserveerde doorvoer voor de container.

## <a id="RequestRateTooLargeAPIforMongoDB"></a>Gereserveerde doorvoerlimieten in API overschrijden voor MongoDB
Toepassingen die groter is dan de ingerichte aanvraageenheden voor een verzameling wordt pas de frequentie waarmee het niveau van de gereserveerde worden beperkt. Wanneer er een vertraging optreedt, de back-end optie preventief beëindigd wanneer de aanvraag met een *16500* foutcode - *te veel aanvragen*. Standaard-API voor MongoDB wordt automatisch opnieuw maximaal 10 keer voordat er een *te veel aanvragen* foutcode. Als er veel *te veel aanvragen* foutcodes, kunt u ook beide toe te voegen gedrag voor het opnieuw in routines voor foutafhandeling van uw toepassing of [waardoor de gereserveerde doorvoer voor de verzameling](set-throughput.md).

## <a name="next-steps"></a>Volgende stappen
Lees deze informatiebronnen voor meer informatie over gereserveerde doorvoer met Azure Cosmos DB databases:

* [Prijzen van Azure DB Cosmos](https://azure.microsoft.com/pricing/details/cosmos-db/)
* [Gegevens in Azure Cosmos DB te partitioneren](partition-data.md)

Zie voor meer informatie over Azure Cosmos DB, de Azure DB die Cosmos [documentatie](https://azure.microsoft.com/documentation/services/cosmos-db/). 

Als u wilt beginnen met de schaal en prestaties testen met Azure Cosmos DB, Zie [prestaties en schaal testen met Azure Cosmos DB](performance-testing.md).

[1]: ./media/request-units/queryexplorer.png 
[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png
[6]: ./media/request-units/api-for-mongodb-metrics.png
