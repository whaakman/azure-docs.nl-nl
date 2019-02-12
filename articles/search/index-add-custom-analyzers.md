---
title: Aangepaste analyse - Azure Search toevoegen
description: Tekst tokenizers en teken filters gebruikt in Azure Search volledige-tekstquery's wijzigen.
ms.date: 01/31/2019
services: search
ms.service: search
ms.topic: conceptual
author: Yahnoosh
ms.author: jlembicz
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 150510ec09744b1350a93bde4e2a4dcb141867c0
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56008284"
---
# <a name="add-custom-analyzers-to-an-azure-search-index"></a>Analysevoorzieningen aanpassen aan een Azure Search-index toevoegen

Een *aangepaste analysefunctie* is een door de gebruiker opgegeven combinatie van tokenizer en optionele filters gebruikt voor het aanpassen van tekst te verwerken in de zoekmachine. Bijvoorbeeld, kunt u een aangepaste analysefunctie met een *char filter* HTML-opmaak verwijderen voordat teksten worden getokeniseerd.

## <a name="overview"></a>Overzicht

 De rol van een [zoeken in volledige tekst engine](search-lucene-query-architecture.md), simpel gezegd is om te verwerken en opslaan van documenten op een manier waarmee efficiënte query's en ophalen van bestanden. Op hoog niveau gaat alle omlaag om belangrijke woorden extraheren van documenten, ze in een index te plaatsen en vervolgens met behulp van de index om te zoeken naar documenten die overeenkomen met woorden van een bepaalde query. Het proces van het extraheren van woorden van documenten en zoekopdrachten wordt lexicale analyse genoemd. Onderdelen die lexicale analyse uitvoeren worden analyzers genoemd.

 In Azure Search kunt u kiezen uit een set vooraf gedefinieerde agnostische taalanalyse in de [Analyzers](#AnalyzerTable) tabel- en taalinstellingen specifieke analyzers die worden vermeld in [taalanalyse &#40;Azure Search Service REST API&#41;](index-add-language-analyzers.md). U hebt ook een optie voor het definiëren van uw eigen aangepaste analyse.  

 Een aangepaste analysefunctie kunt u controle over het proces van het converteren van tekst in tokens worden geïndexeerd en doorzoekbaar. Het is een gebruiker gedefinieerde configuratie die bestaan uit een enkele vooraf gedefinieerde tokenizer, een of meer token filters en een of meer char-filters. Het tokenizer is verantwoordelijk voor het verdelen van tekst in tokens en tokens filters voor het wijzigen van tokens die zijn gegenereerd door de tokenizer. CHAR-filters worden toegepast voor invoertekst voorbereiden voordat deze wordt verwerkt door het tokenizer. Char-filter kan bijvoorbeeld bepaalde tekens of symbolen vervangen.

 Populaire mogelijke scenario's met aangepaste analysefuncties zijn onder andere:  

-   Fonetische zoeken. Een fonetische filter om in te schakelen zoeken op basis van hoe een woord klinkt, niet hoe het gespeld toevoegen.  

-   Lexicale analyse uitschakelen. Het sleutelwoord analyzer gebruiken om te maken van doorzoekbare velden die niet worden geanalyseerd.  

-   Snelle voorvoegsel/achtervoegsel zoeken. Het token Edge N-gram-filter toevoegen aan voorvoegsels van woorden om in te schakelen snelle voorvoegsel die overeenkomt met index. Combineer deze met het filter voor omgekeerde token om u te achtervoegsel die overeenkomt met.  

-   Aangepaste tokeniseren. De witruimte tokenizer bijvoorbeeld gebruiken om zinnen in met behulp van spaties als scheidingsteken  

-   ASCII-vouwen. Voeg de standaard ASCII vouwen filter voor het normaliseren van diakritische tekens zoals ö of ê in zoektermen.  

 U kunt meerdere aangepaste analysefuncties voor het variëren van de combinatie van filters definiëren, maar elk veld kan alleen een analyzer gebruiken voor het indexeren van analyse en één voor search-analyse.  

 Deze pagina bevat een lijst met ondersteunde analyzers, tokenizers token filters en char filters. U vindt hier ook een beschrijving van wijzigingen in de definitie van de index met een voorbeeld van gebruik. Zie voor meer achtergrondinformatie over de onderliggende technologie gebruikt in de implementatie van Azure Search [Analysis pakket samenvatting (Lucene)](https://lucene.apache.org/core/4_10_0/core/org/apache/lucene/codecs/lucene410/package-summary.html). Zie voor meer voorbeelden van configuraties analyzer [analyse in Azure Search > voorbeelden](https://docs.microsoft.com/azure/search/search-analyzers#examples).


## <a name="default-analyzer"></a>Standaard analyzer  

Standaard doorzoekbare velden in Azure Search worden geanalyseerd met de [Apache Lucene Standard analyzer (standard lucene)](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) die tekst opgesplitst in de volgende elementen de ['Segmentering Unicode-tekst'](https://unicode.org/reports/tr29/) regels. De standaard analyzer converteert bovendien alle tekens naar hun vorm kleine letters. Zowel geïndexeerde documenten en zoektermen gaat u door de analyse tijdens het indexeren en verwerking van query's.  

 Dit wordt automatisch gebruikt in elk doorzoekbaar veld, tenzij u expliciet met een andere analyzer in het velddefinitie overschrijven. Alternatieve analyzers mag een aangepaste analysefunctie of een andere vooraf gedefinieerde analyzer uit de lijst met beschikbare [Analyzers](#AnalyzerTable) hieronder.

## <a name="validation-rules"></a>Validatieregels  
 Namen van analyse, tokenizers token filters en char filters moeten uniek zijn en mag niet hetzelfde als een van de vooraf gedefinieerde analyzers, tokenizers, token filters of char filters. Zie de [eigenschapverwijzing](#PropertyReference) voor namen al in gebruik.

## <a name="create-a-custom-analyzer"></a>Een aangepaste analysefunctie maken
 U kunt aangepaste analyse tijdens het maken van een index definiëren. De syntaxis voor het opgeven van een aangepaste analysefunctie wordt in deze sectie beschreven. U kunt Maak uzelf ook vertrouwd met de syntaxis van de aan de hand van voorbeeld-definities in [analyse in Azure Search](https://docs.microsoft.com/azure/search/search-analyzers#examples).  

 De definitie van een analyzer bevat een naam, type, een of meer char-filters, maximaal één tokenizer en een of meer filters token voor de verwerking van na tokeniseren. CHAR-filter worden toegepast voordat tokeniseren. Token filters en char filters worden toegepast van links naar rechts.

 De `tokenizer_name` is de naam van een tokenizer `token_filter_name_1` en `token_filter_name_2` zijn de namen van token filters en `char_filter_name_1` en `char_filter_name_2` zijn de namen van char filters (Zie de [Tokenizers](#Tokenizers), [ Token filters](#TokenFilters) en [Char filters](#CharFilters) tabellen voor geldige waarden).

De definitie van de analyzer is een onderdeel van de grotere index. Zie [API ' Create Index '](https://docs.microsoft.com/rest/api/searchservice/create-index) voor informatie over de rest van de index.

```  
"analyzers":(optional)[  
   {  
      "name":"name of analyzer",  
      "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",  
      "charFilters":[  
         "char_filter_name_1",  
         "char_filter_name_2"  
      ],  
      "tokenizer":"tokenizer_name",  
      "tokenFilters":[  
         "token_filter_name_1",  
         "token_filter_name_2"  
      ]  
   },  
   {  
      "name":"name of analyzer",  
      "@odata.type":"#analyzer_type",  
      "option1":value1,  
      "option2":value2,  
      ...  
   }  
],  
"charFilters":(optional)[  
   {  
      "name":"char_filter_name",  
      "@odata.type":"#char_filter_type",  
      "option1":value1,  
      "option2":value2,  
      ...  
   }  
],  
"tokenizers":(optional)[  
   {  
      "name":"tokenizer_name",  
      "@odata.type":"#tokenizer_type",  
      "option1":value1,  
      "option2":value2,  
      ...  
   }  
],  
"tokenFilters":(optional)[  
   {  
      "name":"token_filter_name",  
      "@odata.type":"#token_filter_type",  
      "option1":value1,  
      "option2":value2,  
      ...  
   }  
]  
```  

> [!NOTE]  
>  Analysevoorzieningen aanpassen die u maakt, worden niet weergegeven in de Azure-portal. De enige manier om toe te voegen een aangepaste analysefunctie is via code aanroepen naar de API waarmee u bij het definiëren van een index.  

 Binnen een definitie van de index, kunt u een willekeurige plaats in deze sectie plaatsen in de hoofdtekst van een aanvraag van de index maken, maar meestal gaat aan het einde:  

```  
{  
  "name": "name_of_index",  
  "fields": [ ],  
  "suggesters": [ ],  
  "scoringProfiles": [ ],  
  "defaultScoringProfile": (optional) "...",  
  "corsOptions": (optional) { },  
  "analyzers":(optional)[ ],  
  "charFilters":(optional)[ ],  
  "tokenizers":(optional)[ ],  
  "tokenFilters":(optional)[ ]  
}  
```  

Definities voor char filters, tokenizers en token filters worden toegevoegd aan de index alleen als u aangepaste opties instelt. Een bestaand filter of tokenizer als te gebruiken-is, geef deze op naam in de definitie van de analyzer.

<a name="Testing custom analyzers"></a>
## <a name="test-a-custom-analyzer"></a>Een aangepaste analysefunctie testen

U kunt de **Test Analyzer bewerking** in de [REST-API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) om te zien hoe een analyzer-einden opgegeven tekst in tokens.

**Aanvraag**
~~~~
  POST https://[search service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
  Content-Type: application/json
    api-key: [admin key]

  {
     "analyzer":"my_analyzer",
     "text": "Vis-à-vis means Opposite"
  }
~~~~
**Antwoord**
~~~~
  {
    "tokens": [
      {
        "token": "vis_a_vis",
        "startOffset": 0,
        "endOffset": 9,
        "position": 0
      },
      {
        "token": "vis_à_vis",
        "startOffset": 0,
        "endOffset": 9,
        "position": 0
      },
      {
        "token": "means",
        "startOffset": 10,
        "endOffset": 15,
        "position": 1
      },
      {
        "token": "opposite",
        "startOffset": 16,
        "endOffset": 24,
        "position": 2
      }
    ]
  }
 ~~~~

 ## <a name="update-a-custom-analyzer"></a>Een aangepaste analysefunctie bijwerken

Zodra een analyzer, een tokenizer, een token filter of een char-filter is gedefinieerd, kan niet worden gewijzigd. Nieuwe waarden kunnen worden toegevoegd aan een bestaande index alleen als de `allowIndexDowntime` vlag is ingesteld op ' True ' in de aanvraag van de update index:

~~~~
PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true
~~~~

Met deze bewerking duurt voordat uw index offline ten minste een paar seconden, waardoor uw verzoeken, indexeren en query's mislukken. Prestaties en schrijven van de beschikbaarheid van de index kan worden gehinderd voor enkele minuten nadat de index bijgewerkt of langer voor zeer grote indexen is, maar deze effecten zijn tijdelijk en uiteindelijk op hun eigen oplossen.

 <a name="ReferenceIndexAttributes"></a>

## <a name="index-attribute-reference"></a>De kenmerkverwijzing index

De onderstaande tabellen lijst van de configuratie-eigenschappen voor de analyzers, tokenizers, token filters en char filter-sectie van de indexdefinitie van een. De structuur van een analyzer, tokenizer, of -filter in de index is samengesteld uit deze kenmerken. Zie voor meer informatie de toewijzing van waarde de [eigenschapverwijzing](#PropertyReference).

 ### <a name="analyzers"></a>Analyses

Voor analyse, indexkenmerken variëren, afhankelijk van of u vooraf gedefinieerde of aangepaste analyse.

#### <a name="predefined-analyzers"></a>Vooraf gedefinieerde Analyzers

|||  
|-|-|  
|Name|Deze mag alleen letters, cijfers, spaties, streepjes of onderstrepingstekens bevatten, kunnen alleen starten en eindigen met alfanumerieke tekens bevatten en is beperkt tot 128 tekens.|  
|Type|Analyzer type in de lijst van ondersteunde analyzers. Zie de **analyzer_type** kolom in de [Analyzers](#AnalyzerTable) in de volgende tabel.|  
|Opties|Moet geldige opties van een vooraf gedefinieerde analyzer die worden vermeld in de [Analyzers](#AnalyzerTable) in de volgende tabel.|  

#### <a name="custom-analyzers"></a>Analysevoorzieningen aanpassen

|||  
|-|-|  
|Name|Deze mag alleen letters, cijfers, spaties, streepjes of onderstrepingstekens bevatten, kunnen alleen starten en eindigen met alfanumerieke tekens bevatten en is beperkt tot 128 tekens.|  
|Type|Moet '#Microsoft.Azure.Search.CustomAnalyzer'.|  
|CharFilters|Ingesteld op een van de vooraf gedefinieerde char filters worden vermeld de [Char Filters](#CharFilter) tabel of een aangepaste char-filter dat is opgegeven in de indexdefinitie.|  
|Tokenizer|Vereist. Ingesteld op een van de vooraf gedefinieerde tokenizers die worden vermeld in de [Tokenizers](#Tokenizers) onderstaande tabel of een aangepaste tokenizer opgegeven in de indexdefinitie.|  
|TokenFilters|Ingesteld op een van de vooraf gedefinieerde token filters die worden vermeld in de [Token filters](#TokenFilters) tabel of een aangepaste token filter dat is opgegeven in de indexdefinitie.|  

<a name="CharFilter"></a>

 ### <a name="char-filters"></a>CHAR-Filters

 Een char-filter wordt gebruikt om voor te bereiden invoertekst voordat deze wordt verwerkt door het tokenizer. Ze kunnen bijvoorbeeld bepaalde tekens of symbolen vervangen. U kunt meerdere char filters in een aangepaste analysefunctie hebben. CHAR filters die worden uitgevoerd in de volgorde waarin ze worden weergegeven.  

|||  
|-|-|  
|Name|Deze mag alleen letters, cijfers, spaties, streepjes of onderstrepingstekens bevatten, kunnen alleen starten en eindigen met alfanumerieke tekens bevatten en is beperkt tot 128 tekens.|  
|Type|CHAR filtertype in de lijst van ondersteunde char filters. Zie **char_filter_type** kolom in de [Char Filters](#CharFilter) in de volgende tabel.|  
|Opties|Moet geldige opties van een bepaalde [Char Filters](#CharFilter) type.|  

 ### <a name="tokenizers"></a>Tokenizers

 Een tokenizer verdeelt doorlopende tekst in een reeks van tokens, zoals het analyseren van een zin in woorden.  

 U kunt één tokenizer per aangepaste analysefunctie opgeven. Als u meer dan één tokenizer nodig hebt, kunt u meerdere aangepaste analyse maken en toewijzen op basis van velden in uw indexschema.  
Een aangepaste analysefunctie kunt u een vooraf gedefinieerde tokenizer gebruiken met standaard of aangepaste opties.  

|||  
|-|-|  
|Name|Deze mag alleen letters, cijfers, spaties, streepjes of onderstrepingstekens bevatten, kunnen alleen starten en eindigen met alfanumerieke tekens bevatten en is beperkt tot 128 tekens.|  
|Type|De naam van de tokenizer uit de lijst met ondersteunde tokenizers. Zie **tokenizer_type** kolom in de [Tokenizers](#Tokenizers) in de volgende tabel.|  
|Opties|Moet geldige opties van een bepaalde tokenizer type die worden vermeld in de [Tokenizers](#Tokenizers) in de volgende tabel.|  

 ### <a name="token-filters"></a>Token filters

 Een token filter wordt gebruikt om te filteren of wijzigen van de tokens die worden gegenereerd door een tokenizer. Bijvoorbeeld, kunt u een kleine filter dat alle tekens converteert naar kleine letters.   
U kunt meerdere token filters in een aangepaste analysefunctie hebben. Token filters uitgevoerd in de volgorde waarin ze worden weergegeven.  

|||  
|-|-|  
|Name|Deze mag alleen letters, cijfers, spaties, streepjes of onderstrepingstekens bevatten, kunnen alleen starten en eindigen met alfanumerieke tekens bevatten en is beperkt tot 128 tekens.|  
|Type|De naam van de token filter in de lijst van ondersteunde token filters. Zie **token_filter_type** kolom in de [Token filters](#TokenFilters) in de volgende tabel.|  
|Opties|Moet [Token filters](#TokenFilters) van een filtertype voor het opgegeven token.|  

<a name="PropertyReference"></a>  

## <a name="property-reference"></a>Eigenschapverwijzing

Deze sectie bevat de geldige waarden voor kenmerken die zijn opgegeven in de definitie van een aangepaste analysefunctie, tokenizer, char-filter of token filter in uw index. Analyse, tokenizers en filters die zijn geïmplementeerd met behulp van Apache Lucene hebt koppelingen naar de Lucene-API-documentatie.

<a name="AnalyzerTable"></a>

###  <a name="predefined-analyzers-reference"></a>Vooraf gedefinieerde Analyzers verwijzing

|**analyzer_name**|**analyzer_type**  <sup>1</sup>|**Beschrijving en opties**|  
|-|-|-|  
|[keyword](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html)| (type geldt alleen wanneer opties zijn beschikbaar) |De volledige inhoud van een veld beschouwd als een enkele token. Dit is handig voor gegevens zoals postcodes, -id's en sommige productnamen.|  
|[pattern](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/PatternAnalyzer.html)|PatternAnalyzer|Flexibel worden gescheiden tekst in voorwaarden via een reguliere-expressiepatroon.<br /><br /> **Opties**<br /><br /> kleine letters (type: bool)-bepaalt of voorwaarden zijn met kleine letters. De standaardwaarde is true.<br /><br /> [patroon](https://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html?is-external=true) (type: string)-een reguliere-expressiepatroon zodat deze overeenkomen met token scheidingstekens. De standaardwaarde is \w+.<br /><br /> [vlaggen](https://docs.oracle.com/javase/6/docs/api/java/util/regex/Pattern.html#field_summary) (type: string)-vlaggen voor reguliere expressie. De standaardwaarde is een lege tekenreeks. Toegestane waarden: CANON_EQ, CASE_INSENSITIVE, OPMERKINGEN, DOTALL, LETTERLIJKE WAARDE, MET MEERDERE REGELS, UNICODE_CASE, UNIX_LINES<br /><br /> stopwoorden (type: string-matrix)-een lijst met stopwoorden. De standaardwaarde is een lege lijst.|  
|[Eenvoudige](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/SimpleAnalyzer.html)|(type geldt alleen wanneer opties zijn beschikbaar) |Tekst met een niet-letters deelt en zet deze om in kleine letters. |  
|[standard](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) <br />(Ook wel standard.lucene genoemd)|StandardAnalyzer|Standard Lucene analyzer, bestaat uit de standard tokenizer, kleine letters filteren en stop-filter.<br /><br /> **Opties**<br /><br /> maxTokenLength (type: int)-de maximale lengte van de token. De standaardwaarde is 255. Tokens die langer zijn dan de maximale lengte worden gesplitst. Maximale lengte van de token die kan worden gebruikt, is 300 tekens.<br /><br /> stopwoorden (type: string-matrix)-een lijst met stopwoorden. De standaardwaarde is een lege lijst.|  
|standardasciifolding.lucene|(type geldt alleen wanneer opties zijn beschikbaar) |Standard analyzer met Ascii filter vouwen. |  
|[Stoppen](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/StopAnalyzer.html)|StopAnalyzer|Tekst met een niet-letters deelt, de kleine en stopwoord token filters toegepast.<br /><br /> **Opties**<br /><br /> stopwoorden (type: string-matrix)-een lijst met stopwoorden. De standaardwaarde is een vooraf gedefinieerde lijst voor Engels. |  
|[whitespace](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/WhitespaceAnalyzer.html)|(type geldt alleen wanneer opties zijn beschikbaar) |Een analyzer die gebruikmaakt van het tokenizer witruimte. Tokens die langer dan 255 tekens zijn worden gesplitst.|  

 <sup>1</sup> analyzer typen worden altijd voorafgegaan in code met '#Microsoft.Azure.Search' zodat "PatternAnalyzer" daadwerkelijk zou worden opgegeven als '#Microsoft.Azure.Search.PatternAnalyzer'. We hebben het voorvoegsel voor beknopt alternatief verwijderd de, maar voorvoegsel is vereist in uw code. 
 
De analyzer_type is alleen beschikbaar voor de analyzers die kunnen worden aangepast. Als er geen opties, zijn zoals het geval is met het sleutelwoord analyzer, is er geen bijbehorende #Microsoft.Azure.Search-type.


<a name="CharFilter"></a>

###  <a name="char-filters-reference"></a>CHAR Filters verwijzing

In de onderstaande tabel worden de tekens filters die zijn geïmplementeerd met behulp van Apache Lucene gekoppeld aan de Lucene-API-documentatie.

|**char_filter_name**|**char_filter_type** <sup>1</sup>|**Beschrijving en opties**|  
|-|-|-|
|[html_strip](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/charfilter/HTMLStripCharFilter.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Er wordt een char-filter waarmee wordt geprobeerd het verwijderen van HTML.|  
|[mapping](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/charfilter/MappingCharFilter.html)|MappingCharFilter|Een char-filter die van toepassing gedefinieerd met de optie toewijzingen toewijzingen is. Overeenkomende is greedy (langste gebruik van jokertekens op een bepaald moment wins). Vervanging is toegestaan op de lege tekenreeks.<br /><br /> **Opties**<br /><br /> toewijzingen (type: string-matrix)-een lijst met toewijzingen van de volgende indeling: "een = > b ' (alle exemplaren van het teken"a"zijn vervangen door het teken"b"). Vereist.|  
|[pattern_replace](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/pattern/PatternReplaceCharFilter.html)|PatternReplaceCharFilter|Een char-filter die tekens in de invoerreeks vervangt. Maakt gebruik van een reguliere expressie voor het identificeren van tekenreeksen te behouden en een vervangende patroon voor het identificeren van de tekens wilt vervangen. Bijvoorbeeld, invoertekst = "aa-bb aa bb", pattern="(aa)\\\s+(bb)" vervanging = "$1 #$2", resultaat = "aa #bb aa #bb".<br /><br /> **Opties**<br /><br /> patroon (type: string) - vereist.<br /><br /> vervanging (type: string) - vereist.|  

 <sup>1</sup> filtertypen char worden altijd voorafgegaan in code met '#Microsoft.Azure.Search' zodat "MappingCharFilter" daadwerkelijk zou worden opgegeven als '#Microsoft.Azure.Search.MappingCharFilter. We hebben verwijderd het voorvoegsel voor het verminderen van de breedte van de tabel, maar houd er rekening mee te nemen in uw code. Houd er rekening mee dat char_filter_type is alleen van toepassing op de filters die kunnen worden aangepast. Als er geen opties, zijn zoals het geval is bij html_strip, is er geen bijbehorende #Microsoft.Azure.Search-type.

<a name="Tokenizers"></a>

###  <a name="tokenizers-reference"></a>Tokenizers verwijzing

In de onderstaande tabel worden de tokenizers die zijn geïmplementeerd met behulp van Apache Lucene gekoppeld aan de Lucene-API-documentatie.

|**tokenizer_name**|**tokenizer_type** <sup>1</sup>|**Beschrijving en opties**|  
|-|-|-|  
|[Klassiek](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/standard/ClassicTokenizer.html)|ClassicTokenizer|Grammatica gebaseerd tokenizer dat geschikt is voor de verwerking van documenten in de meeste Europese-taal.<br /><br /> **Opties**<br /><br /> maxTokenLength (type: int)-de maximale lengte van de token. Standaard: 255, maximum: 300. Tokens die langer zijn dan de maximale lengte worden gesplitst.|  
|[edgeNGram](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/ngram/EdgeNGramTokenizer.html)|EdgeNGramTokenizer|Basis van de invoer van een rand woordgrenzen in n-gram/maten gegeven.<br /><br /> **Opties**<br /><br /> minGram (type: int)-standaard: 1, maximum: 300.<br /><br /> maxGram (type: int) - Default: 2, maximum: 300. Moet groter zijn dan minGram.<br /><br /> tokenChars (type: string-matrix)-teken van klassen in de tokens wilt behouden. Toegestane waarden: <br />"letter", "digit", "whitespace", "punctuation", "symbol". Standaard ingesteld op een lege matrix: alle tekens houdt. |  
|[keyword_v2](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/KeywordTokenizer.html)|KeywordTokenizerV2|Verzendt de volledige invoer als een enkele token.<br /><br /> **Opties**<br /><br /> maxTokenLength (type: int)-de maximale lengte van de token. Standaard: 256, maximum: 300. Tokens die langer zijn dan de maximale lengte worden gesplitst.|  
|[letter](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/LetterTokenizer.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Tekst met een niet-letters deelt. Tokens die langer dan 255 tekens zijn worden gesplitst.|  
|[Kleine letters](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/LowerCaseTokenizer.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Tekst met een niet-letters deelt en zet deze om in kleine letters. Tokens die langer dan 255 tekens zijn worden gesplitst.|  
| microsoft_language_tokenizer| MicrosoftLanguageTokenizer| Tekst met behulp van regels voor taalspecifieke deelt.<br /><br /> **Opties**<br /><br /> maxTokenLength (type: int)-de maximale lengte van de token, standaard: 255, maximum: 300. Tokens die langer zijn dan de maximale lengte worden gesplitst. Tokens die langer is dan 300 tekens eerst worden opgesplitst in tokens met lengte 300 en vervolgens elk van deze tokens wordt verdeeld, is afhankelijk van de set maxTokenLength.<br /><br />isSearchTokenizer (type: bool) - is ingesteld op true als gebruikt als de zoekopdracht tokenizer, ingesteld op false als gebruikt als de indexering tokenizer. <br /><br /> taal (type: string)-taal die u wilt gebruiken, standaard "Engels". Toegestane waarden zijn:<br />"Bengaals", "Bulgaars", "Catalaans", "chineseSimplified", "chineseTraditional", "Kroatische", "Tsjechische", "Deense", "Nederlandse", "Engels", "Frankrijk", "Duitsland", "Grieks", "gujarati", "hindi", "IJslandse", "Indonesische", "Italiaans", "Japanse", "kannada" " Koreaans","Maleis","malayalam","marathi","norwegianBokmaal","Pools","Portugees","portugueseBrazilian","punjabi","Roemeens","Russische","serbianCyrillic","serbianLatin","Sloveens","Spaans","Zweedse","tamil","telugu","Thais"" Oekraïens","urdu","Vietnamees" |
| microsoft_language_stemming_tokenizer | MicrosoftLanguageStemmingTokenizer| Tekst met behulp van regels voor taalspecifieke deelt en minder woorden naar hun basis formulieren<br /><br /> **Opties**<br /><br />maxTokenLength (type: int)-de maximale lengte van de token, standaard: 255, maximum: 300. Tokens die langer zijn dan de maximale lengte worden gesplitst. Tokens die langer is dan 300 tekens eerst worden opgesplitst in tokens met lengte 300 en vervolgens elk van deze tokens wordt verdeeld, is afhankelijk van de set maxTokenLength.<br /><br /> isSearchTokenizer (type: bool) - is ingesteld op true als gebruikt als de zoekopdracht tokenizer, ingesteld op false als gebruikt als de indexering tokenizer.<br /><br /> taal (type: string)-taal die u wilt gebruiken, standaard "Engels". Toegestane waarden zijn:<br />"[NULL]"Arabisch","Bengaals","Bulgaars","Catalaans","Kroatische","Tsjechische","Deense","Nederlandse","Engels","Estisch","Fins", Frans,"Duitsland","Grieks","gujarati","Hebreeuwse","hindi","Hongaars","IJslandse","Indonesische","Italiaans","kannada"" Lets","Litouwse","Maleis","malayalam","marathi","norwegianBokmaal","Pools","Portugees","portugueseBrazilian","punjabi","Roemeens","Russische","serbianCyrillic","serbianLatin","Slowaaks","Sloveens","Spaans","Zweedse"" Tamil","telugu","Turkse","Oekraïense","Oerdoe" |
|[nGram](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/ngram/NGramTokenizer.html)|NGramTokenizer|Basis van de invoer in de opgegeven/maten n-gram woordgrenzen.<br /><br /> **Opties**<br /><br /> minGram (type: int)-standaard: 1, maximum: 300.<br /><br /> maxGram (type: int) - Default: 2, maximum: 300. Moet groter zijn dan minGram. <br /><br /> tokenChars (type: string-matrix)-teken van klassen in de tokens wilt behouden. Toegestane waarden: "letter", "cijfer", "witruimte", "leestekens',"symbool". Standaard ingesteld op een lege matrix: alle tekens houdt. |  
|[path_hierarchy_v2](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/path/PathHierarchyTokenizer.html)|PathHierarchyTokenizerV2|Tokenizer voor pad-achtige hiërarchieën.<br /><br /> **Opties**<br /><br /> scheidingsteken (type: string)-standaard: ' /.<br /><br /> vervanging (type: string) - als ingesteld, vervangt het scheidingsteken. Standaard hetzelfde als de waarde van het scheidingsteken.<br /><br /> maxTokenLength (type: int)-de maximale lengte van de token. Standaard: 300, maximum: 300. Paden langer dan maxTokenLength worden genegeerd.<br /><br /> omgekeerde (type: bool): als deze waarde true is, in omgekeerde volgorde-token wordt gegenereerd. Standaard: false.<br /><br /> overslaan (type: bool)-tokens om over te slaan voor de eerste. De standaardwaarde is 0.|  
|[pattern](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/pattern/PatternTokenizer.html)|PatternTokenizer|Deze tokenizer maakt gebruik van regex-patroon zoeken om afzonderlijke tokens samen te stellen.<br /><br /> **Opties**<br /><br /> patroon (type: string)-reguliere-expressiepatroon. De standaardwaarde is \w+.<br /><br /> [vlaggen](https://docs.oracle.com/javase/6/docs/api/java/util/regex/Pattern.html#field_summary) (type: string)-vlaggen voor reguliere expressie. De standaardwaarde is een lege tekenreeks. Toegestane waarden: CANON_EQ, CASE_INSENSITIVE, OPMERKINGEN, DOTALL, LETTERLIJKE WAARDE, MET MEERDERE REGELS, UNICODE_CASE, UNIX_LINES<br /><br /> groep (type: int)-welke groep om op te halen in tokens. De standaardwaarde is 1 (splitsen).|  
|[standard_v2](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/standard/StandardTokenizer.html)|StandardTokenizerV2|Regeleinden tekst na de [Unicode-tekst segmentering regels](https://unicode.org/reports/tr29/).<br /><br /> **Opties**<br /><br /> maxTokenLength (type: int)-de maximale lengte van de token. Standaard: 255, maximum: 300. Tokens die langer zijn dan de maximale lengte worden gesplitst.|  
|[uax_url_email](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/standard/UAX29URLEmailTokenizer.html)|UaxUrlEmailTokenizer|Basis van URL's en e-mailberichten woordgrenzen als een token.<br /><br /> **Opties**<br /><br /> maxTokenLength (type: int)-de maximale lengte van de token. Standaard: 255, maximum: 300. Tokens die langer zijn dan de maximale lengte worden gesplitst.|  
|[whitespace](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/WhitespaceTokenizer.html)|(type geldt alleen wanneer opties zijn beschikbaar) |Verdeelt tekst bij een witruimte zijn. Tokens die langer dan 255 tekens zijn worden gesplitst.|  

 <sup>1</sup> tokenizer-typen worden altijd voorafgegaan in code met '#Microsoft.Azure.Search' zodat "ClassicTokenizer" daadwerkelijk zou worden opgegeven als '#Microsoft.Azure.Search.ClassicTokenizer'. We hebben verwijderd het voorvoegsel voor het verminderen van de breedte van de tabel, maar houd er rekening mee te nemen in uw code. Houd er rekening mee dat tokenizer_type is alleen van toepassing op tokenizers die kunnen worden aangepast. Als er geen opties, zijn zoals het geval is met de letter tokenizer, is er geen bijbehorende #Microsoft.Azure.Search-type.

<a name="TokenFilters"></a>

###  <a name="token-filters-reference"></a>Token Filters verwijzing

In de onderstaande tabel worden de token filters die zijn geïmplementeerd met behulp van Apache Lucene gekoppeld aan de Lucene-API-documentatie.

|**token_filter_name**|**token_filter_type** <sup>1</sup>|**Beschrijving en opties**|  
|-|-|-|  
|[arabic_normalization](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/ar/ArabicNormalizationFilter.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Een filter van het token dat van toepassing de Arabische normalisatiefunctie is voor het normaliseren van de orthography.|  
|[apostrof](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/tr/ApostropheFilter.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Verwijdert alle tekens na een enkel aanhalingsteken (met inbegrip van de apostrof zelf). |  
|[asciifolding](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html)|AsciiFoldingTokenFilter|Converteert letters, cijfers en symbolische Unicode-tekens die niet in de eerste 127 ASCII-tekens (de "Latijn" Unicode-blok) in hun equivalenten ASCII als er een bestaat.<br /><br /> **Opties**<br /><br /> preserveOriginal (type: bool) - als true, het oorspronkelijke token wordt bewaard. De standaardwaarde is false.|  
|[cjk_bigram](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/cjk/CJKBigramFilter.html)|CjkBigramTokenFilter|Bigrams van CJK termen die worden gegenereerd op basis van StandardTokenizer vormt.<br /><br /> **Opties**<br /><br /> ignoreScripts (type: string-matrix)-Scripts moeten worden genegeerd. Toegestane waarden zijn: 'han","hiragana","katakana", 'hangul'. De standaardwaarde is een lege lijst.<br /><br /> outputUnigrams (type: bool): ingesteld op true als u wilt dat altijd om uit te voeren unigrams zowel bigrams. De standaardwaarde is false.|  
|[cjk_width](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/cjk/CJKWidthFilter.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Normaliseert CJK breedte verschillen. Vouwen volledige breedte ASCII-varianten in de equivalente basic Latijnse en halve breedte Katakana varianten in de equivalente kana. |  
|[Klassiek](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/standard/ClassicFilter.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Hiermee verwijdert u de Engelse bezittelijke voornaamwoorden, en punten van acroniemen. |  
|[common_grams](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/commongrams/CommonGramsFilter.html)|CommonGramTokenFilter|Samenstellen bigrams voor vaak optreedt voorwaarden tijdens het indexeren. Enkelvoudige termen zijn nog steeds te, geïndexeerd met bigrams ze elkaar overlappen.<br /><br /> **Opties**<br /><br /> commonWords (type: string-matrix)-de set met veelvoorkomende woorden. De standaardwaarde is een lege lijst. Vereist.<br /><br /> ignoreCase (type: bool) - als true, onderscheid wordt. De standaardwaarde is false.<br /><br /> queryMode (type: bool) - bigrams genereert vervolgens verwijdert u veelvoorkomende woorden en enkelvoudige termen, gevolgd door een algemene woord. De standaardwaarde is false.|  
|[dictionary_decompounder](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/compound/DictionaryCompoundWordTokenFilter.html)|DictionaryDecompounderTokenFilter|Ontleedt samengestelde woorden gevonden in vele Germaanse talen.<br /><br /> **Opties**<br /><br /> Woordenlijst (type: string-matrix)-de lijst met woorden die u wilt vergelijken met. De standaardwaarde is een lege lijst. Vereist.<br /><br /> minWordSize (type: int)-alleen woorden die langer zijn dan deze verwerkt. De standaardwaarde is 5.<br /><br /> minSubwordSize (type: int)-alleen subwords langer zijn dan deze worden gegenereerd. De standaardwaarde is 2.<br /><br /> maxSubwordSize (type: int)-alleen subwords korter zijn dan deze worden gegenereerd. De standaardwaarde is 15.<br /><br /> onlyLongestMatch (type: bool)-toevoegen van alleen de langste overeenkomende subword om uit te voeren. De standaardwaarde is false.|  
|[edgeNGram_v2](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/ngram/EdgeNGramTokenFilter.html)|EdgeNGramTokenFilterV2|N-grammen van de opgegeven/maten genereert niet kan worden gestart vanaf het begin of de achterkant van een invoer-token.<br /><br /> **Opties**<br /><br /> minGram (type: int)-standaard: 1, maximum: 300.<br /><br /> maxGram (type: int) - Default: 2, maximaal 300. Moet groter zijn dan minGram.<br /><br /> zijde (type: string)-Hiermee geeft u op welke kant van de invoer van de n-gram moet worden gegenereerd op basis van. Toegestane waarden: 'front-end', 'terug' |  
|[elision](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/util/ElisionFilter.html)|ElisionTokenFilter|Hiermee verwijdert u elisions. Bijvoorbeeld, wordt "l'avion" (de gegevenslaag) geconverteerd naar 'avion' (vlak).<br /><br /> **Opties**<br /><br /> artikelen (type: string-matrix): een reeks artikelen te verwijderen. De standaardwaarde is een lege lijst. Als er geen lijst met artikelen instellen, standaard worden alle Franse artikelen verwijderd.|  
|[german_normalization](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/de/GermanNormalizationFilter.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Normaliseert Duitse tekens op basis van de methodiek van de [German2 snowball algoritme](https://snowball.tartarus.org/algorithms/german2/stemmer.html) .|  
|[hindi_normalization](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/hi/HindiNormalizationFilter.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Normaliseert tekst in Hindi enkele verschillen in spellingvariaties verwijderen. |  
|[indic_normalization](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/in/IndicNormalizationFilter.html)|IndicNormalizationTokenFilter|De Unicode-weergave van tekst in Indiase talen normaliseert.
|[behouden](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/KeepWordFilter.html)|KeepTokenFilter|Een filter van het token dat alleen houdt u alle tokens met tekst in de opgegeven lijst met woorden.<br /><br /> **Opties**<br /><br /> keepWords (type: string-matrix)-een lijst met woorden te houden. De standaardwaarde is een lege lijst. Vereist.<br /><br /> keepWordsCase (type: bool) - als true, kleine alle woorden eerst case. De standaardwaarde is false.|  
|[keyword_marker](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/KeywordMarkerFilter.html)|KeywordMarkerTokenFilter|Termen als trefwoorden markeert.<br /><br /> **Opties**<br /><br /> trefwoorden (type: string-matrix)-een lijst met woorden die u wilt markeren als trefwoorden. De standaardwaarde is een lege lijst. Vereist.<br /><br /> ignoreCase (type: bool) - als true, kleine alle woorden eerst case. De standaardwaarde is false.|  
|[keyword_repeat](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/KeywordRepeatFilter.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Elke binnenkomende token twee keer één keer als het sleutelwoord en één keer als niet-sleutelwoord verzendt. |  
|[kstem](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/en/KStemFilter.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Een krachtige kstem-filter voor Engels. |  
|[Lengte](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/LengthFilter.html)|LengthTokenFilter|Hiermee verwijdert u woorden die te lang of te kort zijn.<br /><br /> **Opties**<br /><br /> min (type: int)-het minimale aantal. Standaard: 0, maximum: 300.<br /><br /> Max (type: int)-het maximum aantal. Standaard: 300, maximum: 300.|  
|[Limiet](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/LimitTokenCountFilter.html)|Microsoft.Azure.Search.LimitTokenFilter|Beperkt het aantal tokens tijdens het indexeren.<br /><br /> **Opties**<br /><br /> maxTokenCount (type: int)-het maximumaantal tokens te produceren. De standaardwaarde is 1.<br /><br /> consumeAllTokens (type: bool) - of alle tokens uit de invoer moeten worden gebruikt, zelfs als maxTokenCount is bereikt. De standaardwaarde is false.|  
|[Kleine letters](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/LowerCaseFilter.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Normaliseert token tekst naar kleine letters. |  
|[nGram_v2](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/ngram/NGramTokenFilter.html)|NGramTokenFilterV2|N-grammen van de opgegeven/maten genereert.<br /><br /> **Opties**<br /><br /> minGram (type: int)-standaard: 1, maximum: 300.<br /><br /> maxGram (type: int) - Default: 2, maximaal 300. Moet groter zijn dan minGram.|  
|[pattern_capture](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/pattern/PatternCaptureGroupTokenFilter.html)|PatternCaptureTokenFilter|Maakt gebruik van Java regexes om te verzenden van meerdere tokens, één voor elke groep vastleggen in een of meer patronen.<br /><br /> **Opties**<br /><br /> patronen (type: string-matrix)-een lijst van padpatronen voor elk token. Vereist.<br /><br /> preserveOriginal (type: bool): ingesteld op true om terug te keren van het oorspronkelijke token, zelfs als een van de patronen overeenkomt, standaard: true |  
|[pattern_replace](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/pattern/PatternReplaceFilter.html)|PatternReplaceTokenFilter|Een filter van het token dat een patroon geldt voor elk token in de stroom overeenkomst exemplaren vervangen door de opgegeven vervangende tekenreeks.<br /><br /> **Opties**<br /><br /> patroon (type: string) - vereist.<br /><br /> vervanging (type: string) - vereist.|  
|[persian_normalization](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/fa/PersianNormalizationFilter.html)|(type geldt alleen wanneer opties zijn beschikbaar) |Is van toepassing normalisering voor Perzisch. |  
|[phonetic](https://lucene.apache.org/core/4_10_3/analyzers-phonetic/org/apache/lucene/analysis/phonetic/package-tree.html)|PhoneticTokenFilter|Tokens voor fonetische overeenkomsten maken.<br /><br /> **Opties**<br /><br /> coderingsprogramma (type: string)-fonetische coderingsprogramma te gebruiken. Toegestane waarden zijn: 'metaphone","doubleMetaphone","geluid","refinedSoundex","caverphone1","caverphone2","Keulen","nysiis","koelnerPhonetik","haasePhonetik", 'beiderMorse'. Standaard: 'metaphone'. De standaardwaarde is metaphone.<br /><br /> Zie [encoder](https://commons.apache.org/proper/commons-codec/archives/1.10/apidocs/org/apache/commons/codec/language/package-frame.html) voor meer informatie.<br /><br /> Vervang (type: bool) - waar, als gecodeerde tokens oorspronkelijke tokens, false vervangen moeten als ze moeten worden toegevoegd als synoniemen. De standaardwaarde is true.|  
|[porter_stem](https://lucene.apache.org/core/4_10_3/analyzers-common/org/tartarus/snowball/ext/PorterStemmer.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Transformeert de token stream volgens de [Porter afvlakking algoritme](https://tartarus.org/~martin/PorterStemmer/). |  
|[reverse](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/reverse/ReverseStringFilter.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Keert de token tekenreeks. |  
|[scandinavian_normalization](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/ScandinavianNormalizationFilter.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Normaliseert gebruik van de uitwisselbaar Scandinavische tekens. |  
|[scandinavian_folding](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/ScandinavianFoldingFilter.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Vouwen Scandinavische tekens åÅäæÄÆ -> een en öÖøØ-o >. Deze ook van toepassing op basis van gebruik van dubbele klinkers aa, ae, door de ao, oe en oo, zodat alleen de eerste record. |  
|[shingle](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/shingle/ShingleFilter.html)|ShingleTokenFilter|Hiermee maakt u combinaties van tokens als een enkele token.<br /><br /> **Opties**<br /><br /> maxShingleSize (type: int)-standaard ingesteld op 2.<br /><br /> minShingleSize (type: int)-standaard ingesteld op 2.<br /><br /> outputUnigrams (type: bool) - als true, de uitvoerstroom de invoer-tokens (unigrams) evenals dakbinten bevat. De standaardwaarde is true.<br /><br /> outputUnigramsIfNoShingles (type: bool): als deze waarde true is, overschrijven de werking van outputUnigrams == false in voor de tijdstippen als er geen dakbinten beschikbaar zijn. De standaardwaarde is false.<br /><br /> tokenSeparator (type: string)-de tekenreeks die moet worden bij het toevoegen van de aangrenzende tokens gebruiken om te vormen een hangen de vlag. De standaardwaarde is "".<br /><br /> filterToken (type: string)-de tekenreeks om in te voegen voor elke functie waarop er geen token is. De standaardwaarde is '_'.|  
|[snowball](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/snowball/SnowballFilter.html)|SnowballTokenFilter|Snowball Token Filter.<br /><br /> **Opties**<br /><br /> taal (type: string)-waarden zijn toegestaan: "Armeense", "Baskisch", "Catalaans", "Deense", "Nederlandse", "Engels", "Fins", "Frankrijk", "Duitsland", "german2", "Hongaars", "Italiaans", "kp", "lovins", "Noorse", "porter", "Portugees", "Roemeens", " Russisch","Spaans","Zweedse","Turks"|  
|[sorani_normalization](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/ckb/SoraniNormalizationFilter.html)|SoraniNormalizationTokenFilter|De Unicode-weergave van Sorani tekst normaliseert.<br /><br /> **Opties**<br /><br /> Geen.|  
|Ontleding|StemmerTokenFilter|Taalspecifieke afvlakking filter.<br /><br /> **Opties**<br /><br /> taal (type: string)-toegestane waarden zijn onder andere: <br /> -   ["arabic"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/ar/ArabicStemmer.html)<br />-   ["Armeense"](https://snowball.tartarus.org/algorithms/armenian/stemmer.html)<br />-   ["Baskisch"](https://snowball.tartarus.org/algorithms/basque/stemmer.html)<br />-   ["Braziliaans"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/br/BrazilianStemmer.html)<br />-   ["Bulgaars"](https://members.unine.ch/jacques.savoy/Papers/BUIR.pdf)<br />-   ["Catalaans"](https://snowball.tartarus.org/algorithms/catalan/stemmer.html)<br />-   ["czech"](https://portal.acm.org/citation.cfm?id=1598600)<br />-   ["Deense"](https://snowball.tartarus.org/algorithms/danish/stemmer.html)<br />-   ["dutch"](https://snowball.tartarus.org/algorithms/dutch/stemmer.html)<br />-   ["dutchKp"](https://snowball.tartarus.org/algorithms/kraaij_pohlmann/stemmer.html)<br />-   ["Engels"](https://snowball.tartarus.org/algorithms/porter/stemmer.html)<br />-   ["lightEnglish"](https://ciir.cs.umass.edu/pubfiles/ir-35.pdf)<br />-   ["minimalEnglish"](https://www.researchgate.net/publication/220433848_How_effective_is_suffixing)<br />-   ["possessiveEnglish"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/en/EnglishPossessiveFilter.html)<br />-   ["porter2"](https://snowball.tartarus.org/algorithms/english/stemmer.html)<br />-   ["lovins"](https://snowball.tartarus.org/algorithms/lovins/stemmer.html)<br />-   ["Fins"](https://snowball.tartarus.org/algorithms/finnish/stemmer.htm)<br />-   ["lightFinnish"](https://clef.isti.cnr.it/2003/WN_web/22.pdf)<br />-   ["french"](https://snowball.tartarus.org/algorithms/french/stemmer.html)<br />-   ["lightFrench"](https://dl.acm.org/citation.cfm?id=1141523)<br />-   ["minimalFrench"](https://dl.acm.org/citation.cfm?id=318984)<br />-   ["Galicisch"](https://bvg.udc.es/recursos_lingua/stemming.jsp)<br />-   ["minimalGalician"](https://bvg.udc.es/recursos_lingua/stemming.jsp)<br />-   ["german"](https://snowball.tartarus.org/algorithms/german/stemmer.html)<br />-   ["german2"](https://snowball.tartarus.org/algorithms/german2/stemmer.html)<br />-   ["lightGerman"](https://dl.acm.org/citation.cfm?id=1141523)<br />-   ["minimalGerman"](https://members.unine.ch/jacques.savoy/clef/morpho.pdf)<br />-   ["greek"](https://sais.se/mthprize/2007/ntais2007.pdf)<br />-   ["hindi"](https://computing.open.ac.uk/Sites/EACLSouthAsia/Papers/p6-Ramanathan.pdf)<br />-   ["Hongaars"](https://snowball.tartarus.org/algorithms/hungarian/stemmer.html)<br />-   ["lightHungarian"](https://dl.acm.org/citation.cfm?id=1141523&dl=ACM&coll=DL&CFID=179095584&CFTOKEN=80067181)<br />-   ["Indonesische"](https://www.illc.uva.nl/Publications/ResearchReports/MoL-2003-02.text.pdf)<br />-   ["Ierse"](https://snowball.tartarus.org/otherapps/oregan/intro.html)<br />-   ["Italiaans"](https://snowball.tartarus.org/algorithms/italian/stemmer.html)<br />-   ["lightItalian"](https://www.ercim.eu/publication/ws-proceedings/CLEF2/savoy.pdf)<br />-   ["sorani"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/ckb/SoraniStemmer.html)<br />-   ["Lets"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/lv/LatvianStemmer.html)<br />-   ["Noorse"](https://snowball.tartarus.org/algorithms/norwegian/stemmer.html)<br />-   ["lightNorwegian"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/no/NorwegianLightStemmer.html)<br />-   ["minimalNorwegian"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/no/NorwegianMinimalStemmer.html)<br />-   ["lightNynorsk"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/no/NorwegianLightStemmer.html)<br />-   ["minimalNynorsk"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/no/NorwegianMinimalStemmer.html)<br />-   ["Portugees"](https://snowball.tartarus.org/algorithms/portuguese/stemmer.html)<br />-   ["lightPortuguese"](https://dl.acm.org/citation.cfm?id=1141523&dl=ACM&coll=DL&CFID=179095584&CFTOKEN=80067181)<br />-   ["minimalPortuguese"](https://www.inf.ufrgs.br/~buriol/papers/Orengo_CLEF07.pdf)<br />-   ["portugueseRslp"](https://www.inf.ufrgs.br//~viviane/rslp/index.htm)<br />-   ["Roemeens"](https://snowball.tartarus.org/algorithms/romanian/stemmer.html)<br />-   ["Russische"](https://snowball.tartarus.org/algorithms/russian/stemmer.html)<br />-   ["lightRussian"](https://doc.rero.ch/lm.php?url=1000%2C43%2C4%2C20091209094227-CA%2FDolamic_Ljiljana_-_Indexing_and_Searching_Strategies_for_the_Russian_20091209.pdf)<br />-   ["Spaans"](https://snowball.tartarus.org/algorithms/spanish/stemmer.html)<br />-   ["lightSpanish"](https://www.ercim.eu/publication/ws-proceedings/CLEF2/savoy.pdf)<br />-   ["Zweedse"](https://snowball.tartarus.org/algorithms/swedish/stemmer.html)<br />-   ["lightSwedish"](https://clef.isti.cnr.it/2003/WN_web/22.pdf)<br />-   ["Turkse"](https://snowball.tartarus.org/algorithms/turkish/stemmer.html)|  
|[stemmer_override](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/StemmerOverrideFilter.html)|StemmerOverrideTokenFilter|Alle termen woordenlijst voort zijn gemarkeerd als trefwoorden, waardoor deze niet kan afleiding verder in de keten. Moet worden geplaatst voordat afvlakking filters.<br /><br /> **Opties**<br /><br /> regels (type: string-matrix)-als gevolg van regels in de volgende indeling ' word = > stam ' bijvoorbeeld ' uitgevoerd = > uitvoeren '. De standaardwaarde is een lege lijst.  Vereist.|  
|[stopwords](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/StopFilter.html)|StopwordsTokenFilter|Hiermee verwijdert u stopwoorden via een token stroom. Standaard wordt met het filter een lijst met vooraf gedefinieerde stop word gebruikt voor Engels.<br /><br /> **Opties**<br /><br /> stopwoorden (type: string-matrix)-een lijst met stopwoorden. Kan niet worden opgegeven als een stopwordsList is opgegeven.<br /><br /> stopwordsList (type: string)-een vooraf gedefinieerde lijst stopwoorden. Kan niet worden opgegeven als stopwoorden is opgegeven. Toegestane waarden zijn: 'Arabisch","Armeense","Baskisch","Braziliaans","Bulgaars","Catalaans","Tsjechische","Deense","Nederlandse","Engels","Fins","Frans","Galicisch","Duitsland","Grieks","hindi","Hongaars","Indonesische","Ierse","Italiaans","Lets ","Noors","Perzische","Portugees","Roemeens","Russische","sorani","Spaans","Zweedse","Thais","Turkse", standaard:"Engels". Kan niet worden opgegeven als stopwoorden is opgegeven. <br /><br /> ignoreCase (type: bool): als deze waarde true is, alle woorden zijn lager omhuld eerste. De standaardwaarde is false.<br /><br /> removeTrailing (type: bool): als deze waarde true is, de laatste zoekterm negeren als het een woord stoppen. De standaardwaarde is true.
|[synonym](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/synonym/SynonymFilter.html)|SynonymTokenFilter|Komt overeen met één of meerdere word synoniemen in een stream van het token.<br /><br /> **Opties**<br /><br /> synoniemen (type: string-matrix) - vereist. Lijst met synoniemen in de volgende twee verschillende indelingen:<br /><br /> -ongelooflijke, onvoorstelbare, fantastische = > fantastische - alle voorwaarden aan de linkerkant van = > symbool aan alle voorwaarden aan de rechterkant worden vervangen.<br /><br /> -ongelooflijke, onvoorstelbare, geweldig, fantastische - een door komma's gescheiden lijst met gelijkwaardige woorden. Stel de optie uit te breiden om te wijzigen hoe deze lijst wordt geïnterpreteerd.<br /><br /> ignoreCase (type: bool)-invoer voor het afstemmen van Case-vouwen. De standaardwaarde is false.<br /><br /> Vouw (type: bool) - als true, alle woorden in de lijst met synoniemen (als = > notation wordt niet gebruikt) worden toegewezen aan elkaar. <br />De volgende lijst: fantastische ongelooflijke, onvoorstelbare, geweldig, is gelijk aan: ongelooflijke, onvoorstelbare, geweldig, fantastische = > ongelooflijke, onvoorstelbare, geweldig, fantastische<br /><br />-Als onwaar, de volgende lijst: fantastische ongelooflijke, onvoorstelbare, geweldig, gelijk zijn aan: ongelooflijke, onvoorstelbare, geweldig, fantastische = > ongelooflijke.|  
|[trim](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/TrimFilter.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Verwijdert voorloopspaties en volgspaties uit tokens. |  
|[truncate](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/TruncateTokenFilter.html)|TruncateTokenFilter|De voorwaarden worden afgekapt in een specifieke lengte.<br /><br /> **Opties**<br /><br /> de lengte (type: int)-standaard: 300, maximum: 300. Vereist.|  
|[unique](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/RemoveDuplicatesTokenFilter.html)|UniqueTokenFilter|Tokens met dezelfde tekst als het vorige token gefilterd.<br /><br /> **Opties**<br /><br /> onlyOnSamePosition (type: bool) - als instellen, verwijdert u dubbele alleen op dezelfde positie. De standaardwaarde is true.|  
|[hoofdletters](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/UpperCaseFilter.html)|(type geldt alleen wanneer opties zijn beschikbaar)  |Normaliseert token tekst naar hoofdletters. |  
|[word_delimiter](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/WordDelimiterFilter.html)|WordDelimiterTokenFilter|Splitst woorden in subwords en optionele transformaties uitvoert op subword groepen.<br /><br /> **Opties**<br /><br /> generateWordParts (type: bool)-zorgt ervoor dat de onderdelen van woorden moeten worden gegenereerd, bijvoorbeeld 'Azuresearch ' omdat' wordt "Azure'"zoeken". De standaardwaarde is true.<br /><br /> generateNumberParts (type: bool)-zorgt ervoor dat nummer subwords moet worden gegenereerd. De standaardwaarde is true.<br /><br /> catenateWords (type: bool)-maximum aantal uitvoeringen van word-onderdelen om te worden catenated, zorgt ervoor dat voor het voorbeeld 'Azure-Search' wordt 'Azuresearch ' omdat'. De standaardwaarde is false.<br /><br /> catenateNumbers (type: bool)-oorzaken maximale wordt uitgevoerd van het aantal onderdelen om te worden catenated, bijvoorbeeld "1-2" wordt "12". De standaardwaarde is false.<br /><br /> catenateAll (type: bool)-oorzaken van alle subword onderdelen om te worden catenated, bijvoorbeeld 'Azure-Search-1' wordt 'AzureSearch1'. De standaardwaarde is false.<br /><br /> splitOnCaseChange (type: bool) - als true, splitsingen woorden op caseChange, bijvoorbeeld 'azuresearch 'omdat "Azure'"zoeken"wordt. De standaardwaarde is true.<br /><br /> preserveOriginal - oorspronkelijke woorden worden bewaard en toegevoegd aan de lijst subword oorzaken. De standaardwaarde is false.<br /><br /> splitOnNumerics (type: bool) - als true, worden opgesplitst in getallen, bijvoorbeeld 'Azure1Search' 'Azure' "1" wordt "zoeken". De standaardwaarde is true.<br /><br /> stemEnglishPossessive (type: bool) - oorzaken die eindigen op een 'de' voor elke subword worden verwijderd. De standaardwaarde is true.<br /><br /> protectedWords (type: string-matrix)-Tokens te beschermen tegen worden gescheiden. De standaardwaarde is een lege lijst.|  

 <sup>1</sup> token filtertypen worden altijd voorafgegaan in code met '#Microsoft.Azure.Search' zodat "ArabicNormalizationTokenFilter" daadwerkelijk zou worden opgegeven als '#Microsoft.Azure.Search.ArabicNormalizationTokenFilter'.  We hebben verwijderd het voorvoegsel voor het verminderen van de breedte van de tabel, maar houd er rekening mee te nemen in uw code.  

> [!NOTE]
> Het is vereist dat u uw aangepaste analyzer niet produceren van tokens die langer is dan 300 tekens configureren. Mislukt voor documenten met behulp van deze tokens te indexeren. Als u wilt deze trim of negeren, gebruikt u de **TruncateTokenFilter** en de **LengthTokenFilter** respectievelijk.


## <a name="see-also"></a>Zie ook  
 [Azure Search Service REST](https://docs.microsoft.com/rest/api/searchservice/)   
 [Analyse in Azure Search > Voorbeelden](https://docs.microsoft.com/azure/search/search-analyzers#examples)    
 [Index maken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)  
