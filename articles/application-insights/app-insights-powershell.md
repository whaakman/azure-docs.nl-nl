---
title: Azure Application Insights met PowerShell automatiseren | Microsoft Docs
description: Automatiseren maken resource, waarschuwing en beschikbaarheid tests in PowerShell met een Azure Resource Manager-sjabloon.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 9f73b87f-be63-4847-88c8-368543acad8b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: mbullwin
ms.openlocfilehash: f4f9d1558d2ef9dc5e1b7b248ad5bc8753f59cf9
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
#  <a name="create-application-insights-resources-using-powershell"></a>Application Insights-resources maken met PowerShell
Dit artikel laat zien hoe te automatiseren voor het maken en bijwerken van de [Application Insights](app-insights-overview.md) resources automatisch met behulp van Azure Resource Management. U kunt bijvoorbeeld doen als onderdeel van een buildproces. Samen met de basic Application Insights-resource, kunt u [webtests voor beschikbaarheid](app-insights-monitor-web-app-availability.md)Stel [waarschuwingen](app-insights-alerts.md)stelt de [prijzen schema](app-insights-pricing.md), en andere Azure-resources te maken .

De sleutel voor het maken van deze resources is een JSON-sjablonen voor [Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md). Kortom, de procedure is: downloaden van de JSON-definities van bestaande resources; parameter van bepaalde waarden zoals namen; en voer vervolgens de sjabloon wanneer u wilt maken van een nieuwe resource. Kunt u verschillende resources samen verpakken, om ze te maken in een gaan - bijvoorbeeld een app-monitor met beschikbaarheidstests, waarschuwingen en opslag voor continue export. Er zijn enkele eigenaardigheden tot een aantal van de parameteriseringen, wordt hier uitgelegd.

## <a name="one-time-setup"></a>Eenmalige instelling
Als u dit nog niet hebt PowerShell met uw Azure-abonnement voordat gebruikt:

De Azure Powershell-module installeren op de computer waar u de scripts worden uitgevoerd:

