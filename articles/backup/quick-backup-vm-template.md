---
title: 'Azure-Snelstart: Maak een Back-up van een virtuele machine met Resource Manager-sjabloon'
description: Meer informatie over het back-up van uw virtuele machines met Azure Resource Manager-sjabloon
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 05/14/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: b144d7509562b8ca0bca6299caee4a7ce292f4a6
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481367"
---
# <a name="back-up-a-virtual-machine-in-azure-with-resource-manager-template"></a>Back-up van een virtuele machine in Azure met Resource Manager-sjabloon

[Azure Backup](backup-overview.md) back-ups van on-premises computers en -apps en virtuele Azure-machines. Dit artikel ziet u hoe u back-up van een Azure VM met Resource Manager-sjabloon en Azure PowerShell. In deze snelstart richt zich op het proces voor het implementeren van een Resource Manager-sjabloon voor het maken van een herstellen Services-kluis. Zie voor meer informatie over het ontwikkelen van Resource Manager-sjablonen [Resource Manager-documentatie](/azure/azure-resource-manager/) en de [sjabloonverwijzing](/azure/templates/microsoft.recoveryservices/allversions).

U kunt ook een back-up een VM met [Azure PowerShell](./quick-backup-vm-powershell.md), wordt de [Azure CLI](quick-backup-vm-cli.md), of in de [Azure-portal](quick-backup-vm-portal.md).

## <a name="create-a-vm-and-recovery-services-vault"></a>Een virtuele machine maken en Recovery Services-kluis

Een [Recovery Services-kluis](backup-azure-recovery-services-vault-overview.md) is een logische container waarin de back-upgegevens voor beveiligde bronnen, zoals virtuele Azure-machines. Wanneer een back-uptaak wordt uitgevoerd, maakt een herstelpunt in de Recovery Services-kluis. U kunt vervolgens een van deze herstelpunten gebruiken om gegevens voor dat tijdstip te herstellen.

De sjabloon die wordt gebruikt in deze Quick Start is afkomstig van [Azure-snelstartsjablonen](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/). Deze sjabloon kunt u eenvoudige Windows-VM en Recovery Services-kluis is geconfigureerd met de DefaultPolicy voor beveiliging implementeren.

Als u wilt implementeren de sjabloon, selecteert u **uitproberen** Azure Cloud shell openen en plak het volgende PowerShell-script in de shell-venster. Als u de code, met de rechtermuisknop op de shell-venster en selecteer vervolgens **plakken**.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name (limited to eight characters) that is used to generate Azure resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the administrator username for the virtual machine"
$adminPassword = Read-Host -Prompt "Enter the administrator password for the virtual machine" -AsSecureString
$dnsPrefix = Read-Host -Prompt "Enter the unique DNS Name for the Public IP used to access the virtual machine"

$resourceGroupName = "${projectName}rg"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-recovery-services-create-vm-and-configure-backup/azuredeploy.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName -adminUsername $adminUsername -adminPassword $adminPassword -dnsLabelPrefix $dnsPrefix
```

Azure PowerShell wordt gebruikt voor het implementeren van de Resource Manager-sjabloon in deze Quick Start. De [Azure-portal](../azure-resource-manager/resource-group-template-deploy-portal.md), [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md), en [Rest-API](../azure-resource-manager/resource-group-template-deploy-rest.md) kan ook worden gebruikt om sjablonen te implementeren.

## <a name="start-a-backup-job"></a>Een back-uptaak starten

De sjabloon maakt een virtuele machine en schakelt terug op de virtuele machine. Nadat u de sjabloon hebt geïmplementeerd, moet u een back-uptaak te starten. Zie voor meer informatie, [starten van een back-uptaak](./quick-backup-vm-powershell.md#start-a-backup-job).

## <a name="monitor-the-backup-job"></a>Uitvoering van back-uptaak volgen

Zie voor het controleren van de back-uptaak [bewaken van de back-uptaak](./quick-backup-vm-powershell.md#monitor-the-backup-job).

## <a name="clean-up-the-deployment"></a>Opschonen van de implementatie

Als u niet meer nodig voor back-up van de virtuele machine, kunt u het opschonen.

- Als u wilt voor het uitproberen van het herstellen van de virtuele machine, gaat u het opschonen van.
- Als u een bestaande VM hebt gebruikt, kunt u de laatste overslaan [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) cmdlet de resourcegroep en VM intact laten.

Schakel de beveiliging, verwijdert u de herstelpunten en de kluis. Verwijder de resourcegroep en de bijbehorende VM-resources als volgt:

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $item -RemoveRecoveryPoints
$vault = Get-AzRecoveryServicesVault -Name "myRecoveryServicesVault"
Remove-AzRecoveryServicesVault -Vault $vault
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een Recovery Services-kluis gemaakt, de beveiliging op een VM ingeschakeld en het eerste herstelpunt gemaakt.

- [Informatie over hoe](tutorial-backup-vm-at-scale.md) naar back-up van virtuele machines in Azure portal.
- [Informatie over hoe](tutorial-restore-disk.md) snel een virtuele machine terugzetten
