---
title: SQL-syntaxis in Azure Cosmos DB
description: In dit artikel wordt uitgelegd dat de SQL-query-syntaxis in Azure Cosmos DB, andere operatoren en trefwoorden die beschikbaar zijn in deze taal gebruikt.
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.custom: seodec18
ms.openlocfilehash: 1d874b9c8f14b1489ab5e5b8bbdddaff0669165e
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65145181"
---
# <a name="sql-language-reference-for-azure-cosmos-db"></a>Naslaginformatie voor Azure Cosmos DB SQL-taal 

Azure Cosmos DB ondersteunt het opvragen van documenten met behulp van een vertrouwde SQL (Structured Query Language) zoals grammatica via hiërarchische JSON-documenten zonder een expliciet schema of secundaire indexen worden gemaakt. Dit artikel bevat documentatie voor de syntaxis van de SQL-query in SQL-API-accounts gebruikt. Zie voor een overzicht van voorbeeld van de SQL-query's, [voorbeelden van de SQL-query in Cosmos DB](how-to-sql-query.md).  
  
Ga naar de [testomgeving voor Query's](https://www.documentdb.com/sql/demo), kunt u Cosmos DB uitproberen en SQL-query's op een voorbeeldgegevensset uitvoeren.  
  
## <a name="select-query"></a>SELECT-query  
Elke query bestaat uit een SELECT-component en optionele FROM- en WHERE-componenten conform ANSI-SQL-standaarden. Normaal gesproken voor elke query, de bron in de component FROM is geïnventariseerd en vervolgens het filter in de component WHERE wordt toegepast op de bron om op te halen van een subset van JSON-documenten. Ten slotte wordt de SELECT-component gebruikt om de vereiste JSON-waarden in de SELECT-lijst te projecten. Zie voor voorbeelden van [SELECT-query-voorbeelden](how-to-sql-query.md#SelectClause)
  
**Syntaxis**  
  
```sql
<select_query> ::=  
SELECT <select_specification>   
    [ FROM <from_specification>]   
    [ WHERE <filter_condition> ]  
    [ ORDER BY <sort_specification> ] 
    [ OFFSET <offset_amount> LIMIT <limit_amount>]
```  
  
 **Opmerkingen**  
  
 Zie de volgende secties voor meer informatie op elke component:  
  
-   [SELECT-component](#bk_select_query)    
-   [FROM-component](#bk_from_clause)    
-   [WHERE-component](#bk_where_clause)    
-   [ORDER BY-component](#bk_orderby_clause)  
-   [De component OFFSET LIMIET](#bk_offsetlimit_clause)

  
De clausules in de SELECT-instructie moeten worden besteld, zoals hierboven. Een van de optionele componenten kan worden weggelaten. Maar als de optionele componenten zijn gebruikt, moeten ze worden weergegeven in de juiste volgorde.  
  
### <a name="logical-processing-order-of-the-select-statement"></a>Logische verwerkingsvolgorde van de SELECT-instructie  
  
De volgorde waarin de EU worden verwerkt, is:  

1.  [FROM-component](#bk_from_clause)  
2.  [WHERE-component](#bk_where_clause)  
3.  [ORDER BY-component](#bk_orderby_clause)  
4.  [SELECT-component](#bk_select_query)
5.  [De component OFFSET LIMIET](#bk_offsetlimit_clause)

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
      | [DISTINCT] <object_property_list>   
      | [DISTINCT] VALUE <scalar_expression> [[ AS ] value_alias]  
  
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
 
- `DISTINCT`
  
  Hiermee geeft u op dat de kopieën van de verwachte eigenschappen moeten worden verwijderd.  

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
  
- Een join tussen container-set A en binnen het bereik van container set B, resulteert in een vectorproduct van alle elementen in sets A en B.
  
- Een koppeling tussen een set en binnen het bereik van document set B, resulteert in een samenvoeging van alle sets die zijn verkregen door het evalueren van binnen het bereik van document set B voor elk document van A. instellen  
  
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
  
- `<filter_condition>`  
  
   Hiermee geeft u de voorwaarde moet worden voldaan voor de documenten die moeten worden geretourneerd.  
  
- `<scalar_expression>`  
  
   Expressie voor de waarde die moet worden berekend. Zie de [scalaire expressies](#bk_scalar_expressions) sectie voor meer informatie.  
  
  **Opmerkingen**  
  
  Opdat het document dat moet worden geretourneerd van een expressie die is opgegeven als filter moet voorwaarde resulteren in waar. Alleen Booleaanse waarde ' True ', voldoen aan de voorwaarde, een andere waarde: niet-gedefinieerde, null, ingesteld op false, getal, matrix of Object zal voldoen niet aan de voorwaarde.  
  
##  <a name="bk_orderby_clause"></a> ORDER BY-component  
 Hiermee geeft u de sorteervolgorde voor de resultaten geretourneerd door de query. Zie voor voorbeelden van [ORDER BY-component-voorbeelden](how-to-sql-query.md#OrderByClause)
  
 **Syntaxis**  
  
```sql  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= {<scalar_expression> [ASC | DESC]} [ ,...n ]  
  
```  

 **Argumenten**  
  
- `<sort_specification>`  
  
   Hiermee geeft u een eigenschap of een expressie waarop u wilt sorteren van de queryresultaatset. Een sorteerkolom kan worden opgegeven als een alias voor de naam of de eigenschap.  
  
   Meerdere eigenschappen kunnen worden opgegeven. Namen van eigenschappen moeten uniek zijn. De volgorde van de eigenschappen van de sortering in de component ORDER BY definieert de organisatie van de gesorteerde resultatenset. Dat wil zeggen, de resultatenset is gesorteerd op de eerste eigenschap en vervolgens die geordende lijst is gesorteerd op de tweede eigenschap, enzovoort.  
  
   De namen van eigenschappen waarnaar wordt verwezen in de component ORDER BY moeten overeenkomen met ofwel een eigenschap in de lijst selecteren of een eigenschap die is gedefinieerd in de verzameling die is opgegeven in de component FROM zonder eventuele dubbelzinnigheden.  
  
- `<sort_expression>`  
  
   Hiermee geeft u een of meer eigenschappen of expressies waarop u wilt sorteren van de queryresultaatset.  
  
- `<scalar_expression>`  
  
   Zie de [scalaire expressies](#bk_scalar_expressions) sectie voor meer informatie.  
  
- `ASC | DESC`  
  
   Hiermee geeft u op dat de waarden in de opgegeven kolom moeten worden gesorteerd in oplopende of aflopende volgorde. ASC sorteren van de laagste waarde naar hoogste waarde. DESC sorteren van hoogste waarde naar laagste waarde. ASC is de standaardsorteervolgorde. Null-waarden worden behandeld als de laagste mogelijke waarden.  
  
  **Opmerkingen**  
  
   De component ORDER BY is vereist dat het indexeringsbeleid bevatten een index voor de velden worden gesorteerd. De runtime van de query Azure Cosmos DB biedt ondersteuning voor sorteren op basis van een eigenschapsnaam en niet op basis van de berekende eigenschappen. Azure Cosmos DB biedt ondersteuning voor meerdere ORDER BY-eigenschappen. Als u wilt een query uitvoert met meerdere ORDER BY-eigenschappen, moet u definiëren een [samengestelde index](index-policy.md#composite-indexes) op de velden worden gesorteerd.


##  <a name=bk_offsetlimit_clause></a> De component OFFSET LIMIET

Hiermee geeft u het aantal items overgeslagen en het aantal geretourneerde items. Zie voor voorbeelden van [OFFSET LIMIET component voorbeelden](how-to-sql-query.md#OffsetLimitClause)
  
 **Syntaxis**  
  
```sql  
OFFSET <offset_amount> LIMIT <limit_amount>
```  
  
 **Argumenten**  
 
- `<offset_amount>`

   Hiermee geeft u het geheel getal van de items die de resultaten van de query moeten worden overgeslagen.


- `<limit_amount>`
  
   Hiermee geeft u het geheel getal van de items die de queryresultaten bevatten

  **Opmerkingen**  
  
  Zowel de verschuiving van het aantal en het maximum aantal zijn vereist in de component OFFSET LIMIET. Als een optionele `ORDER BY` component wordt gebruikt, wordt de resultatenset wordt geproduceerd door de overslaan doen via de geordende waarden. Anders retourneert de query een vaste volgorde van waarden.

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
  
- `<constant>`  
  
   Hiermee geeft u een constante waarde. Zie [constanten](#bk_constants) sectie voor meer informatie.  
  
- `input_alias`  
  
   Hiermee geeft u een waarde die is gedefinieerd door de `input_alias` die is geïntroduceerd in de `FROM` component.  
  Deze waarde kan niet worden gegarandeerd **niet-gedefinieerde** –**niet-gedefinieerde** waarden in de invoer worden overgeslagen.  
  
- `<scalar_expression>.property_name`  
  
   Hiermee geeft u een waarde van de eigenschap van een object. Als de eigenschap niet bestaat of eigenschap van een waarde die niet een object wordt verwezen, wordt de expressie wordt geëvalueerd als **niet-gedefinieerde** waarde.  
  
- `<scalar_expression>'['"property_name"|array_index']'`  
  
   Hiermee geeft u een waarde van de eigenschap met de naam `property_name` of matrixelement met index `array_index` van een object/matrix. Als de eigenschap/matrixindex niet bestaat of index van de eigenschap/array wordt verwezen op een waarde die is geen object/matrix, wordt de expressie wordt geëvalueerd als niet-gedefinieerde waarde.  
  
- `unary_operator <scalar_expression>`  
  
   Hiermee geeft u een operator die wordt toegepast op een enkele waarde. Zie [Operators](#bk_operators) sectie voor meer informatie.  
  
- `<scalar_expression> binary_operator <scalar_expression>`  
  
   Een operator die wordt toegepast op de twee waarden vertegenwoordigt. Zie [Operators](#bk_operators) sectie voor meer informatie.  
  
- `<scalar_function_expression>`  
  
   Hiermee geeft u een waarde die is gedefinieerd door een resultaat van een aanroep van de functie.  
  
- `udf_scalar_function`  
  
   De naam van de gebruiker gedefinieerde scalaire functie.  
  
- `builtin_scalar_function`  
  
   Naam van de ingebouwde scalaire functie.  
  
- `<create_object_expression>`  
  
   Hiermee geeft u een waarde die is verkregen door het maken van een nieuw object met de opgegeven eigenschappen en hun waarden.  
  
- `<create_array_expression>`  
  
   Hiermee geeft u een waarde die is verkregen door het maken van een nieuwe matrix met de opgegeven waarden als elementen  
  
- `parameter_name`  
  
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
|**Bitsgewijze**|Operator verwacht invoerwaarde(n) ondertekende 32-bits geheel getal (s). Uitvoer is ook 32-bits geheel getal.<br /><br /> Een niet-integer-waarde wordt afgerond. Positieve waarde wordt afgerond naar beneden en negatieve getallen naar boven afgerond.<br /><br /> Een waarde buiten het bereik van de 32-bits geheel getal wordt geconverteerd, door te nemen van de laatste 32-bits van de twee van bits.<br /><br /> Als een van de invoer is **niet-gedefinieerde** of typ dan het getal, dan is het resultaat **niet-gedefinieerde**.<br /><br /> **Opmerking:** De bovenstaande gedrag is compatibel met JavaScript bitsgewijze operator gedrag.|  
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
|**Logische**|**EN**<br /><br /> **OR**|Logische combinatie. Retourneert **waar** als beide argumenten zijn **waar**, retourneert **false** anders.<br /><br /> Logische scheiding. Retourneert **waar** als alle argumenten zijn **waar**, retourneert **false** anders.|  
|**Vergelijking**|**=**<br /><br /> **!=, <>**<br /><br /> **>**<br /><br /> **>=**<br /><br /> **<**<br /><br /> **<=**<br /><br /> **??**|Is gelijk aan. Retourneert **waar** als argumenten gelijk zijn, retourneert **false** anders.<br /><br /> Niet gelijk aan. Retourneert **waar** als argumenten niet gelijk zijn zijn, retourneert **false** anders.<br /><br /> Groter dan. Retourneert **waar** als eerste argument groter dan de tweede waarde is, retourneren **false** anders.<br /><br /> Groter dan of gelijk zijn aan. Retourneert **waar** als eerste argument groter dan of gelijk zijn aan de tweede waarde is, retourneren **false** anders.<br /><br /> Kleiner dan. Retourneert **waar** als eerste argument minder is dan het tweede één rendement **false** anders.<br /><br /> Kleiner dan of gelijk zijn aan. Retourneert **waar** als eerste argument kleiner dan of gelijk zijn aan de tweede waarde, retourneren **false** anders.<br /><br /> Samenvoegen. Het tweede argument geeft als resultaat als het eerste argument is een **niet-gedefinieerde** waarde.|  
|**String**|**&#124;&#124;**|Samenvoeging. Retourneert een samenvoeging van beide argumenten.|  
  
 **Ternair operators:**  

|**Naam**|**Operator**|**Details**| 
|-|-|-|  
|Ternaire operator|?|Het tweede argument geeft als resultaat als het eerste argument wordt geëvalueerd als **waar**; anders wordt het derde argument geretourneerd.|  

  
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
  
 Dit betekent dat een query zoals: Selecteer * uit ROOT r waar r.Age = 21, retourneert alleen documenten waarvoor de eigenschap leeftijd gelijk zijn aan het getal 21. Documenten met de eigenschap leeftijd is gelijk aan de tekenreeks '21' of de tekenreeks '0021' wordt niet overeenkomen, als de expressie "21" = 21 tot niet-gedefinieerde evalueert. Hiermee kunt u een beter gebruik van indexen, omdat het opzoeken van een bepaalde waarde (zoals het aantal 21) is veel sneller dan zoeken naar een onbeperkt aantal potentiële komt overeen met (het nummer 21 of tekenreeksen "21", "021", "21,0"...). Dit wijkt af van de manier waarop JavaScript operators op basis van waarden van verschillende typen evalueert.  
  
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
|\\\ |omgekeerde schuine streep (\\)|U + 005C|  
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
  
- Gebruik gelijkheidsoperator (=) met-padexpressie aan een document en een constante zijn.  
  
- Bereik-operators gebruiken (<, \<=, >, > =) met-padexpressie aan een document en aantal constanten.  
  
- Document-padexpressie aan staat voor een expressie die een constante pad in de documenten uit de databasecontainer waarnaar wordt verwezen, identificeert.  
  
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
|[Datum- en tijdfuncties](#bk_date_and_time_functions)|De functies date en time kunnen u de huidige UTC-datum en tijd in twee vormen; een numerieke tijdstempel waarvan de waarde is de Unix-epoche in milliseconden of als een tekenreeks die aan de ISO 8601-notatie voldoet.|
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
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld toont de resultaten van het gebruik van de functie ABS op drie verschillende aantallen.  
  
```  
SELECT ABS(-1) AS abs1, ABS(0) AS abs2, ABS(1) AS abs3 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{abs1: 1, abs2: 0, abs3: 1}]  
```  
  
####  <a name="bk_acos"></a> ACOS  
 Retourneert de hoek, in radialen, waarvan de cosinus de opgegeven numerieke expressie is. Dit wordt ook wel de arccosinus genoemd.  
  
 **Syntaxis**  
  
```  
ACOS(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de ACOS van-1.  
  
```  
SELECT ACOS(-1) AS acos 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"acos": 3.1415926535897931}]  
```  
  
####  <a name="bk_asin"></a> ASIN  
 Retourneert de hoek, in radialen, waarvan de sinus de opgegeven numerieke expressie is. Dit wordt ook boogsinus genoemd.  
  
 **Syntaxis**  
  
```  
ASIN(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de ASIN-1.  
  
```  
SELECT ASIN(-1) AS asin  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"asin": -1.5707963267948966}]  
```  
  
####  <a name="bk_atan"></a> ATAN  
 Retourneert de hoek, in radialen, waarvan de tangens de opgegeven numerieke expressie is. Dit wordt ook wel de arctangens genoemd.  
  
 **Syntaxis**  
  
```  
ATAN(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de ATAN van de opgegeven waarde.  
  
```  
SELECT ATAN(-45.01) AS atan  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"atan": -1.5485826962062663}]  
```  
  
####  <a name="bk_atn2"></a> ATN2  
 Retourneert de hoofdsom van de arctangens van y / x, uitgedrukt in radialen.  
  
 **Syntaxis**  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld berekent de ATN2 voor de opgegeven x- en y onderdelen.  
  
```  
SELECT ATN2(35.175643, 129.44) AS atn2  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"atn2": 1.3054517947300646}]  
```  
  
####  <a name="bk_ceiling"></a> MAXIMUM  
 Retourneert het kleinste gehele getal dat groter is dan of gelijk is aan de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
CEILING (<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld toont een positieve numerieke, negatief en nulwaarden met de functie CEILING.  
  
```  
SELECT CEILING(123.45) AS c1, CEILING(-123.45) AS c2, CEILING(0.0) AS c3  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{c1: 124, c2: -123, c3: 0}]  
```  
  
####  <a name="bk_cos"></a> COS  
 Retourneert de trigonometrische cosinus van de opgegeven hoek, in radialen, in de opgegeven expressie.  
  
 **Syntaxis**  
  
```  
COS(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld berekent de Cosinus van de opgegeven hoek.  
  
```  
SELECT COS(14.78) AS cos  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"cos": -0.59946542619465426}]  
```  
  
####  <a name="bk_cot"></a> COT  
 Retourneert de trigonometrische cotangens van de opgegeven hoek, in radialen, in de opgegeven expressie.  
  
 **Syntaxis**  
  
```  
COT(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld berekent de COT van de opgegeven hoek.  
  
```  
SELECT COT(124.1332) AS cot  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"cot": -0.040311998371148884}]  
```  
  
####  <a name="bk_degrees"></a> GRADEN  
 Retourneert de overeenkomende hoek in graden voor een hoek die is opgegeven in radialen.  
  
 **Syntaxis**  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert het aantal graden in een hoek van PI/2 radialen.  
  
```  
SELECT DEGREES(PI()/2) AS degrees  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"degrees": 90}]  
```  
  
####  <a name="bk_floor"></a> FLOOR  
 Retourneert het grootste gehele getal dat kleiner is dan of gelijk is aan de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld toont een positieve numerieke, negatief en nulwaarden met de functie FLOOR.  
  
```  
SELECT FLOOR(123.45) AS fl1, FLOOR(-123.45) AS fl2, FLOOR(0.0) AS fl3  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{fl1: 123, fl2: -124, fl3: 0}]  
```  
  
####  <a name="bk_exp"></a> EXP  
 Retourneert de exponentiële waarde van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
EXP (<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Opmerkingen**  
  
  De constante **e** (2.718281...) vormt de basis van natuurlijke logaritmen.  
  
  De exponent van een getal is de constante **e** verheven tot de macht van het getal. Bijvoorbeeld, EXP(1.0) = e ^ 1.0 = 2.71828182845905 en EXP(10) = e ^ 10 = 22026.4657948067.  
  
  De exponentiële waarde van de natuurlijke logaritme van een getal is het aantal zelf: EXP (logboek (n)) = n. En de natuurlijke logaritme van de exponentiële van een getal is het aantal zelf: LOGBOEK (EXP (n)) = n.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt een variabele gedeclareerd en berekent de exponentiële waarde van de opgegeven variabele (10).  
  
```  
SELECT EXP(10) AS exp  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{exp: 22026.465794806718}]  
```  
  
 Het volgende voorbeeld retourneert de exponentiële waarde van de natural logarithm van 20 en de natuurlijke logaritme van de exponentiële van 20. Omdat deze functies inverse functies van elkaar zijn, is de geretourneerde waarde met de afronding voor zwevende punt math in beide gevallen 20.  
  
```  
SELECT EXP(LOG(20)) AS exp1, LOG(EXP(20)) AS exp2  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{exp1: 19.999999999999996, exp2: 20}]  
```  
  
####  <a name="bk_log"></a> LOGBOEK  
 Retourneert de natuurlijke logaritme van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
- `base`  
  
   Optionele numerieke argument dat Hiermee stelt u de basis voor de logaritme.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Opmerkingen**  
  
  Standaard retourneert LOG() de natuurlijke logaritme. U kunt het grondtal van de logaritme met een andere waarde wijzigen met behulp van de optionele parameter basis.  
  
  De natuurlijke logaritme is de logaritme voor de base **e**, waarbij **e** is een constante irrational ongeveer gelijk zijn aan 2.718281828.  
  
  De natuurlijke logaritme van de exponentiële van een getal is het aantal zelf: LOGBOEK (EXP (n)) = n. En de exponentiële waarde van de natuurlijke logaritme van een getal is het aantal zelf: EXP (logboek (n)) = n.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt een variabele gedeclareerd en retourneert de logaritme-waarde van de opgegeven variabele (10).  
  
```  
SELECT LOG(10) AS log  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{log: 2.3025850929940459}]  
```  
  
 Het volgende voorbeeld wordt het logboek voor de exponent van een getal berekend.  
  
```  
SELECT EXP(LOG(10)) AS expLog  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{expLog: 10.000000000000002}]  
```  
  
####  <a name="bk_log10"></a> LOG10  
 Retourneert de logaritme met grondtal 10 van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Opmerkingen**  
  
  De functies LOG10 en POWER zijn omgekeerd met elkaar gerelateerd. Bijvoorbeeld, 10 ^ LOG10(n) = n.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt een variabele gedeclareerd en retourneert de waarde LOG10 van de opgegeven variabele (100).  
  
```  
SELECT LOG10(100) AS log10 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{log10: 2}]  
```  
  
####  <a name="bk_pi"></a> PI  
 Retourneert de constante waarde van PI.  
  
 **Syntaxis**  
  
```  
PI ()  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de waarde van PI.  
  
```  
SELECT PI() AS pi 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"pi": 3.1415926535897931}]  
```  
  
####  <a name="bk_power"></a> ENERGIEBEHEER  
 Retourneert de waarde van de opgegeven expressie voor de opgegeven macht.  
  
 **Syntaxis**  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
- `y`  
  
   Is de kracht waarnaar u wilt verhogen `numeric_expression`.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet u een getal tot de macht 3 (de kubus van het getal) verhogen.  
  
```  
SELECT POWER(2, 3) AS pow1, POWER(2.5, 3) AS pow2  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{pow1: 8, pow2: 15.625}]  
```  
  
####  <a name="bk_radians"></a> RADIALEN  
 Retourneert radialen als een numerieke expressie in graden wordt ingevoerd.  
  
 **Syntaxis**  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld duurt een paar hoeken als invoer en retourneert de overeenkomstige waarden ervan Radiaal.  
  
```  
SELECT RADIANS(-45.01) AS r1, RADIANS(-181.01) AS r2, RADIANS(0) AS r3, RADIANS(0.1472738) AS r4, RADIANS(197.1099392) AS r5  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{  
       "r1": -0.7855726963226477,  
       "r2": -3.1592204790349356,  
       "r3": 0,  
       "r4": 0.0025704127119236249,  
       "r5": 3.4402174274458375  
   }]  
```  
  
####  <a name="bk_round"></a> AFRONDEN  
 Retourneert een numerieke waarde, afgerond naar het dichtstbijzijnde gehele getal.  
  
 **Syntaxis**  
  
```  
ROUND(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt de volgende positieve en negatieve getallen naar het dichtstbijzijnde gehele getal afgerond.  
  
```  
SELECT ROUND(2.4) AS r1, ROUND(2.6) AS r2, ROUND(2.5) AS r3, ROUND(-2.4) AS r4, ROUND(-2.6) AS r5  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{r1: 2, r2: 3, r3: 3, r4: -2, r5: -3}]  
```  
  
####  <a name="bk_sign"></a> MELD U  
 Retourneert de positief (+ 1), nul (0) of minteken (-1) van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
SIGN(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de waarden van de aanmelding van de getallen van -2 naar 2.  
  
```  
SELECT SIGN(-2) AS s1, SIGN(-1) AS s2, SIGN(0) AS s3, SIGN(1) AS s4, SIGN(2) AS s5  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{s1: -1, s2: -1, s3: 0, s4: 1, s5: 1}]  
```  
  
####  <a name="bk_sin"></a> SIN  
 Retourneert de trigonometrische sinus van de opgegeven hoek, in radialen, in de opgegeven expressie.  
  
 **Syntaxis**  
  
```  
SIN(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld berekent de SINUS van de opgegeven hoek.  
  
```  
SELECT SIN(45.175643) AS sin  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"sin": 0.929607286611012}]  
```  
  
####  <a name="bk_sqrt"></a> WORTEL  
 Retourneert de vierkantswortel van de opgegeven numerieke waarde.  
  
 **Syntaxis**  
  
```  
SQRT(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de vierkantswortel van getallen 1-3.  
  
```  
SELECT SQRT(1) AS s1, SQRT(2.0) AS s2, SQRT(3) AS s3  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{s1: 1, s2: 1.4142135623730952, s3: 1.7320508075688772}]  
```  
  
####  <a name="bk_square"></a> VIERKANT  
 Retourneert het kwadraat van de opgegeven numerieke waarde.  
  
 **Syntaxis**  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de kwadraten van getallen 1-3.  
  
```  
SELECT SQUARE(1) AS s1, SQUARE(2.0) AS s2, SQUARE(3) AS s3  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{s1: 1, s2: 4, s3: 9}]  
```  
  
####  <a name="bk_tan"></a> TAN  
 Retourneert de tangens van de opgegeven hoek in radialen in de opgegeven expressie.  
  
 **Syntaxis**  
  
```  
TAN (<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld berekent de tangens van PI () / 2.  
  
```  
SELECT TAN(PI()/2) AS tan 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"tan": 16331239353195370 }]  
```  
  
####  <a name="bk_trunc"></a> GEHEEL  
 Retourneert een numerieke waarde, afgekapt tot het dichtstbijzijnde gehele getal.  
  
 **Syntaxis**  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt de volgende positieve en negatieve getallen naar het dichtstbijzijnde gehele getal afgekapt.  
  
```  
SELECT TRUNC(2.4) AS t1, TRUNC(2.6) AS t2, TRUNC(2.5) AS t3, TRUNC(-2.4) AS t4, TRUNC(-2.6) AS t5  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{t1: 2, t2: 2, t3: 2, t4: -2, t5: -2}]  
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
  
- `expression`  
  
   Een geldige expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_ARRAY.  
  
```  
SELECT   
 IS_ARRAY(true) AS isArray1,   
 IS_ARRAY(1) AS isArray2,  
 IS_ARRAY("value") AS isArray3,  
 IS_ARRAY(null) AS isArray4,  
 IS_ARRAY({prop: "value"}) AS isArray5,   
 IS_ARRAY([1, 2, 3]) AS isArray6,  
 IS_ARRAY({prop: "value"}.prop2) AS isArray7  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"isArray1":false,"isArray2":false,"isArray3":false,"isArray4":false,"isArray5":false,"isArray6":true,"isArray7":false}]
```  
  
####  <a name="bk_is_bool"></a> IS_BOOL  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een Booleaanse waarde is.  
  
 **Syntaxis**  
  
```  
IS_BOOL(<expression>)  
```  
  
 **Argumenten**  
  
- `expression`  
  
   Een geldige expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_BOOL.  
  
```  
SELECT   
    IS_BOOL(true) AS isBool1,   
    IS_BOOL(1) AS isBool2,  
    IS_BOOL("value") AS isBool3,   
    IS_BOOL(null) AS isBool4,  
    IS_BOOL({prop: "value"}) AS isBool5,   
    IS_BOOL([1, 2, 3]) AS isBool6,  
    IS_BOOL({prop: "value"}.prop2) AS isBool7  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"isBool1":true,"isBool2":false,"isBool3":false,"isBool4":false,"isBool5":false,"isBool6":false,"isBool7":false}]
