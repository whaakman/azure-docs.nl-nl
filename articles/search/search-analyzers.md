---
title: Analyzers in Azure Search | Microsoft Docs
description: Analyzers toewijzen aan velden van doorzoekbare tekst in een index te vervangen standaard standaard Lucene met aangepaste, vooraf gedefinieerde of taalspecifieke alternatieven.
services: search
manager: jhubbard
author: HeidiSteen
documentationcenter: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 09/11/2017
ms.author: heidist
ms.openlocfilehash: 1b9dea2978c11955da3ea4df8b90dc10a866d3f1
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="analyzers-in-azure-search"></a>Analyse in Azure Search

Een *analyzer* is een onderdeel van [zoeken in volledige tekst](search-lucene-query-architecture.md) verantwoordelijk voor het verwerken van tekst in de queryreeksen en geïndexeerde documenten. De volgende transformaties zijn typische tijdens de analyse:

+ Niet-essentiële woorden (stopwoorden) en leestekens worden verwijderd.
+ Woordgroepen en afgebroken woorden zijn onderverdeeld in onderdelen.
+ Hoofdletters woorden zijn kleine geïntegreerd.
+ Woorden worden teruggebracht tot hoofdmap formulieren zodat een overeenkomst ongeacht tijd kan worden gevonden.

Taalkundige analyzers converteren een tekst invoer voor primitieve of hoofdmap formulieren die efficiënt zijn voor gegevens opslaan en ophalen. Conversie van deze gebeurtenis treedt op tijdens het indexeren, wanneer de index is samengesteld, en vervolgens nogmaals tijdens de zoekopdracht wanneer de index wordt gelezen. Is het meer waarschijnlijk de zoekresultaten die u verwacht dat als u de dezelfde tekst analyzer voor beide bewerkingen gebruiken ophalen.

Azure Search gebruikt de [standaard Lucene analyzer](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) als standaardwaarde. U kunt de standaardinstelling op basis van door veld negeren. Dit artikel beschrijft het bereik van de keuzes en aanbevolen procedures voor het aangepaste analyse biedt. Het bevat ook een van de voorbeeldconfiguraties voor belangrijke scenario's.

## <a name="supported-analyzers"></a>Ondersteunde analyzers

De volgende lijst wordt beschreven welke analyzers worden ondersteund in Azure Search.

