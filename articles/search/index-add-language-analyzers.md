---
title: Taalanalyse - Azure Search toevoegen
description: Van meerdere talen lexicale tekstanalyse voor niet-Engelse query's en indexen in Azure Search.
ms.date: 02/14/2019
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
ms.openlocfilehash: bb7fbdeea9c19b8a6fabe06687261296110b4064
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301799"
---
# <a name="add-language-analyzers-to-an-azure-search-index"></a>Taalanalyse toevoegen aan een Azure Search-index

Een *taalanalyse* is van een specifiek type [tekst analyzer](search-analyzers.md) die voert lexicale analyse uit met behulp van de linguïstische regels van de doel-taal. Elke doorzoekbaar veld heeft een **analyzer** eigenschap. Als uw index vertaalde tekenreeksen, zoals afzonderlijke velden voor Engels en Chinese bevat, kunt u taalanalyse opgeven voor elk veld voor toegang tot de uitgebreide linguïstische mogelijkheden van deze analyse.  

Azure Search biedt ondersteuning voor 35 analyzers die worden ondersteund door Lucene en 50 analyzers die worden ondersteund door een eigen Microsoft verwerking van natuurlijke taal technologie die wordt gebruikt in Office en Bing.

## <a name="comparing-analyzers"></a>Vergelijking van de analyzers

