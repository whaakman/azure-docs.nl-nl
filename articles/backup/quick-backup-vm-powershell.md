---
title: Azure-quickstart - Een back-up van een VM maken met PowerShell
description: Lees hoe u een back-up van virtuele machines maakt via Azure PowerShell
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 03/05/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: aa637571ca11ea294b1f95df49855d7ee81b3001
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58258867"
---
# <a name="back-up-a-virtual-machine-in-azure-with-powershell"></a>Een back-up van een virtuele machine maken in Azure met PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

De [Azure PowerShell AZ](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.4.0) module wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. 

[Azure Backup](backup-overview.md) back-ups van on-premises computers en -apps en virtuele Azure-machines. Dit artikel ziet u hoe u back-up van een Azure-VM met de AZ-module. U kunt ook u kunt back-up van een virtuele machine met de [Azure CLI](quick-backup-vm-cli.md), of in de [Azure-portal](quick-backup-vm-portal.md).

Deze quickstart is bedoeld voor een back-up van een bestaande VM in Azure. Als u nog een VM wilt maken, kan dat ook [met Azure PowerShell](../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json).

In deze snelstartgids AZ moduleversie 1.0.0 van Azure PowerShell vereist of hoger. Voer ` Get-Module -ListAvailable Az` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps).


## <a name="log-in-and-register"></a>Aanmelden en registreren

1. Meld u aan bij uw Azure-abonnement met de opdracht `Connect-AzAccount` en volg de instructies op het scherm.

    ```powershell
    Connect-AzAccount
    ```
2. De eerste keer dat u Azure Backup gebruikt, moet u de provider Azure Recovery Service registreren in uw abonnement met [registreren AzResourceProvider](/powershell/module/az.Resources/Register-azResourceProvider), als volgt:

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```


## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

Een [Recovery Services-kluis](backup-azure-recovery-services-vault-overview.md) is een logische container waarin de back-upgegevens voor beveiligde bronnen, zoals virtuele Azure-machines. Wanneer een back-uptaak wordt uitgevoerd, maakt een herstelpunt in de Recovery Services-kluis. U kunt vervolgens een van deze herstelpunten gebruiken om gegevens voor dat tijdstip te herstellen.

Wanneer u de kluis maken:

- Geef de resourcegroep en locatie van de virtuele machine die u back wilt-up voor de resourcegroep en locatie.
- Als u dit gebruikt [voorbeeldscript](../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) voor het maken van de virtuele machine, de resourcegroep is **myResourceGroup**, de virtuele machine is ***myVM**, de resources bevinden zich de **Europa West**  regio.
- Azure Backup verwerkt automatisch de opslag voor back-ups van gegevens. De kluis maakt standaard gebruik [geografisch redundante opslag (GRS)](../storage/common/storage-redundancy-grs.md). Geo-redundantie zorgt ervoor dat back-ups van gegevens naar een secundaire Azure-regio grote afstand van de primaire regio worden gerepliceerd.

Nu een kluis maken.


1. Gebruik de [New-AzRecoveryServicesVault](/powershell/module/az.recoveryservices/new-azrecoveryservicesvault)om de kluis te maken:

    ```powershell
    New-AzRecoveryServicesVault `
        -ResourceGroupName "myResourceGroup" `
        -Name "myRecoveryServicesVault" `
    -Location "WestEurope"
    ```

