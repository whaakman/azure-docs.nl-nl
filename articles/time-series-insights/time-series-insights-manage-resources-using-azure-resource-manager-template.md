---
title: Het beheren van uw Azure Time Series Insights-omgeving met behulp van Azure Resource Manager-sjablonen | Microsoft Docs
description: Dit artikel wordt beschreven hoe u uw Azure Time Series Insights omgeving programmatisch met behulp van Azure Resource Manager beheert.
services: time-series-insights
ms.service: time-series-insights
author: sandshadow
ms.author: edett
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: article
ms.date: 12/08/2017
ms.openlocfilehash: b09d4a1aea56a4e306f80a1b43d519d313fd73ab
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="create-time-series-insights-resources-using-azure-resource-manager-templates"></a>Time Series Insights-resources met behulp van Azure Resource Manager-sjablonen maken

In dit artikel wordt beschreven hoe maken en implementeren van Time Series Insights-resources met behulp van Azure Resource Manager-sjablonen, PowerShell en de tijd reeks Insights-resourceprovider.

Time Series Insights ondersteunt de volgende bronnen:
   | Resource | Beschrijving |
   | --- | --- |
   | Omgeving | Een omgeving Time Series Insights is een logische groepering van gebeurtenissen die zijn gelezen uit de gebeurtenis beleggingsmakelaars, opgeslagen en beschikbaar gesteld voor query. Zie voor meer informatie [plannen van uw Azure Time Series Insights-omgeving](time-series-insights-environment-planning.md) |
   | Bron van gebeurtenis | De bron van een gebeurtenis is een verbinding met een broker gebeurtenis waaruit Time Series Insights leest en gebeurtenissen opgenomen in de omgeving. Ondersteunde gebeurtenisbronnen zijn IoT Hub en Event Hub. |
   | Referentie-gegevensset | Sets van verwijzingsgegevens bieden metagegevens over de gebeurtenissen in de omgeving. Metagegevens in de sets van verwijzingsgegevens wordt gekoppeld aan gebeurtenissen tijdens inkomend. Sets van verwijzingsgegevens worden gedefinieerd als resources door de sleuteleigenschappen van de gebeurtenis. De metagegevens van de werkelijke waaruit de gegevensset verwijzing is geüpload of gewijzigd via data vlak API's. |
   | Beleid voor toegang | Toegangsbeleid verlenen machtigingen voor gegevensquery's uitgeven, referentiegegevens in de omgeving bewerken en opgeslagen query's en perspectieven die zijn gekoppeld aan de omgeving delen. Zie voor meer informatie [gegevenstoegang verlenen tot een tijd reeks Insights-omgeving met Azure portal](time-series-insights-data-access.md) |

Resource Manager-sjabloon is een JSON-bestand dat de infrastructuur en configuratie van resources in een resourcegroep definieert. Zie de volgende documenten voor meer informatie:

- [Overzicht van Azure Resource Manager - sjabloon voor implementatie](../azure-resource-manager/resource-group-overview.md#template-deployment)
- [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)

De [201-timeseriesinsights-omgeving-met-eventhub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-timeseriesinsights-environment-with-eventhub) Quick Start-sjabloon is gepubliceerd op GitHub. Deze sjabloon maakt een tijd reeks Insights-omgeving, een onderliggende gebeurtenisbron geconfigureerd voor het gebruiken van gebeurtenissen van een Event Hub en toegangsbeleid die toegang tot gegevens van de omgeving verlenen. Als een bestaande Event Hub niet wordt opgegeven, wordt een gemaakt met de implementatie.

## <a name="deploy-the-quickstart-template-locally-using-powershell"></a>De lokaal met behulp van PowerShell Quick Start-sjabloon implementeren

De volgende procedure beschrijft hoe u PowerShell gebruikt voor het implementeren van een Azure Resource Manager-sjabloon die u maakt u een tijd reeks Insights-omgeving, een onderliggende gebeurtenisbron geconfigureerd voor het gebruiken van gebeurtenissen van een Event Hub en toegangsbeleid die toegang tot verlenen de omgevingsgegevens. Als een bestaande Event Hub niet wordt opgegeven, wordt een gemaakt met de implementatie.

De geschatte werkstroom is als volgt:

1. Installeer PowerShell.
1. De sjabloon en een parameterbestand maken.
1. In PowerShell, moet u zich aanmelden bij uw Azure-account.
1. Een nieuwe resourcegroep maken als deze niet bestaat.
1. De implementatie testen.
1. De sjabloon implementeert.

### <a name="install-powershell"></a>PowerShell installeren

Azure PowerShell installeren door de instructies in [aan de slag met Azure PowerShell](/powershell/azure/get-started-azureps).

### <a name="create-a-template"></a>Een sjabloon maken

Kloon of kopieer de [201-timeseriesinsights-omgeving-met-eventhub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-timeseriesinsights-environment-with-eventhub/azuredeploy.json) sjabloon vanuit GitHub.

### <a name="create-a-parameters-file"></a>Maak een parameterbestand

Voor het maken van een parameterbestand, kopieert u de [201-timeseriesinsights-omgeving-met-eventhub](https://github.com/Azure/azure-quickstart-templates/blob/master/201-timeseriesinsights-environment-with-eventhub/azuredeploy.parameters.json) bestand.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "eventHubNamespaceName": {
          "value": "GEN-UNIQUE"
      },
      "eventHubName": {
          "value": "GEN-UNIQUE"
      },
      "consumerGroupName": {
          "value": "GEN-UNIQUE"
      },
      "environmentName": {
        "value": "GEN-UNIQUE"
      },
      "eventSourceName": {
        "value": "GEN-UNIQUE"
      }
  }
}
```

#### <a name="required-parameters"></a>Vereiste Parameters

   | Parameter | Beschrijving |
   | --- | --- |
   | eventHubNamespaceName | De naamruimte van de bron event hub. |
   | eventHubName | De naam van de bron event hub. |
   | consumerGroupName | De naam van de consumergroep die de Time Series Insights-service gebruiken voor het lezen van de gegevens van de event hub. **Opmerking:** om bronconflicten te voorkomen, deze consumergroep moet specifiek zijn voor de Time Series Insights-service en niet gedeeld met andere lezers. |
   | EnvironmentName | De naam van de omgeving. De naam kan niet bevatten: ' <', ' >', '%', '&', ': ','\\','?', '/' en willekeurig control-tekens. Alle andere tekens zijn toegestaan.|
   | eventSourceName | De naam van de gebeurtenis bron onderliggende resource. De naam kan niet bevatten: ' <', ' >', '%', '&', ': ','\\','?', '/' en willekeurig control-tekens. Alle andere tekens zijn toegestaan. |

#### <a name="optional-parameters"></a>Optionele Parameters:

   | Parameter | Beschrijving |
   | --- | --- |
   | existingEventHubResourceId | Een optionele bron-ID van een bestaande Event Hub die wordt verbonden met de omgeving Time Series Insights via de gebeurtenisbron. **Opmerking:** de gebruiker voor het implementeren van de sjabloon moet over bevoegdheden beschikken om de listkeys-bewerking op de Event Hub. Als er geen waarde wordt doorgegeven, wordt een nieuwe event hub wordt gemaakt door de sjabloon. |
   | environmentDisplayName | Een optionele beschrijvende naam in interfaces tooling of gebruiker in plaats van de naam van de omgeving kunnen worden weergegeven. |
   | environmentSkuName | De naam van de sku. Zie voor meer informatie de [pagina met prijzen van Time Series Insights](https://azure.microsoft.com/pricing/details/time-series-insights/).  |
   | environmentSkuCapacity | De capaciteit van de eenheid van de Sku. Zie voor meer informatie de [pagina met prijzen van Time Series Insights](https://azure.microsoft.com/pricing/details/time-series-insights/).|
   | environmentDataRetentionTime | De minimale timespan gebeurtenissen van de omgeving zijn beschikbaar voor query. De waarde moet worden opgegeven in de ISO 8601-notatie, bijvoorbeeld 'P30D' voor een bewaarbeleid van 30 dagen. |
   | eventSourceDisplayName | Een optionele beschrijvende naam in tooling of gebruiker kunnen worden weergegeven in plaats van de naam van de gebeurtenis-interfaces. |
   | eventSourceTimestampPropertyName | De gebeurteniseigenschap die wordt gebruikt als de gebeurtenisbron timestamp. Als een waarde is niet opgegeven voor timestampPropertyName of als null of lege tekenreeks is opgegeven, wordt de gebeurtenis Aanmaaktijd worden gebruikt. |
   | eventSourceKeyName | De naam van de gedeelde toegangssleutel die de Time Series Insights-service gebruikt voor de verbinding naar de event hub. |
   | accessPolicyReaderObjectIds | Een lijst met object-id's van de gebruikers of toepassingen in Azure AD dat lezerstoegang tot de omgeving hebben. De service principal objectId kan worden verkregen door het aanroepen van de **Get-AzureRMADUser** of de **Get-AzureRMADServicePrincipal** cmdlets. Maken van een toegangsbeleid voor Azure AD-groepen is nog niet ondersteund. |
   | accessPolicyContributorObjectIds | Een lijst met object-id's van de gebruikers of toepassingen in Azure AD dat Inzender toegang tot de omgeving hebben. De service principal objectId kan worden verkregen door het aanroepen van de **Get-AzureRMADUser** of de **Get-AzureRMADServicePrincipal** cmdlets. Maken van een toegangsbeleid voor Azure AD-groepen is nog niet ondersteund. |

Als u bijvoorbeeld moet het volgende parameterbestand worden gebruikt voor het maken van een omgeving en een gebeurtenisbron die gebeurtenissen uit een bestaande event hub kan lezen. Dit leidt ook tot twee toegangsbeleid dat Inzender toegang tot de omgeving verlenen.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "eventHubNamespaceName": {
            "value": "tsiTemplateTestNamespace"
        },
        "eventHubName": {
            "value": "tsiTemplateTestEventHub"
        },
        "consumerGroupName": {
            "value": "tsiTemplateTestConsumerGroup"
        },
        "environmentName": {
          "value": "tsiTemplateTestEnvironment"
        },
        "eventSourceName": {
          "value": "tsiTemplateTestEventSource"
        },
        "existingEventHubResourceId": {
          "value": "/subscriptions/{yourSubscription}/resourceGroups/MyDemoRG/providers/Microsoft.EventHub/namespaces/tsiTemplateTestNamespace/eventhubs/tsiTemplateTestEventHub"
        },
        "accessPolicyContributorObjectIds": {
            "value": [
                "AGUID001-0000-0000-0000-000000000000",
                "AGUID002-0000-0000-0000-000000000000"
            ]
        }
    }
  }
```

