---
title: Over het beheren van uw Azure Time Series Insights-omgeving met behulp van Azure Resource Manager-sjablonen | Microsoft Docs
description: Dit artikel wordt beschreven hoe u uw Azure Time Series Insights-omgeving via een programma met behulp van Azure Resource Manager beheert.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 12/08/2017
ms.openlocfilehash: 3ca9af8c2c504f75322e00fdaaeac9a3e727a820
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627124"
---
# <a name="create-time-series-insights-resources-using-azure-resource-manager-templates"></a>Time Series Insights-resources met behulp van Azure Resource Manager-sjablonen maken

In dit artikel wordt beschreven hoe u maken en implementeren van Time Series Insights-resources met behulp van Azure Resource Manager-sjablonen, PowerShell en de Time Series Insights-resourceprovider.

Time Series Insights ondersteunt de volgende bronnen:
   | Resource | Beschrijving |
   | --- | --- |
   | Omgeving | Een Time Series Insights-omgeving is een logische groepering van gebeurtenissen die worden gelezen uit de gebeurtenis-brokers, opgeslagen en beschikbaar gesteld voor de query. Zie voor meer informatie [uw Azure Time Series Insights-omgeving plannen](time-series-insights-environment-planning.md) |
   | Bron van gebeurtenis | Een gebeurtenisbron is een verbinding met een gebeurtenis-broker van Time Series Insights leest en gebeurtenissen kan opnemen in de omgeving. Momenteel ondersteunde gebeurtenisbronnen zijn IoT-Hub en Event Hub. |
   | Referentiegegevensset | Verwijzingsgegevenssets behoren metagegevens over de gebeurtenissen in de omgeving. Metagegevens in de sets van verwijzingsgegevens worden met gebeurtenissen worden toegevoegd tijdens de inkomende gegevens. Sets van verwijzingsgegevens worden gedefinieerd als resources door hun belangrijkste eigenschappen van gebeurtenis. De metagegevens van de werkelijke waaruit de referentiegegevensset is geüpload naar of gewijzigd via de gegevenslaag API's. |
   | Toegangsbeleid | Toegangsbeleid voor verleent machtigingen voor het uitvoeren van gegevensquery's, het bewerken van referentiegegevens in de omgeving en het delen van opgeslagen query's en perspectieven die aan de omgeving zijn gekoppeld. Zie voor meer informatie [gegevenstoegang verlenen tot een Time Series Insights-omgeving met behulp van Azure portal](time-series-insights-data-access.md) |

Resource Manager-sjabloon is een JSON-bestand dat de infrastructuur en configuratie van resources in een resourcegroep definieert. Zie de volgende documenten voor meer informatie:

- [Overzicht van Azure Resource Manager - sjabloon implementeren](../azure-resource-manager/resource-group-overview.md#template-deployment)
- [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)

De [201-timeseriesinsights-omgeving-met-eventhub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-timeseriesinsights-environment-with-eventhub) quickstart-sjabloon is gepubliceerd op GitHub. Deze sjabloon maakt u een Time Series Insights-omgeving, een onderliggende gebeurtenisbron geconfigureerd voor het gebruiken van gebeurtenissen van een Event Hub en toegangsbeleid die toegang tot gegevens van de omgeving verlenen. Als een bestaande Event Hub is niet opgegeven, wordt een gemaakt met de implementatie.

## <a name="deploy-the-quickstart-template-locally-using-powershell"></a>Lokaal via PowerShell quickstart-sjabloon implementeren

De volgende procedure wordt beschreven hoe u PowerShell gebruiken voor het implementeren van een Azure Resource Manager-sjabloon die wordt gemaakt van een Time Series Insights-omgeving, een onderliggende gebeurtenisbron geconfigureerd voor het gebruiken van gebeurtenissen van een Event Hub en toegangsbeleid die toegang tot verlenen de gegevens van de omgeving. Als een bestaande Event Hub is niet opgegeven, wordt een gemaakt met de implementatie.

De geschatte werkstroom is als volgt:

1. PowerShell installeren.
1. De sjabloon en een parameterbestand maken.
1. In PowerShell, moet u zich aanmelden bij uw Azure-account.
1. Maak een nieuwe resourcegroep als dit nog niet bestaat.
1. De implementatie testen.
1. Implementeer de sjabloon.

### <a name="install-powershell"></a>PowerShell installeren

Azure PowerShell installeren door de instructies in [aan de slag met Azure PowerShell](/powershell/azure/get-started-azureps).

### <a name="create-a-template"></a>Een sjabloon maken

Kloon of kopieer de [201-timeseriesinsights-omgeving-met-eventhub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-timeseriesinsights-environment-with-eventhub/azuredeploy.json) -sjabloon.

### <a name="create-a-parameters-file"></a>Maak een parameterbestand

Voor het maken van een parameterbestand, Kopieer de [201-timeseriesinsights-omgeving-met-eventhub](https://github.com/Azure/azure-quickstart-templates/blob/master/201-timeseriesinsights-environment-with-eventhub/azuredeploy.parameters.json) bestand.

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
   | consumerGroupName | De naam van de consumergroep die de Time Series Insights-service wordt gebruikt om te lezen van de gegevens van de event hub. **Opmerking:** om bronconflicten te voorkomen, deze consumergroep moet worden toegewezen aan de Time Series Insights-service en niet wordt gedeeld met andere lezers. |
   | EnvironmentName | De naam van de omgeving. De naam kan niet bevatten: ' <', ' >', '%', '&', ': ','\\','?', '/' en alle stuurcodes. Alle andere tekens zijn toegestaan.|
   | eventSourceName | De naam van de onderliggende bron van gebeurtenis bron. De naam kan niet bevatten: ' <', ' >', '%', '&', ': ','\\','?', '/' en alle stuurcodes. Alle andere tekens zijn toegestaan. |

#### <a name="optional-parameters"></a>Optionele Parameters

   | Parameter | Beschrijving |
   | --- | --- |
   | existingEventHubResourceId | Een optionele resource-ID van een bestaande Event Hub die wordt verbonden met de Time Series Insights-omgeving via de gebeurtenisbron. **Opmerking:** de implementatie van de sjabloon gebruiker moet over bevoegdheden beschikken om de bewerking listkeys op de Event Hub. Als er geen waarde wordt doorgegeven, wordt een nieuwe event hub wordt gemaakt door de sjabloon. |
   | environmentDisplayName | Een optionele beschrijvende naam om weer te geven in interfaces voor hulpprogramma's of een gebruiker in plaats van de omgevingsnaam van de. |
   | environmentSkuName | De naam van de sku. Zie voor meer informatie de [pagina met prijzen van Time Series Insights](https://azure.microsoft.com/pricing/details/time-series-insights/).  |
   | environmentSkuCapacity | De capaciteit van de eenheid van de Sku. Zie voor meer informatie de [pagina met prijzen van Time Series Insights](https://azure.microsoft.com/pricing/details/time-series-insights/).|
   | environmentDataRetentionTime | De minimale duur van de omgeving gebeurtenissen zijn beschikbaar voor de query. De waarde moet worden opgegeven in de ISO 8601-notatie, bijvoorbeeld 'P30D' voor een beleid voor het bewaren van 30 dagen. |
   | eventSourceDisplayName | Een optionele beschrijvende naam om weer te geven in hulpprogramma's of de gebruiker in plaats van de naam van de gebeurtenis-interfaces. |
   | eventSourceTimestampPropertyName | De gebeurteniseigenschap die wordt gebruikt als de tijdstempel van de bron van de gebeurtenis. Als een waarde niet is opgegeven voor timestampPropertyName, of als null of lege tekenreeks is opgegeven, wordt de aanmaaktijd van de gebeurtenis worden gebruikt. |
   | eventSourceKeyName | De naam van de gedeelde toegangssleutel die de Time Series Insights-service wordt gebruikt voor het verbinding maken met de event hub. |
   | accessPolicyReaderObjectIds | Een lijst met object-id's van de gebruikers of toepassingen in Azure AD die moet lezerstoegang tot de omgeving hebben. De service principal object-id kan worden verkregen door het aanroepen van de **Get-AzureRMADUser** of de **Get-AzureRMADServicePrincipal** cmdlets. Het maken van een toegangsbeleid voor Azure AD-groepen wordt nog niet ondersteund. |
   | accessPolicyContributorObjectIds | Een lijst met object-id's van de gebruikers of toepassingen in Azure AD die Inzender-toegang tot de omgeving moet. De service principal object-id kan worden verkregen door het aanroepen van de **Get-AzureRMADUser** of de **Get-AzureRMADServicePrincipal** cmdlets. Het maken van een toegangsbeleid voor Azure AD-groepen wordt nog niet ondersteund. |

Als een voorbeeld zou de volgende parameterbestand worden gebruikt om een omgeving en de bron van een gebeurtenis die gebeurtenissen in een bestaande event hub leest te maken. Het maakt ook twee toegangsbeleid die Inzender-toegang tot de omgeving verlenen.

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

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Meld u aan bij Azure en stel het Azure-abonnement

Voer de volgende opdracht uit vanuit een PowerShell-prompt:

```powershell
Connect-AzureRmAccount
```

U wordt gevraagd om aan te melden bij uw Azure-account. Voer na het aanmelden, de volgende opdracht uit om de beschikbare abonnementen weer te geven:

```powershell
Get-AzureRMSubscription
```

Met deze opdracht retourneert een lijst met beschikbare Azure-abonnementen. Kies een abonnement voor de huidige sessie met de volgende opdracht. Vervang `<YourSubscriptionId>` met de GUID voor de Azure-abonnement u wilt gebruiken:

```powershell
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Instellen van de resourcegroep

Als u nog geen een bestaande resource groep, maakt u een nieuwe resourcegroep met de **New-AzureRmResourceGroup** opdracht. Geef de naam van de resourcegroep en de locatie die u wilt gebruiken. Bijvoorbeeld:

```powershell
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Als dit lukt, wordt een overzicht van de nieuwe resourcegroep weergegeven.

```powershell
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>De implementatie testen

Uw implementatie valideren door het uitvoeren van de `Test-AzureRmResourceGroupDeployment` cmdlet. Bij het testen van de implementatie, parameters opgeven precies zoals u zou doen bij het uitvoeren van de implementatie.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

### <a name="create-the-deployment"></a>De implementatie maken

Voer voor het maken van de nieuwe implementatie de `New-AzureRmResourceGroupDeployment` -cmdlet en geef de vereiste parameters als u wordt gevraagd. De parameters bevatten een naam voor uw implementatie, de naam van de resourcegroep, en het pad of de URL naar het sjabloonbestand. Als de **modus** parameter niet wordt opgegeven, de standaardwaarde van **incrementele** wordt gebruikt. Zie voor meer informatie, [incrementele en volledige implementaties](../azure-resource-manager/deployment-modes.md).

De volgende opdracht vraagt u om de vijf vereiste parameters in de PowerShell-venster:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

Als u wilt opgeven in plaats daarvan een parameterbestand, gebruik de volgende opdracht:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

U kunt ook een inline-parameters gebruiken wanneer u de implementatie-cmdlet uitvoert. De opdracht is als volgt:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

Om uit te voeren een [voltooid](../azure-resource-manager/deployment-modes.md) implementatie, de **modus** parameter **voltooid**:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

## <a name="verify-the-deployment"></a>De implementatie controleren

Als de resources worden geïmplementeerd, wordt een samenvatting van de implementatie weergegeven in het PowerShell-venster:

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

## <a name="deploy-the-quickstart-template-through-the-azure-portal"></a>De quickstart-sjabloon via Azure portal implementeren

De startpagina van de quickstart-sjabloon op GitHub bevat ook een **implementeren in Azure** knop. Erop te klikken, opent een aangepaste implementatie-pagina in Azure portal. Op deze pagina kunt u opgeven of waarden selecteren voor elk van de parameters van de [vereiste parameters](time-series-insights-manage-resources-using-azure-resource-manager-template.md#required-parameters) of [optionele parameters](time-series-insights-manage-resources-using-azure-resource-manager-template.md#optional-parameters) tabellen. Na het invullen van de instellingen, te klikken op de **aankoop** knop de sjabloonimplementatie worden gestart.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-timeseriesinsights-environment-with-eventhub%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het programmatisch beheren van Time Series Insights-resources met behulp van REST-API's [Time Series Insights Management](https://docs.microsoft.com/rest/api/time-series-insights-management/).
