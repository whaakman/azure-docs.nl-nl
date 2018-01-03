---
title: PowerShell gebruiken om Azure Event Hubs-resources te beheren | Microsoft Docs
description: Gebruik PowerShell-module maken en beheren van Event Hubs
services: event-hubs
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: de86b8241166d4e0bd03beb22550464457e3db5e
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="use-powershell-to-manage-event-hubs-resources"></a>PowerShell gebruiken om Event Hubs-resources te beheren

Microsoft Azure PowerShell is een scriptomgeving op servers die u gebruiken kunt om te beheren en de implementatie en beheer van Azure services automatiseren. In dit artikel wordt beschreven hoe u de [Event Hubs Resource Manager PowerShell-module](/powershell/module/azurerm.eventhub) inrichten en beheren van Event Hubs-entiteiten (naamruimten, afzonderlijke event hubs en consumergroepen) met behulp van een lokale Azure PowerShell-console of script.

U kunt ook Event Hubs-resources met behulp van Azure Resource Manager-sjablonen beheren. Zie voor meer informatie het artikel [maken van een naamruimte Event Hubs met event hub- en groep met een Azure Resource Manager-sjabloon](event-hubs-resource-manager-namespace-event-hub.md).

## <a name="prerequisites"></a>Vereisten

Voordat u begint, hebt u het volgende nodig:

* Een Azure-abonnement. Zie voor meer informatie over het verkrijgen van een abonnement [aanschafopties][purchase options], [lid biedt][member offers], of [gratis account][free account].
* Een computer met Azure PowerShell. Zie voor instructies [aan de slag met Azure PowerShell-cmdlets](/powershell/azure/get-started-azureps).
* Een algemeen begrip van de PowerShell-scripts, NuGet-pakketten en .NET Framework.

## <a name="get-started"></a>Aan de slag

De eerste stap is het gebruik van PowerShell zich aanmelden bij uw Azure-account en de Azure-abonnement. Volg de instructies in [aan de slag met Azure PowerShell-cmdlets](/powershell/azure/get-started-azureps) voor het aanmelden bij uw Azure-account, klikt u vervolgens toegang te krijgen tot de resources in uw Azure-abonnement.

## <a name="provision-an-event-hubs-namespace"></a>Inrichten van een naamruimte Event Hubs

Als u werkt met Event Hubs-naamruimten, kunt u de [Get-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace), [nieuw AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace), [verwijderen AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace), en [Set AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) cmdlets.

In dit voorbeeld maakt een paar lokale variabelen in het script; `$Namespace` en `$Location`.

* `$Namespace`is de naam van de Event Hubs-naamruimte die we wilt werken.
* `$Location`identificeert het datacenter waarin we de naamruimte inricht.
* `$CurrentNamespace`slaat de referentie-naamruimte die we ophalen (of maak).

In een werkelijke script `$Namespace` en `$Location` kunnen worden doorgegeven als parameters.

Dit deel van het script doet het volgende:

1. Probeert op te halen van een naamruimte Event Hubs met de opgegeven naam.
2. Als de naamruimte wordt gevonden, rapporteert wat is gevonden.
3. Als de naamruimte niet wordt gevonden, maakt u de naamruimte en vervolgens haalt de zojuist gemaakte naamruimte.

    ```powershell
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
   
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Host "The namespace $Namespace already exists in the $Location region:"
        # Report what was found
        Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "The $Namespace namespace does not exist."
        Write-Host "Creating the $Namespace namespace in the $Location region..."
        New-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
        Write-Host "The $Namespace namespace in Resource Group $ResGrpName in the $Location region has been successfully created."
    }
    ```

## <a name="create-an-event-hub"></a>Een Event Hub maken

Voer een naamruimte-controle met het script in de vorige sectie voor het maken van een event hub. Gebruik vervolgens de [nieuw AzureRmEventHub](/powershell/module/azurerm.eventhub/new-azurermeventhub) cmdlet voor het maken van de event hub:

```powershell
# Check if event hub already exists
$CurrentEH = Get-AzureRMEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName

if($CurrentEH)
{
    Write-Host "The event hub $EventHubName already exists in the $Location region:"
    # Report what was found
    Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "The $EventHubName event hub does not exist."
    Write-Host "Creating the $EventHubName event hub in the $Location region..."
    New-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -Location $Location -MessageRetentionInDays 3
    $CurrentEH = Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "The $EventHubName event hub in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

### <a name="create-a-consumer-group"></a>Een consumergroep maken

Een consumergroep binnen een event hub maken, voert u de naamruimte en event hub wordt gecontroleerd met behulp van de scripts in de vorige sectie. Gebruik vervolgens de [nieuw AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) cmdlet om de consumergroep binnen event hub te maken. Bijvoorbeeld:

```powershell
# Check if consumer group already exists
$CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -ErrorAction Ignore

if($CurrentCG)
{
    Write-Host "The consumer group $ConsumerGroupName in event hub $EventHubName already exists in the $Location region:"
    # Report what was found
    Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "The $ConsumerGroupName consumer group does not exist."
    Write-Host "Creating the $ConsumerGroupName consumer group in the $Location region..."
    New-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
    $CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "The $ConsumerGroupName consumer group in event hub $EventHubName in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

#### <a name="set-user-metadata"></a>Metagegevens van de gebruiker instellen

Na het uitvoeren van de scripts in de vorige secties, kunt u de [Set AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) cmdlet bij te werken van de eigenschappen van een consumergroep, zoals in het volgende voorbeeld:

```powershell
# Set some user metadata on the CG
Write-Host "Setting the UserMetadata field to 'Testing'"
Set-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -UserMetadata "Testing"
# Show result
Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
```

## <a name="remove-event-hub"></a>Verwijderen van de event hub

Als u wilt verwijderen van de event-hubs die u hebt gemaakt, kunt u de `Remove-*` cmdlets, zoals in het volgende voorbeeld:

```powershell
# Clean up
Remove-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
Remove-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
Remove-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
```

## <a name="next-steps"></a>Volgende stappen

- Raadpleeg de documentatie voor volledige Event Hubs Resource Manager PowerShell-module bij [hier](/powershell/module/azurerm.eventhub). Deze pagina worden alle beschikbare cmdlets.
- Zie het artikel voor informatie over het gebruik van Azure Resource Manager-sjablonen [maken van een naamruimte Event Hubs met event hub- en groep met een Azure Resource Manager-sjabloon](event-hubs-resource-manager-namespace-event-hub.md).
- Informatie over [Event Hubs .NET managementbibliotheken](event-hubs-management-libraries.md).

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
