---
title: Aanvraageenheden en het schatten van de doorvoer - Azure Cosmos DB | Microsoft Docs
description: Meer informatie over het begrijpen, opgeven en maak een schatting van aanvraag eenheid vereisten in Azure Cosmos DB.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: rimman
ms.openlocfilehash: 23a3e629e12e2a4d417757c9fef5db804bb72c9e
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248751"
---
# <a name="throughput-and-request-units-in-azure-cosmos-db"></a>Doorvoer- en aanvraageenheden in Azure Cosmos DB

Azure Cosmos DB-resources worden in rekening gebracht op basis van de ingerichte doorvoer en opslag. Azure Cosmos DB-doorvoer wordt uitgedrukt in termen van **Aanvraageenheden per seconde (RU/s)**. Azure Cosmos DB ondersteunt verschillende API's met verschillende bewerkingen, variërend van eenvoudige leest en schrijft naar complexe graph-query's. Elke aanvraag verbruikt aanvraageenheden op basis van de hoeveelheid berekening voor het uitvoeren van de aanvraag. Het aantal aanvraageenheden voor een bewerking is deterministisch. U kunt het aantal aanvraageenheden die worden verbruikt door elke bewerking in Azure Cosmos DB met behulp van de reactieheader bijhouden. Om te bieden voorspelbare prestaties, moet u de doorvoer in eenheden van 100 ru's / seconde reserveren. U kunt uw doorvoer moet met behulp van de Azure Cosmos DB schatten [aanvraag eenheid calculator](https://www.documentdb.com/capacityplanner).

U kunt doorvoer op twee granulaties inrichten in Azure Cosmos DB: 

1. **Azure Cosmos DB-container:** de doorvoer die is ingericht voor een container is gereserveerd voor alleen dat specifieke container. Bij het toewijzen van throughput(RU/s) op het niveau van de container, de containers kunnen worden gemaakt als **vaste** of **onbeperkt**. 

  Containers met vaste grootte hebben een limiet van de maximale doorvoer van 10.000 RU/s en de opslaglimiet van 10 GB. Voor het maken van een onbeperkte container, moet u een minimale doorvoer van 1000 RU/s en een [partitiesleutel](partition-data.md). Omdat uw gegevens kunnen worden verdeeld over meerdere partities, hebt u het kiezen van een partitiesleutel waarvoor een hoge kardinaliteit (100 naar miljoenen afzonderlijke waarden). U selecteert een partitiesleutel met veel verschillende waarden, Azure Cosmos DB zorgt ervoor dat de aanvragen voor een verzameling, tabel en een graaf op uniforme wijze worden geschaald. 

2. **Azure Cosmos DB-database:** de doorvoer die is ingericht voor een database wordt gedeeld door alle containers in die database. Bij het inrichten van doorvoer op het databaseniveau van de, kunt u expliciet uitsluit bepaalde containers en in plaats daarvan worden ingericht met doorvoer voor de containers op het niveau van de container. Database-level doorvoer is vereist voor alle verzamelingen worden gemaakt met een partitiesleutel. Bij het toewijzen van doorvoer op het databaseniveau van de, de containers die deel uitmaken van deze database moeten worden gemaakt met een partitiesleutel omdat elke verzameling is een **onbeperkt** container.  

Op basis van de ingerichte doorvoer, toewijzen Azure Cosmos DB fysieke partities voor het hosten van uw container (s) en worden gegevens over meerdere partities wanneer deze groeit. De volgende afbeelding ziet u inrichting doorvoer op verschillende niveaus:

  ![Inrichting van aanvraageenheden voor afzonderlijke containers en instellen van containers](./media/request-units/provisioning_set_containers.png)

> [!NOTE] 
> Doorvoer op het niveau van de container en het databaseniveau wordt ingericht zijn aparte aanbiedingen en schakelen tussen een van deze moet migreren van gegevens van bron naar doel. Dit betekent dat u wilt maken van een nieuwe database of een nieuwe verzameling en vervolgens migreren van gegevens met behulp van [bulksgewijs executor bibliotheek](bulk-executor-overview.md) of [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md).

