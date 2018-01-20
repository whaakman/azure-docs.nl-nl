---
title: Automatisering van resource voor een functie-app in Azure Functions | Microsoft Docs
description: "Informatie over het bouwen van een Azure Resource Manager-sjabloon die de functie-app wordt geïmplementeerd."
services: Functions
documtationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: Azure functions, functies, zonder server architectuur, infrastructuur als code, azure resourcemanager
ms.assetid: d20743e3-aab6-442c-a836-9bcea09bfd32
ms.server: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: glenga
ms.openlocfilehash: 6f31ba7b43c70f52bdd67d27512a322ec6258608
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a>Implementatie van de resource voor de functie-app in Azure Functions automatiseren

U kunt een Azure Resource Manager-sjabloon gebruiken voor het implementeren van een functie-app. In dit artikel bevat een overzicht van de vereiste resources en de parameters voor doet. Mogelijk moet u aanvullende resources implementeren afhankelijk van de [triggers en bindingen](functions-triggers-bindings.md) in uw app functie.

Zie voor meer informatie over het maken van sjablonen [Azure Resource Manager-sjablonen samenstellen](../azure-resource-manager/resource-group-authoring-templates.md).

Zie voor een voorbeeldsjablonen:
- [functie-app van verbruik plan]
- [functie-app in Azure App Service-plan]

## <a name="required-resources"></a>Vereiste resources

Een functie-app is vereist voor deze resources:

* Een [Azure Storage](../storage/index.yml) account
* Een hosting-indeling (verbruik plan of App Service-abonnement)
* Een functie-app 

### <a name="storage-account"></a>Storage-account

