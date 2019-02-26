---
title: Automatiseren met PowerShell Azure Application Insights | Microsoft Docs
description: Automatiseer het maken van het tests voor de resource, waarschuwing en beschikbaarheid in PowerShell met een Azure Resource Manager-sjabloon.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 9f73b87f-be63-4847-88c8-368543acad8b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/02/2017
ms.author: mbullwin
ms.openlocfilehash: 74da56b5e90512f8b903d5a62f7dde4e903560b8
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56817861"
---
#  <a name="create-application-insights-resources-using-powershell"></a>Application Insights-resources maken met PowerShell
Dit artikel leest u hoe u voor het automatiseren van het maken en bijwerken van de [Application Insights](../../azure-monitor/app/app-insights-overview.md) resources automatisch met behulp van Azure Resource Management. U kunt bijvoorbeeld doen als onderdeel van een buildproces. Samen met de basic Application Insights-resource, kunt u [webtests voor beschikbaarheid](../../azure-monitor/app/monitor-web-app-availability.md)Stel [waarschuwingen](../../azure-monitor/app/alerts.md), stel de [prijzen schema](pricing.md), en andere Azure-resources maken .

De sleutel voor het maken van deze resources is JSON-sjablonen voor [Azure Resource Manager](../../azure-resource-manager/manage-resources-powershell.md). Kortom, de procedure is: downloaden van de JSON-definities van bestaande resources. bepaalde waarden, zoals namen; parameteriseren en voer vervolgens de sjabloon wanneer u wilt maken van een nieuwe resource. Kunt u verschillende bronnen samen verpakken, om ze te maken in een Ga - bijvoorbeeld een app bewaken met betrekking tot beschikbaarheidstests, waarschuwingen en opslag voor continue export. Er zijn enkele finesses tot een aantal van de parameteriseringen, wordt hier uitgelegd.

## <a name="one-time-setup"></a>Eenmalige installatie
Als u PowerShell met uw Azure-abonnement voordat u dit nog niet hebt gebruikt:

De Azure Powershell-module installeren op de computer waar u de scripts uit te voeren:

