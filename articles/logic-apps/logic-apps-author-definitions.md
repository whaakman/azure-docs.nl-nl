---
title: "Werkstromen met JSON - Azure Logic Apps definiëren | Microsoft Docs"
description: Het schrijven van werkstroomdefinities in JSON voor logic apps
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 03/29/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 7dde5bc4733af1aba34199f332379d2faf566725
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="create-workflow-definitions-for-logic-apps-using-json"></a>Werkstroomdefinities voor logische apps met JSON maken

U kunt de werkstroomdefinities voor de maken [Azure Logic Apps](logic-apps-overview.md) met eenvoudig, declaratief JSON-taal. Als u nog niet gedaan hebt, moet u rekening houden [uw eerste logische app maken met Logic App-ontwerper](quickstart-create-first-logic-app-workflow.md). Zie ook de [volledige verwijzing voor de werkstroom Definition Language](http://aka.ms/logicappsdocs).

## <a name="repeat-steps-over-a-list"></a>Herhaal de stappen via een lijst

Als u wilt een matrix met maximaal 10.000 items doorlopen en een actie uitvoert voor elk item, gebruiken de [foreach type](logic-apps-loops-and-scopes.md).

## <a name="handle-failures-if-something-goes-wrong"></a>Afhandelen van fouten als er iets mis gaat

Meestal kunt u wilt opnemen een *herstel stap* : bepaalde logica die wordt uitgevoerd *als* een of meer van uw aanroepen mislukken. In dit voorbeeld ontvangt gegevens van verschillende locaties, maar als de aanroep is mislukt, willen we een bericht ergens zodat we later omlaag die is mislukt bijhouden kunt:  

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      }
    },
    "postToErrorMessageQueue": {
      "type": "ApiConnection",
      "inputs": "...",
      "runAfter": {
        "readData": [
          "Failed"
        ]
      }
    }
  },
  "outputs": {}
}
```

Om op te geven die `postToErrorMessageQueue` alleen wordt uitgevoerd na `readData` heeft `Failed`, gebruiken de `runAfter` eigenschap, bijvoorbeeld een lijst van mogelijke waarden opgeven zodat `runAfter` kan worden `["Succeeded", "Failed"]`.

Ten slotte omdat in dit voorbeeld wordt de fout nu verwerkt, wordt niet langer markeren het run as- `Failed`. Omdat we de stap voor het verwerken van deze fout in dit voorbeeld hebt toegevoegd, wordt de uitvoering is `Succeeded` maar één stap `Failed`.

## <a name="execute-two-or-more-steps-in-parallel"></a>Twee of meer stappen parallel uitvoeren

Meerdere acties uitvoeren parallel, de `runAfter` eigenschap moet gelijkwaardige tijdens runtime. 

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "triggers": {
    "Request": {
      "kind": "http",
      "type": "Request"
    }
  },
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      }
    },
    "branch1": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "branch2": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

In dit voorbeeld beide `branch1` en `branch2` zijn ingesteld op uitvoeren na `readData`. Als gevolg hiervan beide vertakkingen parallel worden uitgevoerd. Het tijdstempel voor beide vertakkingen is identiek.

![Parallel](media/logic-apps-author-definitions/parallel.png)

## <a name="join-two-parallel-branches"></a>Twee parallelle vertakkingen koppelen

U kunt deelnemen aan twee acties die zijn ingesteld op parallel worden uitgevoerd door de items toevoegen aan de `runAfter` eigenschap zoals in het vorige voorbeeld.

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {}
    },
    "branch1": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "branch2": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "join": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "branch1": [
          "Succeeded"
        ],
        "branch2": [
          "Succeeded"
        ]
      }
    }
  },
  "parameters": {},
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "Http",
      "inputs": {
        "schema": {}
      }
    }
  },
  "contentVersion": "1.0.0.0",
  "outputs": {}
}
```

![Parallel](media/logic-apps-author-definitions/join.png)

## <a name="map-list-items-to-a-different-configuration"></a>Lijstitems worden toegewezen aan een andere configuratie