1. Installeer [Microsoft Web Platform Installer (v5 of hoger)](http://www.microsoft.com/web/downloads/platform.aspx).
2. Gebruik dit voor het installeren van Microsoft Azure Powershell.

## <a name="create-an-azure-resource-manager-template"></a>Een Azure Resource Manager-sjabloon maken
Maak een nieuw .json-bestand - gaan we deze aanroepen `template1.json` in dit voorbeeld. Kopieer deze inhoud naar het:

```JSON
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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
                    "HockeyAppBridge",
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
   
    `Login-AzureRmAccount`
2. Voer een opdracht als volgt uit:
   
    ```PS
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -appName myNewApp

    ``` 
   
   * `-ResourceGroupName`is de groep waar u wilt maken van de nieuwe resources.
   * `-TemplateFile`Er moeten plaatsvinden voordat de aangepaste parameters.
   * `-appName`De naam van de resource te maken.

U kunt andere parameters toevoegen - bijbehorende beschrijvingen vindt in het gedeelte parameters van de sjabloon.

## <a name="to-get-the-instrumentation-key"></a>De instrumentatiesleutel ophalen
Na het maken van de bron van een toepassing, moet u de instrumentatiesleutel: 

```PS
    $resource = Find-AzureRmResource -ResourceNameEquals "<YOUR APP NAME>" -ResourceType "Microsoft.Insights/components"
    $details = Get-AzureRmResource -ResourceId $resource.ResourceId
    $ikey = $details.Properties.InstrumentationKey
```


<a id="price"></a>
## <a name="set-the-price-plan"></a>De prijs planning instellen

U kunt instellen de [prijs plan](app-insights-pricing.md).

Een resource-app maken met het Enterprise prijs plan, met de bovenstaande sjabloon:

```PS
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -priceCode 2 `
               -appName myNewApp
```

|priceCode|Plannen|
|---|---|
|1|Basic|
|2|Enterprise|

* Als u alleen het standaardabonnement basisprijs gebruiken wilt, kunt u de resource CurrentBillingFeatures van de sjabloon weglaten.
* Als u het plan prijs wijzigen wilt nadat de onderdeel-resource is gemaakt, kunt u een sjabloon die de resource 'microsoft.insights/components' wordt weggelaten. Laat ook de `dependsOn` knooppunt uit de facturering resource. 

Kijken om te controleren of het plan bijgewerkte prijs, de 'Functies + prijzen' blade in de browser. **Vernieuw de browserweergave** om ervoor te zorgen dat u de meest recente toestand zien.



## <a name="add-a-metric-alert"></a>Waarschuwing voor een metrische toevoegen

Als u een waarschuwing voor metrische instelt op hetzelfde moment als uw app-resource, het sjabloonbestand samenvoegen code als volgt:

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

Als u de sjabloon aanroept, kunt u eventueel deze parameter toevoegen:

    `-responseTime 2`

U kunt uiteraard parameter van andere velden. 

Voor meer informatie over de namen en configuratie-informatie van andere regels voor waarschuwingen, maakt u handmatig een regel en vervolgens controleren in [Azure Resource Manager](https://resources.azure.com/). 


## <a name="add-an-availability-test"></a>Een beschikbaarheidstest toevoegen

In dit voorbeeld is voor een ping-test (voor het testen van één pagina).  

**Er zijn twee delen** in een beschikbaarheidstest: de test zelf en de waarschuwing die een melding van fouten.

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
          "WebTest": "[concat('<WebTest   Name=\"', variables('pingTestName'), '\"   Enabled=\"True\"         CssProjectStructure=\"\"    CssIteration=\"\"  Timeout=\"120\"  WorkItemIds=\"\"         xmlns=\"http://microsoft.com/schemas/VisualStudio/TeamTest/2010\"         Description=\"\"  CredentialUserName=\"\"  CredentialPassword=\"\"         PreAuthenticate=\"True\"  Proxy=\"default\"  StopOnError=\"False\"         RecordedResultFile=\"\"  ResultsLocale=\"\">  <Items>  <Request Method=\"GET\"    Version=\"1.1\"  Url=\"', parameters('Url'),   '\" ThinkTime=\"0\"  Timeout=\"300\" ParseDependentRequests=\"True\"         FollowRedirects=\"True\" RecordResult=\"True\" Cache=\"False\"         ResponseTimeGoal=\"0\"  Encoding=\"utf-8\"  ExpectedHttpStatusCode=\"200\"         ExpectedResponseUrl=\"\" ReportingName=\"\" IgnoreHttpStatusCode=\"False\" />        </Items>  <ValidationRules> <ValidationRule  Classname=\"Microsoft.VisualStudio.TestTools.WebTesting.Rules.ValidationRuleFindText, Microsoft.VisualStudio.QualityTools.WebTestFramework, Version=10.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a\" DisplayName=\"Find Text\"         Description=\"Verifies the existence of the specified text in the response.\"         Level=\"High\"  ExectuionOrder=\"BeforeDependents\">  <RuleParameters>        <RuleParameter Name=\"FindText\" Value=\"',   parameters('pingText'), '\" />  <RuleParameter Name=\"IgnoreCase\" Value=\"False\" />  <RuleParameter Name=\"UseRegularExpression\" Value=\"False\" />  <RuleParameter Name=\"PassIfTextFound\" Value=\"True\" />  </RuleParameters> </ValidationRule>  </ValidationRules>  </WebTest>')]"
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

Voor het detecteren van de codes voor andere testlocaties of het maken van een complexere webtests automatiseren handmatig maken van een voorbeeld en vervolgens de code van de parameter [Azure Resource Manager](https://resources.azure.com/).

## <a name="add-more-resources"></a>Voeg meer resources toe

Voor het automatiseren van het maken van een andere bron enige vorm van een voorbeeld handmatig maken en kopieert en voorzien van de code van [Azure Resource Manager](https://resources.azure.com/). 

1. Open [Azure Resource Manager](https://resources.azure.com/). Navigeren naar beneden `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, de bron van de toepassing. 
   
    ![Navigatie in de Azure-Resource Explorer](./media/app-insights-powershell/01.png)
   
    *Onderdelen* zijn de basic Application Insights-resources voor het weergeven van toepassingen. Er zijn afzonderlijke bronnen voor de gekoppelde waarschuwingsregels en webtests voor beschikbaarheid.
2. De JSON van het onderdeel kopiëren naar de juiste plaats in `template1.json`.
3. Verwijder deze eigenschappen:
   
   * `id`
   * `InstrumentationKey`
   * `CreationDate`
   * `TenantId`
4. Open de secties webtests en alertrules en kopieert u de JSON voor afzonderlijke items in de sjabloon. (Van de knooppunten webtests of alertrules niet kopiëren: Ga naar de items eronder.)
   
    Elke WebTest heeft een bijbehorende waarschuwingsregel, zodat u hoeft te kopiëren van beide.
   
    U kunt ook waarschuwingen op metrische gegevens opnemen. [De namen van de metrische](app-insights-powershell-alerts.md#metric-names).
5. Deze regel invoegen in elke resource:
   
    `"apiVersion": "2015-05-01",`

### <a name="parameterize-the-template"></a>Parameter van de sjabloon
U hebt nu vervangen door de namen van de specifieke parameters. Naar [voorzien van een sjabloon](../azure-resource-manager/resource-group-authoring-templates.md), schrijven van expressies met een [reeks hulpfuncties](../azure-resource-manager/resource-group-template-functions.md). 

Kunnen niet worden voorzien van slechts een deel van een tekenreeks, dus gebruik `concat()` om tekenreeksen samen te stellen.

Hier volgen enkele voorbeelden van de die u wilt maken. Er zijn meerdere exemplaren van elke vervanging. Mogelijk moet u anderen in uw sjabloon. Deze voorbeelden worden de parameters en variabelen die we gedefinieerd aan de bovenkant van de sjabloon gebruiken.

| zoeken | vervangen |
| --- | --- |
| `"hidden-link:/subscriptions/.../components/MyAppName"` |`"[concat('hidden-link:',`<br/>` resourceId('microsoft.insights/components',` <br/> ` parameters('appName')))]"` |
| `"/subscriptions/.../alertrules/myAlertName-myAppName-subsId",` |`"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",` |
| `"/subscriptions/.../webtests/myTestName-myAppName",` |`"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",` |
| `"myWebTest-myAppName"` |`"[variables(testName)]"'` |
| `"myTestName-myAppName-subsId"` |`"[variables('alertRuleName')]"` |
| `"myAppName"` |`"[parameters('appName')]"` |
| `"myappname"`(kleine letters) |`"[toLower(parameters('appName'))]"` |
| `"<WebTest Name=\"myWebTest\" ...`<br/>` Url=\"http://fabrikam.com/home\" ...>"` |`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]"`<br/>Verwijderen van de Guid en -id. |

### <a name="set-dependencies-between-the-resources"></a>Set afhankelijkheden tussen resources
Azure moet de resources in strikte volgorde instellen. Om te zorgen dat een installatie is voltooid voordat de volgende begint, moet u afhankelijkheid regels toevoegen:

* Test in de beschikbaarheid van de resource:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`
* In de waarschuwing voor een beschikbaarheidstest:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`



## <a name="next-steps"></a>Volgende stappen
Andere automation-artikelen:

* [Een Application Insights-resource maken](app-insights-powershell-script-create-resource.md) -snelle methode zonder een sjabloon te gebruiken.
* [Waarschuwingen instellen](app-insights-powershell-alerts.md)
* [Webtests maken](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [Diagnostische Azure-gegevens verzenden naar Application Insights](app-insights-powershell-azure-diagnostics.md)
* [Implementeren in Azure vanuit GitHub](http://blogs.msdn.com/b/webdev/archive/2015/09/16/deploy-to-azure-from-github-with-application-insights.aspx)
* [Release aantekeningen maken](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)