## <a name="request-units-and-request-charges"></a>Aanvraageenheden en kosten aanvragen

Azure Cosmos DB biedt snelle, voorspelbare prestaties door het reserveren van resources om te voldoen aan de doorvoerbehoeften van uw toepassing. Laad- en toegangspatronen van toepassingen veranderen in de loop der tijd. Met Azure Cosmos DB kunt u gemakkelijk de hoeveelheid gereserveerde doorvoer vergroten of verkleinen die beschikbaar is voor uw toepassing.

Met Azure Cosmos DB is gereserveerde doorvoer die is opgegeven in termen van aanvraageenheid per seconde verwerken. U kunt de basis van aanvraageenheden beschouwen als doorvoer valuta. U reserveren een aantal gegarandeerde aanvraageenheden beschikbaar voor uw toepassing op basis van de per-seconde. Elke bewerking in Azure Cosmos DB, met inbegrip van het schrijven van een document, een query wordt uitgevoerd en bijwerken van een document, verbruikt CPU, geheugen en IOP's. Dat wil zeggen, is elke bewerking leidt tot een aanvraag in rekening gebracht, die wordt uitgedrukt in reserveringseenheden. Als u de factoren die invloed hebben op aanvraag eenheid kosten in rekening gebracht en de doorvoer van uw toepassingsvereisten begrijpt, kunt u uw toepassing kunt uitvoeren als de kosten-effectief mogelijk. 

## <a name="throughput-isolation-in-globally-distributed-databases"></a>Isolatie van de doorvoer in wereldwijd gedistribueerde databases

Als u uw database in meer dan één regio repliceert, biedt Azure Cosmos DB doorvoer isolatie om ervoor te zorgen dat verbruik van aanvraageenheden in één regio heeft geen invloed op verbruik van aanvraageenheden in een andere regio. Bijvoorbeeld, als u gegevens in één regio schrijven en lezen van gegevens uit een andere regio, de aanvraageenheden die worden gebruikt voor het uitvoeren van de schrijfbewerking in een regio niet nemen van de aanvraageenheden die worden gebruikt voor de leesbewerking in regio B. aanvraag eenheden cro worden niet splitsen SS de regio's waarin u uw database hebt geïmplementeerd. Elke regio waarin de database worden gerepliceerd, is het volledige nummer van aanvraageenheden ingericht. Zie voor meer informatie over globale replicatie [hoe u gegevens globaal distribueren met Azure Cosmos DB](distribute-data-globally.md).

## <a name="request-unit-considerations"></a>Overwegingen voor aanvraag-eenheid
Wanneer u een schatting maken van het aantal aanvraageenheden om in te richten, is het belangrijk dat u rekening houden met de volgende variabelen:

* **Item grootte**. Als grootte toeneemt, neemt het aantal aanvraageenheden gebruikt om te lezen of schrijven van de gegevens ook toe.
* **De eigenschap count-item**. Ervan uitgaande dat het standaard indexering van alle eigenschappen, de eenheden die worden gebruikt voor het schrijven van een verhoging van het document, knooppunt of entiteit als de eigenschap count toeneemt.
* **Gegevensconsistentie**. Wanneer u gegevens consistentiemodellen, zoals sterk of gebonden veroudering, worden extra aanvraageenheden gebruikt voor het lezen van de items.
* **Geïndexeerde eigenschappen**. Een beleid index op elke container wordt bepaald welke eigenschappen standaard worden geïndexeerd. Door het aantal geïndexeerde eigenschappen te beperken of door in te schakelen vertraagde indexeren, kunt u het gebruik van de aanvraag-eenheid voor schrijfbewerkingen verminderen.
* **Indexeren van document**. Elk item wordt standaard automatisch geïndexeerd. U minder aanvraageenheden gebruiken als u wilt het aantal objecten niet te indexeren.
* **Query uitvoeren op patronen**. De complexiteit van een query is van invloed op het aantal aanvraageenheden voor een bewerking worden verbruikt. Het aantal query resulteert, het aantal predikaten, de aard van de predikaten, het nummer van de gebruiker gedefinieerde functies, de grootte van de brongegevens en alle projecties van invloed zijn op de kosten van querybewerkingen.
* **Gebruik een script**. Net als bij query's, opgeslagen procedures en triggers gebruiken van aanvraageenheden op basis van de complexiteit van de bewerkingen die worden uitgevoerd. Inspecteer de aanvraagheader van de kosten in rekening gebracht voor meer informatie over hoe elke bewerking aanvraag eenheid capaciteit verbruikt tijdens het ontwikkelen van uw toepassing.

