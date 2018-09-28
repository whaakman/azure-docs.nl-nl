---
title: Azure Service Bus-resources met behulp van Resource Manager-sjablonen maken | Microsoft Docs
description: Azure Resource Manager-sjablonen gebruiken om automatisch te maken van Service Bus-bronnen
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: 24f6a207-0fa4-49cf-8a58-963f9e2fd655
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 09/11/2018
ms.author: spelluru
ms.openlocfilehash: 5693307f9d032ad544a44eabd19126700c5a4099
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47407158"
---
# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Service Bus-bronnen met behulp van Azure Resource Manager-sjablonen maken

In dit artikel wordt beschreven hoe u maken en implementeren van Service Bus-resources met behulp van Azure Resource Manager-sjablonen, PowerShell en de Service Bus-resourceprovider.

Azure Resource Manager-sjablonen helpen u bij het definiëren van de resources te implementeren voor een oplossing en om op te geven van de parameters en variabelen vaststellen waarmee u de waarden voor verschillende omgevingen. De sjabloon is geschreven in JSON en bestaat uit de expressies die u gebruiken kunt om waarden voor uw implementatie samen te stellen. Zie voor gedetailleerde informatie over het schrijven van Azure Resource Manager-sjablonen, en een beschrijving van de sjabloonindeling, [structuur en de syntaxis van Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md).

> [!NOTE]
> De voorbeelden in dit artikel ziet u hoe u Azure Resource Manager gebruiken voor het maken van een Service Bus-naamruimte en een Berichtentiteit (wachtrij). Voor andere sjabloonvoorbeelden, gaat u naar de [galerie met Azure-Snelstartsjablonen] [ Azure Quickstart Templates gallery] en zoek naar de **Service Bus**.
>
>

## <a name="service-bus-resource-manager-templates"></a>Service Bus-Resource Manager-sjablonen

Deze Service Bus Azure Resource Manager-sjablonen zijn beschikbaar voor downloaden en implementeren. Klik op de volgende koppelingen voor meer informatie over elk ervan, met koppelingen naar de sjablonen van GitHub:

* [Een Service Bus-naamruimte maken](service-bus-resource-manager-namespace.md)
* [Een Service Bus-naamruimte met een wachtrij maken](service-bus-resource-manager-namespace-queue.md)
* [Een Service Bus-naamruimte maken met een onderwerp en abonnement](service-bus-resource-manager-namespace-topic.md)
* [Een Service Bus-naamruimte maken met de wachtrij en de autorisatie-regel](service-bus-resource-manager-namespace-auth-rule.md)
* [Een Service Bus-naamruimte maken met een onderwerp, abonnement en regel](service-bus-resource-manager-namespace-topic-with-rule.md)

## <a name="deploy-with-powershell"></a>Implementeren met PowerShell

De volgende procedure wordt beschreven hoe u PowerShell gebruiken voor het implementeren van een Azure Resource Manager-sjabloon waarmee u een Standard-laag Service Bus-naamruimte en een wachtrij binnen deze naamruimte maakt. In dit voorbeeld is gebaseerd op de [een Service Bus-naamruimte maken met de wachtrij](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) sjabloon. De geschatte werkstroom is als volgt:

1. PowerShell installeren.
2. De sjabloon en (optioneel) een parameterbestand maken.
3. In PowerShell, moet u zich aanmelden bij uw Azure-account.
4. Maak een nieuwe resourcegroep als dit nog niet bestaat.
5. De implementatie testen.
6. Indien gewenst, de implementatiemodus instellen.
7. Implementeer de sjabloon.

Zie voor meer informatie over het implementeren van Azure Resource Manager-sjablonen [resources implementeren met Azure Resource Manager-sjablonen][Deploy resources with Azure Resource Manager templates].

### <a name="install-powershell"></a>PowerShell installeren

Azure PowerShell installeren door de instructies in [aan de slag met Azure PowerShell](/powershell/azure/get-started-azureps).

### <a name="create-a-template"></a>Een sjabloon maken

