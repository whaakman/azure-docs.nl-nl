---
title: Azure PowerShell-Script voorbeeld - Maak een Back-up van een virtuele machine in Azure | Microsoft Docs
description: Azure PowerShell-Script voorbeeld - Maak een Back-up van een virtuele machine van Azure
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
tags: 
ms.assetid: 
ms.service: backup
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/07/2017
ms.author: markgal
ms.custom: mvc
ms.openlocfilehash: 4376add4a2e51806bd5db228ad2fe2afcf2e4f57
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="back-up-an-encrypted-azure-virtual-machine-with-powershell"></a>Back-up van een versleutelde Azure virtuele machine met PowerShell

Dit script maakt een Recovery Services-kluis met geografisch redundante opslag (GRS) voor een versleutelde Azure virtuele machine. Het standaardbeleid voor de beveiliging wordt toegepast op de kluis. Het beleid genereert een dagelijkse back-up voor de virtuele machine en behoudt elke back-up voor 30 dagen. Het script wordt ook het eerste herstelpunt voor de virtuele machine wordt geactiveerd en behoudt dat herstelpunt voor 365 dagen. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/backup/backup-encrypted-vm/backup-encrypted-vm.ps1 "Back up encrypted virtual machine")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten om de implementatie te maken. Elk item in de tabel is gekoppeld aan de specifieke documentatie opdracht.


| Opdracht | Opmerkingen | 
|---|---| 
| [Nieuwe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Maakt een resourcegroep waarin alle resources worden opgeslagen. | 
| [Nieuwe AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/New-AzureRmRecoveryServicesVault) | Hiermee maakt u een recovery services-kluis voor het opslaan van back-ups. | 
| [Set-AzureRmRecoveryServicesBackupProperties](/powershell/module/azurerm.recoveryservices/Set-AzureRmRecoveryServicesBackupProperties) | Sets back-up van de eigenschappen van de opslag op de Recovery Services-kluis. | 
| [Nieuwe AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)| Hiermee maakt protection-beleid met behulp van beleid voor planning en bewaarbeleid in Recovery Services-kluis. | 
| [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) | Stelt de machtigingen op de Sleutelkluis de service principal toegang verlenen tot versleutelingssleutels. | 
| [Schakel AzureRmRecoveryServicesBackupProtection](/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection) | Hiermee kunt back-up voor een item met een opgegeven back-up protection-beleid. | 
| [Set-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy)| Hiermee wijzigt u een bestaande back-up protection-beleid. | 
| [Back-up AzureRmRecoveryServicesBackupItem](/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem) | Hiermee start u een back-up voor een beveiligde Azure Backup-item dat is niet gekoppeld aan het back-upschema. |
| [Wacht AzureRmRecoveryServicesBackupJob](/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob) | Er wordt gewacht op een Azure backup-taak is voltooid. | 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep en alle resources binnen. | 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