```  
  
####  <a name="bk_is_defined"></a> IS_DEFINED  
 Retourneert een Booleaanse waarde die aangeeft of aan de eigenschap een waarde is toegewezen.  
  
 **Syntaxis**  
  
```  
IS_DEFINED(<expression>)  
```  
  
 **Argumenten**  
  
- `expression`  
  
   Een geldige expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt gecontroleerd op de aanwezigheid van een eigenschap binnen het opgegeven JSON-document. De eerste retourneert ' True ', omdat "a" aanwezig is, maar de tweede ' false ' retourneert omdat "b" afwezig is.  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a) AS isDefined1, IS_DEFINED({ "a" : 5 }.b) AS isDefined2 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"isDefined1":true,"isDefined2":false}]  
```  
  
####  <a name="bk_is_null"></a> IS_NULL  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie null is.  
  
 **Syntaxis**  
  
```  
IS_NULL(<expression>)  
```  
  
 **Argumenten**  
  
- `expression`  
  
   Een geldige expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_NULL.  
  
```  
SELECT   
    IS_NULL(true) AS isNull1,   
    IS_NULL(1) AS isNull2,  
    IS_NULL("value") AS isNull3,   
    IS_NULL(null) AS isNull4,  
    IS_NULL({prop: "value"}) AS isNull5,   
    IS_NULL([1, 2, 3]) AS isNull6,  
    IS_NULL({prop: "value"}.prop2) AS isNull7  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"isNull1":false,"isNull2":false,"isNull3":false,"isNull4":true,"isNull5":false,"isNull6":false,"isNull7":false}]
```  
  