## <a name="estimating-throughput-needs"></a>Schatten van doorvoervereisten te kunnen voldoen
Een aanvraageenheid is een genormaliseerde meting van de kosten voor verwerking van aanvragen. De eenheid van een enkele aanvraag vertegenwoordigt de verwerkingscapaciteit die vereist zijn om te lezen (via Self link- of -ID) een één item van 1KB die uit 10 unieke eigenschapswaarden bestaat (met uitzondering van Systeemeigenschappen). Een aanvraag voor het maken van (invoegen), vervangen of verwijderen van hetzelfde artikel verbruikt meer verwerking van de service en daarmee vereist meer aanvraageenheden. 

> [!NOTE]
> De basislijn van 1 aanvraageenheid voor een item van 1 KB komt overeen met een eenvoudige ophalen door self link-of-ID van het item.
> 
> 

Dit is bijvoorbeeld een tabel waarin het aantal aanvraageenheden om in te richten voor artikelen met drie verschillende grootten (1 KB, 4 KB en 64 KB) en voor twee verschillende prestatieniveaus (500 leesbewerkingen per seconde + 100 schrijfbewerkingen per seconde en 500 leesbewerkingen per seconde + 500 schrijfbewerkingen per seconde). In dit voorbeeld wordt de consistentie van gegevens is ingesteld op **sessie**, en het indexeringsbeleid is ingesteld op **geen**.

| Grootte van item | Leesbewerkingen per seconde | Schrijfbewerkingen per seconde | Aanvraageenheden
| --- | --- | --- | --- |
| 1 KB | 500 | 100 | (500 * 1) + (100 * 5) = 1000 RU/s
| 1 KB | 500 | 500 | (500 * 1) + (500 * 5) 3.000 RU/s =
| 4 KB | 500 | 100 | (500 * 1.3) + (100 * 7) = 1.350 RU/s
| 4 KB | 500 | 500 | (500 * 1.3) + (500 * 7) 4,150 RU/s =
| 64 kB | 500 | 100 | (500 * 10) + (100 * 48) 9,800 RU/s =
| 64 kB | 500 | 500 | (500 * 10) + (500 * 48) 29.000 RU/s =

