---
title: Automatiseer de implementatie van de resource voor een functie-app in Azure Functions | Microsoft Docs
description: Meer informatie over het bouwen van een Azure Resource Manager-sjabloon die uw functie-app implementeert.
services: Functions
documtationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure functions, functions, serverloze architectuur, infrastructuur als code, azure resourcemanager
ms.assetid: d20743e3-aab6-442c-a836-9bcea09bfd32
ms.service: azure-functions
ms.server: functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: glenga
ms.openlocfilehash: 5d028768c062ef7df74d48f83ccc4e27a506f1ac
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60737054"
---
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a>Automatiseer de implementatie van de resource voor uw functie-app in Azure Functions

U kunt een Azure Resource Manager-sjabloon gebruiken om een functie-app te implementeren. In dit artikel bevat een overzicht van de vereiste resources en de parameters voor het doen. Mogelijk moet u aanvullende resources implementeren afhankelijk van de [triggers en bindingen](functions-triggers-bindings.md) in uw functie-app.

Zie voor meer informatie over het maken van sjablonen, [Authoring Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md).

Zie voor voorbeeldsjablonen:
- [Functie-app op verbruiksabonnement]
- [Functie-app in Azure App Service-plan]

> [!NOTE]
> Het Premium-abonnement voor het hosten van Azure Functions is momenteel in preview. Zie voor meer informatie, [Azure Functions Premium-abonnement](functions-premium-plan.md).

## <a name="required-resources"></a>Vereiste resources

De implementatie van een Azure Functions bestaat gewoonlijk uit deze resources:

| Resource                                                                           | Vereiste | Documentatie over de syntaxis en eigenschappen                                                         |   |
|------------------------------------------------------------------------------------|-------------|-----------------------------------------------------------------------------------------|---|
| Een functie-app                                                                     | Vereist    | [Microsoft.Web/sites](/azure/templates/microsoft.web/sites)                             |   |
| Een [Azure Storage](../storage/index.yml) account                                   | Vereist    | [Microsoft.Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts) |   |
| Een [Application Insights](../azure-monitor/app/app-insights-overview.md) onderdeel | Optioneel    | [Microsoft.Insights/components](/azure/templates/microsoft.insights/components)         |   |
| Een [hostingplan](./functions-scale.md)                                             | Optionele<sup>1</sup>    | [Microsoft.Web/serverfarms](/azure/templates/microsoft.web/serverfarms)                 |   |

<sup>1</sup>een hostingplan is alleen vereist als u kiest voor het uitvoeren van uw functie-app op een [Premium-abonnement](./functions-premium-plan.md) (in Preview-versie) of op een [App Service-plan](../app-service/overview-hosting-plans.md).

> [!TIP]
> Hoewel het niet vereist, is het raadzaam dat u Application Insights voor uw app configureren.

<a name="storage"></a>
### <a name="storage-account"></a>Storage-account

Een Azure storage-account is vereist voor een functie-app. U moet een account voor algemeen gebruik die ondersteuning biedt voor blobs, tabellen, wachtrijen en bestanden. Zie voor meer informatie, [vereisten voor Azure Functions een opslagaccount](functions-create-function-app-portal.md#storage-account-requirements).

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2018-07-01",
    "location": "[resourceGroup().location]",
    "kind": "StorageV2",
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
]
```

### <a name="application-insights"></a>Application Insights

Application Insights wordt aanbevolen voor het bewaken van uw functie-apps. De Application Insights-resource is gedefinieerd met het type **Microsoft.Insights/components** en het type **web**:

```json
        {
            "apiVersion": "2015-05-01",
            "name": "[variables('appInsightsName')]",
            "type": "Microsoft.Insights/components",
            "kind": "web",
            "location": "[resourceGroup().location]",
            "tags": {
                "[concat('hidden-link:', resourceGroup().id, '/providers/Microsoft.Web/sites/', variables('functionAppName'))]": "Resource"
            },
            "properties": {
                "Application_Type": "web",
                "ApplicationId": "[variables('functionAppName')]"
            }
        },
```

Bovendien de instrumentatiesleutel moet worden opgegeven met de functie app via de `APPINSIGHTS_INSTRUMENTATIONKEY` toepassingsinstelling. Deze eigenschap is opgegeven in de `appSettings` verzameling in de `siteConfig` object:

```json
"appSettings": [
    {
        "name": "APPINSIGHTS_INSTRUMENTATIONKEY",
        "value": "[reference(resourceId('microsoft.insights/components/', variables('appInsightsName')), '2015-05-01').InstrumentationKey]"
    }
]
```

### <a name="hosting-plan"></a>Hostingabonnement

De definitie van het hostingabonnement kan varieert, en een van de volgende opties:
* [Verbruiksabonnement](#consumption) (standaard)
* [Premium-abonnement](#premium) (in Preview-versie)
* [App Service-plan](#app-service-plan)

### <a name="function-app"></a>Function App

De resource voor de functie-app wordt gedefinieerd door middel van een resource van het type **Microsoft.Web/sites** en type **functionapp**:

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Insights/components', variables('appInsightsName'))]"
    ]
```

