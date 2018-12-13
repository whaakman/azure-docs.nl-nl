---
title: Het model complexe gegevenstypen - Azure Search
description: Geneste of hiërarchische gegevensstructuren kunnen worden gemodelleerd in een Azure Search-index met behulp van platte rijenset en het gegevenstype van verzamelingen.
author: brjohnstmsft
manager: jlembicz
ms.author: brjohnst
tags: complex data types; compound data types; aggregate data types
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/01/2017
ms.custom: seodec2018
ms.openlocfilehash: 973623d6c4cb57518af2012bccf67c969146d23c
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53311980"
---
# <a name="how-to-model-complex-data-types-in-azure-search"></a>Hoe u complexe gegevenstypen modelleren in Azure Search
Externe gegevenssets die worden gebruikt voor het vullen van een Azure Search-index soms bevatten hiërarchische of geneste substructuren die niet omlaag netjes in een rijenset in tabelvorm verbreken. Voorbeelden van dergelijke structuren mogelijk meerdere locaties en telefoonnummers voor één klant, meerdere kleuren en formaten omvatten voor een enkele SKU, meerdere auteurs van een enkel rapport, enzovoort. Voorwaarden modelleren, ziet u deze structuren aangeduid als *complexe gegevenstypen*, *samengestelde gegevenstypen*, *samengestelde gegevenstypen*, of *cumulatieve gegevenstypen*, een paar te noemen.

Complexe gegevenstypen niet systeemeigen worden ondersteund in Azure Search, maar een bewezen oplossing omvat een proces in twee stappen plat maken van de structuur en klik vervolgens met behulp van een **verzameling** gegevenstype aan de binnenkant structuur reconstrueren. De techniek die beschreven in dit artikel te volgen kan de inhoud die moet worden doorzocht, meervoudige, gefilterd en gesorteerd.

## <a name="example-of-a-complex-data-structure"></a>Voorbeeld van een complexe gegevensstructuur
De gegevens in kwestie bevindt zich doorgaans voor als een set van JSON of XML-documenten, of als items in een NoSQL-archief, zoals Azure Cosmos DB. De uitdaging komen structureel, voort uit met meerdere onderliggende items die moeten worden doorzocht en gefilterd.  Als een beginpunt voor het aanduiden van de tijdelijke oplossing, voert u de volgende JSON-document met een lijst met een set met contactpersonen als voorbeeld:

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

Hoewel de velden met de naam 'id', 'name' en 'bedrijf' kunnen eenvoudig worden toegewezen als de velden in een Azure Search-index-op-een, is het veld 'locaties' bevat een matrix van locaties, die beide een set van locatie-id's, evenals de beschrijvingen van de locatie. Gezien het feit dat Azure Search heeft geen een gegevenstype dat wordt ondersteund, moeten we een andere manier om dit te modelleren in Azure Search. 

> [!NOTE]
> Deze techniek wordt ook beschreven door Kirk Evans in een blogbericht [Azure Cosmos DB met Azure Search indexeren](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), waarin een techniek die met de naam 'plat maken van de gegevens', waarbij u een veld met de naam moet `locationsID` en `locationsDescription` die geen van beide [verzamelingen](https://msdn.microsoft.com/library/azure/dn798938.aspx) (of een matrix met tekenreeksen).   
> 
> 

## <a name="part-1-flatten-the-array-into-individual-fields"></a>Deel 1: De matrix in afzonderlijke velden samenvoegen
Voor het maken van een Azure Search-index die geschikt is voor deze gegevensset, maakt u afzonderlijke velden voor de geneste substructure: `locationsID` en `locationsDescription` met het gegevenstype van [verzamelingen](https://msdn.microsoft.com/library/azure/dn798938.aspx) (of een matrix met tekenreeksen). In deze velden zou u de waarden '1' en '2' index in de `locationsID` veld voor John Smith en de waarden '3' & '4' in de `locationsID` veld voor Jen Campbell.  

Uw gegevens in Azure Search zou er als volgt: 

![voorbeeldgegevens, 2 rijen](./media/search-howto-complex-data-types/sample-data.png)

## <a name="part-2-add-a-collection-field-in-the-index-definition"></a>Deel 2: Een verzameling-veld in de definitie van de index toevoegen
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

## <a name="validate-search-behaviors-and-optionally-extend-the-index"></a>Zoekgedrag te valideren en (optioneel) de index uit te breiden
Ervan uitgaande dat u hebt gemaakt van de index en de gegevens laadde, kunt u nu testen van de oplossing om te controleren of de queryuitvoering zoeken op basis van de gegevensset. Elke **verzameling** veld moeten **doorzoekbare**, **Filterbaar** en **geschikt voor facetten**. U moet kunnen uitvoeren van query's zoals:

* Zoek alle mensen die in het hoofdkantoor van het Adventureworks werken.
* Krijg een telling van het aantal mensen die in een Home Office werken.  
* Weergeven van de mensen die op een startpagina Office werkt, welke andere kantoren ze samen met een telling van de personen op elke locatie werken.  

Waar deze techniek valt uit elkaar is wanneer u wilt een zoekopdracht die zowel de locatie-id als de beschrijving van de locatie worden gecombineerd. Bijvoorbeeld:

* Zoek alle personen waar ze een kantoor aan huis hebben en heeft een locatie-ID van 4.  

Als u de oorspronkelijke inhoud gezocht als volgt:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Echter nu dat we de gegevens in afzonderlijke velden zijn verdeeld, er is geen manier omdat u weet dat als de Home-Office voor Jen Campbell is gekoppeld aan `locationsID 3` of `locationsID 4`.  

Voor het afhandelen van deze aanvraag, definieert u een ander veld in de index waarin alle gegevens worden gecombineerd in één verzameling.  In ons voorbeeld noemen we dit veld `locationsCombined` en we zullen de inhoud met afzonderlijke een `||` maar u kunt een scheidingsteken voor duizendtallen waarvan u denkt dat een unieke set van tekens in voor uw inhoud zou zijn. Bijvoorbeeld: 

![voorbeeldgegevens, 2 rijen met scheidingsteken](./media/search-howto-complex-data-types/sample-data-2.png)

Met deze `locationsCombined` veld, we kunnen nu worden aangepast aan nog meer query's, zoals:

* Een aantal van de mensen die op een 'Home Office' met locatie-Id van '4 werken' weergeven.  
* Zoeken naar personen die bij een introductiepagina kantoor werken met locatie-Id '4'. 

## <a name="limitations"></a>Beperkingen
Dit is handig voor een aantal scenario's, maar het is niet van toepassing zijn in elk geval.  Bijvoorbeeld:

1. Als u geen een vaste set velden in uw complex gegevenstype en er is geen manier om te alle mogelijke typen worden toegewezen aan één veld. 
2. De geneste objecten bijwerken vereist extra bewerkingen uitvoeren om te bepalen wat er moet exact moet worden bijgewerkt in de Azure Search-index

## <a name="sample-code"></a>Voorbeeldcode
U ziet een voorbeeld van hoe u een complexe reeks van de JSON-gegevens in Azure Search-index en een aantal query's uitvoeren via deze gegevensset op deze [GitHub-opslagplaats](https://github.com/liamca/AzureSearchComplexTypes).

## <a name="next-step"></a>Volgende stap
[Stem voor systeemeigen ondersteuning voor complexe gegevenstypen](https://feedback.azure.com/forums/263029-azure-search) in de Azure Search UserVoice-pagina en geef eventuele aanvullende invoer die u graag ons te houden met betrekking tot implementatie. U kunt ook contact me rechtstreeks op Twitter op @liamca.