####  <a name="bk_is_number"></a> IS_NUMBER  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een getal is.  
  
 **Syntaxis**  
  
```  
IS_NUMBER(<expression>)  
```  
  
 **Argumenten**  
  
- `expression`  
  
   Een geldige expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_NULL.  
  
```  
SELECT   
    IS_NUMBER(true) AS isNum1,   
    IS_NUMBER(1) AS isNum2,  
    IS_NUMBER("value") AS isNum3,   
    IS_NUMBER(null) AS isNum4,  
    IS_NUMBER({prop: "value"}) AS isNum5,   
    IS_NUMBER([1, 2, 3]) AS isNum6,  
    IS_NUMBER({prop: "value"}.prop2) AS isNum7  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"isNum1":false,"isNum2":true,"isNum3":false,"isNum4":false,"isNum5":false,"isNum6":false,"isNum7":false}]  
```  
  
####  <a name="bk_is_object"></a> IS_OBJECT  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een JSON-object is.  
  
 **Syntaxis**  
  
```  
IS_OBJECT(<expression>)  
```  
  
 **Argumenten**  
  
- `expression`  
  
   Een geldige expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_OBJECT.  
  
```  
SELECT   
    IS_OBJECT(true) AS isObj1,   
    IS_OBJECT(1) AS isObj2,  
    IS_OBJECT("value") AS isObj3,   
    IS_OBJECT(null) AS isObj4,  
    IS_OBJECT({prop: "value"}) AS isObj5,   
    IS_OBJECT([1, 2, 3]) AS isObj6,  
    IS_OBJECT({prop: "value"}.prop2) AS isObj7  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"isObj1":false,"isObj2":false,"isObj3":false,"isObj4":false,"isObj5":true,"isObj6":false,"isObj7":false}]
```  
  
