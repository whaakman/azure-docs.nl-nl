---
title: Aanvraageenheden & schatten van de doorvoer - Azure Cosmos DB | Microsoft Docs
description: Meer informatie over het begrijpen en vereisten voor aanvraag-eenheid in Azure Cosmos DB schatten opgeven.
services: cosmos-db
author: SnehaGunda
manager: kfile
documentationcenter: ''
ms.assetid: d0a3c310-eb63-4e45-8122-b7724095c32f
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: rimman
ms.openlocfilehash: 0aa87aeaf852d7309c29c1298e326c101a944904
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="request-units-in-azure-cosmos-db"></a>Aanvraageenheden in Azure Cosmos DB

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) van Microsoft wereldwijd gedistribueerde database voor meerdere model is. Met Azure Cosmos DB er geen virtuele machines te verhuren, software implementeren of databases bewaken. Azure Cosmos DB wordt beheerd en continu bewaakt door Microsoft bovenste engineers leveren world klasse beschikbaarheid, prestaties en beveiliging. U toegang hebt tot uw gegevens met behulp van de API's van uw keuze, zoals de [SQL-API](documentdb-introduction.md), [MongoDB API](mongodb-introduction.md), [tabel API](table-introduction.md), en in een grafiek de [Gremlin API](graph-introduction.md) - alle systeemeigen worden ondersteund. 

De valuta van Azure DB die Cosmos is de **aanvragen Unit (RU)**. Met RUs hoeft u niet reserveren lezen/schrijven capaciteiten of inrichten CPU, geheugen en IOPS. Azure Cosmos DB ondersteunt een aantal API's met verschillende bewerkingen, variërend van eenvoudige leest en schrijft naar grafiek complexe query's. Omdat niet alle aanvragen gelijk zijn, zijn ze een genormaliseerde hoeveelheid toegewezen **aanvraageenheden** op basis van de hoeveelheid berekeningen voor het uitvoeren van de aanvraag. Het aantal aanvraageenheden voor een bewerking is deterministisch en kunt u het aantal aanvraageenheden verbruikt door elke bewerking in Azure Cosmos DB via een antwoordheader bijhouden. 

