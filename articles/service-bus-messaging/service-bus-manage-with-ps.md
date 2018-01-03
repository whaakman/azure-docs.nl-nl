---
title: PowerShell gebruiken om Azure Service Bus-resources te beheren | Microsoft Docs
description: Gebruik PowerShell-module maken en beheren van Service Bus-resources
services: service-bus-messaging
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/21/2017
ms.author: sethm
ms.openlocfilehash: 4fbc6e18565ec14a3ccb4499b24804f681026023
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="use-powershell-to-manage-service-bus-resources"></a>PowerShell gebruiken om Service Bus-resources beheren

Microsoft Azure PowerShell is een scriptomgeving op servers die u gebruiken kunt om te beheren en de implementatie en beheer van Azure services automatiseren. In dit artikel wordt beschreven hoe u de [Service Bus-Resource Manager PowerShell-module](/powershell/module/azurerm.servicebus) inrichten en beheren van Service Bus-entiteiten (naamruimten, wachtrijen, onderwerpen en abonnementen) met behulp van een lokale Azure PowerShell-console of het script.

U kunt ook beheren met behulp van Azure Resource Manager-sjablonen van Service Bus-entiteiten. Zie voor meer informatie het artikel [maken van Service Bus-resources met behulp van Azure Resource Manager-sjablonen](service-bus-resource-manager-overview.md).

## <a name="prerequisites"></a>Vereisten

Voordat u begint, moet u de volgende vereisten:

* Een Azure-abonnement. Zie voor meer informatie over het verkrijgen van een abonnement [aanschafopties][purchase options], [lid biedt][member offers], of [gratis account][free account].
* Een computer met Azure PowerShell. Zie voor instructies [aan de slag met Azure PowerShell-cmdlets](/powershell/azure/get-started-azureps).
* Een algemeen begrip van de PowerShell-scripts, NuGet-pakketten en .NET Framework.

## <a name="get-started"></a>Aan de slag

De eerste stap is het gebruik van PowerShell zich aanmelden bij uw Azure-account en de Azure-abonnement. Volg de instructies in [aan de slag met Azure PowerShell-cmdlets](/powershell/azure/get-started-azureps) voor aanmelden bij uw Azure-account en toegang te krijgen tot de resources in uw Azure-abonnement.

## <a name="provision-a-service-bus-namespace"></a>Inrichten van een Service Bus-naamruimte

Als u werkt met Service Bus-naamruimten, kunt u de [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace), [nieuw AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace), [verwijderen AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace), en [Set AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespace) cmdlets.

In dit voorbeeld maakt een paar lokale variabelen in het script; `$Namespace` en `$Location`.

* `$Namespace`is de naam van de Service Bus-naamruimte die we wilt werken.
* `$Location`identificeert het datacenter waarin we de naamruimte inrichten.
* `$CurrentNamespace`slaat de referentie-naamruimte die we ophalen (of maak).

In een werkelijke script `$Namespace` en `$Location` kunnen worden doorgegeven als parameters.

Dit deel van het script doet het volgende:

1. Probeert op te halen van een Service Bus-naamruimte met de opgegeven naam.
2. Als de naamruimte wordt gevonden, rapporteert wat is gevonden.
3. Als de naamruimte niet wordt gevonden, maakt u de naamruimte en vervolgens haalt de zojuist gemaakte naamruimte.
   
    ``` powershell
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
   
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Host "The namespace $Namespace already exists in the $Location region:"
        # Report what was found
        Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "The $Namespace namespace does not exist."
        Write-Host "Creating the $Namespace namespace in the $Location region..."
        New-AzureRmServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
        Write-Host "The $Namespace namespace in Resource Group $ResGrpName in the $Location region has been successfully created."
                
    }
    ```

### <a name="create-a-namespace-authorization-rule"></a>Een autorisatieregel naamruimte maken