1. Installeer [Microsoft Web Platform Installer (versie 5 of hoger)](https://www.microsoft.com/web/downloads/platform.aspx).
2. Gebruik het Microsoft Azure Powershell installeren.

## <a name="create-an-azure-resource-manager-template"></a>Een Azure Resource Manager-sjabloon maken
Maak een nieuw .json-bestand - noemen we deze `template1.json` in dit voorbeeld. Kopieer deze inhoud naar het:

```JSON
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "appName": {
                "type": "string",
                "metadata": {
                    "description": "Enter the application name."
                }
            },
            "appType": {
                "type": "string",
                "defaultValue": "web",
                "allowedValues": [
                    "web",
                    "java",
                    "other"
                ],
                "metadata": {
                    "description": "Enter the application type."
                }
            },
            "appLocation": {
                "type": "string",
                "defaultValue": "East US",
                "allowedValues": [
                    "South Central US",
                    "West Europe",
                    "East US",
                    "North Europe"
                ],
                "metadata": {
                    "description": "Enter the application location."
                }
            },
            "priceCode": {
                "type": "int",
                "defaultValue": 1,
                "allowedValues": [
                    1,
                    2
                ],
                "metadata": {
                    "description": "1 = Basic, 2 = Enterprise"
                }
            },
            "dailyQuota": {
                "type": "int",
                "defaultValue": 100,
                "minValue": 1,
                "metadata": {
                    "description": "Enter daily quota in GB."
                }
            },
            "dailyQuotaResetTime": {
                "type": "int",
                "defaultValue": 24,
                "metadata": {
                    "description": "Enter daily quota reset hour in UTC (0 to 23). Values outside the range will get a random reset hour."
                }
            },
            "warningThreshold": {
                "type": "int",
                "defaultValue": 90,
                "minValue": 1,
                "maxValue": 100,
                "metadata": {
                    "description": "Enter the % value of daily quota after which warning mail to be sent. "
                }
            }
        },
        "variables": {
            "priceArray": [
                "Basic",
                "Application Insights Enterprise"
            ],
            "pricePlan": "[take(variables('priceArray'),parameters('priceCode'))]",
            "billingplan": "[concat(parameters('appName'),'/', variables('pricePlan')[0])]"
        },
        "resources": [
            {
                "type": "microsoft.insights/components",
                "kind": "[parameters('appType')]",
                "name": "[parameters('appName')]",
                "apiVersion": "2014-04-01",
                "location": "[parameters('appLocation')]",
                "tags": {},
                "properties": {
                    "ApplicationId": "[parameters('appName')]"
                },
                "dependsOn": []
            },
            {
                "name": "[variables('billingplan')]",
                "type": "microsoft.insights/components/CurrentBillingFeatures",
                "location": "[parameters('appLocation')]",
                "apiVersion": "2015-05-01",
                "dependsOn": [
                    "[resourceId('microsoft.insights/components', parameters('appName'))]"
                ],
                "properties": {
                    "CurrentBillingFeatures": "[variables('pricePlan')]",
                    "DataVolumeCap": {
                        "Cap": "[parameters('dailyQuota')]",
                        "WarningThreshold": "[parameters('warningThreshold')]",
                        "ResetTime": "[parameters('dailyQuotaResetTime')]"
                    }
                }
            }
        ]
    }
```



## <a name="create-application-insights-resources"></a>Application Insights-resources maken
1. In PowerShell, moet u zich aanmelden bij Azure:
   
    `Connect-AzureRmAccount`
2. Voer een opdracht als volgt uit:
   
    ```PS
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -appName myNewApp

    ``` 
   
   * `-ResourceGroupName` is de groep waar u wilt maken van de nieuwe resources.
   * `-TemplateFile` voordat u de aangepaste parameters moet plaatsvinden.
   * `-appName` De naam van de resource te maken.

U kunt andere parameters toevoegen - bijbehorende beschrijvingen vindt u in de parametersectie van de sjabloon.

## <a name="to-get-the-instrumentation-key"></a>De instrumentatiesleutel ophalen
Na het maken van de bron van een toepassing, moet u de instrumentatiesleutel: 

```PS
    $resource = Find-AzureRmResource -ResourceNameEquals "<YOUR APP NAME>" -ResourceType "Microsoft.Insights/components"
    $details = Get-AzureRmResource -ResourceId $resource.ResourceId
    $ikey = $details.Properties.InstrumentationKey
```


<a id="price"></a>
## <a name="set-the-price-plan"></a>De prijs-planning instellen

U kunt instellen dat de [prijsplan](pricing.md).

Een resource-app maken met het Enterprise prijs-plan met de bovenstaande sjabloon:

```PS
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -priceCode 2 `
               -appName myNewApp
```

|priceCode|plan|
|---|---|
|1|Basic|
|2|Enterprise|

* Als u wilt dat alleen gebruik van de prijscategorie Basic standaard, kunt u de CurrentBillingFeatures-resource in de sjabloon weglaten.
* Als u het prijsplan is gewijzigd wilt nadat de onderdeel-resource is gemaakt, kunt u een sjabloon die de resource 'microsoft.insights/components' wordt weggelaten. Laat ook de `dependsOn` knooppunt van de facturering-resource. 

Kijken om te controleren of de bijgewerkte prijs-abonnement, de **pagina gebruik en geschatte kosten** blade in de browser. **Vernieuw de browserweergave van de** om ervoor te zorgen dat u de meest recente status zien.



## <a name="add-a-metric-alert"></a>Een metrische waarschuwing toevoegen

Als u een waarschuwing voor metrische gegevens instelt op hetzelfde moment als uw app-resource, het sjabloonbestand samenvoegen code zoals deze:

```JSON
{
    parameters: { ... // existing parameters ...
            "responseTime": {
              "type": "int",
              "defaultValue": 3,
              "minValue": 1,
              "metadata": {
                "description": "Enter response time threshold in seconds."
              }
    },
    variables: { ... // existing variables ...
      // Alert names must be unique within resource group.
      "responseAlertName": "[concat('ResponseTime-', toLower(parameters('appName')))]"
    }, 
    resources: { ... // existing resources ...
     {
      //
      // Metric alert on response time
      //
      "name": "[variables('responseAlertName')]",
      "type": "Microsoft.Insights/alertrules",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]",
      // Ensure this resource is created after the app resource:
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('responseAlertName')]",
        "description": "response time alert",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/components', parameters('appName'))]",
            "metricName": "request.duration"
          },
          "threshold": "[parameters('responseTime')]", //seconds
          "windowSize": "PT15M" // Take action if changed state for 15 minutes
        },
        "actions": [
          {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
            "sendToServiceOwners": true,
            "customEmails": []
          }
        ]
      }
    }
}
```

Als u de sjabloon aanroept, kunt u deze parameter optioneel toevoegen:

    `-responseTime 2`

U kunt natuurlijk parameter van andere velden. 

Als u wilt weten de namen en configuratie-informatie van andere regels voor waarschuwingen, maakt u een regel handmatig uit en Controleer in [Azure Resource Manager](https://resources.azure.com/). 


## <a name="add-an-availability-test"></a>Een beschikbaarheidstest toevoegen

In dit voorbeeld is voor een ping-test (op één pagina testen).  

**Er zijn twee onderdelen** in een beschikbaarheidstest: de test zelf, en de waarschuwing die op de hoogte gebracht van fouten.

Samenvoegen met de volgende code in de sjabloonbestand dat de app maakt.

```JSON
{
    parameters: { ... // existing parameters here ...
      "pingURL": { "type": "string" },
      "pingText": { "type": "string" , defaultValue: ""}
    },
    variables: { ... // existing variables here ...
      "pingTestName":"[concat('PingTest-', toLower(parameters('appName')))]",
      "pingAlertRuleName": "[concat('PingAlert-', toLower(parameters('appName')), '-', subscription().subscriptionId)]"
    },
    resources: { ... // existing resources here ...
    { //
      // Availability test: part 1 configures the test
      //
      "name": "[variables('pingTestName')]",
      "type": "Microsoft.Insights/webtests",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]",
      // Ensure this is created after the app resource:
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "Name": "[variables('pingTestName')]",
        "Description": "Basic ping test",
        "Enabled": true,
        "Frequency": 900, // 15 minutes
        "Timeout": 120, // 2 minutes
        "Kind": "ping", // single URL test
        "RetryEnabled": true,
        "Locations": [
          {
            "Id": "us-va-ash-azr"
          },
          {
            "Id": "emea-nl-ams-azr"
          },
          {
            "Id": "apac-jp-kaw-edge"
          }
        ],
        "Configuration": {
          "WebTest": "[concat('<WebTest   Name=\"', variables('pingTestName'), '\"   Enabled=\"True\"         CssProjectStructure=\"\"    CssIteration=\"\"  Timeout=\"120\"  WorkItemIds=\"\"         xmlns=\"http://microsoft.com/schemas/VisualStudio/TeamTest/2010\"         Description=\"\"  CredentialUserName=\"\"  CredentialPassword=\"\"         PreAuthenticate=\"True\"  Proxy=\"default\"  StopOnError=\"False\"         RecordedResultFile=\"\"  ResultsLocale=\"\">  <Items>  <Request Method=\"GET\"    Version=\"1.1\"  Url=\"', parameters('Url'),   '\" ThinkTime=\"0\"  Timeout=\"300\" ParseDependentRequests=\"True\"         FollowRedirects=\"True\" RecordResult=\"True\" Cache=\"False\"         ResponseTimeGoal=\"0\"  Encoding=\"utf-8\"  ExpectedHttpStatusCode=\"200\"         ExpectedResponseUrl=\"\" ReportingName=\"\" IgnoreHttpStatusCode=\"False\" />        </Items>  <ValidationRules> <ValidationRule  Classname=\"Microsoft.VisualStudio.TestTools.WebTesting.Rules.ValidationRuleFindText, Microsoft.VisualStudio.QualityTools.WebTestFramework, Version=10.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a\" DisplayName=\"Find Text\"         Description=\"Verifies the existence of the specified text in the response.\"         Level=\"High\"  ExecutionOrder=\"BeforeDependents\">  <RuleParameters>        <RuleParameter Name=\"FindText\" Value=\"',   parameters('pingText'), '\" />  <RuleParameter Name=\"IgnoreCase\" Value=\"False\" />  <RuleParameter Name=\"UseRegularExpression\" Value=\"False\" />  <RuleParameter Name=\"PassIfTextFound\" Value=\"True\" />  </RuleParameters> </ValidationRule>  </ValidationRules>  </WebTest>')]"
        },
        "SyntheticMonitorId": "[variables('pingTestName')]"
      }
    },

    {
      //
      // Availability test: part 2, the alert rule
      //
      "name": "[variables('pingAlertRuleName')]",
      "type": "Microsoft.Insights/alertrules",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]", 
      "dependsOn": [
        "[resourceId('Microsoft.Insights/webtests', variables('pingTestName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource",
        "[concat('hidden-link:', resourceId('Microsoft.Insights/webtests', variables('pingTestName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('pingAlertRuleName')]",
        "description": "alert for web test",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.LocationThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.LocationThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/webtests', variables('pingTestName'))]",
            "metricName": "GSMT_AvRaW"
          },
          "windowSize": "PT15M", // Take action if changed state for 15 minutes
          "failedLocationCount": 2
        },
        "actions": [
          {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
            "sendToServiceOwners": true,
            "customEmails": []
          }
        ]
      }
    }
}
```

Voor het detecteren van de codes voor andere testlocaties, of voor het automatiseren van het maken van meer complexe webtests, een voorbeeld van het handmatig maken en vervolgens voorzien van de code van de [Azure Resource Manager](https://resources.azure.com/).

## <a name="add-more-resources"></a>Meer resources toevoegen

Voor het automatiseren van het maken van een andere bron van welke aard dan ook, een voorbeeld van het handmatig maken en kopieert en voorzien van de code van [Azure Resource Manager](https://resources.azure.com/). 

1. Open [Azure Resource Manager](https://resources.azure.com/). Navigeren naar beneden `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, aan de bron van uw toepassing. 
   
    ![Navigatie in Azure Resource Explorer](./media/powershell/01.png)
   
    *Onderdelen* zijn de basic Application Insights-resources voor het weergeven van toepassingen. Er zijn afzonderlijke resources voor de gekoppelde regels voor waarschuwingen en webtests voor beschikbaarheid.