> [!IMPORTANT]
> Als u een hostingplan expliciet definieert, zou een extra item in de matrix dependsOn nodig zijn: `"[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"`

Instellingen voor deze toepassing moet worden opgenomen in een functie-app:

| Naam van instelling                 | Description                                                                               | Voorbeeldwaarden                        |
|------------------------------|-------------------------------------------------------------------------------------------|---------------------------------------|
| AzureWebJobsStorage          | Een verbindingsreeks naar een storage-account dat de Functions-runtime voor de interne wachtrij | Zie [Storage-account](#storage)       |
| FUNCTIONS_EXTENSION_VERSION  | De versie van de Azure Functions-runtime                                                | `~2`                                  |
| FUNCTIONS_WORKER_RUNTIME     | De stack taal moet worden gebruikt voor functies in deze app                                   | `dotnet`, `node`, `java`, of `python` |
| WEBSITE_NODE_DEFAULT_VERSION | Alleen nodig als u met behulp van de `node` taal stack, geeft u de versie te gebruiken              | `10.14.1`                             |

Deze eigenschappen zijn opgegeven in de `appSettings` verzameling in de `siteConfig` eigenschap:

```json
"properties": {
    "siteConfig": {
        "appSettings": [
            {
                "name": "AzureWebJobsStorage",
                "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
            },
            {
                "name": "FUNCTIONS_WORKER_RUNTIME",
                "value": "node"
            },
            {
                "name": "WEBSITE_NODE_DEFAULT_VERSION",
                "value": "10.14.1"
            },
            {
                "name": "FUNCTIONS_EXTENSION_VERSION",
                "value": "~2"
            }
        ]
    }
}
```

<a name="consumption"></a>

## <a name="deploy-on-consumption-plan"></a>Implementeren op verbruiksabonnement

Het verbruiksabonnement rekencapaciteit automatisch toegewezen wanneer uw code wordt uitgevoerd, wordt uitgeschaald naar behoefte om belasting te verwerken en vervolgens schaalt omlaag wanneer de code wordt niet uitgevoerd. U hoeft te betalen voor niet-actieve virtuele machines en u hoeft te voren capaciteit reserveren. Zie voor meer informatie, [Azure Functions-schaal en hosting](functions-scale.md#consumption-plan).

Zie voor een voorbeeld van Azure Resource Manager-sjabloon, [functie-app op verbruiksabonnement].

### <a name="create-a-consumption-plan"></a>Maken van een verbruiksabonnement

Een verbruiksabonnement hoeft niet te worden gedefinieerd. Een zal automatisch worden gemaakt of op basis van de per regio worden geselecteerd wanneer u de functie-app-resource zelf maakt.

Het verbruiksabonnement is een speciaal type "serverfarm" resource. Voor Windows, kunt u deze opgeven met behulp van de `Dynamic` waarde voor de `computeMode` en `sku` eigenschappen:

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

> [!NOTE]
> Het verbruiksabonnement kan niet expliciet worden gedefinieerd voor Linux. Er wordt automatisch gemaakt.

Als u uw abonnement consumption expliciet definieert, moet u om in te stellen de `serverFarmId` eigenschap van de app, zodat deze naar de resource-ID van het plan verwijst. U moet ervoor zorgen dat de functie-app heeft een `dependsOn` instellen voor het abonnement ook.

### <a name="create-a-function-app"></a>Een functie-app maken

#### <a name="windows"></a>Windows

Voor Windows, een verbruiksabonnement is vereist twee extra instellingen in de siteconfiguratie: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` en `WEBSITE_CONTENTSHARE`. Deze eigenschappen configureren de storage-account en het bestandspad waarin de functie-app-code en de configuratie zijn opgeslagen.

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "siteConfig": {
            "appSettings": [
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
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        }
    }
}
```

#### <a name="linux"></a>Linux

Op Linux, de functie-app hebben de `kind` ingesteld op `functionapp,linux`, en moet de `reserved` eigenschap ingesteld op `true`:

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp,linux",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountName'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        },
        "reserved": true
    }
}
```



<a name="premium"></a>

## <a name="deploy-on-premium-plan"></a>Implementeren op Premium-abonnement

Het Premium-abonnement biedt dezelfde schaal als het abonnement consumption maar bevat toegewezen resources en aanvullende mogelijkheden. Zie voor meer informatie, [Azure Functions Premium plannen (Preview)](./functions-premium-plan.md).

### <a name="create-a-premium-plan"></a>Een Premium-abonnement maken

Een Premium-abonnement is een speciaal type "serverfarm" resource. Kunt u deze opgeven met behulp van `EP1`, `EP2`, of `EP3` voor de `sku` eigenschapswaarde.

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "sku": "EP1"
    }
}
```

