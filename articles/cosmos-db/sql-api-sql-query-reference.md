---
title: 'Azure Cosmos DB: Naslaginformatie SQL-syntaxis query | Microsoft Docs'
description: Documentatie voor de taal van Azure Cosmos DB SQL-query.
services: cosmos-db
author: LalithaMV
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 10/18/2017
ms.author: laviswa
ms.openlocfilehash: 968f9e4d643228e02a1d09aae1b6ce82d6775f25
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="azure-cosmos-db-sql-syntax-reference"></a>Naslaginformatie over Azure Cosmos DB SQL-syntaxis

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Azure DB Cosmos ondersteunt het uitvoeren van query's documenten met een bekende SQL (Structured Query Language) zoals grammatica via hiërarchische JSON-documenten zonder expliciet schema of het maken van secundaire indexen. Dit onderwerp vindt u documentatie voor de SQL-querytaal compatibel met SQL-API-accounts is.

Zie voor een overzicht van de SQL-querytaal [SQL-query's voor Azure Cosmos DB](sql-api-sql-query.md).  
  
We nodigen ook u gaat u naar de [Query Playground](http://www.documentdb.com/sql/demo) kunt u proberen Azure Cosmos DB en SQL-query's uitvoeren op onze gegevensset.  
  
## <a name="select-query"></a>SELECT-query  
JSON-documenten opgehaald uit de database. Ondersteunt de evaluatie van de expressie, projecties, filteren en lid wordt.  De conventies voor de beschrijving van de SELECT-instructies zijn in een tabel in de sectie syntaxis overeenkomsten weergegeven.  
  
**Syntaxis**  
  
```
<select_query> ::=  
SELECT <select_specification>   
    [ FROM <from_specification>]   
    [ WHERE <filter_condition> ]  
    [ ORDER BY <sort_specification> ]  
```  
  
 **Opmerkingen**  
  
 Zie de volgende secties voor meer informatie op elke component:  
  
-   [SELECT-component](#bk_select_query)  
  
-   [FROM-component](#bk_from_clause)  
  
-   [WHERE-component](#bk_where_clause)  
  
-   [ORDER BY-component](#bk_orderby_clause)  
  
De componenten in de SELECT-instructie moeten worden besteld, zoals hierboven. Een van de optionele componenten kan worden weggelaten. Maar als optionele componenten worden gebruikt, moeten ze worden weergegeven in de juiste volgorde.  
  
**Logische verwerkingsvolgorde van de SELECT-instructie**  
  
De volgorde waarin componenten worden verwerkt is:  

1.  [FROM-component](#bk_from_clause)  
2.  [WHERE-component](#bk_where_clause)  
3.  [ORDER BY-component](#bk_orderby_clause)  
4.  [SELECT-component](#bk_select_query)  

Houd er rekening mee dat dit verschilt van de volgorde waarin ze worden weergegeven in de syntaxis. De volgorde is dat alle nieuwe symbolen die zijn geïntroduceerd in een component verwerkte zichtbaar zijn en kunnen worden gebruikt in de componenten die later wordt verwerkt. Bijvoorbeeld, aliassen die zijn gedeclareerd in een FROM-component in, waarbij toegankelijk zijn en SELECT-component.  

**Spaties en -opmerkingen**  

Alle spatietekens die deel uitmaken van een tekenreeks tussen aanhalingstekens of aanhalingstekens id maken geen deel uit van de taal-grammatica en worden genegeerd tijdens het parseren.  

De query language ondersteunt T-SQL-stijl opmerkingen zoals  

-   SQL-instructie`-- comment text [newline]`  

Terwijl uit witruimte bestaat en -opmerkingen bevatten geen betekenis in de grammatica, moet het worden gebruikt voor het scheiden van tokens. Bijvoorbeeld: `-1e5` is één nummer token, even`: – 1 e5` wordt een min token gevolgd door nummer 1 en id e5.  

##  <a name="bk_select_query"></a>SELECT-component  
De componenten in de SELECT-instructie moeten worden besteld, zoals hierboven. Een van de optionele componenten kan worden weggelaten. Maar als optionele componenten worden gebruikt, moeten ze worden weergegeven in de juiste volgorde.  

**Syntaxis**  
```  
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | <object_property_list>   
      | VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
 **Argumenten**  
  
 `<select_specification>`  
  
 Eigenschappen of -waarde worden geselecteerd voor de resultatenset.  
  
 `'*'`  
  
Hiermee geeft u op dat de waarde zonder wijzigingen moet worden opgehaald. Specifiek als de verwerkte waarde een object is, worden alle eigenschappen worden opgehaald.  
  
 `<object_property_list>`  
  
Hiermee geeft u de lijst met eigenschappen worden opgehaald. Elke geretourneerde waarde is een object met de eigenschappen die zijn opgegeven.  
  
`VALUE`  
  
Geeft aan dat de JSON-waarde moet worden opgehaald in plaats van de volledige JSON-object. Dit, in tegenstelling tot `<property_list>` loopt niet de verwachte waarde in een object.  
  
`<scalar_expression>`  
  
De expressie voor de waarde moeten worden berekend. Zie [scalaire expressies](#bk_scalar_expressions) sectie voor meer informatie.  
  
**Opmerkingen**  
  
De `SELECT *` syntaxis is alleen geldig als FROM-component precies één alias is gedeclareerd. `SELECT *`biedt een identity-projectie handig is als er geen projectie is vereist. Selecteer * is alleen geldig als FROM-component is opgegeven en er slechts één invoer bron geïntroduceerd.  
  
Houd er rekening mee dat `SELECT <select_list>` en `SELECT *` 'syntactische suiker' zijn en u kunt ook kan worden uitgedrukt met behulp van eenvoudige SELECT-instructies zoals hieronder wordt weergegeven.  
  
1.  `SELECT * FROM ... AS from_alias ...`  
  
     is gelijk aan:  
  
     `SELECT from_alias FROM ... AS from_alias ...`  
  
2.  `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
     is gelijk aan:  
  
     `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
**Zie ook**  
  
[Scalaire expressies](#bk_scalar_expressions)  
[SELECT-component](#bk_select_query)  
  
##  <a name="bk_from_clause"></a>FROM-component  
Hiermee geeft u de bron- of gekoppelde bronnen. De component FROM is optioneel. Als dat niet wordt opgegeven, andere componenten nog steeds uitgevoerd alsof FROM-component opgegeven één document.  
  
**Syntaxis**  
  
```  
FROM <from_specification>  
  
<from_specification> ::=   
        <from_source> {[ JOIN <from_source>][,...n]}  
  
<from_source> ::=   
          <collection_expression> [[AS] input_alias]  
        | input_alias IN <collection_expression>  
  
<collection_expression> ::=   
        ROOT   
     | collection_name  
     | input_alias  
     | <collection_expression> '.' property_name  
     | <collection_expression> '[' "property_name" | array_index ']'  
```  
  
**Argumenten**  
  
`<from_source>`  
  
Hiermee geeft u een gegevensbron met of zonder een alias. Als alias niet opgegeven is, wordt deze afgeleid van de `<collection_expression>` met volgende regels:  
  
-   De expressie is een verzamelingnaam, wordt verzamelingnaam worden gebruikt als een alias.  
  
-   Als de expressie `<collection_expression>`, en vervolgens kubuskenmerkbinding en vervolgens kubuskenmerkbinding wordt gebruikt als alias. De expressie is een verzamelingnaam, wordt verzamelingnaam worden gebruikt als een alias.  
  
ALS IN DE`input_alias`  
  
Hiermee wordt aangegeven dat de `input_alias` is een set met waarden geretourneerd door de onderliggende verzamelingsexpressie.  
 
`input_alias`IN  
  
Hiermee wordt aangegeven dat de `input_alias` moet de set met waarden die zijn verkregen door iteratie van alle matrixelementen van elke matrix geretourneerd door de onderliggende verzamelingsexpressie vertegenwoordigen. Een waarde die is geretourneerd door de onderliggende verzamelingsexpressie is geen matrix wordt genegeerd.  
  
`<collection_expression>`  
  
Hiermee geeft u de verzamelingsexpressie voor het ophalen van de documenten worden gebruikt in de.  
  
`ROOT`  
  
Hiermee geeft u op dat document moet worden opgehaald van de standaardwaarde, momenteel verbonden verzameling.  
  
`collection_name`  
  
Hiermee geeft u op dat document moet worden opgehaald uit de opgegeven verzameling. De naam van de verzameling moet overeenkomen met de naam van de momenteel verbonden met verzameling.  
  
`input_alias`  
  
Hiermee geeft u op dat document moet worden opgehaald uit de andere bron gedefinieerd door de opgegeven alias.  
  
`<collection_expression> '.' property_`  
  
Geeft aan dat document moet worden opgehaald door het openen van de `property_name` eigenschap of matrixindex matrixelement voor alle documenten die worden opgehaald door opgegeven verzamelingsexpressie.  
  
`<collection_expression> '[' "property_name" | array_index ']'`  
  
Geeft aan dat document moet worden opgehaald door het openen van de `property_name` eigenschap of matrixindex matrixelement voor alle documenten die worden opgehaald door opgegeven verzamelingsexpressie.  
  
**Opmerkingen**  
  
Alle aliassen verstrekt of afgeleid in de `<from_source>(`s) moet uniek zijn. De syntaxis van de `<collection_expression>.`kubuskenmerkbinding is hetzelfde als `<collection_expression>' ['"property_name"']'`. De syntaxis van de laatste kan echter worden gebruikt als een eigenschapsnaam een niet-id-tekens bevat.  
  
**Ontbrekende eigenschappen, ontbrekende matrixelementen, niet-gedefinieerde waarden verwerken**  
  
Als een verzamelingsexpressie voor een naar eigenschappen of matrixelementen en waarde niet bestaat, wordt die waarde genegeerd en niet verder worden verwerkt.  
  
**Verzameling expressie context scoping**  
  
Een verzamelingsexpressie is mogelijk binnen het bereik van verzameling of binnen het bereik van document:  
  
-   Een expressie is verzameling-bereik, als de onderliggende gegevensbron van de verzamelingsexpressie ofwel ROOT is of `collection_name`. Een dergelijke expressie vertegenwoordigt een verzameling van documenten die zijn opgehaald uit de verzameling rechtstreeks en is niet afhankelijk van de verwerking van andere expressies verzameling.  
  
-   Een expressie is document-bereik, als de onderliggende gegevensbron van de verzamelingsexpressie `input_alias` geïntroduceerd eerder in de query. Een dergelijke expressie vertegenwoordigt een verzameling van documenten die zijn verkregen door het evalueren van de verzamelingsexpressie in het bereik van de van elk document die horen bij de set die zijn gekoppeld aan de verzameling alias hebben.  De resulterende set worden een samenvoeging van die worden verkregen door de verzamelingsexpressie voor elk van de documenten in de onderliggende set te evalueren.  
  
**Joins**  
  
In de huidige release ondersteunt Azure Cosmos DB inner joins. Er zijn extra join mechanismen toekomstige.

Inner join resulteert in een volledige vectorproduct van de sets die deel uitmaken van de join. Het resultaat van een join N manier is een set met tuples zijn N-element, waarbij elke waarde in de tuple is gekoppeld aan de alias instellen die deel uitmaken van de join en toegankelijk zijn voor het verwijzen naar deze alias in andere componenten.  
  
De evaluatie van de join, is afhankelijk van het bereik van de context van de deelnemende sets:  
  
-  Een join tussen verzamelingsset A en gericht op de verzameling ingesteld B, resulteert in een vectorproduct van alle elementen in sets A en B.
  
-   Een koppeling tussen het paar A en binnen het bereik van document set B, resulteert in een samenvoeging van alle sets die zijn verkregen door het evalueren van binnen het bereik van document set B voor elk document van A. instellen  
  
 In de huidige release wordt maximaal één expressie gericht op de verzameling ondersteund door de queryprocessor.  
  
**Voorbeelden van joins:**  
  
Bekijk de volgende FROM-component:`<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`  
  
 Elke bron definiëren, kunnen `input_alias1, input_alias2, …, input_aliasN`. Deze component FROM retourneert een set met N-tuples (tuple met N waarden). Elke tuple heeft geproduceerd door alle verzameling aliassen iteratie van hun respectieve sets waarden.  
  
*Voorbeeld 1, met 2 bronnen JOIN:*  
  
- Laat `<from_source1>` verzameling bereik en de set {A, B, C} vertegenwoordigen.  
  
- Laat `<from_source2>` worden document binnen het bereik van verwijzende input_alias1 en vertegenwoordigen sets:  
  
    {1, 2} voor`input_alias1 = A,`  
  
    {3} voor`input_alias1 = B,`  
  
    {4, 5} voor`input_alias1 = C,`  
  
- De component FROM `<from_source1> JOIN <from_source2>` leidt ertoe dat de volgende tuples:  
  
    (`input_alias1, input_alias2`):  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
*Voorbeeld 2, met 3 bronnen JOIN:*  
  
- Laat `<from_source1>` verzameling bereik en de set {A, B, C} vertegenwoordigen.  
  
- Laat `<from_source2>` worden binnen het bereik van document verwijzen naar `input_alias1` sets omvatten:  
  
    {1, 2} voor`input_alias1 = A,`  
  
    {3} voor`input_alias1 = B,`  
  
    {4, 5} voor`input_alias1 = C,`  
  
- Laat `<from_source3>` worden binnen het bereik van document verwijzen naar `input_alias2` sets omvatten:  
  
    {100, 200} voor`input_alias2 = 1,`  
  
    {300} voor`input_alias2 = 3,`  
  
- De component FROM `<from_source1> JOIN <from_source2> JOIN <from_source3>` leidt ertoe dat de volgende tuples:  
  
    (input_alias1, input_alias2, input_alias3):  
  
    (A, 1, 100), (A, 1, 200), (B, 3, 300)  
  
> [!NOTE]
> Gebrek aan tuples voor andere waarden van `input_alias1`, `input_alias2`, waarvoor de `<from_source3>` heeft geen waarden retourneren.  
  
*Voorbeeld 3, met 3 bronnen JOIN:*  
  
- Laat < from_source1 > worden gericht op de verzameling en vertegenwoordigen set {A, B, C}.  
  
- Laat `<from_source1>` verzameling bereik en de set {A, B, C} vertegenwoordigen.  
  
- < From_source2 > verwijzende input_alias1 binnen het bereik van document zijn en stelt vertegenwoordigen, kunnen:  
  
    {1, 2} voor`input_alias1 = A,`  
  
    {3} voor`input_alias1 = B,`  
  
    {4, 5} voor`input_alias1 = C,`  
  
- Laat `<from_source3>` moet binnen het bereik `input_alias1` sets omvatten:  
  
    {100, 200} voor`input_alias2 = A,`  
  
    {300} voor`input_alias2 = C,`  
  
- De component FROM `<from_source1> JOIN <from_source2> JOIN <from_source3>` leidt ertoe dat de volgende tuples:  
  
    (`input_alias1, input_alias2, input_alias3`):  
  
    (A, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200), C, 4, 300, (C, 5, 300)  
  
> [!NOTE]
> Dit resulteerde in vectorproduct tussen `<from_source2>` en `<from_source3>` omdat beide zijn gericht op hetzelfde `<from_source1>`.  Dit resulteerde in 4 (2 x 2) tuples met waarde A, 0 tuples met waarde B (1 x 0) en 2 (2 x 1) tuples met waarde C.  
  
**Zie ook**  
  
 [SELECT-component](#bk_select_query)  
  
##  <a name="bk_where_clause"></a>WHERE-component  
 Hiermee geeft u het zoekcriterium voor de documenten die worden geretourneerd door de query.  
  
 **Syntaxis**  
  
```  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
 **Argumenten**  
  
-   `<filter_condition>`  
  
     Geeft aan welke voorwaarde moet worden voldaan om de documenten die moeten worden geretourneerd.  
  
-   `<scalar_expression>`  
  
     De expressie voor de waarde moeten worden berekend. Zie de [scalaire expressies](#bk_scalar_expressions) sectie voor meer informatie.  
  
 **Opmerkingen**  
  
 Om het document moet worden geretourneerd van een expressie die is opgegeven als filter moet voorwaarde resulteren in waar. Alleen Booleaanse waarde true voldoen aan de voorwaarde, een andere waarde: niet-gedefinieerde, null, false, getal, matrix of Object niet voldoen aan de voorwaarde.  
  
##  <a name="bk_orderby_clause"></a>ORDER BY-component  
 Hiermee geeft u de sorteervolgorde voor de resultaten die door de query zijn geretourneerd.  
  
 **Syntaxis**  
  
```  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= <scalar_expression> [ASC | DESC]  
  
```  
  
 **Argumenten**  
  
-   `<sort_specification>`  
  
     Hiermee geeft u een eigenschap of een expressie waarmee moet worden gesorteerd van de queryresultatenset. Een kolom sorteren kan worden opgegeven als een alias voor een naam of kolom.  
  
     Meerdere sortering-kolommen kunnen worden opgegeven. Kolomnamen moet uniek zijn. De volgorde van de kolommen sorteren in de component ORDER BY definieert de organisatie van de gesorteerde resultatenset. Dat wil zeggen, de resultatenset is gesorteerd op de eerste eigenschap en vervolgens die geordende lijst is gesorteerd op de tweede eigenschap, enzovoort.  
  
     De kolomnamen van de waarnaar wordt verwezen in de component ORDER BY moeten naar een kolom in de selectielijst of naar een kolom die is gedefinieerd in een tabel die is opgegeven in de component FROM zonder eventuele dubbelzinnigheden overeenkomen.  
  
-   `<sort_expression>`  
  
     Hiermee geeft u een één eigenschap of een expressie waarmee moet worden gesorteerd van de queryresultatenset.  
  
-   `<scalar_expression>`  
  
     Zie de [scalaire expressies](#bk_scalar_expressions) sectie voor meer informatie.  
  
-   `ASC | DESC`  
  
     Hiermee geeft u op dat de waarden in de opgegeven kolom in oplopende of aflopende volgorde moeten worden gesorteerd. ASC sorteren van de laagste waarde naar de hoogste waarde. DESC sorteren van hoogste waarde naar de laagste waarde. ASC is de standaardsorteervolgorde. Null-waarden worden behandeld als de laagst mogelijke waarden.  
  
 **Opmerkingen**  
  
 Terwijl de querygrammatica meerdere volgorde door eigenschappen ondersteunt, wordt de runtime van Azure DB die Cosmos-query ondersteunt alleen tegen één eigenschap, en alleen eigenschapnamen, dat wil zeggen, niet op berekende eigenschappen sorteren. Sorteren is ook vereist dat het indexeringsbeleid een bereikindex voor de eigenschap en het opgegeven type met de maximale precisie bevat. Raadpleeg de indexering beleid-documentatie voor meer informatie.  
  
##  <a name="bk_scalar_expressions"></a>Scalaire expressies  
 Een scalaire expressie die is een combinatie van de symbolen en operators die kunnen worden geëvalueerd om te verkrijgen van een enkele waarde. Eenvoudige expressies kunnen bestaan constanten, eigenschap verwijzingen, matrix element verwijzingen, alias-verwijzingen of functieaanroepen. Eenvoudige expressies kunnen worden gecombineerd tot complexe expressies met operators.  
  
 Zie voor informatie over welke scalaire expressie die u wellicht waarden, [constanten](#bk_constants) sectie.  
  
 **Syntaxis**  
  
```  
<scalar_expression> ::=  
       <constant>   
     | input_alias   
     | parameter_name  
     | <scalar_expression>.property_name  
     | <scalar_expression>'['"property_name"|array_index']'  
     | unary_operator <scalar_expression>  
     | <scalar_expression> binary_operator <scalar_expression>    
     | <scalar_expression> ? <scalar_expression> : <scalar_expression>  
     | <scalar_function_expression>  
     | <create_object_expression>   
     | <create_array_expression>  
     | (<scalar_expression>)   
  
<scalar_function_expression> ::=  
        'udf.' Udf_scalar_function([<scalar_expression>][,…n])  
        | builtin_scalar_function([<scalar_expression>][,…n])  
  
<create_object_expression> ::=  
   '{' [{property_name | "property_name"} : <scalar_expression>][,…n] '}'  
  
<create_array_expression> ::=  
   '[' [<scalar_expression>][,…n] ']'  
  
```  
  
 **Argumenten**  
  
-   `<constant>`  
  
     Vertegenwoordigt een constante waarde. Zie [constanten](#bk_constants) sectie voor meer informatie.  
  
-   `input_alias`  
  
     Hiermee geeft u een waarde die is gedefinieerd door de `input_alias` geïntroduceerd in de `FROM` component.  
    Deze waarde kan niet worden gegarandeerd **niet-gedefinieerde** –**niet-gedefinieerde** waarden in de invoer worden overgeslagen.  
  
-   `<scalar_expression>.property_name`  
  
     Vertegenwoordigt een waarde van de eigenschap van een object. Als de eigenschap niet bestaat of eigenschap van een waarde die niet een object wordt verwezen, wordt de expressie resulteert in **niet-gedefinieerde** waarde.  
  
-   `<scalar_expression>'['"property_name"|array_index']'`  
  
     Hiermee geeft u een waarde van de eigenschap met de naam `property_name` of matrixelement met index `array_index` van een objectmatrix. Als de eigenschap/matrixindex niet bestaat of de eigenschap/matrixindex op een waarde die niet een objectmatrix wordt verwezen, klikt u vervolgens resulteert de expressie in niet-gedefinieerde waarde.  
  
-   `unary_operator <scalar_expression>`  
  
     Vertegenwoordigt een operator die wordt toegepast op een enkele waarde. Zie [Operators](#bk_operators) sectie voor meer informatie.  
  
-   `<scalar_expression> binary_operator <scalar_expression>`  
  
     Vertegenwoordigt een operator die wordt toegepast op twee waarden. Zie [Operators](#bk_operators) sectie voor meer informatie.  
  
-   `<scalar_function_expression>`  
  
     Vertegenwoordigt een waarde die is gedefinieerd door een resultaat van een functieaanroep.  
  
-   `udf_scalar_function`  
  
     Naam van de gebruiker gedefinieerde scalaire functie.  
  
-   `builtin_scalar_function`  
  
     De naam van de ingebouwde scalaire functie.  
  
-   `<create_object_expression>`  
  
     Hiermee geeft u een waarde die wordt verkregen door het maken van een nieuw object met de opgegeven eigenschappen en hun waarden.  
  
-   `<create_array_expression>`  
  
     Hiermee geeft u een waarde die wordt verkregen door het maken van een nieuwe matrix met de opgegeven waarden als elementen  
  
-   `parameter_name`  
  
     Vertegenwoordigt een waarde van de opgegeven parameternaam. Parameternamen moeten één @ hebben als het eerste teken.  
  
 **Opmerkingen**  
  
 Bij het aanroepen van een ingebouwde of de gebruiker gedefinieerde scalaire functie moeten alle argumenten worden gedefinieerd. Als een van de argumenten is gedefinieerd, wordt de functie wordt niet aangeroepen en wordt het resultaat is niet gedefinieerd.  
  
 Bij het maken van een object, worden alle eigenschappen die niet-gedefinieerde waarde is toegewezen overgeslagen en niet is opgenomen in het gemaakte object.  
  
 Wanneer het maken van een matrix, maar een elementwaarde die is toegewezen **niet-gedefinieerde** waarde wordt overgeslagen en niet is opgenomen in het gemaakte object. Hierdoor wordt het volgende gedefinieerde element de plaatsvindt zodanig dat de gemaakte matrix wordt niet hebt overgeslagen indexen.  
  
##  <a name="bk_operators"></a>Operators  
 Deze sectie beschrijft de ondersteunde operators. Elke operator kan worden toegewezen aan één categorie.  
  
 Zie **Operator categorieën** tabel hieronder voor meer informatie met betrekking tot de verwerking van **niet-gedefinieerde** waarden, type-vereisten voor de invoerwaarden en verwerking van waarden met niet overeenkomende typen.  
  
 **Operator categorieën:**  
  
|**Categorie**|**Details**|  
|-|-|  
|**rekenkundige bewerking**|Operator input(s) (s) worden verwacht. Uitvoer is ook een getal. Als een van de invoerwaarden **niet-gedefinieerde** of type dan het aantal vervolgens het resultaat is **niet-gedefinieerde**.|  
|**Bitsgewijze**|Operator verwacht input(s) worden 32-bits geheel getal met teken (s). Uitvoer is ook een 32-bits geheel getal.<br /><br /> Een niet-integerwaarde worden afgerond. Positieve waarde wordt afgerond, negatieve getallen naar boven afgerond.<br /><br /> Een willekeurige waarde die buiten het bereik voor 32-bits geheel getal wordt geconverteerd, door middel van de laatste 32-bits van de twee van bits.<br /><br /> Als een van de invoerwaarden **niet-gedefinieerde** of andere dan getal, typ dan is het resultaat **niet-gedefinieerde**.<br /><br /> **Opmerking:** het bovenstaande gedrag is compatibel met JavaScript bitsgewijze operator gedrag.|  
|**logische**|Operator input(s) Boolean(s) worden verwacht. Uitvoer is ook een Booleaanse waarde.<br />Als een van de invoerwaarden **niet-gedefinieerde** of typ dan Boolean, wordt het resultaat **niet-gedefinieerde**.|  
|**vergelijking**|Operator verwacht input(s) hetzelfde type en niet worden gedefinieerd. Uitvoer is een Booleaanse waarde.<br /><br /> Als een van de invoerwaarden **niet-gedefinieerde** of de invoerwaarden hebben verschillende typen en vervolgens het resultaat is **niet-gedefinieerde**.<br /><br /> Zie **volgorde van waarden voor vergelijking** tabel voor waarde ordening details.|  
|**tekenreeks**|Operator input(s) meer worden verwacht. Uitvoer is ook een tekenreeks.<br />Als een van de invoerwaarden **niet-gedefinieerde** of andere dan tekenreeks typen en vervolgens het resultaat is **niet-gedefinieerde**.|  
  
 **Unaire operators:**  
  
|**Naam**|**Operator**|**Details**|  
|-|-|-|  
|**rekenkundige bewerking**|+<br /><br /> -|Retourneert de waarde.<br /><br /> Bitsgewijze onderhandeling. De numerieke waarde retourneert genegeerde.|  
|**Bitsgewijze**|~|Toepassingsgroepen de aanvulling. Retourneert een reeks een numerieke waarde.|  
|**Logische**|**NIET**|Negatie. Retourneert genegeerde Booleaanse waarde.|  
  
 **Binaire operators:**  
  
|**Naam**|**Operator**|**Details**|  
|-|-|-|  
|**rekenkundige bewerking**|+<br /><br /> -<br /><br /> *<br /><br /> /<br /><br /> %|Toevoeging.<br /><br /> Aftrekken.<br /><br /> Vermenigvuldigen.<br /><br /> Deling.<br /><br /> Modulatie.|  
|**Bitsgewijze**|&#124;<br /><br /> &<br /><br /> ^<br /><br /> <<<br /><br /> >><br /><br /> >>>|Bitsgewijze OR.<br /><br /> Bitsgewijze AND.<br /><br /> Bitsgewijze XOR.<br /><br /> Verschuiving naar links.<br /><br /> Rechts verplaatsen.<br /><br /> Nul opvulling rechts verplaatsen.|  
|**logische**|**EN**<br /><br /> **OR**|Logische verbinding. Retourneert **true** als beide argumenten zijn **true**, retourneert **false** anders.<br /><br /> Logische verbinding. Retourneert **true** als beide argumenten zijn **true**, retourneert **false** anders.|  
|**vergelijking**|**=**<br /><br /> **!=, <>**<br /><br /> **>**<br /><br /> **>=**<br /><br /> **<**<br /><br /> **<=**<br /><br /> **??**|Is gelijk aan. Retourneert **true** als argumenten gelijk zijn, retourneert **false** anders.<br /><br /> Niet gelijk aan. Retourneert **true** als argumenten niet gelijk zijn, retourneert **false** anders.<br /><br /> Groter dan. Retourneert **true** als eerste argument groter dan het tweede is, retourneren **false** anders.<br /><br /> Groter dan of gelijk zijn aan. Retourneert **true** als eerste argument groter dan of gelijk aan het tweede is, geretourneerd **false** anders.<br /><br /> Minder dan. Retourneert **true** als eerste argument kleiner is dan de tweede één terugkeer **false** anders.<br /><br /> Kleiner dan of gelijk zijn aan. Retourneert **true** als eerste argument kleiner dan of gelijk zijn aan de tweede waarde is retourneren **false** anders.<br /><br /> Coalesce. Het tweede argument retourneert als het eerste argument is een **niet-gedefinieerde** waarde.|  
|**Tekenreeks**|**&#124;&#124;**|Samenvoeging. Retourneert een samenvoeging van beide argumenten.|  
  
 **Ternair operators:**  
  
|Ternaire operator|?|Het tweede argument retourneert als het eerste argument wordt geëvalueerd naar **true**; anders het derde argument retourneren.|  
|-|-|-|  
  
 **Ordening van waarden voor vergelijking**  
  
|**Type**|**De volgorde van de waarden**|  
|-|-|  
|**Niet-gedefinieerd**|Niet worden vergeleken.|  
|**Null**|Eén waarde: **null**|  
|**Aantal**|Natuurlijke reëel getal.<br /><br /> Negatieve oneindige waarde is kleiner dan andere numerieke waarde.<br /><br /> Positieve oneindige waarde is groter dan andere numerieke waarde. **NaN** waarde kan niet worden vergeleken. Vergelijken met **NaN** leidt ertoe dat **niet-gedefinieerde** waarde.|  
|**Tekenreeks**|Lexicographical volgorde.|  
|**Matrix**|Er is geen ordening, maar billijke.|  
|**Object**|Er is geen ordening, maar billijke.|  
  
 **Opmerkingen**  
  
 In Azure Cosmos DB, worden de typen waarden vaak niet bekend totdat ze daadwerkelijk worden opgehaald uit de database. Om efficiënt uitvoeren van query's te ondersteunen, hebben de meeste van de operators strikt type vereisten. Operators zelf voeren ook geen impliciete conversies.  
  
 Dit betekent dat een query, zoals: Selecteer * van ROOT r waar r.Age = 21 retourneert alleen documenten met eigenschap leeftijd gelijk zijn aan het getal 21. Documenten met leeftijd gelijk is aan de tekenreeks '21' of de tekenreeks '0021'-eigenschap wordt niet overeenkomen, als de expressie '21' = 21 naar niet-gedefinieerde evalueert. Hiermee kunt u een beter gebruik van indexen, omdat het opzoeken van een specifieke waarde (dat wil zeggen 21 number) is sneller dan zoeken naar oneindig aantal mogelijke overeenkomsten (dat wil zeggen getal 21 of tekenreeksen '21', '021', "21.0"...). Dit wijkt af van hoe JavaScript operators van waarden van verschillende typen evalueert.  
  
 **Matrices en objecten gelijkheid en het vergelijkingstype**  
  
 Vergelijken van de matrix of Object waarden met bereik operators (>, > =, <, < =) leidt ertoe dat niet-gedefinieerde omdat er geen volgorde gedefinieerd op het Object of de matrix waarden. Echter met gelijk-ongelijk operators (=,! =, <>) wordt ondersteund en waarden structureel's worden vergeleken.  
  
 Matrices zijn gelijk als beide matrices hetzelfde aantal elementen hebben en elementen op die overeenkomt met de posities ook gelijk zijn. Het resultaat van de vergelijking van de matrix is niet gedefinieerd als niet alle elementen resulteert in een paar vergelijken gedefinieerd.  
  
 Objecten zijn gelijk als beide objecten hebben dezelfde eigenschappen die zijn gedefinieerd, en als de waarden van eigenschappen die overeenkomt met ook gelijk zijn. Als het vergelijken van elk paar eigenschap waarden resulteert in een niet-gedefinieerd, is het resultaat van de vergelijking object is niet gedefinieerd.  
  
##  <a name="bk_constants"></a>Constanten  
 Een constante, ook wel bekend als een letterlijke waarde of een scalaire waarde is een symbool dat de waarde van een specifieke gegevens vertegenwoordigt. De indeling van een constante is afhankelijk van het gegevenstype van de waarde vertegenwoordigt.  
  
 **Scalaire-gegevenstypen ondersteund:**  
  
|**Type**|**De volgorde van de waarden**|  
|-|-|  
|**Niet-gedefinieerd**|Eén waarde: **niet gedefinieerd**|  
|**Null**|Eén waarde: **null**|  
|**Booleaanse waarde**|Waarden: **false**, **true**.|  
|**Aantal**|Een getal met dubbele precisie drijvende komma IEEE-norm 754.|  
|**Tekenreeks**|Een reeks van nul of meer Unicode-tekens. Tekenreeksen moeten worden ingesloten in enkele of dubbele aanhalingstekens.|  
|**Matrix**|Een reeks van nul of meer elementen. Elk element is een waarde van elk gegevenstype scalaire, behalve Undefined.|  
|**Object**|Een niet-geordende reeks nul of meer naam/waarde-paren. De naam van een Unicode-tekenreeks is, de waarde kan zijn van elk gegevenstype scalaire, behalve **Undefined**.|  
  
 **Syntaxis**  
  
```  
<constant> ::=  
   <undefined_constant>  
     | <null_constant>   
     | <boolean_constant>   
     | <number_constant>   
     | <string_constant>   
     | <array_constant>   
     | <object_constant>   
  
<undefined_constant> ::= undefined  
  
<null_constant> ::= null  
  
<boolean_constant> ::= false | true  
  
<number_constant> ::= decimal_literal | hexadecimal_literal  
  
<string_constant> ::= string_literal  
  
<array_constant> ::=  
    '[' [<constant>][,...n] ']'  
  
<object_constant> ::=   
   '{' [{property_name | "property_name"} : <constant>][,...n] '}'  
  
```  
  
 **Argumenten**  
  
1.  `<undefined_constant>; undefined`  
  
     Geeft niet-gedefinieerde waarde van het type niet gedefinieerd.  
  
2.  `<null_constant>; null`  
  
     Hiermee geeft u **null** waarde van het type **Null**.  
  
3.  `<boolean_constant>`  
  
     Constante van het type Booleaans vertegenwoordigt.  
  
4.  `false`  
  
     Hiermee geeft u **false** waarde van het type Boole-waarde.  
  
5.  `true`  
  
     Hiermee geeft u **true** waarde van het type Boole-waarde.  
  
6.  `<number_constant>`  
  
     Hiermee geeft u een constante.  
  
7.  `decimal_literal`  
  
     Decimale letterlijke waarden zijn getallen weergegeven met behulp van de decimale notatie of wetenschappelijke notatie.  
  
8.  `hexadecimal_literal`  
  
     Hexadecimale letterlijke waarden zijn getallen weergegeven met behulp van het voorvoegsel '0 x' gevolgd door een of meer hexadecimale cijfers.  
  
9. `<string_constant>`  
  
     Hiermee geeft u een constante van het type String.  
  
10. `string _literal`  
  
     Letterlijke tekenreeks zijn vertegenwoordigd door een reeks van nul of meer Unicode-tekens of escapereeksen Unicode-tekenreeksen. Letterlijke tekenreeks zijn ingesloten in enkele aanhalingstekens (apostrof: ') of dubbele aanhalingstekens (aanhalingsteken: ').  
  
 Volgende escapereeksen worden toegestaan:  
  
|**Escape-reeks**|**Beschrijving**|**Unicode-teken**|  
|-|-|-|  
|\\'|enkel aanhalingsteken (')|U + 0027|  
|\\"|aanhalingsteken (")|U + 0022|  
|\\\|omgekeerde schuine streep (\\)|U + 005C|  
|\\/|schuine streep (/)|U + 002F|  
|\ber|BACKSPACE|U + 0008|  
|\f|formulier feed|U + 000C|  
|\n|nieuwe regel|U + 000A|  
|\r|regelterugloop|U + 000D|  
|\t|tabblad|U + 0009|  
|\uXXXX|Een Unicode-teken is gedefinieerd door 4 hexadecimale cijfers.|U + XXXX|  
  
##  <a name="bk_query_perf_guidelines"></a>Richtlijnen voor query-prestaties  
 Om een query voor het efficiënt voor een grote verzameling worden uitgevoerd, moet deze filters die kunnen worden geleverd via een of meer indexen gebruiken.  
  
 De volgende filters wordt voor het opzoeken van de index beschouwd:  
  
-   Gebruik gelijkheidsoperator (=) met een padexpressie document en een constante.  
  
-   Bereikoperatoren gebruiken (<, \<=, >, > =) met een padexpressie document en aantal constanten.  
  
-   Document padexpressie staat voor een expressie waarmee een constante pad in de documenten uit de verzameling database waarnaar wordt verwezen.  
  
 **Document padexpressie**  
  
 Document padexpressies zijn expressies die een pad van de eigenschap of matrix indexeerfunctie beoordelaars via een document dat afkomstig is van de database verzameling documenten. Dit pad kan worden gebruikt voor het identificeren van de locatie van de waarden waarnaar wordt verwezen in een filter rechtstreeks in de documenten in de database-verzameling.  
  
 Voor een expressie die moet worden beschouwd als een document padexpressie, de volgende vereisten:  
  
1.  Verwijst rechtstreeks naar de hoofdmap van de verzameling.  
  
2.  Verwijzing naar eigenschap of constante matrix indexeerfunctie van sommige padexpressie document  
  
3.  Verwijzen naar een alias die bepaalde padexpressie document vertegenwoordigt.  
  
     **Syntaxis conventies**  
  
     De volgende tabel beschrijft de conventies gebruikt om te beschrijven syntaxis in de volgende SQL-verwijzing.  
  
    |**Conventies**|**Gebruikt voor**|  
    |-|-|    
    |HOOFDLETTERS|Niet-hoofdlettergevoelige trefwoorden.|  
    |kleine letters|Trefwoorden hoofdlettergevoelig.|  
    |\<nonterminal >|Niet-eindigende, die is gedefinieerd afzonderlijk.|  
    |\<nonterminal >:: =|De definitie van de syntaxis van de nonterminal.|  
    |other_terminal|Terminal (token) in de woorden in detail beschreven.|  
    |ID|ID. Kan de volgende tekens bevatten: a-z A-Z 0-9 _First teken mag niet een cijfer.|  
    |'tekenreeks'|Tekenreeks tussen aanhalingstekens. Hiermee kunt een geldige tekenreeks. Zie de beschrijving van een string_literal.|  
    |"symbool"|Letterlijke symbool die deel uitmaakt van de syntaxis.|  
    |&#124; (verticale balk)|Alternatieven voor de syntaxis van de items. U kunt slechts één van de items die zijn opgegeven.|  
    |[] /(brackets)|Vierkante haken plaatst u een of meer optionele items.|  
    |[,.. .n]|Hiermee geeft u het vorige item kunt herhaalde n het aantal keren. De exemplaren worden gescheiden door komma's.|  
    |[.. .n]|Hiermee geeft u het vorige item kunt herhaalde n het aantal keren. De exemplaren worden gescheiden door spaties.|  
  
##  <a name="bk_built_in_functions"></a>Ingebouwde functies  
 Azure Cosmos DB biedt veel ingebouwde SQL-functies. De categorieën van ingebouwde functies worden hieronder vermeld.  
  
|Functie|Beschrijving|  
|--------------|-----------------|  
|[Wiskundige functies](#bk_mathematical_functions)|De rekenkundige functies elke uitvoeren een berekening, meestal op basis van de invoerwaarden die zijn opgegeven als argumenten en een numerieke waarde retourneren.|  
|[Controle van de functies van het type](#bk_type_checking_functions)|Het type controleren of functies kunnen u om te controleren van het type van een expressie in SQL-query's.|  
|[Tekenreeksfuncties](#bk_string_functions)|De tekenreeksfuncties een bewerking uitvoeren op een tekenreekswaarde van de invoer en een tekenreeks, numerieke of Booleaanse waarde retourneren.|  
|[Matrixfuncties](#bk_array_functions)|De matrixfuncties uitvoeren voor een bewerking in een matrix invoerwaarde en keer terug numerieke, de waarde voor Boole-waarde of een matrix.|  
|[Ruimtelijke functies](#bk_spatial_functions)|De ruimtelijke functies een bewerking uitvoeren op een invoerwaarde ruimtelijke object en een numeriek gegevenstype of Booleaanse waarde retourneren.|  
  
###  <a name="bk_mathematical_functions"></a>Wiskundige functies  
 De volgende functies elke uitvoeren van een berekening, meestal op basis van de invoerwaarden die zijn opgegeven als argumenten en een numerieke waarde retourneren.  
  
||||  
|-|-|-|  
|[ABS](#bk_abs)|[BOOGCOS](#bk_acos)|[ASIN](#bk_asin)|  
|[BOOGTAN](#bk_atan)|[ATN2](#bk_atn2)|[MAXIMUM](#bk_ceiling)|  
|[CO 'S](#bk_cos)|[COT](#bk_cot)|[GRADEN](#bk_degrees)|  
|[EXP](#bk_exp)|[FLOOR](#bk_floor)|[LOGBOEK](#bk_log)|  
|[LOG10](#bk_log10)|[PI](#bk_pi)|[ENERGIEBEHEER](#bk_power)|  
|[RADIALEN](#bk_radians)|[AFRONDEN](#bk_round)|[SIN](#bk_sin)|  
|[SQRT](#bk_sqrt)|[VIERKANTE](#bk_square)|[AANMELDING](#bk_sign)|  
|[TAN](#bk_tan)|[GEHEEL](#bk_trunc)||  
  
####  <a name="bk_abs"></a>ABS  
 Retourneert de absolute (positieve) waarde van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
ABS (<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld toont de resultaten van het gebruik van de functie ABS op drie verschillende aantallen.  
  
```  
SELECT ABS(-1), ABS(0), ABS(1)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 1, $2: 0, $3: 1}]  
```  
  
####  <a name="bk_acos"></a>BOOGCOS  
 Retourneert de hoek in radialen, waarvan de cosinus de opgegeven numerieke expressie is. ook wel de boogcosinus genoemd.  
  
 **Syntaxis**  
  
```  
ACOS(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert de BOOGCOS van-1.  
  
```  
SELECT ACOS(-1)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <a name="bk_asin"></a>ASIN  
 Retourneert de hoek in radialen, waarvan de sinus de opgegeven numerieke expressie is. Dit wordt ook boogsinus genoemd.  
  
 **Syntaxis**  
  
```  
ASIN(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert de ASIN van-1.  
  
```  
SELECT ASIN(-1)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": -1.5707963267948966}]  
```  
  
####  <a name="bk_atan"></a>BOOGTAN  
 Retourneert de hoek in radialen, waarvan de tangens de opgegeven numerieke expressie is. Dit wordt ook arctangens genoemd.  
  
 **Syntaxis**  
  
```  
ATAN(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert de BOOGTAN van de opgegeven waarde.  
  
```  
SELECT ATAN(-45.01)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": -1.5485826962062663}]  
```  
  
####  <a name="bk_atn2"></a>ATN2  
 Retourneert de hoofdsom van de arctangens van y / x, uitgedrukt in radialen.  
  
 **Syntaxis**  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt de ATN2 voor het opgegeven berekend x en y onderdelen.  
  
```  
SELECT ATN2(35.175643, 129.44)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": 1.3054517947300646}]  
```  
  
####  <a name="bk_ceiling"></a>MAXIMUM  
 Retourneert de kleinste waarde van geheel getal groter dan of gelijk zijn aan de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
CEILING (<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld toont een positieve numerieke, negatieve en nulwaarden met de functie CEILING.  
  
```  
SELECT CEILING(123.45), CEILING(-123.45), CEILING(0.0)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 124, $2: -123, $3: 0}]  
```  
  
####  <a name="bk_cos"></a>CO 'S  
 Retourneert de trigonometrische cosinus van de opgegeven hoek in radialen in de opgegeven expressie.  
  
 **Syntaxis**  
  
```  
COS(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld berekent de Cosinus van de opgegeven hoek.  
  
```  
SELECT COS(14.78)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": -0.59946542619465426}]  
```  
  
####  <a name="bk_cot"></a>COT  
 Retourneert de trigonometrische cotangens van de opgegeven hoek in radialen in het opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
COT(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld berekent de COT van de opgegeven hoek.  
  
```  
SELECT COT(124.1332)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": -0.040311998371148884}]  
```  
  
####  <a name="bk_degrees"></a>GRADEN  
 Retourneert de overeenkomstige hoek in graden voor een hoek aangeduid in radialen.  
  
 **Syntaxis**  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert het aantal graden in een hoek van PI/2 radialen.  
  
```  
SELECT DEGREES(PI()/2)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": 90}]  
```  
  
####  <a name="bk_floor"></a>FLOOR  
 Retourneert het grootste gehele getal kleiner dan of gelijk zijn aan de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld toont een positieve numerieke, negatieve en nulwaarden met de functie FLOOR.  
  
```  
SELECT FLOOR(123.45), FLOOR(-123.45), FLOOR(0.0)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 123, $2: -124, $3: 0}]  
```  
  
####  <a name="bk_exp"></a>EXP  
 Berekent de exponentiële waarde van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
EXP (<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Opmerkingen**  
  
 De constante **e** (2.718281...) de basis van de natuurlijke logaritme.  
  
 De exponent van een getal is de constante **e** tot de macht van het getal. Bijvoorbeeld EXP(1.0) e = ^ 1.0 = 2.71828182845905 en EXP(10) e = ^ 10 = 22026.4657948067.  
  
 De exponentiële waarde van de natuurlijke logaritme van een getal is het aantal zelf: EXP (LOGBOEKREGISTRATIE (n)) = n. En de natuurlijke logaritme van de exponentiële van een getal is het aantal zelf: logboek (EXP (n)) = n.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt een variabele gedeclareerd en berekent de exponentiële waarde van de opgegeven variabele (10).  
  
```  
SELECT EXP(10)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 22026.465794806718}]  
```  
  
 Het volgende voorbeeld retourneert de exponentiële waarde van de natural logarithm van 20 en de natuurlijke logaritme van de exponentiële van 20. Omdat deze functies inverse functies van elkaar zijn, is de retourwaarde voor drijvende punt math in beide gevallen afgeronde 20.  
  
```  
SELECT EXP(LOG(20)), LOG(EXP(20))  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 19.999999999999996, $2: 20}]  
```  
  
####  <a name="bk_log"></a>LOGBOEK  
 Retourneert de natuurlijke logaritme van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
-   `base`  
  
     Optionele numerieke argument dat Hiermee stelt u de basis voor de logaritme.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Opmerkingen**  
  
 Standaard retourneert LOG() de natuurlijke logaritme. U kunt het grondtal van de logaritme met een andere waarde wijzigen met behulp van de optionele parameter basis.  
  
 De natuurlijke logaritme is de logaritme met het grondtal **e**, waarbij **e** is een constante irrational ongeveer gelijk aan 2.718281828.  
  
 De natuurlijke logaritme van de exponentiële van een getal is het aantal zelf: logboek (EXP (n)) = n. En de exponentiële waarde van de natuurlijke logaritme van een getal is het aantal zelf: EXP (LOGBOEKREGISTRATIE (n)) = n.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt een variabele gedeclareerd en retourneert de waarde van de logaritme van de opgegeven variabele (10).  
  
```  
SELECT LOG(10)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 2.3025850929940459}]  
```  
  
 Het volgende voorbeeld wordt het logboek voor de exponent van een getal berekend.  
  
```  
SELECT EXP(LOG(10))  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 10.000000000000002}]  
```  
  
####  <a name="bk_log10"></a>LOG10  
 Retourneert de logaritme met grondtal 10 van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Opmerkingen**  
  
 De functies LOG10 en POWER zijn omgekeerd aan elkaar gerelateerd. Bijvoorbeeld 10 ^ LOG10(n) = n.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt een variabele gedeclareerd en retourneert de waarde LOG10 van de opgegeven variabele (100).  
  
```  
SELECT LOG10(100)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 2}]  
```  
  
####  <a name="bk_pi"></a>PI  
 Retourneert de constante waarde van PI.  
  
 **Syntaxis**  
  
```  
PI ()  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert de waarde van PI.  
  
```  
SELECT PI()  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <a name="bk_power"></a>ENERGIEBEHEER  
 Retourneert de waarde van de opgegeven expressie voor de opgegeven macht.  
  
 **Syntaxis**  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
-   `y`  
  
     Is de macht waarnaar u wilt activeren `numeric_expression`.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt een getal tot de macht 3 (de kubus van het getal) te verhogen.  
  
```  
SELECT POWER(2, 3), POWER(2.5, 3)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 8, $2: 15.625}]  
```  
  
####  <a name="bk_radians"></a>RADIALEN  
 Retourneert radialen wanneer een numerieke expressie, in graden wordt ingevoerd.  
  
 **Syntaxis**  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld duurt een paar hoeken als invoer en de bijbehorende Radiaal waarden retourneert.  
  
```  
SELECT RADIANS(-45.01), RADIANS(-181.01), RADIANS(0), RADIANS(0.1472738), RADIANS(197.1099392)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{  
       "$1": -0.7855726963226477,  
       "$2": -3.1592204790349356,  
       "$3": 0,  
       "$4": 0.0025704127119236249,  
       "$5": 3.4402174274458375  
   }]  
```  
  
####  <a name="bk_round"></a>AFRONDEN  
 Retourneert een numerieke waarde, op het dichtstbijzijnde gehele getal afgerond.  
  
 **Syntaxis**  
  
```  
ROUND(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt de volgende positieve en negatieve getallen naar het dichtstbijzijnde gehele getal afgerond.  
  
```  
SELECT ROUND(2.4), ROUND(2.6), ROUND(2.5), ROUND(-2.4), ROUND(-2.6)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 2, $2: 3, $3: 3, $4: -2, $5: -3}]  
```  
  
####  <a name="bk_sign"></a>AANMELDING  
 Retourneert de positief (+ 1), nul (0) of minteken (-1) van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
SIGN(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert de waarden van de aanmelding van de getallen van -2 op 2.  
  
```  
SELECT SIGN(-2), SIGN(-1), SIGN(0), SIGN(1), SIGN(2)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: -1, $2: -1, $3: 0, $4: 1, $5: 1}]  
```  
  
####  <a name="bk_sin"></a>SIN  
 Retourneert de trigonometrische sinus van de opgegeven hoek in radialen in de opgegeven expressie.  
  
 **Syntaxis**  
  
```  
SIN(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld berekent de SINUS van de opgegeven hoek.  
  
```  
SELECT SIN(45.175643)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": 0.929607286611012}]  
```  
  
####  <a name="bk_sqrt"></a>SQRT  
 Retourneert de vierkantswortel van de numerieke waarde die is opgegeven.  
  
 **Syntaxis**  
  
```  
SQRT(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert de hoofdmappen kwadraat van getallen 1-3.  
  
```  
SELECT SQRT(1), SQRT(2.0), SQRT(3)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 1, $2: 1.4142135623730952, $3: 1.7320508075688772}]  
```  
  
####  <a name="bk_square"></a>VIERKANTE  
 Retourneert het kwadraat van de numerieke waarde die is opgegeven.  
  
 **Syntaxis**  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert de kwadraten van getallen 1-3.  
  
```  
SELECT SQUARE(1), SQUARE(2.0), SQUARE(3)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 1, $2: 4, $3: 9}]  
```  
  
####  <a name="bk_tan"></a>TAN  
 Retourneert de tangens van de opgegeven hoek in radialen in de opgegeven expressie.  
  
 **Syntaxis**  
  
```  
TAN (<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt de tangens van PI () berekend / 2.  
  
```  
SELECT TAN(PI()/2);  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": 16331239353195370 }]  
```  
  
####  <a name="bk_trunc"></a>GEHEEL  
 Retourneert een numerieke waarde, afgekapt tot het dichtstbijzijnde gehele getal.  
  
 **Syntaxis**  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Is een numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt de volgende positieve en negatieve getallen naar het dichtstbijzijnde gehele getal afgekapt.  
  
```  
SELECT TRUNC(2.4), TRUNC(2.6), TRUNC(2.5), TRUNC(-2.4), TRUNC(-2.6)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 2, $2: 2, $3: 2, $4: -2, $5: -2}]  
```  
  
###  <a name="bk_type_checking_functions"></a>Controle van de functies van het type  
 De volgende functies ondersteunen typecontrole tegen invoerwaarden en elk een Booleaanse waarde retourneren.  
  
||||  
|-|-|-|  
|[IS_ARRAY](#bk_is_array)|[IS_BOOL](#bk_is_bool)|[IS_DEFINED](#bk_is_defined)|  
|[IS_NULL](#bk_is_null)|[IS_NUMBER](#bk_is_number)|[IS_OBJECT](#bk_is_object)|  
|[IS_PRIMITIVE](#bk_is_primitive)|[IS_STRING](#bk_is_string)||  
  
####  <a name="bk_is_array"></a>IS_ARRAY  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een matrix is.  
  
 **Syntaxis**  
  
```  
IS_ARRAY(<expression>)  
```  
  
 **Argumenten**  
  
-   `expression`  
  
     Is geldige expressie.  
  
 **Retourtypen**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld controleert objecten van JSON Boolean, getal, string, null, object, matrix en niet-gedefinieerde typen met de functie IS_ARRAY.  
  
```  
SELECT   
 IS_ARRAY(true),   
 IS_ARRAY(1),  
 IS_ARRAY("value"),  
 IS_ARRAY(null),  
 IS_ARRAY({prop: "value"}),   
 IS_ARRAY([1, 2, 3]),  
 IS_ARRAY({prop: "value"}.prop2)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: false, $6: true}]  
```  
  
####  <a name="bk_is_bool"></a>IS_BOOL  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een Booleaanse waarde is.  
  
 **Syntaxis**  
  
```  
IS_BOOL(<expression>)  
```  
  
 **Argumenten**  
  
-   `expression`  
  
     Is geldige expressie.  
  
 **Retourtypen**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld controleert objecten van JSON Boolean, getal, string, null, object, matrix en niet-gedefinieerde typen met de functie IS_BOOL.  
  
```  
SELECT   
    IS_BOOL(true),   
    IS_BOOL(1),  
    IS_BOOL("value"),   
    IS_BOOL(null),  
    IS_BOOL({prop: "value"}),   
    IS_BOOL([1, 2, 3]),  
    IS_BOOL({prop: "value"}.prop2)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: true, $2: false, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_defined"></a>IS_DEFINED  
 Retourneert een Booleaanse waarde die aangeeft of de eigenschap een waarde is toegewezen.  
  
 **Syntaxis**  
  
```  
IS_DEFINED(<expression>)  
```  
  
 **Argumenten**  
  
-   `expression`  
  
     Is geldige expressie.  
  
 **Retourtypen**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt gecontroleerd op de aanwezigheid van een eigenschap binnen het opgegeven JSON-document. De eerste retourneert true omdat "a" aanwezig is, maar de tweede onwaar retourneert omdat "b" ontbreekt.  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a), IS_DEFINED({ "a" : 5 }.b)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{  
       "$1": true,    
       "$2": false   
   }]  
```  
  
####  <a name="bk_is_null"></a>IS_NULL  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie null is.  
  
 **Syntaxis**  
  
```  
IS_NULL(<expression>)  
```  
  
 **Argumenten**  
  
-   `expression`  
  
     Is geldige expressie.  
  
 **Retourtypen**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld controleert objecten van JSON Boolean, getal, string, null, object, matrix en niet-gedefinieerde typen met de functie IS_NULL.  
  
```  
SELECT   
    IS_NULL(true),   
    IS_NULL(1),  
    IS_NULL("value"),   
    IS_NULL(null),  
    IS_NULL({prop: "value"}),   
    IS_NULL([1, 2, 3]),  
    IS_NULL({prop: "value"}.prop2)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: false, $2: false, $3: false, $4: true, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_number"></a>IS_NUMBER  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie is van een getal.  
  
 **Syntaxis**  
  
```  
IS_NUMBER(<expression>)  
```  
  
 **Argumenten**  
  
-   `expression`  
  
     Is geldige expressie.  
  
 **Retourtypen**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld controleert objecten van JSON Boolean, getal, string, null, object, matrix en niet-gedefinieerde typen met de functie IS_NULL.  
  
```  
SELECT   
    IS_NUMBER(true),   
    IS_NUMBER(1),  
    IS_NUMBER("value"),   
    IS_NUMBER(null),  
    IS_NUMBER({prop: "value"}),   
    IS_NUMBER([1, 2, 3]),  
    IS_NUMBER({prop: "value"}.prop2)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: false, $2: true, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_object"></a>IS_OBJECT  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een JSON-object is.  
  
 **Syntaxis**  
  
```  
IS_OBJECT(<expression>)  
```  
  
 **Argumenten**  
  
-   `expression`  
  
     Is geldige expressie.  
  
 **Retourtypen**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld controleert objecten van JSON Boolean, getal, string, null, object, matrix en niet-gedefinieerde typen met de functie IS_OBJECT.  
  
```  
SELECT   
    IS_OBJECT(true),   
    IS_OBJECT(1),  
    IS_OBJECT("value"),   
    IS_OBJECT(null),  
    IS_OBJECT({prop: "value"}),   
    IS_OBJECT([1, 2, 3]),  
    IS_OBJECT({prop: "value"}.prop2)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: true, $6: false}]  
```  
  
####  <a name="bk_is_primitive"></a>IS_PRIMITIVE  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie is van een primitief (string, Boolean, numerieke of null).  
  
 **Syntaxis**  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 **Argumenten**  
  
-   `expression`  
  
     Is geldige expressie.  
  
 **Retourtypen**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld controleert objecten van JSON Boolean, getal, string, null, object, matrix en niet-gedefinieerde typen met de functie IS_PRIMITIVE.  
  
```  
SELECT   
           IS_PRIMITIVE(true),   
           IS_PRIMITIVE(1),  
           IS_PRIMITIVE("value"),   
           IS_PRIMITIVE(null),  
           IS_PRIMITIVE({prop: "value"}),   
           IS_PRIMITIVE([1, 2, 3]),  
           IS_PRIMITIVE({prop: "value"}.prop2)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": true, "$2": true, "$3": true, "$4": true, "$5": false, "$6": false, "$7": false}]  
```  
  
####  <a name="bk_is_string"></a>IS_STRING  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een tekenreeks is.  
  
 **Syntaxis**  
  
```  
IS_STRING(<expression>)  
```  
  
 **Argumenten**  
  
-   `expression`  
  
     Is geldige expressie.  
  
 **Retourtypen**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld controleert objecten van JSON Boolean, getal, string, null, object, matrix en niet-gedefinieerde typen met de functie IS_STRING.  
  
```  
SELECT   
       IS_STRING(true),   
       IS_STRING(1),  
       IS_STRING("value"),   
       IS_STRING(null),  
       IS_STRING({prop: "value"}),   
       IS_STRING([1, 2, 3]),  
       IS_STRING({prop: "value"}.prop2)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: false, $2: false, $3: true, $4: false, $5: false, $6: false}]  
```  
  
###  <a name="bk_string_functions"></a>Tekenreeks-functies  
 De volgende scalaire functies een bewerking uitvoeren op een tekenreekswaarde van de invoer en een tekenreeks, numerieke of Booleaanse waarde retourneren.  
  
||||  
|-|-|-|  
|[CONCAT](#bk_concat)|[BEVAT](#bk_contains)|[ENDSWITH](#bk_endswith)|  
|[INDEX_OF](#bk_index_of)|[LINKS](#bk_left)|[LENGTE](#bk_length)|  
|[LAGERE](#bk_lower)|[LTRIM](#bk_ltrim)|[VERVANGEN](#bk_replace)|  
|[REPLICEREN](#bk_replicate)|[OMKEREN](#bk_reverse)|[RECHTS](#bk_right)|  
|[RTRIM](#bk_rtrim)|[STARTSWITH](#bk_startswith)|[DE SUBTEKENREEKS](#bk_substring)|  
|[BOVENSTE](#bk_upper)|||  
  
####  <a name="bk_concat"></a>CONCAT  
 Retourneert een tekenreeks die het resultaat van het samenvoegen van twee of meer tekenreekswaarden.  
  
 **Syntaxis**  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is geldige tekenreeksexpressie.  
  
 **Retourtypen**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert de samengevoegde tekenreeks van de opgegeven waarden.  
  
```  
SELECT CONCAT("abc", "def")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "abcdef"}  
```  
  
####  <a name="bk_contains"></a>BEVAT  
 Retourneert een Boolean die aangeeft of de eerste expressie tekenreeks de tweede bevat.  
  
 **Syntaxis**  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is geldige tekenreeksexpressie.  
  
 **Retourtypen**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld controleert als "abc" bevat "ab" en "d".  
  
```  
SELECT CONTAINS("abc", "ab"), CONTAINS("abc", "d")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <a name="bk_endswith"></a>ENDSWITH  
 Retourneert een Boolean die aangeeft of de eerste tekenreeksexpressie eindigt op de tweede.  
  
 **Syntaxis**  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is geldige tekenreeksexpressie.  
  
 **Retourtypen**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert dat de "abc" eindigt op "b" en 'bc'.  
  
```  
SELECT ENDSWITH("abc", "b"), ENDSWITH("abc", "bc")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <a name="bk_index_of"></a>INDEX_OF  
 Retourneert de beginpositie van het eerste exemplaar van de tweede tekenreeksexpressie binnen de eerste expressie in de opgegeven tekenreeks is of -1 als de tekenreeks is niet gevonden.  
  
 **Syntaxis**  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is geldige tekenreeksexpressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt de index van verschillende subtekenreeksen binnen "abc".  
  
```  
SELECT INDEX_OF("abc", "ab"), INDEX_OF("abc", "b"), INDEX_OF("abc", "c")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": 0, "$2": 1, "$3": -1}]  
```  
  
####  <a name="bk_left"></a>LINKS  
 Retourneert het linkergedeelte van een tekenreeks zijn met het opgegeven aantal tekens.  
  
 **Syntaxis**  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is geldige tekenreeksexpressie.  
  
-   `num_expr`  
  
     Is geldige numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt het linkergedeelte van "abc" voor verschillende lengtewaarden.  
  
```  
SELECT LEFT("abc", 1), LEFT("abc", 2)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "a", "$2": "ab"}]  
```  
  
####  <a name="bk_length"></a>LENGTE  
 Retourneert het aantal tekens van de opgegeven tekenreeksexpressie.  
  
 **Syntaxis**  
  
```  
LENGTH(<str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is geldige tekenreeksexpressie.  
  
 **Retourtypen**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert de lengte van een tekenreeks.  
  
```  
SELECT LENGTH("abc")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": 3}]  
```  
  
####  <a name="bk_lower"></a>LAGERE  
 Retourneert een tekenreeksexpressie na hoofdletter gegevens converteren naar kleine letters.  
  
 **Syntaxis**  
  
```  
LOWER(<str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is geldige tekenreeksexpressie.  
  
 **Retourtypen**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld laat zien hoe lager gebruiken in een query.  
  
```  
SELECT LOWER("Abc")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "abc"}]  
  
```  
  
####  <a name="bk_ltrim"></a>LTRIM  
 Retourneert een tekenreeksexpressie na het verwijderen van toonaangevende lege waarden.  
  
 **Syntaxis**  
  
```  
LTRIM(<str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is geldige tekenreeksexpressie.  
  
 **Retourtypen**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld laat zien hoe LTRIM gebruiken binnen een query.  
  
```  
SELECT LTRIM("  abc"), LTRIM("abc"), LTRIM("abc   ")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "abc", "$2": "abc", "$3": "abc   "}]  
```  
  
####  <a name="bk_replace"></a>VERVANGEN  
 Vervangt alle instanties van een opgegeven string-waarde met de waarde van een andere tekenreeks.  
  
 **Syntaxis**  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is geldige tekenreeksexpressie.  
  
 **Retourtypen**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld ziet hoe u vervangen in een query.  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "This is a desk"}]  
```  
  
####  <a name="bk_replicate"></a>REPLICEREN  
 Herhaalt een string-waarde van een opgegeven aantal keren.  
  
 **Syntaxis**  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is geldige tekenreeksexpressie.  
  
-   `num_expr`  
  
     Is geldige numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld ziet hoe u REPLICEREN in een query.  
  
```  
SELECT REPLICATE("a", 3)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "aaa"}]  
```  
  
####  <a name="bk_reverse"></a>OMKEREN  
 Retourneert de omgekeerde volgorde van een string-waarde.  
  
 **Syntaxis**  
  
```  
REVERSE(<str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is geldige tekenreeksexpressie.  
  
 **Retourtypen**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld laat zien hoe REVERSE gebruiken in een query.  
  
```  
SELECT REVERSE("Abc")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "cbA"}]  
```  
  
####  <a name="bk_right"></a>RECHTS  
 Retourneert de juiste deel van een tekenreeks zijn met het opgegeven aantal tekens.  
  
 **Syntaxis**  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is geldige tekenreeksexpressie.  
  
-   `num_expr`  
  
     Is geldige numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt het rechterdeel van "abc" voor verschillende lengtewaarden.  
  
```  
SELECT RIGHT("abc", 1), RIGHT("abc", 2)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "c", "$2": "bc"}]  
```  
  
####  <a name="bk_rtrim"></a>RTRIM  
 Retourneert een tekenreeksexpressie na het afsluitende spaties verwijderen.  
  
 **Syntaxis**  
  
```  
RTRIM(<str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is geldige tekenreeksexpressie.  
  
 **Retourtypen**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld laat zien hoe RTRIM gebruiken binnen een query.  
  
```  
SELECT RTRIM("  abc"), RTRIM("abc"), RTRIM("abc   ")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "   abc", "$2": "abc", "$3": "abc"}]  
```  
  
####  <a name="bk_startswith"></a>STARTSWITH  
 Retourneert een Boolean die aangeeft of de eerste expressie tekenreeks begint met het tweede.  
  
 **Syntaxis**  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is geldige tekenreeksexpressie.  
  
 **Retourtypen**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt gecontroleerd of de tekenreeks "abc" met "b begint" en "a".  
  
```  
SELECT STARTSWITH("abc", "b"), STARTSWITH("abc", "a")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <a name="bk_substring"></a>DE SUBTEKENREEKS  
 Retourneert deel van een tekenreeksexpressie die beginnen bij de op nul gebaseerde positie opgegeven teken en blijft de opgegeven lengte of het einde van de tekenreeks.  
  
 **Syntaxis**  
  
```  
SUBSTRING(<str_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is geldige tekenreeksexpressie.  
  
-   `num_expr`  
  
     Is geldige numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert de subtekenreeks van "abc" beginnen bij 1 en voor een lengte van 1 teken.  
  
```  
SELECT SUBSTRING("abc", 1, 1)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "b"}]  
```  
  
####  <a name="bk_upper"></a>BOVENSTE  
 Retourneert een tekenreeksexpressie na kleine letter gegevens converteren naar hoofdletters.  
  
 **Syntaxis**  
  
```  
UPPER(<str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is geldige tekenreeksexpressie.  
  
 **Retourtypen**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld ziet het gebruik van hoofdletters in een query  
  
```  
SELECT UPPER("Abc")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "ABC"}]  
```  
  
###  <a name="bk_array_functions"></a>Matrixfuncties  
 De volgende scalaire functies uitvoeren van een bewerking op een matrix invoerwaarde en retourneren numerieke, Booleaanse waarde of een matrix van waarde  
  
||||  
|-|-|-|  
|[ARRAY_CONCAT](#bk_array_concat)|[ARRAY_CONTAINS](#bk_array_contains)|[ARRAY_LENGTH](#bk_array_length)|  
|[ARRAY_SLICE](#bk_array_slice)|||  
  
####  <a name="bk_array_concat"></a>ARRAY_CONCAT  
 Retourneert een matrix die het resultaat van het samenvoegen van twee of meer matrixwaarden.  
  
 **Syntaxis**  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 **Argumenten**  
  
-   `arr_expr`  
  
     Is geldige matrixexpressie.  
  
 **Retourtypen**  
  
 Retourneert een matrixexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld hoe u voor het samenvoegen van twee matrices.  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"])  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <a name="bk_array_contains"></a>ARRAY_CONTAINS  
Retourneert een Booleaanse waarde die aangeeft of de matrix bevat met de opgegeven waarde. Kunnen opgeven als overeenkomst met de volledige of gedeeltelijke. 

 **Syntaxis**  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr> [, bool_expr])  
```  
  
 **Argumenten**  
  
-   `arr_expr`  
  
     Is geldige matrixexpressie.  
  
-   `expr`  
  
     Is geldige expressie.  

-   `bool_expr`  
  
     Is een Boole-expressie.       
  
 **Retourtypen**  
  
 Retourneert een Booleaanse waarde.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld voor het lidmaatschap van een matrix met ARRAY_CONTAINS controleren.  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples"),  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": true, "$2": false}]  
```  

 Het volgende voorbeeld voor een gedeeltelijke overeenkomst van een JSON-code in een matrix met ARRAY_CONTAINS controleren.  
  
```  
SELECT  
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}, true), 
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}),
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "mangoes"}, true) 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{
  "$1": true,
  "$2": false,
  "$3": false
}] 
```  
  
####  <a name="bk_array_length"></a>ARRAY_LENGTH  
 Retourneert het aantal elementen van de opgegeven matrix-expressie.  
  
 **Syntaxis**  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 **Argumenten**  
  
-   `arr_expr`  
  
     Is geldige matrixexpressie.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld de lengte van een matrix met ARRAY_LENGTH krijgen.  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"])  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": 3}]  
