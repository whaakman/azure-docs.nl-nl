---
title: Aanvraageenheden en het schatten van de doorvoer - Azure Cosmos DB | Microsoft Docs
description: Meer informatie over het begrijpen en vereisten voor aanvraag-eenheid in Azure Cosmos DB schatten opgeven.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: rimman
ms.openlocfilehash: 160ff4e09f70036fd261c07fa59e13772bc00660
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37053324"
---
# <a name="request-units-in-azure-cosmos-db"></a>Aanvraageenheden in Azure Cosmos-DB

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is de Microsoft wereldwijd gedistribueerde multimodel-database. Met Azure Cosmos DB er geen virtuele machines te verhuren, software implementeren of databases bewaken. Azure Cosmos DB wordt beheerd en continu bewaakt door Microsoft bovenste engineers leveren hoogwaardige beschikbaarheid, prestaties en beveiliging. U kunt toegang tot uw gegevens met behulp van API's van uw keuze, zoals de [SQL](documentdb-introduction.md), [MongoDB](mongodb-introduction.md), en [tabel](table-introduction.md) -API's en grafiek via de [Gremlin API](graph-introduction.md). Alle API's worden alle systeemeigen worden ondersteund. 

De valuta van Azure DB die Cosmos is de *aanvraag-eenheid (RU)*. Met aanvraageenheden hoeft u niet te reserveren lezen/schrijven capaciteiten of inrichten CPU, geheugen en IOPS. Azure Cosmos DB ondersteunt verschillende API's die u verschillende bewerkingen hebt, variërend van eenvoudige leest en schrijft naar grafiek complexe query's. Omdat niet alle aanvragen gelijk zijn, worden aanvragen een genormaliseerde aantal aanvraageenheden op basis van de hoeveelheid berekeningen voor het uitvoeren van de aanvraag toegewezen. Het aantal aanvraageenheden voor een bewerking is deterministisch. U kunt het aantal aanvraageenheden die worden gebruikt door een bewerking in Azure Cosmos DB via een antwoordheader bijhouden. 

