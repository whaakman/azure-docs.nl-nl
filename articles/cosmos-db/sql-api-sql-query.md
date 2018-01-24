---
title: SQL-query's voor Azure Cosmos DB | Microsoft Docs
description: Meer informatie over SQL-syntaxis, database-concepten en SQL-query's voor Azure Cosmos DB. SQL kan worden gebruikt als een JSON-querytaal in Azure Cosmos DB.
keywords: SQL-syntaxis, sql-query, sql-query's, json-querytaal, database-concepten en sql-query's, statistische functies
services: cosmos-db
documentationcenter: 
author: LalithaMV
manager: jhubbard
editor: monicar
ms.assetid: a73b4ab3-0786-42fd-b59b-555fce09db6e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: laviswa
ms.openlocfilehash: 69466b15d2a37bee0353a283c9bab59563f3670e
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="sql-queries-for-azure-cosmos-db"></a>SQL-query's voor Azure Cosmos-DB

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Microsoft Azure Cosmos DB ondersteunt documentquery's die gebruikmaken van SQL (Structured Query Language) als een JSON-querytaal op SQL-API-accounts. Azure Cosmos DB is volledig zonder schema. Doordat het gebied van de JSON-gegevensmodel rechtstreeks in de database-engine biedt deze automatische indexering van JSON-documenten zonder expliciet schema of het maken van secundaire indexen.

Tijdens het ontwerpen van de query language voor Cosmos DB, zijn twee drie doelen voor ogen:

* We wilden in plaats van een nieuwe JSON-querytaal vruchtbare, ter ondersteuning van SQL. SQL is een van de bekend en meest populaire querytalen. Cosmos DB SQL biedt een formele programmeermodel voor uitgebreide query's via JSON-documenten.
* We wilden van JavaScript-programmeermodel gebruikt als basis voor de querytaal als een JSON-document-database kan worden uitgevoerd van JavaScript rechtstreeks in de database-engine. De SQL-API is in de JavaScript-typesysteem, evaluatie van expressies en functieaanroep geroot. Deze beurt biedt een natuurlijke programmeermodel voor projecties relationele, hiërarchische navigatie in JSON-documenten, self joins, ruimtelijke query's en aanroep van de gebruiker gedefinieerde functies (UDF's) geschreven volledig in JavaScript, onder andere functies. 

We zijn ervan overtuigd dat deze mogelijkheden essentieel zijn voor het reduceren van de wrijving tussen de toepassing en de database en essentieel voor de productiviteit van ontwikkelaars zijn.

We raden aan de slag door het bekijken van de volgende video, Aravind Ramachandran waar de Cosmos-DB querymogelijkheden toont, en in via onze [Queryspeelplaats](http://www.documentdb.com/sql/demo), waarbij u kunt Cosmos-database uitproberen en SQL-query's op onze gegevensset uitvoeren.

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/DataExposedQueryingDocumentDB/player]
> 
> 

Keer vervolgens terug naar dit artikel, waar u begint met een SQL-query-zelfstudie die u bij enkele eenvoudige JSON-documenten en de SQL-opdrachten helpt.

## <a id="GettingStarted"></a>Aan de slag met SQL-opdrachten in een Cosmos-DB
Als u Cosmos DB SQL op het werk, laten we beginnen met een paar eenvoudige JSON-documenten en enkele eenvoudige query's op deze doorlopen. U kunt deze twee JSON-documenten over twee families. Met Cosmos-DB hoeft we niet expliciet een schema's of secundaire indexen maken. Moet u de JSON-documenten aan een verzameling Cosmos DB invoegen en vervolgens een query. Hier hebben we een eenvoudige JSON-document voor de Andersen family, de bovenliggende items, onderliggende elementen (en hun huisdieren), adres en registratie-informatie. Het document heeft tekenreeksen, getallen, Booleaanse waarden, matrices en geneste eigenschappen. 

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

Hier volgt een tweede document een subtiele verschil – `givenName` en `familyName` worden gebruikt in plaats van `firstName` en `lastName`.

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

Nu gaan we enkele query's op deze gegevens om een idee van de belangrijkste aspecten van de SQL-querytaal Azure Cosmos DB te proberen. De volgende query retourneert bijvoorbeeld de documenten waarbij het veld id overeenkomt met `AndersenFamily`. Omdat het een `SELECT *`, de uitvoer van de query is de volledige JSON-document:

**Query**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

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


Bekijk nu het geval waar moeten we opnieuw indelen van de JSON-uitvoer in een andere vorm. Deze query projecteert een nieuwe JSON-object met twee geselecteerde velden naam en plaats, wanneer het adres stad dezelfde naam als de status heeft. In dit geval 'NY, NY' komt overeen met.

**Query**    

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Resultaten**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


De volgende query retourneert de opgegeven namen van onderliggende items in de familie-id die overeenkomt met `WakefieldFamily` geordend op de plaats waar je woont.

**Query**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Resultaten**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


Wij willen graag aandacht te vestigen op enkele opmerkelijk aspecten van de Cosmos-DB-querytaal via de voorbeelden die we tot nu toe hebt gezien:  

