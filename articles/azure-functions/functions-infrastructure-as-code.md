---
title: Automatiseer de implementatie van de resource voor een functie-app in Azure Functions | Microsoft Docs
description: Meer informatie over het bouwen van een Azure Resource Manager-sjabloon die uw functie-app implementeert.
services: Functions
documtationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure functions, functions, serverloze architectuur, infrastructuur als code, azure resourcemanager
ms.assetid: d20743e3-aab6-442c-a836-9bcea09bfd32
ms.server: functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: glenga
ms.openlocfilehash: d63686524c619b349a590c389e20e473b0d98641
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44091471"
---
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a>Automatiseer de implementatie van de resource voor uw functie-app in Azure Functions

U kunt een Azure Resource Manager-sjabloon gebruiken om een functie-app te implementeren. In dit artikel bevat een overzicht van de vereiste resources en de parameters voor het doen. Mogelijk moet u aanvullende resources implementeren afhankelijk van de [triggers en bindingen](functions-triggers-bindings.md) in uw functie-app.

Zie voor meer informatie over het maken van sjablonen, [Authoring Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md).

Zie voor voorbeeldsjablonen:
- [Functie-app op verbruiksabonnement]
- [Functie-app in Azure App Service-plan]

## <a name="required-resources"></a>Vereiste resources

Een functie-app is vereist voor deze resources:

* Een [Azure Storage](../storage/index.yml) account
* Een hostingplan (verbruiksabonnement of App Service-plan)
* Een functie-app 

### <a name="storage-account"></a>Storage-account

Een Azure storage-account is vereist voor een functie-app. U moet een account voor algemeen gebruik die ondersteuning biedt voor blobs, tabellen, wachtrijen en bestanden. Zie voor meer informatie, [vereisten voor Azure Functions een opslagaccount](functions-create-function-app-portal.md#storage-account-requirements).

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
        "accountType": "[parameters('storageAccountType')]"
    }
}
```

Daarnaast kunnen de eigenschap `AzureWebJobsStorage` moet worden opgegeven als een app-instelling in configuratie van de site. Als geen Application Insights in de functie-app voor bewaking gebruiken, moet ook opgeven `AzureWebJobsDashboard` als een app-instelling.

Maakt gebruik van de Azure Functions-runtime de `AzureWebJobsStorage` verbindingsreeks interne wachtrijen maken.  De runtime wordt gebruikt als Application Insights is niet ingeschakeld, de `AzureWebJobsDashboard` verbindingsreeks om aan te melden voor Azure Table storage en power de **Monitor** tabblad in de portal.

Deze eigenschappen zijn opgegeven in de `appSettings` verzameling in de `siteConfig` object:

```json
"appSettings": [
    {
        "name": "AzureWebJobsStorage",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    },
    {
        "name": "AzureWebJobsDashboard",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    }
```    

### <a name="hosting-plan"></a>Hostingabonnement

De definitie van het hostingabonnement varieert, afhankelijk van of u een planning of op uw App Service gebruiken. Zie [implementeren van een functie-app op in het abonnement Consumption](#consumption) en [implementeren van een functie-app op het App Service-plan](#app-service-plan).

### <a name="function-app"></a>Function App

De resource voor de functie-app wordt gedefinieerd door middel van een resource van het type **Microsoft.Web/Site** en type **functionapp**:

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",            
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ]
```

<a name="consumption"></a>

## <a name="deploy-a-function-app-on-the-consumption-plan"></a>Een functie-app op in het abonnement Consumption implementeren

U kunt een functie-app in twee verschillende modi uitvoeren: het plan verbruik en het App Service-plan. Het verbruiksabonnement rekencapaciteit automatisch toegewezen wanneer uw code wordt uitgevoerd, wordt uitgeschaald naar behoefte om belasting te verwerken en vervolgens schaalt omlaag wanneer de code wordt niet uitgevoerd. Dus u hoeft te betalen voor niet-actieve virtuele machines en u hoeft te voren capaciteit reserveren. Zie voor meer informatie over het hosten van plannen, [Azure Functions-verbruik en App Service-abonnementen](functions-scale.md).

Zie voor een voorbeeld van Azure Resource Manager-sjabloon, [functie-app op verbruiksabonnement].

### <a name="create-a-consumption-plan"></a>Maken van een verbruiksabonnement

Een verbruiksabonnement is een speciaal type "serverfarm" resource. U het opgeeft met behulp van de `Dynamic` waarde voor de `computeMode` en `sku` eigenschappen:

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "computeMode": "Dynamic",
        "sku": "Dynamic"
    }
}
```

### <a name="create-a-function-app"></a>Een functie-app maken

Bovendien twee extra instellingen in de configuratie van de site in een verbruiksabonnement is vereist: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` en `WEBSITE_CONTENTSHARE`. Deze eigenschappen configureren de storage-account en het bestandspad waarin de functie-app-code en de configuratie zijn opgeslagen.

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",            
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsDashboard",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTSHARE",
                    "value": "[toLower(variables('functionAppName'))]"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~1"
                }
            ]
        }
    }
}
```                    

<a name="app-service-plan"></a> 

## <a name="deploy-a-function-app-on-the-app-service-plan"></a>Implementeren van een functie-app op het App Service-plan

In de App Service-plan is de functie-app wordt uitgevoerd op toegewezen virtuele machines op basis, standaard en Premium-SKU's, vergelijkbaar met de web-apps. Zie voor meer informatie over de werking van de App Service-plan de [gedetailleerd overzicht van Azure App Service-plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Zie voor een voorbeeld van Azure Resource Manager-sjabloon, [functie-app in Azure App Service-plan].

### <a name="create-an-app-service-plan"></a>Een App Service-plan maken

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "sku": "[parameters('sku')]",
        "workerSize": "[parameters('workerSize')]",
        "hostingEnvironment": "",
        "numberOfWorkers": 1
    }
}
```