2. Stel de context van de kluis met [Set AzRecoveryServicesVaultContext](/powershell/module/az.RecoveryServices/Set-azRecoveryServicesVaultContext), als volgt:

    ```powershell
    Get-AzRecoveryServicesVault `
        -Name "myRecoveryServicesVault" | Set-AzRecoveryServicesVaultContext
    ```


## <a name="enable-backup-for-an-azure-vm"></a>Back-up voor een virtuele Azure-machine inschakelen

U back-up inschakelen voor een Azure-VM en geef een back-upbeleid.

- Het beleid wordt gedefinieerd wanneer back-ups uitgevoerd, en hoe lang de herstelpunten die zijn gemaakt door de back-ups moeten worden bewaard.
- Volgens het standaardbeveiligingsbeleid wordt een back-up eenmaal per dag uitgevoerd voor de virtuele machine en worden de gemaakte herstelpunten gedurende 30 dagen bewaard. Dit standaardbeleid kunt u snel uw VM te beveiligen. 

Back-up als volgt inschakelen:

1. Stel eerst het standaardbeleid met [Get-AzRecoveryServicesBackupProtectionPolicy](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy):

    ```powershell
    $policy = Get-AzRecoveryServicesBackupProtectionPolicy     -Name "DefaultPolicy"
    ```

2. Back-up van virtuele machine met inschakelen [inschakelen AzRecoveryServicesBackupProtection](/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection). Geef het beleid, de resourcegroep en de naam van de virtuele machine.

    ```powershell
    Enable-AzRecoveryServicesBackupProtection `
        -ResourceGroupName "myResourceGroup" `
        -Name "myVM" `
        -Policy $policy
    ```


## <a name="start-a-backup-job"></a>Een back-uptaak starten

Back-ups uitgevoerd volgens de planning die is opgegeven in de back-upbeleid. U kunt ook een ad-hoc back-up uitvoeren:

- De eerste eerste back-uptaak wordt een volledig herstelpunt gemaakt.
- Na de eerste back-up van elke back-uptaak worden incrementele herstelpunten gemaakt.
- Incrementele herstelpunten zijn efficiënt qua opslag en tijd aangezien ze alleen wijzigingen bevatten die sinds de laatste back-up zijn doorgevoerd.

Als u wilt uitvoeren van een ad-hoc back-up, gebruikt u de[back-up-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupitem). 
- Geeft u een container in de kluis waarin de back-upgegevens met [Get-AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupcontainer).
- Elke VM voor back-up wordt als een item beschouwd. Voor het starten van een back-uptaak u informatie verkrijgen over de virtuele machine met [Get-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupitem).

Voer een ad-hoc back-uptaak als volgt uit:

1. Geeft de container, VM-informatie verkrijgen en de back-up uitvoeren.

    ```powershell
    $backupcontainer = Get-AzRecoveryServicesBackupContainer `
        -ContainerType "AzureVM" `
        -FriendlyName "myVM"

    $item = Get-AzRecoveryServicesBackupItem `
        -Container $backupcontainer `
        -WorkloadType "AzureVM"

    Backup-AzRecoveryServicesBackupItem -Item $item
    ```

2. Mogelijk moet u wachten tot 20 minuten sinds de eerste back-uptaak een volledig herstelpunt wordt gemaakt. De taak bewaakt, zoals beschreven in de volgende procedure.


## <a name="monitor-the-backup-job"></a>Uitvoering van back-uptaak volgen

1. Voer [Get-AzRecoveryservicesBackupJob](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob) voor het bewaken van de taak de status.

    ```powershell
    Get-AzRecoveryservicesBackupJob
    ```
    Uitvoer is vergelijkbaar met het volgende voorbeeld, waarin de taak als **InProgress**:

    ```
    WorkloadName   Operation         Status       StartTime              EndTime                JobID
    ------------   ---------         ------       ---------              -------                -----
    myvm           Backup            InProgress   9/18/2017 9:38:02 PM                          9f9e8f14
    myvm           ConfigureBackup   Completed    9/18/2017 9:33:18 PM   9/18/2017 9:33:51 PM   fe79c739
    ```

2. Wanneer de taak de status is **voltooid**en de virtuele machine is beveiligd met een volledig herstelpunt opgeslagen.


## <a name="clean-up-the-deployment"></a>Opschonen van de implementatie

Als u niet meer nodig voor back-up van de virtuele machine, kunt u het opschonen.
- Als u wilt voor het uitproberen van het herstellen van de virtuele machine, gaat u het opschonen van.
- Als u een bestaande VM hebt gebruikt, kunt u de laatste overslaan [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) cmdlet de resourcegroep en VM intact laten.

Schakel de beveiliging, verwijdert u de herstelpunten en de kluis. U de resourcegroep en de bijbehorende VM-resources als volgt verwijderen:

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