Sommige ontwikkelaars liever met de oplossing meer vertrouwde, eenvoudige, open-source van Lucene. Lucene-taalanalyse sneller zijn, maar de analyzers Microsoft zijn geavanceerde mogelijkheden, zoals lemmatisering, word decompounding (in talen zoals Duits, Deens, Nederlands, Zweeds, Noors, Ests, voltooien, Hongaars, Slowakije) en de entiteit herkenning (URL's, e-mailberichten, datums, getallen). Indien mogelijk moet u vergelijkingen van de Microsoft- en Lucene analyzers om te bepalen welke is beter geschikt uitvoeren. 

Indexeren met analysefuncties van Microsoft is op gemiddelde twee tot drie keer langzamer dan hun equivalenten Lucene, afhankelijk van de taal. Prestaties van de zoekopdracht moet niet aanzienlijk worden beïnvloed voor gemiddelde grootte van query's. 

### <a name="english-analyzers"></a>Engelse analyzers

De standaard-analyzer is standaard Lucene, dat goed voor Engels, maar misschien niet en de Engelse analyzer van Lucene of Engelse analyzer van Microsoft werkt. 
 
+ De Engelse analyzer van Lucene breidt de standaard analyzer. Het verwijdert bezittelijke voornaamwoorden (afsluitende van) van woorden, is van toepassing als gevolg aan de hand van Porter voortvloeit algorithm, en Engelse stopwoorden.  

+ Engelse analyzer van Microsoft voert lemmatisering in plaats van die voortvloeien uit. Dit betekent dat veel beter wat in meer relevante zoekresultaten resulteert verbogen en onregelmatige word-formulieren kunnen worden verwerkt 

 > [!Tip]
 > De [Search Analyzer-Demo](https://alice.unearth.ai/) side-by-side-vergelijking van de resultaten van de standaard Lucene analyzer, de Engelse taal-analysefunctie van Lucene en Microsofts Engelse natuurlijke taal processor biedt. Voor elke invoer zoeken opgeeft u, resultaten van elke analyzer worden weergegeven in de aangrenzende deelvensters.

## <a name="configuring-analyzers"></a>Analyzers configureren

Taalanalysefuncties worden gebruikt als-is. Voor elk veld in het definitie van de index, kunt u instellen de **analyzer** eigenschap in op de naam van een analyzer die welke taal en de leverancier aangeeft. De dezelfde analyzer worden toegepast wanneer het indexeren en zoeken naar dat veld. U kunt bijvoorbeeld afzonderlijke velden voor Engels, Frans en Spaans hotel beschrijvingen die naast elkaar bestaan in dezelfde index hebben.  

Gebruik de **searchFields** queryparameter om op te geven welk veld taalspecifieke om te zoeken op basis van uw query's. Voorbeelden van met de eigenschap analyzer in documenten zoeken, kunt u bekijken. 

Zie voor meer informatie over de Indexeigenschappen van de [Create Index &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index). Zie voor meer informatie over analyse in Azure Search [analyse in Azure Search](https://docs.microsoft.com/azure/search/search-analyzers).

<a name="language-analyzer-list"></a>

## <a name="language-analyzer-list"></a>Lijst van de analyzer taal 
 Hieronder vindt u de lijst met ondersteunde talen, samen met de namen van Lucene en Microsoft analyzer.  

|Taal|De naam van de Microsoft Analyzer|Lucene Analyzer Name|  
|--------------|-----------------------------|--------------------------|  
|Arabisch|ar.microsoft|ar.lucene|  
|Armeens||hy.lucene|  
|Bengalees|bn.microsoft||  
|Baskisch||eu.lucene|  
|Bulgaars|bg.microsoft|bg.lucene|  
|Catalaans|ca.microsoft|ca.lucene|  
|Vereenvoudigd Chinees|zh-Hans.microsoft|zh-Hans.lucene|  
|Traditioneel Chinees|zh-Hant.microsoft|zh-Hant.lucene|  
|Kroatisch|hr.microsoft||  
|Tsjechisch|cs.microsoft|cs.lucene|  
|Deens|da.microsoft|da.lucene|  
|Nederlands|nl.microsoft|nl.lucene|  
|Engels|en.microsoft|en.lucene|  
|Estisch|et.microsoft||  
|Fins|fi.microsoft|fi.lucene|  
|Frans|fr.microsoft|fr.lucene|  
|Galicisch||gl.lucene|  
|Duits|de.microsoft|de.lucene|  
|Grieks|el.microsoft|el.lucene|  
|Gujarati|gu.microsoft||  
|Hebreeuws|he.microsoft||  
|Hindi|hi.microsoft|hi.lucene|  
|Hongaars|hu.microsoft|hu.lucene|  
|IJslands|is.microsoft||  
|Indonesian (Bahasa)|id.microsoft|id.lucene|  
|Iers||ga.lucene|  
|Italiaans|it.microsoft|it.lucene|  
|Japans|ja.microsoft|ja.lucene|  
|Kanarees|ka.microsoft||  
|Koreaans|ko.microsoft|ko.lucene|  
|Lets|lv.microsoft|lv.lucene|  
|Litouws|lt.microsoft||  
|Malayalam|ml.microsoft||  
|Maleis (Latijns)|ms.microsoft||  
|Mahratti|mr.microsoft||  
|Noors|nb.microsoft|no.lucene|  
|Perzisch||fa.lucene|  
|Pools|pl.microsoft|pl.lucene|  
|Portugees (Brazilië)|pt-Br.microsoft|pt-Br.lucene|  
|Portugees (Portugal)|pt-Pt.microsoft|pt-Pt.lucene|  
|Punjabi|pa.microsoft||  
|Roemeens|ro.microsoft|ro.lucene|  
|Russisch|ru.microsoft|ru.lucene|  
|Servisch (Cyrillisch)|sr-cyrillic.microsoft||  
|Servisch (Latijns)|sr-latin.microsoft||  
|Slowaaks|sk.microsoft||  
|Sloveens|sl.microsoft||  
|Spaans|es.microsoft|es.lucene|  
|Zweeds|sv.microsoft|sv.lucene|  
|Tamil|ta.microsoft||  
|Telugu|te.microsoft||  
|Thais|th.microsoft|th.lucene|  
|Turks|tr.microsoft|tr.lucene|  
|Oekraïens|uk.microsoft||  
|Urdu|ur.microsoft||  
|Vietnamees|vi.microsoft||  

 Alle analyzers met namen van aantekeningen voorzien met **Lucene** worden aangestuurd door [van Apache Lucene taalanalyse](https://lucene.apache.org/core/4_9_0/core/overview-summary.html ).

## <a name="see-also"></a>Zie ook  
 [Index maken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)  
 [AnalyzerName klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername)  
 [Video: module 7 van Azure Search MVA-presentatie](https://channel9.msdn.com/Series/Adding-Microsoft-Azure-Search-to-Your-Websites-and-Apps/07).  

