---
title: Analyse voor linguïstische en tekst verwerken - Azure Search
description: De analyzers toewijzen aan doorzoekbare velden in een index te vervangen standaard standard Lucene met aangepaste, vooraf gedefinieerde of taalspecifieke alternatieven.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: heidist
manager: cgronlun
author: HeidiSteen
ms.custom: seodec2018
ms.openlocfilehash: 008a24fe9822ca51b81e1f6979a3731d794a8867
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55964335"
---
# <a name="analyzers-for-text-processing-in-azure-search"></a>Analyzers om tekst te verwerken in Azure Search

Een *analyzer* is een onderdeel van [zoeken in volledige tekst](search-lucene-query-architecture.md) verantwoordelijk voor het verwerken van tekst in querytekenreeksen en geïndexeerde documenten. De volgende transformaties zijn gangbaar tijdens de analyse:

+ Niet-essentiële woorden (stopwoorden) en leestekens worden verwijderd.
+ Zinnen en afgebroken woorden zijn onderverdeeld in onderdelen.
+ Hoofdletters woorden zijn lager-indeling.
+ Woorden worden verkort tot hoofdmap formulieren zodat een overeenkomst ongeacht de tegenwoordige kan worden gevonden.

Taalkundige analyse converteren tekstinvoer in primitief of root-formulieren zijn die op efficiënte informatie opslaan en ophalen. Conversie treedt op tijdens het indexeren, wanneer de index is gemaakt, en vervolgens nogmaals tijdens de zoekopdracht als de index wordt gelezen. U waarschijnlijk de zoekresultaten die u verwacht dat als u de dezelfde tekst analyzer voor beide bewerkingen gebruiken.

Maakt gebruik van Azure Search de [Standard Lucene analyzer](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) als standaardwaarde. U kunt de standaardinstelling op basis van de door veld negeren. In dit artikel wordt het bereik van de opties beschreven en biedt aanbevolen procedures voor het aangepaste analyse. Het biedt ook een van de voorbeeldconfiguraties voor belangrijke scenario's.

## <a name="supported-analyzers"></a>Ondersteunde analyzers

De volgende lijst wordt beschreven welke analyzers worden ondersteund in Azure Search.