Het volgende voorbeeld laat zien hoe voor het beheren van autorisatieregels naamruimte met behulp van de [nieuw AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespaceauthorizationrule), [Get-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespaceauthorizationrule), [Set AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespaceauthorizationrule), en [verwijderen AzureRmServiceBusNamespaceAuthorizationRule cmdlets](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespaceauthorizationrule).

```powershell
# Query to see if rule exists
$CurrentRule = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

# Check if the rule already exists or needs to be created
if ($CurrentRule)
{
    Write-Host "The $AuthRule rule already exists for the namespace $Namespace."
}
else
{
    Write-Host "The $AuthRule rule does not exist."
    Write-Host "Creating the $AuthRule rule for the $Namespace namespace..."
    New-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule -Rights @("Listen","Send")
    $CurrentRule = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
    Write-Host "The $AuthRule rule for the $Namespace namespace has been successfully created."

    Write-Host "Setting rights on the namespace"
    $authRuleObj = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

    Write-Host "Remove Send rights"
    $authRuleObj.Rights.Remove("Send")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Add Send and Manage rights to the namespace"
    $authRuleObj.Rights.Add("Send")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj
    $authRuleObj.Rights.Add("Manage")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Show value of primary key"
    $CurrentKey = Get-AzureRmServiceBusNamespaceKey -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
        
    Write-Host "Remove this authorization rule"
    Remove-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
}
```

## <a name="create-a-queue"></a>Een wachtrij maken

Voer een naamruimte-controle met het script in de vorige sectie voor het maken van een wachtrij of onderwerp. Maak vervolgens de wachtrij:

```powershell
# Check if queue already exists
$CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName

if($CurrentQ)
{
    Write-Host "The queue $QueueName already exists in the $Location region:"
}
else
{
    Write-Host "The $QueueName queue does not exist."
    Write-Host "Creating the $QueueName queue in the $Location region..."
    New-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -EnablePartitioning $True
    $CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName
    Write-Host "The $QueueName queue in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

### <a name="modify-queue-properties"></a>De wachtrijeigenschappen van de wijzigen

Na het uitvoeren van script in de vorige sectie, kunt u de [Set AzureRmServiceBusQueue](/powershell/module/azurerm.servicebus/set-azurermservicebusqueue) cmdlet bij te werken van de eigenschappen van een wachtrij, zoals in het volgende voorbeeld:

```powershell
$CurrentQ.DeadLetteringOnMessageExpiration = $True
$CurrentQ.MaxDeliveryCount = 7
$CurrentQ.MaxSizeInMegabytes = 2048
$CurrentQ.EnableExpress = $True

Set-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -QueueObj $CurrentQ
```

## <a name="provisioning-other-service-bus-entities"></a>Inrichting van andere Service Bus-entiteiten

U kunt de [Service Bus PowerShell-module](/powershell/module/azurerm.servicebus) om in te richten andere entiteiten, zoals onderwerpen en abonnementen. Deze cmdlets zijn syntaxis vergelijkbaar is met de wachtrij maken cmdlets in de vorige sectie wordt gedemonstreerd.

## <a name="next-steps"></a>Volgende stappen

- Zie de volledige documentatie van de Service Bus-Resource Manager PowerShell-module [hier](/powershell/module/azurerm.servicebus). Deze pagina worden alle beschikbare cmdlets.
- Zie het artikel voor informatie over het gebruik van Azure Resource Manager-sjablonen [maken van Service Bus-resources met behulp van Azure Resource Manager-sjablonen](service-bus-resource-manager-overview.md).
- Informatie over [management-Service Bus .NET-bibliotheken](service-bus-management-libraries.md).

Er zijn een aantal alternatieve methoden voor het beheren van Service Bus-entiteiten, zoals beschreven in deze blogberichten:

* [Het maken van Service Bus-wachtrijen, onderwerpen en abonnementen op basis van een PowerShell-script](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Het maken van een Service Bus-Namespace en een Event Hub met een PowerShell-script](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)
* [Service Bus PowerShell-Scripts](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