####  <a name="bk_is_primitive"></a> IS_PRIMITIVE  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een primitieve nemen is (string, Boolean, numerieke of null).  
  
 **Syntaxis**  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 **Argumenten**  
  
- `expression`  
  
   Een geldige expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_PRIMITIVE.  
  
```  
SELECT   
           IS_PRIMITIVE(true) AS isPrim1,   
           IS_PRIMITIVE(1) AS isPrim2,  
           IS_PRIMITIVE("value") AS isPrim3,   
           IS_PRIMITIVE(null) AS isPrim4,  
           IS_PRIMITIVE({prop: "value"}) AS isPrim5,   
           IS_PRIMITIVE([1, 2, 3]) AS isPrim6,  
           IS_PRIMITIVE({prop: "value"}.prop2) AS isPrim7  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"isPrim1": true, "isPrim2": true, "isPrim3": true, "isPrim4": true, "isPrim5": false, "isPrim6": false, "isPrim7": false}]  
```  
  
####  <a name="bk_is_string"></a> IS_STRING  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een tekenreeks is.  
  
 **Syntaxis**  
  
```  
IS_STRING(<expression>)  
```  
  
 **Argumenten**  
  
- `expression`  
  
   Een geldige expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_STRING.  
  
```  
SELECT   
       IS_STRING(true) AS isStr1,   
       IS_STRING(1) AS isStr2,  
       IS_STRING("value") AS isStr3,   
       IS_STRING(null) AS isStr4,  
       IS_STRING({prop: "value"}) AS isStr5,   
       IS_STRING([1, 2, 3]) AS isStr6,  
       IS_STRING({prop: "value"}.prop2) AS isStr7  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"isStr1":false,"isStr2":false,"isStr3":true,"isStr4":false,"isStr5":false,"isStr6":false,"isStr7":false}] 
```  
  
