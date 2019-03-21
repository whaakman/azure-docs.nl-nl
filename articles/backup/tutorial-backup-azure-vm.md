---
title: Back-up van meerdere Azure-virtuele machines met PowerShell
description: Deze zelfstudie vindt u back-ups van meerdere virtuele machines van Azure naar een Recovery Services-kluis met behulp van Azure PowerShell.
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: tutorial
ms.date: 03/05/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 85e5fc7e1c8a4561b51afaf0d665fedb6d9cde1f
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58258374"
---
# <a name="back-up-azure-vms-with-powershell"></a>Back-up van virtuele Azure-machines met PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Deze zelfstudie wordt beschreven hoe u implementeert een [Azure Backup](backup-overview.md) Recovery Services-kluis back-ups meerdere Azure-VM's met behulp van PowerShell.  

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een Recovery Services-kluis en de kluiscontext instellen.
> * Een back-upbeleid definiëren
> * Het back-upbeleid toepassen voor de beveiliging van meerdere virtuele machines
> * Trigger een on-demand back-uptaak voor de beveiligde virtuele machines voordat u kunt een back-up (of Bescherm) een virtuele machine, moet u uitvoeren de [vereisten](backup-azure-arm-vms-prepare.md) om voor te bereiden uw omgeving voor het beveiligen van uw virtuele machines. 

> [!IMPORTANT]
> In deze zelfstudie wordt ervan uitgegaan dat u al een resourcegroep en een virtuele machine van Azure hebt gemaakt.


## <a name="log-in-and-register"></a>Aanmelden en registreren


1. Meld u aan bij uw Azure-abonnement met de opdracht `Connect-AzAccount` en volg de instructies op het scherm.

    ```powershell
    Connect-AzAccount
    ```
2. De eerste keer dat u Azure Backup gebruikt, moet u de provider Azure Recovery Service registreren in uw abonnement met [registreren AzResourceProvider](/powershell/module/az.Resources/Register-azResourceProvider). Als u al hebt geregistreerd, moet u deze stap overslaan.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```


## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

Een [Recovery Services-kluis](backup-azure-recovery-services-vault-overview.md) is een logische container waarin de back-upgegevens voor beveiligde bronnen, zoals virtuele Azure-machines. Wanneer een back-uptaak wordt uitgevoerd, maakt een herstelpunt in de Recovery Services-kluis. U kunt vervolgens een van deze herstelpunten gebruiken om gegevens voor dat tijdstip te herstellen.


- In deze zelfstudie maakt u de kluis in dezelfde resourcegroep en locatie als de virtuele machine die u back wilt-up.
- Azure Backup verwerkt automatisch de opslag voor back-ups van gegevens. De kluis maakt standaard gebruik [geografisch redundante opslag (GRS)](../storage/common/storage-redundancy-grs.md). Geo-redundantie zorgt ervoor dat back-ups van gegevens naar een secundaire Azure-regio grote afstand van de primaire regio worden gerepliceerd.

Maak de kluis als volgt:

1. Gebruik de [New-AzRecoveryServicesVault](/powershell/module/az.recoveryservices/new-azrecoveryservicesvault)om de kluis te maken. Geef de naam van de resourcegroep en locatie van de virtuele machine die u back wilt-up.

    ```powershell
    New-AzRecoveryServicesVault -Name myRSvault -ResourceGroupName "myResourceGroup" -Location "EastUS"
    ```
2. Voor veel Azure Backup-cmdlets is het object Recovery Services-kluis als invoer vereist. Daarom is het handiger het object Backup Recovery Services-kluis in een variabele op te slaan.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault –Name myRSVault
    ```
    
3. Stel de context van de kluis met [Set AzRecoveryServicesVaultContext](/powershell/module/az.RecoveryServices/Set-azRecoveryServicesVaultContext).

   - De context van de kluis is het type gegevens dat in de kluis wordt beveiligd.
   - Zodra de context is ingesteld, wordt toegepast op alle navolgende cmdlets

     ```powershell
     Get-AzRecoveryServicesVault -Name "myRSVault" | Set-AzRecoveryServicesVaultContext
     ```

## <a name="back-up-azure-vms"></a>Back-ups maken van Azure-VM's

Back-ups uitgevoerd volgens de planning die is opgegeven in de back-upbeleid. Als u een Recovery Services-kluis maakt, gaat deze gepaard met standaardbeleid voor beveiliging en retentie.

- Volgens het standaardbeveiligingsbeleid wordt geactiveerd voor een back-uptaak een per dag op een bepaald tijdstip.
- Volgens het standaardbewaarbeleid wordt het dagelijkse herstelpunt gedurende dertig dagen bewaard. 

Als u wilt inschakelen en back-up maken van de Azure-VM in deze zelfstudie, doen we het volgende:

1. Geef een container in de kluis waarin de back-upgegevens met [Get-AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices/get-Azrecoveryservicesbackupcontainer).
2. Elke virtuele machine voor back-up is een item. Voor het starten van een back-uptaak u informatie verkrijgen over de virtuele machine met [Get-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupItem).
3. Uitvoeren van een ad-hoc back-up met[back-up-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/backup-Azrecoveryservicesbackupitem). 
    - De eerste eerste back-uptaak wordt een volledig herstelpunt gemaakt.
    - Na de eerste back-up van elke back-uptaak worden incrementele herstelpunten gemaakt.
    - Incrementele herstelpunten zijn efficiënt qua opslag en tijd aangezien ze alleen wijzigingen bevatten die sinds de laatste back-up zijn doorgevoerd.

Inschakelen en voer de back-up als volgt uit:

```powershell
$namedContainer = Get-AzRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered -FriendlyName "V2VM"
$item = Get-AzRecoveryServicesBackupItem -Container $namedContainer -WorkloadType AzureVM
$job = Backup-AzRecoveryServicesBackupItem -Item $item
```

## <a name="troubleshooting"></a>Problemen oplossen 

Als u problemen tijdens het back-ups van virtuele machines, bekijkt u deze [probleemoplossingsartikel](backup-azure-vms-troubleshoot.md).

### <a name="deleting-a-recovery-services-vault"></a>Een Recovery Services-kluis verwijderen

Als u een kluis verwijderen wilt, verwijdert u eerst herstelpunten in de kluis en vervolgens registratie van de kluis, als volgt:


```powershell
$Cont = Get-AzRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered
$PI = Get-AzRecoveryServicesBackupItem -Container $Cont[0] -WorkloadType AzureVm
Disable-AzRecoveryServicesBackupProtection -RemoveRecoveryPoints $PI[0]
Unregister-AzRecoveryServicesBackupContainer -Container $namedContainer
Remove-AzRecoveryServicesVault -Vault $vault1
```

## <a name="next-steps"></a>Volgende stappen

- [Beoordeling](backup-azure-vms-automation.md) een meer gedetailleerd overzicht van back-up en herstellen van virtuele Azure-machines met PowerShell. 
- [Beheren en controleren van virtuele Azure-machines](backup-azure-manage-vms.md)
- [Azure virtuele machines herstellen](backup-azure-arm-restore-vms.md)