### <a name="use-the-request-unit-calculator"></a>De aanvraag eenheid Rekenmachine gebruiken
Voor hulp bij het afstemmen van uw schattingen doorvoer, kunt u een webgebaseerde [aanvraag eenheid calculator](https://www.documentdb.com/capacityplanner). De Rekenmachine kan helpen uw schatting de vereisten van de aanvraag-eenheid voor normale bewerkingen, met inbegrip van:

* Item wordt gemaakt (schrijven)
* Item leesbewerkingen
* Item verwijderen
* Item-updates

Het hulpprogramma biedt ook ondersteuning voor het schatten van de behoeften voor opslag van gegevens op basis van de voorbeelditems die u opgeeft.

Het hulpprogramma te gebruiken:

1. Upload een of meer representatieve items (bijvoorbeeld, een voorbeeld van JSON-document).
   
    ![Items uploaden naar de Rekenmachine-eenheid van aanvraag][2]
2. Voer voor een schatting van de vereisten voor gegevensopslag, het totale aantal artikelen (bijvoorbeeld, documenten, rijen of hoekpunten) die u verwacht op te slaan.
3. Voer het nummer van het maken, lezen, bijwerken en delete-bewerkingen die u nodig hebt (op basis van per seconde). Voor een schatting van de kosten voor de aanvraag-unit van item update-bewerkingen, uploadt u een kopie van het item voorbeeld uit stap 1, die typisch veld updates bevat. Bijvoorbeeld, als item updates doorgaans twee eigenschappen wijzigen met de naam *lastLogin* en *userVisits*, een voorbeeld te kopiëren, werk de waarden voor deze twee eigenschappen en upload vervolgens het gekopieerde item.
   
    ![Vereisten voor doorvoer in de aanvraag eenheid Rekenmachine invoeren][3]
4. Selecteer **berekenen**, en vervolgens de resultaten te onderzoeken.
   
    ![Eenheid calculator resultaten aanvragen][4]

> [!NOTE]
> Als u itemtypen die aanzienlijk wat betreft grootte en het aantal geïndexeerde eigenschappen afwijken hebt, uploadt u een voorbeeld van elk *type* van typische item voor het hulpprogramma en vervolgens de resultaten te berekenen.
> 
> 

### <a name="use-the-azure-cosmos-db-request-charge-response-header"></a>Gebruik de Azure Cosmos DB-aanvraag kosten in rekening gebracht response-header
Elke reactie van de Azure Cosmos DB-service bestaat uit een aangepaste header (`x-ms-request-charge`) die de basis van aanvraageenheden gebruikt voor een bepaalde aanvraag bevat. U kunt deze header ook openen via de Azure Cosmos DB SDK's. In de .NET SDK, **RequestCharge** is een eigenschap van de **ResourceResponse** object. Voor query's biedt Azure Cosmos DB Data Explorer in Azure portal aanvraaggegevens kosten in rekening gebracht voor de uitgevoerde query's. Voor meer informatie over hoe u aan en doorvoer instellen met behulp van verschillende multi-model-API's Zie [instellen en opvragen van doorvoer in Azure Cosmos DB](set-throughput.md) artikel.

Er is een methode voor het schatten van de hoeveelheid gereserveerde doorvoer die is vereist voor de toepassing om vast te leggen van de kosten van de aanvraag-eenheden die zijn gekoppeld aan met het normale bewerkingen uitvoeren in een representatieve-item dat wordt gebruikt door uw toepassing. Vervolgens, maak een schatting van het aantal bewerkingen die u verwacht dat per seconde uitvoeren. Moet u ook meten en opnemen van typische query's en het gebruik van Azure Cosmos DB-script.

> [!NOTE]
> Als u itemtypen die aanzienlijk wat betreft grootte en het aantal geïndexeerde eigenschappen afwijken hebt, noteert u de kosten van toepassing bewerking aanvraag eenheden die zijn gekoppeld aan elk *type* van gemiddeld item.
> 
> 

Dit zijn bijvoorbeeld de stappen die u kunt uitvoeren:

1. Noteer de aanvraag eenheid kosten in rekening gebracht voor het maken van (invoegen) een gemiddeld item. 
2. De kosten van de aanvraag-eenheden voor het lezen van een gemiddeld item van record.
3. De kosten van de aanvraag-eenheden voor het bijwerken van een gemiddeld item van record.
4. Noteer de kosten van de aanvraag-eenheden van item gebruikelijke, algemene query's.
5. Noteer de aanvraag eenheid kosten in rekening gebracht van eventuele aangepaste scripts (opgeslagen procedures, triggers, de gebruiker gedefinieerde functies) die gebruikmaakt van de toepassing.
6. Berekenen van de vereiste aanvraageenheden krijgt het geschatte aantal bewerkingen die u verwacht dat per seconde worden uitgevoerd.

## <a name="a-request-unit-estimate-example"></a>Een voorbeeld van een aanvraag eenheid schatting
Houd rekening met het volgende document, die ongeveer 1 KB groot is:

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
> Documenten worden minified in Azure Cosmos DB, zodat de grootte door systeem berekend van het document dat hierboven iets minder dan 1 KB is.
> 
> 

De volgende tabel bevat geschatte aanvraag eenheid kosten voor normale bewerkingen voor dit item. (De kosten voor geschatte aanvraag eenheden wordt ervan uitgegaan dat het consistentieniveau account is ingesteld op **sessie** en dat alle items worden automatisch geïndexeerd.)

| Bewerking | Kosten voor aanvraag-eenheden |
| --- | --- |
| Item maken |~15 RU |
| Item lezen |~1 RU |
| Query-item op ID |~2.5 RU |

De volgende tabel bevat geschatte aanvraag eenheid kosten in rekening gebracht voor typische query's in de toepassing gebruikt:

| Query’s uitvoeren | Kosten voor aanvraag-eenheden | Totaal aantal geretourneerde items |
| --- | --- | --- |
| Selecteer food op ID |~2.5 RU |1 |
| Selecteer foods per fabrikant |~7 RU |7 |
| Selecteer door food groep en volgorde door het gewicht |~70 RU |100 |
| Top 10 foods in een groep food selecteren |~10 RU |10 |

> [!NOTE]
> Aanvraag eenheid kosten variëren op basis van het aantal geretourneerde items.
> 
> 

Met deze informatie kunt u de vereisten voor de eenheid van aanvraag voor deze toepassing gegeven van het aantal bewerkingen en query's dat u verwacht per seconde dat schatten:

| Bewerking/query | Het geschatte aantal per seconde | Vereiste aanvraageenheden |
| --- | --- | --- |
| Item maken |10 |150 |
| Item lezen |100 |100 |
| Selecteer foods per fabrikant |25 |175 |
| Voedsel groep selecteren |10 |700 |
| Top 10 selecteren |15 |150 totaal |

In dit geval u verwacht dat de vereiste van een gemiddelde doorvoersnelheid van 1,275 ru's / seconde. Naar beneden afronden naar de dichtstbijzijnde 100, richt u 1,300 ru's / seconde voor deze toepassing container (of set met containers).

## <a id="RequestRateTooLarge"></a> Overschrijding van grenzen van de gereserveerde doorvoer in Azure Cosmos DB
Aanvraag eenheidsverbruik wordt geëvalueerd tegen een tarief per seconde. Aanvragen worden beperkt in de frequentie voor toepassingen die groter zijn dan de ingerichte aanvraagsnelheid eenheid, totdat het percentage lager is dan het niveau van de ingerichte doorvoer. Wanneer een aanvraag beperkt in de snelheid is, wordt de server de aanvraag met preventief te beëindigd `RequestRateTooLargeException` (HTTP-statuscode 429) en retourneert de `x-ms-retry-after-ms` header. De header geeft aan dat de hoeveelheid tijd in milliseconden, die de gebruiker wachten moet voordat u de aanvraag opnieuw uitvoert.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Als u de Client-SDK voor .NET en LINQ-query's, de meeste van de tijd u nooit meer hoeft te bekommeren om deze uitzondering. De huidige versie van de Client-SDK voor .NET impliciet vangt dit antwoord, respecteert de-server opgegeven opnieuw proberen na de header en de aanvraag wordt automatisch opnieuw geprobeerd. Als uw account wordt door meerdere clients tegelijkertijd wordt geopend, wordt de volgende poging slaagt.

Als u hebt meer dan één client cumulatief werken boven de snelheid van aanvragen, het standaardgedrag voor opnieuw proberen is mogelijk onvoldoende en de client genereert een `DocumentClientException` met de status code 429 naar de toepassing. In gevallen als volgt, is het raadzaam om te overwegen verwerken van het gedrag voor opnieuw proberen en de logica in van uw toepassing foutafhandeling routines of vergroot de ingerichte doorvoer voor de container (of een set met containers).

## <a name="next-steps"></a>Volgende stappen
 
- Meer informatie over het [instellen en opvragen van doorvoer in Azure Cosmos DB](set-throughput.md) met behulp van Azure portal en SDK's.
- Meer informatie over [prestaties en schaal testen met Azure Cosmos DB](performance-testing.md).
- Zie voor meer informatie over gereserveerde doorvoer met Azure Cosmos DB-databases, [prijzen voor Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) en [partitioneren van gegevens in Azure Cosmos DB](partition-data.md).
- Zie voor meer informatie over Azure Cosmos DB, de [documentatie voor Azure Cosmos DB](https://azure.microsoft.com/documentation/services/cosmos-db/). 

[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png