Zie voor meer informatie de [Parameters](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) artikel.

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Aanmelden bij Azure en het Azure-abonnement instellen

Voer de volgende opdracht vanaf een PowerShell-prompt:

```powershell
Login-AzureRmAccount
```

U wordt gevraagd om aan te melden bij uw Azure-account. Voer de volgende opdracht om de beschikbare abonnementen weer te geven na aanmelding:

```powershell
Get-AzureRMSubscription
```

Deze opdracht retourneert een lijst met beschikbare Azure-abonnementen. Kies een abonnement voor de huidige sessie met de volgende opdracht. Vervang `<YourSubscriptionId>` met de GUID voor de Azure-abonnement u wilt gebruiken:

```powershell
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Instellen van de resourcegroep

Als u nog geen een bestaande resource groep, maakt u een nieuwe resourcegroep met de **New-AzureRmResourceGroup** opdracht. Geef de naam van de resourcegroep en de locatie die u wilt gebruiken. Bijvoorbeeld:

```powershell
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Als dit lukt, wordt een samenvatting van de nieuwe resourcegroep wordt weergegeven.

```powershell
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>De implementatie testen

Valideren van uw implementatie door de `Test-AzureRmResourceGroupDeployment` cmdlet. Bij het testen van de implementatie, Geef parameters op exact dezelfde manier als bij het uitvoeren van de implementatie.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

### <a name="create-the-deployment"></a>De implementatie maken

Als u wilt maken van nieuwe implementatie uitvoeren de `New-AzureRmResourceGroupDeployment` cmdlet, en geef de benodigde parameters op wanneer u wordt gevraagd. De parameters bevatten een naam voor uw implementatie, de naam van de resourcegroep en het pad of de URL naar het sjabloonbestand. Als de **modus** parameter niet is opgegeven, de standaardwaarde van **incrementele** wordt gebruikt. Zie voor meer informatie [incrementele en volledige implementaties](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).

De volgende opdracht wordt u gevraagd om de vijf vereiste parameters in de PowerShell-venster:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

Als u wilt opgeven in plaats daarvan een parameterbestand, moet u de volgende opdracht gebruiken:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

U kunt ook parameters inline gebruiken wanneer u de deployment-cmdlet uitvoert. De opdracht is als volgt:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

Om uit te voeren een [voltooid](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) implementatie, stel de **modus** -parameter voor **Complete**:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

## <a name="verify-the-deployment"></a>De implementatie controleren

Als de bronnen worden geïmplementeerd, wordt een samenvatting van de implementatie weergegeven in het venster PowerShell:

```powershell
DeploymentName          : azuredeploy
ResourceGroupName       : MyDemoRG
ProvisioningState       : Succeeded
Timestamp               : 12/9/2017 01:06:54
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          eventHubNamespaceName  String               <eventHubNamespaceName>
                          eventHubName     String                     <eventHubName>
                          consumerGroupName  String                   <consumerGroupName>
                          existingEventHubResourceId  String
                          environmentName  String                     <environmentName>
                          environmentDisplayName  String
                          environmentSkuName  String                  S1
                          environmentSkuCapacity  Int                 1
                          environmentDataRetentionTime  String        P30D
                          eventSourceName  String                     <eventSourceName>
                          eventSourceDisplayName  String
                          eventSourceTimestampPropertyName  String
                          eventSourceKeyName  String                  manage
                          accessPolicyReaderObjectIds  Array          []
                          accessPolicyContributorObjectIds  Array     [
                            "AGUID001-0000-0000-0000-000000000000",
                            "AGUID002-0000-0000-0000-000000000000"
                          ]

Outputs                 :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          dataAccessFQDN   String
                          <guid>.env.timeseries.azure.com
```

## <a name="deploy-the-quickstart-template-through-the-azure-portal"></a>De sjabloon Quick Start via de Azure portal implementeren

De startpagina van de sjabloon Quick Start op GitHub bevat ook een **implementeren in Azure** knop. Erop te klikken, opent een aangepaste implementatie-pagina in de Azure portal. Op deze pagina kunt u opgeven of waarden selecteren voor elk van de parameters van de [vereiste parameters](time-series-insights-manage-resources-using-azure-resource-manager-template.md#required-parameters) of [optionele parameters](time-series-insights-manage-resources-using-azure-resource-manager-template.md#optional-parameters) tabellen. Na het invullen van de instellingen, te klikken op de **aankoop** knop initieert de sjabloonimplementatie.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-timeseriesinsights-environment-with-eventhub%2Fazuredeploy.json" target="_blank"> <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het beheren van programmatisch Time Series Insights-resources met REST API's [Time Series Insights Management](https://docs.microsoft.com/rest/api/time-series-insights-management/).