| Categorie | Description |
|----------|-------------|
| [Standard Lucene analyzer](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) | Standaard. Er is geen specificatie of configuratie vereist. Deze algemene analyzer wordt uitgevoerd voor de meeste scenario's en talen.|
| Vooraf gedefinieerde analyzers | Als een voltooide product bedoeld om te worden gebruikt als aangeboden-is, met beperkte aanpassingen. <br/>Er zijn twee typen: gespecialiseerde en taal. Wat is dan "vooraf gedefinieerde" het is u ernaar te verwijzen met de naam, met geen aanpassingen. <br/><br/>[(Taal-neutraal) analyzers gespecialiseerde](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search#AnalyzerTable) worden gebruikt als invoer van tekst speciale verwerking of minimale vereist. Non-language predefined analyzers include **Asciifolding**, **Keyword**, **Pattern**, **Simple**, **Stop**, **Whitespace**.<br/><br/>[Taalanalyse](https://docs.microsoft.com/rest/api/searchservice/language-support) worden gebruikt wanneer u geavanceerde linguïstische ondersteuning nodig hebt voor de afzonderlijke talen. Azure Search biedt ondersteuning voor 35 Lucene taalanalyse en 50 analyzers voor Microsoft-verwerking van natuurlijke taal. |
|[Analysevoorzieningen aanpassen](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search) | Een gebruiker gedefinieerde configuratie van een combinatie van bestaande elementen, die bestaat uit één tokenizer (vereist) en optionele filters (char of token).|

U kunt een vooraf gedefinieerde analyzer, zoals **patroon** of **stoppen**, gebruik van andere opties beschreven in [vooraf gedefinieerde Analyzer verwijzing](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search#AnalyzerTable). Slechts een paar van de vooraf gedefinieerde analyzers hebt opties die u kunt instellen. Als met een aanpassing, Geef uw nieuwe configuratie met een naam, zoals *myPatternAnalyzer* te onderscheiden van het patroon van Lucene analyzer.

## <a name="how-to-specify-analyzers"></a>Analyzers opgeven

1. (voor alleen aangepaste analyse) Maak een **analyzer** sectie in het definitie van de index. Zie voor meer informatie, [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) en ook [aangepaste Analyzers > maken](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search#create-a-custom-analyzer).

2. Op een [veld definitie](https://docs.microsoft.com/rest/api/searchservice/create-index) in de index, stelt u de **analyzer** eigenschap op de naam van een analyse van het doel (bijvoorbeeld `"analyzer" = "keyword"`. Geldige waarden zijn de naam van een vooraf gedefinieerde analyzer, taal-analysefunctie of aangepaste analysefunctie ook in het indexschema zijn gedefinieerd.

3. (Optioneel) in plaats van een **analyzer** eigenschap, kunt u verschillende analyzers om te indexeren en query's uitvoeren met behulp van instellen de **indexAnalyzer** en **searchAnalyzer'** veld de parameters. 

3. Aan de velddefinitie van een toe te voegen een analyzer leidt tot een schrijfbewerking voor de index. Als u een **analyzer** aan een bestaande index, houd er rekening mee de volgende stappen uit:
 
 | Scenario | Impact | Stappen |
 |----------|--------|-------|
 | Een nieuw veld toevoegen | Minimale | Als het veld nog niet in het schema bestaat, is er geen veld revisie omdat het veld nog geen fysieke aanwezigheid in uw index. Gebruik [Index bijwerken](https://docs.microsoft.com/rest/api/searchservice/update-index) en [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) voor deze taak.|
 | Een analyzer toevoegen aan een bestaand geïndexeerde veld. | Opnieuw maken | De omgekeerde index voor dat veld moet worden gemaakt vanaf het begin van en de inhoud voor deze velden moet worden geïndexeerd. <br/> <br/>Voor indexen in ontwikkeling, [verwijderen](https://docs.microsoft.com/rest/api/searchservice/delete-index) en [maken](https://docs.microsoft.com/rest/api/searchservice/create-index) de index om op te halen de velddefinitie van de nieuwe. <br/> <br/>Voor indexen in de productieomgeving, moet u een nieuw veld om te bieden de definitie van de herziene en gaan gebruiken. Gebruik [Index bijwerken](https://docs.microsoft.com/rest/api/searchservice/update-index) en [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) om op te nemen van het nieuwe veld. Later, als onderdeel van de index voor gepland onderhoud, kunt u de index te verwijderen van verouderde velden opschonen. |

## <a name="tips-and-best-practices"></a>Tips en best practices

Deze sectie vindt u advies over het werken met analysefuncties.

### <a name="one-analyzer-for-read-write-unless-you-have-specific-requirements"></a>Een analyzer voor lezen / schrijven, tenzij u specifieke vereisten hebt

Azure Search kunt u opgeven van verschillende analyzers voor indexeren en zoeken via extra `indexAnalyzer` en `searchAnalyzer` veld parameters. Als u niets opgeeft, wordt de analyzer instellen met de `analyzer` eigenschap wordt gebruikt voor zowel indexeren en zoeken. Als `analyzer` is opgegeven, wordt de standaard Standard Lucene analyzer gebruikt.

In het algemeen is het gebruik van de dezelfde analyzer voor indexering en uitvoeren van query's, tenzij u specifieke vereisten standaardclientconfiguratie. Moet u grondig testen. Wanneer de verwerking van de tekst op zoeken en indexeren tijd verschilt, loopt u het risico van toewijzingsbron query de voorwaarden en geïndexeerd wanneer de zoekopdracht en indexering analyzer configuraties zijn niet uitgelijnd.

### <a name="test-during-active-development"></a>Tijdens het ontwikkelen van actieve testen

Overschrijven van de standaard analyzer vereist het opnieuw opbouwen van een index. Indien mogelijk, bepalen op welke analyzers moet worden gebruikt tijdens de ontwikkeling, voordat u een index aanloop naar productie.

### <a name="inspect-tokenized-terms"></a>Tokens voorwaarden controleren

Als een zoekopdracht niet de verwachte resultaten worden geretourneerd, is het meest waarschijnlijke scenario token verschillen tussen de invoer van de termijn voor de query en tokens voorwaarden in de index. Als de tokens niet hetzelfde zijn, mislukt de overeenkomsten om te realiseren. Als u wilt controleren tokenizer-uitvoer, wordt u aangeraden de [analyseren API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) als een hulpprogramma voor onderzoek. De reactie bestaat van tokens, zoals die worden gegenereerd door een specifieke analyzer.

### <a name="compare-english-analyzers"></a>Engelse analyzers vergelijken

De [Search Analyzer-Demo](https://alice.unearth.ai/) is een derde partij demo-app van de vergelijking van de standaard Lucene analyzer, de Engelse taal-analysefunctie van Lucene en Microsofts Engelse natuurlijke taal processor van side-by-side. De index is opgelost; Deze bevat tekst uit een populaire verhaal. Voor elke invoer zoeken opgeeft u, resultaten van elke analyzer worden weergegeven in de deelvensters van de aangrenzende, zodat u een beeld van hoe elke analyzer dezelfde tekenreeks verwerkt. 

## <a name="examples"></a>Voorbeelden

De voorbeelden hieronder ziet u analyzer definities voor enkele belangrijke scenario's.

<a name="Example1"></a>
### <a name="example-1-custom-options"></a>Voorbeeld 1: Aangepaste opties

In dit voorbeeld ziet u een definitie van een analyzer met aangepaste opties. Aangepaste opties voor char filters, tokenizers en token filters worden afzonderlijk opgegeven als benoemde constructies en vervolgens waarnaar wordt verwezen in de definitie van de analyzer. Vooraf gedefinieerde elementen worden gebruikt als-is en eenvoudig waarnaar wordt verwezen door de naam.

Stap voor stap in het volgende voorbeeld:

* Analysefuncties zijn een eigenschap van de veldklasse voor een doorzoekbaar veld.
* Een aangepaste analysefunctie maakt deel uit van een definitie van de index. Hierdoor kan licht worden aangepast (bijvoorbeeld één optie in één filter aanpassen) of op meerdere plaatsen worden aangepast.
* In dit geval wordt de aangepaste analyzer is 'my_analyzer', die op zijn beurt maakt gebruik van een aangepaste standard tokenizer "my_standard_tokenizer" en twee token filters: kleine letters en aangepaste asciifolding filter 'my_asciifolding'.
* Het definieert ook 2 aangepaste char filters 'map_dash' en 'remove_whitespace'. Het eerste item vervangt alle streepjes met onderstrepingstekens terwijl de tweede waarde alle spaties verwijdert. Opslagruimten moeten worden gecodeerd in de regels voor apparaatgroeptoewijzing van UTF-8. De char-filters worden toegepast voordat tokeniseren en is van invloed op de resulterende tokens (de standaard tokenizer-einden op streepjes en spaties, maar niet op onderstrepingsteken).

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"my_analyzer"
        }
     ],
     "analyzers":[
        {
           "name":"my_analyzer",
           "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
           "charFilters":[
              "map_dash",
              "remove_whitespace"
           ],
           "tokenizer":"my_standard_tokenizer",
           "tokenFilters":[
              "my_asciifolding",
              "lowercase"
           ]
        }
     ],
     "charFilters":[
        {
           "name":"map_dash",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["-=>_"]
        },
        {
           "name":"remove_whitespace",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["\\u0020=>"]
        }
     ],
     "tokenizers":[
        {
           "name":"my_standard_tokenizer",
           "@odata.type":"#Microsoft.Azure.Search.StandardTokenizerV2",
           "maxTokenLength":20
        }
     ],
     "tokenFilters":[
        {
           "name":"my_asciifolding",
           "@odata.type":"#Microsoft.Azure.Search.AsciiFoldingTokenFilter",
           "preserveOriginal":true
        }
     ]
  }
~~~~

<a name="Example2"></a>
### <a name="example-2-override-the-default-analyzer"></a>Voorbeeld 2: De standaard-analyzer overschrijven

De analysefunctie voor Standard is de standaardinstelling. Stel dat u wilt de standaardwaarde vervangen door een andere vooraf gedefinieerde analyzer, zoals de analyzer patroon. Als u geen aangepaste instellingen bent, moet u alleen opgeven met de naam in de velddefinitie van het.

Het element 'analyzer' overschrijft de standaard analyzer op basis van de door veld. Er is geen globale onderdrukking. In dit voorbeeld `text1` maakt gebruik van de analyzer patroon en `text2`, die een analyzer niet opgeeft, gebruikt de standaard.

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text1",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"pattern"
        },
        {
           "name":"text2",
           "type":"Edm.String",
           "searchable":true
        }
     ]
  }
~~~~

<a name="Example3"></a>
### <a name="example-3-different-analyzers-for-indexing-and-search-operations"></a>Voorbeeld 3: Verschillende analyzers voor indexeren en zoeken

De API's bevatten extra indexkenmerken voor het opgeven van verschillende analyzers voor indexeren en zoeken. De `searchAnalyzer` en `indexAnalyzer` kenmerken moeten worden opgegeven als een paar, vervangen de één `analyzer` kenmerk.


~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
     ],
  }
