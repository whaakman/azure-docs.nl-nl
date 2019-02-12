---
title: Scoreprofielen toevoegen aan een search-index - Azure Search
description: Boost zoeken positie scores voor Azure Search zoekresultaten door scoreprofielen toe te voegen.
ms.date: 01/31/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
ms.author: brjohnst
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
ms.openlocfilehash: eae7de00294a6a09cb7f942d11ee2391710fc55f
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56008487"
---
# <a name="add-scoring-profiles-to-an-azure-search-index"></a>Scoreprofielen toevoegen aan een Azure Search-index

  Scoring verwijst naar de berekening van een *zoeken score* voor elk item in de zoekresultaten worden geretourneerd. De score is een indicator van de relevantie van een item in de context van de huidige zoekopdracht. Hoe hoger de score, nog belangrijker het item. Items zijn in de zoekresultaten, positie gerangschikt van hoog naar laag, op basis van de search-scores berekend voor elk item.  

 Azure Search gebruikt standaard score berekenen van een eerste score, maar u kunt de berekening via een *scoringprofiel*. Scoreprofielen geven u meer controle over de volgorde van items in lijst met zoekresultaten. U wilt bijvoorbeeld items op basis van hun omzetpotentieel te vergroten, nieuwere objecten promoten of misschien verhogen items die in de inventaris die te lang zijn.  

 Een scoring-profiel maakt deel uit van de indexdefinitie bestaat uit gewogen velden, functies en parameters.  

 Het volgende voorbeeld ziet als u wilt een beeld van hoe een scoringprofiel uitziet, een eenvoudige profiel met de naam 'geo's '. Dit verhoogt de items die de zoekterm die in de **hotelName** veld. Gebruikt ook de `distance` functie voorkeur voor items die binnen tien kilometers van de huidige locatie. Als iemand de term 'inn zoekt' en 'inn' moet deel uitmaken van de naam van het hotel gebeurt, wordt deze documenten met hotels met 'inn' binnen een 10 KM radius van de huidige locatie die hoger in de lijst met zoekresultaten weergegeven.  


```  
"scoringProfiles": [
  {  
    "name":"geo",
    "text": {  
      "weights": {  
        "hotelName": 5
      }                              
    },
    "functions": [
      {  
        "type": "distance",
        "boost": 5,
        "fieldName": "location",
        "interpolation": "logarithmic",
        "distance": {
          "referencePointParameter": "currentLocation",
          "boostingDistance": 10
        }                        
      }                                      
    ]                     
  }            
]
```  


 De query is voor het gebruik van deze scoringprofiel geformuleerd om op te geven van het profiel op de query-tekenreeks. In de onderstaande query ziet u de queryparameter `scoringProfile=geo` in de aanvraag.  

