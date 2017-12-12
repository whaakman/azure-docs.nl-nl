---
title: Het model van complexe gegevenstypen in Azure Search | Microsoft Docs
description: "Genest of hiërarchische gegevensstructuren kunnen worden gemodelleerd in een Azure Search-index met platte rijenset en verzamelingen gegevenstype."
services: search
documentationcenter: 
author: LiamCa
manager: pablocas
editor: 
tags: complex data types; compound data types; aggregate data types
ms.assetid: e4bf86b4-497a-4179-b09f-c1b56c3c0bb2
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: liamca
ms.openlocfilehash: d7a7400fe7470439dfa957f1ddb463e0a7f1a271
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-model-complex-data-types-in-azure-search"></a>Het model complexe gegevenstypen in Azure Search
Externe gegevenssets die worden gebruikt voor het vullen van een Azure Search-index soms bevatten hiërarchische of geneste substructuren die niet netjes in een rijenset in tabelvorm doen uitgesplitst. Voorbeelden van dergelijke structuren mogelijk meerdere locaties en telefoonnummers omvatten voor één klant, meerdere kleuren en grootten voor een enkele SKU, meerdere auteurs van één boek, enzovoort. Voorwaarden modelleren, ziet u mogelijk deze aangeduid als structuren *complexe gegevenstypen*, *samengestelde gegevenstypen*, *samengestelde gegevenstypen*, of *gegevenstypen cumulatieve*, enz.

Complexe gegevenstypen worden niet standaard ondersteund in Azure Search, maar een beproefde tijdelijke oplossing omvat een proces in twee stappen van de structuur plat en vervolgens via een **verzameling** gegevenstype voor de interior structuur opnieuw samenstellen. Na de techniek die wordt beschreven in dit artikel kan de inhoud die moet worden doorzocht, meervoudige, gefilterd en gesorteerd.

## <a name="example-of-a-complex-data-structure"></a>Voorbeeld van een complexe gegevensstructuur
De betrokken gegevens bevindt zich doorgaans voor als een set van JSON of XML-documenten, of als items in een NoSQL-opslag, zoals Azure Cosmos DB. De uitdaging komen structureel, voort van het hebben van meerdere onderliggende items die moeten worden doorzocht en gefilterd.  Neem de volgende JSON-document dat een lijst met contactpersonen als voorbeeld wordt als een beginpunt voor het aanduiden van de tijdelijke oplossing:

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

Hoewel de velden met de naam 'id', 'name' en 'bedrijf' kunnen eenvoudig worden toegewezen-op-een volgens de velden in een Azure Search-index, het veld 'locaties' bevat een matrix met een set locatie-id's, evenals de beschrijvingen van de locatie-locaties. Gezien het feit dat Azure Search geen een gegevenstype dat wordt ondersteund, moeten we een andere manier om dit te modelleren in Azure Search. 

> [!NOTE]
> Deze methode wordt ook beschreven door Kirk Evans in een blogbericht [Azure Cosmos DB met Azure Search indexeren](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), waarin een techniek die genoemd 'plat de gegevens', waarbij u een veld met de naam hebt `locationsID` en `locationsDescription` die geen van beide [verzamelingen](https://msdn.microsoft.com/library/azure/dn798938.aspx) (of een matrix met tekenreeksen).   
> 
> 

## <a name="part-1-flatten-the-array-into-individual-fields"></a>Deel 1: De matrix plat in afzonderlijke velden
Voor het maken van een Azure Search-index die geschikt is voor deze gegevensset, maakt u afzonderlijke velden voor de geneste substructure: `locationsID` en `locationsDescription` met het gegevenstype van [verzamelingen](https://msdn.microsoft.com/library/azure/dn798938.aspx) (of een matrix met tekenreeksen). In deze velden zou u de waarden '1' en '2' index naar de `locationsID` veld voor John Smith en de waarden '3' & '4' in de `locationsID` voor Jen Campbell veld.  

Uw gegevens in Azure Search eruit als volgt: 

![voorbeeldgegevens, 2 rijen](./media/search-howto-complex-data-types/sample-data.png)

## <a name="part-2-add-a-collection-field-in-the-index-definition"></a>Deel 2: Een verzameling veld toevoegen in de indexdefinitie
In het schema van de index eruit de velddefinities als in dit voorbeeld.

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## <a name="validate-search-behaviors-and-optionally-extend-the-index"></a>Zoekgedrag valideren en de index uitbreiden
Ervan uitgaande dat u hebt gemaakt van de index en de gegevens geladen, test u de oplossing om te controleren of de queryuitvoering zoeken op basis van de gegevensset. Elke **verzameling** veld moet **doorzoekbare**, **Filterbaar** en **geschikt voor facetten**. U moet uitvoeren van query's zoals:

* Alle mensen die in het hoofdkantoor van het Adventureworks werken vinden.
* Haal een telling van het aantal mensen die in een Home Office werken.  
* Weergeven van de personen die op een startpagina Office werkt, welke andere kantoren ze samen met een telling van de personen die u in elke vestiging werken.  

Indien deze techniek valt uit elkaar is wanneer u wilt een zoekopdracht die zowel de locatie-id als de beschrijving van de locatie combineert. Bijvoorbeeld:

* Alle personen vinden waar ze een kantoor aan huis hebben en een locatie-ID 4 heeft.  

Als u de oorspronkelijke inhoud opgezocht als volgt:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Echter, nu dat we de gegevens in afzonderlijke velden gescheiden hebben, we hebben geen manier omdat u weet dat als de Home Office voor Jen Campbell is gekoppeld aan `locationsID 3` of `locationsID 4`.  

Definieer een ander veld in de index waarin alle gegevens worden gecombineerd in één verzameling voor de verwerking van deze aanvraag.  In ons voorbeeld noemen we dit veld `locationsCombined` en we zullen de inhoud met afzonderlijke een `||` maar u kunt scheidingsteken die u denkt dat een unieke reeks tekens voor uw inhoud zijn. Bijvoorbeeld: 

![voorbeeldgegevens, 2 rijen met scheidingsteken](./media/search-howto-complex-data-types/sample-data-2.png)

Met deze `locationsCombined` veld we nu aankan nog meer query's, zoals:

* Een aantal van mensen die op een 'Home kantoor' locatie-Id van '4 werken' weergegeven.  
* Zoeken naar personen die op een startpagina Office werkt met Id '4' locatie. 

## <a name="limitations"></a>Beperkingen
Dit is handig voor een aantal scenario's, maar het is niet van toepassing is in elk geval.  Bijvoorbeeld:

1. Als u geen een vaste set van velden in uw complex gegevenstype hebt en er kon geen enkele manier de mogelijke typen worden toegewezen aan één veld. 
2. Bijwerken van de geneste objecten vereist extra bewerkingen uitvoeren om te bepalen wat moet exact worden bijgewerkt in de Azure Search-index

## <a name="sample-code"></a>Voorbeeldcode
U kunt een voorbeeld zien over het een complexe verzameling van de JSON-gegevens in Azure Search-index en een aantal query's uitvoeren via deze gegevensset op deze [GitHub-repo-](https://github.com/liamca/AzureSearchComplexTypes).

## <a name="next-step"></a>Volgende stap
[Stem voor systeemeigen ondersteuning voor complexe gegevenstypen](https://feedback.azure.com/forums/263029-azure-search) pagina op de Azure Search UserVoice en eventuele aanvullende gegevens die u graag in overweging moet nemen met betrekking tot uitvoering van de functie opgeven. U kunt ook bereiken mij rechtstreeks op Twitter op @liamca.