Om te bieden voorspelbare prestaties, die u wilt reserveren doorvoer in eenheden van 100 RU/seconde. U kunt [schat uw doorvoer moet](request-units.md#estimating-throughput-needs) met behulp van de Cosmos Azure DB [aanvraag eenheid Rekenmachine](https://www.documentdb.com/capacityplanner).

![Doorvoer Rekenmachine][5]

Na het lezen van dit artikel, hebt u mogelijk de volgende vragen beantwoorden:  

* Wat zijn aanvraageenheden en kosten van de aanvraag in Azure Cosmos DB?
* Hoe geef aanvraag eenheid capaciteit voor een container of een set van containers in Azure Cosmos DB?
* Hoe u schat dat mijn toepassing aanvraag heeft?
* Wat gebeurt er als ik aanvraag eenheid capaciteit voor een container of een set van containers in Azure Cosmos DB overschrijdt?

Omdat Azure Cosmos DB een database met meerdere modellen. het is belangrijk te weten dat in dit artikel van toepassing op alle gegevensmodellen en in Azure DB die Cosmos-API's is. Dit artikel wordt de algemene voorwaarden zoals *container* en een *item* om te verwijzen algemeen naar een verzameling, grafiek, of een tabel en een document, een knooppunt of een entiteit, respectievelijk.

## <a name="request-units-and-request-charges"></a>Aanvraageenheden en kosten van de aanvraag
Azure Cosmos DB biedt snelle en voorspelbare prestaties door *reserveren* bronnen om te voldoen aan de behoeften van uw toepassing doorvoer.  Omdat de toepassing laden en patronen wijzigen na verloop van tijd toegang, wordt Azure Cosmos DB kunt u eenvoudig vergroten of verkleinen van de hoeveelheid gereserveerde doorvoer die beschikbaar zijn voor uw toepassing.

Met Azure Cosmos DB is gereserveerde doorvoer opgegeven in termen van aanvraageenheden per seconde verwerkt. U kunt zien aanvraageenheden als valuta doorvoer, waarbij u *reserveren* een hoeveelheid gegarandeerde aanvraageenheden die beschikbaar zijn voor uw toepassing op basis van per seconde.  Elke bewerking in Azure Cosmos DB - schrijven, uitvoeren van een query, het bijwerken van een document - verbruikt CPU, geheugen en IOPS.  Dat wil zeggen, elke bewerking leidt ertoe dat een *aanvragen kosten*, die wordt uitgedrukt in *aanvraageenheden*.  Informatie over de factoren die van invloed zijn op aanvraag eenheid kosten, samen met de vereisten van de doorvoer van uw toepassing, kunt u uw toepassing uitvoeren als de kosten-effectief mogelijk. De Data Explorer in de Azure portal is ook een fantastische hulpprogramma voor het testen van de kern van een query.

Het is raadzaam om aan de slag door het bekijken van de volgende video, waarbij Azure Cosmos DB Program Manager Andrew Liu aanvraageenheden besproken.

> [!VIDEO https://www.youtube.com/embed/stk5WSp5uX0]
> 
> 

## <a name="throughput-isolation-in-globally-distributed-databases"></a>Isolatie van doorvoer in globaal gedistribueerde databases

Wanneer de database hebt met meer dan één regio worden gerepliceerd, biedt Azure Cosmos DB doorvoer isolatie om ervoor te zorgen dat RU gebruik in één regio is geen invloed heeft RU gebruik in een andere regio. Bijvoorbeeld, als u gegevens naar één regio schrijven en gegevens uit een andere regio lezen, de RUs gebruikt voor de schrijfbewerking uitvoeren in regio *A* worden pas van kracht weg van de RUs gebruikt voor de leesbewerking in regio *B*. RUs zijn niet gesplitst in de regio's die u hebt geïmplementeerd. Elke regio waarin de database wordt gerepliceerd heeft het volledige nummer van RUs ingericht. Zie voor meer informatie over globale replicatie [het distribueren van gegevens met Azure Cosmos DB globaal](distribute-data-globally.md).

## <a name="request-unit-considerations"></a>Overwegingen voor aanvraag-eenheid
Bij het bepalen van het aantal aanvraageenheden om in te richten, is het belangrijk dat u rekening met de volgende variabelen:

* **De grootte van item**. Als de grootte van het aantal toeneemt verhoogt ook aanvraageenheden gebruikt om te lezen of schrijven van de gegevens.
* **Aantal eigenschappen item**. Ervan uitgaande dat standaard indexeren van alle eigenschappen, de eenheden verbruikt voor het schrijven van een verhoging van het knooppunt-document/entiteit als de eigenschap aantal toeneemt.
* **Gegevensconsistentie**. Wanneer consistentie gegevensmodellen zoals sterke of gebonden veroudering wordt gebruikt, worden extra aanvraageenheden verbruikt voor het lezen van de items.
* **Geïndexeerde eigenschappen**. Een index-beleid op elke container bepaalt welke eigenschappen standaard worden geïndexeerd. Door het aantal geïndexeerde eigenschappen te beperken of doordat de vertraagde indexeren, kunt u uw aanvraag eenheidsverbruik voor schrijfbewerkingen verminderen.
* **Document indexeren**. Elk item wordt standaard automatisch geïndexeerd. U minder aanvraageenheden gebruiken als u wilt het aantal objecten niet te indexeren.
* **Query uitvoeren op patronen**. De complexiteit van een query heeft gevolgen voor het aantal aanvraageenheden voor een bewerking worden verbruikt. Het aantal queryresultaten, aantal predicaten, aard van de predikaten, projecties, aantal UDF's en de grootte van de brongegevens - beïnvloeden alle de kosten van querybewerkingen.
* **Gebruik een script**.  Net als bij query's, opgeslagen procedures en triggers in beslag nemen aanvraageenheden op basis van de complexiteit van de bewerkingen die worden uitgevoerd. Inspecteer de aanvraagheader kosten om beter te begrijpen hoe elke bewerking aanvraag eenheid capaciteit is verbruikt tijdens het ontwikkelen van uw toepassing.

## <a name="estimating-throughput-needs"></a>Schatten van doorvoerbehoeften
Een aanvraag-eenheid is een genormaliseerde meting van de kosten voor aanvraagverwerking. Een enkele aanvraag eenheid vertegenwoordigt de verwerkingscapaciteit vereist om te lezen (via self link- of -id) van een enkele 1 KB item dat bestaat uit 10 unieke eigenschapswaarden (met uitzondering van Systeemeigenschappen). Een aanvraag wilt maken (invoegen), vervangen of verwijderen van hetzelfde artikel verbruikt meer verwerken van de service en zodoende meer aanvraageenheden.   

> [!NOTE]
> De basislijn van de eenheid voor een 1 KB-artikel 1 aanvraag komt overeen met een eenvoudige GET door self link of id van het item.
> 
> 

Hier is bijvoorbeeld een tabel die wordt aangegeven hoeveel aanvraageenheden om in te richten voor artikelen met drie verschillende grootten (1 KB, 4 KB en 64 KB) en op twee verschillende prestatieniveaus (500 leesbewerkingen per seconde + 100 schrijfbewerkingen per seconde en 500 leesbewerkingen per seconde + 500 schrijfbewerkingen per seconde). De consistentie van de gegevens is geconfigureerd op *sessie*, en het indexeringsbeleid is ingesteld op *geen*.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>De grootte van artikel</strong></p></td>
            <td valign="top"><p><strong>Reads/second</strong></p></td>
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

1. Upload een of meer representatieve items (bijvoorbeeld een voorbeeld JSON-document).
   
    ![Items uploaden naar de Rekenmachine-eenheid van aanvraag][2]
2. Voor een schatting van de vereisten voor gegevensopslag, typ het totaal aantal items (bijvoorbeeld documenten, rijen of hoekpunten) u verwacht op te slaan.
3. Voer het nummer van het maken, lezen, bijwerken en delete-bewerkingen die u nodig hebt (op basis van het per seconde). Upload een kopie van de voorbeeld-item uit stap 1 bovenstaande typische veld updates bevat voor een schatting van de kosten van de aanvraag-eenheid van item update-bewerkingen.  Bijvoorbeeld, als item updates doorgaans twee eigenschappen wijzigen met de naam *lastLogin* en *userVisits*, klikt u vervolgens een voorbeeld kopiëren, werk de waarden voor deze twee eigenschappen en upload het gekopieerde item.
   
    ![Doorvoer vereisten invoeren in de aanvraag eenheid Rekenmachine][3]
4. Klik op berekenen en bekijk de resultaten.
   
    ![Eenheid Rekenmachine resultaten aanvragen][4]

> [!NOTE]
> Als u itemtypen die aanzienlijk verschillen in termen van de grootte en het aantal geïndexeerde eigenschappen hebt wordt, uploadt u een voorbeeld van elke *type* van typische punt voor het hulpprogramma en de resultaten vervolgens te berekenen.
> 
> 

### <a name="use-the-azure-cosmos-db-request-charge-response-header"></a>Gebruik de antwoordheader van Azure DB die Cosmos aanvraag kosten
Elke reactie van de service Azure Cosmos DB bestaat uit een aangepaste header (`x-ms-request-charge`) die de aanvraageenheden dat is gebruikt voor een bepaalde aanvraag bevat. Deze header is ook toegankelijk via de Azure Cosmos DB SDK's. In de .NET SDK, `RequestCharge` is een eigenschap van de `ResourceResponse` object.  Azure Cosmos DB Data Explorer in de Azure portal biedt voor query's, kosten aanvraaggegevens voor uitgevoerde query's.

Met dat gegeven in gedachte, één methode voor het schatten van de hoeveelheid gereserveerde doorvoer vereist door uw toepassing is voor het vastleggen van de aanvraag eenheid kosten die zijn gekoppeld aan met het normale bewerkingen uitvoeren in een representatieve item gebruikt door de toepassing en vervolgens schatting maken van de aantal bewerkingen die om uit te voeren per seconde wordt verwacht.  Zorg ervoor dat voor het meten en zijn typische query's en ook gebruik van Azure DB die Cosmos-script.

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

## <a name="a-request-unit-estimate-example"></a>Een voorbeeld van een aanvraag eenheid schatting
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

De volgende tabel bevat de geschatte aanvraag eenheid kosten voor normale bewerkingen op dit object (de geschatte aanvraag eenheid kosten wordt ervan uitgegaan dat het niveau van de consistentie account is ingesteld op *sessie* en dat alle items worden automatisch geïndexeerd):

| Bewerking | Aanvraag eenheid kosten |
| --- | --- |
| Item maken |~15 RU |
| Item lezen |~1 RU |
| Query-item met id |~2.5 RU |

In deze tabel ziet een benadering aanvraag bovendien eenheid kosten voor typische query's in de toepassing gebruikt:

| Query’s uitvoeren | Aanvraag eenheid kosten | het aantal geretourneerde artikelen |
| --- | --- | --- |
| Selecteer voeding door-id |~2.5 RU |1 |
| Selecteer levensmiddelen op fabrikant |~7 RU |7 |
| Selecteer door Voedingsgroep en de volgorde op basis van gewicht |~70 RU |100 |
| Bovenste 10 levensmiddelen in een Voedingsgroep selecteren |~10 RU |10 |

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

In dit geval verwacht u dat de vereiste van een gemiddelde doorvoersnelheid van 1,275 RU/s.  Afronden naar de dichtstbijzijnde 100, zou u inrichten 1.300 RU/s voor deze toepassing container (of reeks containers).

## <a id="RequestRateTooLarge"></a> Overschrijding van gereserveerde doorvoer grenzen in Azure Cosmos-DB
Intrekken dat verzoek eenheidsverbruik met een snelheid per seconde wordt geëvalueerd. Voor toepassingen die groter is dan de ingerichte aanvraagsnelheid eenheid aanvragen snelheid beperkt worden pas de frequentie waarmee het niveau van de ingerichte doorvoer. Wanneer een aanvraag snelheid beperkt ontvangt, wordt de server de aanvraag met optie preventief beëindigd `RequestRateTooLargeException` (HTTP-statuscode 429) en retourneert de `x-ms-retry-after-ms` header die de hoeveelheid tijd in milliseconden, dat de gebruiker wachten moet voordat u probeert de aanvraag aangeeft.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Als u gebruikmaakt van de Client-SDK voor .NET en LINQ-query's en vervolgens de meeste van de tijd die u niet hoeft te gaan met deze uitzondering als de huidige versie van de Client-SDK voor .NET impliciet dit antwoord dat, respecteert de server opgegeven probeer het opnieuw nadat de header en probeert om opnieuw de automatisch aanvragen. Tenzij uw account wordt door meerdere clients tegelijkertijd geopend, wordt de volgende poging slaagt.

Als er meer dan één client cumulatief werken boven het percentage aanvragen, het standaardgedrag voor opnieuw proberen niet toereikend zijn en de client genereert een `DocumentClientException` met de status code 429 tot de toepassing. U kunt in gevallen als volgt, kunt u de logica in routines voor foutafhandeling van uw toepassing en het gedrag voor het opnieuw of verhoog de doorvoer die zijn ingericht voor de container (of de set van containers).

## <a name="next-steps"></a>Volgende stappen
 
Zie voor meer informatie over het instellen en doorvoer ophalen met behulp van Azure-portal en SDK de:

* [Stel en doorvoer in Azure Cosmos DB ophalen](set-throughput.md)

Lees deze informatiebronnen voor meer informatie over gereserveerde doorvoer met Azure Cosmos DB databases:

* [Prijzen van Azure DB Cosmos](https://azure.microsoft.com/pricing/details/cosmos-db/)
* [Gegevens in Azure Cosmos DB te partitioneren](partition-data.md)

Zie voor meer informatie over Azure Cosmos DB, de Azure DB die Cosmos [documentatie](https://azure.microsoft.com/documentation/services/cosmos-db/). 

Als u wilt beginnen met de schaal en prestaties testen met Azure Cosmos DB, Zie [prestaties en schaal testen met Azure Cosmos DB](performance-testing.md).

[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png

