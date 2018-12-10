---
title: SQL-syntaxis in Azure Cosmos DB
description: In dit artikel wordt uitgelegd dat de SQL-query-syntaxis in Azure Cosmos DB, andere operatoren en trefwoorden die beschikbaar zijn in deze taal gebruikt.
author: LalithaMV
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.topic: reference
ms.date: 12/07/2018
ms.author: laviswa
ms.custom: seodec18
ms.openlocfilehash: eec3846319a93e94ca362d9ef6815a73d0ca958a
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53142560"
---
# <a name="azure-cosmos-db-sql-language-reference"></a>Naslaginformatie voor Azure Cosmos DB SQL-taal 

Azure Cosmos DB ondersteunt het opvragen van documenten met behulp van een vertrouwde SQL (Structured Query Language) zoals grammatica via hiërarchische JSON-documenten zonder een expliciet schema of secundaire indexen worden gemaakt. Dit artikel bevat documentatie voor de SQL-query language-syntaxis, die compatibel met SQL API-accounts is. Zie voor een overzicht van voorbeeld van de SQL-query's, [SQL-query's in Cosmos DB](how-to-sql-query.md).  
  
Ga naar de [testomgeving voor Query's](https://www.documentdb.com/sql/demo) kunt u Cosmos DB uitproberen en SQL-query's uitvoeren in de gegevensset.  
  
## <a name="select-query"></a>SELECT-query  
Elke query bestaat uit een SELECT-component en optionele FROM- en WHERE-componenten conform ANSI-SQL-standaarden. Normaal gesproken wordt de bron in de FROM-component voor elke query geïnventariseerd. Het filter in de component WHERE wordt vervolgens toegepast op de bron om op te halen van een subset van JSON-documenten. Ten slotte wordt de SELECT-component gebruikt om de vereiste JSON-waarden in de SELECT-lijst te projecten. De conventies voor het beschrijven van de SELECT-instructies zijn in de sectie syntaxis conventies tabelindeling. Zie voor voorbeelden van [SELECT-query-voorbeelden](how-to-sql-query.md#SelectClause)
  
**Syntaxis**  
  
```sql
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
  
De clausules in de SELECT-instructie moeten worden besteld, zoals hierboven. Een van de optionele componenten kan worden weggelaten. Maar als de optionele componenten zijn gebruikt, moeten ze worden weergegeven in de juiste volgorde.  
  
### <a name="logical-processing-order-of-the-select-statement"></a>Logische verwerkingsvolgorde van de SELECT-instructie  
  
De volgorde waarin de EU worden verwerkt, is:  

1.  [FROM-component](#bk_from_clause)  
2.  [WHERE-component](#bk_where_clause)  
3.  [ORDER BY-component](#bk_orderby_clause)  
4.  [SELECT-component](#bk_select_query)  

Houd er rekening mee dat dit wijkt af van de volgorde waarin ze worden weergegeven in de syntaxis. De volgorde is dat alle nieuwe symbolen die door een verwerkte component zichtbaar zijn en kunnen worden gebruikt in de componenten die later worden verwerkt. Bijvoorbeeld, aliassen gedeclareerd in een FROM-component in, waarbij toegankelijk zijn en SELECT-component.  

### <a name="whitespace-characters-and-comments"></a>Spaties bevatten en opmerkingen  

Alleen spaties bevatten die geen deel uitmaken van een tekenreeks tussen aanhalingstekens of tussen aanhalingstekens id maken geen deel uit van de taal-grammatica en worden genegeerd tijdens het parseren.  

De querytaal biedt ondersteuning voor T-SQL-stijl opmerkingen, zoals  

-   SQL-instructie `-- comment text [newline]`  

Terwijl uit witruimte bestaat en opmerkingen niet een betekenis in de grammatica hebt, moeten ze worden gebruikt voor het scheiden van tokens. Bijvoorbeeld: `-1e5` is één getal token, even`: – 1 e5` wordt-token van een minteken, gevolgd door nummer 1 en id e5.  

##  <a name="bk_select_query"></a> SELECT-component  
De clausules in de SELECT-instructie moeten worden besteld, zoals hierboven. Een van de optionele componenten kan worden weggelaten. Maar als de optionele componenten zijn gebruikt, moeten ze worden weergegeven in de juiste volgorde. Zie voor voorbeelden van [SELECT-queryvoorbeelden](how-to-sql-query.md#SelectClause).

**Syntaxis**  

```sql
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | <object_property_list>   
      | VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
 **Argumenten**  
  
- `<select_specification>`  

  De eigenschappen of de waarde die moet worden geselecteerd voor de resultatenset.  
  
- `'*'`  

  Hiermee geeft u op dat de waarde zonder wijzigingen moet worden opgehaald. Specifiek als de verwerkte waarde een object is, worden alle eigenschappen worden opgehaald.  
  
- `<object_property_list>`  
  
  Hiermee geeft u de lijst met eigenschappen die moeten worden opgehaald. Elke geretourneerde waarde is een object met de eigenschappen die zijn opgegeven.  
  
- `VALUE`  

  Hiermee geeft u op dat de JSON-waarde moet worden opgehaald in plaats van de volledige JSON-object. Dit, in tegenstelling tot `<property_list>` loopt niet de verwachte waarde in een object.  
  
- `<scalar_expression>`  

  Expressie voor de waarde die moet worden berekend. Zie [scalaire expressies](#bk_scalar_expressions) sectie voor meer informatie.  
  
**Opmerkingen**  
  
De `SELECT *` syntaxis is alleen geldig als FROM-component precies één alias is gedeclareerd. `SELECT *` biedt een identity-projectie handig is als er geen projectie is vereist. Selecteer * is alleen geldig als FROM-component is opgegeven en slechts één invoer bron geïntroduceerd.  
  
Beide `SELECT <select_list>` en `SELECT *` 'syntactische suiker' zijn en kan ook worden uitgedrukt met behulp van eenvoudige SELECT-instructies zoals hieronder wordt weergegeven.  
  
1. `SELECT * FROM ... AS from_alias ...`  
  
   is gelijk aan:  
  
   `SELECT from_alias FROM ... AS from_alias ...`  
  
2. `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
   is gelijk aan:  
  
   `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
**Zie ook**  
  
[Scalaire expressies](#bk_scalar_expressions)  
[SELECT-component](#bk_select_query)  
  
##  <a name="bk_from_clause"></a> FROM-component  
Hiermee geeft u de bron- of gekoppelde gegevensbronnen. De component FROM is optioneel, tenzij de bron is gefilterd of verderop in de query verwachte. Het doel van deze component is om op te geven van de gegevensbron waarop de query moet functioneren. De hele container is vaak de bron, maar één een subset van de container in plaats daarvan kunt opgeven. Als deze component niet opgegeven is, wordt nog steeds andere componenten uitgevoerd alsof de FROM-component die één document. Zie voor voorbeelden van [uit component-voorbeelden](how-to-sql-query.md#FromClause)
  
**Syntaxis**  
  
```sql  
FROM <from_specification>  
  
<from_specification> ::=   
        <from_source> {[ JOIN <from_source>][,...n]}  
  
<from_source> ::=   
          <container_expression> [[AS] input_alias]  
        | input_alias IN <container_expression>  
  
<container_expression> ::=   
        ROOT   
     | container_name  
     | input_alias  
     | <container_expression> '.' property_name  
     | <container_expression> '[' "property_name" | array_index ']'  
```  
  
**Argumenten**  
  
- `<from_source>`  
  
  Hiermee geeft u een gegevensbron, met of zonder een alias. Als alias niet opgegeven is, wordt deze worden afgeleid van de `<container_expression>` van de volgende regels:  
  
  -  De expressie is een container_name, zal container_name worden gebruikt als een alias.  
  
  -  Als de expressie `<container_expression>`, en vervolgens %{Property_Name/ en vervolgens %{Property_Name/ wordt gebruikt als een alias. De expressie is een container_name, zal container_name worden gebruikt als een alias.  
  
- AS `input_alias`  
  
  Hiermee wordt aangegeven dat de `input_alias` is een set waarden die zijn geretourneerd door de onderliggende container-expressie.  
 
- `input_alias` IN  
  
  Hiermee wordt aangegeven dat de `input_alias` de set met waarden die zijn verkregen met iteratie van alle matrixelementen van elk matrix geretourneerd door de onderliggende container-expressie moet vertegenwoordigen. Een waarde die is geretourneerd door onderliggende container-expressie die is geen matrix wordt genegeerd.  
  
- `<container_expression>`  
  
  Hiermee geeft u de container-expressie moet worden gebruikt om de documenten te halen.  
  
- `ROOT`  
  
  Hiermee geeft u op dat document moet worden opgehaald van de standaardwaarde, momenteel verbonden container.  
  
- `container_name`  
  
  Hiermee geeft u op dat document moet worden opgehaald uit de opgegeven container. De naam van de container moet overeenkomen met de naam van de container momenteel verbonden.  
  
- `input_alias`  
  
  Hiermee geeft u op dat document moet worden opgehaald uit de andere bron die wordt gedefinieerd door de opgegeven alias.  
  
- `<container_expression> '.' property_`  
  
  Hiermee geeft u het document moet worden opgehaald door het openen van de `property_name` eigenschap of matrixindex matrixelement voor alle documenten die zijn opgehaald door bits-container expressie opgegeven.  
  
- `<container_expression> '[' "property_name" | array_index ']'`  
  
  Hiermee geeft u het document moet worden opgehaald door het openen van de `property_name` eigenschap of matrixindex matrixelement voor alle documenten die zijn opgehaald door bits-container expressie opgegeven.  
  
**Opmerkingen**  
  
Alle aliassen opgegeven of afgeleid in de `<from_source>(`s) moet uniek zijn. De syntaxis van de `<container_expression>.`%{Property_Name/ is hetzelfde als `<container_expression>' ['"property_name"']'`. De syntaxis van de laatste kan echter worden gebruikt als een eigenschapsnaam een niet-id-teken bevat.  
  
### <a name="handling-missing-properties-missing-array-elements-and-undefined-values"></a>afhandeling van ontbrekende eigenschappen, ontbrekende matrixelementen en niet-gedefinieerde waarden
  
Als een expressie voor een container toegang heeft tot de eigenschappen of matrixelementen en waarde niet bestaat, wordt die waarde genegeerd en niet verder wordt verwerkt.  
  
### <a name="container-expression-context-scoping"></a>Container expressie context scoping  
  
Een expressie voor een container zijn binnen het bereik van container of binnen het bereik van document:  
  
-   Een expressie is container binnen het bereik, als de onderliggende bron van de container-expressie de hoofdmap is of `container_name`. Deze expressie vertegenwoordigt een set documenten die zijn opgehaald uit de container rechtstreeks en is niet afhankelijk van de verwerking van andere expressies container.  
  
-   Een expressie is document binnen het bereik, als de onderliggende bron van de container-expressie is `input_alias` die eerder in de query is geïntroduceerd. Deze expressie vertegenwoordigt een set documenten die zijn verkregen door het evalueren van de container-expressie in het bereik van elk document die behoren tot de verzameling die zijn gekoppeld aan de alias-container.  De resulterende set is een samenvoeging van die worden verkregen door het evalueren van de container-expressie voor elk van de documenten in de onderliggende verzameling.  
  
### <a name="joins"></a>Samenvoegingen 
  
In de huidige release ondersteunt Cosmos DB inner joins. Lid worden van aanvullende mogelijkheden worden aangebracht. 

Inner joins leiden tot een volledige vectorproduct van de sets die deel uitmaken van de join. Het resultaat van een join N manier is een set met tuples van de N-element, waarbij elke waarde in de tuple is gekoppeld aan de alias instellen die deel uitmaken van de join en kan worden geopend door te verwijzen naar deze alias in andere componenten. Zie voor voorbeelden van [JOIN sleutelwoord voorbeelden](how-to-sql-query.md#Joins)
  
De evaluatie van de join, is afhankelijk van het bereik van de context van de deelnemende sets:  
  
-  Een join tussen container-set A en binnen het bereik van container set B, resulteert in een vectorproduct van alle elementen in sets A en B.
  
-   Een koppeling tussen een set en binnen het bereik van document set B, resulteert in een samenvoeging van alle sets die zijn verkregen door het evalueren van binnen het bereik van document set B voor elk document van A. instellen  
  
 In de huidige release, wordt maximaal één binnen het bereik van container-expressie ondersteund door de queryprocessor.  
  
### <a name="examples-of-joins"></a>Voorbeelden van joins  
  
Bekijk de volgende FROM-component: `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`  
  
 Elke bron definiëren, kunnen `input_alias1, input_alias2, …, input_aliasN`. Deze component FROM retourneert een set met N-tuples (tuple met N-waarden). Elke tuple heeft waarden die worden geproduceerd door alle containeraliassen te herhalen voor hun respectieve sets.  
  
**Voorbeeld 1** -2 bronnen  
  
- Laat `<from_source1>` container bereik en set {A, B, C} vertegenwoordigen.  
  
- Laat `<from_source2>` worden document binnen het bereik van verwijst naar een input_alias1 en sets vertegenwoordigen:  
  
    {1, 2} voor `input_alias1 = A,`  
  
    {3} voor `input_alias1 = B,`  
  
    {4, 5} voor `input_alias1 = C,`  
  
- De component FROM `<from_source1> JOIN <from_source2>` resulteert in de volgende tuples:  
  
    (`input_alias1, input_alias2`):  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
**Voorbeeld 2** -3-bronnen  
  
- Laat `<from_source1>` container bereik en set {A, B, C} vertegenwoordigen.  
  
- Laat `<from_source2>` verwijst naar een document binnen het bereik worden `input_alias1` en sets vertegenwoordigen:  
  
    {1, 2} voor `input_alias1 = A,`  
  
    {3} voor `input_alias1 = B,`  
  
    {4, 5} voor `input_alias1 = C,`  
  
- Laat `<from_source3>` verwijst naar een document binnen het bereik worden `input_alias2` en sets vertegenwoordigen:  
  
    {100, 200} voor `input_alias2 = 1,`  
  
    {300} voor `input_alias2 = 3,`  
  
- De component FROM `<from_source1> JOIN <from_source2> JOIN <from_source3>` resulteert in de volgende tuples:  
  
    (input_alias1, input_alias2, input_alias3):  
  
    (A, 1, 100), (A, 1, 200), (B, 3, 300)  
  
  > [!NOTE]
  > Gebrek aan tuples voor andere waarden van `input_alias1`, `input_alias2`, waarvoor de `<from_source3>` heeft geen waarden geretourneerd.  
  
**Voorbeeld 3** -3-bronnen  
  
- Laat < from_source1 > worden binnen het bereik van container en set {A, B, C} vertegenwoordigen.  
  
- Laat `<from_source1>` container bereik en set {A, B, C} vertegenwoordigen.  
  
- < From_source2 > worden verwijzende input_alias1 document binnen het bereik en sets vertegenwoordigen, kunnen:  
  
    {1, 2} voor `input_alias1 = A,`  
  
    {3} voor `input_alias1 = B,`  
  
    {4, 5} voor `input_alias1 = C,`  
  
- Laat `<from_source3>` worden afgestemd op `input_alias1` en sets vertegenwoordigen:  
  
    {100, 200} voor `input_alias2 = A,`  
  
    {300} voor `input_alias2 = C,`  
  
- De component FROM `<from_source1> JOIN <from_source2> JOIN <from_source3>` resulteert in de volgende tuples:  
  
    (`input_alias1, input_alias2, input_alias3`):  
  
    (A, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200), C, 4, 300, (C, 5, 300)  
  
  > [!NOTE]
  > Dit heeft geresulteerd in vectorproduct tussen `<from_source2>` en `<from_source3>` omdat beide zijn gericht op hetzelfde `<from_source1>`.  Dit leidde tot 4 (2 x 2) tuples die waarde A of 0 tuples waarde B (1, 0) en 2 (2 x 1) tuples waarde C.  
  
**Zie ook**  
  
 [SELECT-component](#bk_select_query)  
  
##  <a name="bk_where_clause"></a> WHERE-component  
 Hiermee geeft u de zoekvoorwaarde voor de documenten die zijn geretourneerd door de query. Zie voor voorbeelden van [WHERE-component voorbeelden](how-to-sql-query.md#WhereClause)
  
 **Syntaxis**  
  
```sql  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
 **Argumenten**  
  
-   `<filter_condition>`  
  
     Hiermee geeft u de voorwaarde moet worden voldaan voor de documenten die moeten worden geretourneerd.  
  
-   `<scalar_expression>`  
  
     Expressie voor de waarde die moet worden berekend. Zie de [scalaire expressies](#bk_scalar_expressions) sectie voor meer informatie.  
  
 **Opmerkingen**  
  
 Opdat het document dat moet worden geretourneerd van een expressie die is opgegeven als filter moet voorwaarde resulteren in waar. Alleen Booleaanse waarde ' True ', voldoen aan de voorwaarde, een andere waarde: niet-gedefinieerde, null, ingesteld op false, getal, matrix of Object zal voldoen niet aan de voorwaarde.  
  
##  <a name="bk_orderby_clause"></a> ORDER BY-component  
 Hiermee geeft u de sorteervolgorde voor de resultaten geretourneerd door de query. Zie voor voorbeelden van [ORDER BY-component-voorbeelden](how-to-sql-query.md#OrderByClause)
  
 **Syntaxis**  
  
```sql  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= <scalar_expression> [ASC | DESC]  
  
```  
  
 **Argumenten**  
  
-   `<sort_specification>`  
  
     Hiermee geeft u een eigenschap of een expressie waarop u wilt sorteren van de queryresultaatset. Een sorteerkolom kan worden opgegeven als een alias voor de naam of kolom.  
  
     Meerdere kolommen voor sorteren kunnen worden opgegeven. Kolomnamen moeten uniek zijn. De volgorde van de kolommen sorteren in de component ORDER BY definieert de organisatie van de gesorteerde resultatenset. Dat wil zeggen, de resultatenset is gesorteerd op de eerste eigenschap en vervolgens die geordende lijst is gesorteerd op de tweede eigenschap, enzovoort.  
  
     De namen van de kolommen waarnaar wordt verwezen in de component ORDER BY moeten overeenkomen met een kolom in de lijst selecteren of een kolom die is gedefinieerd in een tabel die is opgegeven in de component FROM zonder eventuele dubbelzinnigheden.  
  
-   `<sort_expression>`  
  
     Hiermee geeft u een één eigenschap of een expressie waarop u wilt sorteren van de queryresultaatset.  
  
-   `<scalar_expression>`  
  
     Zie de [scalaire expressies](#bk_scalar_expressions) sectie voor meer informatie.  
  
-   `ASC | DESC`  
  
     Hiermee geeft u op dat de waarden in de opgegeven kolom moeten worden gesorteerd in oplopende of aflopende volgorde. ASC sorteren van de laagste waarde naar hoogste waarde. DESC sorteren van hoogste waarde naar laagste waarde. ASC is de standaardsorteervolgorde. Null-waarden worden behandeld als de laagste mogelijke waarden.  
  
 **Opmerkingen**  
  
 Terwijl de querygrammatica biedt ondersteuning voor meerdere order met eigenschappen, ondersteunt de Cosmos DB-query-runtime sorteren alleen tegen één eigenschap, en alleen de namen van eigenschappen (niet tegen berekende eigenschappen). Sorteren is ook vereist dat het indexeringsbeleid een index van het bereik voor de eigenschap en het opgegeven type, met de maximale precisie bevat. Raadpleeg de indexering beleid-documentatie voor meer informatie.  
  
##  <a name="bk_scalar_expressions"></a> Scalaire expressies  
 Een scalaire expressie die is een combinatie van tekens en operators die kunnen worden geëvalueerd om te verkrijgen van een enkele waarde. Eenvoudige expressies mag bestaan uit constanten, verwijzen naar eigenschappen, matrix-element verwijst naar, alias-verwijzingen of functieaanroepen. Eenvoudige expressies kunnen worden gecombineerd tot complexe expressies operators gebruiken. Zie voor voorbeelden van [scalaire expressies voorbeelden](how-to-sql-query.md#scalar-expressions)
  
 Zie voor meer informatie op basis van waarden dat scalaire expressie die u mogelijk [constanten](#bk_constants) sectie.  
  
 **Syntaxis**  
  
```sql  
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
  
     Hiermee geeft u een constante waarde. Zie [constanten](#bk_constants) sectie voor meer informatie.  
  
-   `input_alias`  
  
     Hiermee geeft u een waarde die is gedefinieerd door de `input_alias` die is geïntroduceerd in de `FROM` component.  
    Deze waarde kan niet worden gegarandeerd **niet-gedefinieerde** –**niet-gedefinieerde** waarden in de invoer worden overgeslagen.  
  
-   `<scalar_expression>.property_name`  
  
     Hiermee geeft u een waarde van de eigenschap van een object. Als de eigenschap niet bestaat of eigenschap van een waarde die niet een object wordt verwezen, wordt de expressie wordt geëvalueerd als **niet-gedefinieerde** waarde.  
  
-   `<scalar_expression>'['"property_name"|array_index']'`  
  
     Hiermee geeft u een waarde van de eigenschap met de naam `property_name` of matrixelement met index `array_index` van een object/matrix. Als de eigenschap/matrixindex niet bestaat of index van de eigenschap/array wordt verwezen op een waarde die is geen object/matrix, wordt de expressie wordt geëvalueerd als niet-gedefinieerde waarde.  
  
-   `unary_operator <scalar_expression>`  
  
     Hiermee geeft u een operator die wordt toegepast op een enkele waarde. Zie [Operators](#bk_operators) sectie voor meer informatie.  
  
-   `<scalar_expression> binary_operator <scalar_expression>`  
  
     Een operator die wordt toegepast op de twee waarden vertegenwoordigt. Zie [Operators](#bk_operators) sectie voor meer informatie.  
  
-   `<scalar_function_expression>`  
  
     Hiermee geeft u een waarde die is gedefinieerd door een resultaat van een aanroep van de functie.  
  
-   `udf_scalar_function`  
  
     De naam van de gebruiker gedefinieerde scalaire functie.  
  
-   `builtin_scalar_function`  
  
     Naam van de ingebouwde scalaire functie.  
  
-   `<create_object_expression>`  
  
     Hiermee geeft u een waarde die is verkregen door het maken van een nieuw object met de opgegeven eigenschappen en hun waarden.  
  
-   `<create_array_expression>`  
  
     Hiermee geeft u een waarde die is verkregen door het maken van een nieuwe matrix met de opgegeven waarden als elementen  
  
-   `parameter_name`  
  
     Vertegenwoordigt een waarde van de opgegeven parameternaam. Namen van parameters ze beschikken over één \@ als het eerste teken.  
  
 **Opmerkingen**  
  
 Bij het aanroepen van een ingebouwde of door de gebruiker gedefinieerde scalaire functie kan alle argumenten moeten worden gedefinieerd. Als een van de argumenten niet is gedefinieerd, wordt de functie niet wordt aangeroepen en wordt het resultaat is niet gedefinieerd.  
  
 Bij het maken van een object, wordt elke eigenschap die niet-gedefinieerde waarde is toegewezen overgeslagen en niet opgenomen in het gemaakte object.  
  
 Wanneer het maken van een matrix, een elementwaarde die is toegewezen **niet-gedefinieerde** waarde wordt overgeslagen en niet opgenomen in het gemaakte object. Dit zorgt ervoor dat de volgende gedefinieerde element moet de plaatsvinden zodanig dat de gemaakte matrix wordt niet hebt overgeslagen indexen.  
  
##  <a name="bk_operators"></a> Operators  
 Deze sectie beschrijft de ondersteunde operators. Elke gebruiker kan worden toegewezen aan één categorie.  
  
 Zie **Operator categorieën** tabel hieronder voor meer informatie met betrekking tot de verwerking van **niet-gedefinieerde** waarden, vereisten voor gegevenstypen voor invoerwaarden en verwerking van de waarden met niet overeenkomende typen.  
  
 **Operator categorieën:**  
  
|**Categorie**|**Details**|  
|-|-|  
|**Rekenkundige**|Operator verwacht invoerwaarde(n) (s) zijn. Uitvoer is ook een getal. Als een van de invoer is **niet-gedefinieerde** of type dan het getal klikt u vervolgens het resultaat is **niet-gedefinieerde**.|  
|**Bitsgewijze**|Operator verwacht invoerwaarde(n) ondertekende 32-bits geheel getal (s). Uitvoer is ook 32-bits geheel getal.<br /><br /> Een niet-integer-waarde wordt afgerond. Positieve waarde wordt afgerond naar beneden en negatieve getallen naar boven afgerond.<br /><br /> Een waarde buiten het bereik van de 32-bits geheel getal wordt geconverteerd, door te nemen van de laatste 32-bits van de twee van bits.<br /><br /> Als een van de invoer is **niet-gedefinieerde** of typ dan het getal, dan is het resultaat **niet-gedefinieerde**.<br /><br /> **Opmerking:** het bovenstaande gedrag is compatibel met JavaScript bitsgewijze operator gedrag.|  
|**Logische**|Operator verwacht invoerwaarde(n) Boolean(s) zijn. Uitvoer is ook een Booleaanse waarde.<br />Als een van de invoer is **niet-gedefinieerde** of typ dan Booleaanse waarde, wordt het resultaat **niet-gedefinieerde**.|  
|**Vergelijking**|Operator verwacht invoerwaarde(n) hetzelfde type en niet zijn niet gedefinieerd. Uitvoer is een Booleaanse waarde.<br /><br /> Als een van de invoer is **niet-gedefinieerde** of de invoer hebben verschillende typen en vervolgens het resultaat is **niet-gedefinieerde**.<br /><br /> Zie **volgorde van de waarden voor vergelijking** tabel voor details bestellen-waarde.|  
|**Tekenreeks**|Operator verwacht invoerwaarde(n) meer zijn. Uitvoer is ook een tekenreeks.<br />Als een van de invoer is **niet-gedefinieerde** of andere dan tekenreeks typt en vervolgens het resultaat is **niet-gedefinieerde**.|  
  
 **De unitaire operators:**  
  
|**Naam**|**Operator**|**Details**|  
|-|-|-|  
|**Rekenkundige**|+<br /><br /> -|Retourneert de waarde.<br /><br /> Bitsgewijze onderhandeling. De numerieke waarde retourneert genegeerde.|  
|**Bitsgewijze**|~|Aanvulling zijn. Retourneert een aanvulling op van een numerieke waarde.|  
|**Logical**|**NIET**|Negatie. Retourneert genegeerde Booleaanse waarde.|  
  
 **Binaire operators:**  
  
|**Naam**|**Operator**|**Details**|  
|-|-|-|  
|**Rekenkundige**|+<br /><br /> -<br /><br /> *<br /><br /> /<br /><br /> %|Toevoeging.<br /><br /> Aftrekken.<br /><br /> Vermenigvuldigen.<br /><br /> Delen.<br /><br /> Modulatie.|  
|**Bitsgewijze**|&#124;<br /><br /> &<br /><br /> ^<br /><br /> <<<br /><br /> >><br /><br /> >>>|Bitsgewijze OR.<br /><br /> Bitsgewijze AND.<br /><br /> Bitsgewijze XOR.<br /><br /> Verschuiving naar links uit.<br /><br /> Verschuiving naar rechts uit.<br /><br /> Nul opvulling rechts verplaatsen.|  
|**Logische**|**EN**<br /><br /> **OR**|Logische combinatie. Retourneert **waar** als beide argumenten zijn **waar**, retourneert **false** anders.<br /><br /> Logische combinatie. Retourneert **waar** als beide argumenten zijn **waar**, retourneert **false** anders.|  
|**Vergelijking**|**=**<br /><br /> **!=, <>**<br /><br /> **>**<br /><br /> **>=**<br /><br /> **<**<br /><br /> **<=**<br /><br /> **??**|Is gelijk aan. Retourneert **waar** als argumenten gelijk zijn, retourneert **false** anders.<br /><br /> Niet gelijk aan. Retourneert **waar** als argumenten niet gelijk zijn zijn, retourneert **false** anders.<br /><br /> Groter dan. Retourneert **waar** als eerste argument groter dan de tweede waarde is, retourneren **false** anders.<br /><br /> Groter dan of gelijk zijn aan. Retourneert **waar** als eerste argument groter dan of gelijk zijn aan de tweede waarde is, retourneren **false** anders.<br /><br /> Kleiner dan. Retourneert **waar** als eerste argument minder is dan het tweede één rendement **false** anders.<br /><br /> Kleiner dan of gelijk zijn aan. Retourneert **waar** als eerste argument kleiner dan of gelijk zijn aan de tweede waarde, retourneren **false** anders.<br /><br /> Samenvoegen. Het tweede argument geeft als resultaat als het eerste argument is een **niet-gedefinieerde** waarde.|  
|**String**|**&#124;&#124;**|Samenvoeging. Retourneert een samenvoeging van beide argumenten.|  
  
 **Ternair operators:**  
  
|Ternaire operator|?|Het tweede argument geeft als resultaat als het eerste argument wordt geëvalueerd als **waar**; anders wordt het derde argument geretourneerd.|  
|-|-|-|  
  
 **Volgorde van de waarden voor vergelijking**  
  
|**Type**|**Volgorde van waarden**|  
|-|-|  
|**Undefined**|Niet vergelijkbaar.|  
|**Null**|Enkele waarde: **null**|  
|**Number**|Natuurlijke reëel getal.<br /><br /> Negatieve oneindigheid waarde is kleiner dan andere numerieke waarde.<br /><br /> Positieve oneindigheid waarde is groter dan andere numerieke waarde. **NaN** waarde kan niet worden vergeleken. Vergelijken met **NaN** zal leiden tot **niet-gedefinieerde** waarde.|  
|**String**|Lexicographical volgorde.|  
|**Array**|Er is geen bestellen, maar billijke.|  
|**Object**|Er is geen bestellen, maar billijke.|  
  
 **Opmerkingen**  
  
 In Cosmos DB, worden de typen van waarden zijn vaak niet bekend totdat ze zijn opgehaald uit de database. Ter ondersteuning van efficiënte uitvoering van query's, hebben de meeste van de operators strikte eisen. Operators op zichzelf kunnen ook niet uitvoeren voor impliciete conversies.  
  
 Dit betekent dat een query, zoals: Selecteer * uit ROOT r waar r.Age = 21, retourneert alleen documenten waarvoor de eigenschap leeftijd gelijk zijn aan het getal 21. Documenten met de eigenschap leeftijd is gelijk aan de tekenreeks '21' of de tekenreeks '0021' wordt niet overeenkomen, als de expressie "21" = 21 tot niet-gedefinieerde evalueert. Hiermee kunt u een beter gebruik van indexen, omdat het opzoeken van een bepaalde waarde (zoals het aantal 21) is veel sneller dan zoeken naar een onbeperkt aantal potentiële komt overeen met (het nummer 21 of tekenreeksen "21", "021", "21,0"...). Dit wijkt af van de manier waarop JavaScript operators op basis van waarden van verschillende typen evalueert.  
  
 **Matrices en objecten gelijkheid en vergelijking**  
  
 Vergelijking van de matrix of één Object waarden bereik operators gebruiken (>, > =, <, < =) resulteert in een niet-gedefinieerde omdat er geen volgorde gedefinieerd op basis van waarden voor Object of een matrix. Maar met gelijkheid/ongelijkheid operators (=,! =, <>) wordt ondersteund en waarden structureel worden vergeleken.  
  
 Matrices die gelijk is als beide matrices hetzelfde aantal elementen hebben en elementen op die overeenkomt met de posities ook gelijk zijn. Als het vergelijken van een paar van de resultaten van de elementen in een niet-gedefinieerde, is het resultaat van de vergelijking van de matrix is niet gedefinieerd.  
  
 Objecten zijn gelijk als beide objecten hebben dezelfde eigenschappen die zijn gedefinieerd, en als de waarden van eigenschappen die overeenkomen met ook gelijk zijn. Als het vergelijken van een paar van de resultaten van de eigenschap waarden in een niet-gedefinieerde, is het resultaat van de vergelijking object is niet gedefinieerd.  
  
##  <a name="bk_constants"></a> Constanten  
 Een constante, ook wel bekend als een letterlijke tekenreeks of een scalaire waarde is een symbool waarmee een specifieke waarde vertegenwoordigt. De indeling van een constante is afhankelijk van het gegevenstype van de waarde vertegenwoordigt.  
  
 **Ondersteunde scalaire gegevenstypen:**  
  
|**Type**|**Volgorde van waarden**|  
|-|-|  
|**Undefined**|Enkele waarde: **niet gedefinieerd**|  
|**Null**|Enkele waarde: **null**|  
|**Boolean**|Waarden: **false**, **waar**.|  
|**Number**|Een getal met dubbele precisie getal met drijvende komma, IEEE 754 standard.|  
|**String**|Een reeks van nul of meer Unicode-tekens. Tekenreeksen moeten tussen enkele of dubbele aanhalingstekens worden geplaatst.|  
|**Array**|Een reeks van nul of meer elementen. Elk element kan een waarde van elk gegevenstype scalaire, met uitzondering van Undefined zijn.|  
|**Object**|Een niet-geordende set van nul of meer naam/waarde-paren. Naam is een Unicode-tekenreeks, de waarde kan zijn van elk gegevenstype scalaire, behalve **Undefined**.|  
  
 **Syntaxis**  
  
```sql  
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
  
* `<undefined_constant>; undefined`  
  
  Geeft een niet-gedefinieerde waarde van het type niet gedefinieerd.  
  
* `<null_constant>; null`  
  
  Hiermee geeft u **null** waarde van het type **Null**.  
  
* `<boolean_constant>`  
  
  Constante van het type Boolean-waarde vertegenwoordigt.  
  
* `false`  
  
  Hiermee geeft u **false** waarde van het type Booleaanse waarde.  
  
* `true`  
  
  Hiermee geeft u **waar** waarde van het type Booleaanse waarde.  
  
* `<number_constant>`  
  
  Hiermee geeft u een constante zijn.  
  
* `decimal_literal`  
  
  Decimale letterlijke waarden zijn getallen weergegeven met behulp van de decimale notatie of wetenschappelijke notatie.  
  
* `hexadecimal_literal`  
  
  Hexadecimale letterlijke waarden zijn getallen weergegeven met behulp van voorvoegsel '0 x' gevolgd door een of meer hexadecimale cijfers.  
  
* `<string_constant>`  
  
  Hiermee geeft u een constante van het type tekenreeks.  
  
* `string _literal`  
  
  Letterlijke tekenreeks zijn vertegenwoordigd door een reeks van nul of meer Unicode-tekens of escapereeksen Unicode-tekenreeksen. Letterlijke tekenreeks zijn ingesloten in enkele aanhalingstekens (apostrof: ') of dubbele aanhalingstekens (aanhalingsteken: ").  
  
 Volgende escapereeksen zijn toegestaan:  
  
|**Escape-reeks**|**Beschrijving**|**Unicode-teken**|  
|-|-|-|  
|\\'|enkel aanhalingsteken (')|U+0027|  
|\\"|dubbel aanhalingsteken (")|U+0022|  
|\\\|omgekeerde schuine streep (\\)|U + 005C|  
|\\/|schuine streep (/)|U+002F|  
|\b|BACKSPACE|U+0008|  
|\f|formulier-feed|U + 000C|  
|\n|regelinvoer|U + 000A|  
|\r|regelterugloop|U + 000D|  
|\t|tab|U+0009|  
|\uXXXX|Een Unicode-teken wordt gedefinieerd door 4 hexadecimale cijfers.|U + XXXX|  
  
##  <a name="bk_query_perf_guidelines"></a> Richtlijnen voor prestaties van query  
 In de order voor een query efficiënt worden uitgevoerd voor een grote container, moet deze filters die kunnen worden weergegeven via een of meer indexen gebruiken.  
  
 De volgende filters wordt voor het opzoeken van de index beschouwd:  
  
-   Gebruik gelijkheidsoperator (=) met-padexpressie aan een document en een constante zijn.  
  
-   Bereik-operators gebruiken (<, \<=, >, > =) met-padexpressie aan een document en aantal constanten.  
  
-   Document-padexpressie aan staat voor een expressie die een constante pad in de documenten uit de databasecontainer waarnaar wordt verwezen, identificeert.  
  
 **Document-padexpressie aan**  
  
 Document padexpressies expressies zijn waarmee een pad van de eigenschap of matrix indexeerfunctie beoordelaars via een document die afkomstig zijn van de database-container-documenten. Dit pad kan worden gebruikt voor het identificeren van de locatie van de waarden waarnaar wordt verwezen in een filter rechtstreeks in de documenten in de databasecontainer.  
  
 Voor een expressie die moet worden beschouwd als een document padexpressie, moet deze:  
  
1.  De hoofdmap van de container rechtstreeks verwijzen naar.  
  
2.  Verwijzing naar eigenschap of constante matrix indexeerfunctie van sommige padexpressie document  
  
3.  Verwijzen naar een alias dat sommige padexpressie document vertegenwoordigt.  
  
     **Syntaxis van de overeenkomsten**  
  
     De volgende tabel beschrijft de conventies gebruikt om te beschrijven-syntaxis in de volgende SQL-referentie.  
  
    |**Overeenkomst**|**Gebruikt voor**|  
    |-|-|    
    |HOOFDLETTERS|Niet-hoofdlettergevoelige trefwoorden.|  
    |Kleine letters|Hoofdlettergevoelige trefwoorden.|  
    |\<nonterminal >|Niet-eindigende, afzonderlijk gedefinieerd.|  
    |\<nonterminal >:: =|De definitie van de syntaxis van de nonterminal.|  
    |other_terminal|Terminal (token), in de woorden in detail beschreven.|  
    |ID|ID. Hiermee kunnen alleen tekens te volgen: a-z A-Z 0-9 _First teken mag niet een cijfer.|  
    |'tekenreeks'|Tekenreeks tussen aanhalingstekens. Hiermee kunt een geldige tekenreeks. Zie de beschrijving van een string_literal.|  
    |"symbool"|Letterlijke symbool die deel uitmaakt van de syntaxis.|  
    |&#124;(verticale balk)|Alternatieven voor de syntaxis van de items. U kunt slechts één van de items die zijn opgegeven.|  
    |[] /(brackets)|Vierkante haken plaatst u een of meer optionele items.|  
    |[,.. .n]|Geeft aan dat het voorgaande item herhaalde n het aantal keren dat kan worden. De exemplaren worden gescheiden door komma's.|  
    |[.. .n]|Geeft aan dat het voorgaande item herhaalde n het aantal keren dat kan worden. De exemplaren worden gescheiden door spaties.|  
  
##  <a name="bk_built_in_functions"></a> Ingebouwde functies  
 Cosmos DB biedt veel ingebouwde SQL-functies. De categorieën ingebouwde functies worden hieronder vermeld.  
  
|Function|Description|  
|--------------|-----------------|  
|[Wiskundige functies](#bk_mathematical_functions)|Wiskundige functies elke uitvoeren van een berekening, meestal op basis van de invoerwaarden die zijn opgegeven als argumenten en retourneert een numerieke waarde.|  
|[Controle van functies van het type](#bk_type_checking_functions)|Met de typecontrolefuncties kunt u het type van een expressie in SQL-query's controleren.|  
|[Tekenreeksfuncties](#bk_string_functions)|De tekenreeks-functies uitvoeren van een bewerking op een tekenreekswaarde voor invoer en retourneert een tekenreeks, een numerieke of Booleaanse waarde.|  
|[Matrixfuncties](#bk_array_functions)|De matrixfuncties uitvoeren van een bewerking op een matrix invoerwaarde en retourneren numerieke, Booleaanse waarde of Matrixwaarde.|  
|[Ruimtelijke-functies](#bk_spatial_functions)|De ruimtelijke functies uitvoeren van een bewerking op een invoerwaarde ruimtelijke index en een numerieke of Booleaanse waarde retourneren.|  
  
###  <a name="bk_mathematical_functions"></a> Wiskundige functies  
 De volgende functies uitvoeren van een berekening, meestal op basis van de invoerwaarden die zijn opgegeven als argumenten en retourneert een numerieke waarde.  
  
||||  
|-|-|-|  
|[ABS](#bk_abs)|[ACOS](#bk_acos)|[ASIN](#bk_asin)|  
|[ATAN](#bk_atan)|[ATN2](#bk_atn2)|[MAXIMUM](#bk_ceiling)|  
|[COS](#bk_cos)|[COT](#bk_cot)|[DEGREES](#bk_degrees)|  
|[EXP](#bk_exp)|[FLOOR](#bk_floor)|[LOGBOEK](#bk_log)|  
|[LOG10](#bk_log10)|[PI](#bk_pi)|[ENERGIEBEHEER](#bk_power)|  
|[RADIANS](#bk_radians)|[AFRONDEN](#bk_round)|[SIN](#bk_sin)|  
|[SQRT](#bk_sqrt)|[SQUARE](#bk_square)|[SIGN](#bk_sign)|  
|[TAN](#bk_tan)|[TRUNC](#bk_trunc)||  
  
####  <a name="bk_abs"></a> ABS  
 Retourneert de absolute (positieve) waarde van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
ABS (<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
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
  
####  <a name="bk_acos"></a> ACOS  
 Retourneert de hoek, in radialen, waarvan de cosinus de opgegeven numerieke expressie is. Dit wordt ook wel de arccosinus genoemd.  
  
 **Syntaxis**  
  
```  
ACOS(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert de ACOS van-1.  
  
```  
SELECT ACOS(-1)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <a name="bk_asin"></a> ASIN  
 Retourneert de hoek, in radialen, waarvan de sinus de opgegeven numerieke expressie is. Dit wordt ook boogsinus genoemd.  
  
 **Syntaxis**  
  
```  
ASIN(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert de ASIN-1.  
  
```  
SELECT ASIN(-1)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": -1.5707963267948966}]  
```  
  
####  <a name="bk_atan"></a> ATAN  
 Retourneert de hoek, in radialen, waarvan de tangens de opgegeven numerieke expressie is. Dit wordt ook wel de arctangens genoemd.  
  
 **Syntaxis**  
  
```  
ATAN(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert de ATAN van de opgegeven waarde.  
  
```  
SELECT ATAN(-45.01)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": -1.5485826962062663}]  
```  
  
####  <a name="bk_atn2"></a> ATN2  
 Retourneert de hoofdsom van de arctangens van y / x, uitgedrukt in radialen.  
  
 **Syntaxis**  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld berekent de ATN2 voor de opgegeven x- en y onderdelen.  
  
```  
SELECT ATN2(35.175643, 129.44)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": 1.3054517947300646}]  
```  
  
####  <a name="bk_ceiling"></a> MAXIMUM  
 Retourneert het kleinste gehele getal dat groter is dan of gelijk is aan de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
CEILING (<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld toont een positieve numerieke, negatief en nulwaarden met de functie CEILING.  
  
```  
SELECT CEILING(123.45), CEILING(-123.45), CEILING(0.0)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 124, $2: -123, $3: 0}]  
```  
  
####  <a name="bk_cos"></a> COS  
 Retourneert de trigonometrische cosinus van de opgegeven hoek, in radialen, in de opgegeven expressie.  
  
 **Syntaxis**  
  
```  
COS(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
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
  
####  <a name="bk_cot"></a> COT  
 Retourneert de trigonometrische cotangens van de opgegeven hoek, in radialen, in de opgegeven expressie.  
  
 **Syntaxis**  
  
```  
COT(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
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
  
####  <a name="bk_degrees"></a> GRADEN  
 Retourneert de overeenkomende hoek in graden voor een hoek die is opgegeven in radialen.  
  
 **Syntaxis**  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
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
  
####  <a name="bk_floor"></a> FLOOR  
 Retourneert het grootste gehele getal dat kleiner is dan of gelijk is aan de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld toont een positieve numerieke, negatief en nulwaarden met de functie FLOOR.  
  
```  
SELECT FLOOR(123.45), FLOOR(-123.45), FLOOR(0.0)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 123, $2: -124, $3: 0}]  
```  
  
####  <a name="bk_exp"></a> EXP  
 Retourneert de exponentiële waarde van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
EXP (<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie.  
  
 **Opmerkingen**  
  
 De constante **e** (2.718281...) vormt de basis van natuurlijke logaritmen.  
  
 De exponent van een getal is de constante **e** verheven tot de macht van het getal. Bijvoorbeeld, EXP(1.0) = e ^ 1.0 = 2.71828182845905 en EXP(10) = e ^ 10 = 22026.4657948067.  
  
 De exponentiële waarde van de natuurlijke logaritme van een getal is het aantal zelf: EXP (logboek (n)) = n. En de natuurlijke logaritme van de exponentiële van een getal is het aantal zelf: LOG (EXP (n)) = n.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt een variabele gedeclareerd en berekent de exponentiële waarde van de opgegeven variabele (10).  
  
```  
SELECT EXP(10)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 22026.465794806718}]  
```  
  
 Het volgende voorbeeld retourneert de exponentiële waarde van de natural logarithm van 20 en de natuurlijke logaritme van de exponentiële van 20. Omdat deze functies inverse functies van elkaar zijn, is de geretourneerde waarde met de afronding voor zwevende punt math in beide gevallen 20.  
  
```  
SELECT EXP(LOG(20)), LOG(EXP(20))  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 19.999999999999996, $2: 20}]  
```  
  
####  <a name="bk_log"></a> LOGBOEK  
 Retourneert de natuurlijke logaritme van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
-   `base`  
  
     Optionele numerieke argument dat Hiermee stelt u de basis voor de logaritme.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie.  
  
 **Opmerkingen**  
  
 Standaard retourneert LOG() de natuurlijke logaritme. U kunt het grondtal van de logaritme met een andere waarde wijzigen met behulp van de optionele parameter basis.  
  
 De natuurlijke logaritme is de logaritme voor de base **e**, waarbij **e** is een constante irrational ongeveer gelijk zijn aan 2.718281828.  
  
 De natuurlijke logaritme van de exponentiële van een getal is het aantal zelf: LOG (EXP (n)) = n. En de exponentiële waarde van de natuurlijke logaritme van een getal is het aantal zelf: EXP (logboek (n)) = n.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt een variabele gedeclareerd en retourneert de logaritme-waarde van de opgegeven variabele (10).  
  
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
  
####  <a name="bk_log10"></a> LOG10  
 Retourneert de logaritme met grondtal 10 van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie.  
  
 **Opmerkingen**  
  
 De functies LOG10 en POWER zijn omgekeerd met elkaar gerelateerd. Bijvoorbeeld, 10 ^ LOG10(n) = n.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt een variabele gedeclareerd en retourneert de waarde LOG10 van de opgegeven variabele (100).  
  
```  
SELECT LOG10(100)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 2}]  
```  
  
####  <a name="bk_pi"></a> PI  
 Retourneert de constante waarde van PI.  
  
 **Syntaxis**  
  
```  
PI ()  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
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
  
####  <a name="bk_power"></a> ENERGIEBEHEER  
 Retourneert de waarde van de opgegeven expressie voor de opgegeven macht.  
  
 **Syntaxis**  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
-   `y`  
  
     Is de kracht waarnaar u wilt verhogen `numeric_expression`.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld ziet u een getal tot de macht 3 (de kubus van het getal) verhogen.  
  
```  
SELECT POWER(2, 3), POWER(2.5, 3)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 8, $2: 15.625}]  
```  
  
####  <a name="bk_radians"></a> RADIALEN  
 Retourneert radialen als een numerieke expressie in graden wordt ingevoerd.  
  
 **Syntaxis**  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld duurt een paar hoeken als invoer en retourneert de overeenkomstige waarden ervan Radiaal.  
  
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
  
####  <a name="bk_round"></a> AFRONDEN  
 Retourneert een numerieke waarde, afgerond naar het dichtstbijzijnde gehele getal.  
  
 **Syntaxis**  
  
```  
ROUND(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
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
  
####  <a name="bk_sign"></a> MELD U  
 Retourneert de positief (+ 1), nul (0) of minteken (-1) van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
SIGN(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert de waarden van de aanmelding van de getallen van -2 naar 2.  
  
```  
SELECT SIGN(-2), SIGN(-1), SIGN(0), SIGN(1), SIGN(2)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: -1, $2: -1, $3: 0, $4: 1, $5: 1}]  
```  
  
####  <a name="bk_sin"></a> SIN  
 Retourneert de trigonometrische sinus van de opgegeven hoek, in radialen, in de opgegeven expressie.  
  
 **Syntaxis**  
  
```  
SIN(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
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
  
####  <a name="bk_sqrt"></a> WORTEL  
 Retourneert de vierkantswortel van de opgegeven numerieke waarde.  
  
 **Syntaxis**  
  
```  
SQRT(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert de vierkantswortel van getallen 1-3.  
  
```  
SELECT SQRT(1), SQRT(2.0), SQRT(3)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{$1: 1, $2: 1.4142135623730952, $3: 1.7320508075688772}]  
```  
  
####  <a name="bk_square"></a> VIERKANT  
 Retourneert het kwadraat van de opgegeven numerieke waarde.  
  
 **Syntaxis**  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
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
  
####  <a name="bk_tan"></a> TAN  
 Retourneert de tangens van de opgegeven hoek in radialen in de opgegeven expressie.  
  
 **Syntaxis**  
  
```  
TAN (<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld berekent de tangens van PI () / 2.  
  
```  
SELECT TAN(PI()/2);  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": 16331239353195370 }]  
```  
  
####  <a name="bk_trunc"></a> GEHEEL  
 Retourneert een numerieke waarde, afgekapt tot het dichtstbijzijnde gehele getal.  
  
 **Syntaxis**  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 **Argumenten**  
  
-   `numeric_expression`  
  
     Een numerieke expressie is.  
  
 **Typen retourneren**  
  
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
  
###  <a name="bk_type_checking_functions"></a> Controle van functies van het type  
 De volgende functies ondersteunen controleren op basis van de invoerwaarden van het type en elke een Booleaanse waarde retourneren.  
  
||||  
|-|-|-|  
|[IS_ARRAY](#bk_is_array)|[IS_BOOL](#bk_is_bool)|[IS_DEFINED](#bk_is_defined)|  
|[IS_NULL](#bk_is_null)|[IS_NUMBER](#bk_is_number)|[IS_OBJECT](#bk_is_object)|  
|[IS_PRIMITIVE](#bk_is_primitive)|[IS_STRING](#bk_is_string)||  
  
####  <a name="bk_is_array"></a> IS_ARRAY  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een matrix is.  
  
 **Syntaxis**  
  
```  
IS_ARRAY(<expression>)  
```  
  
 **Argumenten**  
  
-   `expression`  
  
     Een geldige expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_ARRAY.  
  
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
  
####  <a name="bk_is_bool"></a> IS_BOOL  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een Booleaanse waarde is.  
  
 **Syntaxis**  
  
```  
IS_BOOL(<expression>)  
```  
  
 **Argumenten**  
  
-   `expression`  
  
     Een geldige expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_BOOL.  
  
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
  
####  <a name="bk_is_defined"></a> IS_DEFINED  
 Retourneert een Booleaanse waarde die aangeeft of aan de eigenschap een waarde is toegewezen.  
  
 **Syntaxis**  
  
```  
IS_DEFINED(<expression>)  
```  
  
 **Argumenten**  
  
-   `expression`  
  
     Een geldige expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt gecontroleerd op de aanwezigheid van een eigenschap binnen het opgegeven JSON-document. De eerste retourneert ' True ', omdat "a" aanwezig is, maar de tweede ' false ' retourneert omdat "b" afwezig is.  
  
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
  
####  <a name="bk_is_null"></a> IS_NULL  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie null is.  
  
 **Syntaxis**  
  
```  
IS_NULL(<expression>)  
```  
  
 **Argumenten**  
  
-   `expression`  
  
     Een geldige expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_NULL.  
  
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
  
####  <a name="bk_is_number"></a> IS_NUMBER  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een getal is.  
  
 **Syntaxis**  
  
```  
IS_NUMBER(<expression>)  
```  
  
 **Argumenten**  
  
-   `expression`  
  
     Een geldige expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_NULL.  
  
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
  
####  <a name="bk_is_object"></a> IS_OBJECT  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een JSON-object is.  
  
 **Syntaxis**  
  
```  
IS_OBJECT(<expression>)  
```  
  
 **Argumenten**  
  
-   `expression`  
  
     Een geldige expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_OBJECT.  
  
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
  
####  <a name="bk_is_primitive"></a> IS_PRIMITIVE  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een primitieve nemen is (string, Boolean, numerieke of null).  
  
 **Syntaxis**  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 **Argumenten**  
  
-   `expression`  
  
     Een geldige expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_PRIMITIVE.  
  
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
  
####  <a name="bk_is_string"></a> IS_STRING  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een tekenreeks is.  
  
 **Syntaxis**  
  
```  
IS_STRING(<expression>)  
```  
  
 **Argumenten**  
  
-   `expression`  
  
     Een geldige expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_STRING.  
  
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
  
###  <a name="bk_string_functions"></a> Tekenreeksfuncties  
 Met de volgende scalaire functies wordt een bewerking op een tekenreeksinvoerwaarde uitgevoerd en een tekenreeks, numerieke waarde of Booleaanse waarde geretourneerd.  
  
||||  
|-|-|-|  
|[CONCAT](#bk_concat)|[BEVAT](#bk_contains)|[ENDSWITH](#bk_endswith)|  
|[INDEX_OF](#bk_index_of)|[LEFT](#bk_left)|[LENGTE](#bk_length)|  
|[LAGERE](#bk_lower)|[LTRIM](#bk_ltrim)|[REPLACE](#bk_replace)|  
|[REPLICEREN](#bk_replicate)|[REVERSE](#bk_reverse)|[RIGHT](#bk_right)|  
|[RTRIM](#bk_rtrim)|[STARTSWITH](#bk_startswith)|[DE SUBTEKENREEKS](#bk_substring)|  
|[ToString](#bk_tostring)|[TRIM](#bk_trim)|[BOVENSTE](#bk_upper)||| 
  
####  <a name="bk_concat"></a> CONCAT  
 Retourneert een tekenreeks die het resultaat is van het samenvoegen van twee of meer tekenreekswaarden.  
  
 **Syntaxis**  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
 **Typen retourneren**  
  
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
  
####  <a name="bk_contains"></a> BEVAT  
 Retourneert een Booleaanse waarde die aangeeft of de eerste tekenreeksexpressie de tweede bevat.  
  
 **Syntaxis**  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
 **Typen retourneren**  
  
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
  
####  <a name="bk_endswith"></a> ENDSWITH  
 Retourneert een Booleaanse waarde die aangeeft of de eerste expressie tekenreeks eindigt met de tweede.  
  
 **Syntaxis**  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
 **Typen retourneren**  
  
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
  
####  <a name="bk_index_of"></a> INDEX_OF  
 Retourneert de beginpositie van het eerste exemplaar van de tweede tekenreeksexpressie binnen de eerste opgegeven tekenreeksexpressie, of -1 als de tekenreeks niet is gevonden.  
  
 **Syntaxis**  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt de index van verschillende subtekenreeksen in "abc".  
  
```  
SELECT INDEX_OF("abc", "ab"), INDEX_OF("abc", "b"), INDEX_OF("abc", "c")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": 0, "$2": 1, "$3": -1}]  
```  
  
####  <a name="bk_left"></a> LINKS  
 Retourneert het linkerdeel van een tekenreeks met het opgegeven aantal tekens.  
  
 **Syntaxis**  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
-   `num_expr`  
  
     Een geldige numerieke expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt het linkerdeel van "abc" voor verschillende waarden voor lengte.  
  
```  
SELECT LEFT("abc", 1), LEFT("abc", 2)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "a", "$2": "ab"}]  
```  
  
####  <a name="bk_length"></a> LENGTE  
 Retourneert het aantal tekens van de opgegeven tekenreeksexpressie.  
  
 **Syntaxis**  
  
```  
LENGTH(<str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
 **Typen retourneren**  
  
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
  
####  <a name="bk_lower"></a> LAGERE  
 Retourneert een tekenreeksexpressie na het converteren van tekens in hoofdletters naar kleine letters.  
  
 **Syntaxis**  
  
```  
LOWER(<str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
 **Typen retourneren**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld ziet hoe u in een query lager gebruiken.  
  
```  
SELECT LOWER("Abc")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "abc"}]  
  
```  
  
####  <a name="bk_ltrim"></a> LTRIM  
 Retourneert een tekenreeksexpressie na het verwijderen van lege voorlooptekens.  
  
 **Syntaxis**  
  
```  
LTRIM(<str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
 **Typen retourneren**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld ziet hoe u LTRIM binnen een query gebruiken.  
  
```  
SELECT LTRIM("  abc"), LTRIM("abc"), LTRIM("abc   ")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "abc", "$2": "abc", "$3": "abc   "}]  
```  
  
####  <a name="bk_replace"></a> VERVANGEN  
 Vervangt alle exemplaren van een opgegeven tekenreekswaarde door een andere tekenreekswaarde.  
  
 **Syntaxis**  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
 **Typen retourneren**  
  
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
  
####  <a name="bk_replicate"></a> REPLICEREN  
 Herhaalt een tekenreekswaarde een opgegeven aantal keren.  
  
 **Syntaxis**  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
-   `num_expr`  
  
     Een geldige numerieke expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld ziet hoe u kunt REPLICEREN in een query gebruiken.  
  
```  
SELECT REPLICATE("a", 3)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "aaa"}]  
```  
  
####  <a name="bk_reverse"></a> OMKEREN  
 Retourneert een tekenreekswaarde in de omgekeerde volgorde.  
  
 **Syntaxis**  
  
```  
REVERSE(<str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
 **Typen retourneren**  
  
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
  
####  <a name="bk_right"></a> RECHTS  
 Retourneert het rechterdeel van een tekenreeks met het opgegeven aantal tekens.  
  
 **Syntaxis**  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
-   `num_expr`  
  
     Een geldige numerieke expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt het rechterdeel van "abc" voor verschillende waarden voor lengte.  
  
```  
SELECT RIGHT("abc", 1), RIGHT("abc", 2)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "c", "$2": "bc"}]  
```  
  
####  <a name="bk_rtrim"></a> RTRIM  
 Retourneert een tekenreeksexpressie na het afsluitende spaties verwijderen.  
  
 **Syntaxis**  
  
```  
RTRIM(<str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
 **Typen retourneren**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld ziet hoe u RTRIM binnen een query gebruiken.  
  
```  
SELECT RTRIM("  abc"), RTRIM("abc"), RTRIM("abc   ")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "   abc", "$2": "abc", "$3": "abc"}]  
```  
  
####  <a name="bk_startswith"></a> STARTSWITH  
 Retourneert een Booleaanse waarde die aangeeft of de eerste expressie tekenreeks begint met de tweede.  
  
 **Syntaxis**  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
 **Typen retourneren**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld wordt als de tekenreeks "abc" met "b begint" en "a".  
  
```  
SELECT STARTSWITH("abc", "b"), STARTSWITH("abc", "a")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <a name="bk_substring"></a> DE SUBTEKENREEKS  
 Onderdeel van een tekenreeksexpressie vanaf de op nul gebaseerde positie van het opgegeven teken geretourneerd en blijft aan de opgegeven lengte of aan het einde van de tekenreeks.  
  
 **Syntaxis**  
  
```  
SUBSTRING(<str_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
-   `num_expr`  
  
     Een geldige numerieke expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld retourneert de subtekenreeks van "abc" beginnen op 1 en een lengte van 1 teken bevatten.  
  
```  
SELECT SUBSTRING("abc", 1, 1)  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "b"}]  
```  
####  <a name="bk_tostring"></a> ToString  
 Retourneert een tekenreeksrepresentatie van de scalaire expressie. 
  
 **Syntaxis**  
  
```  
ToString(<expr>)
```  
  
 **Argumenten**  
  
-   `expr`  
  
     Is geldige scalaire expressie.  
  
 **Typen retourneren**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld laat zien hoe ToString gedraagt zich op verschillende typen.   
  
```  
SELECT ToString(1.0000), ToString("Hello World"), ToString(NaN), ToString(Infinity),
ToString(IS_STRING(ToString(undefined))), IS_STRING(ToString(0.1234), ToString(false), ToString(undefined))
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "1", "$2": "Hello World", "$3": "NaN", "$4": "Infinity", "$5": "false", "$6": true, "$7": "false"}]  
```  
 De volgende invoer gegeven:
```  
{"Products":[{"ProductID":1,"Weight":4,"WeightUnits":"lb"},{"ProductID":2,"Weight":32,"WeightUnits":"kg"},{"ProductID":3,"Weight":400,"WeightUnits":"g"},{"ProductID":4,"Weight":8999,"WeightUnits":"mg"}]}
```    
 Het volgende voorbeeld laat zien hoe ToString met andere tekenreeksfuncties zoals CONCAT kan worden gebruikt.   
 
```  
SELECT 
CONCAT(ToString(p.Weight), p.WeightUnits) 
FROM p in c.Products 
```  

 Hier volgt de resultatenset.  
  
```  
[{"$1":"4lb" },
 {"$1":"32kg"},
 {"$1":"400g" },
 {"$1":"8999mg" }]

```  
De volgende invoer wordt gegeven.
```
{"id":"08259","description":"Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX","nutrients":[{"id":"305","description":"Caffeine","units":"mg"},{"id":"306","description":"Cholesterol, HDL","nutritionValue":30,"units":"mg"},{"id":"307","description":"Sodium, NA","nutritionValue":612,"units":"mg"},{"id":"308","description":"Protein, ABP","nutritionValue":60,"units":"mg"},{"id":"309","description":"Zinc, ZN","nutritionValue":null,"units":"mg"}]}
```
 Het volgende voorbeeld laat zien hoe ToString kan worden gebruikt met een andere tekenreeks-functies, zoals vervangen.   
```
SELECT 
    n.id AS nutrientID,
    REPLACE(ToString(n.nutritionValue), "6", "9") AS nutritionVal
FROM food 
JOIN n IN food.nutrients
```
 Hier volgt de resultatenset.  
 ```
[{"nutrientID":"305"},
{"nutrientID":"306","nutritionVal":"30"},
{"nutrientID":"307","nutritionVal":"912"},
{"nutrientID":"308","nutritionVal":"90"},
{"nutrientID":"309","nutritionVal":"null"}]
 ``` 
 
####  <a name="bk_trim"></a> TRIM  
 Retourneert een tekenreeksexpressie na het verwijderen van voorloopspaties en volgspaties.  
  
 **Syntaxis**  
  
```  
TRIM(<str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
 **Typen retourneren**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld ziet hoe u TRIM binnen een query.  
  
```  
SELECT TRIM("   abc"), TRIM("   abc   "), TRIM("abc   "), TRIM("abc")   
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "abc", "$2": "abc", "$3": "abc", "$4": "abc"}]  
``` 
####  <a name="bk_upper"></a> BOVENSTE  
 Retourneert een tekenreeksexpressie na het converteren van tekens in kleine letters naar hoofdletters.  
  
 **Syntaxis**  
  
```  
UPPER(<str_expr>)  
```  
  
 **Argumenten**  
  
-   `str_expr`  
  
     Is een geldige tekenreeks-expressie.  
  
 **Typen retourneren**  
  
 Retourneert een tekenreeksexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld laat zien hoe het gebruik van hoofdletters in een query  
  
```  
SELECT UPPER("Abc")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": "ABC"}]  
```  
  
###  <a name="bk_array_functions"></a> Matrixfuncties  
 De volgende scalaire functies uitvoeren van een bewerking op een matrix invoerwaarde en retourneren numerieke, Booleaanse waarde of een matrix-waarde  
  
||||  
|-|-|-|  
|[ARRAY_CONCAT](#bk_array_concat)|[ARRAY_CONTAINS](#bk_array_contains)|[ARRAY_LENGTH](#bk_array_length)|  
|[ARRAY_SLICE](#bk_array_slice)|||  
  
####  <a name="bk_array_concat"></a> ARRAY_CONCAT  
 Retourneert een matrix die het resultaat is van het samenvoegen van twee of meer matrixwaarden.  
  
 **Syntaxis**  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 **Argumenten**  
  
-   `arr_expr`  
  
     Is een geldige matrix-expressie.  
  
 **Typen retourneren**  
  
 Retourneert een matrixexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld over het samenvoegen van twee matrices.  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"])  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <a name="bk_array_contains"></a> ARRAY_CONTAINS  
Retourneert een Booleaanse waarde die aangeeft of de matrix de opgegeven waarde bevat. U kunt kiezen tussen een volledige of gedeeltelijke overeenkomst. 

 **Syntaxis**  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr> [, bool_expr])  
```  
  
 **Argumenten**  
  
-   `arr_expr`  
  
     Is een geldige matrix-expressie.  
  
-   `expr`  
  
     Een geldige expressie is.  

-   `bool_expr`  
  
     Een Booleaanse expressie is.       
  
 **Typen retourneren**  
  
 Retourneert een Booleaanse waarde.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld hoe om te controleren voor lidmaatschap in een matrix met behulp van ARRAY_CONTAINS.  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples"),  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes")  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": true, "$2": false}]  
```  

 Het volgende voorbeeld hoe om te controleren op een gedeeltelijke overeenkomst van een JSON-code in een matrix met behulp van ARRAY_CONTAINS.  
  
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
  
####  <a name="bk_array_length"></a> ARRAY_LENGTH  
 Retourneert het aantal elementen van de opgegeven matrixexpressie.  
  
 **Syntaxis**  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 **Argumenten**  
  
-   `arr_expr`  
  
     Is een geldige matrix-expressie.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld over het verkrijgen van de lengte van een matrix met behulp van ARRAY_LENGTH.  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"])  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"$1": 3}]  
```  
  
####  <a name="bk_array_slice"></a> ARRAY_SLICE  
 Retourneert een deel van een matrixexpressie.
  
 **Syntaxis**  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Argumenten**  
  
-   `arr_expr`  
  
     Is een geldige matrix-expressie.  
  
-   `num_expr`  
  
     Numerieke op nul gebaseerde index op waarop u wilt beginnen met de matrix. Negatieve waarden kunnen worden gebruikt om op te geven van de startIndex ten opzichte van het laatste element van de matrix-dat wil zeggen 1 verwijst naar het laatste element in de matrix.  

-   `num_expr`  

     Maximum aantal elementen in de resulterende matrix.    

 **Typen retourneren**  
  
 Retourneert een matrixexpressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld ziet hoe u verschillende segmenten van een matrix met behulp van ARRAY_SLICE.  
  
```  
SELECT   
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1),  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1),
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 1),
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 2),
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 0),
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1000),
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, -100)      
  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{  
           "$1": ["strawberries", "bananas"],   
           "$2": ["strawberries"],
           "$3": ["strawberries"],  
           "$4": ["strawberries", "bananas"], 
           "$5": [],
           "$6": ["strawberries", "bananas"],
           "$7": [] 
}]  
```  
 
###  <a name="bk_spatial_functions"></a> Ruimtelijke-functies  
 De volgende scalaire functies uitvoeren van een bewerking op een invoerwaarde ruimtelijke index en een numerieke of Booleaanse waarde retourneren.  
  
||||  
|-|-|-|  
|[ST_DISTANCE](#bk_st_distance)|[ST_WITHIN](#bk_st_within)|[ST_INTERSECTS](#bk_st_intersects)|[ST_ISVALID](#bk_st_isvalid)|  
|[ST_ISVALIDDETAILED](#bk_st_isvaliddetailed)|||  
  
####  <a name="bk_st_distance"></a> ST_DISTANCE  
 Retourneert de afstand tussen de twee GeoJSON Point-, Polygon- of LineString-expressies.  
  
 **Syntaxis**  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumenten**  
  
-   `spatial_expr`  
  
     Is geldige expressie voor GeoJSON-punt, Polygon of LineString-object.  
  
 **Typen retourneren**  
  
 Retourneert een numerieke expressie met de afstand. Dit wordt uitgedrukt in meters voor de verwijzing van het systeem.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld ziet hoe alle familie documenten die binnen 30 kilometer van de opgegeven locatie met behulp van de ingebouwde functie ST_DISTANCE worden geretourneerd. .  
  
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
  
####  <a name="bk_st_within"></a> ST_WITHIN  
 Retourneert een Booleaanse expressie die aangeeft of de GeoJSON-object dat in het eerste argument opgegeven (punt, Polygon of LineString) binnen de GeoJSON (punt, Polygon of LineString) in het tweede argument.  
  
 **Syntaxis**  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumenten**  
  
-   `spatial_expr`  
  
     Is geldige expressie voor GeoJSON-punt, Polygon of LineString-object.  
 
-   `spatial_expr`  
  
     Is geldige expressie voor GeoJSON-punt, Polygon of LineString-object.  
  
 **Typen retourneren**  
  
 Retourneert een Booleaanse waarde.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld ziet hoe u kunt zoeken naar alle familie documenten binnen een veelhoek met behulp van ST_WITHIN.  
  
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

####  <a name="bk_st_intersects"></a> ST_INTERSECTS  
 Retourneert een Booleaanse expressie waarmee wordt aangegeven of de GeoJSON-object dat in het eerste argument opgegeven (punt, Polygon of LineString) samen met de GeoJSON (punt, Polygon of LineString) in het tweede argument.  
  
 **Syntaxis**  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumenten**  
  
-   `spatial_expr`  
  
     Is geldige expressie voor GeoJSON-punt, Polygon of LineString-object.  
 
-   `spatial_expr`  
  
     Is geldige expressie voor GeoJSON-punt, Polygon of LineString-object.  
  
 **Typen retourneren**  
  
 Retourneert een Booleaanse waarde.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld ziet hoe u kunt alle gebieden die elkaar met de opgegeven veelhoek overlappen vinden.  
  
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
  
####  <a name="bk_st_isvalid"></a> ST_ISVALID  
 Retourneert een Booleaanse waarde die aangeeft of de opgegeven GeoJSON Point-, Polygon- of LineString-expressie geldig is.  
  
 **Syntaxis**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Argumenten**  
  
-   `spatial_expr`  
  
     Is geldige GeoJSON-punt, Polygon of LineString-expressie.  
  
 **Typen retourneren**  
  
 Retourneert een Booleaanse expressie.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld laat zien hoe om te controleren of een punt geldig met behulp van ST_VALID is.  
  
 Bijvoorbeeld, heeft dit punt een Breedtegraadwaarde die zich niet in het geldige waardenbereik [tussen-90 en, 90], zodat de query retourneert ' false '.  
  
 Voor polygonen, de GeoJSON-specificatie is vereist dat de opgegeven combinatie van laatste coördinaat moet hetzelfde zijn als de eerste pagina, het maken van een gesloten vorm. Punten in een polygoon moeten worden opgegeven in volgorde van linksom draaien. Een veelhoek in rechtsom volgorde opgegeven vertegenwoordigt de omgekeerde waarde van de regio binnen het.  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{ "$1": false }]  
```  
  
####  <a name="bk_st_isvaliddetailed"></a> ST_ISVALIDDETAILED  
 Retourneert een JSON-waarde met een Booleaanse waarde die aangeeft of de opgegeven GeoJSON Point-, Polygon- of LineString-expressie geldig is. Als de expressie ongeldig is, worden ook de reden daarvoor en een tekenreekswaarde geretourneerd.  
  
 **Syntaxis**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Argumenten**  
  
-   `spatial_expr`  
  
     Een geldige GeoJSON-veelhoek-punt of de expressie is.  
  
 **Typen retourneren**  
  
 Retourneert een JSON-waarde met een Booleaanse waarde als de opgegeven GeoJSON-veelhoek-punt of de expressie is geldig, en als ongeldig, ook de reden als een string-waarde.  
  
 **Voorbeelden**  
  
 Het volgende voorbeeld hoe u controleert de geldigheid (met gegevens) met behulp van ST_ISVALIDDETAILED.  
  
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

- [SQL-syntaxis en SQL-query voor Cosmos DB](how-to-sql-query.md)

- [Documentatie voor cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)  
