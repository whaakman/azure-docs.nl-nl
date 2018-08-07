---
title: Met PowerShell back-ups implementeren en beheren voor door Resource Manager geïmplementeerde virtuele machines
description: PowerShell gebruiken om te implementeren en beheren van back-ups in Azure voor Resource Manager geïmplementeerde virtuele machines
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 8/06/2018
ms.author: markgal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1d8e2d3e6a303009f5718a86772cdc3db8ed332a
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39523849"
---
# <a name="use-azurermrecoveryservicesbackup-cmdlets-to-back-up-virtual-machines"></a>AzureRM.RecoveryServices.Backup-cmdlets gebruiken voor het back-up van virtuele machines

In dit artikel leest u hoe u Azure PowerShell-cmdlets voor back-up en herstel van een Azure virtuele machine (VM) van een Recovery Services-kluis. Een Recovery Services-kluis is een Azure Resource Manager-resource en wordt gebruikt voor het beveiligen van gegevens en assets in zowel Azure Backup en Azure Site Recovery-services. U kunt een Recovery Services-kluis gebruiken om virtuele machines die met Azure Service Manager en Azure Resource Manager geïmplementeerde virtuele machines te beschermen.

> [!NOTE]
> Azure heeft twee implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel is bedoeld voor gebruik met virtuele machines die zijn gemaakt met behulp van de Resource Manager-model.
>
>

In dit artikel begeleidt u bij het beveiligen van een virtuele machine en gegevens herstellen vanaf een herstelpunt met behulp van PowerShell.

## <a name="concepts"></a>Concepten
Als u niet bekend met de Azure Backup-service, voor een overzicht van de service bent, bekijk dan [wat is Azure Backup?](backup-introduction-to-azure-backup.md) Voordat u begint, zorg ervoor dat u de basisbeginselen over de vereisten die nodig zijn voor het werken met Azure Backup en de beperkingen van de huidige VM-back-upoplossing dekt.

Voor het gebruik van PowerShell effectief, is het nodig om inzicht in de hiërarchie van objecten en van waar u wilt beginnen.