Kloon de opslagplaats of kopieer de [201-service bus-maken-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) -sjabloon:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "serviceBusNamespaceName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Service Bus namespace"
      }
    },
    "serviceBusQueueName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Queue"
      }
    }
  },
  "variables": {
    "defaultSASKeyName": "RootManageSharedAccessKey",
    "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]",
    "sbVersion": "2017-04-01"
  },
  "resources": [
    {
      "apiVersion": "2017-04-01",
      "name": "[parameters('serviceBusNamespaceName')]",
      "type": "Microsoft.ServiceBus/Namespaces",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard"
      },
      "properties": {},
      "resources": [
        {
          "apiVersion": "2017-04-01",
          "name": "[parameters('serviceBusQueueName')]",
          "type": "Queues",
          "dependsOn": [
            "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
          ],
          "properties": {
            "lockDuration": "PT5M",
            "maxSizeInMegabytes": "1024",
            "requiresDuplicateDetection": "false",
            "requiresSession": "false",
            "defaultMessageTimeToLive": "P10675199DT2H48M5.4775807S",
            "deadLetteringOnMessageExpiration": "false",
            "duplicateDetectionHistoryTimeWindow": "PT10M",
            "maxDeliveryCount": "10",
            "autoDeleteOnIdle": "P10675199DT2H48M5.4775807S",
            "enablePartitioning": "false",
            "enableExpress": "false"
          }
        }
      ]
    }
  ],
  "outputs": {
    "NamespaceConnectionString": {
      "type": "string",
      "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
    },
    "SharedAccessPolicyPrimaryKey": {
      "type": "string",
      "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
    }
  }
}
```

### <a name="create-a-parameters-file-optional"></a>Maak een parameterbestand (optioneel)

Een optionele parameters-bestand wilt gebruiken, kopieert u de [201-service bus-maken-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) bestand. Vervang de waarde van `serviceBusNamespaceName` met de naam van de Service Bus-naamruimte die u wilt maken in deze implementatie en vervang de waarde van `serviceBusQueueName` met de naam van de wachtrij die u wilt maken.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2017-04-01"
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

Als u nog geen een bestaande resource groep, maakt u een nieuwe resourcegroep met de ** New-AzureRmResourceGroup ** opdracht. Geef de naam van de resourcegroep en de locatie die u wilt gebruiken. Bijvoorbeeld:

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
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>De implementatie maken

Voer voor het maken van de nieuwe implementatie de `New-AzureRmResourceGroupDeployment` -cmdlet en geef de vereiste parameters als u wordt gevraagd. De parameters bevatten een naam voor uw implementatie, de naam van de resourcegroep, en het pad of de URL naar het sjabloonbestand. Als de **modus** parameter niet wordt opgegeven, de standaardwaarde van **incrementele** wordt gebruikt. Zie voor meer informatie, [incrementele en volledige implementaties](../azure-resource-manager/deployment-modes.md).

De volgende opdracht vraagt u om de drie parameters in de PowerShell-venster:

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

### <a name="verify-the-deployment"></a>De implementatie controleren
Als de resources worden geïmplementeerd, wordt een samenvatting van de implementatie weergegeven in het PowerShell-venster:

```powershell
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2017 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2017-04-01

```

## <a name="next-steps"></a>Volgende stappen
U hebt nu gezien de basiswerkstroom en opdrachten voor het implementeren van een Azure Resource Manager-sjabloon. Ga naar de volgende koppelingen voor meer gedetailleerde informatie:

* [Overzicht van Azure Resource Manager][Azure Resource Manager overview]
* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell][Deploy resources with Azure Resource Manager templates]
* [Azure Resource Manager-sjablonen maken](../azure-resource-manager/resource-group-authoring-templates.md)

[Azure Resource Manager overview]: ../azure-resource-manager/resource-group-overview.md
[Deploy resources with Azure Resource Manager templates]: ../azure-resource-manager/resource-group-template-deploy.md
[Azure Quickstart Templates gallery]: https://azure.microsoft.com/documentation/templates/?term=service+bus