### <a name="create-a-function-app"></a>Een functie-app maken

Een functie-app op een Premium-abonnement moet hebben de `serverFarmId` eigenschap is ingesteld op de resource-ID van het abonnement eerder hebt gemaakt. Daarnaast heeft een Premium-abonnement nodig voor twee extra instellingen in de siteconfiguratie: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` en `WEBSITE_CONTENTSHARE`. Deze eigenschappen configureren de storage-account en het bestandspad waarin de functie-app-code en de configuratie zijn opgeslagen.

```json
{
    "apiVersion": "2016-03-01",
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
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        }
    }
}
```


<a name="app-service-plan"></a> 

## <a name="deploy-on-app-service-plan"></a>Implementeren in App Service-plan

In de App Service-plan is de functie-app wordt uitgevoerd op toegewezen virtuele machines op basis, standaard en Premium-SKU's, vergelijkbaar met de web-apps. Zie voor meer informatie over de werking van de App Service-plan de [gedetailleerd overzicht van Azure App Service-plannen](../app-service/overview-hosting-plans.md).

Zie voor een voorbeeld van Azure Resource Manager-sjabloon, [functie-app in Azure App Service-plan].

### <a name="create-an-app-service-plan"></a>Een App Service-plan maken

Een App Service-plan wordt gedefinieerd door een 'serverfarm'-resource.

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

Als u wilt uitvoeren van uw app op Linux, moet u ook instellen de `kind` naar `Linux`:

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "kind": "Linux",
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

Een functie-app op een App Service-plan moet hebben de `serverFarmId` eigenschap is ingesteld op de resource-ID van het abonnement eerder hebt gemaakt.

```json
{
    "apiVersion": "2016-03-01",
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
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        }
    }
}
```

Linux-apps moeten ook een `linuxFxVersion` eigenschap onder `siteConfig`. Als u alleen code implementeert, wordt de waarde voor dit wordt bepaald door de gewenste runtimestack:

| Stack            | Voorbeeldwaarde                                         |
|------------------|-------------------------------------------------------|
| Python (Preview) | `DOCKER|microsoft/azure-functions-python3.6:2.0`      |
| Javascript       | `DOCKER|microsoft/azure-functions-node8:2.0`          |
| .NET             | `DOCKER|microsoft/azure-functions-dotnet-core2.0:2.0` |

```json
{
    "apiVersion": "2016-03-01",
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
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ],
            "linuxFxVersion": "DOCKER|microsoft/azure-functions-node8:2.0"
        }
    }
}
```

Als u [implementeren van een aangepaste containerinstallatiekopie](./functions-create-function-linux-custom-image.md), moet u opgeven met `linuxFxVersion` en met de configuratie waarmee u uw installatiekopie worden opgehaald, in [Web App for Containers](/azure/app-service/containers). Stel ook `WEBSITES_ENABLE_APP_SERVICE_STORAGE` naar `false`, omdat de inhoud van uw app is opgegeven in de container zelf:

```json
{
    "apiVersion": "2016-03-01",
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
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                },
                {
                    "name": "DOCKER_REGISTRY_SERVER_URL",
                    "value": "[parameters('dockerRegistryUrl')]"
                },
                {
                    "name": "DOCKER_REGISTRY_SERVER_USERNAME",
                    "value": "[parameters('dockerRegistryUsername')]"
                },
                {
                    "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
                    "value": "[parameters('dockerRegistryPassword')]"
                },
                {
                    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
                    "value": "false"
                }
            ],
            "linuxFxVersion": "DOCKER|myacr.azurecr.io/myimage:mytag"
        }
    }
}
```

## <a name="customizing-a-deployment"></a>Een implementatie aanpassen

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
            {
                "name": "FUNCTIONS_EXTENSION_VERSION",
                "value": "~2"
            },
            {
                "name": "Project",
                "value": "src"
            }
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
          "AzureWebJobsDashboard": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]",
          "FUNCTIONS_EXTENSION_VERSION": "~2",
          "FUNCTIONS_WORKER_RUNTIME": "dotnet",
          "Project": "src"
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
[![Deploy to Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

Hier volgt een voorbeeld waarin HTML wordt gebruikt:

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"></a>
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het ontwikkelen en Azure Functions configureert.

* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)
* [Azure-functie-app-instellingen configureren](functions-how-to-use-azure-function-app-settings.md)
* [Uw eerste Azure-functie maken](functions-create-first-azure-function.md)

<!-- LINKS -->

[Functie-app op verbruiksabonnement]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[Functie-app in Azure App Service-plan]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json