###  <a name="bk_string_functions"></a> Tekenreeksfuncties  
 Met de volgende scalaire functies wordt een bewerking op een tekenreeksinvoerwaarde uitgevoerd en een tekenreeks, numerieke waarde of Booleaanse waarde geretourneerd.  
  
||||  
|-|-|-|  
|[CONCAT](#bk_concat)|[BEVAT](#bk_contains)|[ENDSWITH](#bk_endswith)|  
|[INDEX_OF](#bk_index_of)|[LEFT](#bk_left)|[LENGTE](#bk_length)|  
|[LAGERE](#bk_lower)|[LTRIM](#bk_ltrim)|[REPLACE](#bk_replace)|  
|[REPLICEREN](#bk_replicate)|[REVERSE](#bk_reverse)|[RIGHT](#bk_right)|  
|[RTRIM](#bk_rtrim)|[STARTSWITH](#bk_startswith)|[StringToArray](#bk_stringtoarray)|
|[StringToBoolean](#bk_stringtoboolean)|[StringToNull](#bk_stringtonull)|[StringToNumber](#bk_stringtonumber)|
|[StringToObject](#bk_stringtoobject)|[DE SUBTEKENREEKS](#bk_substring)|[ToString](#bk_tostring)|
|[TRIM](#bk_trim)|[BOVENSTE](#bk_upper)||
  
####  <a name="bk_concat"></a> CONCAT  
 Retourneert een tekenreeks die het resultaat is van het samenvoegen van twee of meer tekenreekswaarden.  
  
 **Syntaxis**  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de samengevoegde tekenreeks van de opgegeven waarden.  
  
```  
SELECT CONCAT("abc", "def") AS concat  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"concat": "abcdef"}  
```  
  
####  <a name="bk_contains"></a> BEVAT  
 Retourneert een Booleaanse waarde die aangeeft of de eerste tekenreeksexpressie de tweede bevat.  
  
 **Syntaxis**  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld controleert als "abc" bevat "ab" en "d".  
  
```  
SELECT CONTAINS("abc", "ab") AS c1, CONTAINS("abc", "d") AS c2 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"c1": true, "c2": false}]  
```  
  
####  <a name="bk_endswith"></a> ENDSWITH  
 Retourneert een Booleaanse waarde die aangeeft of de eerste expressie tekenreeks eindigt met de tweede.  
  
 **Syntaxis**  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert dat de "abc" eindigt op "b" en 'bc'.  
  
```  
SELECT ENDSWITH("abc", "b") AS e1, ENDSWITH("abc", "bc") AS e2 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"e1": false, "e2": true}]  
```  
  
####  <a name="bk_index_of"></a> INDEX_OF  
 Retourneert de beginpositie van het eerste exemplaar van de tweede tekenreeksexpressie binnen de eerste opgegeven tekenreeksexpressie, of -1 als de tekenreeks niet is gevonden.  
  
 **Syntaxis**  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt de index van verschillende subtekenreeksen in "abc".  
  
```  
SELECT INDEX_OF("abc", "ab") AS i1, INDEX_OF("abc", "b") AS i2, INDEX_OF("abc", "c") AS i3 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"i1": 0, "i2": 1, "i3": -1}]  
```  
  
####  <a name="bk_left"></a> LINKS  
 Retourneert het linkerdeel van een tekenreeks met het opgegeven aantal tekens.  
  
 **Syntaxis**  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
- `num_expr`  
  
   Een geldige numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt het linkerdeel van "abc" voor verschillende waarden voor lengte.  
  
```  
SELECT LEFT("abc", 1) AS l1, LEFT("abc", 2) AS l2 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"l1": "a", "l2": "ab"}]  
```  
  
####  <a name="bk_length"></a> LENGTE  
 Retourneert het aantal tekens van de opgegeven tekenreeksexpressie.  
  
 **Syntaxis**  
  
```  
LENGTH(<str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de lengte van een tekenreeks.  
  
```  
SELECT LENGTH("abc") AS len 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"len": 3}]  
```  
  
####  <a name="bk_lower"></a> LAGERE  
 Retourneert een tekenreeksexpressie na het converteren van tekens in hoofdletters naar kleine letters.  
  
 **Syntaxis**  
  
```  
LOWER(<str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u in een query lager gebruiken.  
  
```  
SELECT LOWER("Abc") AS lower
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"lower": "abc"}]  
  
```  
  
####  <a name="bk_ltrim"></a> LTRIM  
 Retourneert een tekenreeksexpressie na het verwijderen van lege voorlooptekens.  
  
 **Syntaxis**  
  
```  
LTRIM(<str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u LTRIM binnen een query gebruiken.  
  
```  
SELECT LTRIM("  abc") AS l1, LTRIM("abc") AS l2, LTRIM("abc   ") AS l3 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"l1": "abc", "l2": "abc", "l3": "abc   "}]  
```  
  
####  <a name="bk_replace"></a> VERVANGEN  
 Vervangt alle exemplaren van een opgegeven tekenreekswaarde door een andere tekenreekswaarde.  
  
 **Syntaxis**  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u vervangen in een query.  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk") AS replace 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"replace": "This is a desk"}]  
```  
  
####  <a name="bk_replicate"></a> REPLICEREN  
 Herhaalt een tekenreekswaarde een opgegeven aantal keren.  
  
 **Syntaxis**  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
- `num_expr`  
  
   Een geldige numerieke expressie is. Als num_expr negatief of niet-begrensde is, is het resultaat is niet gedefinieerd.

  > [!NOTE]
  > De maximale lengte van het resultaat is 10.000 tekens dat wil zeggen (length(str_expr) * num_expr) < = 10.000.
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u kunt REPLICEREN in een query gebruiken.  
  
```  
SELECT REPLICATE("a", 3) AS replicate  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"replicate": "aaa"}]  
```  
  
####  <a name="bk_reverse"></a> OMKEREN  
 Retourneert een tekenreekswaarde in de omgekeerde volgorde.  
  
 **Syntaxis**  
  
```  
REVERSE(<str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld laat zien hoe REVERSE gebruiken in een query.  
  
```  
SELECT REVERSE("Abc") AS reverse  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"reverse": "cbA"}]  
```  
  
####  <a name="bk_right"></a> RECHTS  
 Retourneert het rechterdeel van een tekenreeks met het opgegeven aantal tekens.  
  
 **Syntaxis**  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
- `num_expr`  
  
   Een geldige numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt het rechterdeel van "abc" voor verschillende waarden voor lengte.  
  
```  
SELECT RIGHT("abc", 1) AS r1, RIGHT("abc", 2) AS r2 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"r1": "c", "r2": "bc"}]  
```  
  
####  <a name="bk_rtrim"></a> RTRIM  
 Retourneert een tekenreeksexpressie na het afsluitende spaties verwijderen.  
  
 **Syntaxis**  
  
```  
RTRIM(<str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u RTRIM binnen een query gebruiken.  
  
```  
SELECT RTRIM("  abc") AS r1, RTRIM("abc") AS r2, RTRIM("abc   ") AS r3  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"r1": "   abc", "r2": "abc", "r3": "abc"}]  
```  
  
####  <a name="bk_startswith"></a> STARTSWITH  
 Retourneert een Booleaanse waarde die aangeeft of de eerste expressie tekenreeks begint met de tweede.  
  
 **Syntaxis**  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt als de tekenreeks "abc" met "b begint" en "a".  
  
```  
SELECT STARTSWITH("abc", "b") AS s1, STARTSWITH("abc", "a") AS s2  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"s1": false, "s2": true}]  
```  

  ####  <a name="bk_stringtoarray"></a> StringToArray  
 Expressie vertaald naar een matrix retourneert. Als de expressie kan niet worden vertaald, retourneert niet-gedefinieerde.  
  
 **Syntaxis**  
  
```  
StringToArray(<expr>)  
```  
  
 **Argumenten**  
  
- `expr`  
  
   Is geldige scalaire expressie die moet worden geëvalueerd als een JSON-matrix-expressie. Houd er rekening mee dat geneste tekenreekswaarden moeten zijn geschreven met dubbele aanhalingstekens om geldig te zijn. Zie voor meer informatie over de JSON-indeling, [json.org](https://json.org/)
  
  **Typen retourneren**  
  
  Retourneert een matrixexpressie of een niet-gedefinieerde.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld laat zien hoe StringToArray gedraagt zich op verschillende typen. 
  
 Hier volgen enkele voorbeelden met geldige invoer.

```
SELECT 
    StringToArray('[]') AS a1, 
    StringToArray("[1,2,3]") AS a2,
    StringToArray("[\"str\",2,3]") AS a3,
    StringToArray('[["5","6","7"],["8"],["9"]]') AS a4,
    StringToArray('[1,2,3, "[4,5,6]",[7,8]]') AS a5
```

Hier volgt de resultatenset.

```
[{"a1": [], "a2": [1,2,3], "a3": ["str",2,3], "a4": [["5","6","7"],["8"],["9"]], "a5": [1,2,3,"[4,5,6]",[7,8]]}]
```

Hier volgt een voorbeeld van ongeldige invoer. 
   
 Enkele aanhalingstekens binnen de matrix zijn geen geldige JSON.
Hoewel ze geldig in een query zijn, worden ze niet parseren naar geldige matrices. Tekenreeksen in de matrix-tekenreeks moeten ofwel worden weergegeven "[\\"\\"] ' of de omringende offerte moet één ' [" "]'.

```
SELECT
    StringToArray("['5','6','7']")
```

Hier volgt de resultatenset.

```
[{}]
```

Hier volgen enkele voorbeelden van ongeldige invoer.
   
 De expressie doorgegeven zal worden geparseerd als een JSON-matrix. het volgende doen niet evalueren voor het type matrix en dus retourneren niet gedefinieerd.
   
```
SELECT
    StringToArray("["),
    StringToArray("1"),
    StringToArray(NaN),
    StringToArray(false),
    StringToArray(undefined)
```

Hier volgt de resultatenset.

```
[{}]
```

####  <a name="bk_stringtoboolean"></a> StringToBoolean  
 Expressie vertaald naar een Booleaanse waarde retourneert. Als de expressie kan niet worden vertaald, retourneert niet-gedefinieerde.  
  
 **Syntaxis**  
  
```  
StringToBoolean(<expr>)  
```  
  
 **Argumenten**  
  
- `expr`  
  
   Is geldige scalaire expressie die moet worden geëvalueerd als een Boole-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie of een niet-gedefinieerde.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld laat zien hoe StringToBoolean gedraagt zich op verschillende typen. 
 
 Hier volgen enkele voorbeelden met geldige invoer.

Witruimte mag alleen vóór of na 'true '/ ' false'.

```  
SELECT 
    StringToBoolean("true") AS b1, 
    StringToBoolean("    false") AS b2,
    StringToBoolean("false    ") AS b3
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"b1": true, "b2": false, "b3": false}]
```  

Hier volgen enkele voorbeelden met ongeldige invoer.

 Booleaanse waarden zijn hoofdlettergevoelig en moeten worden geschreven met alle kleine letters, dat wil zeggen 'true' en 'false'.

```  
SELECT 
    StringToBoolean("TRUE"),
    StringToBoolean("False")
```  

Hier volgt de resultatenset.  
  
```  
[{}]
``` 

De expressie doorgegeven zal worden geparseerd als een Booleaanse expressie; deze invoer niet geëvalueerd voor het type Boolean en dus retourneren niet gedefinieerd.

```  
SELECT 
    StringToBoolean("null"),
    StringToBoolean(undefined),
    StringToBoolean(NaN), 
    StringToBoolean(false), 
    StringToBoolean(true)
```  

Hier volgt de resultatenset.  
  
```  
[{}]
```  

####  <a name="bk_stringtonull"></a> StringToNull  
 Expressie vertaald naar null retourneert. Als de expressie kan niet worden vertaald, retourneert niet-gedefinieerde.  
  
 **Syntaxis**  
  
```  
StringToNull(<expr>)  
```  
  
 **Argumenten**  
  
- `expr`  
  
   Is geldige scalaire expressie die moet worden geëvalueerd als een null-expressie.
  
  **Typen retourneren**  
  
  Retourneert een null-expressie of een niet-gedefinieerde.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld laat zien hoe StringToNull gedraagt zich op verschillende typen. 

Hier volgen enkele voorbeelden met geldige invoer.

 Witruimte mag alleen vóór of na 'null'.

```  
SELECT 
    StringToNull("null") AS n1, 
    StringToNull("  null ") AS n2,
    IS_NULL(StringToNull("null   ")) AS n3
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"n1": null, "n2": null, "n3": true}]
```  

Hier volgen enkele voorbeelden met ongeldige invoer.

Null is hoofdlettergevoelig en moet worden geschreven met alle kleine letters dat wil zeggen 'null'.

```  
SELECT    
    StringToNull("NULL"),
    StringToNull("Null")
```  
  
 Hier volgt de resultatenset.  
  
```  
[{}]
```  

De expressie doorgegeven zal worden geparseerd als een null-expressie. deze invoer niet geëvalueerd voor het type null en dus retourneren niet gedefinieerd.

```  
SELECT    
    StringToNull("true"), 
    StringToNull(false), 
    StringToNull(undefined),
    StringToNull(NaN) 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{}]
```  

####  <a name="bk_stringtonumber"></a> StringToNumber  
 Expressie vertaald naar een getal retourneert. Als de expressie kan niet worden vertaald, retourneert niet-gedefinieerde.  
  
 **Syntaxis**  
  
```  
StringToNumber(<expr>)  
```  
  
 **Argumenten**  
  
- `expr`  
  
   Is geldige scalaire expressie die moet worden geëvalueerd als een getal van de JSON-expressie. Getallen in JSON moet een geheel getal of een drijvende komma zijn. Zie voor meer informatie over de JSON-indeling, [json.org](https://json.org/)  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie of een niet-gedefinieerde.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld laat zien hoe StringToNumber gedraagt zich op verschillende typen. 

Witruimte mag alleen vóór of na het getal.

```  
SELECT 
    StringToNumber("1.000000") AS num1, 
    StringToNumber("3.14") AS num2,
    StringToNumber("   60   ") AS num3, 
    StringToNumber("-1.79769e+308") AS num4
```  
  
 Hier volgt de resultatenset.  
  
```  
{{"num1": 1, "num2": 3.14, "num3": 60, "num4": -1.79769e+308}}
```  

Worden in de JSON die een geldig getal ofwel moet een geheel getal of een drijvende-kommagetal zijn.

```  
SELECT   
    StringToNumber("0xF")
```  
  
 Hier volgt de resultatenset.  
  
```  
{{}}
```  

De expressie doorgegeven zal worden geparseerd als een numerieke expressie. deze invoer niet geëvalueerd typt nummer en dus terugkeren niet gedefinieerd. 

```  
SELECT 
    StringToNumber("99     54"),   
    StringToNumber(undefined),
    StringToNumber("false"),
    StringToNumber(false),
    StringToNumber(" "),
    StringToNumber(NaN)
```  
  
 Hier volgt de resultatenset.  
  
```  
{{}}
```  

####  <a name="bk_stringtoobject"></a> StringToObject  
 Expressie vertaald naar een Object geretourneerd. Als de expressie kan niet worden vertaald, retourneert niet-gedefinieerde.  
  
 **Syntaxis**  
  
```  
StringToObject(<expr>)  
```  
  
 **Argumenten**  
  
- `expr`  
  
   Is geldige scalaire expressie die moet worden geëvalueerd als een JSON-object-expressie. Houd er rekening mee dat geneste tekenreekswaarden moeten zijn geschreven met dubbele aanhalingstekens om geldig te zijn. Zie voor meer informatie over de JSON-indeling, [json.org](https://json.org/)  
  
  **Typen retourneren**  
  
  Retourneert een objectexpressie of een niet-gedefinieerde.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld laat zien hoe StringToObject gedraagt zich op verschillende typen. 
  
 Hier volgen enkele voorbeelden met geldige invoer.

``` 
SELECT 
    StringToObject("{}") AS obj1, 
    StringToObject('{"A":[1,2,3]}') AS obj2,
    StringToObject('{"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]}') AS obj3, 
    StringToObject("{\"C\":[{\"c1\":[5,6,7]},{\"c2\":8},{\"c3\":9}]}") AS obj4
``` 

Hier volgt de resultatenset.

```
[{"obj1": {}, 
  "obj2": {"A": [1,2,3]}, 
  "obj3": {"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]},
  "obj4": {"C":[{"c1":[5,6,7]},{"c2":8},{"c3":9}]}}]
```

 Hier volgen enkele voorbeelden met ongeldige invoer.
Hoewel ze geldig in een query zijn, worden ze niet parseren naar geldige objecten. Tekenreeksen in de tekenreeks van object ofwel moeten worden weergegeven ' {\\"een\\":\\"str\\'} ' of de omringende offerte moet één ' {"a": 'str'}'.

Enkele aanhalingstekens rond de namen van eigenschappen zijn niet geldig JSON.

``` 
SELECT 
    StringToObject("{'a':[1,2,3]}")
```

Hier volgt de resultatenset.

```  
[{}]
```  

Eigenschapnamen zonder omringende aanhalingstekens zijn niet geldig JSON.

``` 
SELECT 
    StringToObject("{a:[1,2,3]}")
```

Hier volgt de resultatenset.

```  
[{}]
``` 

Hier volgen enkele voorbeelden met ongeldige invoer.

 De expressie doorgegeven zal worden geparseerd als een JSON-object. deze invoer niet geëvalueerd voor het type object en dus retourneren niet gedefinieerd.

``` 
SELECT 
    StringToObject("}"),
    StringToObject("{"),
    StringToObject("1"),
    StringToObject(NaN), 
    StringToObject(false), 
    StringToObject(undefined)
``` 
 
 Hier volgt de resultatenset.

```
[{}]
```

####  <a name="bk_substring"></a> DE SUBTEKENREEKS  
 Onderdeel van een tekenreeksexpressie vanaf de op nul gebaseerde positie van het opgegeven teken geretourneerd en blijft aan de opgegeven lengte of aan het einde van de tekenreeks.  
  
 **Syntaxis**  
  
```  
SUBSTRING(<str_expr>, <num_expr>, <num_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
- `num_expr`  
  
   Is een geldige numerieke expressie om de begin- en -teken aan te geven.    
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de subtekenreeks van "abc" beginnen op 1 en een lengte van 1 teken bevatten.  
  
```  
SELECT SUBSTRING("abc", 1, 1) AS substring  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"substring": "b"}]  
```  
####  <a name="bk_tostring"></a> ToString  
 Retourneert een tekenreeksrepresentatie van de scalaire expressie. 
  
 **Syntaxis**  
  
```  
ToString(<expr>)
```  
  
 **Argumenten**  
  
- `expr`  
  
   Is geldige scalaire expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld laat zien hoe ToString gedraagt zich op verschillende typen.   
  
```  
SELECT 
    ToString(1.0000) AS str1, 
    ToString("Hello World") AS str2, 
    ToString(NaN) AS str3, 
    ToString(Infinity) AS str4,
    ToString(IS_STRING(ToString(undefined))) AS str5, 
    ToString(0.1234) AS str6, 
    ToString(false) AS str7, 
    ToString(undefined) AS str8
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"str1": "1", "str2": "Hello World", "str3": "NaN", "str4": "Infinity", "str5": "false", "str6": "0.1234", "str7": "false"}]  
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
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u TRIM binnen een query.  
  
