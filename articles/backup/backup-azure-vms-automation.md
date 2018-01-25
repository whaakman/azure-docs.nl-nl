---
title: "Implementeren en beheren van back-ups voor Resource Manager geïmplementeerde VM's met behulp van PowerShell | Microsoft Docs"
description: "PowerShell gebruiken om te implementeren en beheren van back-ups in Azure voor Resource Manager geïmplementeerde VM 's"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 68606e4f-536d-4eac-9f80-8a198ea94d52
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/20/2017
ms.author: markgal;trinadhk;pullabhk
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e2eda7cee90d307d646ff68e104750c3057dcb06
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="use-azurermrecoveryservicesbackup-cmdlets-to-back-up-virtual-machines"></a>AzureRM.RecoveryServices.Backup-cmdlets gebruiken om back-up van virtuele machines

Dit artikel ziet u het gebruik van Azure PowerShell-cmdlets voor back-up en herstellen van Azure een virtuele machine (VM) vanuit een Recovery Services-kluis. Een Recovery Services-kluis is een Azure Resource Manager-bron en wordt gebruikt voor het beveiligen van gegevens en activa in Azure Backup- en Azure Site Recovery services. U kunt een Recovery Services-kluis gebruiken om Azure Service Manager geïmplementeerde VM's en Azure Resource Manager geïmplementeerde VM's te beveiligen.

> [!NOTE]
> Azure heeft twee implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel is voor gebruik met virtuele machines die zijn gemaakt met behulp van de Resource Manager-model.
>
>

Dit artikel begeleidt u bij het beveiligen van een virtuele machine en gegevens terugzetten vanaf een herstelpunt wordt gemaakt met behulp van PowerShell.

## <a name="concepts"></a>Concepten
Als u niet bekend met de Azure Backup-service voor een overzicht van de service bent, uitchecken [wat is Azure Backup?](backup-introduction-to-azure-backup.md) Voordat u begint, zorg ervoor dat u de essentials over de vereisten die nodig zijn voor het werken met Azure Backup en de beperkingen van de huidige VM back-upoplossing dekt.

PowerShell als effectief wilt gebruiken, is het nodig om inzicht in de hiërarchie van objecten en van waar u wilt beginnen.