Een Azure storage-account is vereist voor een functie-app. U moet een account voor algemene doeleinden die ondersteuning biedt voor blobs, tabellen, wachtrijen en -bestanden. Zie voor meer informatie [-account voor Azure Functions opslagvereisten](functions-create-function-app-portal.md#storage-account-requirements).

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

Daarnaast biedt de eigenschappen `AzureWebJobsStorage` en `AzureWebJobsDashboard` moet worden opgegeven als de app-instellingen in de siteconfiguratie. De Azure Functions-runtime gebruikt de `AzureWebJobsStorage` verbindingsreeks interne wachtrijen maken. De verbindingsreeks `AzureWebJobsDashboard` wordt gebruikt voor aanmelding tot Azure Table storage en power de **Monitor** tabblad in de portal.

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

### <a name="hosting-plan"></a>Hosting-plan

De definitie van het plan hosting varieert, afhankelijk van of u een verbruik of App Service-abonnement gebruiken. Zie [implementeren van een functie-app op het plan verbruik](#consumption) en [implementeren van een functie-app op het App Service-abonnement](#app-service-plan).

### <a name="function-app"></a>Functie-app

De resource voor de functie-app wordt gedefinieerd door middel van een bron van het type **Microsoft.Web/Site** en type **functionapp**:

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

## <a name="deploy-a-function-app-on-the-consumption-plan"></a>Een functie-app op het plan verbruik implementeren

U kunt een functie-app uitvoeren in twee verschillende modi: het plan verbruik en de App Service-abonnement. Het plan verbruik wijst automatisch rekencapaciteit wanneer uw code wordt uitgevoerd, indien nodig om belasting te verwerken uitgeschaald en vervolgens omlaag geschaald wanneer de code wordt niet uitgevoerd. Dus u hoeft te betalen voor niet-actieve virtuele machines en u hoeft te worden van tevoren capaciteit reserveren. Zie voor meer informatie over hostingplannen, [Azure Functions gebruiks- en App Service-plannen](functions-scale.md).

Zie voor een Azure Resource Manager voorbeeldsjabloon [functie-app van verbruik plan].

### <a name="create-a-consumption-plan"></a>Maak een plan verbruik

Een plan verbruik is een speciaal type resource 'serverfarm'. U het opgeeft via de `Dynamic` waarde voor de `computeMode` en `sku` eigenschappen:

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

Bovendien een plan verbruik vereist twee extra instellingen in de siteconfiguratie: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` en `WEBSITE_CONTENTSHARE`. Deze eigenschappen configureren voor de storage-account en het bestandspad waar de functie app-code en de configuratie zijn opgeslagen.

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

## <a name="deploy-a-function-app-on-the-app-service-plan"></a>Implementeren van een functie-app op het App Service-abonnement

In de App Service-abonnement functie-app uitgevoerd via exclusieve virtuele machines op Basic, Standard en Premium-SKU's, vergelijkbaar met web-apps. Zie voor meer informatie over de werking van de App Service-abonnement de [gedetailleerd overzicht van Azure App Service-plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Zie voor een Azure Resource Manager voorbeeldsjabloon [functie-app in Azure App Service-plan].

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

Nadat u een optie schaling hebt geselecteerd, maakt u een functie-app. De app is de container waarin alle uw functies.

Een functie-app heeft veel onderliggende resources die u in uw implementatie gebruiken kunt, met inbegrip van app-instellingen en opties voor beheer. U kunt desgewenst ook verwijderen de **sourcecontrols** onderliggende resource, en gebruik een andere [Implementatieoptie](functions-continuous-deployment.md) in plaats daarvan.

> [!IMPORTANT]
> Als u wilt uw toepassing implementeren met behulp van Azure Resource Manager, is het belangrijk te begrijpen hoe resources worden geïmplementeerd in Azure. In het volgende voorbeeld wordt op het hoogste niveau configuraties zijn toegepast met behulp van **siteConfig**. Het is belangrijk dat deze configuraties ingesteld op het hoogste niveau, omdat ze gegevens naar de runtime en implementatie-engine van functies worden beschouwd. Op het hoogste niveau informatie is vereist voordat de onderliggende **sourcecontrols of web** resource wordt toegepast. Hoewel het mogelijk deze instellingen te configureren in het onderliggende niveau **config/appSettings** bron, in sommige gevallen functie-app moet worden geïmplementeerd *voordat* **config/appSettings** wordt toegepast. Bijvoorbeeld, wanneer u werkt in combinatie met gebruikt [Logic Apps](../logic-apps/index.yml), uw functies zijn afhankelijk van een andere resource.

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
> Deze sjabloon worden gebruikt voor de [Project](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) waarde van app-instellingen, waarmee de basismap waarin de implementatie-engine van functies (Kudu) wordt gezocht voor implementeerbare code wordt ingesteld. In onze opslagplaats onze functies zijn in een submap van de **src** map. Dus in het voorgaande voorbeeld wordt de app instellingen waarde instelt op `src`. Als uw functies in de hoofdmap van uw opslagplaats, of als u niet vanuit resourcebeheer implementeert, kunt u de waarde van deze app-instellingen verwijderen.

## <a name="deploy-your-template"></a>De sjabloon implementeren

U kunt een van de volgende manieren om uw sjabloon te implementeren:

* [PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure-CLI](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [Azure Portal](../azure-resource-manager/resource-group-template-deploy-portal.md)
* [REST API](../azure-resource-manager/resource-group-template-deploy-rest.md)

### <a name="deploy-to-azure-button"></a>Implementeren naar Azure-knop

Vervang ```<url-encoded-path-to-azuredeploy-json>``` met een [URL-codering](https://www.bing.com/search?q=url+encode) versie van het pad onbewerkte van uw `azuredeploy.json` -bestand in GitHub.

Hier volgt een voorbeeld die gebruikmaakt van markdown:

```markdown
[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

Hier volgt een voorbeeld waarin HTML wordt gebruikt:

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"></a>
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het ontwikkelen en configureren van Azure Functions.

* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)
* [Het Azure-functie app-instellingen configureren](functions-how-to-use-azure-function-app-settings.md)
* [Uw eerste Azure-functie maken](functions-create-first-azure-function.md)

<!-- LINKS -->

[functie-app van verbruik plan]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[functie-app in Azure App Service-plan]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json
