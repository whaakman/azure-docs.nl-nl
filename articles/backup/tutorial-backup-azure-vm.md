---
title: Back-up op schaal maken van Azure-VM's
description: In deze zelfstudie wordt uitgebreid ingegaan op het maken van back-ups van virtuele machines van Azure voor een Recovery Services-kluis.
services: backup
author: rayne-wiselman
manager: carmonm
keywords: back-up van virtuele machine; back-up maken van virtuele machine; back-up en herstel na noodgeval
ms.service: backup
ms.topic: tutorial
ms.date: 09/06/2017
ms.author: trinadhk
ms.custom: mvc
ms.openlocfilehash: d2b83963f7af52101ed298e85b6c7fd64fc99a07
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52875583"
---
# <a name="back-up-azure-virtual-machines-in-azure-at-scale"></a>Back-up op schaal maken van virtuele machines van Azure

In deze zelfstudie wordt uitgebreid ingegaan hoe u back-ups maakt van virtuele machines van Azure voor een Recovery Services-kluis. Het meeste werk bij het maken van back-ups van virtuele machines bestaat uit de voorbereiding. Voordat u een back-up kunt maken van een virtuele machine (of als u deze wilt beveiligen) moet u de [vereisten](backup-azure-arm-vms-prepare.md) voltooien voor het voorbereiden van uw omgeving voor het beveiligen van uw VM's. 

> [!IMPORTANT]
> In deze zelfstudie wordt ervan uitgegaan dat u al een resourcegroep en een virtuele machine van Azure hebt gemaakt.

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

Een [Recovery Services-kluis](backup-azure-recovery-services-vault-overview.md) is een container met de herstelpunten voor de items waarvan u een back-up wilt maken. Een Recovery Services-kluis is een Azure-resource die kan worden geïmplementeerd en beheerd als onderdeel van een Azure-resourcegroep. In deze zelfstudie kunt u een Recovery Services-kluis maken in dezelfde resourcegroep als de virtuele machine die wordt beveiligd.


De eerste keer dat u Azure Backup gebruikt, moet u de Azure Recovery Service-provider voor uw abonnement registreren. Als u de provider al voor uw abonnement hebt geregistreerd, gaat u naar de volgende stap.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices
```

Maak een Recovery Services-kluis met **New-AzureRmRecoveryServicesVault**. Geef de naam van de resourcegroep op en de locatie die u gebruikt bij het configureren van de virtuele machine waarvan u een back-up wilt maken. 

```powershell
New-AzureRmRecoveryServicesVault -Name myRSvault -ResourceGroupName "myResourceGroup" -Location "EastUS"
```

Voor veel Azure Backup-cmdlets is het object Recovery Services-kluis als invoer vereist. Daarom is het handiger het object Backup Recovery Services-kluis in een variabele op te slaan. Gebruik vervolgens **Set-AzureRmRecoveryServicesBackupProperties** om de optie **-BackupStorageRedundancy** in te stellen op [geografisch redundante opslag (GRS)](../storage/common/storage-redundancy-grs.md). 

```powershell
$vault1 = Get-AzureRmRecoveryServicesVault –Name myRSVault
Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
```

## <a name="back-up-azure-virtual-machines"></a>Back-ups maken van virtuele Azure-machines

Stel de context van de kluis in voordat u de initiële back-up gaat uitvoeren. De context van de kluis is het type gegevens dat in de kluis wordt beveiligd. Als u een Recovery Services-kluis maakt, gaat deze gepaard met standaardbeleid voor beveiliging en retentie. Volgens het standaardbeveiligingsbeleid wordt elke dag op een bepaald tijdstip een back-uptaak getriggerd. Volgens het standaardbewaarbeleid wordt het dagelijkse herstelpunt gedurende dertig dagen bewaard. Accepteer het standaardbeleid voor deze zelfstudie. 

Gebruik **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** om de context van de kluis in te stellen. Zodra deze is ingesteld, is deze op alle navolgende cmdlets van toepassing. 

```powershell
Get-AzureRmRecoveryServicesVault -Name myRSVault | Set-AzureRmRecoveryServicesVaultContext
```

Gebruik **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)** om de back-uptaak te triggeren. Met de back-uptaak wordt een herstelpunt gemaakt. Als het om de initiële back-up gaat, bestaat het herstelpunt uit de volledige back-up. Navolgende back-ups bestaan uit kopieën met een toevoeging.

```powershell
$namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered -FriendlyName "V2VM"
$item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType AzureVM
$job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
```

## <a name="delete-the-recovery-services-vault"></a>Recovery Services-kluis verwijderen

Als u een Recovery Services-kluis wilt verwijderen, moet u eerst herstelpunten in de kluis verwijderen en vervolgens de registratie van de kluis ongedaan maken. Met de volgende opdrachten worden deze stappen in detail uiteengezet. 


```powershell
$Cont = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered
$PI = Get-AzureRmRecoveryServicesBackupItem -Container $Cont[0] -WorkloadType AzureVm
Disable-AzureRmRecoveryServicesBackupProtection -RemoveRecoveryPoints $PI[0]
Unregister-AzureRmRecoveryServicesBackupContainer -Container $namedContainer
Remove-AzureRmRecoveryServicesVault -Vault $vault1
```

## <a name="troubleshooting-errors"></a>Fouten oplossen
Zie [Problemen oplossen met back-ups van virtuele Azure-machines](backup-azure-vms-troubleshoot.md) als er tijdens het maken van back-ups van virtuele machines fouten optreden.

## <a name="next-steps"></a>Volgende stappen
U hebt uw virtuele machines beveiligd. Zie de volgende artikelen voor informatie over beheertaken en hoe u virtuele machines vanuit een herstelpunt kunt herstellen.

* Zie [AzureRM.RecoveryServices.Backup-cmdlets gebruiken om back-ups te maken van virtuele machines](backup-azure-vms-automation.md#create-a-protection-policy) om het back-upbeleid te wijzigen.
* [Uw virtuele machines beheren en controleren](backup-azure-manage-vms.md)
* [Virtuele machines herstellen](backup-azure-arm-restore-vms.md)
