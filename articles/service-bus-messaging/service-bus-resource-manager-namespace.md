---
title: Service Bus Messaging-naamruimte maken met Azure Resource Manager-sjabloon | Microsoft Docs
description: Gebruik Azure Resource Manager-sjabloon voor het maken van een Service Bus Messaging-naamruimte
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: dc0d6482-6344-4cef-8644-d4573639f5e4
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/21/2019
ms.author: spelluru
ms.openlocfilehash: 4162775153a48dc8ea28e06f7c99f9927b9c602a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444760"
---
# <a name="create-a-service-bus-namespace-by-using-an-azure-resource-manager-template"></a>Een Service Bus-naamruimte maken met behulp van een Azure Resource Manager-sjabloon

Informatie over het implementeren van een Azure Resource Manager-sjabloon voor het maken van een Service Bus-naamruimte. U kunt deze sjabloon gebruiken voor uw eigen implementaties of de sjabloon aanpassen aan uw eisen. Zie voor meer informatie over het maken van sjablonen, [Azure Resource Manager-documentatie](/azure/azure-resource-manager/).

De volgende sjablonen zijn ook beschikbaar voor het maken van Service Bus-naamruimten:

* [Een Service Bus-naamruimte met een wachtrij maken](./service-bus-resource-manager-namespace-queue.md)
* [Een Service Bus-naamruimte maken met een onderwerp en abonnement](./service-bus-resource-manager-namespace-topic.md)
* [Een Service Bus-naamruimte maken met de wachtrij en de autorisatie-regel](./service-bus-resource-manager-namespace-auth-rule.md)
* [Een Service Bus-naamruimte maken met een onderwerp, abonnement en regel](./service-bus-resource-manager-namespace-topic-with-rule.md)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="create-a-service-bus-namespace"></a>Een service bus-naamruimte maken

In deze quickstart maakt u een [bestaande Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/azuredeploy.json) van [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/):

[!code-json[create-azure-service-bus-namespace](~/quickstart-templates/101-servicebus-create-namespace/azuredeploy.json)]

Meer voorbeelden van de sjabloon, Zie [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Servicebus&pageNumber=1&sort=Popular).

Een service bus-naamruimte maken met het implementeren van een sjabloon:

1. Selecteer **uitproberen** uit het volgende codeblok en volg de instructies om aan te melden bij de Azure Cloud shell.

    ```azurepowershell-interactive
    $serviceBusNamespaceName = Read-Host -Prompt "Enter a name for the service bus namespace to be created"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${serviceBusNamespaceName}rg"
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json"

    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -serviceBusNamespaceName $serviceBusNamespaceName

    Write-Host "Press [ENTER] to continue ..."
    ```

    Naam van de resourcegroep is de naam van service bus-naamruimte met **rg** toegevoegd.

2. Selecteer **Kopiëren** om het PowerShell-script te kopiëren.
3. Met de rechtermuisknop op de shell-console en selecteer vervolgens **plakken**.

Het duurt een paar minuten naar een event hub maken.

## <a name="verify-the-deployment"></a>De implementatie controleren

Als u wilt zien van de geïmplementeerde service bus-naamruimte, kunt u opent u de resourcegroep van de Azure-portal of gebruik het volgende Azure PowerShell-script. Als de cloudshell nog steeds geopend is, moet u niet de eerste en tweede regel van het volgende script kopiëren of uitgevoerd.

```azurepowershell-interactive
$serviceBusNamespaceName = Read-Host -Prompt "Enter the same service bus namespace name used earlier"
$resourceGroupName = "${serviceBusNamespaceName}rg"

Get-AzServiceBusNamespace -ResourceGroupName $resourceGroupName -Name $serviceBusNamespaceName

Write-Host "Press [ENTER] to continue ..."
```

Azure PowerShell wordt gebruikt voor het implementeren van de sjabloon in deze zelfstudie. Zie voor andere methoden voor het implementeren van sjabloon:

* [Met behulp van de Azure-portal](../azure-resource-manager/resource-group-template-deploy-portal.md).
* [Met behulp van Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).
* [Met behulp van REST-API](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen. Als de cloudshell nog steeds geopend is, moet u niet de eerste en tweede regel van het volgende script kopiëren of uitgevoerd.

```azurepowershell-interactive
$serviceBusNamespaceName = Read-Host -Prompt "Enter the same service bus namespace name used earlier"
$resourceGroupName = "${serviceBusNamespaceName}rg"

Remove-AzResourceGroup -ResourceGroupName $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een Service Bus-naamruimte gemaakt. Zie de andere Quick starts om informatie over het maken van wachtrijen, onderwerpen/abonnementen, en ze gebruiken:

* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Aan de slag met Service Bus-onderwerpen](service-bus-dotnet-how-to-use-topics-subscriptions.md)