### <a name="create-a-function-app"></a>Een functie-app maken 

Nadat u een optie voor vergroten/verkleinen hebt geselecteerd, maakt u een functie-app. De app is de container met alle uw functies.

Een functie-app heeft veel onderliggende resources die u in uw implementatie gebruiken kunt, met inbegrip van app-instellingen en opties voor broncodebeheer. U kunt desgewenst ook verwijderen de **sourcecontrols** onderliggende resources en gebruik een andere [Implementatieoptie](functions-continuous-deployment.md) in plaats daarvan.

> [!IMPORTANT]
> Als u wilt uw toepassing implementeren met behulp van Azure Resource Manager, is het belangrijk om te begrijpen hoe resources worden geïmplementeerd in Azure. In het volgende voorbeeld wordt op het hoogste niveau configuraties zijn toegepast met behulp van **siteConfig**. Het is belangrijk dat deze configuraties instellen op het hoogste niveau, omdat ze overbrengen van gegevens met de Functions-runtime en implementatie-engine. Op het hoogste niveau informatie is vereist voordat het onderliggende **sourcecontrols/web** resource wordt toegepast. Hoewel het mogelijk deze instellingen configureren in het onderliggende niveau **config/appSettings** resource, in sommige gevallen de functie-app moet worden geïmplementeerd *voordat* **config/appSettings**  wordt toegepast. Bijvoorbeeld, wanneer u werkt met gebruikt [Logic Apps](../logic-apps/index.yml), uw functies zijn afhankelijk van een andere resource.

```json
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('appName')]",
  "type": "Microsoft.Web/sites",
  "kind": "functionapp",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Web/serverfarms', parameters('appName'))]"
  ],
  "properties": {
     "serverFarmId": "[variables('appServicePlanName')]",
     "siteConfig": {
        "alwaysOn": true,
        "appSettings": [
            { "name": "FUNCTIONS_EXTENSION_VERSION", "value": "~1" },
            { "name": "Project", "value": "src" }
        ]
     }
  },
  "resources": [
     {
        "apiVersion": "2015-08-01",
        "name": "appsettings",
        "type": "config",
        "dependsOn": [
          "[resourceId('Microsoft.Web/Sites', parameters('appName'))]",
          "[resourceId('Microsoft.Web/Sites/sourcecontrols', parameters('appName'), 'web')]",
          "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
        ],
        "properties": {
          "AzureWebJobsStorage": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]",
          "AzureWebJobsDashboard": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
        }
     },
     {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites/', parameters('appName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('sourceCodeRepositoryURL')]",
            "branch": "[parameters('sourceCodeBranch')]",
            "IsManualIntegration": "[parameters('sourceCodeManualIntegration')]"
          }
     }
  ]
}
```
> [!TIP]
> Deze sjabloon maakt gebruik van de [Project](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) waarde van app-instellingen, die de basismap waarin de functies implementatie-engine (Kudu) code kan worden geïmplementeerd zoekt. In onze opslagplaats onze functies zijn in een submap van de **src** map. Daarom stellen we in het voorgaande voorbeeld wordt de waarde van de app-instellingen op `src`. Als uw functies in de hoofdmap van uw opslagplaats zijn, of als u niet vanuit broncodebeheer implementeert, kunt u de waarde van deze app-instellingen verwijderen.

## <a name="deploy-your-template"></a>De sjabloon implementeren

U kunt een van de volgende manieren om uw sjabloon te implementeren:

* [PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure-CLI](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [Azure Portal](../azure-resource-manager/resource-group-template-deploy-portal.md)
* [REST API](../azure-resource-manager/resource-group-template-deploy-rest.md)

### <a name="deploy-to-azure-button"></a>Implementeren op Azure-knop

Vervang ```<url-encoded-path-to-azuredeploy-json>``` met een [URL-gecodeerde](https://www.bing.com/search?q=url+encode) versie van de onbewerkte pad van uw `azuredeploy.json` -bestand in GitHub.

Hier volgt een voorbeeld dat gebruikmaakt van markdown:

```markdown
[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

Hier volgt een voorbeeld waarin HTML wordt gebruikt:

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"></a>
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het ontwikkelen en Azure Functions configureert.

* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)
* [Azure-functie-app-instellingen configureren](functions-how-to-use-azure-function-app-settings.md)
* [Uw eerste Azure-functie maken](functions-create-first-azure-function.md)

<!-- LINKS -->

[Functie-app op verbruiksabonnement]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[Functie-app in Azure App Service-plan]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json
