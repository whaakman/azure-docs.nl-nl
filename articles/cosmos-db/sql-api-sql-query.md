---
title: SQL-query's voor Azure Cosmos DB | Microsoft Docs
description: Meer informatie over SQL-syntaxis, database-concepten en SQL-query's voor Azure Cosmos DB. SQL kan worden gebruikt als een JSON-query-taal in Azure Cosmos DB.
keywords: SQL-syntaxis, sql-query, sql-query's, querytaal voor json, database-concepten en sql-query's, statistische functies
services: cosmos-db
author: LalithaMV
manager: kfile
editor: monicar
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: laviswa
ms.openlocfilehash: ee804ddc9e8fe9901173bb3d9357a273ea28057d
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056814"
---
# <a name="sql-queries-for-azure-cosmos-db"></a>SQL-query's voor Azure Cosmos DB

Microsoft Azure Cosmos DB biedt ondersteuning voor documentquery met behulp van SQL (Structured Query Language) als een JSON-querytaal op SQL-API-accounts. Azure Cosmos DB is echt schema's. Omdat aan onze verplichting tot het JSON-gegevensmodel rechtstreeks in de database-engine biedt het automatisch indexeren van JSON-documenten zonder een expliciet schema of secundaire indexen worden gemaakt.

Tijdens het ontwerpen van de querytaal voor Cosmos DB, hadden we twee drie doelen voor ogen:

* In plaats van een nieuwe querytaal voor JSON vruchtbare, willen we ondersteuning voor SQL. SQL is een van de bekend en meest populaire querytalen. Cosmos DB SQL biedt een formele programmeermodel voor uitgebreide query's via JSON-documenten.
* Als een JSON-documentdatabase kan worden uitgevoerd van JavaScript direct in de database-engine, willen we de JavaScript-programmeermodel gebruikt als basis voor onze querytaal. De SQL-API is verankerd ligt in de JavaScript-typesysteem, evaluatie van de expressie en functieaanroepen. Deze beurt biedt een natuurlijke programmeermodel voor projecties van relationele, hiërarchische navigatie in JSON-documenten, self joins, ruimtelijke query's en aanroepen van de gebruiker gedefinieerde functies (UDF's) die zijn geschreven in JavaScript, onder andere volledig. 

Wij geloven dat deze mogelijkheden essentieel zijn voor het reduceren van de ergernis tussen de toepassing en de database en essentieel voor productiviteit van ontwikkelaars zijn.