* Omdat SQL-API op JSON-waarden werkt, behandelt structuur in de vorm van entiteiten in plaats van rijen en kolommen. De taal kunt u knooppunten van de structuur op elke willekeurige diepte zoals Raadpleeg daarom `Node1.Node2.Node3…..Nodem`, vergelijkbaar met relationele SQL verwijst naar de twee onderdeelverwijzing van `<table>.<column>`.   
* De structured query language werkt met schema minder gegevens. Daarom moet het typesysteem dynamisch worden gebonden. Verschillende typen op verschillende documenten kan resulteert in dezelfde expressie. Het resultaat van een query is een geldige JSON-waarde, maar van een vast schema kan niet worden gegarandeerd.  
* Cosmos DB ondersteunt alleen strikte JSON-documenten. Dit betekent dat de systeem- en expressies zijn beperkt tot alleen met JSON-typen zijn getroffen. Raadpleeg de [JSON-specificatie](http://www.json.org/) voor meer informatie.  
* Een Cosmos-DB-verzameling is een container zonder schema van JSON-documenten. De relaties in gegevensentiteiten binnen en tussen documenten in een verzameling worden impliciet door containment en niet door de primaire sleutel en refererende sleutels relaties vastgelegd. Dit is een belangrijk aspect waard wijzen op in het licht de intra-document wordt besproken verderop in dit artikel.

## <a id="Indexing"></a>Cosmos DB indexeren
Voordat we in de SQL-syntaxis ophalen, is het waard het ontwerp van de indexering in Azure Cosmos DB. 

Het doel van de indexen van de database is voor het uitvoeren van query's in hun diverse vormen en vormen met minimale brongebruik (zoals CPU en input/output) en tegelijkertijd goede doorvoer en lage latentie. Vaak is de keuze van de juiste index voor query's in een database vereist veel planning en experimenteren. Deze aanpak vormt een uitdaging voor schema-minder databases waarin de gegevens voldoen niet aan een strikte schema en snel ontwikkeld. 

Wanneer we de Cosmos-DB indexering subsysteem ontworpen, instellen wij daarom de volgende doelen:

* Documenten te indexeren zonder schema: het subsysteem voor indexering geen vereist een schema-informatie of veronderstellingen over schema van de documenten. 
* Ondersteuning voor efficiënte, geavanceerde hiërarchische en relationele query's: de index ondersteunt de Cosmos-DB-querytaal efficiënt, inclusief ondersteuning voor hiërarchische en relationele projecties.
* Ondersteuning voor consistente query's in face of een volgehouden volume van schrijfbewerkingen: voor schrijven met hoge doorvoer werkbelastingen met consistente query's, de index incrementeel efficiënt en online met betrekking tot een continue volume van schrijfbewerkingen wordt bijgewerkt. De consistente index-update is van cruciaal belang om te dienen als de query's op het consistentieniveau van de waarin de gebruiker het documentservice hebt geconfigureerd.
* Ondersteuning voor multitenancy: gegeven het model op basis van een reservering voor de resource governance tussen tenants, index-updates worden uitgevoerd binnen het budget van systeembronnen (CPU, geheugen en input/output-bewerkingen per seconde) die per replica worden toegewezen. 
* Opslagruimte: voor de kosteneffectiviteit, de schijfopslag op overhead van de index is gebonden en voorspelbaar. Dit is cruciaal omdat Cosmos DB kan de ontwikkelaar maken op basis van kosten-en nadelen tussen index overhead ten opzichte van de prestaties van query's.  

Raadpleeg de [Azure Cosmos DB voorbeelden](https://github.com/Azure/azure-documentdb-net) op MSDN voor voorbeelden waarin wordt getoond hoe het indexeringsbeleid voor een verzameling configureren. Laten we nu krijgen tot de details van de Azure Cosmos DB SQL-syntaxis.

## <a id="Basics"></a>Basisprincipes van een Azure Cosmos DB SQL-query
Elke query bestaat uit een SELECT-component en een optionele FROM en WHERE-componenten per ANSI SQL-standaarden. Normaal gesproken voor elke query moet de bron in de component FROM is geïnventariseerd. Het filter in de component WHERE wordt vervolgens toegepast op de bron voor het ophalen van een subset van JSON-documenten. Ten slotte wordt de SELECT-component gebruikt om de aangevraagde JSON-waarden in de select-lijst.

    SELECT <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a id="FromClause"></a>FROM-component
De `FROM <from_specification>` component is optioneel tenzij de bron is gefilterd of geprojecteerd verderop in de query. Het doel van deze component is om op te geven van de gegevensbron waarop de query moet functioneren. De volledige verzameling is vaak de bron, maar een kunt in plaats daarvan een subset van de verzameling opgeven. 

Een query, zoals `SELECT * FROM Families` geeft aan dat de volledige verzameling worden Families de bron via die u wilt inventariseren. Een speciale ROOT-id kan worden gebruikt voor de verzameling in plaats van met de naam van de verzameling. De volgende lijst bevat de regels die per query worden afgedwongen:

* De verzameling mag alias hebben, zoals `SELECT f.id FROM Families AS f` of gewoon `SELECT f.id FROM Families f`. Hier `f` is het equivalent van `Families`. `AS`is een optioneel trefwoord tot de alias van de id.
* Eenmaal alias hebben, de oorspronkelijke bron kan niet worden gebonden. Bijvoorbeeld: `SELECT Families.id FROM Families f` syntaxis is ongeldig omdat de id 'Families' kan niet meer worden omgezet.
* Alle eigenschappen die moeten worden verwezen moet volledig gekwalificeerd zijn. Dit wordt afgedwongen in het ontbreken van de toepassing van de strikte schema om te voorkomen dat geen bindingen niet eenduidig. Daarom `SELECT id FROM Families f` syntaxis is ongeldig omdat de eigenschap `id` niet is gebonden.

### <a name="subdocuments"></a>Subdocumenten
De bron kan ook worden teruggebracht naar een kleinere subset. Voor het exemplaar voor het inventariseren van alleen een substructuur in elk document kan de subroot dan de bron, zoals weergegeven in het volgende voorbeeld:

**Query**

    SELECT * 
    FROM Families.children

**Resultaten**  

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

Het bovenstaande voorbeeld wordt een matrix als bron gebruikt, een object kan ook worden gebruikt als bron is wat wordt weergegeven in het volgende voorbeeld: een geldige JSON-waarde (geen niet-gedefinieerd) die kan worden gevonden in de bron wordt beschouwd als voor opname in het resultaat van de query. Als sommige families beschikt niet over een `address.state` waarde, in de resultaten worden uitgesloten.

**Query**

    SELECT * 
    FROM Families.address.state

**Resultaten**

    [
      "WA", 
      "NY"
    ]


## <a id="WhereClause"></a>WHERE-component
De WHERE-component (**`WHERE <filter_condition>`**) is optioneel. Hiermee geeft u de voorwaarden die de JSON-documenten die zijn opgegeven door de bron moet voldoen om te worden opgenomen als onderdeel van het resultaat. De opgegeven voorwaarden op "true" worden beschouwd voor het resultaat moet worden geëvalueerd door elk JSON-document. De WHERE-component wordt gebruikt door de laag index om te bepalen van de absolute kleinste subset van de brondocumenten die deel van het resultaat uitmaken kunnen. 

De volgende query aanvragen documenten met een eigenschap name waarvan de waarde `AndersenFamily`. Elk document dat u beschikt niet over een eigenschap name of waar de waarde komt niet overeen met `AndersenFamily` is uitgesloten. 

**Query**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


Het vorige voorbeeld blijkt een eenvoudige gelijkheid-query. De SQL-API ondersteunt ook een groot aantal scalaire expressies. De meest gebruikte zijn binaire bestanden en unaire expressies. Eigenschap verwijzingen van de bron JSON-object zijn ook geldig expressies. 

De volgende binaire operators worden momenteel ondersteund en kunnen worden gebruikt in query's, zoals wordt weergegeven in de volgende voorbeelden:  

<table>
<tr>
<td>Rekenkundige bewerking</td>    
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Bitsgewijs</td>    
<td>|, &, ^, <<>>,, >>> (nul opvulling rechts verplaatsen)</td>
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
<td>Tekenreeks</td>    
<td>|| (samenvoegen)</td>
</tr>
</table>  


We gaan verdiepen in enkele query's met behulp van de binaire operators.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


De unaire operators +,-, ~ worden ook ondersteund, en niet kan worden gebruikt in query's, zoals wordt weergegeven in het volgende voorbeeld:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



Naast de binaire bestanden en unaire operators, zijn ook verwijzingen van de eigenschap toegestaan. Bijvoorbeeld: `SELECT * FROM Families f WHERE f.isRegistered` retourneert het JSON-document met de eigenschap `isRegistered` waarvoor de eigenschap waarde gelijk is aan de JSON `true` waarde. Alle overige waarden (false, null, niet-gedefinieerde, `<number>`, `<string>`, `<object>`, `<array>`, enzovoort) leidt tot het brondocument wordt uitgesloten van het resultaat. 

### <a name="equality-and-comparison-operators"></a>Gelijkheid en het vergelijkingstype operators
De volgende tabel geeft het resultaat van de gelijkheid vergelijkingen in de SQL-API tussen twee typen die JSON.

<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>Op</strong>
         </td>
         <td valign="top">
            <strong>Niet-gedefinieerd</strong>
         </td>
         <td valign="top">
            <strong>Null</strong>
         </td>
         <td valign="top">
            <strong>Boolean</strong>
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
            <strong>Array</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Niet-gedefinieerd<strong>
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
            <strong>Boolean<strong>
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
            <strong>Array<strong>
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

Voor andere vergelijkingsoperators zoals >, > =,! =, < en < =, de volgende regels toepassen:   

* Vergelijking van verschillende typen resulteert in Undefined.
* Vergelijking tussen de twee objecten of twee matrices resulteert in een Undefined.   

Als het resultaat van de scalaire expressie die in het filter is niet gedefinieerd, het bijbehorende document zou niet opgenomen in het resultaat, aangezien Undefined niet logisch neer op 'true'.

### <a name="between-keyword"></a>TUSSEN sleutelwoord
U kunt ook het sleutelwoord BETWEEN op query's op bereiken met waarden zoals in de ANSI SQL express gebruiken. TUSSEN kan worden gebruikt met tekenreeksen of getallen.

Deze query retourneert bijvoorbeeld alle familie documenten waarmee het eerste onderliggende klasse tussen de 1-5 (zowel liggen is). 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

In tegenstelling tot in de ANSI-SQL kun je de BETWEEN-component in de component FROM zoals in het volgende voorbeeld.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

Houd er rekening mee voor snellere tijden in uitvoering, voor het maken van een indexeringsbeleid die gebruikmaakt van een type bereik index op basis van een numerieke eigenschappen/paden die in de component BETWEEN zijn gefilterd. 

Het belangrijkste verschil tussen het gebruik van BETWEEN in de SQL-API en de ANSI SQL is dat u kunt snelle bereik een query uitgevoerd naar eigenschappen van gemengde gegevenstypen: bijvoorbeeld wellicht 'hoogwaardige' is geen getal (5) in een aantal documenten en tekenreeksen in andere gevallen ('grade4'). In dergelijke gevallen, wordt zoals in JavaScript, een vergelijking tussen twee verschillende typen resulteert in een 'niet-gedefinieerde' en het document overgeslagen.

### <a name="logical-and-or-and-not-operators"></a>Logische (AND, OR en NOT) operators
Logische operators bewerken Boole-waarden. De logische waarheid tabellen voor deze operators worden weergegeven in de volgende tabellen.

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
Het sleutelwoord kan worden gebruikt om te controleren of een opgegeven waarde komt overeen met elke waarde in een lijst. Deze query retourneert bijvoorbeeld alle familie documenten waar de id van 'WakefieldFamily' of 'AndersenFamily' is. 

    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

In dit voorbeeld retourneert alle documenten waarin de status is een van de opgegeven waarden.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Ternair (?) en operators Coalesce (?)
De operators Ternair en Coalesce kunnen worden gebruikt voor het bouwen van voorwaardelijke expressies, vergelijkbaar met populaire programmeertalen, zoals C# en JavaScript. 

De operator Ternair (?) is heel handig bij het maken van nieuwe JSON-eigenschappen op elk gewenst moment. Bijvoorbeeld, kunt nu u query's voor het classificeren van het niveau van de klasse in een menselijke leesbare vorm zoals beginnende/tussenliggende/Geavanceerd zoals hieronder wordt weergegeven.

     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

U kunt ook het aanroepen van de operator zoals in de onderstaande query nesten.

    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Als met andere operators query als de eigenschappen waarnaar wordt verwezen in de conditionele expressie ontbreken in elk document of als de typen worden vergeleken verschillen, vervolgens die documenten worden niet opgenomen in de queryresultaten.

De operator Coalesce (?) kan worden gebruikt voor het efficiënt controleren op de aanwezigheid van een eigenschap (ook is gedefinieerd) in een document. Dit is handig wanneer een query op semi-gestructureerde of gegevens van gemengde typen. Bijvoorbeeld, retourneert deze query 'Achternaam' indien aanwezig, of de 'Achternaam' als dit niet aanwezig.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a id="EscapingReservedKeywords"></a>Tussen aanhalingstekens eigenschapsaccessor
U kunt ook toegang tot eigenschappen met behulp van de operator tussen aanhalingstekens eigenschap `[]`. Bijvoorbeeld: `SELECT c.grade` en `SELECT c["grade"]` equivalent zijn. Deze syntaxis is handig wanneer u een eigenschap die spaties, speciale tekens bevat, of er gebeurt moet met dezelfde naam als een SQL-sleutelwoord of een gereserveerd woord escape.

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a id="SelectClause"></a>SELECT-component
De component SELECT (**`SELECT <select_list>`**) is verplicht en Hiermee geeft u de waarden die worden opgehaald uit de query, net zoals in ANSI SQL. De subset die boven op de brondocumenten zijn gefilterd op de Projectiefase, waarbij de opgegeven JSON-waarden worden opgehaald en een nieuwe JSON-object is samengesteld, voor elke doorgegeven op deze invoer worden doorgegeven. 

Het volgende voorbeeld toont een typische SELECT-query. 

**Query**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>Geneste eigenschappen
In het volgende voorbeeld we twee geneste eigenschappen projecteert `f.address.state` en `f.address.city`.

**Query**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Projectie biedt ook ondersteuning voor JSON-expressies, zoals wordt weergegeven in het volgende voorbeeld:

**Query**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Bekijk de rol van `$1` hier. De `SELECT` component moet een JSON-object maken en omdat er geen sleutel is opgegeven, gebruiken we de impliciete argument variabele namen die beginnen met `$1`. Deze query retourneert bijvoorbeeld twee impliciete argumentvariabelen, met het label `$1` en `$2`.

**Query**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

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
Nu gaan we het bovenstaande voorbeeld expliciete aliasing waarden uitbreiden. Het sleutelwoord gebruikt voor aliasing is. Dit is optioneel, zoals wordt weergegeven tijdens het projecteren van de tweede waarde als `NameInfo`. 

Als een query twee eigenschappen met dezelfde naam heeft, moet aliasing om de naam van een of beide van de eigenschappen, zodat ze ondubbelzinnig zijn gemaakt in het verwachte resultaat te worden gebruikt.

**Query**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

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
Naast de verwijzingen van de eigenschap ondersteunt de component SELECT ook scalaire expressies zoals constanten, aritmetische expressies, logische expressies, enzovoort. Hier is bijvoorbeeld een eenvoudige 'Hallo wereld'-query.

**Query**

    SELECT "Hello World"

**Resultaten**

    [{
      "$1": "Hello World"
    }]


Hier volgt een voorbeeld van een complexere die gebruikmaakt van een scalaire expressie.

**Query**

    SELECT ((2 + 11 % 7)-2)/3    

**Resultaten**

    [{
      "$1": 1.33333
    }]


In het volgende voorbeeld is het resultaat van de scalaire expressie die een Booleaanse waarde.

**Query**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f    

**Resultaten**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Maken van het object en array
Een andere belangrijke functie van de SQL-API is array-object maken. In het vorige voorbeeld, houd er rekening mee dat er een nieuwe JSON-object gemaakt. Op deze manier kan een ook matrices samenstellen zoals weergegeven in de volgende voorbeelden:

**Query**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f    

**Resultaten**  

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

### <a id="ValueKeyword"></a>WAARDE sleutelwoord
De **waarde** sleutelwoord biedt een manier om JSON-waarde te retourneren. De onderstaande query retourneert bijvoorbeeld scalaire `"Hello World"` in plaats van `{$1: "Hello World"}`.

**Query**

    SELECT VALUE "Hello World"

**Resultaten**

    [
      "Hello World"
    ]


De volgende query retourneert de waarde van de JSON zonder de `"address"` label in de resultaten.

**Query**

    SELECT VALUE f.address
    FROM Families f    

**Resultaten**  

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

Het volgende voorbeeld breidt deze optie als u wilt laten zien hoe u JSON primitieve waarden (het knooppuntniveau van de JSON-structuur) retourneren. 

**Query**

    SELECT VALUE f.address.state
    FROM Families f    

**Resultaten**

    [
      "WA",
      "NY"
    ]


### <a name="-operator"></a>* Operator
De speciale operator (*) wordt ondersteund voor het document als project-is. Wanneer gebruikt, moet dit het enige verwachte veld. Terwijl een query zoals `SELECT * FROM Families f` geldig is, `SELECT VALUE * FROM Families f ` en `SELECT *, f.id FROM Families f ` zijn niet geldig.

**Query**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultaten**

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

### <a id="TopKeyword"></a>Operator TOP
Het sleutelwoord TOP kan worden gebruikt om te beperken het aantal waarden van een query. Als eerste wordt gebruikt in combinatie met de component ORDER BY, is de resultatenset beperkt tot de eerste N aantal geordende waarden; anders wordt het eerste N aantal resultaten in een niet-gedefinieerde volgorde. Als een best practice in een instructie SELECT gebruik altijd een component ORDER BY met de TOP-component. Dit is de enige manier om voorspelbare aangeven welke rijen zijn beïnvloed door boven. 

**Query**

    SELECT TOP 1 * 
    FROM Families f 

**Resultaten**

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

TOP kan worden gebruikt met een constante waarde (zoals hierboven) of met de waarde van een variabele query's met parameters. Zie geparameteriseerde query's hieronder voor meer informatie.

### <a id="Aggregates"></a>Statistische functies
U kunt ook uitvoeren aggregaties in de `SELECT` component. Statistische functies uitvoeren van een berekening op een set waarden en één waarde retourneren. Bijvoorbeeld retourneert de volgende query de telling van familie documenten binnen de verzameling.

**Query**

    SELECT COUNT(1) 
    FROM Families f 

**Resultaten**

    [{
        "$1": 2
    }]

U kunt ook de scalaire waarde van de statistische functie retourneren met behulp van de `VALUE` sleutelwoord. De volgende query retourneert bijvoorbeeld het aantal waarden als een enkel getal:

**Query**

    SELECT VALUE COUNT(1) 
    FROM Families f 

**Resultaten**

    [ 2 ]

U kunt ook statistische functies uitvoeren in combinatie met filters. De volgende query retourneert bijvoorbeeld het aantal documenten met het adres in de staat Washington.

**Query**

    SELECT VALUE COUNT(1) 
    FROM Families f
    WHERE f.address.state = "WA" 

**Resultaten**

    [ 1 ]

De volgende tabel bevat de lijst met ondersteunde statistische functies in de SQL-API. `SUM`en `AVG` via numerieke waarden worden uitgevoerd terwijl `COUNT`, `MIN`, en `MAX` via getallen, tekenreeksen, Booleaanse waarden en null-waarden kunnen worden uitgevoerd. 

| Gebruik | Beschrijving |
|-------|-------------|
| AANTAL | Retourneert het aantal items in de expressie. |
| SOM   | Retourneert de som van alle waarden in de expressie. |
| MIN   | Retourneert de minimale waarde in de expressie. |
| MAX   | Retourneert de maximumwaarde in de expressie. |
| GEM   | Retourneert het gemiddelde van de waarden in de expressie. |

Statistische functies kunnen ook worden uitgevoerd via de resultaten van een matrix herhaling. Zie voor meer informatie [herhaling van de matrix in query's](#Iteration).

> [!NOTE]
> Wanneer u de Azure portal Queryverkenner, houd er rekening mee dat aggregatie van query's kunnen het resultaat gedeeltelijk geaggregeerde via een querypagina. De SDK's produceert één cumulatieve waarde op alle pagina's. 
> 
> Als u wilt uitvoeren met code aggregatie-query's, moet u de .NET SDK 1.12.0, .NET Core SDK 1.1.0 of Java SDK 1.9.5 of hoger.    
>

## <a id="OrderByClause"></a>ORDER BY-component
Zoals in de ANSI-SQL, u een optionele component Order By tijdens het opvragen opnemen kunt. De component kan bevatten een optioneel argument ASC/DESC de volgorde waarin de resultaten moeten worden opgehaald.

Hier is bijvoorbeeld een query waarmee families in volgorde van de residente plaatsnaam opgehaald.

**Query**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city

**Resultaten**

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

En een query waarmee opgehaald families in volgorde van de aanmaakdatum, die wordt opgeslagen als een getal dat de epoche tijd, Internet Explorer, verstreken tijd sinds 1 januari 1970 in seconden voor hier.

**Query**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC

**Resultaten**

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
Een nieuwe constructie is toegevoegd de **IN** -sleutelwoord in de SQL-API om ondersteuning te bieden met iteratie van JSON-matrices. De FROM-bron biedt ondersteuning voor herhaling. Laten we beginnen met het volgende voorbeeld:

**Query**

    SELECT * 
    FROM Families.children

**Resultaten**  

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

Nu we bekijken een andere query die herhaling via onderliggende elementen in de verzameling uitvoert. Noteer het verschil in de uitvoermatrix. In dit voorbeeld wordt gesplitst `children` en worden de resultaten samengevoegd in één array.  

**Query**

    SELECT * 
    FROM c IN Families.children

**Resultaten**  

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

**Resultaten**  

    [{
      "givenName": "Lisa"
    }]

U kunt ook aggregatie uitvoeren via het resultaat van de matrix herhaling. Bijvoorbeeld telt de volgende query het aantal onderliggende items onder alle families.

**Query**

    SELECT COUNT(child) 
    FROM child IN Families.children

**Resultaten**  

    [
      { 
        "$1": 3
      }
    ]

### <a id="Joins"></a>Joins
In een relationele database is de noodzaak om toe te voegen tussen de tabellen belangrijk. Het is het gevolg van logische ontwerpen genormaliseerde schema's. De SQL-API behandelt het gedenormaliseerd gegevensmodel zonder schema documenten met dit. Dit is het logische equivalent van een 'self-join'.

De syntaxis die ondersteuning biedt voor de taal is < from_source1 > JOIN < from_source2 > JOIN... JOIN < from_sourceN >. Over het algemeen dit retourneert een reeks **N**- tuples (tuple met **N** waarden). Elke tuple heeft geproduceerd door alle verzameling aliassen iteratie van hun respectieve sets waarden. Dit is met andere woorden, een volledige vectorproduct van de sets die deel uitmaken van de join.

De volgende voorbeelden ziet de werking van de JOIN-component. In het volgende voorbeeld wordt het resultaat is leeg omdat het vectorproduct van elk document van de bron en een lege set is leeg.

**Query**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Resultaten**  

    [{
    }]


In het volgende voorbeeld wordt de join is tussen de hoofdmap van het document en de `children` subroot. Het is een vectorproduct tussen twee JSON-objecten. Het feit dat onderliggende elementen is een matrix is niet effectief in de JOIN omdat we werkt met een enkele basis die de onderliggende matrix is. Het resultaat bevat daarom slechts twee resultaten, omdat het vectorproduct van elk document met de matrix exact slechts één document geeft.

**Query**

    SELECT f.id
    FROM Families f
    JOIN f.children

**Resultaten**

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

**Resultaten**

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



Het eerste dat te weten is dat de `from_source` van de **JOIN** component is een iterator. Ja, de stroom in dit geval is als volgt:  

* Vouw van elk onderliggend element **c** in de matrix.
* Toepassen van een vectorproduct met de hoofdmap van het document **f** met elk onderliggend element **c** dat is samengevoegd in de eerste stap.
* Ten slotte het hoofdobject project **f** naameigenschap alleen. 

Het eerste document (`AndersenFamily`) bevat slechts één onderliggend element, zodat de resultatenset bevat slechts één object hoort bij dit document. Het tweede document (`WakefieldFamily`) twee onderliggende items bevat. Het vectorproduct produceert dus een afzonderlijke object voor elke onderliggende, waardoor wat resulteert in twee objecten, één voor elke onderliggende overeenkomt met dit document. De basis-velden in beide deze documenten zijn hetzelfde, net zoals u in een vectorproduct verwacht.

Het echte hulpprogramma van de JOIN is van het vectorproduct in een shape die anders moeilijk te project naar formulier tuples. Bovendien ziet u in het onderstaande voorbeeld kon u filteren op de combinatie van een tuple dat kiest, kunnen de gebruiker heeft ervoor gekozen een voorwaarde voldaan door de tuples algemene.

**Query**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets

**Resultaten**

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



In dit voorbeeld is een natuurlijke extensie van het vorige voorbeeld en voert een dubbele koppeling. Het vectorproduct kan dus worden weergegeven als de volgende pseudo code:

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

`AndersenFamily`heeft één onderliggende die één huisdier heeft. Dus het vectorproduct resulteert in een rij is (1\*1\*1) van deze reeks. WakefieldFamily echter twee onderliggende items heeft, maar slechts één onderliggende 'Jesse' huisdieren heeft. Jesse heeft echter twee huisdieren. Daarom het vectorproduct 1 levert\*1\*2 = 2 rijen uit deze reeks.

In het volgende voorbeeld, er is een extra filter op `pet`. Dit omvat niet alle tuples waar de pet-naam geen is 'Schaduwkopie'. U ziet dat we kunnen bouwen tuples uit matrices, filter op een van de elementen van de tuple en een combinatie van de elementen project. 

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

**Resultaten**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a id="JavaScriptIntegration"></a>Integratie van JavaScript
Azure Cosmos DB biedt een programmeermodel voor het uitvoeren op basis van JavaScript-toepassingslogica rechtstreeks op de verzamelingen in termen van opgeslagen procedures en triggers. Hiermee kunt u beide:

* Mogelijkheid tot high-performance transactionele CRUD-bewerkingen en een query uitgevoerd naar documenten in een verzameling doordat de diepe integratie van JavaScript-runtime rechtstreeks in de database-engine. 
* Een natuurlijke modellering van Controlestroom, variabele scopes en -toewijzing en integratie van primitieven met databasetransacties afhandeling van uitzonderingen. Raadpleeg de JavaScript-serverzijde programmeerbaarheid-documentatie voor meer informatie over Azure DB die Cosmos-ondersteuning voor integratie van JavaScript.

### <a id="UserDefinedFunctions"></a>Gebruiker gedefinieerde functies (UDF's)
De SQL-API biedt samen met de typen is al gedefinieerd in dit artikel, ondersteuning voor gebruiker gedefinieerde functies (UDF). In het bijzonder wordt scalaire UDF's waar de ontwikkelaars kunnen doorgeven in nul of meerdere argumenten en terug één argument resultaat geretourneerd ondersteund. Elk van deze argumenten wordt voor geldige JSON-waarden worden gecontroleerd.  

De SQL-syntaxis wordt uitgebreid ter ondersteuning van aangepaste toepassingslogica gebruik van deze door de gebruiker gedefinieerde functies. UDF's kunnen worden geregistreerd met de SQL-API en vervolgens naar worden verwezen als onderdeel van een SQL-query. De UDF's zijn in feite exquisitely ontworpen om te worden aangeroepen door query's. Als gevolg van deze keuze hebt UDF's geen toegang tot het contextobject met de andere JavaScript typen (opgeslagen procedures en triggers). Omdat de query's worden uitgevoerd als alleen-lezen, kunnen ze worden uitgevoerd op de primaire of op secundaire replica's. Daarom zijn UDF's ontworpen om uit te voeren op secundaire replica's in tegenstelling tot andere typen JavaScript.

Hieronder volgt een voorbeeld van hoe een UDF kan worden geregistreerd bij de database Cosmos DB specifiek onder een documentverzameling.

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

Het voorgaande voorbeeld maakt een UDF met de naam `REGEX_MATCH`. Deze twee waarden voor JSON-tekenreeks accepteert `input` en `pattern` en controleert of het eerste overeenkomt met het patroon die zijn opgegeven in de tweede van JavaScript string.match() functie te gebruiken.

We kunnen deze UDF nu gebruiken in een query in een projectie. UDF's moeten worden gekwalificeerd met de hoofdlettergevoelige voorvoegsel "udf." Wanneer aangeroepen vanuit query's. 

> [!NOTE]
> Voorafgaand aan 3-17-2015 ondersteund Cosmos DB UDF aanroepen zonder de 'udf." voorvoegsel zoals REGEX_MATCH() selecteren. Dit patroon van aanroepen is afgeschaft.  
> 
> 

**Query**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Resultaten**

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

**Resultaten**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


In principe UDF's geldig scalaire expressies zijn en kunnen worden gebruikt in projecties en filters. 

Als op de kracht van UDF's wilt uitbreiden, bekijken we een ander voorbeeld met voorwaardelijke logica:

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

**Resultaten**

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


Als de voorgaande voorbeelden presenteren, UDF's de kracht van JavaScript-taal geïntegreerd met de SQL-API biedt een uitgebreide programmeerbare interface hiervoor complexe procedurele, heeft voorwaardelijke logica met behulp van de ingebouwde mogelijkheden voor JavaScript-runtime.

De SQL-API biedt de argumenten voor de UDF's voor elk document in de bron in het huidige stadium (WHERE-component of SELECT-component) van de verwerking van de UDF. Het resultaat is opgenomen in de algehele pijplijn naadloos. Als de eigenschappen waarnaar wordt verwezen door de UDF parameters zijn niet beschikbaar in de JSON-waarde, de parameter wordt beschouwd als niet-gedefinieerde en daarom de UDF-aanroep voor het volledig is overgeslagen. Op dezelfde manier als het resultaat van de UDF gedefinieerd is, het niet opgenomen in het resultaat. 

Kortom zijn UDF's geweldige tools complexe bedrijfslogica doen als onderdeel van de query.

### <a name="operator-evaluation"></a>Evaluatie van operator
Cosmos DB, tekent werkt hetzelfde als met JavaScript-operators en de evaluatie-semantiek ingevolge is een JSON-database. Terwijl Cosmos DB probeert te behouden JavaScript-semantiek in termen van JSON-ondersteuning, wordt de evaluatie van de bewerking in sommige gevallen afwijkt.

In de SQL-API zijn in tegenstelling tot traditionele SQL de typen van waarden vaak niet bekend totdat de waarden uit de database zijn opgehaald. Om efficiënt query's worden uitgevoerd, wordt in de meeste van de operators strikt type vereisten hebben. 

De SQL-API biedt geen impliciete conversies, in tegenstelling tot JavaScript uitvoeren. Een query voor het exemplaar, zoals `SELECT * FROM Person p WHERE p.Age = 21` overeenkomt met de documenten met een eigenschap Age waarvan de waarde 21 is. Een ander document waarvan de eigenschap leeftijd overeenkomt met de tekenreeks '21' of andere mogelijk oneindige variaties, zoals '021', '21,0', '0021', '00021', enzovoort worden niet overeen. Zo wordt daarentegen JavaScript waar de tekenreekswaarden impliciet omgezet naar het getallen worden (op basis van de operator ex: ==). Deze keuze is van cruciaal belang voor efficiënte index die overeenkomen met in de SQL-API. 

## <a name="parameterized-sql-queries"></a>SQL-query's met parameters
Cosmos DB ondersteunt query's met parameters worden uitgedrukt met de vertrouwde @ notatie. Geparameteriseerde SQL biedt robuuste verwerken en de aanhalingstekens van gebruikersinvoer, onbedoeld blootstelling van gegevens via SQL-injectie voorkomen. 

U kunt bijvoorbeeld een query schrijven waarmee achternaam en status van adres als parameters neemt en vervolgens uitgevoerd voor verschillende waarden van de laatste naam en adres staat op basis van de invoer van gebruiker.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

Deze aanvraag kan vervolgens worden verzonden aan de Cosmos-database als een JSON-query met parameters zoals hieronder wordt weergegeven.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

Het argument voor TOP kan worden ingesteld met query's met parameters, zoals hieronder wordt weergegeven.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

Parameterwaarden mag geldige JSON (tekenreeksen, cijfers, Booleaanse waarden, null, zelfs als deze matrices of geneste JSON). Ook omdat Cosmos DB schema minder, zijn parameters niet gevalideerd met elk type.

## <a id="BuiltinFunctions"></a>Ingebouwde functies
Cosmos DB ondersteunt ook een aantal ingebouwde functies voor algemene bewerkingen, die kunnen worden gebruikt in query's als de gebruiker gedefinieerde functies (UDF's).

| Functie-groep          | Bewerkingen                                                                                                                                          |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Wiskundige functies  | ABS, maximum, EXP, FLOOR, logboek, LOG10, POWER, ROUND, aanmelding, SQRT, VIERKANT, geheel, BOOGCOS, ASIN, BOOGTAN, ATN2, Cosinus, COT, graden PI, radialen, SIN en TAN |
| Controle van de functies van het type | IS_ARRAY, IS_BOOL IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED en IS_PRIMITIVE                                                           |
| Tekenreeksfuncties        | CONCAT, bevat ENDSWITH, INDEX_OF, links, lengte, kleine, LTRIM, vervangen, REPLICEREN, REVERSE, rechts, RTRIM, STARTSWITH, SUBTEKENREEKS en hoofdletters       |
| Matrixfuncties         | ARRAY_CONCAT, ARRAY_CONTAINS ARRAY_LENGTH en ARRAY_SLICE                                                                                         |
| Ruimtelijke functies       | ST_DISTANCE, ST_WITHIN ST_INTERSECTS, ST_ISVALID en ST_ISVALIDDETAILED                                                                           | 

Als u een gebruiker gedefinieerde functie (UDF) waarvoor een ingebouwde functie nu beschikbaar is momenteel gebruikt, moet u de bijbehorende ingebouwde functie als is het verstandig om sneller worden uitgevoerd en efficiënter. 

### <a name="mathematical-functions"></a>Wiskundige functies
De rekenkundige functies elke uitvoeren van een berekening op basis van de invoerwaarden die zijn opgegeven als argumenten en een numerieke waarde retourneren. Hier volgt een lijst met ondersteunde ingebouwde, rekenkundige functies.


| Gebruik | Beschrijving |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [[ABS (num_expr)](#bk_abs) | Retourneert de absolute (positieve) waarde van de opgegeven numerieke expressie. |
| [MAXIMUM (num_expr)](#bk_ceiling) | Retourneert de kleinste waarde van geheel getal groter dan of gelijk zijn aan de opgegeven numerieke expressie. |
| [FLOOR (num_expr)](#bk_floor) | Retourneert het grootste gehele getal kleiner dan of gelijk zijn aan de opgegeven numerieke expressie. |
| [EXP (num_expr)](#bk_exp) | Retourneert de exponent van de opgegeven numerieke expressie. |
| [LOG (num_expr [,base])](#bk_log) | Retourneert de natuurlijke logaritme van de opgegeven numerieke expressie, of de logaritme met behulp van het opgegeven grondtal |
| [LOG10 (num_expr)](#bk_log10) | Retourneert de logaritmische waarde grondtal 10 van de opgegeven numerieke expressie. |
| [ROUND (num_expr)](#bk_round) | Retourneert een numerieke waarde, op het dichtstbijzijnde gehele getal afgerond. |
| [TRUNC (num_expr)](#bk_trunc) | Retourneert een numerieke waarde, afgekapt tot het dichtstbijzijnde gehele getal. |
| [SQRT (num_expr)](#bk_sqrt) | Retourneert de vierkantswortel van de opgegeven numerieke expressie. |
| [SQUARE (num_expr)](#bk_square) | Retourneert het kwadraat van de opgegeven numerieke expressie. |
| [VOEDING (num_expr, num_expr)](#bk_power) | Retourneert de kracht van de opgegeven numerieke expressie voor de opgegeven waarde. |
| [SIGN (num_expr)](#bk_sign) | Retourneert de waarde teken (-1, 0, 1) van de opgegeven numerieke expressie. |
| [ACOS (num_expr)](#bk_acos) | Retourneert de hoek in radialen, waarvan de cosinus de opgegeven numerieke expressie is. ook wel de boogcosinus genoemd. |
| [ASIN (num_expr)](#bk_asin) | Retourneert de hoek in radialen, waarvan de sinus de opgegeven numerieke expressie is. Dit wordt ook boogsinus genoemd. |
| [ATAN (num_expr)](#bk_atan) | Retourneert de hoek in radialen, waarvan de tangens de opgegeven numerieke expressie is. Dit wordt ook arctangens genoemd. |
| [ATN2 (num_expr)](#bk_atn2) | Als resultaat geeft de hoek in radialen tussen de positieve x-as en de ray vanuit de oorsprong naar het punt (y, x), waarbij x en y zijn de waarden van de twee opgegeven float-expressies. |
| [COS (num_expr)](#bk_cos) | Retourneert de trigonometrische cosinus van de opgegeven hoek in radialen in de opgegeven expressie. |
| [COT (num_expr)](#bk_cot) | Retourneert de trigonometrische cotangens van de opgegeven hoek in radialen in het opgegeven numerieke expressie. |
| [DEGREES (num_expr)](#bk_degrees) | Retourneert de overeenkomstige hoek in graden voor een hoek aangeduid in radialen. |
| [PI ()](#bk_pi) | Retourneert de constante waarde van PI. |
| [RADIANS (num_expr)](#bk_radians) | Retourneert radialen wanneer een numerieke expressie, in graden wordt ingevoerd. |
| [SIN (num_expr)](#bk_sin) | Retourneert de trigonometrische sinus van de opgegeven hoek in radialen in de opgegeven expressie. |
| [TAN (num_expr)](#bk_tan) | Retourneert de tangens van de invoer expressie in de opgegeven expressie. |

U kunt nu bijvoorbeeld query's als volgt uitvoeren:

**Query**

    SELECT VALUE ABS(-4)

**Resultaten**

    [4]

Het belangrijkste verschil tussen Cosmos-database functies ten opzichte van ANSI SQL is dat ze zijn ontworpen om te werken goed met schemagegevens zonder schema en een gemengde. Als u hebt een document waarbij de eigenschap Size ontbreekt of heeft een niet-numerieke waarde, zoals 'Onbekend' vervolgens het document is overgeslagen, klikt u in plaats van een fout geretourneerd.

### <a name="type-checking-functions"></a>Controle van de functies van het type
Het type controleren of functies kunnen u om te controleren van het type van een expressie in SQL-query's. Type controleren of functies kunnen worden gebruikt om te bepalen het type van de eigenschappen in een-documenten wanneer deze variabele of een onbekend. Hier volgt een lijst met ondersteunde ingebouwde typecontrole functies.

<table>
<tr>
  <td><strong>Gebruik</strong></td>
  <td><strong>Beschrijving</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (expr)</a></td>
  <td>Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een matrix is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (expr)</a></td>
  <td>Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een Booleaanse waarde is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (expr)</a></td>
  <td>Retourneert een Booleaanse waarde die aangeeft of het type van de waarde null is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (expr)</a></td>
  <td>Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een getal is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (expr)</a></td>
  <td>Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een JSON-object is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (expr)</a></td>
  <td>Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een tekenreeks is.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (expr)</a></td>
  <td>Retourneert een Booleaanse waarde die aangeeft of de eigenschap een waarde is toegewezen.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (expr)</a></td>
  <td>Retourneert een Booleaanse waarde die aangeeft of het type van de waarde een tekenreeks, getal, Booleaanse waarde of null zijn is.</td>
</tr>

</table>

U kunt nu een query's als volgt uitvoeren met behulp van deze functies:

**Query**

    SELECT VALUE IS_NUMBER(-4)

**Resultaten**

    [true]

### <a name="string-functions"></a>Tekenreeksfuncties
De volgende scalaire functies een bewerking uitvoeren op een tekenreekswaarde van de invoer en een tekenreeks, numerieke of Booleaanse waarde retourneren. Hier volgt een lijst met ingebouwde tekenreeks-functies:

| Gebruik | Beschrijving |
| --- | --- |
| [LENGTE (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length) |Retourneert het aantal tekens van de opgegeven tekenreeksexpressie |
| [CONCAT (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat) |Retourneert een tekenreeks die het resultaat van het samenvoegen van twee of meer tekenreekswaarden. |
| [De SUBTEKENREEKS (str_expr, num_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring) |Retourneert deel van een tekenreeksexpressie. |
| [STARTSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith) |Retourneert een Boolean die aangeeft of de eerste expressie tekenreeks begint met de tweede |
| [ENDSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith) |Retourneert een Boolean die aangeeft of de eerste tekenreeksexpressie eindigt op de tweede |
| [BEVAT (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains) |Retourneert een Boolean die aangeeft of de eerste expressie tekenreeks de tweede bevat. |
| [INDEX_OF (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of) |Retourneert de beginpositie van het eerste exemplaar van de tweede tekenreeksexpressie binnen de eerste expressie in de opgegeven tekenreeks is of -1 als de tekenreeks is niet gevonden. |
| [LEFT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left) |Retourneert het linkergedeelte van een tekenreeks zijn met het opgegeven aantal tekens. |
| [RIGHT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right) |Retourneert de juiste deel van een tekenreeks zijn met het opgegeven aantal tekens. |
| [LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim) |Retourneert een tekenreeksexpressie na het verwijderen van toonaangevende lege waarden. |
| [RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim) |Retourneert een tekenreeksexpressie na het afkappen van alle volgspaties. |
| [LOWER (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower) |Retourneert een tekenreeksexpressie na hoofdletter gegevens converteren naar kleine letters. |
| [UPPER (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper) |Retourneert een tekenreeksexpressie na kleine letter gegevens converteren naar hoofdletters. |
| [REPLACE (str_expr, str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace) |Vervangt alle instanties van een opgegeven string-waarde met de waarde van een andere tekenreeks. |
| [REPLICEREN (str_expr, num_expr)](https://docs.microsoft.com/azure/cosmos-db/sql-api-sql-query-reference#bk_replicate) |Herhaalt een string-waarde van een opgegeven aantal keren. |
| [REVERSE (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse) |Retourneert de omgekeerde volgorde van een string-waarde. |

U kunt nu een query's als volgt uitvoeren met behulp van deze functies. U kunt bijvoorbeeld de familienaam in hoofdletters als volgt:

**Query**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Resultaten**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Of samenvoegen van strings zoals in dit voorbeeld:

**Query**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Resultaten**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Tekenreeks-functies kunnen ook worden gebruikt in de component WHERE om resultaten te filteren, zoals in het volgende voorbeeld:

**Query**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Resultaten**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>Matrixfuncties
De volgende scalaire functies een bewerking uitvoeren op een matrix invoerwaarde en retourneren numerieke, Booleaanse waarde of een matrix van waarde. Hier volgt een lijst met ingebouwde matrixfuncties:

| Gebruik | Beschrijving |
| --- | --- |
| [ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length) |Retourneert het aantal elementen van de opgegeven matrix-expressie. |
| [ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat) |Retourneert een matrix die het resultaat van het samenvoegen van twee of meer matrixwaarden. |
| [ARRAY_CONTAINS (arr_expr, expr [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains) |Retourneert een Booleaanse waarde die aangeeft of de matrix bevat met de opgegeven waarde. Kunnen opgeven als overeenkomst met de volledige of gedeeltelijke. |
| [ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice) |Retourneert deel van een matrixexpressie. |

Matrixfuncties kunnen worden gebruikt voor het bewerken van matrices in JSON. Hier is bijvoorbeeld een query die alle documenten retourneert waarbij een van de ouders 'Robin Wakefield' is. 

**Query**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Resultaten**

    [{
      "id": "WakefieldFamily"
    }]

U kunt opgeven dat een gedeeltelijke fragment voor overeenkomende elementen binnen de matrix. De volgende query vindt alle bovenliggende items met de `givenName` van `Robin`.

**Query**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)

**Resultaten**

    [{
      "id": "WakefieldFamily"
    }]


Hier volgt een voorbeeld waarin ARRAY_LENGTH wordt gebruikt voor het aantal onderliggende items per serie.

**Query**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Resultaten**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Ruimtelijke functies
De volgende ingebouwde functies voor Open georuimtelijke Consortium (OGC) biedt ondersteuning voor cosmos DB georuimtelijke query's. 

<table>
<tr>
  <td><strong>Gebruik</strong></td>
  <td><strong>Beschrijving</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Retourneert de afstand tussen de twee GeoJSON punt, Polygon of LineString expressies.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Retourneert een Booleaanse expressie die aangeeft of het eerste GeoJSON-object (punt, Polygon of LineString) binnen het tweede GeoJSON-object (punt, Polygon of LineString).</td>
</tr>
<tr>
  <td>ST_INTERSECTS (spatial_expr, spatial_expr)</td>
  <td>Retourneert een Booleaanse expressie waarmee wordt aangegeven of de twee opgegeven GeoJSON objecten (punt, Polygon of LineString) intersect.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Retourneert een Booleaanse waarde die aangeeft of de opgegeven expressie voor GeoJSON punt, Polygon of LineString ongeldig is.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Retourneert een JSON-waarde met een Booleaanse waarde als de opgegeven expressie voor GeoJSON punt, Polygon of LineString is geldig, en als ongeldig, ook de reden als een string-waarde.</td>
</tr>
</table>

Ruimtelijke functies kunnen worden gebruikt om uit te voeren nabijheid query's op ruimtelijke gegevens. Hier is bijvoorbeeld een query die alle familie documenten die binnen 30 km van de opgegeven locatie met de ingebouwde functie ST_DISTANCE retourneert. 

**Query**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Resultaten**

    [{
      "id": "WakefieldFamily"
    }]

Zie voor meer informatie over ondersteuning voor Cosmos DB georuimtelijke [werken met gegevens in Azure Cosmos DB georuimtelijke](geospatial.md). Die loopt van ruimtelijke functies en de SQL-syntaxis voor Cosmos-DB. Nu eens kijken hoe LINQ-query werkt en hoe deze samenwerkt met de syntaxis we hebt gezien tot nu toe.

## <a id="Linq"></a>LINQ naar SQL-API
LINQ is een .NET-programmeermodel waarin berekeningen query's voor stromen van objecten. Cosmos DB biedt een client-side '-bibliotheek voor LINQ-interface doordat een conversie tussen JSON en .NET-objecten en een toewijzing van een subset van LINQ-query's aan Cosmos-DB-query's. 

De volgende afbeelding toont de architectuur van LINQ-query's met behulp van de Cosmos-DB ondersteunen.  Met behulp van de Cosmos-DB-client kunnen ontwikkelaars maken een **IQueryable** object waarmee een query rechtstreeks op de Cosmos-DB-provider opvragen, die vervolgens de LINQ-query in een Cosmos-DB-query zet. De query wordt vervolgens doorgegeven aan de Cosmos-databaseserver voor het ophalen van een set resultaten in JSON-indeling. De geretourneerde resultaten worden in een stream met .NET-objecten aan de clientzijde gedeserialiseerd.

![Architectuur van de LINQ-query's met behulp van de SQL-API - SQL-syntaxis, JSON-querytaal concepten van de database en SQL-query's ondersteunen][1]

### <a name="net-and-json-mapping"></a>.NET- en JSON-toewijzing
De toewijzing tussen .NET-objecten en JSON-documenten is natuurlijk - elk veld gegevens lid is toegewezen aan een JSON-object, waarbij de veldnaam is toegewezen aan het gedeelte 'sleutel' van het object en het onderdeel '' waarde '' recursief toegewezen aan de waarde van het object. Bekijk het volgende voorbeeld: de familie-object gemaakt is toegewezen aan het JSON-document, zoals hieronder wordt weergegeven. En omgekeerd, het JSON-document is toegewezen aan een .NET-object.

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



### <a name="linq-to-sql-translation"></a>LINQ SQL vertaling
De Cosmos-DB-provider opvragen voert een best effort toewijzing van een LINQ-query naar een Cosmos DB SQL-query. In de volgende beschrijving gaan we ervan uit dat de lezer heeft een elementaire kennis van LINQ.

Voor het typesysteem ondersteunen we eerst alle JSON primitieve typen – numerieke typen, Booleaanse waarde, tekenreeks en null. Alleen deze JSON-typen worden ondersteund. De volgende scalaire expressies worden ondersteund.

* Constante waarden – hierbij constante waarden van de primitieve gegevenstypen op het moment dat de query wordt geëvalueerd.
* Eigenschappenmatrix/indexexpressies – deze expressies verwijzen naar de eigenschap van een object of een element van de matrix.
  
     familie. ID;    Family.children[0].familyName;    Family.children[0].Grade;    Family.children[n].Grade; n is een int-variabele
* Aritmetische expressies - deze algemene aritmetische expressies op numerieke en Booleaanse waarden bevatten. Raadpleeg de SQL-specificatie voor de volledige lijst.
  
     2 * family.children[0].grade;    x + y;
* Vergelijking van tekenreeksexpressie - hierbij een string-waarde naar een constante string-waarde te vergelijken.  
  
     mother.familyName == 'Smith';    child.givenName == s; s is een string-variabele
* Object/matrix maken van expressie - deze expressies return een object van het waardetype van de samengestelde of anoniem type of een matrix van dergelijke objecten. Deze waarden kunnen worden genest.
  
     nieuwe bovenliggende {familyName = 'Smit', givenName = "Jan"}; nieuwe {eerst = 1, tweede = 2}; een anoniem type met twee velden              
     nieuwe int [] {3, child.grade, 5};

### <a id="SupportedLinqOperators"></a>Lijst met ondersteunde LINQ-operators
Hier volgt een lijst met ondersteunde LINQ-operators in de LINQ-provider opgenomen met de SQL .NET SDK.

* **Selecteer**: projecties vertalen naar de SQL SELECT-objectconstructie inclusief
* **Waar**: Filters vertalen naar SQL WHERE en ondersteuning voor de conversie van & &, || en! aan de SQL-operators
* **SelectMany**: kunt ongedaan maken van matrices voor de SQL-JOIN-component. Kan worden gebruikt voor expressies kunt u filteren op matrixelementen keten/nest
* **OrderBy en OrderByDescending**: vertaalt in ORDER BY oplopend/aflopend
* **Aantal**, **som**, **Min**, **Max**, en **gemiddelde** operators voor aggregatie en de async-equivalenten **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, en **AverageAsync**.
* **CompareTo**: wordt omgezet in bereik vergelijkingen. Meestal gebruikt voor tekenreeksen omdat ze nog niet vergeleken in .NET
* **Nemen**: kan het aan de BOVENKANT SQL voor het beperken van de resultaten van een query
* **Rekenkundige functies**: de vertaling van ondersteunt. De NET Abs, BOOGCOS, Asin, BOOGTAN, maximum, CO, Exp, Floor, logboek, Log10, Pow, Round, aanmelding, Sin, Sqrt, Tan, Truncate aan de gelijkwaardige ingebouwde functies van SQL.
* **Functies String**: de vertaling van ondersteunt. NET van Concat, bevat EndsWith, IndexOf, Count, ToLower, TrimStart, vervangen, Reverse, TrimEnd, StartsWith, subtekenreeks, ToUpper aan de gelijkwaardige ingebouwde functies van SQL.
* **Matrix van functies**: de vertaling van ondersteunt. NET van Concat, bevat en aantal voor de equivalente ingebouwde SQL-functies.
* **Extensiefuncties georuimtelijke**: vertaling van stub-methoden afstand binnen IsValid en IsValidDetailed naar de equivalente ingebouwde SQL-functies ondersteunt.
* **Gebruiker gedefinieerde functie extensiefunctie**: ondersteunt de vertaling van de stubmethode UserDefinedFunctionProvider.Invoke naar de bijbehorende door de gebruiker gedefinieerde functie.
* **Diverse**: biedt ondersteuning voor omzetting van de coalesce en voorwaardelijke operators. Bevat tekenreeks bevat, ARRAY_CONTAINS of de SQL-IN, afhankelijk van de context kunnen worden omgezet.

### <a name="sql-query-operators"></a>SQL-query-operators
Hier volgen enkele voorbeelden die aangeven hoe sommige van de standaard LINQ-query's worden omgezet naar beneden Cosmos-DB-query's.

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
De syntaxis is `input.SelectMany(x => f(x))`, waarbij `f` is een scalaire expressie die als resultaat een verzamelingstype geeft.

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
De bovenstaande operators kunnen worden samengesteld om krachtige query's. Aangezien Cosmos DB geneste verzamelingen ondersteunt, kan de samenstelling worden samengevoegd of genest.

#### <a name="concatenation"></a>Samenvoeging
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
De syntaxis is `input.SelectMany(x=>x.Q())` waarbij Q is een `Select`, `SelectMany`, of `Where` operator.

In een geneste query, wordt de interne query toegepast op elk element van de buitenste verzameling. Een belangrijk onderdeel is dat de interne query naar de velden van de elementen in de buitenste verzameling zoals verwijzen kunt zelf-joins.

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
Cosmos DB Ontsluit resources via een REST-API die kan worden aangeroepen via elke taal waarmee HTTP/HTTPS-aanvragen. Daarnaast biedt Cosmos DB programmeringsbibliotheken voor verschillende veelgebruikte talen zoals .NET, Node.js, JavaScript en Python. De REST-API en de verschillende bibliotheken ondersteuning voor het uitvoeren van query's via SQL. De .NET SDK biedt ondersteuning voor LINQ-query naast SQL.

De volgende voorbeelden laten zien hoe u een query maken en te verzenden op basis van een Cosmos-DB-databaseaccount.

### <a id="RestAPI"></a>REST API
Cosmos DB biedt een open RESTful-programmeermodel via HTTP. Database-accounts kunnen worden ingericht met behulp van een Azure-abonnement. Het model van de resource Cosmos DB bestaat uit een set resources onder de databaseaccount van een, die elk opgevraagd met een logische en stabiele-URI is. Een set resources wordt aangeduid als een feed in dit document. Een databaseaccount bestaat uit een reeks databases, die elk meerdere verzamelingen met elk van welke beurt documenten, UDF's en andere brontypen bevatten.

Het model basic interactie met deze resources is via de HTTP-woorden GET, PUT, POST en DELETE met hun standaard interpretatie. De POST-bewerking wordt gebruikt voor het maken van een nieuwe resource, voor het uitvoeren van een opgeslagen procedure of voor het uitgeven van een Cosmos-DB-query. Query's zijn altijd alleen-lezen bewerkingen met geen bijwerkingen.

De volgende voorbeelden ziet een POST voor een SQL-API-query ten opzichte van een verzameling met de twee voorbeelddocumenten dat we tot nu toe hebt bekeken. De query heeft een eenvoudige filter op de naameigenschap JSON. Let op het gebruik van de `x-ms-documentdb-isquery` en Content-Type: `application/query+json` headers om aan te geven dat de bewerking een query wordt.

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


**Resultaten**

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


Het tweede voorbeeld ziet u een complexere query die meerdere resultaten uit de join retourneert.

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


**Resultaten**

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


Als de queryresultaten niet binnen één pagina met resultaten passen, wordt de REST-API een vervolgtoken via retourneert de `x-ms-continuation-token` antwoordheader. Clients kunnen resultaten pagineren door de header in de volgende resultaten. Het aantal resultaten per pagina kan ook worden beheerd via de `x-ms-max-item-count` nummer header. Als de opgegeven query een statistische functie zoals heeft `COUNT`, en vervolgens de querypagina een gedeeltelijk cumulatieve waarde kan worden geretourneerd gedurende de pagina met resultaten. De clients moeten een aggregatie tweede niveau uitvoeren via deze resultaten met het uiteindelijke resultaat, bijvoorbeeld, som is opgegeven via de tellingen geretourneerd in de afzonderlijke pagina's om de totale telling te retourneren.

Gebruik het beleid voor de consistentie van gegevens voor query's beheren de `x-ms-consistency-level` header bijvoorbeeld alle aanvragen voor REST-API. Voor sessieconsistentie, is het vereist ook de meest recente echo `x-ms-session-token` Cookie-kop in de queryaanvraag. De aangevraagde verzameling indexeringsbeleid kan ook invloed hebben op de consistentie van de queryresultaten. Met de standaard beleidsinstellingen indexeren, voor verzamelingen de index is altijd de meest recente inhoud van het document en queryresultaten overeenkomen met de consistentiecontrole gekozen voor de gegevens. Als het indexeringsbeleid is versoepeld naar Lazy, kunnen query's verlopen resultaten geretourneerd. Zie voor meer informatie [Azure Cosmos DB Consistentieniveaus][consistency-levels].

Als de opgegeven query kan niet worden ondersteund door het geconfigureerde indexeringsbeleid voor de verzameling, retourneert de server Azure Cosmos DB 400 'onjuiste aanvraag'. Dit wordt geretourneerd voor bereik query's op paden die zijn geconfigureerd voor lookups op hash (gelijkheid) en voor paden expliciet is uitgesloten van het indexeren. De `x-ms-documentdb-query-enable-scan` header kan worden opgegeven waarmee de query voor een scan uitvoeren als een index niet beschikbaar is.

U kunt gedetailleerde metrische gegevens over de uitvoering van de query opvragen door in te stellen `x-ms-documentdb-populatequerymetrics` koptekst tot `True`. Zie voor meer informatie [SQL query metrische gegevens voor Azure Cosmos DB](sql-api-sql-query-metrics.md).

### <a id="DotNetSdk"></a>C# (.NET) SDK
De .NET SDK biedt ondersteuning voor LINQ- en SQL uitvoeren van query's. Het volgende voorbeeld ziet hoe u de eenvoudige filterquery geïntroduceerd eerder in dit document uitvoert.

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


Dit voorbeeld vergelijkt twee eigenschappen op gelijkheid binnen elk document en maakt gebruik van anonieme projecties. 

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


Het volgende voorbeeld toont joins, die zijn uitgedrukt via LINQ SelectMany.

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



De .NET-client worden automatisch alle pagina's van de resultaten van de query in de foreach-blokken zoals hierboven doorlopen. De query-opties die zijn geïntroduceerd in de REST-API-sectie zijn ook beschikbaar in de .NET SDK met de `FeedOptions` en `FeedResponse` klassen in de methode CreateDocumentQuery. Het aantal pagina's kan worden beheerd met behulp van de `MaxItemCount` instelling. 

U kunt ook expliciet paginering bepalen door het maken van `IDocumentQueryable` met behulp van de `IQueryable` object, klikt u vervolgens op het lezen van de` ResponseContinuationToken` waarden en deze weer toe als `RequestContinuationToken` in `FeedOptions`. `EnableScanInQuery`kan worden ingesteld voor het inschakelen van scans als de query kan niet worden ondersteund door het geconfigureerde indexeringsbeleid. Voor gepartitioneerde verzamelingen kunt u `PartitionKey` aan de query uitvoeren voor één partitie (Hoewel Cosmos DB dit automatisch uit de querytekst ophalen kunt), en `EnableCrossPartitionQuery` query's uitvoeren die mogelijk moet worden uitgevoerd tegen meerdere partities. 

Raadpleeg [Azure Cosmos DB .NET-voorbeelden](https://github.com/Azure/azure-documentdb-net) voor meer voorbeelden met query's. 

### <a id="JavaScriptServerSideApi"></a>JavaScript-serverzijde-API
Cosmos DB biedt een programmeermodel voor het uitvoeren op basis van JavaScript-toepassingslogica rechtstreeks op de verzamelingen met opgeslagen procedures en triggers. De JavaScript-logica geregistreerd op het niveau van een verzameling kan databasebewerkingen uitvoeren op de bewerkingen op de documenten van de opgegeven verzameling vervolgens uitgeven. Deze bewerkingen zijn verpakt in een ambient ACID-transactions.

Het volgende voorbeeld laat zien hoe de queryDocuments op de server in JavaScript API gebruiken om te maken van query's van binnen opgeslagen procedures en triggers.

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
2. [Azure SQL voor Cosmos-DB-specificatie](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3. [Azure Cosmos DB .NET-voorbeelden](https://github.com/Azure/azure-documentdb-net)
4. [Azure Cosmos DB Consistency Levels][consistency-levels]
5. ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6. JSON [http://json.org/](http://json.org/)
7. Javascript Specification [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8. LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9. Van evaluatietechnieken voor grote databases query [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)
10. Queryverwerking in parallelle relationele databasesystemen, IEEE Computer samenleving Press, 1994
11. Lu, Ooi, Tan queryverwerking in parallelle relationele databasesystemen, IEEE Computer samenleving Press, 1994.
12. Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: Pig Latin: een niet zodat vreemde taal voor gegevensverwerking, SIGMOD 2008.
13. G. Graefe. Het framework trapsgewijs voor queryoptimalisatie. IEEE-gegevens eng Bull., 18(3): 1995.

[1]: ./media/sql-api-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md