```  
SELECT TRIM("   abc") AS t1, TRIM("   abc   ") AS t2, TRIM("abc   ") AS t3, TRIM("abc") AS t4
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"t1": "abc", "t2": "abc", "t3": "abc", "t4": "abc"}]  
``` 
####  <a name="bk_upper"></a> BOVENSTE  
 Retourneert een tekenreeksexpressie na het converteren van tekens in kleine letters naar hoofdletters.  
  
 **Syntaxis**  
  
```  
UPPER(<str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld laat zien hoe het gebruik van hoofdletters in een query  
  
```  
SELECT UPPER("Abc") AS upper  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"upper": "ABC"}]  
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
  
- `arr_expr`  
  
   Is een geldige matrix-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een matrixexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld over het samenvoegen van twee matrices.  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"]) AS arrayConcat 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"arrayConcat": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <a name="bk_array_contains"></a> ARRAY_CONTAINS  
Retourneert een Booleaanse waarde die aangeeft of de matrix de opgegeven waarde bevat. U kunt een gedeeltelijke of volledige overeenkomst van een object controleren met behulp van een Booleaanse expressie in de opdracht. 

**Syntaxis**  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr> [, bool_expr])  
```  
  
 **Argumenten**  
  
- `arr_expr`  
  
   Is een geldige matrix-expressie.  
  
- `expr`  
  
   Een geldige expressie is.  

- `bool_expr`  
  
   Een Booleaanse expressie is. Als deze ingesteld op ' true'and als de opgegeven waarde een object is, de opdracht wordt gecontroleerd voor een gedeeltelijke overeenkomst (de search-object is een subset van een van de objecten). Als deze ingesteld op 'false', controleert de opdracht een volledige overeenkomst van alle objecten in de matrix. De standaardwaarde indien niet opgegeven is ingesteld op false. 
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse waarde.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld hoe om te controleren voor lidmaatschap in een matrix met behulp van ARRAY_CONTAINS.  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples") AS b1,  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes") AS b2  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"b1": true, "b2": false}]  
```  

