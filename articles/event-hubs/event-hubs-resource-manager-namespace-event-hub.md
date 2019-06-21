---
title: Een event hub maken met consumergroep - Azure Event Hubs | Microsoft Docs
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
ms.date: 06/14/2019
ms.author: shvija
ms.openlocfilehash: 007e016672f8548956b37b961805183a504d6bf0
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67154070"
---
# <a name="quickstart-create-an-event-hub-by-using-an-azure-resource-manager-template"></a>Quickstart: Een event hub maken met behulp van een Azure Resource Manager-sjabloon

Azure Event Hubs is een big data-platform voor het streamen van gegevens en een gebeurtenisopneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Zie [Overzicht van Event Hubs](event-hubs-about.md) en [Functies van Event Hubs](event-hubs-features.md) voor een gedetailleerd overzicht van Event Hubs.

In deze quickstart maakt u een event hub maken met behulp van een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-overview.md). Implementeren van een Azure Resource Manager-sjabloon voor het maken van een naamruimte van het type [Event Hubs](event-hubs-what-is-event-hubs.md), met een event hub. Het artikel wordt beschreven hoe om te definiëren welke resources worden geïmplementeerd en over het definiëren van parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of de sjabloon aanpassen aan uw eisen. Zie voor meer informatie over het maken van sjablonen [Authoring Azure Resource Manager-sjablonen][Authoring Azure Resource Manager templates]. Zie [Microsoft.EventHub resource types](/azure/templates/microsoft.eventhub/allversions) (Microsoft.EventHub-resourcetypen) voor de JSON-syntaxis en eigenschappen die u kunt gebruiken in een sjabloon.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="create-an-event-hub"></a>Een Event Hub maken

In deze quickstart maakt u een [bestaande Resource Manager-sjabloon](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/101-eventhubs-create-namespace-and-eventhub/azuredeploy.json). Meer voorbeelden van de sjabloon, Zie [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/?term=eventhub&pageNumber=1&sort=Popular).

Voor het implementeren van de sjabloon:

1. Selecteer **uitproberen** uit het volgende codeblok en volg de instructies om aan te melden bij de Azure Cloud shell.

   ```azurepowershell-interactive
   $projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
   $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
   $resourceGroupName = "${projectName}rg"
   $templateUri = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/101-eventhubs-create-namespace-and-eventhub/azuredeploy.json"

   New-AzResourceGroup -Name $resourceGroupName -Location $location
   New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName

   Write-Host "Press [ENTER] to continue ..."
   ```

   Het duurt een paar minuten naar een event hub maken.

1. Selecteer **Kopiëren** om het PowerShell-script te kopiëren.
1. Met de rechtermuisknop op de shell-console en selecteer vervolgens **plakken**.

## <a name="verify-the-deployment"></a>De implementatie controleren

Als u wilt controleren of de implementatie, kunt u de resourcegroep van openen de [Azure-portal](https://portal.azure.com), of gebruik de volgende Azure PowerShell-script.  Als de cloudshell nog steeds geopend is, moet u niet de eerste regel (Read-Host) kopiëren of uitgevoerd.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"
$namespaceName = "${projectName}ns"

Get-AzEventHub -ResourceGroupName $resourceGroupName -Namespace $namespaceName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen. Als de cloudshell nog steeds geopend is, moet u niet de eerste regel (Read-Host) kopiëren of uitgevoerd.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -ResourceGroupName $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een Event Hubs-naamruimte en een event hub in de naamruimte gemaakt. Zie voor stapsgewijze instructies voor het verzenden van gebeurtenissen naar (of) gebeurtenissen ontvangen van een event hub, de **verzenden en ontvangen van gebeurtenissen** zelfstudies:

- [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
- [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md)
- [Java](event-hubs-java-get-started-send.md)
- [Python](event-hubs-python-get-started-send.md)
- [Node.js](event-hubs-node-get-started-send.md)
- [Go](event-hubs-go-get-started-send.md)
- [C (alleen verzenden)](event-hubs-c-getstarted-send.md)
- [Apache Storm (alleen reecive)](event-hubs-storm-getstarted-receive.md)


[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