```  
  
####  <a name="bk_array_slice"></a>ARRAY_SLICE  
 Retourneert deel van een matrixexpressie.
  
 **Syntaxis**  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Argumenten**  
  
-   `arr_expr`  
  
     Is geldige matrixexpressie.  
  
-   `num_expr`  
  
     Is geldige numerieke expressie.  
  
 **Retourtypen**  
  
 Retourneert een Booleaanse waarde.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld het ophalen van een deel van een matrix met ARRAY_SLICE.  
  
```  
SELECT   
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1),  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{  
           "$1": ["strawberries", "bananas"],   
           "$2": ["strawberries"]  
       }]  
```  
  
###  <a name="bk_spatial_functions"></a>Ruimtelijke functies  
 De volgende scalaire functies een bewerking uitvoeren op een invoerwaarde ruimtelijke object en een numeriek gegevenstype of Booleaanse waarde retourneren.  
  
||||  
|-|-|-|  
|[ST_DISTANCE](#bk_st_distance)|[ST_WITHIN](#bk_st_within)|[ST_INTERSECTS](#bk_st_intersects)|[ST_ISVALID](#bk_st_isvalid)|  
|[ST_ISVALIDDETAILED](#bk_st_isvaliddetailed)|||  
  
####  <a name="bk_st_distance"></a>ST_DISTANCE  
 Retourneert de afstand tussen de twee GeoJSON punt, Polygon of LineString expressies.  
  
 **Syntaxis**  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumenten**  
  
-   `spatial_expr`  
  
     Is geldige expressie voor GeoJSON punt, Polygon of LineString-object.  
  
 **Retourtypen**  
  
 Retourneert een numerieke expressie met de afstand. Dit wordt uitgedrukt in meters voor de verwijzing van het systeem.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld laat zien hoe alle familie documenten die binnen 30 km van de opgegeven locatie met de ingebouwde functie ST_DISTANCE retourneren. .  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{  
  "id": "WakefieldFamily"  
}]  
```  
  