We raden aan de slag met het bekijken van de volgende video, waarbij Azure Cosmos DB Program Manager Andrew Liu ziet u een query uitvoeren op Azure Cosmos DB-mogelijkheden en ziet u de online [testomgeving voor Query's](http://www.documentdb.com/sql/demo), waar u Azure kunt uitproberen Cosmos DB en voer SQL-query's op onze gegevensset zoals in de video wordt gedemonstreerd.

> [!VIDEO https://www.youtube.com/embed/1LqUQRpHfFI]
>
>

Meer geavanceerde query-technieken worden getoond in deze opvolgen video:

> [!VIDEO https://www.youtube.com/embed/kASU9NOIR90]
>
>

Keer vervolgens terug naar dit artikel, waar we beginnen met een SQL-query-zelfstudie maakt u kennis met enkele eenvoudige JSON-documenten en de SQL-opdrachten.

## <a id="GettingStarted"></a>Aan de slag met SQL-opdrachten in Cosmos DB
Als u wilt zien Cosmos DB SQL op het werk, laten we beginnen met een paar eenvoudige JSON-documenten en doorloop enkele eenvoudige query's op deze. Houd rekening met deze twee JSON-documenten over twee families. Met Cosmos DB hoeft er niet expliciet een schema of secundaire indexen maken. We gewoon wilt invoegen van de JSON-documenten in een Cosmos DB-verzameling en vervolgens op te vragen. Hier hebben we een eenvoudige JSON document voor de Andersen family, de bovenliggende, onderliggende items (en hun huisdieren)-adres en registratie-informatie. Het document heeft tekenreeksen, getallen, Booleaanse waarden, matrices en geneste eigenschappen. 

**Document**  

```JSON
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "parents": [
     { "firstName": "Thomas" },
     { "firstName": "Mary Kay"}
  ],
  "children": [
     {
         "firstName": "Henriette Thaulow", 
         "gender": "female", 
         "grade": 5,
         "pets": [{ "givenName": "Fluffy" }]
     }
  ],
  "address": { "state": "WA", "county": "King", "city": "seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

Hier volgt een tweede document met een subtiele verschil: `givenName` en `familyName` worden gebruikt in plaats van `firstName` en `lastName`.

**Document**  

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

Nu we gaan enkele query's op deze gegevens om te begrijpen van enkele van de belangrijkste aspecten van Azure Cosmos DB SQL-querytaal. De volgende query retourneert bijvoorbeeld de documenten waarin het id-veld gelijk `AndersenFamily`. Omdat het een `SELECT *`, de uitvoer van de query is de volledige JSON-document:

**Query**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Results**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


Bekijk nu het geval waarin we wilt formatteren van de JSON-uitvoer in een andere vorm. Deze query projecteert een nieuwe JSON-object met twee geselecteerde velden, naam en plaats, wanneer het adres stad dezelfde naam als de status heeft. In dit geval "NY, NY" komt overeen met.

**Query**    

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Results**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


De volgende query retourneert alle opgegeven namen van kinderen in de familie waarvan de id komt overeen met `WakefieldFamily` geordend op de plaats van wonen.

**Query**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Results**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


We willen graag de aandacht vestigen op een paar opmerkelijk aspecten van de querytaal van Cosmos DB met behulp van de voorbeelden die we tot nu toe hebben gezien:  

* Omdat SQL API op JSON-waarden werkt, behandelt structuur vormgegeven entiteiten in plaats van rijen en kolommen. Daarom de taal kunt u verwijzen naar de knooppunten van de structuur op elke willekeurige diepte zoals `Node1.Node2.Node3…..Nodem`, vergelijkbaar met de relationele SQL die verwijzen naar de twee onderdeelverwijzing van `<table>.<column>`.   
* De structured query language werkt met gegevens zonder schema. Daarom moet het typesysteem dynamisch worden gebonden. Dezelfde expressie kan verschillende typen op verschillende documenten opleveren. Het resultaat van een query is een geldige JSON-waarde, maar is niet noodzakelijkerwijs van een vast schema.  
* Cosmos DB biedt alleen ondersteuning voor strikte JSON-documenten. Dit betekent dat het systeem en voor expressies zijn beperkt tot alleen bekommeren om JSON-typen. Raadpleeg de [JSON-specificatie](http://www.json.org/) voor meer informatie.  
* Een Cosmos DB-verzameling is een container schema's van JSON-documenten. De relaties in gegevensentiteiten binnen en tussen documenten in een verzameling worden impliciet door containment en niet door de primaire sleutel en refererende sleutels relaties vastgelegd. Dit is een belangrijk aspect om te zeggen, aanleiding van de intra-document wordt verderop in dit artikel besproken.

## <a id="Indexing"></a> Cosmos DB indexeren
Voordat we tot de SQL-syntaxis krijgen, is het waard is om het ontwerp van de indexering in Azure Cosmos DB verkennen. 

Het doel van de indexen van de database is voor het bieden van query's in hun verschillende formulieren en vormen met minimale resourceverbruik (zoals CPU en input/output) terwijl met een goede doorvoer en lage latentie. De keuze van de juiste index voor query's in een database is vaak het geval is, vereist veel planning en experimenten. Deze aanpak is een uitdaging voor databases zonder schema waar de gegevens voldoen niet aan een strikt schema en zich snel verder ontwikkelt. 

Tijdens de indexering Cosmos DB-subsysteem, instellen we daarom de volgende doelen:

* Documenten te indexeren zonder schema: het subsysteem voor indexering niet vereisen dat de schema-informatie of geen veronderstellingen doen over het schema van de documenten. 
* Ondersteuning voor efficiënte, geavanceerde hiërarchische en relationele query's: de index biedt ondersteuning voor de Cosmos DB-querytaal efficiënt, inclusief ondersteuning voor hiërarchische en relationele projecties.
* Ondersteuning voor consistente-query's in face of een doorlopend volume van schrijfbewerkingen: voor hoge schrijven doorvoer workloads met een consistente-query's, de index incrementeel, efficiënt en online geval van een doorlopend volume van schrijfbewerkingen wordt bijgewerkt. De consistente index-update is van cruciaal belang voor het bieden van de query's op het consistentieniveau van de waarin de gebruiker de documentservice hebt geconfigureerd.
* Ondersteuning voor multitenancy: gegeven het model op basis van een reservering voor resourcebeheer voor tenants, index-updates worden uitgevoerd binnen het budget van systeembronnen (CPU, geheugen en i/o-bewerkingen per seconde) per replica toegewezen. 
* Opslagefficiëntie: voor de kosteneffectiviteit, de opslagoverhead op de schijf van de index is gebonden en voorspelbare. Dit is essentieel omdat Cosmos DB kan de ontwikkelaar moet dienen tussen index overhead ten opzichte van de prestaties van query's op basis van kosten.  

Raadpleeg de [voorbeelden van Azure Cosmos DB](https://github.com/Azure/azure-documentdb-net) op MSDN voor voorbeelden waarin wordt getoond hoe het indexeringsbeleid voor een verzameling configureren. Laten we nu toegang tot de details van de Azure Cosmos DB SQL-syntaxis.

## <a id="Basics"></a>Basisprincipes van een Azure Cosmos DB SQL-query
Elke query bestaat uit een SELECT-component en een optionele FROM en WHERE-componenten per ANSI SQL-standaarden. Normaal gesproken voor elke query moet de bron in de component FROM is geïnventariseerd. Het filter in de component WHERE wordt vervolgens toegepast op de bron om op te halen van een subset van JSON-documenten. Ten slotte wordt de SELECT-component gebruikt om de vereiste JSON-waarden in de selectielijst.

    SELECT <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a id="FromClause"></a>FROM-component
De `FROM <from_specification>` component is optioneel, tenzij de bron is gefilterd of verderop in de query verwachte. Het doel van deze component is om op te geven van de gegevensbron waarop de query moet functioneren. De volledige verzameling is vaak de bron, maar één een subset van de verzameling in plaats daarvan kunt opgeven. 

Een query zoals `SELECT * FROM Families` geeft aan dat de volledige verzameling worden Families de bron via die u wilt inventariseren. Een speciale ROOT-id kan worden gebruikt voor de verzameling in plaats van de naam van de verzameling. De volgende lijst bevat de regels die per query gelden:

* De verzameling kunt aliasnamen worden gebruikt, zoals `SELECT f.id FROM Families AS f` of gewoon `SELECT f.id FROM Families f`. Hier `f` is het equivalent van `Families`. `AS` is een optioneel trefwoord tot de alias van de id.
* Eenmaal alias, de oorspronkelijke bron kan niet worden gekoppeld. Bijvoorbeeld, `SELECT Families.id FROM Families f` syntaxis is ongeldig omdat de id 'Families' kan niet meer worden omgezet.
* Alle eigenschappen die moeten worden verwezen naar de moeten volledig gekwalificeerde zijn. In de afwezigheid van strikt schema voldoet, wordt dit afgedwongen om te voorkomen dat een niet-eenduidige bindingen. Daarom `SELECT id FROM Families f` syntaxis is ongeldig omdat de eigenschap `id` niet is gebonden.

### <a name="subdocuments"></a>Subdocumenten
De bron kan ook worden teruggebracht tot een kleinere subset. Bijvoorbeeld voor het inventariseren van alleen een substructuur in elk document, kan de subroot vervolgens worden de bron, zoals wordt weergegeven in het volgende voorbeeld:

**Query**

    SELECT * 
    FROM Families.children

**Results**  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

Hoewel het bovenstaande voorbeeld gebruikt een matrix als de bron, een object kan ook worden gebruikt als de bron, dit is wat wordt weergegeven in het volgende voorbeeld: een geldig JSON-waarde (niet ongedefinieerd) die kan worden gevonden in de bron wordt beschouwd als voor opname in het resultaat van de query. Als sommige families geen een `address.state` -waarde in het queryresultaat worden uitgesloten.

**Query**

    SELECT * 
    FROM Families.address.state

**Results**

    [
      "WA", 
      "NY"
    ]


## <a id="WhereClause"></a>WHERE-component
De component WHERE (**`WHERE <filter_condition>`**) is optioneel. Hiermee geeft u de voorwaarden die de JSON-documenten die zijn opgegeven door de bron moet voldoen om te worden opgenomen als onderdeel van het resultaat. Elk JSON-document moet de opgegeven voorwaarden op "true" worden overwogen voor het resultaat opleveren. De WHERE-component wordt gebruikt door de index-laag om te bepalen van de absolute kleinste subset van de brondocumenten die deel van het resultaat uitmaken kunnen. 

De volgende query-documenten met een eigenschap name waarvan de waarde vraagt `AndersenFamily`. Elk document dat geen een eigenschap name of waar de waarde komt niet overeen met `AndersenFamily` is uitgesloten. 

**Query**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Results**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


Het vorige voorbeeld hebt u een eenvoudige gelijkheid query geleerd. De SQL-API biedt ook ondersteuning voor tal van scalaire expressies. Het meest worden gebruikt zijn binaire bestanden en unaire expressies. De eigenschap verwijzingen van de bron-JSON-object zijn ook geldig expressies. 

De volgende binaire operators worden momenteel ondersteund en kunnen worden gebruikt in query's, zoals wordt weergegeven in de volgende voorbeelden:  

<table>
<tr>
<td>Rekenkundige</td>    
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Bitsgewijze</td>    
<td>|, &, ^, <<>>,, >>> (shift-nul-opvulling rechts)</td>
</tr>
<tr>
<td>Logische</td>
<td>EN, OF NIET</td>
</tr>
<tr>
<td>Vergelijking</td>    
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>Reeks</td>    
<td>|| (samenvoegen)</td>
</tr>
</table>  


Laten we eens enkele query's met behulp van binaire operators.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


De unitaire operators +,-, ~ worden ook ondersteund, en niet kan worden gebruikt in query's, zoals wordt weergegeven in het volgende voorbeeld:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



Naast de binaire bestanden en unaire operators, zijn ook verwijzen naar eigenschappen toegestaan. Bijvoorbeeld, `SELECT * FROM Families f WHERE f.isRegistered` retourneert het JSON-document met de eigenschap `isRegistered` waar de waarde van de eigenschap is gelijk aan de JSON `true` waarde. Alle overige waarden (de waarde false, null, niet-gedefinieerde, `<number>`, `<string>`, `<object>`, `<array>`, enzovoort) leidt tot de brondocument wordt uitgesloten van het resultaat. 

### <a name="equality-and-comparison-operators"></a>Gelijkheid en vergelijking van de operators
De volgende tabel toont het resultaat van de gelijkheid vergelijkingen in de SQL-API tussen elke twee JSON-typen.

<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>Op</strong>
         </td>
         <td valign="top">
            <strong>Niet-gedefinieerde</strong>
         </td>
         <td valign="top">
            <strong>Null</strong>
         </td>
         <td valign="top">
            <strong>Booleaanse waarde</strong>
         </td>
         <td valign="top">
            <strong>Aantal</strong>
         </td>
         <td valign="top">
            <strong>Tekenreeks</strong>
         </td>
         <td valign="top">
            <strong>Object</strong>
         </td>
         <td valign="top">
            <strong>Matrix</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Niet-gedefinieerde<strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Null<strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Booleaanse waarde<strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Aantal<strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Tekenreeks<strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Object<strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Matrix<strong>
         </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
Niet gedefinieerd </td>
         <td valign="top">
            <strong>OK</strong>
         </td>
      </tr>
   </tbody>
</table>

Voor andere vergelijkingsoperators zoals >, > =,! =, < en < =, de volgende regels van toepassing:   

* Vergelijking van de verschillende typen leidt er niet gedefinieerd.
* Vergelijking tussen de twee objecten of twee matrices leidt niet gedefinieerd.   

Als het resultaat van de scalaire expressie die u in het filter is niet gedefinieerd, de bijbehorende document zou niet opgenomen in het resultaat, omdat Undefined logisch niet met een 'true overeen'.

### <a name="between-keyword"></a>TUSSEN trefwoord
U kunt ook het sleutelwoord BETWEEN-query's op het bereik van waarden, zoals in de ANSI SQL Express gebruiken. TUSSEN kan worden gebruikt voor tekenreeksen of getallen.

Deze query retourneert bijvoorbeeld alle familie documenten waarin het eerste onderliggende niveau tussen 1-5 (zowel inclusief is). 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

In tegenstelling tot in ANSI-SQL, kunt u ook gebruiken de BETWEEN-component in de component FROM, zoals in het volgende voorbeeld.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

Houd er rekening mee voor snellere query uitvoeringstijden, om te maken van een indexeringsbeleid die gebruikmaakt van een type bereik index op basis van een numerieke eigenschappen/paden die zijn gefilterd in de component BETWEEN. 

Het belangrijkste verschil tussen het gebruik van BETWEEN in de SQL API en de ANSI SQL is dat u kunt de bereik-query's op basis van eigenschappen van gemengde gegevenstypen express – bijvoorbeeld, u mogelijk "Geavanceerde" een getal (5) in bepaalde documenten en tekenreeksen in andere gevallen ("grade4"). In dergelijke gevallen wordt, zoals in JavaScript, een vergelijking tussen twee verschillende typen resulteert in 'niet-gedefinieerde' en het document overgeslagen.

### <a name="logical-and-or-and-not-operators"></a>Logische (AND, OR en NOT) operators
Logische operators worden uitgevoerd voor Booleaanse waarden. De logische waarheid tabellen voor deze operators worden weergegeven in de volgende tabellen.

| OF | True | False | Niet gedefinieerd |
| --- | --- | --- | --- |
| True |True |True |True |
| False |True |False |Niet gedefinieerd |
| Niet gedefinieerd |True |Niet gedefinieerd |Niet gedefinieerd |

| EN | True | False | Niet gedefinieerd |
| --- | --- | --- | --- |
| True |True |False |Niet gedefinieerd |
| False |False |False |False |
| Niet gedefinieerd |Niet gedefinieerd |False |Niet gedefinieerd |

| NIET |  |
| --- | --- |
| True |False |
| False |True |
| Niet gedefinieerd |Niet gedefinieerd |

### <a name="in-keyword"></a>TREFWOORD
Het sleutelwoord kan worden gebruikt om te controleren of een opgegeven waarde komt overeen met een willekeurige waarde in een lijst. Bijvoorbeeld, retourneert deze query alle familie documenten waarin de id is een van de 'WakefieldFamily' of 'AndersenFamily'. 

    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

In dit voorbeeld retourneert alle documenten waarin de status is een van de opgegeven waarden.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Ternair (?) en operators samenvoegen (?)
De operators Ternair en samenvoegen kunnen worden gebruikt om te bouwen voorwaardelijke expressies, die vergelijkbaar is met populaire programmeertalen, zoals C# en JavaScript. 

De operator Ternair (?) is heel handig bij het maken van nieuwe JSON-eigenschappen op elk gewenst moment. Bijvoorbeeld: nu kunt u query's voor het classificeren van het niveau van de klasse in een mens leesbaar formulier zoals Beginner/tussenliggende/Geavanceerd zoals hieronder wordt weergegeven.

     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

U kunt ook het aanroepen van de operator, zoals in de onderstaande query nesten.

    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Als met andere standaardoperators voor query's, als de eigenschappen waarnaar wordt verwezen in de voorwaardelijke expressie ontbreekt in een document, of als het type dat wordt vergeleken verschillen, vervolgens die documenten worden niet opgenomen in de queryresultaten.

De operator samenvoegen (?) kan worden gebruikt om efficiënt te controleren op de aanwezigheid van een eigenschap (ook wel) gedefinieerd) in een document. Dit is handig bij het uitvoeren van query's op basis van semi-gestructureerde of gegevens van verschillende typen. Deze query retourneert er bijvoorbeeld voor dat de 'lastName' indien aanwezig, of de "Achternaam" Als deze niet aanwezig is.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a id="EscapingReservedKeywords"></a>Tussen aanhalingstekens eigenschapsaccessor
U kunt ook toegang tot eigenschappen met behulp van de operator tussen aanhalingstekens eigenschap `[]`. Bijvoorbeeld, `SELECT c.grade` en `SELECT c["grade"]` gelijk zijn. Deze syntaxis is handig wanneer u nodig hebt als u een eigenschap die spaties, speciale tekens bevat of gebeurt er met dezelfde naam als een SQL-sleutelwoord of een gereserveerd woord.

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a id="SelectClause"></a>SELECT-component
De component SELECT (**`SELECT <select_list>`**) is verplicht en geeft aan welke waarden worden opgehaald uit de query, net zoals in de ANSI SQL. De subset die zijn gefilterd op de brondocumenten zijn doorgegeven aan de Projectiefase, waarbij de opgegeven JSON-waarden worden opgehaald en een nieuwe JSON-object is samengesteld, voor elke invoer doorgegeven aan deze. 

Het volgende voorbeeld toont een typische SELECT-query. 

**Query**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Results**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>Geneste eigenschappen
In het volgende voorbeeld zijn er twee geneste eigenschappen projecteren `f.address.state` en `f.address.city`.

**Query**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Results**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Projectie biedt ook ondersteuning voor JSON-expressies, zoals wordt weergegeven in het volgende voorbeeld:

**Query**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Results**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Bekijk de rol van `$1` hier. De `SELECT` component moet een JSON-object maken en omdat er geen sleutel is opgegeven, gebruiken we de impliciete argument variabele namen die beginnen met `$1`. Deze query retourneert bijvoorbeeld twee argumentvariabelen voor de impliciete, met het label `$1` en `$2`.

**Query**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Results**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>Aliasing
Nu gaan we uitgebreid in het voorbeeld hierboven met expliciete aliasing van waarden. Als het sleutelwoord gebruikt voor aliasing is. Het is optioneel, zoals wordt weergegeven tijdens het projecteren van de tweede waarde als `NameInfo`. 

Als een query twee eigenschappen met dezelfde naam heeft, moet aliasing worden gebruikt om een of beide van de eigenschappen wijzigen zodat ze zijn disambiguated in het verwachte resultaat.

**Query**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Results**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>Scalaire expressies
Naast de verwijzingen van de eigenschap ondersteuning de component SELECT ook voor scalaire expressies, zoals constanten, rekenkundige bewerkingen, logische expressies, enzovoort. Dit is bijvoorbeeld een eenvoudige 'Hallo wereld'-query.

**Query**

    SELECT "Hello World"

**Results**

    [{
      "$1": "Hello World"
    }]


Hier volgt een meer complex voorbeeld die gebruikmaakt van een scalaire expressie.

**Query**

    SELECT ((2 + 11 % 7)-2)/3    

**Results**

    [{
      "$1": 1.33333
    }]


In het volgende voorbeeld is het resultaat van de scalaire expressie die een Booleaanse waarde.

**Query**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f    

**Results**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Object en matrix maken
Een andere belangrijke functie van de SQL-API is object array/maken. In het vorige voorbeeld, houd er rekening mee dat we een nieuwe JSON-object gemaakt. Op deze manier kan een matrices ook bouwen zoals wordt weergegeven in de volgende voorbeelden:

**Query**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f    

**Results**  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <a id="ValueKeyword"></a>WAARDE trefwoord
De **waarde** sleutelwoord biedt een manier om JSON-waarde te retourneren. De onderstaande query retourneert bijvoorbeeld scalaire `"Hello World"` in plaats van `{$1: "Hello World"}`.

**Query**

    SELECT VALUE "Hello World"

**Results**

    [
      "Hello World"
    ]


De volgende query retourneert de JSON-waarde zonder de `"address"` label in de resultaten.

**Query**

    SELECT VALUE f.address
    FROM Families f    

**Results**  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

Het volgende voorbeeld is een uitbreiding om toe te laten zien hoe u JSON primitieve waarden (het knooppuntniveau van de JSON-structuur) retourneren. 

**Query**

    SELECT VALUE f.address.state
    FROM Families f    

**Results**

    [
      "WA",
      "NY"
    ]


### <a name="-operator"></a>* Operator
De speciale operator (*) wordt ondersteund voor het document als project-is. Als u gebruikt, moet deze de enige verwachte veld. Terwijl een query zoals `SELECT * FROM Families f` geldig is, `SELECT VALUE * FROM Families f ` en `SELECT *, f.id FROM Families f ` zijn niet geldig.

**Query**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Results**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

### <a id="TopKeyword"></a>TOP-Operator
Het sleutelwoord TOP kan worden gebruikt om het aantal waarden uit een query te beperken. Als boven wordt gebruikt in combinatie met de component ORDER BY, is de resultatenset beperkt tot de eerste N aantal geordende waarden; anders, wordt de eerste N-nummer van de resultaten in een niet-gedefinieerde volgorde geretourneerd. Als een best practice, in een instructie SELECT gebruik altijd een ORDER BY-component met de TOP-component. Dit is de enige manier om aan te geven zoals verwacht waarin rijen zijn beïnvloed door boven. 

**Query**

    SELECT TOP 1 * 
    FROM Families f 

**Results**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

TOP kan worden gebruikt met een constante waarde (zoals hierboven) of met de waarde van een variabele met geparameteriseerde query's. Zie geparameteriseerde query's hieronder voor meer informatie.

### <a id="Aggregates"></a>Statistische functies
U kunt ook uitvoeren de aggregaties in de `SELECT` component. Statistische functies een berekening uitgevoerd op een set waarden en één waarde retourneren. De volgende query retourneert bijvoorbeeld het aantal familie documenten binnen de verzameling.

**Query**

    SELECT COUNT(1) 
    FROM Families f 

**Results**

    [{
        "$1": 2
    }]

U kunt ook de scalaire waarde van de statistische functie retourneren met behulp van de `VALUE` trefwoord. De volgende query retourneert bijvoorbeeld het aantal waarden als één getal:

**Query**

    SELECT VALUE COUNT(1) 
    FROM Families f 

**Results**

    [ 2 ]

U kunt ook statistische functies uitvoeren in combinatie met filters. De volgende query retourneert bijvoorbeeld het aantal documenten met het adres in de staat Washington.

**Query**

    SELECT VALUE COUNT(1) 
    FROM Families f
    WHERE f.address.state = "WA" 

**Results**

    [ 1 ]

De volgende tabel bevat de lijst met ondersteunde statistische functies in de SQL-API. `SUM` en `AVG` worden uitgevoerd over numerieke waarden, terwijl `COUNT`, `MIN`, en `MAX` in getallen, tekenreeksen, Booleaanse waarden en een null-waarden kunnen worden uitgevoerd. 

| Gebruik | Beschrijving |
|-------|-------------|
| AANTAL | Retourneert het aantal items in de expressie. |
| SOM   | Retourneert de som van alle waarden in de expressie. |
| MIN   | Retourneert de minimumwaarde in de expressie. |
| MAX   | Retourneert de maximumwaarde in de expressie. |
| GEM   | Retourneert het gemiddelde van de waarden in de expressie. |

Statistische functies kunnen ook worden uitgevoerd via de resultaten van een iteratie van de matrix. Zie voor meer informatie, [iteratie van de matrix in query's](#Iteration).

> [!NOTE]
> Wanneer u de Azure portal Data Explorer, houd er rekening mee dat mogelijk aggregatie-query's de gedeeltelijk geaggregeerde resultaten geretourneerd via een querypagina. De SDK's produceert één cumulatieve waarde op alle pagina's. 
> 
> Als u wilt uitvoeren van aggregatie-query's met behulp van code, moet u de .NET SDK 1.12.0, .NET Core SDK 1.1.0 of Java SDK 1.9.5 of hoger.    
>

## <a id="OrderByClause"></a>ORDER BY-component
Net als in de ANSI-SQL, u een optionele Order By-component tijdens het uitvoeren van query's opnemen kunt. De component kan bevatten een optioneel argument ASC/DESC om op te geven van de volgorde waarin de resultaten moeten worden opgehaald.

Bijvoorbeeld, als volgt een query waarmee families in volgorde van de residente plaatsnaam worden opgehaald.

**Query**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city

**Results**

    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"    
      }
    ]

En Hier volgt een query die wordt opgehaald families in volgorde van de aanmaakdatum, dat is opgeslagen als een getal voor de epoche tijd, Internet Explorer, verstreken tijd sinds 1 januari 1970 in seconden.

**Query**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC

**Results**

    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472    
      }
    ]

## <a id="Advanced"></a>Concepten van geavanceerde database en SQL-query 's

### <a id="Iteration"></a>Herhaling
Een nieuwe constructie is toegevoegd de **IN** sleutelwoord in de SQL-API om ondersteuning te bieden voor iteratie van JSON-matrices. De bron van biedt ondersteuning voor iteratie. Laten we beginnen met het volgende voorbeeld:

**Query**

    SELECT * 
    FROM Families.children

**Results**  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

Nu gaan we bekijken een andere query die iteratie van onderliggende items in de verzameling uitvoert. Let op het verschil in de uitvoermatrix. In dit voorbeeld splitst `children` en worden de resultaten in een matrix samengevoegd.  

**Query**

    SELECT * 
    FROM c IN Families.children

**Results**  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

Dit kan verder worden gebruikt om te filteren op elke afzonderlijke vermelding van de matrix, zoals wordt weergegeven in het volgende voorbeeld:

**Query**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**Results**  

    [{
      "givenName": "Lisa"
    }]

U kunt ook globalisatie uitvoeren via het resultaat van de matrix iteratie. Bijvoorbeeld telt de volgende query het aantal onderliggende items tussen alle gezinnen.

**Query**

    SELECT COUNT(child) 
    FROM child IN Families.children

**Results**  

    [
      { 
        "$1": 3
      }
    ]

### <a id="Joins"></a>Joins
In een relationele database, de noodzaak om toe te voegen in tabellen, het is belangrijk. Het is de logische overheidsinstelling voor het ontwerpen van genormaliseerde schema's. De SQL-API behandelt het model gedenormaliseerd gegevens zonder schema documenten niet overeenkomstig dit. Dit is het logische equivalent van een 'self-join'.

De syntaxis die ondersteuning biedt voor de taal is < from_source1 > JOIN < from_source2 > JOIN... JOIN < from_sourceN >. Algemene, deze retourneert een set met **N**- tuples (tuple met **N** waarden). Elke tuple heeft waarden die worden geproduceerd door alle verzameling aliassen iteratie van hun respectieve sets. Dit is met andere woorden, een volledige vectorproduct van de sets die deel uitmaken van de join.

De volgende voorbeelden ziet de werking van de JOIN-component. In het volgende voorbeeld wordt het resultaat is leeg omdat het vectorproduct van elk document van de bron en een lege set is leeg.

**Query**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Results**  

    [{
    }]


In het volgende voorbeeld wordt de join is tussen de hoofdmap van het document en de `children` subroot. Het is een vectorproduct tussen twee JSON-objecten. Het feit dat kinderen is een matrix is niet effectief in de JOIN omdat we te maken hebt met een hoofdmap die de matrix van de onderliggende items. Het resultaat bevat daarom slechts twee resultaten, omdat het vectorproduct van elk document met de matrix precies slechts één document levert.

**Query**

    SELECT f.id
    FROM Families f
    JOIN f.children

**Results**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


Het volgende voorbeeld ziet u een meer conventionele join:

**Query**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**Results**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



Het eerste wat om te weten is dat de `from_source` van de **JOIN** component is een iterator. Ja, de stroom in dit geval is als volgt:  

* Vouw elk onderliggend element **c** in de matrix.
* Een cross-product met de hoofdmap van het document toepassen **f** met elk onderliggend element **c** die in de eerste stap is afgevlakt.
* Ten slotte het hoofdobject van project **f** alleen de eigenschap naam. 

Het eerste document (`AndersenFamily`) bevat slechts één onderliggend element, zodat de resultatenset bevat alleen een enkel object hoort bij dit document. Het tweede document (`WakefieldFamily`) twee onderliggende items bevat. Het vectorproduct produceert dus een afzonderlijke-object voor elke onderliggende, waardoor dit resulteert in twee objecten, één voor elke onderliggende overeenkomt met dit document. De basis-velden in beide deze documenten zijn hetzelfde, net zoals u in een cross-product verwacht.

De echte hulpprogramma van de JOIN is het formulier tuples uit het vectorproduct in een vorm die anders moeilijk te project. U kunt bovendien ziet u in het voorbeeld hieronder, filteren op de combinatie van een tuple dat kiest, kunnen de gebruiker heeft ervoor gekozen een voorwaarde voldaan door de tuples algemene.

**Query**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets

**Results**

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



In dit voorbeeld is een natuurlijke uitbreiding van het vorige voorbeeld, en voert een dubbele koppeling. Het vectorproduct kunnen dus worden weergegeven als de volgende pseudo-code:

    for-each(Family f in Families)
    {    
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

`AndersenFamily` heeft één onderliggende die één huisdier heeft. Dus het vectorproduct resulteert in een rij (1\*1\*1) van deze serie. WakefieldFamily heeft echter twee kinderen, maar slechts één onderliggende "Jesse" huisdieren. Jesse heeft echter twee huisdieren. Daarom het vectorproduct 1 levert\*1\*2 = 2 rijen uit deze serie.

In het volgende voorbeeld wordt er is een extra filter op `pet`. Alle tuples waar de naam van de huisdier 'Schaduwkopie' is niet van toepassing. U ziet dat we kunnen tuples van matrices, filter op een van de elementen van de tuple, het bouwen en een combinatie van de elementen van project. 

**Query**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**Results**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a id="JavaScriptIntegration"></a>JavaScript-integratie
Azure Cosmos DB biedt een programmeermodel voor het uitvoeren van op basis van JavaScript-toepassingslogica rechtstreeks op de verzamelingen in termen van opgeslagen procedures en triggers. Hierdoor kunnen voor beide:

* Mogelijkheid om te krachtige transactionele CRUD-bewerkingen en query's op documenten in een verzameling door de diepe integratie van JavaScript-runtime rechtstreeks in de database-engine. 
* Een natuurlijke modellering van Controlestroom, variabele scopes en -toewijzing en integratie van primitieven databasetransacties afhandeling van uitzonderingen. Raadpleeg de documentatie van de server-side programmeerbaarheid JavaScript voor meer informatie over Azure Cosmos DB-ondersteuning voor JavaScript-integratie.

### <a id="UserDefinedFunctions"></a>Gebruiker gedefinieerde functies (UDF's)
De SQL-API biedt samen met de typen al gedefinieerd in dit artikel, ondersteuning voor gebruiker gedefinieerde functies (UDF's). In het bijzonder wordt scalaire UDF's waar de ontwikkelaars kunnen nul of meer argumenten worden doorgegeven en retourneert een resultaat van één argument weer ondersteund. Elk van deze argumenten is ingeschakeld voor uw geldige JSON-waarden.  

De SQL-syntaxis is uitgebreid ter ondersteuning van aangepaste toepassingslogica met behulp van deze door de gebruiker gedefinieerde functies. UDF's kunnen worden geregistreerd met SQL-API en vervolgens naar worden verwezen als onderdeel van een SQL-query. De UDF's zijn in feite exquisitely ontworpen om te worden aangeroepen door query's. Als gevolg van deze keuze hebt UDF's geen toegang tot het contextobject dat de andere JavaScript typen (opgeslagen procedures en triggers) hebben. Omdat de query's worden uitgevoerd als alleen-lezen, kunnen ze worden uitgevoerd op de primaire of secundaire replica's. Daarom zijn UDF's ontworpen om uit te voeren op secundaire replica's in tegenstelling tot andere typen JavaScript.

Hieronder volgt een voorbeeld van hoe een UDF kan worden geregistreerd bij de Cosmos DB-database, om precies onder een documentverzameling.

       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };

       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  

Het voorgaande voorbeeld wordt een UDF met de naam `REGEX_MATCH`. De cmdlet accepteert twee waarden voor JSON-tekenreeks `input` en `pattern` en controleert of het eerste komt overeen met het patroon die zijn opgegeven in de tweede JavaScript functie string.match() te gebruiken.

We kunnen deze UDF nu gebruiken in een query in een projectie. UDF's moeten worden gekwalificeerd met de hoofdlettergevoelige voorvoegsel "udf." Wanneer aangeroepen vanuit query's. 

> [!NOTE]
> Vóór 17-3/2015 ondersteund Cosmos DB UDF aanroepen zonder de "udf." voorvoegsel zoals REGEX_MATCH() selecteren. Dit patroon aanroepen is afgeschaft.  
> 
> 

**Query**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Results**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

De UDF kan ook worden gebruikt in een filter, zoals wordt weergegeven in het onderstaande voorbeeld ook gekwalificeerd met de 'udf." voorvoegsel:

**Query**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**Results**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


In wezen UDF's geldige scalaire expressies zijn en kunnen worden gebruikt in projecties en filters. 

Als u wilt uitbreiden op de kracht van UDF's, laten we bekijken een ander voorbeeld met voorwaardelijke logica:

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                   switch (city) {
                       case 'seattle':
                           return 520;
                       case 'NY':
                           return 410;
                       case 'Chicago':
                           return 673;
                       default:
                           return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);


Hieronder volgt een voorbeeld waarin de UDF oefeningen.

**Query**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f    

**Results**

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


Als de voorgaande voorbeelden laten zien, integreren UDF's in de kracht van JavaScript-taal de SQL-API biedt een uitgebreide programmeerbare interface hiervoor complexe procedurele, voorwaardelijke logica aan de hand van de geïntegreerde JavaScript-runtime-functionaliteit.

De SQL-API biedt de argumenten voor de UDF's voor elk document in de bron in de huidige fase (WHERE-component of component SELECT) van de verwerking van de UDF. Het resultaat is opgenomen in de algehele pijplijn naadloos. Als de eigenschappen waarnaar wordt verwezen door de UDF parameters zijn niet beschikbaar in de JSON-waarde, de parameter wordt beschouwd als niet-gedefinieerde, en daarom de UDF-aanroep volledig is overgeslagen. Op dezelfde manier als het resultaat van de UDF gedefinieerd is, het niet opgenomen in het resultaat. 

Kortom zijn UDF's geweldige hulpprogramma's voor complexe bedrijfslogica doen als onderdeel van de query.

### <a name="operator-evaluation"></a>Operator-evaluatie
Cosmos DB, tekent parallels met JavaScript-operators en de evaluatie-semantiek van het feit dat een JSON-database. Terwijl de Cosmos DB probeert te behouden van JavaScript-semantiek in termen van JSON-ondersteuning, wordt de evaluatie van de bewerking in sommige gevallen afwijkt.

In de SQL-API, zijn in tegenstelling tot in traditionele SQL, de typen van waarden vaak niet bekend totdat de waarden worden opgehaald uit de database. De meeste van de operators hebben om efficiënt query's uitvoeren, strikte eisen. 

De SQL-API biedt geen impliciete conversies, in tegenstelling tot JavaScript uitvoeren. Bijvoorbeeld, een query zoals `SELECT * FROM Person p WHERE p.Age = 21` komt overeen met documenten met een eigenschap leeftijd is waarvan de waarde 21 is. Een ander document waarvan de eigenschap leeftijd komt overeen met de tekenreeks '21' of andere mogelijk oneindige variaties, zoals "021", "21,0", "0021", "00021", enzovoort wordt niet overeen. Dit is daarentegen aan de JavaScript waar de tekenreekswaarden impliciet omgezet naar getallen zijn (op basis van de operator, bijvoorbeeld: ==). Deze keuze is van cruciaal belang voor efficiënte index die overeenkomen met in de SQL-API. 

## <a name="parameterized-sql-queries"></a>Geparameteriseerde SQL-query 's
Cosmos DB biedt ondersteuning voor query's met parameters worden uitgedrukt met de vertrouwde \@ notatie. Geparameteriseerde SQL biedt robuuste verwerken en aanhalingstekens van gebruikersinvoer, zo wordt voorkomen dat onbedoelde blootstelling van gegevens via SQL-injectie. 

U kunt bijvoorbeeld een query schrijven waarmee de achternaam en -adresstatus als parameters duurt en vervolgens uitvoeren voor de verschillende waarden van de laatste naam en adres staat op basis van de invoer van de gebruiker.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

Deze aanvraag kan vervolgens worden verzonden naar Cosmos DB als een JSON-query met parameters, zoals hieronder wordt weergegeven.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

Het argument voor TOP kan worden ingesteld met geparameteriseerde query's, zoals hieronder wordt weergegeven.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

Parameterwaarden mag geldige JSON (tekenreeksen, getallen, Booleaanse waarden, null, zelfs matrices of geneste JSON). Ook omdat Cosmos DB zonder schema, worden parameters niet gevalideerd met elk type.

## <a id="BuiltinFunctions"></a>Ingebouwde functies
Cosmos DB ondersteunt ook een aantal ingebouwde functies voor algemene bewerkingen, die kunnen worden gebruikt in query's zoals de gebruiker gedefinieerde functies (UDF's).

| Functie-groep          | Bewerkingen                                                                                                                                          |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Wiskundige functies  | ABS, maximum, EXP, FLOOR, LOG, LOG10, POWER, ROUND, ONDERTEKENEN, WORTEL, VIERKANT, geheel, ACOS, ASIN, ATAN, ATN2, COS, COT, graden, PI, RADIANS, SIN en TAN |
| Controle van functies van het type | IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED en IS_PRIMITIVE                                                           |
| Tekenreeksfuncties        | CONCAT, bevat ENDSWITH, INDEX_OF, links, lengte, lager, LTRIM, vervangen, REPLICEREN, OMGEKEERD, rechts, RTRIM, STARTSWITH, SUBTEKENREEKS en hoofdletters       |
| Matrixfuncties         | ARRAY_CONCAT, ARRAY_CONTAINS ARRAY_LENGTH en ARRAY_SLICE                                                                                         |
| Ruimtelijke-functies       | ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID en ST_ISVALIDDETAILED                                                                           | 

Als u momenteel van een gebruiker gedefinieerde functie (UDF's gebruikmaakt) waarvoor een ingebouwde functie nu beschikbaar is is, moet u de bijbehorende ingebouwde functie gebruiken als het gaat vaak sneller worden uitgevoerd en efficiënter. 

### <a name="mathematical-functions"></a>Wiskundige functies
Wiskundige functies elke uitvoeren van een berekening op basis van de invoerwaarden die zijn opgegeven als argumenten en retourneert een numerieke waarde. Hier volgt een lijst met ondersteunde ingebouwde wiskundige functies.


| Gebruik | Beschrijving |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [[ABS (num_expr)](#bk_abs) | Retourneert de absolute (positief) waarde van de opgegeven numerieke expressie. |
| [MAXIMUM (num_expr)](#bk_ceiling) | Retourneert de kleinste integer-waarde groter dan of gelijk zijn aan de opgegeven numerieke expressie. |
| [FLOOR (num_expr)](#bk_floor) | Retourneert het grootste gehele getal kleiner dan of gelijk zijn aan de opgegeven numerieke expressie. |
| [EXP (num_expr)](#bk_exp) | Retourneert de exponent van de opgegeven numerieke expressie. |
| [LOGBOEK (num_expr [, base])](#bk_log) | Retourneert de natuurlijke logaritme van de opgegeven numerieke expressie, of de logaritme met behulp van het opgegeven grondtal |
| [LOG10 (num_expr)](#bk_log10) | Retourneert de logaritmische waarde grondtal 10 van de opgegeven numerieke expressie. |
| [RONDE (num_expr)](#bk_round) | Retourneert een numerieke waarde, afgerond naar het dichtstbijzijnde gehele getal. |
| [GEHEEL (num_expr)](#bk_trunc) | Retourneert een numerieke waarde, afgekapt naar het dichtstbijzijnde gehele getal. |
| [SQRT (num_expr)](#bk_sqrt) | Retourneert de vierkantswortel van de opgegeven numerieke expressie. |
| [SQUARE (num_expr)](#bk_square) | Retourneert het kwadraat van de opgegeven numerieke expressie. |
| [VOEDING (num_expr, num_expr)](#bk_power) | Retourneert de kracht van de opgegeven numerieke expressie voor de opgegeven waarde. |
| [ONDERTEKENEN (num_expr)](#bk_sign) | Retourneert de waarde teken (-1, 0, 1) van de opgegeven numerieke expressie. |
| [ACOS (num_expr)](#bk_acos) | Retourneert de hoek in radialen, waarvan de cosinus de opgegeven numerieke expressie is. ook wel de boogcosinus genoemd. |
| [ASIN (num_expr)](#bk_asin) | Retourneert de hoek in radialen, waarvan de sinus de opgegeven numerieke expressie is. Dit wordt ook boogsinus genoemd. |
| [ATAN (num_expr)](#bk_atan) | Retourneert de hoek in radialen, waarvan de tangens de opgegeven numerieke expressie is. Dit wordt ook de boogtangens genoemd. |
| [ATN2 (num_expr)](#bk_atn2) | Geeft als resultaat de hoek in radialen tussen de positieve x-as en de ray bij de oorsprong naar de punt (y, x), waarbij x en y zijn de waarden van de twee opgegeven float-expressies. |
| [COS (num_expr)](#bk_cos) | Retourneert de trigonometrische cosinus van de opgegeven hoek in radialen in de opgegeven expressie. |
| [COT (num_expr)](#bk_cot) | Retourneert de trigonometrische cotangens van de opgegeven hoek in radialen in het opgegeven numerieke expressie. |
| [GRADEN (num_expr)](#bk_degrees) | Retourneert de bijbehorende hoek in graden voor een hoek aangeduid in radialen. |
| [PI ()](#bk_pi) | Retourneert de constante waarde van PI. |
| [RADIANS (num_expr)](#bk_radians) | Retourneert een radialen als een numerieke expressie, in graden is ingevoerd. |
| [SIN (num_expr)](#bk_sin) | Retourneert de trigonometrische sinus van de opgegeven hoek in radialen in de opgegeven expressie. |
| [TAN (num_expr)](#bk_tan) | Retourneert de tangens van de invoer-expressie in de opgegeven expressie. |

Bijvoorbeeld, kunt u nu query's als volgt uitvoeren:

**Query**

    SELECT VALUE ABS(-4)

**Results**

    [4]

Het belangrijkste verschil tussen van Cosmos DB-functies ten opzichte van ANSI SQL is dat ze zijn ontworpen om te werken goed voor schemagegevens zonder schema en een gemengde. Hebt u een document waarin de grootte van de eigenschap ontbreekt of heeft een niet-numerieke waarde als 'Onbekend' en het document is overgeslagen, klikt u in plaats van een fout geretourneerd.

### <a name="type-checking-functions"></a>Controle van functies van het type
Het type controle-functies kunnen u om te controleren van het type van een expressie in SQL-query's. Type controle functies kunnen worden gebruikt om te bepalen van het type van de eigenschappen in documenten op elk gewenst moment wanneer deze variabele of onbekend. Hier volgt een lijst met ondersteunde ingebouwde controle van het type functies.

<table>
<tr>
  <td><strong>Gebruik</strong></td>
  <td><strong>Beschrijving</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (markering)</a></td>
  <td>Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een matrix is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (markering)</a></td>
  <td>Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een Booleaanse waarde is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (markering)</a></td>
  <td>Retourneert een Booleaanse waarde die aangeeft of het type van de waarde null is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (markering)</a></td>
  <td>Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een getal is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (markering)</a></td>
  <td>Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een JSON-object is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (markering)</a></td>
  <td>Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een tekenreeks is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (markering)</a></td>
  <td>Retourneert een Booleaanse waarde waarmee wordt aangegeven als de eigenschap een waarde is toegewezen.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (markering)</a></td>
  <td>Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een tekenreeks, getal, Booleaanse waarde of null zijn is.</td>
</tr>

</table>

U kunt nu een query's als volgt uitvoeren met behulp van deze functies:

**Query**

    SELECT VALUE IS_NUMBER(-4)

**Results**

    [true]

### <a name="string-functions"></a>Tekenreeksfuncties
De volgende scalaire functies uitvoeren van een bewerking op een tekenreekswaarde voor invoer en retourneert een tekenreeks, een numerieke of Booleaanse waarde. Hier volgt een tabel met ingebouwde tekenreeksfuncties:

| Gebruik | Beschrijving |
| --- | --- |
| [De lengte (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length) |Retourneert het aantal tekens van de opgegeven tekenreeks-expressie |
| [CONCAT (str_expr str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat) |Retourneert een tekenreeks die het resultaat van het samenvoegen van twee of meer tekenreekswaarden. |
| [De SUBTEKENREEKS (str_expr, num_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring) |Retourneert deel van een tekenreeksexpressie. |
| [STARTSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith) |Retourneert een Booleaanse waarde die aangeeft of de eerste expressie tekenreeks begint met de tweede |
| [ENDSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith) |Retourneert een Booleaanse waarde die aangeeft of de eerste expressie tekenreeks eindigt met de tweede |
| [BEVAT (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains) |Retourneert een Booleaanse waarde die aangeeft of de eerste expressie tekenreeks de tweede bevat. |
| [INDEX_OF (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of) |Retourneert de beginpositie van het eerste exemplaar van de tweede tekenreeksexpressie binnen de eerste expressie in de opgegeven tekenreeks, of -1 als de tekenreeks is niet gevonden. |
| [LEFT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left) |Retourneert het linkerdeel van een tekenreeks zijn met het opgegeven aantal tekens. |
| [RIGHT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right) |Retourneert de juiste deel van een tekenreeks zijn met het opgegeven aantal tekens. |
| [LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim) |Retourneert een tekenreeksexpressie na het verwijderen van toonaangevende lege waarden. |
| [RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim) |Retourneert een tekenreeksexpressie na het afkappen van alle volgspaties. |
| [KLEINE (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower) |Retourneert een tekenreeksexpressie na hoofdletter gegevens converteren naar kleine letters. |
| [UPPER (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper) |Retourneert een tekenreeksexpressie na kleine letter gegevens converteren naar hoofdletters. |
| [Vervang (str_expr, str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace) |Vervangt alle instanties van een opgegeven tekenreeks-waarde met de waarde van een andere tekenreeks. |
| [REPLICEREN (str_expr, num_expr)](https://docs.microsoft.com/azure/cosmos-db/sql-api-sql-query-reference#bk_replicate) |Een tekenreekswaarde die herhaald een opgegeven aantal keren. |
| [REVERSE (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse) |Retourneert de omgekeerde volgorde van een string-waarde. |

U kunt nu een query's als volgt uitvoeren met behulp van deze functies. Bijvoorbeeld, kunt u de familienaam in retourneren hoofdletters als volgt:

**Query**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Results**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Of tekenreeksen, zoals in dit voorbeeld:

**Query**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Results**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Tekenreeksfuncties kunnen ook worden gebruikt in de WHERE-component om resultaten te filteren, zoals in het volgende voorbeeld:

**Query**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Results**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>Matrixfuncties
De volgende scalaire functies uitvoeren van een bewerking op een matrix invoerwaarde en retourneren numerieke, Booleaanse waarde of een matrix-waarde. Hier volgt een tabel met ingebouwde matrixfuncties:

| Gebruik | Beschrijving |
| --- | --- |
| [ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length) |Retourneert het aantal elementen van de opgegeven matrix-expressie. |
| [ARRAY_CONCAT (arr_expr arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat) |Retourneert een matrix die het resultaat van het samenvoegen van twee of meer matrixwaarden. |
| [ARRAY_CONTAINS (arr_expr, markering [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains) |Retourneert een Booleaanse waarde die aangeeft of de matrix de opgegeven waarde bevat. Kunt opgeven als de overeenkomst volledig of gedeeltelijk is. |
| [ARRAY_SLICE (arr_expr num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice) |Onderdeel van een matrixexpressie retourneert. |

Matrixfuncties kunnen worden gebruikt voor het bewerken van matrices in JSON. Bijvoorbeeld, als volgt een query die alle documenten retourneert waarbij een van de ouders 'Robin Wakefield' is. 

**Query**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Results**

    [{
      "id": "WakefieldFamily"
    }]

U kunt een gedeeltelijke fragment voor het afstemmen van elementen in de matrix. De volgende query vindt u alle bovenliggende items met de `givenName` van `Robin`.

**Query**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)

**Results**

    [{
      "id": "WakefieldFamily"
    }]


Hier volgt een voorbeeld waarin ARRAY_LENGTH om het aantal onderliggende items per serie.

**Query**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Results**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Ruimtelijke-functies
Cosmos DB biedt ondersteuning voor de volgende Open georuimtelijke Consortium (OGC) ingebouwde functies voor georuimtelijke query's. 

<table>
<tr>
  <td><strong>Gebruik</strong></td>
  <td><strong>Beschrijving</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Geeft de afstand tussen de twee GeoJSON-punt, Polygon of LineString expressies.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Retourneert een Booleaanse expressie die aangeeft of het eerste GeoJSON-object (punt, Polygon of LineString) in het tweede GeoJSON-object (punt, Polygon of LineString).</td>
</tr>
<tr>
  <td>ST_INTERSECTS (spatial_expr, spatial_expr)</td>
  <td>Retourneert een Booleaanse expressie waarmee wordt aangegeven of de twee opgegeven GeoJSON-objecten (punt, Polygon of LineString) elkaar overlappen.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Retourneert een Booleaanse waarde die aangeeft of de opgegeven expressie voor de GeoJSON-punt, Polygon of LineString ongeldig is.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Retourneert een JSON-waarde met een Booleaanse waarde als de opgegeven expressie voor de GeoJSON-punt, Polygon of LineString geldig is en als ongeldig, ook de reden als een string-waarde.</td>
</tr>
</table>

Ruimtelijke functies kunnen worden gebruikt om uit te voeren van de service-query's op ruimtelijke gegevens. Bijvoorbeeld, als volgt een query waarmee alle familie documenten die binnen 30 kilometer van de opgegeven locatie met behulp van de ingebouwde functie ST_DISTANCE retourneert. 

**Query**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Results**

    [{
      "id": "WakefieldFamily"
    }]

Zie voor meer informatie over ondersteuning voor georuimtelijke in Cosmos DB [werken met georuimtelijke gegevens in Azure Cosmos DB](geospatial.md). Afgerond ruimtelijke functies en de SQL-syntaxis voor Cosmos DB. Nu eens kijken hoe LINQ-query's werkt en hoe deze samenwerkt met de syntaxis van de we hebben gezien tot nu toe.

## <a id="Linq"></a>LINQ naar SQL-API
LINQ is een .NET-programmeermodel die berekening als query's voor streams van objecten. Cosmos DB biedt een client-side-bibliotheek voor de interactie met LINQ doordat een conversie tussen JSON- en .NET-objecten en een toewijzing van een subset van de LINQ-query's naar Cosmos DB-query's. 

De volgende afbeelding toont de architectuur van het LINQ-query's met Cosmos DB ondersteunt.  Met de Cosmos DB-client, kunnen ontwikkelaars maken een **IQueryable** -object dat de Cosmos DB-query-provider, die vervolgens de LINQ-query in een Cosmos DB-query zet rechtstreeks een query uitvoert. De query wordt vervolgens doorgegeven aan de Cosmos DB-server om een set resultaten in JSON-indeling te halen. De geretourneerde resultaten worden in een stream van .NET-objecten aan de clientzijde gedeserialiseerd.

![Architectuur van het LINQ-query's met de SQL-API - SQL-syntaxis, querytaal voor JSON, database-concepten en SQL-query's ondersteunen][1]

### <a name="net-and-json-mapping"></a>.NET- en JSON-toewijzing
De toewijzing tussen .NET-objecten en JSON-documenten is natuurlijk - elk gegevensveld lid is toegewezen aan een JSON-object, waarbij de naam van het veld is toegewezen aan het gedeelte 'sleutel' van het object en het gedeelte "waarde" recursief toegewezen aan het waardedeel van het object. Houd rekening met het volgende voorbeeld: de familie-object gemaakt is toegewezen aan het JSON-document, zoals hieronder wordt weergegeven. En omgekeerd, het JSON-document is toegewezen aan een .NET-object.

**C#-klasse**

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };

    public struct Parent
    {
        public string familyName;
        public string givenName;
    };

    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };

    public class Pet
    {
        public string givenName;
    };

    public class Address
    {
        public string state;
        public string county;
        public string city;
    };

    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


**JSON**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-to-sql-translation"></a>LINQ tot vertaling van SQL
De Cosmos DB-provider query voert een aanbevolen inspanning toewijzing van een LINQ-query in een Cosmos DB SQL-query. In de volgende beschrijving, nemen we aan dat de lezer heeft een mooi van LINQ.

Voor het typesysteem ondersteunen we eerst alle JSON primitieve typen – numerieke typen, Booleaanse waarde, string en null. Alleen deze JSON-typen worden ondersteund. De volgende scalaire expressies worden ondersteund.

* Constante waarden – hierbij constante waarden van de primitieve gegevenstypen op het moment dat de query wordt geëvalueerd.
* De eigenschap/matrix indexexpressies: deze expressies verwijzen naar de eigenschap van een object of een element van de matrix.
  
     -familie. ID;    Family.children[0].familyName;    Family.children[0].Grade;    Family.children[n].Grade; n is een int-variabele
* Wiskundige expressies - hierbij veelgebruikte wiskundige expressies op basis van numerieke en Booleaanse waarden. Raadpleeg de SQL-specificatie voor de volledige lijst.
  
     2 * family.children[0].grade;    x + y;
* Vergelijking van tekenreeksexpressie - hierbij een string-waarde naar een constante tekenreeks-waarde te vergelijken.  
  
     mother.familyName == 'Smith';    child.givenName == s; s is een string-variabele
* Object/matrix expressie voor het maken - deze expressies retourneren een object van samengestelde waarde of een anoniem type of een matrix met objecten. Deze waarden kunnen worden genest.
  
     nieuwe bovenliggende {familyName 'Smith', givenName = = "Jaap"}; Nieuw {eerst = 1, tweede = 2}; een anoniem type met twee velden              
     nieuwe int [] {3, child.grade, 5};

### <a id="SupportedLinqOperators"></a>Lijst met ondersteunde LINQ-operators
Hier volgt een lijst met ondersteunde LINQ-operators in het LINQ-provider en de SQL .NET SDK is opgenomen.

* **Selecteer**: projecties vertalen naar de SQL SELECT-objectconstructie inclusief
* **Waar**: Filters vertalen naar SQL waar en ondersteuning voor de conversie van & &, || en! de SQL-operators
* **SelectMany**: kunt ongedaan maken van matrices naar de SQL-JOIN-component. Kan worden gebruikt om expressies te filteren op matrixelementen keten/nesten
* **OrderBy en OrderByDescending**: vertaalt in ORDER BY oplopend/aflopend
* **Aantal**, **som**, **Min**, **Max**, en **gemiddelde** operators voor aggregatie en hun equivalenten asynchrone **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, en **AverageAsync**.
* **CompareTo**: wordt omgezet in het bereik vergelijkingen. Doorgaans gebruikt voor tekenreeksen, omdat ze niet vergelijkbaar in .NET
* **Nemen**: vertaalt naar de BOVENKANT van de SQL voor het beperken van de resultaten van een query
* **Wiskundige functies**: biedt ondersteuning voor vertaling van. De NET Abs, Acos, Asin, Atan, maximum, Cos, Exp, Floor, Log, Log10, Pow, Round, ondertekenen, Sin, WORTEL, Tan, Truncate naar de equivalente ingebouwde functies van SQL.
* **Functies de tekenreeks**: biedt ondersteuning voor vertaling van. NET van Concat, bevat EndsWith, IndexOf, aantal, ToLower, TrimStart, vervangen, omgekeerd, TrimEnd, StartsWith, subtekenreeks, ToUpper naar de equivalente ingebouwde functies van SQL.
* **Matrix van functies**: biedt ondersteuning voor vertaling van. De NET Concat, bevat en aantal naar de equivalente ingebouwde functies van SQL.
* **Georuimtelijke Extensiefuncties**: biedt ondersteuning voor vertaling van stub-methoden afstand binnen IsValid en IsValidDetailed naar de equivalente ingebouwde functies van SQL.
* **Gebruiker gedefinieerde functie-extensie**: biedt ondersteuning voor vertaling van de methode stub UserDefinedFunctionProvider.Invoke naar de bijbehorende door de gebruiker gedefinieerde functie.
* **Diverse**: biedt ondersteuning voor vertalingen van het samenvoegen en voorwaardelijke operators. Bevat tekenreeks bevat, ARRAY_CONTAINS of de IN SQL, afhankelijk van de context kunnen worden vertaald.

### <a name="sql-query-operators"></a>SQL-query-operators
Hier volgen enkele voorbeelden die laten zien hoe u enkele van de standaardoperators voor query van LINQ naar Cosmos DB-query's worden omgezet.

#### <a name="select-operator"></a>Operator selecteren
De syntaxis is `input.Select(x => f(x))`, waarbij `f` is een scalaire expressie.

**LINQ lambda-expressie**

    input.Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**LINQ lambda-expressie**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**LINQ lambda-expressie**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>SelectMany operator
De syntaxis is `input.SelectMany(x => f(x))`, waarbij `f` is een scalaire expressie die een verzamelingtype geretourneerd.

**LINQ lambda-expressie**

    input.SelectMany(family => family.children);

**SQL** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>Waar operator
De syntaxis is `input.Where(x => f(x))`, waarbij `f` is een scalaire expressie die een Booleaanse waarde retourneert.

**LINQ lambda-expressie**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**LINQ lambda-expressie**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>Samengestelde SQL-query 's
De bovenstaande operators kunnen worden samengesteld uit om te krachtige query's vormen. Sinds de Cosmos DB biedt ondersteuning voor geneste verzamelingen, kan de samenstelling worden samengevoegd of genest.

#### <a name="concatenation"></a>Samenvoegen
De syntaxis is `input(.|.SelectMany())(.Select()|.Where())*`. Een samengevoegde query kunt beginnen met een optionele `SelectMany` query gevolgd door meerdere `Select` of `Where` operators.

**LINQ lambda-expressie**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**LINQ lambda-expressie**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**LINQ lambda-expressie**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**LINQ lambda-expressie**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>Nesten
De syntaxis is `input.SelectMany(x=>x.Q())` waar Q is een `Select`, `SelectMany`, of `Where` operator.

In een geneste query, wordt de binnenste query toegepast op elk element van de buitenste verzameling. Een belangrijk onderdeel is dat de binnenste query naar de velden van de elementen in de buitenste verzameling zoals verwijzen kan zelf-joins.

**LINQ lambda-expressie**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**LINQ lambda-expressie**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**LINQ lambda-expressie**

    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a id="ExecutingSqlQueries"></a>SQL-query's uitvoeren
Cosmos DB Ontsluit resources via een REST-API die kan worden aangeroepen in elke taal waarmee HTTP/HTTPS-aanvragen. Cosmos DB biedt bovendien programmeringsbibliotheken voor verschillende veelgebruikte talen zoals .NET, Node.js, JavaScript en Python. De REST-API en de verschillende bibliotheken ondersteuning voor het uitvoeren van query's via SQL. De .NET SDK biedt ondersteuning voor LINQ-query's uitvoeren naast SQL.

De volgende voorbeelden ziet hoe u een query maken en verzenden op basis van een Cosmos DB-databaseaccount geldt.

### <a id="RestAPI"></a>REST-API
Cosmos DB biedt een open RESTful-programmeermodel via HTTP. Database-accounts kunnen worden ingericht met behulp van een Azure-abonnement. Het Cosmos DB-resourcemodel bestaat uit een set met resources onder een databaseaccount,, die elk opgevraagd met de URI van een logische en stabiel is. Een set met resources wordt aangeduid als een feed in dit document. Een databaseaccount bestaat uit een reeks databases, die elk meerdere verzamelingen, met elk van die beurt documenten, UDF's en andere resourcetypen bevatten.

Het model basic interactie met deze resources is via de HTTP-woorden GET, PUT, POST en verwijderen met hun standaard vertaling. De POST-bewerking wordt gebruikt voor het maken van een nieuwe resource, voor het uitvoeren van een opgeslagen procedure of voor het uitgeven van een Cosmos DB-query. Query's zijn altijd alleen-lezen bewerkingen met geen neveneffecten.

De volgende voorbeelden ziet een bericht voor een SQL-API-query uitgevoerd voor een verzameling met de twee voorbeelddocumenten dat tot nu toe hebt gezien. De query is een eenvoudige filter op de naameigenschap JSON. Let op het gebruik van de `x-ms-documentdb-isquery` en Content-Type: `application/query+json` headers om aan te duiden dat de bewerking een query wordt.

**Aanvraag**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }


**Results**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


Het tweede voorbeeld ziet een complexere query die meerdere resultaten uit de join retourneert.

**Aanvraag**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**Results**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


Als de resultaten van een query niet binnen één pagina met resultaten past en vervolgens de REST-API een vervolgtoken via retourneert de `x-ms-continuation-token` response-header. Clients kunnen resultaten pagineren door op te nemen van de koptekst in de volgende resultaten. Het aantal resultaten per pagina kan ook worden beheerd via de `x-ms-max-item-count` getal-header. Als de opgegeven query een statistische functie, zoals bevat `COUNT`, de querypagina kan een gedeeltelijk geaggregeerde waarde geretourneerd via de pagina met resultaten. De clients moeten een aggregatie op het tweede niveau uitvoeren via deze resultaten het uiteindelijke resultaat, bijvoorbeeld, som via de aantallen die worden geretourneerd in de afzonderlijke pagina's om de totale telling te retourneren.

Voor het beheren van het beleid van de consistentie van gegevens voor query's, gebruikt u de `x-ms-consistency-level` header, zoals alle REST API-aanvragen. Sessieconsistentie, deze is vereist voor de meest recente ook echo `x-ms-session-token` Cookie-header in de query-aanvragen. De aangevraagde verzameling indexeringsbeleid kan ook invloed hebben op de consistentie van de queryresultaten. Met de standaardbeleidsregels voor indexering beleidsinstellingen, voor verzamelingen van de index is altijd actueel met inhoud van het document en queryresultaten overeenkomen met de consistentie gekozen voor gegevens. Als het indexeringsbeleid is soepele naar Lazy, kunnen query's verlopen resultaten geretourneerd. Zie voor meer informatie, [Azure Cosmos DB-Consistentieniveaus][consistency-levels].

Als de opgegeven query kan niet worden ondersteund door het geconfigureerde indexeringsbeleid op de verzameling, retourneert de Azure Cosmos DB-server 400 'Ongeldige aanvraag". Dit wordt geretourneerd voor de bereik-query's op paden die zijn geconfigureerd voor lookups op hash (gelijkheid) en voor paden expliciet is uitgesloten van het indexeren. De `x-ms-documentdb-query-enable-scan` header kan worden opgegeven voor de query een scan uitvoeren als een index niet beschikbaar is.

U kunt gedetailleerde metrische gegevens krijgen bij uitvoeren van query's door in te stellen `x-ms-documentdb-populatequerymetrics` koptekst `True`. Zie voor meer informatie, [SQL query metrische gegevens voor Azure Cosmos DB](sql-api-sql-query-metrics.md).

### <a id="DotNetSdk"></a>SDK VOOR C# (.NET)
De .NET SDK biedt ondersteuning voor zowel SQL als LINQ uitvoeren van query's. Het volgende voorbeeld ziet hoe u de eenvoudige filterquery die eerder in dit document is geïntroduceerd.

    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


In dit voorbeeld vergelijkt twee eigenschappen voor gelijkheid binnen elk document en maakt gebruik van anonieme projecties. 

    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Het volgende voorbeeld toont joins, uitgedrukt via LINQ SelectMany.

    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }

    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



De .NET-client worden automatisch alle pagina's van de resultaten van de query in de foreach-blokken zoals hierboven doorlopen. De opties voor query's die zijn geïntroduceerd in de sectie REST-API zijn ook beschikbaar in de .NET-SDK met de `FeedOptions` en `FeedResponse` klassen in de methode CreateDocumentQuery. Het aantal pagina's kan worden beheerd met behulp van de `MaxItemCount` instelling. 

U kunt ook expliciet paginering beheren met het maken van `IDocumentQueryable` met behulp van de `IQueryable` object, klikt u vervolgens met het lezen van de` ResponseContinuationToken` waarden en geven ze weer als `RequestContinuationToken` in `FeedOptions`. `EnableScanInQuery` kan worden ingesteld op scans in te schakelen wanneer de query kan niet worden ondersteund door het geconfigureerde beleid voor indexering. Voor gepartitioneerde verzamelingen, kunt u `PartitionKey` aan de query uitvoeren op een enkele partitie (Hoewel Cosmos DB dit automatisch uit de querytekst extraheren kan), en `EnableCrossPartitionQuery` query's uitvoeren die mogelijk voor meerdere partities worden uitgevoerd. 

Raadpleeg [Azure Cosmos DB .NET-voorbeelden](https://github.com/Azure/azure-documentdb-net) voor meer voorbeelden met query's. 

### <a id="JavaScriptServerSideApi"></a>Server-side '-JavaScript-API
Cosmos DB biedt een programmeermodel voor het uitvoeren van op basis van JavaScript-toepassingslogica rechtstreeks op de verzamelingen met behulp van opgeslagen procedures en triggers. De JavaScript-logica die is geregistreerd op het niveau van een verzameling kan vervolgens databasebewerkingen op de bewerkingen op de documenten van de desbetreffende verzameling. Deze bewerkingen zijn verpakt in ambient ACID-transactions.

Het volgende voorbeeld laat zien hoe de queryDocuments op de server in JavaScript API gebruiken om te maken van query's uit binnen opgeslagen procedures en triggers.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()

        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a id="References"></a>Verwijzingen
1. [Inleiding tot Azure Cosmos DB][introduction]
2. [Azure Cosmos DB SQL-specificatie](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3. [Azure Cosmos DB .NET-voorbeelden](https://github.com/Azure/azure-documentdb-net)
4. [Azure Cosmos DB-Consistentieniveaus][consistency-levels]
5. ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6. JSON [http://json.org/](http://json.org/)
7. JavaScript-specificatie [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8. LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9. Query-evaluatietechnieken voor grote databases [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)
10. Verwerking van parallelle relationele databasesystemen, druk op de samenleving van IEEE Computer 1994 query
11. Lu, Ooi, Tan, queryverwerking in parallelle relationele databasesystemen, druk op de samenleving van IEEE Computer 1994.
12. Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: Pig Latin: een niet zo vreemde taal voor gegevensverwerking, SIGMOD 2008.
13. G. Graefe. Het framework trapsgewijs voor queryoptimalisatie. IEEE-gegevens eng Bull., 18, lid 3: 1995.

[1]: ./media/sql-api-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md