| Category | Beschrijving |
|----------|-------------|
| [Standaard Lucene analyzer](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) | De standaardinstelling. Er is geen specificatie of configuratie vereist. Deze algemene analyzer voert ook voor de meeste scenario's en talen.|
| Vooraf gedefinieerde analyzers | Aangeboden als een voltooide product bedoeld om te worden gebruikt als-is met beperkte aanpassing. <br/>Er zijn twee typen: gespecialiseerde- en taalinstellingen. De reden waarom ze 'vooraf gedefinieerde', is dat u ernaar te met de naam met niets aanpassing verwijzen. <br/><br/>[Gespecialiseerde analyzers (taal networkdirect)](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search#AnalyzerTable) worden gebruikt als invoer voor tekst speciale verwerking of minimale vereist. Vooraf gedefinieerde niet taalanalyse omvatten **Asciifolding**, **sleutelwoord**, **patroon**, **eenvoudige**, **stoppen**, **Witruimte**.<br/><br/>[Taalanalyse](https://docs.microsoft.com/rest/api/searchservice/language-support) worden gebruikt wanneer u uitgebreide taalkundige ondersteuning nodig hebt voor afzonderlijke talen. Azure Search ondersteunt 35 Lucene taalanalyse en 50 analyzers voor Microsoft-verwerking van natuurlijke taal. |
|[Analysevoorzieningen aanpassen](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search) | Een door de gebruiker gedefinieerde configuratie uit een combinatie van bestaande elementen, die bestaan uit één tokenizer (vereist) en optionele filters (char of token).|

U kunt een vooraf gedefinieerde analyzer, zoals **patroon** of **stoppen**, gebruik van alternatieve opties beschreven in [vooraf gedefinieerde Analyzer verwijzing](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search#AnalyzerTable). Slechts enkele van de vooraf gedefinieerde analyzers hebt opties die u kunt instellen. Zoals met elke aanpassing, Geef uw nieuwe configuratie met een naam, zoals *myPatternAnalyzer* te onderscheiden van de analysefunctie voor Lucene-patroon.

## <a name="how-to-specify-analyzers"></a>Analyzers opgeven

1. (voor aangepaste analyzers alleen) Maak een **analyzer** sectie in de definitie van de index. Zie voor meer informatie [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) en ook [aangepaste Analyzers > maken](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search#create-a-custom-analyzer).

2. Op een [veld definitie](https://docs.microsoft.com/rest/api/searchservice/create-index) in de index, stelt u de **analyzer** eigenschap in op de naam van een doel analyzer (bijvoorbeeld `"analyzer" = "keyword"`. Geldige waarden zijn naam van een vooraf gedefinieerde analyzer, taalanalyse of aangepaste analyzer ook in het indexschema dat is gedefinieerd.

3. U kunt desgewenst in plaats van een **analyzer** eigenschap, kunt u verschillende analyzers voor indexering en uitvoeren van query's met behulp van instellen de **indexAnalyzer** en **searchAnalyzer'** veld parameters. 

3. Een analyzer toe te voegen aan de velddefinitie van een leidt ertoe dat een schrijfbewerking voor de index. Als u een **analyzer** aan een bestaande index Let op de volgende stappen uit:
 
 | Scenario | Impact | Stappen |
 |----------|--------|-------|
 | Een nieuw veld toevoegen | minimale | Als het veld nog niet in het schema bestaat, is er geen veld revisie omdat het veld heeft nog geen fysieke aanwezigheid in uw index. Gebruik [Index bijwerken](https://docs.microsoft.com/rest/api/searchservice/update-index) en [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) voor deze taak.|
 | Een analyzer toevoegen aan een bestaand geïndexeerde veld. | Opnieuw samenstellen | De omgekeerde index voor dat veld compleet moet worden gemaakt en de inhoud voor deze velden moet opnieuw worden geïndexeerd. <br/> <br/>Voor indexen momenteel actieve ontwikkeld [verwijderen](https://docs.microsoft.com/rest/api/searchservice/delete-index) en [maken](https://docs.microsoft.com/rest/api/searchservice/create-index) de index van de definitie van de nieuwe velden kunnen worden opgepikt. <br/> <br/>Voor de indexen in de productieomgeving, moet u een nieuw veld om de bijgewerkte definitie opgeven en vervolgens gebruiken maken. Gebruik [Index bijwerken](https://docs.microsoft.com/rest/api/searchservice/update-index) en [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) opnemen van het nieuwe veld. Onderhoud van geplande index later kunt u de index te verwijderen van verouderde velden opruimen. |

## <a name="tips-and-best-practices"></a>Tips en best practices

Deze sectie vindt u advies over het werken met analyzers.

### <a name="one-analyzer-for-read-write-unless-you-have-specific-requirements"></a>Een analyzer voor alleen-lezen tenzij er specifieke vereisten

Azure Search kunt u verschillende analyzers voor indexeren opgeven en zoek via aanvullende `indexAnalyzer` en `searchAnalyzer` veld parameters. Als u niets opgeeft, wordt de analyzer ingesteld met de `analyzer` eigenschap wordt gebruikt voor het indexeren en het zoeken. Als `analyzer` is niet is opgegeven, wordt de standaard standaard Lucene analyzer gebruikt.

In het algemeen is het gebruik van de dezelfde analyzer voor indexering en uitvoeren van query's, tenzij anders dicteren de specifieke vereisten. Zorg ervoor dat grondig te testen. Wanneer de verwerking van tekst op zoeken en indexeren tijd verschilt, loopt u het risico van de query de voorwaarden en geïndexeerde wanneer de zoekopdracht en indexering analyzer configuraties zijn niet uitgelijnd komt niet overeen.

### <a name="test-during-active-development"></a>Tijdens het ontwikkelen van actieve testen

Het overschrijven van de standaard analyzer vereist het opnieuw opbouwen van een index. Indien mogelijk, kiezen welke analyzers tijdens active ontwikkeling, alvorens een index in productie wilt gebruiken.

### <a name="inspect-tokenized-terms"></a>Tokens voorwaarden controleren

Als een zoekopdracht niet de verwachte resultaten worden geretourneerd, is het meest waarschijnlijke scenario token verschillen tussen de term in op de query- en tokens voorwaarden in de index. Als de tokens niet hetzelfde zijn, niet overeenkomt met voordoen. Als u wilt controleren tokenizer-uitvoer, wordt u aangeraden de [analyseren API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) als een hulpprogramma onderzoek. Het antwoord bestaat van tokens, zoals die worden gegenereerd door een specifieke analyzer.

### <a name="compare-english-analyzers"></a>Engels analyzers vergelijken

De [zoeken Analyzer Demo](http://alice.unearth.ai/) is een derde partij demo-app met een side-by-side vergelijking van de standaard Lucene analyzer, Lucene de Engelse taal analyzer en Microsoft Engelse natuurlijke taal processor. De index is vastgesteld; Deze bevat tekst uit een populaire artikel. Voor elke invoer zoeken opgeeft u, resultaten van elke analyzer worden weergegeven in de aangrenzende deelvensters, zodat u een beeld van hoe elk analyzer dezelfde tekenreeks verwerkt. 

## <a name="examples"></a>Voorbeelden

De volgende voorbeelden tonen analyzer definities voor enkele belangrijke scenario's.

<a name="Example1"></a>
### <a name="example-1-custom-options"></a>Voorbeeld 1: Aangepaste opties

In dit voorbeeld ziet u de definitie van een analyzer met aangepaste opties. Aangepaste opties voor char filters, tokenizers en token filters zijn opgegeven afzonderlijk als benoemde constructies en vervolgens waarnaar wordt verwezen in de definitie analyzer. Vooraf gedefinieerde elementen worden gebruikt als-is en gewoon waarnaar wordt verwezen door de naam.

Stap voor stap in dit voorbeeld:

* Analyzers zijn een eigenschap van de veldklasse voor een doorzoekbaar veld.
* Een aangepaste analyzer is onderdeel van de indexdefinitie van een. Hierdoor kan licht worden aangepast (bijvoorbeeld één optie in één filter aanpassen) of op meerdere plaatsen zijn aangepast.
* In dit geval wordt de aangepaste analyzer is 'my_analyzer', die weer gebruikmaakt van een aangepaste standaard tokenizer 'my_standard_tokenizer' en twee token filters: kleine letters en aangepaste asciifolding filter 'my_asciifolding'.
* Het definieert ook een aangepaste 'map_dash' char filter ter vervanging van alle streepjes met onderstrepingstekens voordat tokeniseren (de standaard tokenizer-einden op dash maar niet op onderstrepingsteken).

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
              "map_dash"
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
        }
     ],
     "tokenizers":[
        {
           "name":"my_standard_tokenizer",
           "@odata.type":"#Microsoft.Azure.Search.StandardTokenizer",
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
### <a name="example-2-override-the-default-analyzer"></a>Voorbeeld 2: De standaard analyzer overschrijven

De standaard analyzer is de standaardinstelling. Stel dat u wilt de standaard vervangen door een andere vooraf gedefinieerde analyzer, zoals de analyzer patroon. Als u geen aangepaste instellingsopties bent, moet u alleen opgeven met de naam in de velddefinitie van het.

Het element 'analyzer' overschrijft de standaard analyzer op basis van veld veld. Er is geen globale overschrijving. In dit voorbeeld `text1` maakt gebruik van de analyzer patroon en `text2`, die een analyzer niet opgeven, gebruikt de standaardinstallatielocatie.

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
### <a name="example-3-different-analyzers-for-indexing-and-search-operations"></a>Voorbeeld 3: Verschillende analyzers voor indexerings- en bewerkingen

De API's bevatten extra indexkenmerken voor het opgeven van verschillende analyzers voor indexeren en de zoekcriteria. De `searchAnalyzer` en `indexAnalyzer` kenmerken moeten worden opgegeven als een paar, vervangt het enkelvoudige `analyzer` kenmerk.


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

Velden met tekenreeksen in verschillende talen kunt een taalanalyse gebruiken, terwijl andere velden behoudt u de standaardwaarde (of een aantal andere vooraf gedefinieerde of aangepaste analyzer gebruiken). Als u een taalanalyse gebruikt, moet het worden gebruikt voor bewerkingen voor indexering en zoeken. Velden die gebruikmaken van een taalanalyse kan hebben verschillende analyzers voor indexering en zoeken.

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

+ Bekijk onze uitgebreide uitleg van [hoe volledige tekst zoeken werkt in Azure Search](search-lucene-query-architecture.md). In dit artikel bevat voorbeelden waarin wordt uitgelegd gedrag dat vindt op het eerste gezicht lijken.

+ Probeer extra querysyntaxis van de [documenten zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) voorbeeld in deze sectie of van [vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) in Search explorer in de portal.

+ Meer informatie over het toepassen van [taalspecifieke lexicale analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support).

+ [Configureren van aangepaste analyzers](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) voor minimale verwerking of speciale verwerking op afzonderlijke velden.

+ [Standard- en Engelse analyzers vergelijken](http://alice.unearth.ai/) in aangrenzende deelvensters op deze demo-website. 

## <a name="see-also"></a>Zie ook

 [REST-API documenten zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents) 

 [Vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 

 [Volledige Lucene-querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 
 [Zoekresultaten verwerken](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