Het volgende voorbeeld hoe om te controleren op een gedeeltelijke overeenkomst van een JSON-code in een matrix met behulp van ARRAY_CONTAINS.  
  
```  
SELECT  
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}, true) AS b1, 
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}) AS b2,
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "mangoes"}, true) AS b3 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{
  "b1": true,
  "b2": false,
  "b3": false
}] 
```  
  
####  <a name="bk_array_length"></a> ARRAY_LENGTH  
 Retourneert het aantal elementen van de opgegeven matrixexpressie.  
  
 **Syntaxis**  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 **Argumenten**  
  
- `arr_expr`  
  
   Is een geldige matrix-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld over het verkrijgen van de lengte van een matrix met behulp van ARRAY_LENGTH.  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"]) AS len  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"len": 3}]  
```  
  
####  <a name="bk_array_slice"></a> ARRAY_SLICE  
 Retourneert een deel van een matrixexpressie.
  
 **Syntaxis**  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Argumenten**  
  
- `arr_expr`  
  
   Is een geldige matrix-expressie.  
  
- `num_expr`  
  
   Numerieke op nul gebaseerde index op waarop u wilt beginnen met de matrix. Negatieve waarden kunnen worden gebruikt om op te geven van de startIndex ten opzichte van het laatste element van de matrix-dat wil zeggen 1 verwijst naar het laatste element in de matrix.  

- `num_expr`  

   Maximum aantal elementen in de resulterende matrix.    

  **Typen retourneren**  
  
  Retourneert een matrixexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u verschillende segmenten van een matrix met behulp van ARRAY_SLICE.  
  
```  
SELECT   
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1) AS s1,  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1) AS s2,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 1) AS s3,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 2) AS s4,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 0) AS s5,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1000) AS s6,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, -100) AS s7      
  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{  
           "s1": ["strawberries", "bananas"],   
           "s2": ["strawberries"],
           "s3": ["strawberries"],  
           "s4": ["strawberries", "bananas"], 
           "s5": [],
           "s6": ["strawberries", "bananas"],
           "s7": [] 
}]  
```  

###  <a name="bk_date_and_time_functions"></a> Datum- en tijdfuncties
 De volgende scalaire functies kunnen u de huidige UTC-datum en tijd in twee vormen; een numerieke tijdstempel waarvan de waarde is de Unix-epoche in milliseconden of als een tekenreeks die aan de ISO 8601-notatie voldoet. 

|||
|-|-|
|[GetCurrentDateTime](#bk_get_current_date_time)|[GetCurrentTimestamp](#bk_get_current_timestamp)||

####  <a name="bk_get_current_date_time"></a> GetCurrentDateTime
 Retourneert de huidige UTC-datum en tijd als een ISO 8601-tekenreeks.
  
 **Syntaxis**
  
```
GetCurrentDateTime ()
```
  
  **Typen retourneren**
  
  Retourneert de huidige UTC datum en tijd ISO 8601 string-waarde. 

  Dit wordt uitgedrukt in de indeling JJJJ-MM-DDThh:mm:ss.sssZ waar:
  
  |||
  |-|-|
  |JJJJ|jaar in vier cijfers|
  |MM|maand in twee cijfers (01 = januari, enz.)|
  |DD|twee cijfers dag van maand (01 tot en met 31)|
  |T|signifier voor begin van de tijdselementen|
  |hh|twee cijfers uur (00 en 23)|
  |mm|twee cijfers minuten (00 en 59)|
  |ss|twee cijfers seconden (00 en 59)|
  |.sss|drie cijfers van decimalen van een seconde|
  |Z|UTC (Coordinated Universal Time) designator||
  
  Zie voor meer informatie over de ISO 8601-notatie [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601)

  **Opmerkingen**

  GetCurrentDateTime is een niet-deterministische functie. 
  
  Het resultaat wordt UTC (Coordinated Universal Time).

  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u aan de huidige UTC-datum tijd met de ingebouwde functie GetCurrentDateTime.
  
```  
SELECT GetCurrentDateTime() AS currentUtcDateTime
```  
  
 Hier volgt een voorbeeld van de resultatenset.
  
```  
[{
  "currentUtcDateTime": "2019-05-03T20:36:17.784Z"
}]  
```  

####  <a name="bk_get_current_timestamp"></a> GetCurrentTimestamp
 Retourneert het aantal milliseconden dat is verstreken sinds 00:00:00 donderdag 1 januari 1970. 
  
 **Syntaxis**  
  
```  
GetCurrentTimestamp ()  
```  
  
  **Typen retourneren**  
  
  Retourneert een numerieke waarde, het huidige aantal milliseconden dat is verstreken sinds de Unix-epoche dat wil zeggen het aantal milliseconden dat is verstreken sinds 00:00:00 donderdag 1 januari 1970.

  **Opmerkingen**

  GetCurrentTimestamp is een niet-deterministische functie. 
  
  Het resultaat wordt UTC (Coordinated Universal Time).

  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u aan de huidige tijdstempel met de ingebouwde functie GetCurrentTimestamp.
  
```  
SELECT GetCurrentTimestamp() AS currentUtcTimestamp
```  
  
 Hier volgt een voorbeeld van de resultatenset.
  
```  
[{
  "currentUtcTimestamp": 1556916469065
}]  
```  

###  <a name="bk_spatial_functions"></a> Ruimtelijke-functies  
 De volgende scalaire functies uitvoeren van een bewerking op een invoerwaarde ruimtelijke index en een numerieke of Booleaanse waarde retourneren.  
  
|||||
|-|-|-|-|
|[ST_DISTANCE](#bk_st_distance)|[ST_WITHIN](#bk_st_within)|[ST_INTERSECTS](#bk_st_intersects)|[ST_ISVALID](#bk_st_isvalid)|
|[ST_ISVALIDDETAILED](#bk_st_isvaliddetailed)||||
  
####  <a name="bk_st_distance"></a> ST_DISTANCE  
 Retourneert de afstand tussen de twee GeoJSON Point-, Polygon- of LineString-expressies.  
  
 **Syntaxis**  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumenten**  
  
- `spatial_expr`  
  
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
  
- `spatial_expr`  
  
   Is geldige expressie voor GeoJSON-punt, Polygon of LineString-object.  
 
- `spatial_expr`  
  
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
  
- `spatial_expr`  
  
   Is geldige expressie voor GeoJSON-punt, Polygon of LineString-object.  
 
- `spatial_expr`  
  
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
  
- `spatial_expr`  
  
   Is geldige GeoJSON-punt, Polygon of LineString-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld laat zien hoe om te controleren of een punt geldig met behulp van ST_VALID is.  
  
  Bijvoorbeeld, heeft dit punt een Breedtegraadwaarde die zich niet in het geldige waardenbereik [tussen-90 en, 90], zodat de query retourneert ' false '.  
  
  Voor polygonen, de GeoJSON-specificatie is vereist dat de opgegeven combinatie van laatste coördinaat moet hetzelfde zijn als de eerste pagina, het maken van een gesloten vorm. Punten in een polygoon moeten worden opgegeven in volgorde van linksom draaien. Een veelhoek in rechtsom volgorde opgegeven vertegenwoordigt de omgekeerde waarde van de regio binnen het.  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] }) AS b 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{ "b": false }]  
```  
  
####  <a name="bk_st_isvaliddetailed"></a> ST_ISVALIDDETAILED  
 Retourneert een JSON-waarde met een Booleaanse waarde die aangeeft of de opgegeven GeoJSON Point-, Polygon- of LineString-expressie geldig is. Als de expressie ongeldig is, worden ook de reden daarvoor en een tekenreekswaarde geretourneerd.  
  
 **Syntaxis**  
  
```  
ST_ISVALIDDETAILED(<spatial_expr>)  
```  
  
 **Argumenten**  
  
- `spatial_expr`  
  
   Een geldige GeoJSON-veelhoek-punt of de expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een JSON-waarde met een Booleaanse waarde als de opgegeven GeoJSON-veelhoek-punt of de expressie is geldig, en als ongeldig, ook de reden als een string-waarde.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld hoe u controleert de geldigheid (met gegevens) met behulp van ST_ISVALIDDETAILED.  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
}) AS b  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{  
  "b": {   
    "valid": false,   
    "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points."   
  }  
}]  
```  
 
## <a name="next-steps"></a>Volgende stappen  

- [SQL-syntaxis en SQL-query voor Cosmos DB](how-to-sql-query.md)

- [Documentatie voor cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)  