![Recovery Services-objecthiërarchie](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

De verwijzing AzureRm.RecoveryServices.Backup PowerShell-cmdlet, Zie de [Azure Backup - Cmdlets voor Recovery Services](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) in de Azure-bibliotheek.

## <a name="setup-and-registration"></a>De installatie en registratie
Om te beginnen met:

1. [Download de nieuwste versie van PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (de minimaal vereiste versie is: 1.4.0)
2. De beschikbare Azure back-up PowerShell-cmdlets vinden door de volgende opdracht te typen:

```
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
3. Aanmelden bij uw Azure-account met **Login-AzureRmAccount**. Deze cmdlet wordt een webpagina wordt u gevraagd uw accountreferenties: 
    - Ook kunt u uw accountreferenties opnemen als een parameter in de **Login-AzureRmAccount** cmdlet, met behulp van de **-referentie** parameter.
    - Als u CSP partner werken namens een tenant de klant opgeven als een tenant met behulp van de naam van de primaire domeincontroller tenantID of tenant. Bijvoorbeeld: **Login-AzureRmAccount-Tenant 'fabrikam.com'**
4. Het abonnement dat u gebruiken met de acount wilt omdat een account kan meerdere abonnementen hebben koppelen:

    ```
    PS C:\> Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

5. Als u Azure Backup voor de eerste keer gebruikt, moet u de  **[registreren AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)**  cmdlet worden de Azure Recovery Service provider geregistreerd bij uw abonnement.

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Backup"
    ```

6. U kunt controleren of de Providers geregistreerd, met de volgende opdrachten:
    ```
    PS C:\> Get-AzureRmResourceProvider -ProviderNamespace  "Microsoft.RecoveryServices"
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Backup"
    ``` 
In de opdrachtuitvoer de **RegistrationState** moet ingesteld op **geregistreerde**. Als dat niet alleen opnieuw uitvoeren de  **[registreren AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)**  cmdlet hierboven weergegeven.

De volgende taken kunnen worden geautomatiseerd met PowerShell:

* Een Recovery Services-kluis maken
* Back-ups maken van Azure-VM's
* Een back-uptaak wordt geactiveerd
* Een back-uptaak bewaken
* Een Azure-virtuele machine herstellen

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken
De volgende stappen leiden u bij het maken van een Recovery Services-kluis. Een Recovery Services-kluis is anders dan een back-upkluis.

1. De Recovery Services-kluis is een Resource Manager-bron, dus u moet deze binnen een resourcegroep te plaatsen. Gebruik een bestaande resourcegroep of maak een resourcegroep met de  **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)**  cmdlet. Bij het maken van een resourcegroep, geef de naam en locatie voor de resourcegroep.  

    ```
    PS C:\> New-AzureRmResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Gebruik de  **[nieuw AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)**  cmdlet voor het maken van de Recovery Services-kluis. Zorg ervoor dat Geef dezelfde locatie voor de kluis werd gebruikt voor de resourcegroep.

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
3. Geef het type van de redundantie van gegevensopslag worden gebruikt. u kunt [lokaal redundante opslag (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) of [geografisch redundante opslag (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage). Het volgende voorbeeld ziet dat de optie - BackupStorageRedundancy voor testvault is ingesteld op GeoRedundant.

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault -Name "testvault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > Veel Azure Backup-cmdlets moet de Recovery Services-kluis-object als invoer. Daarom is het handig zijn voor het opslaan van het back-up Recovery Services-kluis-object in een variabele.
   >
   >

## <a name="view-the-vaults-in-a-subscription"></a>De kluizen in een abonnement weergeven
Gebruik  **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)**  de lijst met alle kluizen weergeven in het huidige abonnement. U kunt deze opdracht gebruiken om te controleren of een nieuwe kluis is gemaakt, of om te zien van de beschikbare kluizen in het abonnement.

Voer de opdracht Get-AzureRmRecoveryServicesVault om alle kluizen in het abonnement weer te geven. Het volgende voorbeeld ziet de informatie die wordt weergegeven voor elke kluis.

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
Gebruik een Recovery Services-kluis ter bescherming van uw virtuele machines. Als u de beveiliging wilt toepassen, stelt u de kluis context (het type van de gegevens die worden beveiligd in de kluis) en controleer of het beveiligingsbeleid. Het beveiligingsbeleid is de planning voor wanneer de back-uptaken uitgevoerd en hoe lang de momentopname van elke back-up wordt bewaard.

### <a name="set-vault-context"></a>Set kluis context
Voordat u de beveiliging op een virtuele machine inschakelt, gebruikt u  **[Set AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  om in te stellen de context van de kluis. Zodra de kluis context is ingesteld, wordt het toegepast op alle latere cmdlets. Het volgende voorbeeld wordt de context van de kluis voor de kluis *testvault*.

```
PS C:\> Get-AzureRmRecoveryServicesVault -Name "testvault" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a>Een beveiligingsbeleid maken
Als u een Recovery Services-kluis maakt, beschikt u over de standaardbeveiliging en bewaarbeleid. Het standaardbeleid voor beveiliging een back-uptaak elke dag op de opgegeven tijd wordt geactiveerd. Het bewaarbeleid standaard wordt het dagelijkse herstelpunt voor 30 dagen bewaard. Het standaardbeleid kunt u snel uw virtuele machine te beveiligen en het beleid met andere informatie later bewerken.

Gebruik  **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)**  om weer te geven van het beveiligingsbeleid in de kluis. U kunt deze cmdlet gebruiken om op te halen van een specifiek beleid of om weer te geven van de beleidsregels die zijn gekoppeld aan een type werkbelasting. Het volgende voorbeeld wordt een beleid voor het type werkbelasting, AzureVM.

```
PS C:\> Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> De tijdzone van het veld BackupTime in PowerShell is UTC. Echter, wanneer de back-uptijd wordt weergegeven in de Azure portal, de tijd wordt aangepast aan uw lokale tijdzone.
>
>

Het beveiligingsbeleid van een back-up is gekoppeld aan ten minste één bewaarbeleid. Bewaarbeleid definieert hoe lang een herstelpunt wordt bewaard voordat deze wordt verwijderd. Gebruik  **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)**  om het bewaarbeleid standaard weer te geven.  Op dezelfde manier kunt u  **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)**  verkrijgen van het standaardbeleid voor planning. De  **[nieuw AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  cmdlet maakt een PowerShell-object dat back-upbeleid informatie bevat. De beleidsobjecten planning en retentie worden gebruikt als invoer voor de  **[nieuw AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  cmdlet. Het volgende voorbeeld wordt het beleid van de planning en het bewaarbeleid in variabelen. In het voorbeeld deze variabelen worden gebruikt voor het definiëren van de parameters bij het maken van een beveiligingsbeleid *NewPolicy*.

```
PS C:\> $schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```


### <a name="enable-protection"></a>Beveiliging inschakelen
Als u het back-beveiligingsbeleid hebt gedefinieerd, moet u nog steeds het beleid voor een item inschakelen. Gebruik  **[inschakelen AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)**  beveiliging in te schakelen. Inschakelen van beveiliging vereist twee objecten - het item en het beleid. Als het beleid is gekoppeld aan de kluis, wordt de back-werkstroom wordt geactiveerd op het moment dat is gedefinieerd in de planning van het beleid.

Het volgende voorbeeld wordt de beveiliging voor het item, V2VM, met het beleid, NewPolicy. De beveiliging op niet-versleutelde Resource Manager virtuele machines in te schakelen

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Om de beveiliging op versleutelde virtuele machines (versleuteld met behulp van BEK en KEK) inschakelt, moet u de Azure Backup-service machtigen sleutels en geheimen lezen uit de sleutelkluis.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Versleuteld zodat de beveiliging op virtuele machines (versleuteld met BEK alleen), moet u de Azure Backup-service machtigen om te lezen van geheimen van de sleutelkluis.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> Als u van de cloud Azure Government gebruikmaakt, gebruikt u de ff281ffe-705c-4f53-9f37-a40e6f2c68f3 waarde voor de parameter **- ServicePrincipalName** in [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet.
>
>

Voor klassieke virtuele machines

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### <a name="modify-a-protection-policy"></a>Een beveiligingsbeleid wijzigen
Gebruik voor het wijzigen van het beveiligingsbeleid [Set AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) om de objecten SchedulePolicy of RetentionPolicy te wijzigen.

Het volgende voorbeeld wordt de bewaarperiode herstel en 365 dagen.

```
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol.DailySchedule.DurationCountInDays = 365
PS C:\> $pol= Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a>Activeren van een back-up
U kunt  **[back-up AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)**  voor het activeren van een back-uptaak. Als de eerste back-up is, is een volledige back-up. Volgende back-ups duren voordat een incrementele kopie. Zorg ervoor dat u  **[Set AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  de context van de kluis instellen voordat de back-uptaak. Het volgende voorbeeld wordt ervan uitgegaan kluis context is ingesteld.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
PS C:\> $job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup               InProgress            4/23/2016 5:00:30 PM                       cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> De tijdzone van de velden StartTime en EndTime in PowerShell is UTC. Echter, wanneer de tijd wordt weergegeven in de Azure portal, de tijd wordt aangepast aan uw lokale tijdzone.
>
>

## <a name="monitoring-a-backup-job"></a>Bewaking van een back-uptaak
U kunt langlopende bewerkingen, zoals back-uptaken bewaken zonder gebruik van de Azure-portal. Als u de status van een taak wordt uitgevoerd, gebruikt de  **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)**  cmdlet. Deze cmdlet wordt de back-uptaken voor een specifieke kluis en de kluis die is opgegeven in de context van de kluis. Het volgende voorbeeld wordt de status van een taak wordt uitgevoerd als een matrix en slaat de status in de variabele $joblist.

```
PS C:\> $joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
PS C:\> $joblist[0]
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016 5:00:30 PM           cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

In plaats van deze taken voor voltooiing - dit is niet nodig aanvullende code - polling gebruiken de  **[wacht AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  cmdlet. Deze cmdlet onderbreekt de uitvoering totdat de taak is voltooid of de opgegeven time-outwaarde is bereikt.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a>Een Azure-virtuele machine herstellen
Er is een belangrijk verschil tussen het herstellen van een virtuele machine met de Azure portal en herstellen van een virtuele machine met behulp van PowerShell. Met PowerShell, is de herstelbewerking is voltooid zodra de schijven en configuratie-informatie uit het herstelpunt worden gemaakt. Als u wilt herstellen of enkele bestanden van een virtuele machine van Azure back-up herstellen, raadpleegt u de [bestand sectie herstel](backup-azure-vms-automation.md#restore-files-from-an-azure-vm-backup)

> [!NOTE]
> De herstelbewerking maakt geen een virtuele machine.
>
>

Zie de sectie voor informatie over het maken van een virtuele machine van de schijf [de virtuele machine maken van herstelde schijven](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). De basisstappen voor het herstellen van een Azure VM zijn:

* Selecteer de virtuele machine
* Kies een herstelpunt
* De schijven herstellen
* De virtuele machine maken van opgeslagen schijven

De volgende afbeelding ziet de objecthiërarchie van de RecoveryServicesVault omlaag naar de BackupRecoveryPoint.

![Recovery Services-objecthiërarchie BackupContainer weergeven](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

Identificeer het artikel back-up en het herstelpunt dat de punt in tijd gegevens bevat voor het herstellen van back-upgegevens. Gebruik de  **[terugzetten AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  cmdlet voor het terugzetten van gegevens uit de kluis aan het account van de klant.

### <a name="select-the-vm"></a>Selecteer de virtuele machine
Als u het PowerShell-object waarmee de juiste back-artikel, start vanuit de container in de kluis en werk van de object-hiërarchie. Gebruik om te selecteren in de container waarin de VM vertegenwoordigt, de  **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)**  cmdlet en doorsluizen die u wilt de  **[Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)**  cmdlet.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Kies een herstelpunt
Gebruik de  **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)**  cmdlet voor een lijst met alle herstelpunten voor de back-item. Kies het herstelpunt te herstellen. Als u niet zeker welk herstelpunt weet te gebruiken, is het verstandig om te kiezen van de meest recente RecoveryPointType = AppConsistent punt in de lijst.

In het volgende script wordt de variabele **$rp**, is een matrix van herstelpunten voor de geselecteerde back-item van de afgelopen zeven dagen. De matrix is in omgekeerde volgorde gesorteerd tijd met de meest recente herstelpunt bij index 0. Gebruik standaard PowerShell matrix indexeren om op te halen van het herstelpunt. $Rp [0] selecteert in het voorbeeld wordt het meest recente herstelpunt.

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
Gebruik de  **[terugzetten AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  cmdlet om de gegevens en configuratie van een back-item herstellen naar een herstelpunt wordt gemaakt. Nadat u een herstelpunt hebt geïdentificeerd, deze gebruiken als de waarde voor de **- RecoveryPoint** parameter. In de vorige voorbeeldcode **$rp [0]** is het herstelpunt te gebruiken. In de volgende voorbeeldcode **$rp [0]** is het herstelpunt dat moet worden gebruikt voor het herstellen van de schijf.

Herstellen van de schijven en configuratie-informatie:

```
PS C:\> $restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
PS C:\> $restorejob
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Gebruik de  **[wacht AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  cmdlet moet worden gewacht op de hersteltaak om te voltooien.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

Zodra de hersteltaak is voltooid, gebruikt u de  **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)**  cmdlet om de details ophalen van de herstelbewerking opnieuw. De eigenschap JobDetails heeft de benodigde informatie om het opnieuw opbouwen van de virtuele machine.

```
PS C:\> $restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmRecoveryServicesBackupJobDetails -Job $restorejob
```

Zodra u de schijven hebt hersteld, gaat u naar de volgende sectie voor de virtuele machine maken.

## <a name="create-a-vm-from-restored-disks"></a>Een virtuele machine maken van herstelde schijven
Nadat u de schijven hebt teruggezet, volg deze stappen voor het maken en configureren van de virtuele machine van de schijf.

> [!NOTE]
> Als u wilt maken versleutelde virtuele machines van de herstelde schijven, uw Azure moet beschikt over bevoegdheid om de actie uitvoeren **Microsoft.KeyVault/vaults/deploy/action**. Als uw rol is niet gemachtigd dit, maakt u een aangepaste rol met deze actie. Zie voor meer informatie [aangepaste rollen in Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).
>
>

1. De schijfeigenschappen van de herstelde voor de taakdetails opvragen.

  ```
  PS C:\> $properties = $details.properties
  PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
  PS C:\> $containerName = $properties["Config Blob Container Name"]
  PS C:\> $blobName = $properties["Config Blob Name"]
  ```

2. De context van de Azure-opslag instellen en herstel van het JSON-configuratiebestand.

    ```
    PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
    PS C:\> $destination_path = "C:\vmconfig.json"
    PS C:\> Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path
    PS C:\> $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
    ```

3. Gebruik het JSON-configuratiebestand om de configuratie van de virtuele machine te maken.

    ```
   PS C:\> $vm = New-AzureRmVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
    ```

4. De besturingssysteemschijf en gegevensschijven koppelen. Zie de relevante sectie om weer te geven van de respectieve cmdlets afhankelijk van de configuratie van uw virtuele machines:

    #### <a name="non-managed-non-encrypted-vms"></a>Niet-beheerde, niet-versleutelde virtuele machines

    Gebruik het volgende voorbeeld voor niet-beheerde, niet-versleutelde virtuele machines.

    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
    PS C:\> foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
    {
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="non-managed-encrypted-vms-bek-only"></a>Niet-beheerde, gecodeerde VMs (alleen BEK)

    Voor niet-beheerde, gecodeerde virtuele machines (versleuteld met BEK alleen), moet u het geheim aan de sleutelkluis herstellen voordat u schijven kunt koppelen. Zie voor meer informatie het artikel [een versleutelde virtuele machine herstellen vanaf een herstelpunt voor Azure Backup](backup-azure-restore-key-secret.md). Het volgende voorbeeld laat zien hoe besturingssysteem en gegevensschijven koppelen voor versleutelde virtuele machines.

    ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

    #### <a name="non-managed-encrypted-vms-bek-and-kek"></a>Niet-beheerde, gecodeerde VMs (BEK en KEK)

    Voor niet-beheerde, gecodeerde virtuele machines (versleuteld met behulp van BEK en KEK), moet u de sleutel en geheime tot de sleutelkluis herstellen voordat u schijven kunt koppelen. Zie voor meer informatie het artikel [een versleutelde virtuele machine herstellen vanaf een herstelpunt voor Azure Backup](backup-azure-restore-key-secret.md). Het volgende voorbeeld laat zien hoe besturingssysteem en gegevensschijven koppelen voor versleutelde virtuele machines.

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

    #### <a name="managed-non-encrypted-vms"></a>Beheerd, niet-versleutelde virtuele machines

    Voor beheerde niet-versleutelde virtuele machines moet u beheerde schijven maken van blob-opslag, en voeg vervolgens de schijven. Voor gedetailleerde informatie, Zie het artikel [een gegevensschijf koppelen aan een virtuele machine van Windows met behulp van PowerShell](../virtual-machines/windows/attach-disk-ps.md). De volgende voorbeeldcode laat zien hoe de gegevensschijven koppelen voor niet-versleutelde VM's van beheerde.

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

    #### <a name="managed-encrypted-vms-bek-only"></a>Beheerd, gecodeerde VMs (alleen BEK)

    Voor beheerde versleutelde virtuele machines (versleuteld met BEK alleen), moet u beheerde schijven maken van blob-opslag, en voeg vervolgens de schijven. Voor gedetailleerde informatie, Zie het artikel [een gegevensschijf koppelen aan een virtuele machine van Windows met behulp van PowerShell](../virtual-machines/windows/attach-disk-ps.md). De volgende voorbeeldcode laat zien hoe de gegevensschijven koppelen voor beheerde versleutelde virtuele machines.

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

    #### <a name="managed-encrypted-vms-bek-and-kek"></a>Beheerde, gecodeerde VMs (BEK en KEK)

    Voor beheerde versleutelde virtuele machines (versleuteld met behulp van BEK en KEK), moet u beheerde schijven maken van blob-opslag, en voeg vervolgens de schijven. Voor gedetailleerde informatie, Zie het artikel [een gegevensschijf koppelen aan een virtuele machine van Windows met behulp van PowerShell](../virtual-machines/windows/attach-disk-ps.md). De volgende voorbeeldcode laat zien hoe de gegevensschijven koppelen voor beheerde versleutelde virtuele machines.

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

5. De instellingen van het netwerk instellen.

    ```
    PS C:\> $nicName="p1234"
    PS C:\> $pip = New-AzureRmPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    PS C:\> $vnet = Get-AzureRmVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    PS C:\> $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    PS C:\> $vm=Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    ```
6. Maak de virtuele machine.

    ```    
    PS C:\> New-AzureRmVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

## <a name="restore-files-from-an-azure-vm-backup"></a>Bestanden terugzetten vanaf een virtuele machine van Azure back-up

Naast het herstellen van schijven kunt u ook afzonderlijke bestanden terugzetten vanaf een back-up van virtuele machine van Azure. De functionaliteit van de bestanden terugzetten krijgt toegang tot alle bestanden in een herstelpunt wordt gemaakt en u ze kunt beheren via de Verkenner zoals u voor normale bestanden doen zou.

De basisstappen voor het terugzetten van een bestand van de virtuele machine van Azure back-up zijn:

* Selecteer de virtuele machine
* Kies een herstelpunt
* Koppel de schijven van een herstelpunt
* Kopieer de benodigde bestanden
* Ontkoppel de schijf


### <a name="select-the-vm"></a>Selecteer de virtuele machine
Als u het PowerShell-object waarmee de juiste back-artikel, start vanuit de container in de kluis en werk van de object-hiërarchie. Gebruik om te selecteren in de container waarin de VM vertegenwoordigt, de  **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)**  cmdlet en doorsluizen die u wilt de  **[Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)**  cmdlet.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Kies een herstelpunt
Gebruik de  **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)**  cmdlet voor een lijst met alle herstelpunten voor de back-item. Kies het herstelpunt te herstellen. Als u niet zeker welk herstelpunt weet te gebruiken, is het verstandig om te kiezen van de meest recente RecoveryPointType = AppConsistent punt in de lijst.

In het volgende script wordt de variabele **$rp**, is een matrix van herstelpunten voor de geselecteerde back-item van de afgelopen zeven dagen. De matrix is in omgekeerde volgorde gesorteerd tijd met de meest recente herstelpunt bij index 0. Gebruik standaard PowerShell matrix indexeren om op te halen van het herstelpunt. $Rp [0] selecteert in het voorbeeld wordt het meest recente herstelpunt.

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

### <a name="mount-the-disks-of-recovery-point"></a>Koppel de schijven van een herstelpunt

Gebruik de  **[Get-AzureRmRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprpmountscript)**  cmdlet ophalen van het script om alle schijven van de herstelpunten te koppelen.

> [!NOTE]
> De schijven zijn gekoppeld als iSCSI-gekoppelde schijven op de computer waarop het script wordt uitgevoerd. Daarom is bijna onmiddellijk en heeft geen gevolgen voor eventuele kosten
>
>

```
PS C:\> Get-AzureRmRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]

OsType  Password        Filename
------  --------        --------
Windows e3632984e51f496 V2VM_wus2_8287309959960546283_451516692429_cbd6061f7fc543c489f1974d33659fed07a6e0c2e08740.exe
```
Voer het script op de computer waar u de bestanden te herstellen. U moet invoeren van het wachtwoord dat wordt weergegeven boven het script wilt uitvoeren. Nadat de schijven zijn gekoppeld, moet u de Windows Verkenner gebruiken om te bladeren door de nieuwe volumes en bestanden. Raadpleeg voor meer informatie de [bestand herstel documentatie](backup-azure-restore-files-from-vm.md)

### <a name="unmount-the-disks"></a>Ontkoppel de schijven
Nadat de vereiste bestanden zijn gekopieerd, ontkoppel de schijven met de  **[uitschakelen AzureRmRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/disable-azurermrecoveryservicesbackuprpmountscript?view=azurermps-5.0.0)**  cmdlet. Dit wordt sterk aanbevolen omdat hiermee u ervoor zorgt dat toegang tot de bestanden van het herstelpunt is verwijderd

```
PS C:\> Disable-AzureRmRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]
```


## <a name="next-steps"></a>Volgende stappen
Als u liever PowerShell gebruiken om na te gaan met uw Azure-resources, Zie het artikel PowerShell [implementeren en beheren van back-up voor Windows Server](backup-client-automation.md). Als u DPM-back-ups beheren, Zie het artikel [implementeren en beheren van Backup voor DPM](backup-dpm-automation.md). Hebben beide van deze artikelen een versie voor implementaties van Resource Manager en klassieke implementaties.  
