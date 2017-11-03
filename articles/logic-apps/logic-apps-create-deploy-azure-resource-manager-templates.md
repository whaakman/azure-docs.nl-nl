---
title: Logische apps van Azure Resource Manager-sjablonen maken | Microsoft Docs
description: Maken en implementeren van logic app werkstromen met Azure Resource Manager-sjablonen
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7574cc7c-e5a1-4b7c-97f6-0cffb1a5d536
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: LADocs; mandia
ms.openlocfilehash: e30ed8b1b8e2241bbebab1d7c5f337fabf37e1dd
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/21/2017
---
# <a name="create-and-deploy-logic-apps-with-azure-resource-manager-templates"></a>Logic apps met Azure Resource Manager-sjablonen maken en implementeren

Logische Apps van Azure biedt Azure Resource Manager-sjablonen die u gebruiken kunt, niet alleen voor het maken van logische apps voor het automatiseren van werkstromen, maar ook voor het definiëren van de resources en de parameters die worden gebruikt voor implementatie. U kunt deze sjabloon voor uw eigen zakelijke scenario's gebruiken of aanpassen van de sjabloon die moeten voldoen aan uw vereisten. Meer informatie over de [Resource Manager-sjabloon voor logische apps](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json) en [structuur van Azure Resource Manager-sjabloon en de syntaxis](../azure-resource-manager/resource-group-authoring-templates.md).

## <a name="define-the-logic-app"></a>De logische app definiëren

De definitie voor dit voorbeeld logic app eens per uur wordt uitgevoerd en pingt de locatie die is opgegeven in de `testUri` parameter.
De sjabloon maakt gebruik van parameterwaarden voor de naam van de logic app (```logicAppName```) en de locatie op voor het testen van ping (```testUri```). Meer informatie over [voor het definiëren van deze parameters in de sjabloon](#define-parameters). De sjabloon wordt ook de locatie voor de logische app ingesteld op dezelfde locatie als de Azure-resourcegroep. 

``` json
{
    "type": "Microsoft.Logic/workflows",
    "apiVersion": "2016-06-01",
    "name": "[parameters('logicAppName')]",
    "location": "[resourceGroup().location]",
    "tags": {
        "displayName": "LogicApp"
    },
    "properties": {
        "definition": {
            "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
                "testURI": {
                "type": "string",
                "defaultValue": "[parameters('testUri')]"
                }
            },
            "triggers": {
                "Recurrence": {
                    "type": "Recurrence",
                    "recurrence": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            },
            "actions": {
                "Http": {
                    "type": "Http",
                    "inputs": {
                        "method": "GET",
                        "uri": "@parameters('testUri')"
                    },
                    "runAfter": {}
                }
            },
            "outputs": {}
        },
        "parameters": {}
    }
}
``` 

<a name="define-parameters"></a>

### <a name="define-parameters"></a>parameters definiëren

[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

Hier volgen beschrijvingen van de parameters in de sjabloon:

| Parameter | Beschrijving | Voorbeeld van JSON-definitie | 
| --------- | ----------- | ----------------------- | 
| `logicAppName` | Hiermee definieert u de naam van de logische app dat de sjabloon wordt gemaakt. | "logicAppName": {'type': 'string', "metagegevens": {'beschrijving': 'myExampleLogicAppName'}} |
| `testUri` | Hiermee definieert u de locatie op voor het testen van ping. | "testUri": {'type': 'string', "defaultValue": "http://azure.microsoft.com/status/feed/"} | 
||||

Meer informatie over [REST-API voor werkstroomdefinitie van Logic Apps en eigenschappen](https://docs.microsoft.com/rest/api/logic/workflows) en [bouwen op logic app-definities met JSON](logic-apps-author-definitions.md).

## <a name="deploy-logic-apps-automatically"></a>Logische apps automatisch implementeren

Als u wilt maken en implementeren automatisch een logische app in Azure, kies **implementeren in Azure** hier:

[![Implementeren in Azure](./media/logic-apps-create-deploy-azure-resource-manager-templates/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

Deze actie zich aanmeldt bij de Azure portal kunt u uw bedrijfslogica Geef details op van de app en wijzigingen aanbrengen in de sjabloon of de parameters. Bijvoorbeeld, de Azure-portal wordt u gevraagd deze details:

* De naam van de Azure-abonnement
* Resourcegroep die u wilt gebruiken
* Logic app locatie
* Een naam voor uw logische app
* Een test-URI
* Acceptatie van de opgegeven voorwaarden en bepalingen

## <a name="deploy-logic-apps-with-commands"></a>Logic apps implementeren met opdrachten

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup
``` 

### <a name="azure-cli"></a>Azure CLI

```
azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup
```

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Logische apps controleren](../logic-apps/logic-apps-monitor-your-logic-apps.md)