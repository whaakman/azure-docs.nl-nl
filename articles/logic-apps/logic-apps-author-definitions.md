---
title: Maken, bewerken of JSON voor logic app-definities - Azure Logic Apps uitbreiden | Microsoft Docs
description: Ontwerpen en aanpassen van logic app-definities in JSON
author: ecfan
manager: jeconnoc
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/01/2018
ms.author: estfan; LADocs
ms.openlocfilehash: 9793fdf2bd351bd1f15bcb88ffd25d6b19485303
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35297849"
---
# <a name="create-edit-or-customize-json-for-logic-app-definitions"></a>Maken, bewerken of JSON voor logic app-definities aanpassen

Bij het maken van enterprise integratieoplossingen met geautomatiseerde werkstromen in [Azure Logic Apps](../logic-apps/logic-apps-overview.md), gebruikt u de onderliggende logic app-definities eenvoudig en declaratieve notatie JSON (JavaScript Object) samen met de [ Werkstroom Definition Language (WDL) schema](../logic-apps/logic-apps-workflow-definition-language.md) voor hun beschrijving en validatie. Deze indelingen eenvoudiger logic app-definities worden gelezen en begrepen zonder te weten veel code. Als u maken en implementeren van logische apps automatiseren wilt, kunt u logic app-definities als opnemen [Azure-resources](../azure-resource-manager/resource-group-overview.md) binnen [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-overview.md#template-deployment). Maken, beheren en implementeren van logic apps, u kunt [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.logicapp), [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md), of de [Azure Logic Apps REST API's](https://docs.microsoft.com/rest/api/logic/).

Open de editor van de codeweergave als u werkt in de Azure-portal of in Visual Studio met logic app-definities in JSON wilt werken, of de definitie in een editor die u wilt kopiëren. Als u geen ervaring met logische apps, raadpleegt u [het maken van uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

> [!NOTE]
> Sommige Azure Logic Apps mogelijkheden, zoals parameters en meerdere triggers definiëren in logic app-definities, zijn alleen beschikbaar in JSON, niet de ontwerpfunctie voor Logic Apps. Daarom moet u voor deze taken ook werken in de codeweergave of een andere editor.

## <a name="edit-json---azure-portal"></a>JSON - Azure-portal bewerken

1. Meld u aan bij <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

2. Kies in het menu links **alle services**. In het zoekvak vinden 'logische apps' en selecteer vervolgens uw logische app in de resultaten.

3. Uw logische app menu onder **ontwikkelingsprogramma's**, selecteer **Logic App codeweergave**.

   De weergave van de Code-editor wordt geopend en toont de definitie van de logische app in JSON-indeling.

## <a name="edit-json---visual-studio"></a>JSON - Visual Studio bewerken

Voordat u op de definitie van de logische app in Visual Studio werken kunt, zorg ervoor dat u bent [de vereiste hulpprogramma's geïnstalleerd](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#prerequisites). Als een logische app maken met Visual Studio, Bekijk [Snelstartgids: automatiseren van taken en processen met Azure Logic Apps - Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

U kunt logische apps die zijn gemaakt en geïmplementeerd hetzij rechtstreeks vanuit de Azure-portal als Azure Resource Manager-projecten vanuit Visual Studio openen in Visual Studio.

1. Open de Visual Studio-oplossing of [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) project met uw logische app.

2. Zoek en open uw logische app definition, die standaard wordt weergegeven in een [Resource Manager-sjabloon](../azure-resource-manager/resource-group-overview.md#template-deployment)met de naam **LogicApp.json**. U kunt gebruiken en aanpassen van deze sjabloon voor implementatie in verschillende omgevingen.

3. Open het snelmenu voor de definitie van logic Apps en de sjabloon. Selecteer **Openen met Logic App-ontwerpfunctie**.

   ![Open logische app in Visual Studio-oplossing](./media/logic-apps-author-definitions/open-logic-app-designer.png)

4. Kies aan de onderkant van de ontwerpfunctie **codeweergave**. 

   De weergave van de Code-editor wordt geopend en toont de definitie van de logische app in JSON-indeling.

5. Als u wilt terugkeren naar de ontwerpfunctie weergave onder aan de weergave Code-editor kiezen **ontwerp**.

## <a name="parameters"></a>Parameters

Parameters kunnen u waarden in uw logische app opnieuw gebruiken en geschikt zijn voor het vervangen van waarden die u vaak wijzigt. Als u een e-mailadres die u gebruiken op meerdere plaatsen wilt hebt, moet u dat e-mailadres definiëren als een parameter. 

Parameters zijn ook handig als u wilt overschrijven parameters in verschillende omgevingen, meer informatie over [parameters voor de implementatie van](#deployment-parameters) en de [REST-API voor Azure Logic Apps documentatie](https://docs.microsoft.com/rest/api/logic).

> [!NOTE]
> Parameters zijn alleen beschikbaar in de codeweergave.

In de [eerste voorbeeld logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md), u een werkstroom die wordt verzonden e-mailberichten wanneer nieuwe advertenties worden weergegeven in de RSS-feed van een website gemaakt. De URL van de feed is vastgelegd, zodat dit voorbeeld laat hoe de waarde van de query vervangen door een parameter zien, zodat u de URL van feed eenvoudiger kunt wijzigen.

1. Zoeken in de codeweergave de `parameters : {}` object en voeg een `currentFeedUrl` object:

   ``` json
     "currentFeedUrl" : {
      "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
   }
   ```

2. In de `When_a_feed-item_is_published` actie, vinden de `queries` sectie en vervang de querywaarde met `"feedUrl": "#@{parameters('currentFeedUrl')}"`. 

   **Voordat u**
   ``` json
   }
      "queries": {
          "feedUrl": "https://s.ch9.ms/Feeds/RSS"
       }
   },   
   ```

   **Na**
   ``` json
   }
      "queries": {
          "feedUrl": "#@{parameters('currentFeedUrl')}"
       }
   },   
   ```

   Als u wilt deelnemen aan twee of meer tekenreeksen, u kunt ook de `concat` functie. 
   Bijvoorbeeld: `"@concat('#',parameters('currentFeedUrl'))"` werkt hetzelfde als het vorige voorbeeld.

3.  Als u bent klaar, kiest u **Opslaan**. 

Nu kunt u de website RSS-feed door een andere URL via de `currentFeedURL` object.

<a name="deployment-parameters"></a>

## <a name="deployment-parameters-for-different-environments"></a>Implementatieparameters voor verschillende omgevingen

Implementatie levenscycli hebben meestal omgevingen voor ontwikkeling, fasering en productie. U kunt bijvoorbeeld de dezelfde definitie van logische Apps gebruiken in alle deze omgevingen maar verschillende databases gebruiken. U wilt ook gebruik van dezelfde definitie over verschillende regio's voor hoge beschikbaarheid, maar elk exemplaar van de app logica van de regio-database te gebruiken. 

> [!NOTE] 
> Dit scenario verschilt van de parameters op te nemen *runtime* waar moet u de `trigger()` werken in plaats daarvan.

Hier volgt een basisdefinitie:

``` json
{
    "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
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
In de werkelijke `PUT` aanvragen voor de logic apps kunt u de parameter bieden `uri`. In elke omgeving, kunt u opgeven van een andere waarde voor de `connection` parameter. Omdat een standaardwaarde niet meer bestaat, is deze parameter vereist in de nettolading van de logische app:

``` json
{
    "properties": {},
        "definition": {
          /// Use the definition from above here
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

Zie voor meer informatie, de [REST-API voor Azure Logic Apps documentatie](https://docs.microsoft.com/rest/api/logic/).

## <a name="process-strings-with-functions"></a>Proces tekenreeksen met functies

Logic Apps heeft diverse functies voor het werken met tekenreeksen. Stel dat u wilt een bedrijfsnaam vanuit een order doorgeven naar een ander systeem. Echter, u niet zeker bent over het afhandelen van de juiste voor codering. Base64-codering op deze tekenreeks kan worden uitgevoerd, maar om te voorkomen dat Hiermee heft u in de URL, kunt u verschillende tekens in plaats daarvan vervangen. Bovendien hoeft u alleen een subtekenreeks voor naam van het bedrijf omdat de eerste vijf tekens niet worden gebruikt. 

``` json
{
  "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1",
        "companyName": "NAME=Contoso"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "request": {
      "type": "Request",
      "kind": "Http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').companyName,5,sub(length(parameters('order').companyName), 5) )),'+','-') ,'/' ,'_' )}"
      }
    }
  },
  "outputs": {}
}
```

Deze stappen wordt beschreven hoe deze tekenreeks, werken van binnenuit aan de buitenkant in dit voorbeeld worden verwerkt:

``` 
"uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').companyName,5,sub(length(parameters('order').companyName), 5) )),'+','-') ,'/' ,'_' )}"
```

1. Ophalen van de [ `length()` ](../logic-apps/logic-apps-workflow-definition-language.md) voor de naam van het bedrijf, zodat u het totale aantal tekens.

2. Als u een kortere tekenreeks, afgetrokken `5`.

3. Nu een [ `substring()` ](../logic-apps/logic-apps-workflow-definition-language.md). Start bij index `5`, en Ga naar de rest van de tekenreeks.

4. Deze reeks die u wilt converteren een [ `base64()` ](../logic-apps/logic-apps-workflow-definition-language.md) tekenreeks.

5. Nu [ `replace()` ](../logic-apps/logic-apps-workflow-definition-language.md) alle de `+` tekens `-` tekens.

6. Ten slotte [ `replace()` ](../logic-apps/logic-apps-workflow-definition-language.md) alle de `/` tekens `_` tekens.

## <a name="map-list-items-to-property-values-then-use-maps-as-parameters"></a>Lijstitems worden toegewezen aan eigenschapswaarden en gebruik vervolgens maps als parameters

Als u verschillende resultaten op basis van de waarde van een eigenschap, kunt u een map die overeenkomt met de waarde van elke eigenschap aan een resultaat maken en vervolgens gebruiken die zijn toegewezen als een parameter. 

Deze werkstroom wordt bijvoorbeeld sommige categorieën gedefinieerd als parameters en een toewijzing die overeenkomt met deze categorieën met een specifieke URL. De werkstroom wordt eerst een lijst met artikelen. De werkstroom wordt de kaart vinden van de URL die overeenkomt met de categorie voor elk artikel.

*   De [ `intersection()` ](../logic-apps/logic-apps-workflow-definition-language.md) functie gecontroleerd of de categorie overeenkomt met een bekende categorie die gedefinieerd.

*   Nadat u een overeenkomende categorie, haalt het voorbeeld het item uit een kaart met vierkante haken: `parameters[...]`

``` json
{
  "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
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

## <a name="get-data-with-date-functions"></a>Ophalen van gegevens met de datum-functies

Gegevens ophalen uit een gegevensbron die geen systeemeigen ondersteunt *triggers*, kunt u datum fungeert voor het werken met tijden en datums in plaats daarvan. Bijvoorbeeld hoe lang deze werkstroom stappen zijn duurt, worden gevonden voor deze expressie van binnenuit werken aan de buitenkant:

``` json
"expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
```

1. Van de `order` extract-actie voor de `startTime`. 
2. Ophalen van de huidige tijd met `utcNow()`.
3. Trek één seconde:

   [`addseconds(..., -1)`](../logic-apps/logic-apps-workflow-definition-language.md) 

   U kunt andere tijdseenheden, zoals `minutes` of `hours`. 

3. U kunt nu deze twee waarden vergelijken. 

   Als de eerste waarde kleiner dan de tweede waarde en klik vervolgens op meer dan één seconde is is verstreken sinds de volgorde voor het eerst is geplaatst.

Als u wilt opmaken datums, kunt u formatters tekenreeks. Bijvoorbeeld, als u de RFC1123, gebruikt [ `utcnow('r')` ](../logic-apps/logic-apps-workflow-definition-language.md). Meer informatie over [datum opmaak](../logic-apps/logic-apps-workflow-definition-language.md).

``` json
{
  "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder-id"
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


## <a name="next-steps"></a>Volgende stappen

* [Voer stappen uit op basis van een voorwaarde (voorwaardelijke instructies)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Voer stappen uit op basis van verschillende waarden (switch instructies)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Uitvoeren en herhaalt u stap (lussen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Uitvoeren of samenvoegen van parallelle stappen (vertakkingen)](../logic-apps/logic-apps-control-flow-branches.md)
* [Voer stappen uit op basis van Actiestatus van de gegroepeerde (scopes genoemd)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
* Meer informatie over de [werkstroom Definition Language-schema voor Azure Logic Apps](../logic-apps/logic-apps-workflow-definition-language.md)
* Meer informatie over [werkstroomacties en triggers voor Azure Logic Apps](../logic-apps/logic-apps-workflow-actions-triggers.md)