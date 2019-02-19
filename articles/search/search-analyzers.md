---
title: Analyse voor linguïstische en tekst verwerken - Azure Search
description: De analyzers toewijzen aan doorzoekbare velden in een index te vervangen standaard standard Lucene met aangepaste, vooraf gedefinieerde of taalspecifieke alternatieven.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: heidist
manager: cgronlun
author: HeidiSteen
ms.custom: seodec2018
ms.openlocfilehash: 7306258b6a7eee66df0961b2b993d0bcc9de94b9
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56343269"
---
# <a name="analyzers-for-text-processing-in-azure-search"></a>Analyzers om tekst te verwerken in Azure Search

Een *analyzer* is een onderdeel van de [engine voor zoekopdrachten in volledige tekst](search-lucene-query-architecture.md) verantwoordelijk voor het verwerken van tekst in querytekenreeksen en geïndexeerde documenten. Verschillende analyzers bewerken als tekst op verschillende manieren, afhankelijk van het scenario. Taalanalyse verwerken tekst met behulp van de linguïstische regels om te verbeteren van zoeken kwaliteit, terwijl andere analyzers eenvoudige taken, zoals tekens te converteren naar kleine letters, bijvoorbeeld uitvoeren. 

Taalanalyse zijn het meest worden gebruikt en er is standaard taalanalyse toegewezen aan elke doorzoekbaar veld van een Azure Search-index. De volgende taal transformaties zijn gangbaar tijdens de analyse van tekst:

+ Niet-essentiële woorden (stopwoorden) en leestekens worden verwijderd.
+ Zinnen en afgebroken woorden zijn onderverdeeld in onderdelen.
+ Hoofdletters woorden zijn lager-indeling.
+ Woorden worden verkort tot hoofdmap formulieren zodat een overeenkomst ongeacht de tegenwoordige kan worden gevonden.

Taal analyzers converteren tekstinvoer in primitief of root-formulieren zijn die op efficiënte informatie opslaan en ophalen. Conversie treedt op tijdens het indexeren, wanneer de index is gemaakt, en vervolgens nogmaals tijdens de zoekopdracht als de index wordt gelezen. U waarschijnlijk de zoekresultaten die u verwacht dat als u de dezelfde analyzer voor beide bewerkingen gebruiken.

## <a name="default-analyzer"></a>Standaard analyzer  