Volgende, stel willen we andere inhoud op basis van de waarde van een eigenschap ophalen. We kunnen een overzicht van waarden naar bestemmingen maken als een parameter:  

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "specialCategories": {
      "defaultValue": [
        "science",
        "google",
        "microsoft",
        "robots",
        "NSA"
      ],
      "type": "Array"
    },
    "destinationMap": {
      "defaultValue": {
        "science": "http://www.nasa.gov",
        "microsoft": "https://www.microsoft.com/en-us/default.aspx",
        "google": "https://www.google.com",
        "robots": "https://en.wikipedia.org/wiki/Robot",
        "NSA": "https://www.nsa.gov/"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "http"
    }
  },
  "actions": {
    "getArticles": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
      }
    },
    "forEachArticle": {
      "type": "foreach",
      "foreach": "@body('getArticles').responseData.feed.entries",
      "actions": {
        "ifGreater": {
          "type": "if",
          "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)",
          "actions": {
            "getSpecialPage": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
              }
            }
          }
        }
      },
      "runAfter": {
        "getArticles": [
          "Succeeded"
        ]
      }
    }
  }
}
```

In dit geval krijgen we eerst een lijst met artikelen. Op basis van de categorie die is gedefinieerd als een parameter, de tweede stap een kaart gebruikt om de URL voor het ophalen van de inhoud te zoeken.

Een aantal keer te worden hier: 

*   De [ `intersection()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) functie gecontroleerd of de categorie overeenkomt met een van de bekende gedefinieerde categorieën.

*   Nadat we de categorie hebt ontvangen, kunnen we het item uit een kaart met vierkante haken ophalen:`parameters[...]`

## <a name="process-strings"></a>Proces tekenreeksen

U kunt diverse functies gebruiken om te manipuleren van tekenreeksen. Stel bijvoorbeeld dat we hebben een tekenreeks die u moet doorgeven aan een systeem, maar we zijn ervan overtuigd over het afhandelen van de juiste voor codering niet. Een mogelijkheid is het base64 coderen van deze tekenreeks. Om te voorkomen aanhalingstekens in een URL, gaan we echter een paar tekens wilt vervangen. 

We willen ook een subtekenreeks van de naam van de volgorde van omdat de eerste vijf tekens niet worden gebruikt.

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1",
        "orderer": "NAME=Contoso"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
      }
    }
  },
  "outputs": {}
}
```

Werken vanaf binnen naar buiten:

1. Ophalen van de [ `length()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) voor de naam van de besteller, dus we terughalen het totale aantal tekens.

2. Aftrekken 5 aangezien we een kortere tekenreeks.

3. Pas de [ `substring()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring). We beginnen bij index `5` en gaat u de rest van de tekenreeks.

4. Deze reeks die u wilt converteren een [ `base64()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) tekenreeks.

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)alle de `+` tekens `-` tekens.

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)alle de `/` tekens `_` tekens.

## <a name="work-with-date-times"></a>Werken met datums en tijden

Datums en tijden kan handig zijn met name als u wilt ophalen van gegevens uit een gegevensbron die natuurlijk biedt geen ondersteuning voor *triggers*. U kunt ook datums en tijden voor het vinden van hoe lang verschillende stappen nemen.

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{parameters('order').id}"
      }
    },
    "ifTimingWarning": {
      "type": "If",
      "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
      "actions": {
        "timingWarning": {
          "type": "Http",
          "inputs": {
            "method": "GET",
            "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
          }
        }
      },
      "runAfter": {
        "order": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

In dit voorbeeld halen we de `startTime` van de vorige stap. Vervolgens wordt de huidige tijd niet ophalen en afgetrokken van één seconde:

[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) 

U kunt andere tijdseenheden, zoals `minutes` of `hours`. Ten slotte kunnen we deze twee waarden vergelijken. Als de eerste waarde kleiner dan de tweede waarde en klik vervolgens op meer dan één seconde is is verstreken sinds de volgorde voor het eerst is geplaatst.

Als u wilt opmaken datums, kunnen we tekenreeks formatters gebruiken. Bijvoorbeeld, als u de RFC1123, gebruiken we [ `utcnow('r')` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow). Zie voor meer informatie over het opmaken van datum, [werkstroom Definition Language](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).

## <a name="deployment-parameters-for-different-environments"></a>Implementatieparameters voor verschillende omgevingen

Implementatie levenscycli hebben doorgaans een ontwikkelomgeving, een testomgeving en een productie-omgeving. U kan bijvoorbeeld gebruik van dezelfde definitie in alle deze omgevingen maar verschillende databases gebruikt. U wilt ook gebruik van dezelfde definitie over verschillende regio's voor hoge beschikbaarheid, maar elke instantie van de app logica wilt Neem contact op met de database van de regio.
Dit scenario verschilt van de parameters op te nemen *runtime* waar in plaats daarvan moet u de `trigger()` werken zoals in het vorige voorbeeld.

U kunt beginnen met een basisdefinitie zoals in dit voorbeeld:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "request": {
          "type": "request",
          "kind": "http"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

In de werkelijke `PUT` aanvragen voor de logic apps kunt u de parameter bieden `uri`. Omdat een standaardwaarde niet meer bestaat, is deze parameter vereist in de nettolading van de logische app:

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

In elke omgeving, kunt u opgeven van een andere waarde voor de `connection` parameter. 

Zie voor alle opties die u hebt voor het maken en beheren van logic apps, de [REST API-documentatie](https://msdn.microsoft.com/library/azure/mt643787.aspx). 