```  
GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2017-11-11  
```  

 Deze query zoekt de term 'inn' en wordt doorgegeven in de huidige locatie. Houd er rekening mee dat deze query andere parameters, zoals bevat `scoringParameter`. Queryparameters worden beschreven in [documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).  

 Klik op [voorbeeld](#bkmk_ex) om te controleren van een uitgebreider voorbeeld van een scoring-profiel.  

## <a name="what-is-default-scoring"></a>Wat is het scoren van standaard?  
 Score berekent een zoekscore voor elk item in een positie geordende resultatenset. Elk item in de resultatenset van een zoekopdracht een zoekscore toegewezen, wordt gerangschikt hoogste naar laagste. Items met de hogere scores keert terug naar de toepassing. Standaard de top 50 worden geretourneerd, maar u kunt de `$top` parameter om terug te keren een kleiner of groter aantal items (maximaal 1000 in één antwoord).  

De search-score wordt berekend op basis van statistische eigenschappen van de gegevens en de query. Azure Search worden documenten gevonden die de zoektermen in de query-tekenreeks opnemen (enkele of alle, afhankelijk van de `searchMode`), voorkeur documenten met veel exemplaren van de zoekterm. De zoekscore toeneemt nog hogere als de term zeldzaam tussen het corpus gegevens, maar algemene in het document. De basis voor deze aanpak voor het computergebruik relevantie wordt ook wel [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) of term frequentie inverse document frequentie.  

 Ervan uitgaande dat er geen aangepaste sorteren, resultaten worden vervolgens informatie gerangschikt volgens het zoekscore voordat ze worden teruggestuurd naar de aanroepende toepassing. 50 items met de hoogste zoekscore, als $top niet opgegeven is, worden geretourneerd.  

 Search score waarden kunnen worden herhaald in een resultatenset. Bijvoorbeeld, mogelijk hebt u 10 items met een score van 1.2, 20 items met een score van 1.0 en 20 items met een score van 0,5. Wanneer meerdere treffers de dezelfde zoekscore hebben, de volgorde van items met dezelfde beoordeelde is niet gedefinieerd en is niet stabiel. Voer de query opnieuw, en u ziet mogelijk items shift positie. Twee items met een identieke score gegeven, is er geen garantie dat als eerste waarvoor een weergegeven.  

## <a name="when-to-use-custom-scoring"></a>Wanneer u aangepast scoren  
 Als de standaard trefwoordenrangschikking gedrag niet gaat ver in aan de wensen van uw zakelijke doelstellingen, moet u een of meer scoreprofielen maken. U kunt bijvoorbeeld besluiten dat toegevoegde items moet voorkeur voor op zoekrelevantie. Evenzo, mogelijk hebt u een veld met winstmarge, of een ander veld die wijzen op mogelijke omzet. Versterking van treffers die voordelen voor uw bedrijf bieden is een belangrijke factor bij het besluit om scoreprofielen gebruiken.  

 Ordenen op basis van relevantie wordt ook geïmplementeerd via scoreprofielen. Houd rekening met pagina's met zoekresultaten u in het verleden waarmee u sorteren op prijs, datum, classificatie of relevantie hebt gebruikt. Scoreprofielen station in Azure Search, de optie 'relevantie'. De definitie van relevantie wordt bepaald door u echter afhankelijk van de zakelijke doelstellingen en het type zoekervaring die u wilt leveren.  

##  <a name="bkmk_ex"></a> Voorbeeld  
 Zoals eerder opgemerkt, wordt aangepast scoren geïmplementeerd via een of meer scoreprofielen die in een indexschema zijn gedefinieerd.  

 In dit voorbeeld ziet u het schema van een index met twee scoreprofielen (`boostGenre`, `newAndHighlyRated`). Een query op deze index met een profiel, zoals een queryparameter het profiel wordt gebruikt om de resultatenset te beoordelen.  

```  
{  
  "name": "musicstoreindex",  
  "fields": [  
    { "name": "key", "type": "Edm.String", "key": true },  
    { "name": "albumTitle", "type": "Edm.String" },  
    { "name": "albumUrl", "type": "Edm.String", "filterable": false },  
    { "name": "genre", "type": "Edm.String" },  
    { "name": "genreDescription", "type": "Edm.String", "filterable": false },  
    { "name": "artistName", "type": "Edm.String" },  
    { "name": "orderableOnline", "type": "Edm.Boolean" },  
    { "name": "rating", "type": "Edm.Int32" },  
    { "name": "tags", "type": "Collection(Edm.String)" },  
    { "name": "price", "type": "Edm.Double", "filterable": false },  
    { "name": "margin", "type": "Edm.Int32", "retrievable": false },  
    { "name": "inventory", "type": "Edm.Int32" },  
    { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }  
  ],  
  "scoringProfiles": [  
    {  
      "name": "boostGenre",  
      "text": {  
        "weights": {  
          "albumTitle": 1.5,  
          "genre": 5,  
          "artistName": 2  
        }  
      }  
    },  
    {  
      "name": "newAndHighlyRated",  
      "functions": [  
        {  
          "type": "freshness",  
          "fieldName": "lastUpdated",  
          "boost": 10,  
          "interpolation": "quadratic",  
          "freshness": {  
            "boostingDuration": "P365D"  
          }  
        },  
        {
          "type": "magnitude",  
          "fieldName": "rating",  
          "boost": 10,  
          "interpolation": "linear",  
          "magnitude": {  
            "boostingRangeStart": 1,  
            "boostingRangeEnd": 5,  
            "constantBoostBeyondRange": false  
          }  
        }  
      ]  
    }  
  ],  
  "suggesters": [  
    {  
      "name": "sg",  
      "searchMode": "analyzingInfixMatching",  
      "sourceFields": [ "albumTitle", "artistName" ]  
    }  
  ]   
}  
```  

## <a name="workflow"></a>Werkstroom  
 Voor het implementeren van aangepaste scoring gedrag, moet u een scoringprofiel toevoegen aan het schema dat de index definieert. U kunt maximaal 100 scoreprofielen in een index hebt (Zie [Servicelimieten](search-limits-quotas-capacity.md)), maar u kunt slechts één profiel tegelijk in een bepaalde query opgeven.  

 Beginnen met de [sjabloon](#bkmk_template) vindt u in dit onderwerp.  

 Geef een naam op. Scoreprofielen zijn optioneel, maar als u een toevoegen, de naam is vereist. Zorg ervoor dat u de naamconventies voor velden (begint met een letter, voorkomt u speciale tekens en gereserveerde woorden). Zie [naamgevingsregels &#40;Azure Search&#41; ](https://docs.microsoft.com/rest/api/searchservice/naming-rules) voor een volledige lijst.  

 De hoofdtekst van het scoringprofiel is samengesteld uit gewogen velden en functies.  

|||  
|-|-|  
|**Gewicht**|Geef de naam / waarde-paren die een relatieve gewicht aan een veld toewijzen. In de [voorbeeld](#bkmk_ex), de velden albumTitle, genre en artistName zijn respectievelijk boosted 1.5, 5 en 2. Waarom wordt genre versterkt veel hoger dan de andere? Als de zoekopdracht wordt uitgevoerd op gegevens die zijn enigszins homogene (zoals het geval is met 'genre' in de `musicstoreindex`), moet u mogelijk een groter verschil in het relatieve gewicht. Bijvoorbeeld, in de `musicstoreindex`, 'rock' wordt weergegeven als beide een genre en in dezelfde geformuleerd genre beschrijvingen. Als u wilt dat genre te bieden, opwegen tegen genre beschrijving, moet het veld genre een veel hoger relatieve gewicht.|  
|**Functies**|Wanneer extra berekeningen vereist voor de specifieke context zijn gebruikt. Geldige waarden zijn `freshness`, `magnitude`, `distance`, en `tag`. Elke functie heeft parameters die uniek voor deze zijn.<br /><br /> -   `freshness` moet worden gebruikt wanneer u wilt verhogen door het nieuwe of oude een item is. Deze functie kan alleen worden gebruikt met `datetime` velden (edm. DataTimeOffset). Houd er rekening mee de `boostingDuration` kenmerk wordt alleen gebruikt met de `freshness` functie.<br />-   `magnitude` moet worden gebruikt wanneer u verhogen op basis wilt van hoe hoog of laag een numerieke waarde wordt. Scenario's die voor deze functie aanroepen zijn versterking van winstmarge, prijs hoogste, laagste koers of een aantal downloads. Deze functie kan alleen worden gebruikt met velden dubbele en geheel getal zijn.<br />     Voor de `magnitude` functie, kunt u het bereik, hoog naar laag, omgekeerde als u wilt dat de inverse patroon (bijvoorbeeld voor de items van de boost goedkopere is meer dan hogere geprijsde items). Een bereik van de prijzen van $100 krijgen tot $1, stelt u `boostingRangeStart` op 100 en `boostingRangeEnd` op 1 om de items goedkopere te verbeteren.<br />-   `distance` moet worden gebruikt wanneer u wilt vergroten door nabijheid of geografische locatie. Deze functie kan alleen worden gebruikt met `Edm.GeographyPoint` velden.<br />-   `tag` moet worden gebruikt wanneer u wilt vergroten door labels in algemene tussen documenten en zoekquery's. Deze functie kan alleen worden gebruikt met `Edm.String` en `Collection(Edm.String)` velden.<br /><br /> **Regels voor het gebruik van functies**<br /><br /> Functietype (`freshness`, `magnitude`, `distance`), `tag` moet kleine letters.<br /><br /> Functions kunnen niet null of lege waarden bevatten. Als u fieldname opnemen, hebt u met name op iets ingesteld.<br /><br /> Functions kunnen alleen worden toegepast op filterbare velden. Zie [Create Index &#40;Azure Search Service REST API&#41; ](https://docs.microsoft.com/rest/api/searchservice/create-index) voor meer informatie over filterbare velden.<br /><br /> Functions kunnen alleen worden toegepast op de velden die zijn gedefinieerd in de verzameling velden van een index.|  

 Nadat de index is gedefinieerd, moet u de index maken door het indexschema, gevolgd door documenten te uploaden. Zie [Create Index &#40;Azure Search Service REST API&#41; ](https://docs.microsoft.com/rest/api/searchservice/create-index) en [documenten toevoegen, bijwerken of verwijderen &#40;Azure Search Service REST API&#41; ](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) voor instructies voor deze bewerkingen. Als de index is gemaakt, hebt u een functionele scoringprofiel die geschikt is voor uw zoekgegevens.  

##  <a name="bkmk_template"></a> sjabloon  
 In deze sectie bevat de syntaxis en de sjabloon voor het scoren van profielen. Raadpleeg [Index kenmerken verwijzing](#bkmk_indexref) in de volgende sectie voor beschrijvingen van de kenmerken.  

```  
. . .   
"scoringProfiles": [  
  {   
    "name": "name of scoring profile",   
    "text": (optional, only applies to searchable fields) {   
      "weights": {   
        "searchable_field_name": relative_weight_value (positive #'s),   
        ...   
      }   
    },   
    "functions": (optional) [  
      {   
        "type": "magnitude | freshness | distance | tag",   
        "boost": # (positive number used as multiplier for raw score != 1),   
        "fieldName": "...",   
        "interpolation": "constant | linear (default) | quadratic | logarithmic",   

        "magnitude": {
          "boostingRangeStart": #,   
          "boostingRangeEnd": #,   
          "constantBoostBeyondRange": true | false (default)
        }  

        // ( - or -)  

        "freshness": {
          "boostingDuration": "..." (value representing timespan over which boosting occurs)   
        }  

        // ( - or -)  

        "distance": {
          "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)   
          "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)   
        }   

        // ( - or -)  

        "tag": {
          "tagsParameter":  "..."(parameter to be passed in queries to specify a list of tags to compare against target field)   
        }
      }
    ],   
    "functionAggregation": (optional, applies only when functions are specified) "sum (default) | average | minimum | maximum | firstMatching"   
  }   
],   
"defaultScoringProfile": (optional) "...",   
. . .  
```  

##  <a name="bkmk_indexref"></a> Index kenmerken verwijzing  

> [!NOTE]  
>  Een scoringfunctie kan alleen worden toegepast op de velden die gefilterd zijn.  

|Kenmerk|Description|  
|---------------|-----------------|  
|`Name`|Vereist. Dit is de naam van het scoring-profiel. De aliasnaam gelden dezelfde van een veld volgt. Deze moet beginnen met een letter, mogen geen punten, dubbele punten bevatten of @-tekens, mag niet beginnen met de zin 'azureSearch' (hoofdlettergevoelig).|  
|`Text`|Bevat de eigenschap gewicht.|  
|`Weights`|Optioneel. Een naam / waarde-paar dat Hiermee geeft u een veldnaam en het relatieve gewicht. Relatieve gewicht moet een positief geheel getal of getal met drijvende komma. De maximumwaarde is int32. MaxValue.<br /><br /> U kunt de naam van het veld zonder een bijbehorende gewicht opgeven. Het gewicht worden gebruikt om aan te geven van het belang van één veld ten opzichte van een andere.|  
|`Functions`|Optioneel. Houd er rekening mee dat een scoringfunctie alleen kan worden toegepast op de velden die gefilterd zijn.|  
|`Type`|Vereist voor scoringfuncties functies. Geeft het type van de functie te gebruiken. Geldige waarden zijn omvang, nieuwheid, afstand en label. U kunt meer dan één functie opnemen in elke scoringprofiel. Naam van de functie moet kleine letters.|  
|`Boost`|Vereist voor scoringfuncties functies. Een positief getal dat wordt gebruikt als vermenigvuldiger voor onbewerkte scores. Deze mag niet gelijk aan 1.|  
|`Fieldname`|Vereist voor scoringfuncties functies. Een scoringfunctie kan alleen worden toegepast op de velden die deel uitmaken van de veldverzameling van de index en die zijn gefilterd. Elk functietype introduceert bovendien extra beperkingen (webdocumenten wordt gebruikt met datetime-velden, magnitude met geheel getal of dubbele velden en op afstand met locatievelden). U kunt slechts één veld per functiedefinitie opgeven. Als u wilt magnitude tweemaal in hetzelfde profiel gebruiken, moet u zou bijvoorbeeld twee definities magnitude, één voor elk veld bevatten.|  
|`Interpolation`|Vereist voor scoringfuncties functies. Definieert de helling waarvoor de scoringversterking toeneemt vanaf het begin van het bereik aan het einde van het bereik. Geldige waarden zijn lineair (standaard), constante ABC en logaritmisch. Zie [interpolations ingesteld](#bkmk_interpolation) voor meer informatie.|  
|`magnitude`|De omvang scoring-functie wordt gebruikt om te wijzigen van classificaties op basis van het bereik van waarden voor een numeriek veld. Enkele van de meest voorkomende gebruiksvoorbeelden hiervan zijn:<br /><br /> -   **Sterwaarderingen:** Wijzig de scores op basis van de waarde in het veld 'Ster score'. Wanneer twee items relevant zijn, wordt het item met de hogere classificatie eerst weergegeven.<br />-   **Zijmarge:** Wanneer twee documenten relevant zijn, kunt detailhandel desgewenst om documenten met hogere marges eerst te verbeteren.<br />-   **Klik op telt:** Voor toepassingen die volgen, klikt u op via de acties op producten of pagina's, kunt u de omvang aan boost artikelen die doorgaans om op te halen van het meeste verkeer kunt gebruiken.<br />-   **Telt het aantal downloaden:** Voor toepassingen bijhouden downloads, de grootte functie kunt u verhogen items die zijn de meeste downloads.|  
|`magnitude` &#124; `boostingRangeStart`|Hiermee stelt u de beginwaarde van het bereik waarvoor de magnitude wordt berekend. De waarde moet een geheel getal of getal met drijvende komma. Voor sterwaarderingen van 1 tot en met 4, zou dit 1 zijn. Voor marges van meer dan 50% zou dit 50 zijn.|  
|`magnitude` &#124; `boostingRangeEnd`|Hiermee stelt u de eindwaarde van het bereik waarvoor de magnitude wordt berekend. De waarde moet een geheel getal of getal met drijvende komma. Voor sterwaarderingen van 1 tot en met 4, zou dit 4 zijn.|  
|`magnitude` &#124; `constantBoostBeyondRange`|Geldige waarden zijn true of false (standaard). Wanneer is ingesteld op true, de volledige versterking om toe te passen op documenten waarvoor een waarde op voor het doelveld hoger is dan de bovengrens van het bereik. Indien onwaar, wordt de versterking van deze functie niet toegepast op documenten met een waarde op voor het doelveld die buiten het bereik valt.|  
|`freshness`|De webdocumenten scoring-functie wordt gebruikt voor het wijzigen van de classificatiescores voor items op basis van waarden in `DateTimeOffset` velden. Een item met een recentere datum kan bijvoorbeeld hoger dan de oudere items worden gerangschikt.<br /><br /> Houd er rekening mee dat het is ook mogelijk op positie items zoals agenda-gebeurtenissen met datums in de toekomst zodat items dichter bij de huidige kunnen worden gerangschikt hoger dan items verder in de toekomst.<br /><br /> In de huidige versie van de service wordt op de huidige tijd ene uiteinde van het bereik opgelost. Het andere uiteinde is een tijd in het verleden op basis van de `boostingDuration`. Als u wilt een aantal keren dat in de toekomst vergroten, gebruikt u een negatieve `boostingDuration`.<br /><br /> De snelheid waarmee de versterking verandert van maximaal en minimaal bereik wordt bepaald door de interpolatie toegepast op de scoringprofiel (Zie de onderstaande afbeelding). Als u wilt omkeren de Gradient boosting factor die wordt toegepast, kiest u een boost factor van minder dan 1.|  
|`freshness` &#124; `boostingDuration`|Hiermee stelt u een vervalperiode in waarna versterking van een bepaald document wordt stopgezet. Zie [boostingDuration ingesteld](#bkmk_boostdur) in de volgende sectie voor de syntaxis en voorbeelden.|  
|`distance`|Sluit u de score van documenten op basis van hoe u de afstand scoringfunctie wordt gebruikt om te bepalen of ver ze zich ten opzichte van een opgegeven geografische locatie. De referentielocatie is opgegeven als onderdeel van de query in een parameter (met behulp van de `scoringParameterquery` optie tekenreeks) als een ion, lat argument.|  
|`distance` &#124; `referencePointParameter`|Een parameter in query's om te gebruiken als referentielocatie worden doorgegeven. `scoringParameter` is een queryparameter. Zie [documenten zoeken &#40;Azure Search Service REST API&#41; ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) voor beschrijvingen van queryparameters.|  
|`distance` &#124; `boostingDistance`|Een getal geeft de afstand in kilometer tot de referentielocatie waar het versterkingsbereik wordt beëindigd.|  
|`tag`|De tag scoring-functie wordt gebruikt om invloed hebben op de score van documenten op basis van tags in documenten en zoekquery's. Documenten met tags die overeenkomen met de zoekopdracht wordt worden verhoogd. De tags voor de zoekopdracht wordt geleverd als een scoring-parameter in elke zoekaanvraag (met behulp van de `scoringParameterquery` optie tekenreeks).|  
|`tag` &#124; `tagsParameter`|Een parameter in query's om op te geven, tags voor een bepaalde aanvraag worden doorgegeven. `scoringParameter` is een queryparameter. Zie [documenten zoeken &#40;Azure Search Service REST API&#41; ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) voor beschrijvingen van queryparameters.|  
|`functionAggregation`|Optioneel. Geldt alleen wanneer functies worden opgegeven. Geldige waarden zijn: sum (standaard), gemiddelde, minimum, maximum- en firstMatching. Een zoekscore is één waarde die wordt berekend uit meerdere variabelen, met inbegrip van meerdere functies. Dit kenmerk geeft aan hoe de profiteert van alle functies zijn gecombineerd tot een stimuleren die vervolgens wordt toegepast op de score base document. De base score is gebaseerd op de [tf-idf](http://www.tfidf.com/) waarde berekend vanaf het document en de zoekquery.|  
|`defaultScoringProfile`|Bij het uitvoeren van een zoekaanvraag als er geen scoringprofiel is opgegeven, wordt standaard scoren wordt gebruikt ([tf-idf](http://www.tfidf.com/) alleen).<br /><br /> Een scoring-profielnaam standaard kan hier worden ingesteld waardoor Azure Search te gebruiken dat profiel als er geen specifiek profiel is opgegeven in de zoekaanvraag.|  

##  <a name="bkmk_interpolation"></a> Set interpolations  
 Interpolations kunt u de vorm van de helling gebruikt voor het scoren van instellen. Omdat scoren van hoog naar laag is, wordt de helling altijd afneemt, maar de interpolatie bepaalt de curve van de richting omlaag. De volgende interpolations kunnen worden gebruikt:  

|||  
|-|-|  
|`Linear`|Voor artikelen die zich binnen het bereik maximale en minimale, wordt de versterking toegepast op het item wordt uitgevoerd in een voortdurend afneemt. Lineaire is de standaard-interpolatie voor een scoringprofiel.|  
|`Constant`|Voor artikelen die zich binnen het begin en eind-bereik, wordt een versterking van constante worden toegepast op de positie resultaten.|  
|`Quadratic`|In vergelijking met een lineaire interpolatie waarvoor een boost voortdurend afneemt, ABC in eerste instantie in kleinere tempo verlaagd en als dit het einde van het bereik nadert, het neemt af op basis van een veel hogere interval. Deze optie interpolatie is niet toegestaan in de tag scoring-functies.|  
|`Logarithmic`|In vergelijking met een lineaire interpolatie waarvoor een boost voortdurend afneemt, logaritmische in eerste instantie in hogere tempo verlaagd en als dit het einde van het bereik nadert, het neemt af op basis van een veel kleinere interval. Deze optie interpolatie is niet toegestaan in de tag scoring-functies.|  

 ![Constante, lineaire, kwadratische, log10 regels op de grafiek](media/scoring-profiles/azuresearch_scorefunctioninterpolationgrapht.png "AzureSearch_ScoreFunctionInterpolationGrapht")  

##  <a name="bkmk_boostdur"></a> Set boostingDuration  
 `boostingDuration` is een kenmerk van de `freshness` functie. Hierin kunt u instellen dat een verlopen periode na een bepaald document waarna versterking wordt stopgezet. Bijvoorbeeld, om te verbeteren en een productlijnen voor een promotieperiode van 10 dagen, moet u de periode van 10 dagen als 'P10D' voor deze documenten opgeven.  

 `boostingDuration` moeten zijn opgemaakt als een XSD-waarde van het 'dayTimeDuration' (een beperkte subset van een ISO 8601-duurwaarde). Het patroon hiervoor is: "P[nD][T[nH][nM][nS]]".  

 De volgende tabel bevat enkele voorbeelden.  

|Duur|boostingDuration|  
|--------------|----------------------|  
|1 dag|"P1D"|  
|2 dagen en 12 uur|"P2DT12H"|  
|15 minuten|"PT15M"|  
|30 dagen, 5 uur, 10 minuten, en 6.334 seconden|"P30DT5H10M6.334S"|  

 Zie voor meer voorbeelden van [XML-Schema: Gegevenstypen (W3.org web site)](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration).  

## <a name="see-also"></a>Zie ook  
 [Azure Search Service REST](https://docs.microsoft.com/rest/api/searchservice/)   
 [Index maken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)   
 [Azure Search .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)  