~~~~

<a name="Example4"></a>
### <a name="example-4-language-analyzer"></a>Voorbeeld 4: Taalanalyse

Velden met tekenreeksen in verschillende talen kunt een taalanalyse, terwijl andere velden de standaardwaarde (of enige andere vooraf gedefinieerde of aangepaste analyzer gebruiken). Als u een taalanalyse gebruikt, moet deze worden gebruikt voor indexeren en zoeken. Velden die gebruikmaken van een taalanalyse kan hebben verschillende analyzers voor indexeren en zoeken.

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
        {
           "name":"text_fr",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"fr.lucene"
        }
     ],
  }
~~~~

## <a name="next-steps"></a>Volgende stappen

+ Bekijk onze uitgebreide uitleg van [hoe volledige tekst zoeken werkt in Azure Search](search-lucene-query-architecture.md). In dit artikel bevat voorbeelden waarin wordt uitgelegd gedragingen die op het oppervlak vindt lijkt.

+ Probeer van aanvullende query-syntaxis van de [documenten zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents#bkmk_examples) voorbeeld in deze sectie of [vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) in Search explorer in de portal.

+ Meer informatie over het toepassen van [taalspecifieke lexicale analyse](https://docs.microsoft.com/rest/api/searchservice/language-support).

+ [Configureren van aangepaste analyse](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) voor minimale verwerking of gespecialiseerde verwerking op afzonderlijke velden.

+ [Standaard- en Engelse analyzers vergelijken](https://alice.unearth.ai/) in aangrenzende deelvensters op deze demo-website. 

## <a name="see-also"></a>Zie ook

 [REST-API voor Search-documenten](https://docs.microsoft.com/rest/api/searchservice/search-documents) 

 [Vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 

 [Volledige Lucene-querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 
 [Zoekresultaten verwerken](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
