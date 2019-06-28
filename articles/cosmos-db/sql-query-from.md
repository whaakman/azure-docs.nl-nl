---
title: FROM-component in Azure Cosmos DB
description: Meer informatie over SQL FROM-component voor Azure Cosmos DB
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: tisande
ms.openlocfilehash: 6bc93569dc9a0405ec3a8dfd719c89ede01df84d
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342900"
---
# <a name="from-clause"></a>FROM-component

De FROM (`FROM <from_specification>`)-component is optioneel, tenzij de bron is gefilterd of verderop in de query verwachte. Een query zoals `SELECT * FROM Families` inventariseert de gehele `Families` container. U kunt ook de speciale ROOT-id gebruiken voor de container in plaats van de containernaam van de.

De FROM-component gebruikt de volgende regels per query:

* Voor de container kunt u een alias gebruiken, zoals `SELECT f.id FROM Families AS f` of gewoon `SELECT f.id FROM Families f`. Hier `f` is de alias voor `Families`. OMDAT een optionele trefwoord [alias](sql-query-aliasing.md) de id.  

* Eenmaal alias, de naam van de oorspronkelijke bron kan niet worden gekoppeld. Bijvoorbeeld, `SELECT Families.id FROM Families f` syntaxis is ongeldig omdat de id `Families` alias is en kan niet meer worden omgezet.  

* Eigenschappen van alle waarnaar wordt verwezen, moet volledig gekwalificeerd zijn, om te voorkomen dat een niet-eenduidige bindingen in de afwezigheid van strikt schema voldoet. Bijvoorbeeld, `SELECT id FROM Families f` syntaxis is ongeldig omdat de eigenschap `id` niet afhankelijk is.

## <a name="syntax"></a>Syntaxis
  
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
  
## <a name="arguments"></a>Argumenten
  
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
  
## <a name="remarks"></a>Opmerkingen
  
Alle aliassen opgegeven of afgeleid in de `<from_source>(`s) moet uniek zijn. De syntaxis van de `<container_expression>.`%{Property_Name/ is hetzelfde als `<container_expression>' ['"property_name"']'`. De syntaxis van de laatste kan echter worden gebruikt als een eigenschapsnaam een niet-id-teken bevat.  
  
### <a name="handling-missing-properties-missing-array-elements-and-undefined-values"></a>afhandeling van ontbrekende eigenschappen, ontbrekende matrixelementen en niet-gedefinieerde waarden
  
Als een expressie voor een container toegang heeft tot de eigenschappen of matrixelementen en waarde niet bestaat, wordt die waarde genegeerd en niet verder wordt verwerkt.  
  
### <a name="container-expression-context-scoping"></a>Container expressie context scoping  
  
Een expressie voor een container zijn binnen het bereik van container of binnen het bereik van document:  
  
-   Een expressie is container binnen het bereik, als de onderliggende bron van de container-expressie de hoofdmap is of `container_name`. Deze expressie vertegenwoordigt een set documenten die zijn opgehaald uit de container rechtstreeks en is niet afhankelijk van de verwerking van andere expressies container.  
  
-   Een expressie is document binnen het bereik, als de onderliggende bron van de container-expressie is `input_alias` die eerder in de query is geïntroduceerd. Deze expressie vertegenwoordigt een set documenten die zijn verkregen door het evalueren van de container-expressie in het bereik van elk document die behoren tot de verzameling die zijn gekoppeld aan de alias-container.  De resulterende set is een samenvoeging van die worden verkregen door het evalueren van de container-expressie voor elk van de documenten in de onderliggende verzameling. 

## <a name="examples"></a>Voorbeelden

### <a name="get-subitems-by-using-the-from-clause"></a>Subitems ophalen met behulp van de FROM-component

De FROM-component kan de bron naar een kleinere subset verminderen. Als u wilt inventariseren alleen een substructuur in elk item, kan de subroot de bron, vormen, zoals weergegeven in het volgende voorbeeld:

```sql
    SELECT *
    FROM Families.children
```

De resultaten zijn:

```json
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
```

De voorgaande query gebruikt een matrix als de bron, maar u kunt ook een object gebruiken als de bron. De query rekening gehouden met een waarde van de JSON geldig, die is gedefinieerd in de bron voor opname in het resultaat. Het volgende voorbeeld zou uitsluiten `Families` die geen een `address.state` waarde.

```sql
    SELECT *
    FROM Families.address.state
```

De resultaten zijn:

```json
    [
      "WA",
      "NY"
    ]
```

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag](sql-query-getting-started.md)
- [SELECT-component](sql-query-select.md)
- [WHERE-component](sql-query-where.md)