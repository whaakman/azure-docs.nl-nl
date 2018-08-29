---
title: Maken, bewerken of uitbreiden van JSON voor de logische app-definities - Azure Logic Apps | Microsoft Docs
description: Ontwerpen en JSON voor de logische app-definities in Azure Logic Apps uitbreiden
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, jehollan, LADocs
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.topic: article
ms.date: 01/01/2018
ms.openlocfilehash: 1f2e136810194ad044255f9d129b5c03549221b9
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43128657"
---
# <a name="create-edit-or-extend-json-for-logic-app-definitions-in-azure-logic-apps"></a>Maken, bewerken of JSON voor de logische app-definities in Azure Logic Apps uitbreiden

Bij het maken van zakelijke oplossingen voor integratie met geautomatiseerde werkstromen in [Azure Logic Apps](../logic-apps/logic-apps-overview.md), gebruikt u de onderliggende definities voor logische Apps eenvoudig en declaratieve JavaScript Object Notation (JSON) samen met de [ Werkstroom Definition Language (WDL) schema](../logic-apps/logic-apps-workflow-definition-language.md) voor hun beschrijving en validatie. Deze indelingen eenvoudiger logic app-definities worden gelezen en begrepen zonder te weten dat veel code. Als u maken en implementeren van logische apps automatiseren wilt, kunt u definities voor logische Apps als opnemen [Azure-resources](../azure-resource-manager/resource-group-overview.md) binnen [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-overview.md#template-deployment). Maken, beheren en implementeren van logic apps gebruikt, kunt u vervolgens [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.logicapp), [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md), of de [Azure Logic Apps REST-API's](https://docs.microsoft.com/rest/api/logic/).

Open de weergave van Code-editor wanneer u werkt in Azure portal of in Visual Studio om te werken met definities voor logische apps in JSON, of de definitie in een editor die u wilt kopiëren. Als u geen ervaring met logische apps, raadpleegt u [over het maken van uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

> [!NOTE]
> Sommige Azure Logic Apps-mogelijkheden, zoals het definiëren van parameters en meerdere triggers in de definities voor logische Apps, zijn alleen beschikbaar in JSON, niet de ontwerper van logische Apps. Daarom moet u voor deze taken ook werken in de Code of een andere editor.

## <a name="edit-json---azure-portal"></a>JSON - Azure-portal bewerken

1. Meld u aan bij <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

2. Kies in het menu links **alle services**. In het zoekvak vinden 'logic apps' en selecteer vervolgens uw logische app in de resultaten.

3. In het menu van uw logische app onder **ontwikkeltools**, selecteer **codeweergave van logische App**.

   De weergave van Code-editor wordt geopend en toont de definitie van uw logische app in JSON-indeling.

## <a name="edit-json---visual-studio"></a>JSON - Visual Studio bewerken

Voordat u aan de definitie van uw logische app in Visual Studio werken kunt, zorgt u ervoor dat u hebt [de benodigde hulpprogramma's geïnstalleerd](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#prerequisites). Voor het maken van een logische app met Visual Studio, Bekijk [Quick Start: automatiseren van taken en processen met Azure Logic Apps - Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

U kunt logische apps die zijn gemaakt en geïmplementeerd hetzij rechtstreeks vanuit Azure portal of Azure Resource Manager-projecten vanuit Visual Studio openen in Visual Studio.

1. Open de Visual Studio-oplossing of [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) project, met daarin uw logische app.

2. Zoeken en openen van de definitie van uw logische app, die standaard wordt weergegeven in een [Resource Manager-sjabloon](../azure-resource-manager/resource-group-overview.md#template-deployment)met de naam **LogicApp.json**. U kunt gebruiken en aanpassen van deze sjabloon voor implementatie naar verschillende omgevingen.

3. Open het snelmenu voor uw logische app definitie en de sjabloon. Selecteer **Openen met Logic App-ontwerpfunctie**.

   ![Open logische app in een Visual Studio-oplossing](./media/logic-apps-author-definitions/open-logic-app-designer.png)

4. Kies aan de onderkant van de ontwerpfunctie **codeweergave**. 

   De weergave van Code-editor wordt geopend en toont de definitie van uw logische app in JSON-indeling.

5. Als u wilt terugkeren naar de ontwerpfunctie weergave aan de onderkant van de weergave van Code-editor kiest **ontwerp**.

## <a name="parameters"></a>Parameters

Parameters kunnen u waarden in uw logische app opnieuw kunt gebruiken als geschikt zijn voor het vervangen van waarden die u vaak veranderen. Hebt u een e-mailadres dat u gebruiken op meerdere plaatsen wilt, moet u dat e-mailadres bijvoorbeeld, als een parameter definiëren. 

Parameters zijn ook handig als u wilt overschrijven van de parameters in verschillende omgevingen, meer informatie over [parameters voor de implementatie van](#deployment-parameters) en de [REST-API voor Azure Logic Apps-documentatie](https://docs.microsoft.com/rest/api/logic).

> [!NOTE]
> Parameters zijn alleen beschikbaar in de codeweergave.

In de [eerste voorbeeld van de logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md), u hebt gemaakt met een werkstroom waarmee e-mailberichten wanneer nieuwe berichten worden weergegeven in de RSS-feed van een website wordt verzonden. URL van de feed wordt vastgelegd, zodat in dit voorbeeld laat hoe zien u de querywaarde met een parameter vervangt, zodat u de URL van feed eenvoudiger kunt wijzigen.

1. In de weergave van code, vinden de `parameters : {}` -object en voeg een `currentFeedUrl` object:

   ``` json
     "currentFeedUrl" : {
      "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
   }
   ```

2. In de `When_a_feed-item_is_published` actie, vinden de `queries` uit en vervang de querywaarde met `"feedUrl": "#@{parameters('currentFeedUrl')}"`. 

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
   Bijvoorbeeld, `"@concat('#',parameters('currentFeedUrl'))"` werkt op dezelfde manier als het vorige voorbeeld.

3.  Als u bent klaar, kiest u **Opslaan**. 

Nu kunt u de website van de RSS-feed door een andere URL via de `currentFeedURL` object.

<a name="deployment-parameters"></a>

## <a name="deployment-parameters-for-different-environments"></a>Implementatieparameters voor verschillende omgevingen

Levenscycli van implementatie hebben doorgaans, ontwikkeling, fasering en productie-omgevingen. Bijvoorbeeld, u mogelijk dezelfde definitie voor de logische app in alle omgevingen met deze maar verschillende databases gebruikt. Op dezelfde manier kunt u gebruikmaken van dezelfde definitie in verschillende regio's voor maximale beschikbaarheid, waarbij elk exemplaar van de logische app het gebruik van de database van die regio. 

> [!NOTE] 
> In dit scenario verschilt van de parameters op te nemen *runtime* waar moet u de `trigger()` werken in plaats daarvan.

Hier volgt de definitie van een basic:

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
In de werkelijke `PUT` aanvragen voor de logic apps, kunt u de parameter bieden `uri`. In elke omgeving, kunt u een andere waarde voor opgeven de `connection` parameter. Omdat een standaardwaarde niet meer bestaat, is deze parameter vereist in de nettolading van de logische app:

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

Zie voor meer informatie, de [REST-API voor Azure Logic Apps-documentatie](https://docs.microsoft.com/rest/api/logic/).

## <a name="process-strings-with-functions"></a>Proces-tekenreeksen met functions

Logic Apps heeft diverse functies voor het werken met tekenreeksen. Stel bijvoorbeeld dat u wilt de naam van een bedrijf van een order doorgeven aan een ander systeem. Echter, u niet zeker weet over het afhandelen van de juiste voor tekencodering. Base64-codering op deze tekenreeks kan worden uitgevoerd, maar om te voorkomen dat Hiermee heft u in de URL, kunt u verschillende tekens in plaats daarvan vervangen. Bovendien hoeft u alleen een subtekenreeks voor naam van het bedrijf omdat de eerste vijf tekens worden niet gebruikt. 

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

Deze stappen wordt beschreven hoe deze tekenreeks, werken op basis van binnen naar buiten in dit voorbeeld worden verwerkt:

``` 
"uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').companyName,5,sub(length(parameters('order').companyName), 5) )),'+','-') ,'/' ,'_' )}"
```

1. Krijgen de [ `length()` ](../logic-apps/logic-apps-workflow-definition-language.md) voor de naam van het bedrijf, zodat u het totale aantal tekens.

2. Als u een kortere tekenreeks, moet worden afgetrokken `5`.

3. Nu u een [ `substring()` ](../logic-apps/logic-apps-workflow-definition-language.md). Beginnen bij index `5`, en Ga naar de rest van de tekenreeks.

4. Deze reeks die u wilt converteren een [ `base64()` ](../logic-apps/logic-apps-workflow-definition-language.md) tekenreeks.

5. Nu [ `replace()` ](../logic-apps/logic-apps-workflow-definition-language.md) alle de `+` tekens met `-` tekens.

6. Ten slotte [ `replace()` ](../logic-apps/logic-apps-workflow-definition-language.md) alle de `/` tekens met `_` tekens.

## <a name="map-list-items-to-property-values-then-use-maps-as-parameters"></a>Items in de lijst worden toegewezen aan de waarden van eigenschappen en vervolgens de kaarten gebruiken als parameters

Als u verschillende resultaten op basis van de waarde van een eigenschap, kunt u een map die overeenkomt met de eigenschapswaarde van elke een resultaat maken en vervolgens gebruiken die zijn toegewezen als een parameter. 

Deze werkstroom wordt bijvoorbeeld sommige categorieën gedefinieerd als parameters en een kaart die overeenkomt met deze categorieën met een specifieke URL. De werkstroom wordt eerst een lijst met artikelen. De werkstroom gebruikt vervolgens de kaart om te vinden van de URL die overeenkomt met de categorie voor elk artikel.

*   De [ `intersection()` ](../logic-apps/logic-apps-workflow-definition-language.md) functie wordt gecontroleerd of de categorie overeenkomt met een bekende gedefinieerde categorie.

*   Nadat u een overeenkomende categorie, haalt het voorbeeld het item uit de toewijzing tussen vierkante haken gebruiken: `parameters[...]`

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

## <a name="get-data-with-date-functions"></a>Ophalen van gegevens met datumfuncties

Gegevens ophalen uit een gegevensbron die geen systeemeigen ondersteuning bieden voor *triggers*, kunt u de datum functies voor het werken met tijden en datums in plaats daarvan. Bijvoorbeeld, hoe lang de stappen van deze werkstroom worden duurt, wordt gevonden voor deze expressie werken op basis van binnen naar buiten:

``` json
"expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
```

1. Uit de `order` actie, extraheren de `startTime`. 
2. Ophalen van de huidige tijd met `utcNow()`.
3. Aftrekken van één seconde:

   [`addseconds(..., -1)`](../logic-apps/logic-apps-workflow-definition-language.md) 

   U kunt andere tijdseenheden, zoals `minutes` of `hours`. 

3. U kunt nu deze twee waarden vergelijken. 

   Als de eerste waarde kleiner dan de tweede waarde en klik vervolgens op meer dan één seconde is is verstreken sinds de order voor het eerst is geplaatst.

Als u wilt opmaken datums, kunt u tekenreeks bevatten. Bijvoorbeeld, als u de RFC1123, gebruikt [ `utcnow('r')` ](../logic-apps/logic-apps-workflow-definition-language.md). Meer informatie over [datum opmaak](../logic-apps/logic-apps-workflow-definition-language.md).

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
* [Voer stappen uit op basis van verschillende waarden (switch-instructies)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Uitvoeren en herhaalt u stap (lussen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Uitvoeren of samenvoegen van parallelle stappen (vertakkingen)](../logic-apps/logic-apps-control-flow-branches.md)
* [Voer stappen uit op basis van gegroepeerde Actiestatus (bereiken)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
* Meer informatie over de [Definitietaal van werkstroom-schema voor Azure Logic Apps](../logic-apps/logic-apps-workflow-definition-language.md)
* Meer informatie over [werkstroomacties en triggers voor Azure Logic Apps](../logic-apps/logic-apps-workflow-actions-triggers.md)