![Recovery Services-objecthiërarchie](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Voor de naslaginformatie over AzureRm.RecoveryServices.Backup PowerShell-cmdlets, raadpleegt u de [Azure Backup - Recovery Services-Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) in de Azure-bibliotheek.

## <a name="setup-and-registration"></a>Installatie en registratie
Om te beginnen met:

1. [Download de nieuwste versie van PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (de minimaal vereiste versie is: 1.4.0)

2. De beschikbare Azure Backup PowerShell-cmdlets vinden door de volgende opdracht te typen:
    ```PS
    PS C:\> Get-Command *azurermrecoveryservices*
    CommandType     Name                                               Version    Source
    -----------     ----                                               -------    ------
    Cmdlet          Backup-AzureRmRecoveryServicesBackupItem           1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Disable-AzureRmRecoveryServicesBackupProtection    1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Enable-AzureRmRecoveryServicesBackupProtection     1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupContainer         1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupItem              1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupJob               1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupJobDetails        1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupManagementServer  1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupProperties        1.4.0      AzureRM.RecoveryServices
    Cmdlet          Get-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRMRecoveryServicesBackupRecoveryPoint     1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupRetentionPolic... 1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupSchedulePolicy... 1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesVault                   1.4.0      AzureRM.RecoveryServices
    Cmdlet          Get-AzureRmRecoveryServicesVaultSettingsFile       1.4.0      AzureRM.RecoveryServices
    Cmdlet          New-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          New-AzureRmRecoveryServicesVault                   1.4.0      AzureRM.RecoveryServices
    Cmdlet          Remove-AzureRmRecoveryServicesProtectionPolicy     1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Remove-AzureRmRecoveryServicesVault                1.4.0      AzureRM.RecoveryServices
    Cmdlet          Restore-AzureRMRecoveryServicesBackupItem          1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Set-AzureRmRecoveryServicesBackupProperties        1.4.0      AzureRM.RecoveryServices
    Cmdlet          Set-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Set-AzureRmRecoveryServicesVaultContext            1.4.0      AzureRM.RecoveryServices
    Cmdlet          Stop-AzureRmRecoveryServicesBackupJob              1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Unregister-AzureRmRecoveryServicesBackupContainer  1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Unregister-AzureRmRecoveryServicesBackupManagem... 1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Wait-AzureRmRecoveryServicesBackupJob              1.4.0      AzureRM.RecoveryServices.Backup
    ```
3. Aanmelden bij uw Azure-account met **Connect-AzureRmAccount**. Deze cmdlet wordt een webpagina vraagt u om referenties voor uw account: 
    - U kunt ook uw accountreferenties opnemen als een parameter in de **Connect-AzureRmAccount** cmdlet, met behulp van de **-referentie** parameter.
    - Als u de CSP-partner werken namens een tenant bent, geeft u de klant als een tenant met behulp van de naam van de primaire domeincontroller tenant-id of tenant. Bijvoorbeeld: **Connect-AzureRmAccount-Tenant "fabrikam.com"**
4. Koppel het abonnement dat u gebruiken met het account wilt, omdat een account kan meerdere abonnementen hebben:

    ```PS
    PS C:\> Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

5. Als u Azure Backup voor de eerste keer gebruikt, moet u de **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)** cmdlet voor het registreren van de Azure Recovery Service-provider met uw abonnement.

    ```PS
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. U kunt controleren of de Providers is geregistreerd met de volgende opdrachten:
    ```PS
    PS C:\> Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ``` 
In de uitvoer van de opdracht, de **RegistrationState** moet ingesteld op **geregistreerde**. Als dit niet het geval is, alleen opnieuw uitgevoerd de **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)** cmdlet hierboven wordt weergegeven.

De volgende taken kunnen worden geautomatiseerd met PowerShell:

* [Een Recovery Services-kluis maken](backup-azure-vms-automation.md#create-a-recovery-services-vault)
* [Back-ups maken van Azure-VM's](backup-azure-vms-automation.md#back-up-azure-vms)
* [Een back-uptaak activeert](backup-azure-vms-automation.md#trigger-a-backup-job)
* [Een back-uptaak controleren](backup-azure-vms-automation.md#monitoring-a-backup-job)
* [Een Azure-VM herstellen](backup-azure-vms-automation.md#restore-an-azure-vm)

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

De volgende stappen leiden u bij het maken van een Recovery Services-kluis. Een Recovery Services-kluis is anders dan een back-upkluis.

1. De Recovery Services-kluis is een Resource Manager-resource, dus u moet dit binnen een resourcegroep te plaatsen. U kunt een bestaande resourcegroep gebruiken, of maak een resourcegroep met de **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)** cmdlet. Bij het maken van een resourcegroep, geef de naam en locatie voor de resourcegroep.  

    ```PS
    PS C:\> New-AzureRmResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Gebruik de **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)** cmdlet voor het maken van de Recovery Services-kluis. Zorg ervoor dat de dezelfde locatie voor de kluis opgeven dat werd gebruikt voor de resourcegroep.

    ```PS
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Geef het type opslagredundantie moet gebruiken. u kunt [lokaal redundante opslag (LRS)](../storage/common/storage-redundancy-lrs.md) of [geografisch redundante opslag (GRS)](../storage/common/storage-redundancy-grs.md). Het volgende voorbeeld ziet dat de optie - BackupStorageRedundancy voor testvault is ingesteld op GeoRedundant.

    ```PS
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault -Name "testvault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > Voor veel Azure Backup-cmdlets is het object Recovery Services-kluis als invoer vereist. Daarom is het handiger het object Backup Recovery Services-kluis in een variabele op te slaan.
   >
   >

## <a name="view-the-vaults-in-a-subscription"></a>De kluizen in een abonnement weergeven
Gebruik **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)** om de lijst met alle kluizen in het huidige abonnement weer te geven. U kunt deze opdracht gebruiken om te controleren of een nieuwe kluis is gemaakt, of om te zien van de beschikbare kluizen in het abonnement.

Voer de opdracht Get-AzureRmRecoveryServicesVault, om alle kluizen in het abonnement weer te geven. Het volgende voorbeeld ziet de informatie die wordt weergegeven voor elke kluis.

```
PS C:\> Get-AzureRmRecoveryServicesVault
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="back-up-azure-vms"></a>Back-ups maken van Azure-VM's
Een Recovery Services-kluis gebruiken ter bescherming van uw virtuele machines. Voordat u de beveiliging toepassen, stelt de context van de kluis (het type van de gegevens die worden beveiligd in de kluis) en controleer of het beveiligingsbeleid. Het beveiligingsbeleid is het schema voor de back-uptaken uitgevoerd en hoelang elke back-upmomentopname wordt bewaard.

### <a name="set-vault-context"></a>De context van de kluis instellen
Voordat u de beveiliging op een virtuele machine inschakelt, gebruikt u **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** om in te stellen van de context van de kluis. Zodra deze is ingesteld, is deze op alle navolgende cmdlets van toepassing. Het volgende voorbeeld wordt de context van de kluis voor de kluis *testvault*.

```
PS C:\> Get-AzureRmRecoveryServicesVault -Name "testvault" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a>Een beveiligingsbeleid maken
Als u een Recovery Services-kluis maakt, gaat deze gepaard met standaardbeleid voor beveiliging en retentie. Volgens het standaardbeveiligingsbeleid wordt elke dag op een bepaald tijdstip een back-uptaak getriggerd. Volgens het standaardbewaarbeleid wordt het dagelijkse herstelpunt gedurende dertig dagen bewaard. Het standaardbeleid kunt u snel uw VM te beveiligen en het beleid later met verschillende gegevens te bewerken.

Gebruik **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)** om weer te geven van het beveiligingsbeleid voor apps in de kluis. U kunt deze cmdlet gebruiken om op te halen van een specifiek beleid, of om de beleidsregels die zijn gekoppeld aan een type werkbelasting weer te geven. Het volgende voorbeeld wordt een beleid voor het type werkbelasting, installatiekopieën van virtuele machines.

```
PS C:\> Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> De tijdzone van het veld BackupTime in PowerShell is UTC. Echter, wanneer de back-uptijd wordt weergegeven in de Azure-portal, de tijd wordt aangepast aan uw lokale tijdzone.
>
>

Een back-protection-beleid is gekoppeld aan ten minste één beleid voor Gegevensretentie. Bewaarbeleid wordt gedefinieerd hoe lang een herstelpunt wordt bewaard voordat deze wordt verwijderd. Gebruik **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)** om het standaardretentiebeleid weer te geven.  Op deze manier kunt u **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)** om op te halen van het standaardbeleid voor planning. De **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** met de cmdlet maakt een PowerShell-object dat back-upbeleid informatie bevat. De beleidsobjecten schema en de retentie worden gebruikt als invoer voor de **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** cmdlet. Het volgende voorbeeld wordt het beleid voor planning en het bewaarbeleid in variabelen. Het voorbeeld deze variabelen worden gebruikt voor het definiëren van de parameters bij het maken van een beveiligingsbeleid *NewPolicy*.

```
PS C:\> $schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```


### <a name="enable-protection"></a>Beveiliging inschakelen
Als u het back-protection-beleid hebt gedefinieerd, moet u nog steeds het beleid voor een item inschakelen. Gebruik **[Enable-AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)** beveiliging in te schakelen. Inschakelen van beveiliging vereist twee objecten - het item en het beleid. Nadat het beleid gekoppeld aan de kluis is, worden de back-werkstroom wordt geactiveerd op het moment dat is gedefinieerd in de planning van het beleid.

Het volgende voorbeeld wordt de beveiliging voor het item, V2VM, met behulp van het beleid, NewPolicy. De beveiliging op niet-versleutelde Resource Manager VM's in te schakelen

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Om in te schakelen de beveiliging van versleutelde virtuele machines (versleuteld met behulp van (bek) en KEK), moet u de Azure Backup-service machtigen sleutels en geheimen worden gelezen vanuit key vault.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Om in te schakelen van de beveiliging van versleutelde virtuele machines die zijn (versleuteld met behulp van de BEK alleen), moet u de Azure Backup-service machtigt om te lezen van geheimen vanuit key vault.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> Als u van de Azure Government-cloud gebruikmaakt, gebruikt u de ff281ffe-705c-4f53-9f37-a40e6f2c68f3 waarde voor de parameter **- ServicePrincipalName** in [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet.
>
>

Voor klassieke VM 's

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### <a name="modify-a-protection-policy"></a>Een beveiligingsbeleid wijzigen
Gebruiken voor het wijzigen van het beveiligingsbeleid, [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) de SchedulePolicy of RetentionPolicy objecten te wijzigen.

Het volgende voorbeeld wordt de bewaarperiode voor herstelpunten en 365 dagen.

```
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol.DailySchedule.DurationCountInDays = 365
PS C:\> $pol= Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a>Een back-up activeren
U kunt **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)** voor het activeren van een back-uptaak. Als dit de eerste back-up is, is een volledige back-up. Volgende back-ups duren voordat een incrementele kopie. Zorg ervoor dat u **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** om in te stellen van de context van de kluis voordat de back-uptaak wordt geactiveerd. Het volgende voorbeeld wordt ervan uitgegaan dat de context van kluis is ingesteld.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
PS C:\> $job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup               InProgress            4/23/2016 5:00:30 PM                       cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> De tijdzone van de StartTime en EndTime velden in PowerShell is UTC. Echter, wanneer de tijd wordt weergegeven in de Azure-portal, de tijd wordt aangepast aan uw lokale tijdzone.
>
>

## <a name="monitoring-a-backup-job"></a>Bewaking van een back-uptaak
U kunt langlopende bewerkingen, zoals back-uptaken controleren zonder gebruik van de Azure-portal. Als u de status van een taak wordt uitgevoerd, gebruikt de **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)** cmdlet. Deze cmdlet haalt de back-uptaken voor een bepaalde kluis en die vault is opgegeven in de context van de kluis. Het volgende voorbeeld wordt de status van een taak wordt uitgevoerd als een matrix en slaat de status in de $joblist-variabele.

```
PS C:\> $joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
PS C:\> $joblist[0]
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016 5:00:30 PM           cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

In plaats van deze taken voor de voltooiing - dit is niet nodig als u meer code - polling gebruiken de **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)** cmdlet. Deze cmdlet wordt de uitvoering onderbroken totdat de taak is voltooid of de opgegeven time-outwaarde is bereikt.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a>Een Azure-VM herstellen
Er is een belangrijk verschil tussen het herstellen van een virtuele machine met behulp van de Azure portal en het herstellen van een virtuele machine met behulp van PowerShell. Met PowerShell, is de herstelbewerking is voltooid nadat de schijven en configuratie-informatie van het herstelpunt worden gemaakt. Als u wilt herstellen of enkele bestanden herstellen van een Azure-VM back-up, raadpleegt u de [sectie herstel-bestand](backup-azure-vms-automation.md#restore-files-from-an-azure-vm-backup)

> [!NOTE]
> De herstelbewerking opnieuw maakt geen een virtuele machine.
>
>

Voor het maken van een virtuele machine van de schijf, Zie de sectie [maken van de VM van herstelde schijven](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). De basisstappen voor het herstellen van een virtuele machine van Azure zijn:

* Selecteer de virtuele machine
* Kies een herstelpunt
* De schijven herstellen
* De virtuele machine maken van opgeslagen schijven

De volgende afbeelding toont de objecthiërarchie van de RecoveryServicesVault omlaag naar de BackupRecoveryPoint.

![Recovery Services-objecthiërarchie BackupContainer weergeven](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

Voor het herstellen van back-upgegevens, Identificeer het item waarvan een back-up is gemaakt en het herstelpunt die de point-in-time-gegevens bevat. Gebruik de **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** cmdlet voor het terugzetten van gegevens uit de kluis aan het account van de klant.

### <a name="select-the-vm"></a>Selecteer de virtuele machine
Als u de PowerShell-object waarmee de juiste back-artikel, starten van de container in de kluis en uw eigen manier omlaag in de objecthiërarchie werken. Selecteer de container die staat voor de virtuele machine, gebruikt u de **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)** cmdlet en doorgeven die u wilt de **[ Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)** cmdlet.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Kies een herstelpunt
Gebruik de **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)** cmdlet om alle herstelpunten voor de back-upitem weer te geven. Kies vervolgens het herstelpunt te herstellen. Als u niet zeker welk herstelpunt weet te gebruiken, is het raadzaam om te kiezen van de meest recente RecoveryPointType = AppConsistent punt in de lijst.

In het volgende script, de variabele **$rp**, is een matrix van herstelpunten voor de geselecteerde back-upitem van de afgelopen zeven dagen. De matrix is in omgekeerde volgorde gesorteerd op tijd wordt opgelost met de meest recente herstelpunt bij index 0. Gebruik de standaard PowerShell matrix indexeren om op te halen van het herstelpunt. In het voorbeeld selecteert $rp [0] het meest recente herstelpunt.

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
PS C:\> $rp[0]
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```



### <a name="restore-the-disks"></a>De schijven herstellen
Gebruik de **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** cmdlet om te herstellen van gegevens en configuratie van een back-upitem naar een herstelpunt. Nadat u hebt vastgesteld dat een herstelpunt, deze gebruiken als de waarde voor de **- RecoveryPoint** parameter. In de vorige voorbeeldcode **$rp [0]** is van het herstelpunt dat u wilt gebruiken. In de volgende voorbeeldcode **$rp [0]** is van het herstelpunt dat u wilt gebruiken voor het herstellen van de schijf.

De schijven en configuratie-informatie terugzetten:

```
PS C:\> $restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
PS C:\> $restorejob
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Gebruik de **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)** cmdlet na afloop van de hersteltaak om te voltooien.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

Als de hersteltaak is voltooid, gebruikt u de **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)** cmdlet om op te halen van de details van de herstelbewerking opnieuw. De eigenschap JobDetails heeft de informatie die nodig zijn voor de virtuele machine opnieuw.

```
PS C:\> $restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmRecoveryServicesBackupJobDetails -Job $restorejob
```

Nadat u de schijven herstellen, gaat u naar de volgende sectie om de VM te maken.

## <a name="create-a-vm-from-restored-disks"></a>Een virtuele machine maken van herstelde schijven
Nadat u de schijven hebt hersteld, gebruikt u deze stappen om te maken en configureren van de virtuele machine van de schijf.

> [!NOTE]
> Voor het maken van versleutelde virtuele machines van herstelde schijven, uw Azure-rol moet gemachtigd zijn om uit te voeren van de actie **Microsoft.KeyVault/vaults/deploy/action**. Als uw rol is niet gemachtigd dit, moet u een aangepaste rol maken met deze actie. Zie voor meer informatie, [aangepaste rollen in Azure RBAC](../role-based-access-control/custom-roles.md).
>
>

1. Query uitvoeren op de herstelde schijf-eigenschappen voor de taakdetails.

  ```
  PS C:\> $properties = $details.properties
  PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
  PS C:\> $containerName = $properties["Config Blob Container Name"]
  PS C:\> $configBlobName = $properties["Config Blob Name"]
  ```

2. Stel de context van de Azure-opslag en herstel van de JSON-configuratiebestand.

    ```
    PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
    PS C:\> $destination_path = "C:\vmconfig.json"
    PS C:\> Get-AzureStorageBlobContent -Container $containerName -Blob $configBlobName -Destination $destination_path
    PS C:\> $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
    ```

3. De JSON-configuratiebestand gebruiken om de configuratie van de virtuele machine te maken.

    ```
   PS C:\> $vm = New-AzureRmVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
    ```

4. Koppel de besturingssysteemschijf en gegevensschijven. Afhankelijk van de configuratie van uw VM's, verwijzen naar de betreffende sectie om respectieve cmdlets weer te geven:

    #### <a name="non-managed-non-encrypted-vms"></a>Niet-beheerde, niet-versleutelde VM 's

    Gebruik het volgende voorbeeld voor niet-beheerde, niet-versleutelde VM's.

    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
    PS C:\> foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
    {
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="non-managed-encrypted-vms-bek-only"></a>Niet-beheerde, versleutelde virtuele machines (alleen BEK)

    Voor niet-beheerde, versleutelde virtuele machines (versleuteld met behulp van de BEK alleen), moet u het geheim voor de key vault herstellen voordat u schijven kunt koppelen. Zie voor meer informatie het artikel [een versleutelde virtuele machine herstellen vanaf een herstelpunt voor Azure Backup](backup-azure-restore-key-secret.md). Het volgende voorbeeld laat zien hoe besturingssysteem en gegevensschijven koppelen voor versleutelde virtuele machines.

    ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $dekUrl = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    ```
    
 Bij het instellen van de besturingssysteemschijf, zorg ervoor dat het relevante type besturingssysteem wordt genoemd.   
    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows/Linux
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
    ```
    
De gegevensversleuteling schijven moet handmatig worden ingeschakeld met behulp van de volgende opdracht uit.

    ```
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $dekUrl -VolumeType Data
    ```
    
   #### <a name="non-managed-encrypted-vms-bek-and-kek"></a>Niet-beheerde, versleutelde virtuele machines (BEK en KEK)

   Voor niet-beheerde, versleutelde virtuele machines (versleuteld met behulp van (bek) en KEK), moet u de sleutel en -geheim voor de key vault herstellen voordat u schijven kunt koppelen. Zie voor meer informatie het artikel [een versleutelde virtuele machine herstellen vanaf een herstelpunt voor Azure Backup](backup-azure-restore-key-secret.md). Het volgende voorbeeld laat zien hoe besturingssysteem en gegevensschijven koppelen voor versleutelde virtuele machines.

    ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

De gegevensversleuteling schijven moet handmatig worden ingeschakeld met behulp van de volgende opdracht uit.

    ```
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $dekUrl -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -VolumeType Data
    ```
    
   #### <a name="managed-non-encrypted-vms"></a>Beheerd, niet-versleutelde VM 's

   Voor beheerde niet-versleutelde VM's moet u het maken van beheerde schijven van blob-opslag, en voeg vervolgens de schijven. Voor gedetailleerde informatie, Zie het artikel [een gegevensschijf koppelen aan een Windows-VM met behulp van PowerShell](../virtual-machines/windows/attach-disk-ps.md). De volgende voorbeeldcode laat zien hoe u de gegevensschijven koppelen voor beheerde niet-versleutelde VM's.

    ```
    PS C:\> $storageType = "StandardLRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

   #### <a name="managed-encrypted-vms-bek-only"></a>Beheerde, versleutelde virtuele machines (alleen BEK)

   Voor beheerde versleutelde virtuele machines (versleuteld met behulp van de BEK alleen), moet u het maken van beheerde schijven van blob-opslag, en voeg vervolgens de schijven. Voor gedetailleerde informatie, Zie het artikel [een gegevensschijf koppelen aan een Windows-VM met behulp van PowerShell](../virtual-machines/windows/attach-disk-ps.md). De volgende voorbeeldcode laat zien hoe u de gegevensschijven koppelen voor beheerde versleutelde virtuele machines.

     ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> $storageType = "StandardLRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

De gegevensversleuteling schijven moet handmatig worden ingeschakeld met behulp van de volgende opdracht uit.

    ```
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -VolumeType Data
    ```
    
   #### <a name="managed-encrypted-vms-bek-and-kek"></a>Beheerde, versleutelde virtuele machines (BEK en KEK)

   Voor beheerde versleutelde virtuele machines (versleuteld met behulp van (bek) en KEK), moet u het maken van beheerde schijven van blob-opslag, en voeg vervolgens de schijven. Voor gedetailleerde informatie, Zie het artikel [een gegevensschijf koppelen aan een Windows-VM met behulp van PowerShell](../virtual-machines/windows/attach-disk-ps.md). De volgende voorbeeldcode laat zien hoe u de gegevensschijven koppelen voor beheerde versleutelde virtuele machines.

     ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> $storageType = "StandardLRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
     }
    ```
De gegevensversleuteling schijven moet handmatig worden ingeschakeld met behulp van de volgende opdracht uit.

    ```
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $dekUrl -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -VolumeType Data
    ```
    
5. Stel de netwerkinstellingen.

    ```
    PS C:\> $nicName="p1234"
    PS C:\> $pip = New-AzureRmPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    PS C:\> $virtualNetwork = New-AzureRmVirtualNetwork -ResourceGroupName "test" -Location "WestUS" -Name "testvNET" -AddressPrefix 10.0.0.0/16
    PS C:\> $virtualNetwork | Set-AzureRmVirtualNetwork
    PS C:\> $vnet = Get-AzureRmVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    PS C:\> $subnetindex=0
    PS C:\> $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    PS C:\> $vm=Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    ```
6. Maak de virtuele machine.

    ```    
    PS C:\> New-AzureRmVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

## <a name="restore-files-from-an-azure-vm-backup"></a>Bestanden herstellen vanaf een back-up van virtuele Azure-machine

Naast het herstellen van schijven, kunt u ook afzonderlijke bestanden herstellen vanuit een Azure-VM back-up. De functionaliteit van de bestanden terugzetten biedt toegang tot alle bestanden in een herstelpunt en u ze kunt beheren via Verkenner net zoals u voor normale bestanden doen zou.

De eenvoudige stappen naar een bestand herstellen vanuit back-up van virtuele Azure-machine zijn:

* Selecteer de virtuele machine
* Kies een herstelpunt
* De schijven van het herstelpunt gekoppeld
* Kopieer de benodigde bestanden
* Ontkoppel de schijf


### <a name="select-the-vm"></a>Selecteer de virtuele machine
Als u de PowerShell-object waarmee de juiste back-artikel, starten van de container in de kluis en uw eigen manier omlaag in de objecthiërarchie werken. Selecteer de container die staat voor de virtuele machine, gebruikt u de **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)** cmdlet en doorgeven die u wilt de **[ Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)** cmdlet.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Kies een herstelpunt
Gebruik de **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)** cmdlet om alle herstelpunten voor de back-upitem weer te geven. Kies vervolgens het herstelpunt te herstellen. Als u niet zeker welk herstelpunt weet te gebruiken, is het raadzaam om te kiezen van de meest recente RecoveryPointType = AppConsistent punt in de lijst.

In het volgende script, de variabele **$rp**, is een matrix van herstelpunten voor de geselecteerde back-upitem van de afgelopen zeven dagen. De matrix is in omgekeerde volgorde gesorteerd op tijd wordt opgelost met de meest recente herstelpunt bij index 0. Gebruik de standaard PowerShell matrix indexeren om op te halen van het herstelpunt. In het voorbeeld selecteert $rp [0] het meest recente herstelpunt.

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
PS C:\> $rp[0]
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```

### <a name="mount-the-disks-of-recovery-point"></a>De schijven van het herstelpunt gekoppeld

Gebruik de **[Get-AzureRmRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprpmountscript)** cmdlet om op te halen van het script om alle schijven van het herstelpunt te koppelen.

> [!NOTE]
> De schijven zijn gekoppeld als iSCSI-gekoppelde schijven aan de machine waarop het script wordt uitgevoerd. Daarom is bijna onmiddellijk en leidt niet tot de kosten in rekening gebracht
>
>

```
PS C:\> Get-AzureRmRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]

OsType  Password        Filename
------  --------        --------
Windows e3632984e51f496 V2VM_wus2_8287309959960546283_451516692429_cbd6061f7fc543c489f1974d33659fed07a6e0c2e08740.exe
```
Voer het script op de computer waar u om de bestanden te herstellen. U moet het wachtwoord voor het uitvoeren van het script hierboven invoeren. Nadat de schijven zijn gekoppeld, gebruikt u de Verkenner van Windows om te bladeren door de nieuwe volumes en bestanden. Raadpleeg voor meer informatie de [recovery documentatie-bestand](backup-azure-restore-files-from-vm.md)

### <a name="unmount-the-disks"></a>De schijven ontkoppelen
Nadat de vereiste bestanden zijn gekopieerd, de schijven ontkoppelen met behulp van de **[uitschakelen AzureRmRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/disable-azurermrecoveryservicesbackuprpmountscript?view=azurermps-5.0.0)** cmdlet. Dit wordt sterk aanbevolen omdat hiermee u ervoor zorgt dat toegang tot de bestanden van het herstelpunt is verwijderd

```
PS C:\> Disable-AzureRmRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]
```


## <a name="next-steps"></a>Volgende stappen
Als u liever PowerShell gebruiken om te communiceren met uw Azure-resources, Zie het artikel PowerShell [implementeren en beheren van back-up voor Windows Server](backup-client-automation.md). Als u DPM back-ups beheren, Zie het artikel [implementeren en beheren van Backup voor DPM](backup-dpm-automation.md). Beide van de volgende artikelen hebben een versie voor implementaties van Resource Manager en klassieke implementaties.  
