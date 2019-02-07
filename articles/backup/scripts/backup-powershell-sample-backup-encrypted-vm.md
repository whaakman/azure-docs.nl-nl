---
title: Voorbeeld van Azure PowerShell-script - Een back-up maken van een virtuele Azure-machine | Microsoft Docs
description: Voorbeeld van Azure PowerShell-script - Een back-up maken van een virtuele Azure-machine
services: backup
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: sample
ms.date: 01/31/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 915c5f6c8e8de1b5a7a7590ba41125cbff7b8f36
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55497635"
---
# <a name="back-up-an-encrypted-azure-virtual-machine-with-powershell"></a>Back-up van een versleutelde virtuele Azure-machine maken met PowerShell

Met dit script wordt een Recovery Services-kluis met geografisch redundante opslag (GRS) gemaakt voor een versleutelde virtuele Azure-machine. Het standaardbeleid voor beveiliging wordt toegepast op de kluis. Met dit beleid wordt er iedere dag een back-up gemaakt van de virtuele machine. Alle back-ups worden gedurende 30 dagen bewaard. Het script triggert ook het eerste herstelpunt voor de virtuele machine en handhaaft dat herstelpunt gedurende 365 dagen. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/backup/backup-encrypted-vm/backup-encrypted-vm.ps1 "Back up encrypted virtual machine")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om de implementatie te maken. Elk item in de tabel is een koppeling naar specifieke documentatie over de opdracht.


| Opdracht | Opmerkingen | 
|---|---| 
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. | 
| [New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/New-AzureRmRecoveryServicesVault) | Hiermee maakt u een Recovery Services-kluis voor het opslaan van back-ups. | 
| [Set-AzureRmRecoveryServicesBackupProperties](/powershell/module/azurerm.recoveryservices/Set-AzureRmRecoveryServicesBackupProperties) | Hiermee stelt u eigenschappen in voor de opslag van back-ups in een Recovery Services-kluis. | 
| [New-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)| Hiermee maakt u een beveiligingsbeleid op basis van het planningsbeleid en bewaarbeleid voor de Recovery Services-kluis. | 
| [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) | Hiermee stelt u de machtigingen in voor de sleutelkluis om de service-principal toegang te bieden tot versleutelingssleutels. | 
| [Enable-AzureRmRecoveryServicesBackupProtection](/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection) | Hiermee kunt u back-up inschakelen voor een item met een opgegeven Backup-beveiligingsbeleid. | 
| [Set-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy)| Hiermee wijzigt u een bestaand Backup-beveiligingsbeleid. | 
| [Backup-AzureRmRecoveryServicesBackupItem](/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem) | Hiermee start u een back-up voor een beveiligd Azure Backup-item dat niet is gekoppeld aan het back-upschema. |
| [Wait-AzureRmRecoveryServicesBackupJob](/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob) | Hiermee wacht u totdat een taak van Azure Backup is voltooid. | 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep en alle daarin opgenomen resources. | 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