Reserveren voor voorspelbare prestaties, doorvoer in eenheden van 100 RU/seconde. U kunt [schat uw doorvoer moet](request-units.md#estimating-throughput-needs) met behulp van de Cosmos Azure DB [aanvraag eenheid Rekenmachine](https://www.documentdb.com/capacityplanner).

![Doorvoer Rekenmachine][5]

Na het lezen van dit artikel, hebt u mogelijk de volgende vragen beantwoorden:

* Wat zijn aanvraageenheden en kosten van de aanvraag in Azure Cosmos DB?
* Hoe geef aanvraag eenheid capaciteit voor een container of een set van containers in Azure Cosmos DB?
* Hoe u schat dat mijn toepassing aanvraag heeft?
* Wat gebeurt er als ik aanvraag eenheid capaciteit voor een container of een set van containers in Azure Cosmos DB overschrijdt?

Omdat Azure Cosmos DB een multimodel database is, is het belangrijk te weten dat in dit artikel van toepassing op alle gegevensmodellen en in Azure DB die Cosmos-API's is. Dit artikel wordt gebruikt voor algemene voorwaarden zoals *container* algemeen verwijzen naar een verzameling of de grafiek en *item* algemeen verwijzen naar een tabel, document, knooppunt of entiteit.

## <a name="request-units-and-request-charges"></a>Aanvraageenheden en kosten van de aanvraag

Azure Cosmos DB biedt snelle, voorspelbare prestaties door te reserveren bronnen om te voldoen aan de doorvoerbehoeften van uw toepassing. Een toepassing werklast en toegang patronen wijzigen gedurende een bepaalde periode. Azure Cosmos-database kunt u eenvoudig vergroten of verkleinen van de hoeveelheid gereserveerde doorvoer die beschikbaar zijn voor uw toepassing.

Met Azure Cosmos DB worden gereserveerde doorvoer is opgegeven in termen van aanvraag eenheid per seconde verwerkt. U kunt aanvraageenheden beschouwen als valuta doorvoer. U reserveren een aantal aanvraageenheden moet beschikbaar zijn voor uw toepassing per seconde op basis van een gegarandeerde. Elke bewerking in Azure Cosmos DB, met inbegrip van schrijven van een document, een query uitvoeren en bijwerken van een document maakt gebruik van CPU, geheugen en IOPS. Dat wil zeggen, elke bewerking leidt ertoe dat de kosten van een aanvraag, uitgedrukt in aanvraageenheden. Als u de factoren die van invloed zijn op aanvraag eenheid kosten en vereisten van uw toepassing doorvoer begrijpt, kunt u uw toepassing uitvoeren als kosten-effectief mogelijk. 

Als u aan de slag wilt, Azure Cosmos DB Program Manager Andrew Liu komen aan bod aanvraageenheden in de volgende video (Er is een kleine typefout gemaakt in het voorbeeld van de aanvraag eenheden van de video. Wanneer 1 KB gegevens met 100.000 records wordt gebruikt, is de totale opslagruimte 100 MB en niet 100 GB): <br /><br />

> [!VIDEO https://www.youtube.com/embed/stk5WSp5uX0]
> 
> 

## <a name="throughput-isolation-in-globally-distributed-databases"></a>Isolatie van doorvoer in globaal gedistribueerde databases

Als u de database naar meer dan één regio repliceert, biedt Azure Cosmos DB doorvoer isolatie om ervoor te zorgen dat gebruik van de aanvraag-eenheid in één regio is niet van invloed op aanvraag eenheid gebruik in een andere regio. Bijvoorbeeld, als u gegevens tot één regio geschreven en gegevens van een andere regio gelezen, de aanvraageenheden die worden gebruikt voor het uitvoeren van de schrijfbewerking in een regio geen onderneemt weg van de aanvraageenheden die worden gebruikt voor de leesbewerking in regio B. aanvraag eenheden cro zijn niet splitsen SS de regio's waarin u de database hebt geïmplementeerd. Elke regio waarin de database wordt gerepliceerd heeft de volledige aantal aanvraageenheden ingericht. Zie voor meer informatie over globale replicatie [het distribueren van gegevens met Azure Cosmos DB globaal](distribute-data-globally.md).

## <a name="request-unit-considerations"></a>Overwegingen voor aanvraag-eenheid
Wanneer u een schatting maken van het aantal aanvraageenheden om in te richten, is het belangrijk dat u rekening houden met de volgende variabelen:

* **De grootte van item**. Als de grootte toeneemt, verhoogt het aantal aanvraageenheden gebruikt om te lezen of schrijven van de gegevens ook.
* **Aantal eigenschappen item**. Ervan uitgaande dat standaard indexeren van alle eigenschappen, de eenheden verbruikt voor het schrijven van een verhoging van het document, knooppunt of entiteit als de eigenschap aantal toeneemt.
* **Gegevensconsistentie**. Wanneer u een consistentiecontrole gegevensmodellen zoals sterke of gebonden veroudering gebruikt, worden extra aanvraageenheden verbruikt voor het lezen van de items.
* **Geïndexeerde eigenschappen**. Een index-beleid op elke container bepaalt welke eigenschappen standaard worden geïndexeerd. Door het aantal geïndexeerde eigenschappen te beperken of doordat de vertraagde indexeren, kunt u uw aanvraag eenheidsverbruik voor schrijfbewerkingen verminderen.
* **Document indexeren**. Elk item wordt standaard automatisch geïndexeerd. U minder aanvraageenheden gebruiken als u wilt het aantal objecten niet te indexeren.
* **Query uitvoeren op patronen**. De complexiteit van een query is van invloed op het aantal aanvraageenheden voor een bewerking worden verbruikt. Het aantal query resulteert, het aantal predicaten, de aard van de predicaten, het nummer van de gebruiker gedefinieerde functies, de grootte van de brongegevens en alle projecties invloed op de kosten van querybewerkingen.
* **Gebruik een script**. Net als bij query's, opgeslagen procedures en triggers in beslag nemen aanvraageenheden op basis van de complexiteit van de bewerkingen die worden uitgevoerd. Inspecteer de aanvraagheader kosten om beter te begrijpen hoe elke bewerking aanvraag eenheid capaciteit verbruikt tijdens het ontwikkelen van uw toepassing.

## <a name="estimating-throughput-needs"></a>Schatten van doorvoerbehoeften
Een aanvraag-eenheid is een genormaliseerde meting van de kosten voor aanvraagverwerking. Een enkele aanvraag eenheid vertegenwoordigt de verwerkingscapaciteit die vereist zijn om te lezen (via self link- of -ID) van een enkele 1 KB-artikel dat uit 10 unieke eigenschapswaarden bestaat (met uitzondering van Systeemeigenschappen). Een aanvraag voor het maken van (invoegen), vervangen of te verwijderen van hetzelfde artikel verbruikt meer verwerking van de service en waardoor vereist meer aanvraageenheden. 

> [!NOTE]
> De basislijn van 1 aanvraag eenheid voor een 1 KB-artikel correspondeert met een eenvoudige GET door self link of ID van het item.
> 
> 

Hier is bijvoorbeeld een tabel die wordt aangegeven hoeveel aanvraageenheden om in te richten voor artikelen met drie verschillende grootten (1 KB, 4 KB en 64 KB) en op twee verschillende prestatieniveaus (500 leesbewerkingen per seconde + 100 schrijfbewerkingen per seconde en 500 leesbewerkingen per seconde + 500 schrijfbewerkingen per seconde). In dit voorbeeld wordt de gegevensconsistentie is ingesteld op **sessie**, en het indexeringsbeleid is ingesteld op **geen**.

| De grootte van artikel | Leesbewerkingen per seconde | Schrijfbewerkingen per seconde | Aanvraageenheden
| --- | --- | --- | --- |
| 1 KB | 500 | 100 | (500 * 1) + (100 * 5) = 1000 RU/s
| 1 KB | 500 | 500 | (500 * 1) + (500 * 5) = 3.000 RU/s
| 4 KB | 500 | 100 | (500 * 1.3) + (100 * 7) = 1,350 RU/s
| 4 KB | 500 | 500 | (500 * 1.3) + (500 * 7) = 4,150 RU/s
| 64 kB | 500 | 100 | (500 * 10) + (100 * 48) = 9,800 RU/s
| 64 kB | 500 | 500 | (500 * 10) + (500 * 48) = 29.000 RU/s


### <a name="use-the-request-unit-calculator"></a>De rekenhulp voor aanvraag-eenheid
Als u uw schattingen doorvoer stemmen, kunt u een webgebaseerde [aanvraag eenheid Rekenmachine](https://www.documentdb.com/capacityplanner). De Rekenmachine kan helpen uw schatting van de vereisten van de aanvraag-eenheid voor normale bewerkingen, inclusief:

* Item maakt (schrijft)
* Item leest
* Item wordt verwijderd
* Item updates

Het hulpprogramma biedt tevens ondersteuning voor het schatten van opslagbehoeften op basis van de voorbeelditems die u opgeeft.

Het hulpprogramma te gebruiken:

1. Upload een of meer representatieve items (bijvoorbeeld een voorbeeld JSON-document).
   
    ![Items uploaden naar de Rekenmachine-eenheid van aanvraag][2]
2. Voer het totale aantal items (bijvoorbeeld documenten, rijen of hoekpunten) die u verwacht op te slaan voor een schatting van de vereisten voor gegevensopslag.
3. Voer het nummer van het maken, lezen, bijwerken en delete-bewerkingen die u nodig hebt (op basis van het per seconde). Upload een kopie van de voorbeeld-item uit stap 1 typische veld updates bevat voor een schatting van de kosten van de aanvraag-eenheid van item update-bewerkingen. Bijvoorbeeld, als item updates doorgaans twee eigenschappen wijzigen met de naam *lastLogin* en *userVisits*, een voorbeeld kopiëren, werk de waarden voor deze twee eigenschappen en upload het gekopieerde item.
   
    ![Doorvoer vereisten invoeren in de aanvraag eenheid Rekenmachine][3]
4. Selecteer **Calculate**, en bekijk de resultaten.
   
    ![Eenheid Rekenmachine resultaten aanvragen][4]

> [!NOTE]
> Als er itemtypen die aanzienlijk verschilt en grootte en het aantal geïndexeerde eigenschappen, uploadt u een voorbeeld van elke *type* van typische item voor het hulpprogramma en de resultaten vervolgens te berekenen.
> 
> 

### <a name="use-the-azure-cosmos-db-request-charge-response-header"></a>Gebruik de antwoordheader van Azure DB die Cosmos aanvraag kosten
Elke reactie van de service Azure Cosmos DB bestaat uit een aangepaste header (`x-ms-request-charge`) die de aanvraageenheden dat is gebruikt voor een bepaalde aanvraag bevat. U kunt deze header ook openen via de Azure Cosmos DB SDK's. In de .NET SDK, **RequestCharge** is een eigenschap van de **ResourceResponse** object. Azure Cosmos DB Data Explorer in de Azure portal biedt voor query's, kosten aanvraaggegevens voor uitgevoerde query's. Voor meer informatie over het ophalen en set-doorvoer met behulp van verschillende modellen API Zie [ingesteld en ophalen van doorvoer in Azure Cosmos DB](set-throughput.md) artikel.

Er is een methode voor het schatten van de hoeveelheid gereserveerde doorvoer vereist door uw toepassing om vast te leggen van de aanvraag eenheid kosten die zijn gekoppeld aan met het normale bewerkingen uitvoeren in een representatieve-item dat wordt gebruikt door uw toepassing. Vervolgens schatting maken van het aantal bewerkingen die om uit te voeren per seconde wordt verwacht. Zorg ervoor dat ook meten en opnemen van typische query's en het gebruik van Azure DB die Cosmos-script.

> [!NOTE]
> Als er itemtypen die aanzienlijk verschilt en grootte en het aantal geïndexeerde eigenschappen, noteert u de toepasselijke bewerking aanvraag eenheid kosten die zijn gekoppeld aan elk *type* van typische item.
> 
> 

Bijvoorbeeld: dit zijn de stappen die u kunt uitvoeren:

1. Noteer de aanvraag eenheid kosten voor het maken van (invoegen) een typische item. 
2. Noteer de aanvraag eenheid kosten voor het lezen van een typische item.
3. Noteer de aanvraag eenheid kosten van het bijwerken van een typische item.
4. Noteer de kosten van de aanvraag-eenheid van item gebruikelijke, algemene query's.
5. Noteer de aanvraag eenheid kosten van alle aangepaste scripts (opgeslagen procedures, triggers, gebruiker gedefinieerde functies) die de toepassing gebruikt.
6. Berekenen van de vereiste aanvraageenheden gezien het geschatte aantal bewerkingen die u verwacht te per seconde worden uitgevoerd.

## <a name="a-request-unit-estimate-example"></a>Een voorbeeld van een aanvraag eenheid schatting
Houd rekening met het volgende document, ongeveer 1 KB groot is:

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
> Documenten worden minified in Azure Cosmos DB, zodat de grootte door systeem berekend van de bovenstaande document iets minder dan 1 KB.
> 
> 

De volgende tabel bevat de geschatte aanvraag eenheid kosten voor normale bewerkingen op dit object. (De geschatte aanvraag eenheid kosten wordt ervan uitgegaan dat het niveau van de consistentie account is ingesteld op **sessie** en alle items automatisch worden geïndexeerd.)

| Bewerking | Aanvraag eenheid kosten |
| --- | --- |
| Item maken |~15 RU |
| Item lezen |~1 RU |
| Query-item met ID |~2.5 RU |

De volgende tabel bevat een benadering aanvraag eenheid kosten voor typische query's in de toepassing gebruikt:

| Query’s uitvoeren | Aanvraag eenheid kosten | het aantal geretourneerde artikelen |
| --- | --- | --- |
| Selecteer voeding door-ID |~2.5 RU |1 |
| Selecteer levensmiddelen op fabrikant |~7 RU |7 |
| Selecteer door Voedingsgroep en de volgorde op basis van gewicht |~70 RU |100 |
| Bovenste 10 levensmiddelen in een Voedingsgroep selecteren |~10 RU |10 |

> [!NOTE]
> Aanvraag eenheid kosten variëren, afhankelijk van het aantal items geretourneerd.
> 
> 

Met deze informatie kunt u de vereisten voor de aanvraag-eenheden voor deze toepassing gezien het aantal bewerkingen en query's die u verwacht dat per seconde schat:

| Bewerking/query | Het geschatte aantal per seconde | Vereiste aanvraageenheden |
| --- | --- | --- |
| Item maken |10 |150 |
| Item lezen |100 |100 |
| Selecteer levensmiddelen op fabrikant |25 |175 |
| Selecteer door de Voedingsgroep |10 |700 |
| Selecteer top 10 |15 |150 totaal |

In dit geval u verwacht dat de vereiste van een gemiddelde doorvoersnelheid van 1,275 RU/seconde. Afronden naar de dichtstbijzijnde 100, zou u inrichten 1,300 RU/seconde voor deze toepassing container (of reeks containers).

## <a id="RequestRateTooLarge"></a> Overschrijding van gereserveerde doorvoer grenzen in Azure Cosmos-DB
Aanvraag eenheidsverbruik wordt een snelheid per seconde geëvalueerd. Aanvragen zijn beperkt in de frequentie voor toepassingen die groter zijn dan het percentage van de eenheid ingerichte aanvragen, pas de frequentie waarmee het niveau van de ingerichte doorvoer. Wanneer een aanvraag snelheid beperkt is, wordt de server de aanvraag met optie preventief beëindigd `RequestRateTooLargeException` (HTTP-statuscode 429) en retourneert de `x-ms-retry-after-ms` header. De header geeft de hoeveelheid tijd in milliseconden, dat de gebruiker wachten moet voordat u probeert de aanvraag.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Als u de Client-SDK voor .NET en LINQ-query's gebruikt, de meeste van de tijd, hebben u nooit om te gaan met deze uitzondering. De huidige versie van de Client-SDK voor .NET impliciet dat dit antwoord, respecteert de server opgegeven probeer het opnieuw nadat de header en de aanvraag wordt automatisch opnieuw geprobeerd. Tenzij uw account wordt door meerdere clients tegelijkertijd geopend, wordt de volgende poging slaagt.

Als er meer dan één client cumulatief werken boven het percentage aanvragen, het standaardgedrag voor opnieuw proberen is mogelijk onvoldoende en de client genereert een `DocumentClientException` met de status code 429 tot de toepassing. In gevallen zo wilt u mogelijk Houd rekening met het verwerken van de logica in uw toepassing foutafhandeling routines en gedrag voor het opnieuw of verhoog de doorvoer die zijn ingericht voor de container (of een set van containers).

## <a name="next-steps"></a>Volgende stappen
 
- Meer informatie over hoe [ingesteld en ophalen van doorvoer in Azure Cosmos DB](set-throughput.md) met behulp van Azure-portal en SDK's.
- Meer informatie over [prestaties en schaal testen met Azure Cosmos DB](performance-testing.md).
- Zie voor meer informatie over gereserveerde doorvoer met Azure Cosmos DB databases, [prijzen van Azure DB die Cosmos](https://azure.microsoft.com/pricing/details/cosmos-db/) en [partitioneren van gegevens in Azure Cosmos DB](partition-data.md).
- Zie voor meer informatie over Azure Cosmos DB, de [Azure Cosmos DB documentatie](https://azure.microsoft.com/documentation/services/cosmos-db/). 

[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png

