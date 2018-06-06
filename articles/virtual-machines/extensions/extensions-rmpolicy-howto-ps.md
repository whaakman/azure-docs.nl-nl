---
title: Azure-beleid gebruiken voor het beperken van de installatie van de VM-extensie | Microsoft Docs
description: Azure-beleid gebruiken om te beperken van extensie-implementaties.
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/23/2018
ms.author: danis;cynthn
ms.openlocfilehash: da5b0db997ba1aa0e998f6fe2645e955b490951d
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "33942681"
---
# <a name="use-azure-policy-to-restrict-extensions-installation-on-windows-vms"></a>Azure-beleid gebruiken voor het beperken van de installatie van de extensies op VM's van Windows

Als u voorkomen dat het gebruik of de installatie van bepaalde extensies op uw Windows-VM's wilt, kunt u een Azure-beleid met behulp van PowerShell om te beperken van uitbreidingen voor virtuele machines binnen een resourcegroep maken. 

Deze zelfstudie wordt Azure PowerShell vanuit de Cloud-Shell die voortdurend wordt bijgewerkt naar de nieuwste versie. Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie versie 3.6 of hoger van de Azure PowerShell-module vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). 

## <a name="create-a-rules-file"></a>Maak een regelbestand

Als u wilt beperken welke uitbreidingen kunnen worden geïnstalleerd, moet u beschikken over een [regel](/azure/azure-policy/policy-definition#policy-rule) voor de logica voor het identificeren van de extensie.

In dit voorbeeld ziet u hoe weigeren uitbreidingen die zijn gepubliceerd door 'Microsoft.Compute' door het maken van een regelbestand in de Azure-Cloud-Shell, maar als u in PowerShell lokaal werkt, u kunt ook maakt een lokaal bestand en wijzig het pad ($home/clouddrive) met het pad naar de lokale bestand op uw computer.

In een [Cloud Shell](https://shell.azure.com/powershell), type:

```azurepowershell-interactive
nano $home/clouddrive/rules.json
```

Kopieer en plak de volgende .json in het bestand.

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Compute/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                "equals": "Microsoft.Compute"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/type",
                "in": "[parameters('notAllowedExtensions')]"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

Wanneer u klaar bent, klik op de **Ctrl + O** en vervolgens **Enter** het bestand wilt opslaan. Klik op **Ctrl + X** te sluiten van het bestand en afsluiten.

## <a name="create-a-parameters-file"></a>Maak een parameterbestand

U moet ook een [parameters](/azure/azure-policy/policy-definition#parameters) -bestand dat wordt gemaakt van een structuur moet worden gebruikt voor het doorgeven in een lijst van de extensies te blokkeren. 

Dit voorbeeld ziet u hoe u een parameterbestand maken voor virtuele machines in de Cloud-Shell, maar als u in PowerShell lokaal werkt, u kunt ook maakt een lokaal bestand en wijzig het pad ($home/clouddrive) met het pad naar het lokale bestand op uw computer.

In [Cloud Shell](https://shell.azure.com/powershell), type:

```azurepowershell-interactive
nano $home/clouddrive/parameters.json
```

Kopieer en plak de volgende .json in het bestand.

```json
{
    "notAllowedExtensions": {
        "type": "Array",
        "metadata": {
            "description": "The list of extensions that will be denied.",
            "strongType": "type",
            "displayName": "Denied extension"
        }
    }
}
```

Wanneer u klaar bent, klik op de **Ctrl + O** en vervolgens **Enter** het bestand wilt opslaan. Klik op **Ctrl + X** te sluiten van het bestand en afsluiten.

## <a name="create-the-policy"></a>Het beleid maken

Een beleidsdefinitie is een object dat wordt gebruikt voor het opslaan van de configuratie die u wilt gebruiken. De beleidsdefinitie gebruikt de regels en de parameters-bestanden voor het definiëren van het beleid. Maak een beleid definitie met de [nieuw AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition) cmdlet.

 De parameters en beleidsregels zijn bestanden die u hebt gemaakt en opgeslagen als .json-bestanden in uw cloud-shell.


```azurepowershell-interactive
$definition = New-AzureRmPolicyDefinition `
   -Name "not-allowed-vmextension-windows" `
   -DisplayName "Not allowed VM Extensions" `
   -description "This policy governs which VM extensions that are explicitly denied."   `
   -Policy 'C:\Users\ContainerAdministrator\clouddrive\rules.json' `
   -Parameter 'C:\Users\ContainerAdministrator\clouddrive\parameters.json'
```




## <a name="assign-the-policy"></a>Wijs het beleid

In dit voorbeeld wordt het beleid toegewezen aan een resource-groep met [nieuw AzureRMPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment). Een virtuele machine gemaakt de **myResourceGroup** resourcegroep kan niet worden voor het installeren van de extensies VM-Agent voor toegang en aangepast Script. 

Gebruik de [Get-AzureRMSubscription | Tabel opmaken](/powershell/module/azurerm.profile/get-azurermsubscription) cmdlet ophalen van uw abonnements-ID in plaats van het certificaat in het voorbeeld.

```azurepowershell-interactive
$scope = "/subscriptions/<subscription id>/resourceGroups/myResourceGroup"
$assignment = New-AzureRMPolicyAssignment `
   -Name "not-allowed-vmextension-windows" `
   -Scope $scope `
   -PolicyDefinition $definition `
   -PolicyParameter '{
    "notAllowedExtensions": {
        "value": [
            "VMAccessAgent",
            "CustomScriptExtension"
        ]
    }
}'
$assignment
```

## <a name="test-the-policy"></a>Het beleid testen

Als u wilt testen van het beleid, proberen te gebruiken van de toegang van de VM-extensie. De volgende zou moeten mislukken met het bericht ' Set AzureRmVMAccessExtension: Resource 'myVMAccess' is niet toegestaan door het beleid. "

```azurepowershell-interactive
Set-AzureRmVMAccessExtension `
   -ResourceGroupName "myResourceGroup" `
   -VMName "myVM" `
   -Name "myVMAccess" `
   -Location EastUS 
```

In de portal niet wijzigen van het wachtwoord moet met de 'de sjabloonimplementatie is mislukt vanwege schending van het beleid." Bericht.

## <a name="remove-the-assignment"></a>De toewijzing verwijderen

```azurepowershell-interactive
Remove-AzureRMPolicyAssignment -Name not-allowed-vmextension-windows -Scope $scope
```

## <a name="remove-the-policy"></a>Het beleid verwijderen

```azurepowershell-interactive
Remove-AzureRmPolicyDefinition -Name not-allowed-vmextension-windows
```
    
## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie [Azure beleid](../../azure-policy/azure-policy-introduction.md).