####  <a name="bk_st_within"></a>ST_WITHIN  
 Retourneert een Booleaanse expressie die aangeeft of het GeoJSON-object opgegeven in het eerste argument (punt, Polygon of LineString) binnen het GeoJSON (punt, Polygon of LineString) in het tweede argument.  
  
 **Syntaxis**  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumenten**  
  
-   `spatial_expr`  
  
     Is geldige expressie voor GeoJSON punt, Polygon of LineString-object.  
 
-   `spatial_expr`  
  
     Is geldige expressie voor GeoJSON punt, Polygon of LineString-object.  
  
 **Retourtypen**  
  
 Retourneert een Booleaanse waarde.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld laat zien hoe alle familie documenten binnen een veelhoek met ST_WITHIN vinden.  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{ "id": "WakefieldFamily" }]  
```  

####  <a name="bk_st_intersects"></a>ST_INTERSECTS  
 Retourneert een Booleaanse expressie die aangeeft of het GeoJSON-object opgegeven in het eerste argument (punt, Polygon of LineString) in de polygoonring GeoJSON (punt, Polygon of LineString) in het tweede argument.  
  
 **Syntaxis**  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumenten**  
  
-   `spatial_expr`  
  
     Is geldige expressie voor GeoJSON punt, Polygon of LineString-object.  
 
-   `spatial_expr`  
  
     Is geldige expressie voor GeoJSON punt, Polygon of LineString-object.  
  
 **Retourtypen**  
  
 Retourneert een Booleaanse waarde.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld laat zien hoe vinden alle gebieden die in de polygoonring met de opgegeven veelhoek.  
  
```  
SELECT a.id   
FROM Areas a   
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{ "id": "IntersectingPolygon" }]  
```  
  
####  <a name="bk_st_isvalid"></a>ST_ISVALID  
 Retourneert een Booleaanse waarde die aangeeft of de opgegeven expressie voor GeoJSON punt, Polygon of LineString ongeldig is.  
  
 **Syntaxis**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Argumenten**  
  
-   `spatial_expr`  
  
     Is geldige GeoJSON punt, Polygon of LineString-expressie.  
  
 **Retourtypen**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld laat zien hoe controleren of een punt ST_VALID geldig is.  
  
 Dit punt heeft bijvoorbeeld een Breedtegraadwaarde die niet in het geldige waardenbereik [-90, 90], dus de query retourneert false.  
  
 Voor multilinestring vereist de specificatie GeoJSON dat de opgegeven combinatie van laatste coördinaat moet hetzelfde zijn als de eerste pagina, voor het maken van een gesloten vorm. Punten in een polygoon moeten worden opgegeven in rechtsom volgorde. Een polygoon die is opgegeven in rechtsom volgorde vertegenwoordigt de inverse van de regio binnen deze.  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{ "$1": false }]  
```  
  
####  <a name="bk_st_isvaliddetailed"></a>ST_ISVALIDDETAILED  
 Retourneert een JSON-waarde met een Booleaanse waarde als de opgegeven expressie voor GeoJSON punt, Polygon of LineString is geldig, en als ongeldig, ook de reden als een string-waarde.  
  
 **Syntaxis**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Argumenten**  
  
-   `spatial_expr`  
  
     Is geldige GeoJSON-punt of de veelhoek expressie.  
  
 **Retourtypen**  
  
 Retourneert een JSON-waarde met een Booleaanse waarde als de opgegeven GeoJSON-punt of de veelhoek expressie is geldig, en als ongeldig, ook de reden als een string-waarde.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld geldigheid (met details) met behulp van ST_ISVALIDDETAILED controleren.  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
})  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{  
  "$1": {   
    "valid": false,   
    "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points."   
  }  
}]  
```  
  
## <a name="next-steps"></a>Volgende stappen  
 [SQL-syntaxis en SQL-query voor Azure Cosmos-DB](sql-api-sql-query.md)   
 [Azure DB Cosmos-documentatie](https://docs.microsoft.com/azure/cosmos-db/)  
  
  