2. Kopieer de JSON van het onderdeel naar de juiste plaats in `template1.json`.
3. Verwijder deze eigenschappen:
   
   * `id`
   * `InstrumentationKey`
   * `CreationDate`
   * `TenantId`
4. Open de secties webtests en alertrules en kopieert u de JSON voor afzonderlijke items in de sjabloon. (Niet kopiëren van de knooppunten webtests of alertrules: Ga naar de items eronder.)
   
    Elke WebTest heeft een bijbehorende waarschuwingsregel, dus u moet het kopiëren van beide.
   
    U kunt ook waarschuwingen over metrische gegevens opnemen. [Metrische namen](powershell-alerts.md#metric-names).
5. Deze regel invoegen in elke resource:
   
    `"apiVersion": "2015-05-01",`

### <a name="parameterize-the-template"></a>De sjabloon parameteriseren
U hebt nu vervangen door de namen van de specifieke parameters. Naar [voorzien van een sjabloon](../../azure-resource-manager/resource-group-authoring-templates.md), schrijft u expressies met een [set hulpfuncties](../../azure-resource-manager/resource-group-template-functions.md). 

U kunt geen parameteriseren slechts een deel van een tekenreeks, Gebruik daarom `concat()` tekenreeksen bouwen.

Hier vindt u voorbeelden van de vervangingen die moet u maken. Er zijn meerdere exemplaren van elke vervanging. Mogelijk moet u anderen in uw sjabloon. Deze voorbeelden gebruiken de parameters en variabelen die we hebben gedefinieerd aan de bovenkant van de sjabloon.

| find | vervangen |
| --- | --- |
| `"hidden-link:/subscriptions/.../../components/MyAppName"` |`"[concat('hidden-link:',`<br/>` resourceId('microsoft.insights/components',` <br/> ` parameters('appName')))]"` |
| `"/subscriptions/.../../alertrules/myAlertName-myAppName-subsId",` |`"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",` |
| `"/subscriptions/.../../webtests/myTestName-myAppName",` |`"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",` |
| `"myWebTest-myAppName"` |`"[variables(testName)]"'` |
| `"myTestName-myAppName-subsId"` |`"[variables('alertRuleName')]"` |
| `"myAppName"` |`"[parameters('appName')]"` |
| `"myappname"` (kleine letters) |`"[toLower(parameters('appName'))]"` |
| `"<WebTest Name=\"myWebTest\" ...`<br/>` Url=\"http://fabrikam.com/home\" ...>"` |`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]"`<br/>Verwijderen van de Guid en -id. |

### <a name="set-dependencies-between-the-resources"></a>Set-afhankelijkheden tussen resources
Azure, moet de resources in een strikte volgorde instellen. Als u wilt controleren of een installatie is voltooid voordat de volgende begint, moet u afhankelijkheid regels toevoegen:

* In de beschikbaarheid van test-resource:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`
* In de resource van de waarschuwing voor een beschikbaarheidstest:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`



## <a name="next-steps"></a>Volgende stappen
Andere automation-artikelen:

* [Maak een Application Insights-resource](powershell-script-create-resource.md) -snelle methode zonder een sjabloon te gebruiken.
* [Waarschuwingen instellen](powershell-alerts.md)
* [Webtests maken](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [Diagnostische Azure-gegevens verzenden naar Application Insights](powershell-azure-diagnostics.md)
* [In Azure implementeren vanuit GitHub](https://blogs.msdn.com/b/webdev/archive/2015/09/16/deploy-to-azure-from-github-with-application-insights.aspx)
* [Release-aantekeningen maken](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)

