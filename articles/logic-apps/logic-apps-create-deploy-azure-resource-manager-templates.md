---
title: Logische apps maken met Azure Resource Manager-sjablonen - Azure Logic Apps | Microsoft Docs
description: Maken en implementeren van werkstromen voor logische app met Azure Resource Manager-sjablonen in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.assetid: 7574cc7c-e5a1-4b7c-97f6-0cffb1a5d536
ms.date: 10/15/2017
ms.openlocfilehash: 8ad70c5d22ca73258fa9e6501d03d5409a4e45d8
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652481"
---
# <a name="create-and-deploy-logic-apps-with-azure-resource-manager-templates"></a>Logische apps met Azure Resource Manager-sjablonen maken en implementeren

Met Azure Logic Apps biedt Azure Resource Manager-sjablonen die u gebruiken kunt, niet alleen om logische apps voor het automatiseren van werkstromen te maken, maar ook om de resources en parameters die worden gebruikt voor de implementatie te definiëren.
U kunt deze sjabloon gebruiken voor uw eigen zakelijke scenario's of de sjabloon om te voldoen aan uw behoeften aanpassen. Meer informatie over de [Resource Manager-sjabloon voor logische apps](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json) en [structuur van Azure Resource Manager-sjabloon en de syntaxis](../azure-resource-manager/resource-group-authoring-templates.md). Zie voor JSON-syntaxis en eigenschappen [Microsoft.Logic resourcetypen](/azure/templates/microsoft.logic/allversions).

## <a name="define-the-logic-app"></a>De logische app definiëren
De definitie van dit voorbeeld van de logische app eenmaal per uur wordt uitgevoerd en pingt de locatie die is opgegeven de `testUri` parameter.
De sjabloon maakt gebruik van parameterwaarden voor de naam van de logische app (```logicAppName```) en de locatie op voor het testen van de opdracht ping (```testUri```). Meer informatie over [definiëren van deze parameters in uw sjabloon](#define-parameters).
De sjabloon wordt de locatie voor de logische app ook ingesteld op dezelfde locatie als de Azure-resourcegroep.

```json
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
         "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
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

### <a name="define-parameters"></a>De parameters definiëren

[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

Hier volgen beschrijvingen voor de parameters in de sjabloon:

| Parameter | Description | Voorbeeld van JSON-definitie |
| --------- | ----------- | ----------------------- |
| `logicAppName` | Hiermee definieert u de naam van de logische app die sjabloon wordt gemaakt. | "logicAppName": {"type": "string", "metagegevens": {'description': "myExampleLogicAppName"}} |
| `testUri` | Definieert de locatie voor het testen van ping. | "testUri": { "type": "string", "defaultValue": "https://azure.microsoft.com/status/feed/"} |
||||

Meer informatie over [REST-API voor Logic Apps-werkstroom-definitie en eigenschappen](https://docs.microsoft.com/rest/api/logic/workflows) en [bouwen op definities voor logische Apps met JSON](logic-apps-author-definitions.md).

## <a name="deploy-logic-apps-automatically"></a>Automatisch implementeren met logische apps

Als u wilt maken en een logische app automatisch geïmplementeerd op Azure, kies **implementeren in Azure** hier:

[![Implementeren in Azure](./media/logic-apps-create-deploy-azure-resource-manager-templates/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

Deze actie zich aanmeldt bij Azure portal kunt u uw logische app details bieden en wijzigingen aanbrengen in de sjabloon of de parameters.
Bijvoorbeeld, de Azure-portal wordt u gevraagd deze details:

* De naam van de Azure-abonnement
* Resourcegroep die u wilt gebruiken
* Logic app-locatie
* Een naam voor uw logische app
* Een test-URI
* Instemming met de opgegeven voorwaarden en bepalingen

## <a name="deploy-logic-apps-with-commands"></a>Logische apps met opdrachten implementeren

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

```powershell
New-AzResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup
```

### <a name="azure-cli"></a>Azure-CLI

```azurecli
azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup
```

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Logische apps controleren](../logic-apps/logic-apps-monitor-your-logic-apps.md)