Maakt gebruik van Azure Search de [Apache Lucene Standard analyzer (standard lucene)](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) als de standaard, die tekst opgesplitst in de volgende elementen de ["Unicode-tekst segmentering"](https://unicode.org/reports/tr29/) regels. De standaard analyzer converteert bovendien alle tekens naar hun vorm kleine letters. Zowel geïndexeerde documenten en zoektermen gaat u door de analyse tijdens het indexeren en verwerking van query's.  

Het wordt automatisch gebruikt op elke doorzoekbaar veld. U kunt de standaardinstelling op basis van de door veld negeren. Alternatieve analyzers mag een [taalanalyse](index-add-language-analyzers.md), [aangepaste analysefunctie](index-add-custom-analyzers.md), of een vooraf gedefinieerde analyzer uit de [lijst met beschikbare analyzers](index-add-custom-analyzers.md#AnalyzerTable).


## <a name="types-of-analyzers"></a>Typen analyzers

De volgende lijst wordt beschreven welke analysefuncties zijn beschikbaar in Azure Search.

| Categorie | Description |
|----------|-------------|
| [Standard Lucene analyzer](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) | Standaard. Er is geen specificatie of configuratie vereist. Deze algemene analyzer wordt uitgevoerd voor de meeste scenario's en talen.|
| Vooraf gedefinieerde analyzers | Als een voltooide product bedoeld om te worden gebruikt als aangeboden-is. <br/>Er zijn twee typen: gespecialiseerde en taal. Wat is dan "vooraf gedefinieerde" het is u ernaar te verwijzen met de naam, zonder configuratie of aanpassen. <br/><br/>[(Taal-neutraal) analyzers gespecialiseerde](index-add-custom-analyzers.md#AnalyzerTable) worden gebruikt als invoer van tekst speciale verwerking of minimale vereist. Non-language predefined analyzers include **Asciifolding**, **Keyword**, **Pattern**, **Simple**, **Stop**, **Whitespace**.<br/><br/>[Taalanalyse](index-add-language-analyzers.md) worden gebruikt wanneer u geavanceerde linguïstische ondersteuning nodig hebt voor de afzonderlijke talen. Azure Search biedt ondersteuning voor 35 Lucene taalanalyse en 50 analyzers voor Microsoft-verwerking van natuurlijke taal. |
|[Analysevoorzieningen aanpassen](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search) | Verwijst naar een door de gebruiker gedefinieerde configuratie van een combinatie van bestaande elementen, die bestaat uit één tokenizer (vereist) en optionele filters (char of token).|

Een aantal vooraf gedefinieerde analysefuncties, zoals **patroon** of **stoppen**, ondersteuning voor een beperkt aantal configuratieopties. Deze opties wilt instellen, u effectief maken van een aangepaste analysefunctie die bestaat uit de vooraf gedefinieerde analyzer en een van de andere opties beschreven in [vooraf gedefinieerde Analyzer verwijzing](index-add-custom-analyzers.md#AnalyzerTable). Als met een aangepaste configuratie, Geef uw nieuwe configuratie met een naam, zoals *myPatternAnalyzer* te onderscheiden van het patroon van Lucene analyzer.

## <a name="how-to-specify-analyzers"></a>Analyzers opgeven

1. (voor alleen aangepaste analyse) Maak een benoemd exemplaar **analyzer** sectie in het definitie van de index. Zie voor meer informatie, [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) en ook [toevoegen van aangepaste analyse](index-add-custom-analyzers.md).

2. Op een [veld definitie](https://docs.microsoft.com/rest/api/searchservice/create-index) instellen in de index van het veld **analyzer** eigenschap op de naam van een analyse van het doel (bijvoorbeeld `"analyzer" = "keyword"`. Geldige waarden zijn de naam van een vooraf gedefinieerde analyzer, taal-analysefunctie of aangepaste analysefunctie ook in het indexschema zijn gedefinieerd. Plan analyzer toewijzen in de fase van de definitie van index voordat de index is gemaakt in de service.

3. (Optioneel) in plaats van een **analyzer** eigenschap, kunt u verschillende analyzers om te indexeren en query's uitvoeren met behulp van instellen de **indexAnalyzer** en **searchAnalyzer** veld de parameters. U kunt verschillende analyzers zou gebruiken voor gegevens voor te bereiden en het ophalen als een van deze activiteiten een specifieke transformatie niet nodig voor de andere vereist.

Toewijzen van **analyzer** of **indexAnalyzer** naar een veld dat al fysiek is gemaakt is niet toegestaan. Als een van deze onduidelijk, controleert u de volgende tabel voor een overzicht van deze acties opgebouwd en waarom.
 
 | Scenario | Impact | Stappen |
 |----------|--------|-------|
 | Een nieuw veld toevoegen | Minimale | Als het veld nog niet in het schema bestaat, is er geen veld revisie omdat het veld nog geen fysieke aanwezigheid in uw index. U kunt [Index bijwerken](https://docs.microsoft.com/rest/api/searchservice/update-index) naar een nieuw veld toevoegt aan een bestaande index en [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) voor het vullen van het.|
 | Voeg een **analyzer** of **indexAnalyzer** aan een bestaand geïndexeerde veld. | [Opnieuw maken](search-howto-reindex.md) | De omgekeerde index voor dat veld opnieuw vanaf de grond moet worden gemaakt en de inhoud voor deze velden moet worden geïndexeerd. <br/> <br/>Voor indexen in ontwikkeling, [verwijderen](https://docs.microsoft.com/rest/api/searchservice/delete-index) en [maken](https://docs.microsoft.com/rest/api/searchservice/create-index) de index om op te halen de velddefinitie van de nieuwe. <br/> <br/>Voor de indexen in de productieomgeving, kunt u opnieuw opbouwen uitstellen door het maken van een nieuw veld om te bieden de definitie van de herziene en gaan gebruiken in plaats van de oude heeft. Gebruik [Index bijwerken](https://docs.microsoft.com/rest/api/searchservice/update-index) om op te nemen van het nieuwe veld en [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) voor het vullen van het. Later, als onderdeel van de index voor gepland onderhoud, kunt u de index te verwijderen van verouderde velden opschonen. |

## <a name="when-to-add-analyzers"></a>Bij het toevoegen van analyse

Het beste moment kunt toevoegen en toewijzen van de analyse is tijdens het ontwikkelen van actieve, wanneer neer te zetten en opnieuw maken van indexen routine is.

Als een indexdefinitie inkoopketen, u nieuwe analysis-constructies aan een index toevoegen kunt, maar u moet doorgeven de **allowIndexDowntime** markering [Index bijwerken](https://docs.microsoft.com/rest/api/searchservice/update-index) als u wilt voorkomen dat deze fout:

*"De index update is niet toegestaan omdat dit leiden uitvaltijd tot zou. Om toe te voegen nieuwe analysefuncties, tokenizers, token filters of teken filters aan een bestaande index, stelt u de queryparameter 'allowIndexDowntime' op 'true' in de aanvraag van de update index. Houd er rekening mee dat uw index offline voor ten minste een paar seconden, waardoor uw indexeren en query-aanvragen mislukken door deze bewerking wordt geplaatst. Prestaties en schrijven van de beschikbaarheid van de index kan worden gehinderd voor enkele minuten nadat de index is bijgewerkt of langer voor zeer grote indexen."*

Hetzelfde geldt wanneer u een analyzer toewijst aan een veld. Een analyzer is een integraal onderdeel van de definitie van het veld, zodat u deze alleen toevoegen kunt wanneer het veld wordt gemaakt. Als u wilt de analyzers toevoegen aan bestaande velden, u zult moeten [verwijderen en opnieuw opbouwen](search-howto-reindex.md) de index, of een nieuw veld met de analyzer die u wilt toevoegen.

Zoals opgemerkt, een uitzondering is de **searchAnalyzer** variant. Van de drie manieren om op te geven analyzers (**analyzer**, **indexAnalyzer**, **searchAnalyzer**), alleen de **searchAnalyzer** kenmerk kan worden gewijzigd op een bestaand veld.

## <a name="recommendations-for-working-with-analyzers"></a>Aanbevelingen voor het werken met analysefuncties

Deze sectie vindt u advies over het werken met analysefuncties.

### <a name="one-analyzer-for-read-write-unless-you-have-specific-requirements"></a>Een analyzer voor lezen / schrijven, tenzij u specifieke vereisten hebt

Azure Search kunt u opgeven van verschillende analyzers voor indexeren en zoeken via extra **indexAnalyzer** en **searchAnalyzer** veld parameters. Als u niets opgeeft, wordt de analyzer instellen met de **analyzer** eigenschap wordt gebruikt voor zowel indexeren en zoeken. Als `analyzer` is opgegeven, wordt de standaard Standard Lucene analyzer gebruikt.

In het algemeen is het gebruik van de dezelfde analyzer voor indexering en uitvoeren van query's, tenzij u specifieke vereisten standaardclientconfiguratie. Moet u grondig testen. Wanneer de verwerking van de tekst op zoeken en indexeren tijd verschilt, loopt u het risico van toewijzingsbron query de voorwaarden en geïndexeerd wanneer de zoekopdracht en indexering analyzer configuraties zijn niet uitgelijnd.

### <a name="test-during-active-development"></a>Tijdens het ontwikkelen van actieve testen

Overschrijven van de standaard analyzer vereist het opnieuw opbouwen van een index. Indien mogelijk, bepalen op welke analyzers moet worden gebruikt tijdens de ontwikkeling, voordat u een index aanloop naar productie.

### <a name="inspect-tokenized-terms"></a>Tokens voorwaarden controleren

Als een zoekopdracht niet de verwachte resultaten worden geretourneerd, is het meest waarschijnlijke scenario token verschillen tussen de invoer van de termijn voor de query en tokens voorwaarden in de index. Als de tokens niet hetzelfde zijn, mislukt de overeenkomsten om te realiseren. Als u wilt controleren tokenizer-uitvoer, wordt u aangeraden de [analyseren API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) als een hulpprogramma voor onderzoek. De reactie bestaat van tokens, zoals die worden gegenereerd door een specifieke analyzer.

### <a name="compare-english-analyzers"></a>Engelse analyzers vergelijken

De [Search Analyzer-Demo](https://alice.unearth.ai/) is een derde partij demo-app van de vergelijking van de standaard Lucene analyzer, de Engelse taal-analysefunctie van Lucene en Microsofts Engelse natuurlijke taal processor van side-by-side. De index is opgelost; Deze bevat tekst uit een populaire verhaal. Voor elke invoer zoeken opgeeft u, resultaten van elke analyzer worden weergegeven in de deelvensters van de aangrenzende, zodat u een beeld van hoe elke analyzer dezelfde tekenreeks verwerkt. 

## <a name="examples"></a>Voorbeelden

De voorbeelden hieronder ziet u analyzer definities voor enkele belangrijke scenario's.

+ [Voorbeeld van de aangepaste analyzer](#Example1)
+ [Analyzers toewijzen aan een voorbeeld van een veld](#Example2)
+ [Met een combinatie van analysefuncties voor indexeren en zoeken](#Example3)
+ [Voorbeeld van de taal analyzer](#Example4)

<a name="Example1"></a>

### <a name="custom-analyzer-example"></a>Voorbeeld van de aangepaste analyzer

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

### <a name="per-field-analyzer-assignment-example"></a>Voorbeeld van een toewijzing analyzer per veld

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

### <a name="mixing-analyzers-for-indexing-and-search-operations"></a>Met een combinatie van analysefuncties voor indexeren en zoeken

De API's bevatten extra indexkenmerken voor het opgeven van verschillende analyzers voor indexeren en zoeken. De **searchAnalyzer** en **indexAnalyzer** kenmerken moeten worden opgegeven als een paar, vervangen de één **analyzer** kenmerk.


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

### <a name="language-analyzer-example"></a>Voorbeeld van de taal analyzer

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

+ Probeer van aanvullende query-syntaxis van de [documenten zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents#bkmk_examples) voorbeeld in deze sectie of [vereenvoudigde querysyntaxis](query-simple-syntax.md) in Search explorer in de portal.

+ Meer informatie over het toepassen van [taalspecifieke lexicale analyse](index-add-language-analyzers.md).

+ [Configureren van aangepaste analyse](index-add-custom-analyzers.md) voor minimale verwerking of gespecialiseerde verwerking op afzonderlijke velden.

+ [Standaard- en Engelse analyzers vergelijken](https://alice.unearth.ai/) in aangrenzende deelvensters op deze demo-website. 

## <a name="see-also"></a>Zie ook

 [REST-API voor Search-documenten](https://docs.microsoft.com/rest/api/searchservice/search-documents) 

 [Vereenvoudigde querysyntaxis](query-simple-syntax.md) 

 [Volledige Lucene-querysyntaxis](query-lucene-syntax.md) 
 
 [Zoekresultaten verwerken](search-pagination-page-layout.md)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
