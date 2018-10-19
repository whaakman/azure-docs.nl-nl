---
title: Maak een Azure Event Hubs-naamruimte en consumenten-groep met behulp van een sjabloon | Microsoft Docs
description: Een Event Hubs-naamruimte maken met een event hub en een consumentengroep met behulp van Azure Resource Manager-sjablonen
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: 28bb4591-1fd7-444f-a327-4e67e8878798
ms.service: event-hubs
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 10/16/2018
ms.author: shvija
ms.openlocfilehash: e3a7b2a7ad866fe6c70c638dc5203b9a31c6b5b3
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49426631"
---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Een Event Hubs-naamruimte maken met event hub en consumenten met een Azure Resource Manager-sjabloon
Azure Event Hubs is een uiterst schaalbaar platform voor het streamen van gegevens en een opneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Deze quickstart laat zien hoe u een event hub met behulp van een Azure Resource Manager-sjabloon maken.

Een Azure Resource Manager-sjabloon maakt u een naamruimte van het type [Event Hubs](event-hubs-what-is-event-hubs.md), met één gebeurtenishub, en een consumentengroep beperkt. Het artikel wordt beschreven hoe om te definiëren welke resources worden geïmplementeerd en over het definiëren van parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of de sjabloon aanpassen aan uw eisen. Zie voor meer informatie over het maken van sjablonen [Authoring Azure Resource Manager-sjablonen][Authoring Azure Resource Manager templates].

Zie voor de volledige sjabloon, de [Event hub- en klanttoepassingen groep sjabloon] [ Event Hub and consumer group template] op GitHub.

> [!NOTE]
> Om te controleren op de meest recente sjablonen, gaat u naar de galerie [Azure-snelstartsjablonen][Azure Quickstart Templates] en zoekt u naar Event Hubs.

## <a name="prerequisites"></a>Vereisten
U hebt een Azure-abonnement nodig om deze snelstart te voltooien. Als u nog geen hebt, [Maak een gratis account] [-] voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u **Azure PowerShell** om de Resource Manager-sjabloon te implementeren lokaal, moet u uitvoeren de nieuwste versie van PowerShell voor het voltooien van deze quickstart. Als u Azure PowerShell wilt installeren of upgraden, raadpleegt u [Azure PowerShell installeren en configureren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

Als u wilt installeren en gebruiken **Azure CLI** om de Resource Manager-sjabloon te implementeren lokaal, in deze zelfstudie moet u Azure CLI versie 2.0.4 of hoger. Voer `az --version` uit om uw versie te controleren. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren]( /cli/azure/install-azure-cli).

## <a name="what-will-you-deploy"></a>Wat wilt u implementeren?

Met deze sjabloon implementeert u een Event Hubs-naamruimte met een event hub en een consumergroep.

Klik op de volgende knop om de implementatie automatisch uit te voeren:

[![Implementeren in Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure Resource Manager kunt u parameters definiëren voor waarden die u wilt opgeven wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam `Parameters` die alle parameterwaarden bevat. U moet een parameter definiëren voor waarden die variëren, gebaseerd op het project dat u wilt implementeren of op basis van de omgeving waarnaar u implementeert. Definieer geen parameters voor waarden die altijd hetzelfde blijven. De waarde van elke parameter in de sjabloon definieert de resources die zijn geïmplementeerd.

De sjabloon definieert de volgende parameters:

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

De naam van de Event Hubs-naamruimte die wordt gemaakt.

```json
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName

De naam van de gebeurtenishub die in de Event Hubs-naamruimte wordt gemaakt.

```json
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName

De naam van de consumergroep hebt gemaakt voor de event hub.

```json
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion

De API-versie van de sjabloon.

```json
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Resources om te implementeren

Hiermee maakt u een naamruimte van het type **EventHubs**, met een event hub en een consumergroep:

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Opdrachten om implementatie uit te voeren

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```azurepowershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```
### <a name="create-a-storage-account-for-event-processor-host"></a>Een opslagaccount voor Event Processor Host maken

Event Processor Host vereenvoudigt het ontvangen van gebeurtenissen van Event Hubs door permanente controlepunten en parallelle ontvangsten te beheren. Voor controlepunten heeft Event Processor Host een opslagaccount nodig. Voer de volgende opdrachten uit als u een opslagaccount wilt maken en de bijbehorende sleutels wilt ophalen:

```azurepowershell-interactive
# Create a standard general purpose storage account 
New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name storage_account_name -Location eastus -SkuName Standard_LRS 
e
# Retrieve the storage account key for accessing it
Get-AzureRmStorageAccountKey -ResourceGroupName myResourceGroup -Name storage_account_name
```

Een verbindingsreeks is vereist om verbinding te maken met uw Event Hub en gebeurtenissen te verwerken. U kunt een verbindingsreeks ophalen door de volgende opdrachten uit te voeren:

```azurepowershell-interactive
Get-AzureRmEventHubKey -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Name RootManageSharedAccessKey
```


## <a name="azure-cli"></a>Azure-CLI

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

### <a name="create-a-storage-account-for-event-processor-host"></a>Een opslagaccount voor Event Processor Host maken
```azurecli-interactive
# Create a general purpose standard storage account for Event Processor Host
az storage account create --name storageAccountName --resource-group myResourceGroup --location eastus2 --sku Standard_RAGRS --encryption blob

# List the storage account access keys
az storage account keys list --resource-group myResourceGroup --account-name storageAccountName

# Get namespace connection string
az eventhubs namespace authorization-rule keys list --resource-group myResourceGroup --namespace-name namespaceName --name RootManageSharedAccessKey
```

Knip en plak de verbindingsreeks naar een tijdelijke locatie, zoals Kladblok, om later te gebruiken.

## <a name="stream-into-event-hubs"></a>Streamen in Event Hubs

U kunt nu beginnen met streamen naar uw Event Hubs. De voorbeelden kunnen worden gedownload of uit Git worden gekloond vanuit de [Event Hubs-opslagplaats](https://github.com/Azure/azure-event-hubs)

### <a name="ingest-events"></a>Gebeurtenissen opnemen

Als u wilt gaan beginnen met het streamen van gebeurtenissen, downloadt u [SampleSender](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) uit GitHub of kloont u de [Event Hubs GitHub-opslagplaats](https://github.com/Azure/azure-event-hubs) door de volgende opdracht te typen:

```bash
git clone https://github.com/Azure/azure-event-hubs.git
```

Navigeer naar de map \azure-event-hubs\samples\DotNet\Microsoft.Azure.EventHubs\SampleSender en laad het bestand SampleSender.sln in Visual Studio.

Voeg vervolgens Nuget-pakket [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) NuGet-pakket aan het project toe.

Vervang in het bestand Program.cs de volgende tijdelijke aanduidingen door de naam van uw Event Hub en de verbindingsreeks:

```C#
private const string EhConnectionString = "Event Hubs connection string";
private const string EhEntityPath = "Event Hub name";

```

Bouw nu het voorbeeld en voer het uit. Zoals u ziet worden de gebeurtenissen in uw Event Hub nu opgenomen:

![][3]

### <a name="receive-and-process-events"></a>Gebeurtenissen ontvangen en verwerken

Download nu het ontvangstvoorbeeld van Event Processor Host, waar de berichten worden ontvangen die u net hebt verzonden. Download [SampleEphReceiver](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) uit GitHub of kloon de [Event Hubs GitHub-opslagplaats](https://github.com/Azure/azure-event-hubs) door de volgende opdracht te typen:

```bash
git clone https://github.com/Azure/azure-event-hubs.git
```

Navigeer naar de map \azure-event-hubs\samples\DotNet\Microsoft.Azure.EventHubs\SampleEphReceiver en laad het oplossingsbestand SampleEphReceiver.sln in Visual Studio.

Voeg vervolgens de NuGet-pakketten [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) en [Microsoft.Azure.EventHubs.Processor](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) toe aan het project.

Vervang de volgende constanten door de bijbehorende waarden in het bestand Program.cs:

```C#
private const string EventHubConnectionString = "Event Hubs connection string";
private const string EventHubName = "Event Hub name";
private const string StorageContainerName = "Storage account container name";
private const string StorageAccountName = "Storage account name";
private const string StorageAccountKey = "Storage account key";
```

Bouw nu het voorbeeld en voer het uit. Zoals u ziet worden de gebeurtenissen nu in uw voorbeeldtoepassing ontvangen:

![][4]

In Azure Portal kunt u als volgt de snelheid zien waarmee gebeurtenissen worden verwerkt voor een bepaalde Event Hubs-naamruimte:

![][5]


## <a name="next-steps"></a>Volgende stappen

In dit artikel, kunt u de Event Hubs-naamruimte gemaakt en voorbeeldtoepassingen die wordt gebruikt voor het verzenden en ontvangen van gebeurtenissen van uw event hub. Zie de volgende zelfstudies voor stapsgewijze instructies voor het verzenden van gebeurtenissen naar (of) gebeurtenissen ontvangen van een event hub: 

1. **Gebeurtenissen verzenden naar een event hub**: [.NET Standard](event-hubs-dotnet-standard-getstarted-send.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md), [Java](event-hubs-java-get-started-send.md), [Python](event-hubs-python-get-started-send.md), [Node.js ](event-hubs-node-get-started-send.md), [Gaat](event-hubs-go-get-started-send.md), [C](event-hubs-c-getstarted-send.md)
2. **Gebeurtenissen ontvangen van een event hub**: [.NET Standard](event-hubs-dotnet-standard-getstarted-receive-eph.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md), [Java](event-hubs-java-get-started-receive-eph.md), [Python](event-hubs-python-get-started-receive.md), [ Node.js](event-hubs-node-get-started-receive.md), [gaat](event-hubs-go-get-started-receive-eph.md), [Apache Storm](event-hubs-storm-getstarted-receive.md)

[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
