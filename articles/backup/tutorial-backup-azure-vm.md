---
title: Maak een back-up van virtuele machines in Azure op schaal Azure | Microsoft Docs
description: Deze zelfstudie gegevens back-ups van meerdere virtuele machines van Azure naar een Recovery Services-kluis.
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: back-up van virtuele machine. back-up van virtuele machine; back-up en herstel na noodgevallen
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/06/2017
ms.author: trinadhk;jimpark;markgal;
ms.custom: mvc
ms.openlocfilehash: 01609c00c6f0585eff4843932b9eb7a090a59c19
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="back-up-azure-virtual-machines-in-azure-at-scale"></a>Back-up van Azure virtuele machines in Azure op schaal

Deze zelfstudie wordt het back-up van virtuele machines van Azure naar een Recovery Services-kluis. Het merendeel van het werk voor een back-ups van virtuele machines is de voorbereiding. Voordat u kunt back-up (of beveiligen) op een virtuele machine, moet u de [vereisten](backup-azure-arm-vms-prepare.md) voorbereiden van uw omgeving voor het beveiligen van uw virtuele machines. 

> [!IMPORTANT]
> Deze zelfstudie wordt ervan uitgegaan dat u al een resourcegroep en Azure een virtuele machine hebt gemaakt.

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

Een [Recovery Services-kluis](backup-azure-recovery-services-vault-overview.md) is een container met de herstelpunten voor de items die back-up wordt gemaakt. Een Recovery Services-kluis is een Azure-resource die kan worden geïmplementeerd en beheerd als onderdeel van een Azure-resourcegroep. In deze zelfstudie maakt u een Recovery Services-kluis in dezelfde resourcegroep bevinden als de virtuele machine wordt beveiligd.


De eerste keer dat u Azure Backup gebruiken, moet u de Azure Recovery Service provider registreren bij uw abonnement. Als u de provider bij uw abonnement al hebt geregistreerd, gaat u naar de volgende stap.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices
```

Maak een Recovery Services-kluis met **New-AzureRmRecoveryServicesVault**. Zorg ervoor dat de naam van een resourcegroep en locatie die wordt gebruikt bij het configureren van de virtuele machine die u back wilt-up opgeven. 

```powershell
New-AzureRmRecoveryServicesVault -Name myRSvault -ResourceGroupName "myResourceGroup" -Location "EastUS"
```

Veel Azure Backup-cmdlets moet de Recovery Services-kluis-object als invoer. Daarom is het handig zijn voor het opslaan van het back-up Recovery Services-kluis-object in een variabele. Gebruik vervolgens **Set AzureRmRecoveryServicesBackupProperties** om in te stellen de **- BackupStorageRedundancy** optie naar [geografisch redundante opslag (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage). 

```powershell
$vault1 = Get-AzureRmRecoveryServicesVault –Name myRSVault
Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
```

## <a name="back-up-azure-virtual-machines"></a>Back-ups maken van virtuele Azure-machines

Voordat u de eerste back-up uitvoeren kunt, moet u de context van de kluis instellen. De context van de kluis is het type gegevens dat is beveiligd in de kluis. Als u een Recovery Services-kluis maakt, beschikt u over de standaardbeveiliging en bewaarbeleid. Het standaardbeleid voor beveiliging een back-uptaak elke dag op de opgegeven tijd wordt geactiveerd. Het bewaarbeleid standaard wordt het dagelijkse herstelpunt voor 30 dagen bewaard. Accepteer het standaardbeleid voor deze zelfstudie. 

Gebruik  **[Set AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  om in te stellen de context van de kluis. Zodra de kluis context is ingesteld, wordt het toegepast op alle latere cmdlets. 

```powershell
Get-AzureRmRecoveryServicesVault -Name myRSVault | Set-AzureRmRecoveryServicesVaultContext
```

Gebruik  **[back-up AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)**  voor het activeren van de back-uptaak. De back-uptaak maakt een herstelpunt. Als dit de eerste back-up, is het herstelpunt dat een volledige back-up. Volgende back-ups maken van een incrementele kopie.

```powershell
$namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered -FriendlyName "V2VM"
$item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType AzureVM
$job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
```

## <a name="delete-the-recovery-services-vault"></a>Verwijder de Recovery Services-kluis

Als u wilt verwijderen een Recovery Services-kluis, moet u eerst verwijderen van herstelpunten in de kluis en vervolgens Hef de registratie van de kluis. De volgende opdrachten worden deze stappen in detail beschreven. 


```powershell
$Cont = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered
$PI = Get-AzureRmRecoveryServicesBackupItem -Container $Cont[0] -WorkloadType AzureVm
Disable-AzureRmRecoveryServicesBackupProtection -RemoveRecoveryPoints $PI[0]
Unregister-AzureRmRecoveryServicesBackupContainer -Container $namedContainer
Remove-AzureRmRecoveryServicesVault -Vault $vault1
```

## <a name="troubleshooting-errors"></a>Het oplossen van problemen
Als u problemen tijdens het back-ups maken van uw virtuele machine, Zie de [Back-up van virtuele machines in Azure probleemoplossing artikel](backup-azure-vms-troubleshoot.md) voor hulp.

## <a name="next-steps"></a>Volgende stappen
Nu u uw virtuele machines hebt beveiligd, gaat u naar de volgende artikelen voor meer informatie over beheertaken en het herstellen van virtuele machines vanaf een herstelpunt.

* Zie het wijzigen van de back-upbeleid [gebruik AzureRM.RecoveryServices.Backup-cmdlets voor back-up van virtuele machines](backup-azure-vms-automation.md#create-a-protection-policy).
* [Uw virtuele machines beheren en controleren](backup-azure-manage-vms.md)
* [Virtuele machines herstellen](backup-azure-arm-restore-vms.md)
