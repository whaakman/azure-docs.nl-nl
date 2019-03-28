---
title: Back-up en herstellen van virtuele Azure-machines met behulp van back-up van Azure PowerShell
description: Hierin wordt beschreven hoe u back-up en herstellen van virtuele Azure-machines met behulp van back-up van Azure PowerShell
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: raynew
ms.openlocfilehash: 230c68b0b1de1ef452de51b7b0661a3c3786ea76
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521700"
---
# <a name="back-up-and-restore-azure-vms-with-powershell"></a>Back-up en herstellen van virtuele Azure-machines met PowerShell

In dit artikel wordt uitgelegd hoe u back-up en herstellen van een Azure-VM in een [Azure Backup](backup-overview.md) Recovery Services-kluis met behulp van PowerShell-cmdlets. 

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Maak een Recovery Services-kluis en de kluiscontext instellen.
> * Een back-upbeleid definiëren
> * Het back-upbeleid toepassen voor de beveiliging van meerdere virtuele machines
> * Trigger een on-demand back-uptaak voor de beveiligde virtuele machines voordat u kunt een back-up (of Bescherm) een virtuele machine, moet u uitvoeren de [vereisten](backup-azure-arm-vms-prepare.md) om voor te bereiden uw omgeving voor het beveiligen van uw virtuele machines. 




## <a name="before-you-start"></a>Voordat u begint

- [Meer informatie](backup-azure-recovery-services-vault-overview.md) over Recovery Services-kluizen.
- [Bekijk](backup-architecture.md#architecture-direct-backup-of-azure-vms) de architectuur voor Azure VM backup, [meer informatie over](backup-azure-vms-introduction.md) het back-upproces en [bekijken](backup-support-matrix-iaas.md) ondersteuning, beperkingen en vereisten.
- Bekijk de PowerShell-objecthiërarchie voor Recovery Services.


## <a name="recovery-services-object-hierarchy"></a>Recovery Services-objecthiërarchie

De objecthiërarchie worden samengevat in het volgende diagram.

![Recovery Services-objecthiërarchie](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Controleer de **Az.RecoveryServices** [cmdlet-verwijzing](https://docs.microsoft.com/powershell/module/Az.RecoveryServices/?view=azps-1.4.0) verwijzing in de Azure-bibliotheek.



## <a name="set-up-and-register"></a>Instellen en registreren

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Om te beginnen met:

1. [Download de nieuwste versie van PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps)

2. De beschikbare Azure Backup PowerShell-cmdlets vinden door de volgende opdracht te typen:

    ```powershell
    Get-Command *azrecoveryservices*
    ```   
 
    De aliassen en cmdlets voor back-up van Azure, Azure Site Recovery en de Recovery Services-kluis worden weergegeven. De volgende afbeelding is een voorbeeld van wat u ziet. Het is niet de volledige lijst met cmdlets.

    ![lijst met Recovery Services](./media/backup-azure-vms-automation/list-of-recoveryservices-ps.png)

3. Aanmelden bij uw Azure-account met **Connect AzAccount**. Deze cmdlet wordt een webpagina vraagt u om referenties voor uw account:

    * U kunt ook uw accountreferenties opnemen als een parameter in de **Connect AzAccount** cmdlet, met behulp van de **-referentie** parameter.
    * Als u de CSP-partner werken namens een tenant bent, geeft u de klant als een tenant met behulp van de naam van de primaire domeincontroller tenant-id of tenant. Bijvoorbeeld: **Connect-AzAccount -Tenant "fabrikam.com"**

4. Koppel het abonnement dat u gebruiken met het account wilt, omdat een account kan meerdere abonnementen hebben:

    ```powershell
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

5. Als u Azure Backup voor de eerste keer gebruikt, moet u de **[registreren AzResourceProvider](https://docs.microsoft.com/powershell/module/az.resources/register-azresourceprovider)** cmdlet voor het registreren van de Azure Recovery Service-provider met uw abonnement.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. U kunt controleren of de Providers is geregistreerd met de volgende opdrachten:
    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
    In de uitvoer van de opdracht, de **RegistrationState** moet wijzigen naar **geregistreerde**. Als niet het geval is, alleen wordt uitgevoerd de **[registreren AzResourceProvider](https://docs.microsoft.com/powershell/module/az.resources/register-azresourceprovider)** cmdlet opnieuw uit.


## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

De volgende stappen leiden u bij het maken van een Recovery Services-kluis. Een Recovery Services-kluis is anders dan een back-upkluis.

1. De Recovery Services-kluis is een Resource Manager-resource, dus u moet dit binnen een resourcegroep te plaatsen. U kunt een bestaande resourcegroep gebruiken, of maak een resourcegroep met de **[New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup)** cmdlet. Bij het maken van een resourcegroep, geef de naam en locatie voor de resourcegroep.  

    ```powershell
    New-AzResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Gebruik de [New-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/new-azrecoveryservicesvault?view=azps-1.4.0) cmdlet voor het maken van de Recovery Services-kluis. Zorg ervoor dat de dezelfde locatie voor de kluis opgeven dat werd gebruikt voor de resourcegroep.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Geef het type opslagredundantie moet gebruiken. u kunt [lokaal redundante opslag (LRS)](../storage/common/storage-redundancy-lrs.md) of [geografisch redundante opslag (GRS)](../storage/common/storage-redundancy-grs.md). Het volgende voorbeeld ziet dat de optie - BackupStorageRedundancy voor testvault is ingesteld op GeoRedundant.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
    Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > Voor veel Azure Backup-cmdlets is het object Recovery Services-kluis als invoer vereist. Daarom is het handiger het object Backup Recovery Services-kluis in een variabele op te slaan.
   >
   >

## <a name="view-the-vaults-in-a-subscription"></a>De kluizen in een abonnement weergeven

U kunt alle kluizen weergeven in het abonnement, met [Get-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault?view=azps-1.4.0):

```powershell
Get-AzRecoveryServicesVault
```

De uitvoer is vergelijkbaar met het volgende voorbeeld, ziet u de dat locatie en de bijbehorende ResourceGroupName worden geleverd.

```
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

Voordat u de beveiliging op een virtuele machine inschakelt, gebruikt u [Set AzRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext?view=azps-1.4.0) om in te stellen van de context van de kluis. Zodra deze is ingesteld, is deze op alle navolgende cmdlets van toepassing. Het volgende voorbeeld wordt de context van de kluis voor de kluis *testvault*.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a>Een beveiligingsbeleid maken

Als u een Recovery Services-kluis maakt, gaat deze gepaard met standaardbeleid voor beveiliging en retentie. Volgens het standaardbeveiligingsbeleid wordt elke dag op een bepaald tijdstip een back-uptaak getriggerd. Volgens het standaardbewaarbeleid wordt het dagelijkse herstelpunt gedurende dertig dagen bewaard. Het standaardbeleid kunt u snel uw VM te beveiligen en het beleid later met verschillende gegevens te bewerken.

Gebruik **[Get-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy) om het beveiligingsbeleid voor apps beschikbaar in de kluis weer te geven. U kunt deze cmdlet gebruiken om op te halen van een specifiek beleid, of om de beleidsregels die zijn gekoppeld aan een type werkbelasting weer te geven. Het volgende voorbeeld wordt een beleid voor het type werkbelasting, installatiekopieën van virtuele machines.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
```

De uitvoer lijkt op die in het volgende voorbeeld:

```
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> De tijdzone van het veld BackupTime in PowerShell is UTC. Echter, wanneer de back-uptijd wordt weergegeven in de Azure-portal, de tijd wordt aangepast aan uw lokale tijdzone.
>
>

Een back-protection-beleid is gekoppeld aan ten minste één beleid voor Gegevensretentie. Een bewaarbeleid wordt gedefinieerd hoe lang een herstelpunt wordt bewaard voordat deze wordt verwijderd.

- Gebruik [Get-AzRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject) om het standaardretentiebeleid weer te geven.
- Op deze manier kunt u [Get-AzRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject) om op te halen van het standaardbeleid voor planning.
- De [New-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy) met de cmdlet maakt een PowerShell-object dat back-upbeleid informatie bevat.
- De beleidsobjecten schema en de retentie worden gebruikt als invoer voor de cmdlet New-AzRecoveryServicesBackupProtectionPolicy.

Het volgende voorbeeld wordt het beleid voor planning en het bewaarbeleid in variabelen. Het voorbeeld deze variabelen worden gebruikt voor het definiëren van de parameters bij het maken van een beveiligingsbeleid *NewPolicy*.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

De uitvoer lijkt op die in het volgende voorbeeld:

```
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```

### <a name="enable-protection"></a>Beveiliging inschakelen

Als u het beveiligingsbeleid hebt gedefinieerd, moet u nog steeds het beleid voor een item inschakelen. Gebruik [inschakelen AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection) beveiliging in te schakelen. Inschakelen van beveiliging vereist twee objecten - het item en het beleid. Nadat het beleid gekoppeld aan de kluis is, worden de back-werkstroom wordt geactiveerd op het moment dat is gedefinieerd in de planning van het beleid.

De volgende voorbeelden inschakelen van de beveiliging voor het item, V2VM, met behulp van het beleid, NewPolicy. De voorbeelden zijn afhankelijk van of de virtuele machine is versleuteld en wat het type versleuteling.

De beveiliging inschakelen op **resourcemanager-VM's niet-versleutelde**:

```powershell
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Enable-AzRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Als u wilt de beveiliging van versleutelde virtuele machines (versleuteld met behulp van (bek) en KEK) inschakelt, moet u de Azure Backup-service-machtiging voor het lezen van sleutels en geheimen van de key vault geven.

```powershell
Set-AzKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Enable-AzRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

De beveiliging inschakelen op **versleutelde virtuele machines (versleuteld met behulp van de BEK alleen)**, moet u de Azure Backup-service-machtiging voor het lezen van geheimen van de key vault geven.

```powershell
Set-AzKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Enable-AzRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> Als u van de Azure Government-cloud gebruikmaakt, gebruikt u de ff281ffe-705c-4f53-9f37-a40e6f2c68f3 waarde voor de parameter ServicePrincipalName in [Set AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) cmdlet.
>


### <a name="modify-a-protection-policy"></a>Een beveiligingsbeleid wijzigen

Gebruiken voor het wijzigen van het beveiligingsbeleid, [Set AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy) de SchedulePolicy of RetentionPolicy objecten te wijzigen.

Het volgende voorbeeld wordt de bewaarperiode voor herstelpunten en 365 dagen.

```powershell
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
$retPol.DailySchedule.DurationCountInDays = 365
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Set-AzRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a>Een back-up activeren

Gebruik [back-up-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem) voor het activeren van een back-uptaak. Als dit de eerste back-up is, is een volledige back-up. Volgende back-ups duren voordat een incrementele kopie. Zorg ervoor dat u **[Set AzRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext)** om in te stellen van de context van de kluis voordat de back-uptaak wordt geactiveerd. Het volgende voorbeeld wordt ervan uitgegaan dat de context van de kluis is al ingesteld.

```powershell
$namedContainer = Get-AzRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
$item = Get-AzRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
$job = Backup-AzRecoveryServicesBackupItem -Item $item
```

De uitvoer lijkt op die in het volgende voorbeeld:

```
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup              InProgress          4/23/2016                  5:00:30 PM                cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> De tijdzone van de StartTime en EndTime velden in PowerShell is UTC. Echter, wanneer de tijd wordt weergegeven in de Azure-portal, de tijd wordt aangepast aan uw lokale tijdzone.
>
>

## <a name="monitoring-a-backup-job"></a>Bewaking van een back-uptaak

U kunt langlopende bewerkingen, zoals back-uptaken controleren zonder gebruik van de Azure-portal. Als u de status van een taak wordt uitgevoerd, gebruikt de [Get-AzRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob) cmdlet. Deze cmdlet haalt de back-uptaken voor een bepaalde kluis en die vault is opgegeven in de context van de kluis. Het volgende voorbeeld wordt de status van een taak wordt uitgevoerd als een matrix en slaat de status in de $joblist-variabele.

```powershell
$joblist = Get-AzRecoveryservicesBackupJob –Status "InProgress"
$joblist[0]
```

De uitvoer lijkt op die in het volgende voorbeeld:

```
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016                5:00:30 PM                cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

In plaats van deze taken voor de voltooiing - dit is niet nodig als u meer code - polling gebruiken de [wacht AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/wait-azrecoveryservicesbackupjob) cmdlet. Deze cmdlet wordt de uitvoering onderbroken totdat de taak is voltooid of de opgegeven time-outwaarde is bereikt.

```powershell
Wait-AzRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a>Een Azure-VM herstellen

Er is een belangrijk verschil tussen het herstellen van een virtuele machine met behulp van de Azure portal en herstellen van een virtuele machine met behulp van PowerShell. Met PowerShell, is de herstelbewerking is voltooid nadat de schijven en configuratie-informatie van het herstelpunt worden gemaakt. De herstelbewerking maken niet de virtuele machine. Voor het maken van een virtuele machine van de schijf, Zie de sectie [maken van de VM van herstelde schijven](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). Als u niet wilt dat de volledige VM herstellen, maar u wilt herstellen of een aantal bestanden herstellen van een Azure-VM back-up, raadpleegt u de [sectie herstel het bestand](backup-azure-vms-automation.md#restore-files-from-an-azure-vm-backup).

> [!Tip]
> De herstelbewerking wordt de virtuele machine niet maken.
>
>

De volgende afbeelding toont de objecthiërarchie van de RecoveryServicesVault omlaag naar de BackupRecoveryPoint.

![Recovery Services-objecthiërarchie BackupContainer weergeven](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

Voor het herstellen van back-upgegevens, Identificeer het item waarvan een back-up is gemaakt en het herstelpunt die de point-in-time-gegevens bevat. Gebruik [terugzetten AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem) voor het terugzetten van gegevens uit de kluis aan uw account.

De basisstappen voor het herstellen van een virtuele machine van Azure zijn:

* Selecteer de VM.
* Kies een herstelpunt.
* Herstel de schijven.
* De virtuele machine maken van opgeslagen schijven.

### <a name="select-the-vm"></a>Selecteer de virtuele machine

Als u de PowerShell-object waarmee de juiste back-artikel, starten van de container in de kluis en uw eigen manier omlaag in de objecthiërarchie werken. Selecteer de container die staat voor de virtuele machine, gebruikt u de [Get-AzRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupcontainer) cmdlet en doorgeven die u wilt de [Get-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupitem) cmdlet.

```powershell
$namedContainer = Get-AzRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
$backupitem = Get-AzRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Een herstelpunt kiezen

Gebruik de [Get-AzRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprecoverypoint) cmdlet om alle herstelpunten voor de back-upitem weer te geven. Kies vervolgens het herstelpunt te herstellen. Als u niet zeker welk herstelpunt weet te gebruiken, is het raadzaam om te kiezen van de meest recente RecoveryPointType = AppConsistent punt in de lijst.

In het volgende script, de variabele **$rp**, is een matrix van herstelpunten voor de geselecteerde back-upitem van de afgelopen zeven dagen. De matrix is in omgekeerde volgorde gesorteerd op tijd wordt opgelost met de meest recente herstelpunt bij index 0. Gebruik de standaard PowerShell matrix indexeren om op te halen van het herstelpunt. In het voorbeeld selecteert $rp [0] het meest recente herstelpunt.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
$rp[0]
```

De uitvoer lijkt op die in het volgende voorbeeld:

```powershell
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

Gebruik de [terugzetten AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem) cmdlet om te herstellen van gegevens en configuratie van een back-upitem naar een herstelpunt. Wanneer u een herstelpunt hebt geïdentificeerd, gebruikt u deze als de waarde voor de **- RecoveryPoint** parameter. In het bovenstaande voorbeeld **$rp [0]** is van het herstelpunt dat u wilt gebruiken. In de volgende voorbeeldcode **$rp [0]** is van het herstelpunt dat u wilt gebruiken voor het herstellen van de schijf.

De schijven en configuratie-informatie terugzetten:

```powershell
$restorejob = Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
$restorejob
```

#### <a name="restore-managed-disks"></a>Beheerde schijven terugzetten

> [!NOTE]
> Als de back-ups virtuele machine schijven beheerde er en u wilt deze als beheerde schijven terugzetten, hebben we de mogelijkheid van Azure PowerShell-RM-module v 6.7.0 geïntroduceerd. en hoger
>
>

Geef een extra parameter **TargetResourceGroupName** om op te geven van de RG waarmee beheerde schijven wordt hersteld. 

> [!NOTE]
> Het wordt sterk aanbevolen gebruik van de **TargetResourceGroupName** parameter voor het herstellen van beheerde schijven omdat deze aanzienlijke prestatieverbeteringen leidt. Ook met Az van Azure Powershell-module 1.0 en hoger is deze parameter verplicht in het geval van een herstelpunt met beheerde schijven
>
>


```powershell
$restorejob = Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG" -TargetResourceGroupName "DestRGforManagedDisks"
```

De **VMConfig.JSON** bestand wordt hersteld naar het opslagaccount en de beheerde schijven worden hersteld met het opgegeven doel-RG.

De uitvoer lijkt op die in het volgende voorbeeld:

```powershell
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Gebruik de [wacht AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/wait-azrecoveryservicesbackupjob) cmdlet na afloop van de hersteltaak om te voltooien.

```powershell
Wait-AzRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

Als de hersteltaak is voltooid, gebruikt u de [Get-AzRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/az.recoveryservices/wait-azrecoveryservicesbackupjob) cmdlet om op te halen van de details van de herstelbewerking opnieuw. De eigenschap JobDetails heeft de informatie die nodig zijn voor de virtuele machine opnieuw.

```powershell
$restorejob = Get-AzRecoveryServicesBackupJob -Job $restorejob
$details = Get-AzRecoveryServicesBackupJobDetails -Job $restorejob
```

Nadat u de schijven herstellen, gaat u naar de volgende sectie om de VM te maken.

## <a name="create-a-vm-from-restored-disks"></a>Een virtuele machine maken van herstelde schijven

Nadat u de schijven herstellen, gebruikt u de volgende stappen uit te maken en configureren van de virtuele machine van de schijf.

> [!NOTE]
> Voor het maken van versleutelde virtuele machines van herstelde schijven, uw Azure-rol moet gemachtigd zijn om uit te voeren van de actie **Microsoft.KeyVault/vaults/deploy/action**. Als uw rol is niet gemachtigd dit, moet u een aangepaste rol maken met deze actie. Zie voor meer informatie, [aangepaste rollen in Azure RBAC](../role-based-access-control/custom-roles.md).
>
>

> [!NOTE]
> Na het herstellen van schijven, kunt u nu een sjabloon voor de implementatie die u rechtstreeks gebruiken kunt om een nieuwe VM te maken krijgen. Niet meer verschillende PS-cmdlets voor het maken van beheerde/niet-beheerde virtuele machines die versleuteld/niet-versleuteld zijn.

De resulterende taakdetails biedt de sjabloon URI die kan worden opgevraagd en geïmplementeerd.

```powershell
   $properties = $details.properties
   $templateBlobURI = $properties["Template Blob Uri"]
```

Alleen implementeren de sjabloon voor het maken van een nieuwe virtuele machine, zoals wordt beschreven [hier](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment ResourceGroupName ExampleResourceGroup -TemplateUri $templateBlobURI -storageAccountType Standard_GRS
```

De volgende sectie bevat stappen die nodig zijn voor het maken van een virtuele machine met behulp van 'UnInitialize'-bestand.

> [!NOTE]
> Het is raadzaam gebruik van de sjabloon voor de implementatie hierboven om een VM te maken. In deze sectie (1-6 punten) zal binnenkort worden afgeschaft.

1. Query uitvoeren op de herstelde schijf-eigenschappen voor de taakdetails.

   ```powershell
   $properties = $details.properties
   $storageAccountName = $properties["Target Storage Account Name"]
   $containerName = $properties["Config Blob Container Name"]
   $configBlobName = $properties["Config Blob Name"]
   ```

2. Stel de context van de Azure-opslag en herstel van de JSON-configuratiebestand.

   ```powershell
   Set-AzCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
   $destination_path = "C:\vmconfig.json"
   Get-AzStorageBlobContent -Container $containerName -Blob $configBlobName -Destination $destination_path
   $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
   ```

3. De JSON-configuratiebestand gebruiken om de configuratie van de virtuele machine te maken.

   ```powershell
   $vm = New-AzVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
   ```

4. Koppel de besturingssysteemschijf en gegevensschijven. Deze stap bevat voorbeelden voor verschillende beheerd en versleutelde VM-configuraties. Gebruik het voorbeeld dat aansluit op uw VM-configuratie.

   * **Niet-beheerde en niet-versleutelde VM's** -gebruik het volgende voorbeeld voor niet-beheerde, niet-versleutelde VM's.

       ```powershell
       Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
       $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
       foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
       {
        $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
       }
       ```

   * **Niet-beheerde en versleutelde virtuele machines met Azure AD (alleen BEK)** -voor niet-beheerde, versleutelde virtuele machines met Azure AD (versleuteld met behulp van de BEK alleen) die u wilt herstellen van het geheim aan de sleutelkluis, voordat u schijven kunt koppelen. Zie voor meer informatie de [een versleutelde virtuele machine herstellen vanaf een herstelpunt voor Azure Backup](backup-azure-restore-key-secret.md). Het volgende voorbeeld laat zien hoe besturingssysteem en gegevensschijven koppelen voor versleutelde virtuele machines. Bij het instellen van de besturingssysteemschijf, zorg ervoor dat het relevante type besturingssysteem vermeld.

      ```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $dekUrl = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows/Linux
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
      foreach($dd in $obj.'properties.storageProfile'.dataDisks)
      {
       $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
      }
      ```

   * **Niet-beheerde en versleutelde virtuele machines met Azure AD (BEK en KEK)** : voor niet-beheerde, versleutelde virtuele machines in Azure AD (versleuteld met behulp van (bek) en KEK), de sleutel en -geheim voor de key vault herstellen voordat u de schijven koppelt. Zie voor meer informatie, [een versleutelde virtuele machine herstellen vanaf een herstelpunt voor Azure Backup](backup-azure-restore-key-secret.md). Het volgende voorbeeld laat zien hoe besturingssysteem en gegevensschijven koppelen voor versleutelde virtuele machines.

      ```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
      foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
      ```

   * **Niet-beheerde en versleutelde virtuele machines zonder Azure AD (alleen BEK)** : voor niet-beheerde, versleutelde virtuele machines zonder Azure AD (versleuteld met behulp van de BEK alleen), als bron **keyVault en-geheim zijn niet beschikbaar** de geheimen herstellen tot key vault met behulp van de procedure in [een niet-versleutelde VM herstellen vanaf een herstelpunt voor Azure Backup](backup-azure-restore-key-secret.md). Voer de volgende scripts voor het instellen van Versleutelingsgegevens voor de herstelde OS-blob (deze stap is niet vereist voor gegevensblob). De $dekurl kunnen worden opgehaald uit de herstelde keyVault.<br>

   Het onderstaande script moet worden uitgevoerd alleen als de Key Vault en-geheim voor bron niet beschikbaar is.

      ```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      $encSetting = "{""encryptionEnabled"":true,""encryptionSettings"":[{""diskEncryptionKey"":{""sourceVault"":{""id"":""$keyVaultId""},""secretUrl"":""$dekUrl""}}]}"
      $osBlobName = $obj.'properties.StorageProfile'.osDisk.name + ".vhd"
      $osBlob = Get-AzStorageBlob -Container $containerName -Blob $osBlobName
      $osBlob.ICloudBlob.Metadata["DiskEncryptionSettings"] = $encSetting
      $osBlob.ICloudBlob.SetMetadata()
      ```

    Na de **geheimen zijn beschikbaar** en de Versleutelingsgegevens ook zijn ingesteld op de OS-Blob, koppel de schijven die met het onderstaande script.<br>

    Als de bron van de keyVault/geheimen al beschikbaar zijn, moet klikt u vervolgens het bovenstaande script niet worden uitgevoerd.

      ```powershell
      Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
      foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
      {
      $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
      }
      ```

   * **Niet-beheerde en versleutelde virtuele machines zonder Azure AD (BEK en KEK)** : voor niet-beheerde, versleutelde virtuele machines zonder Azure AD (versleuteld met de BEK & KEK), als bron **keyVault/sleutels/geheimen zijn niet beschikbaar** de sleutel herstellen en geheimen tot key vault met behulp van de procedure in [een niet-versleutelde VM herstellen vanaf een herstelpunt voor Azure Backup](backup-azure-restore-key-secret.md). Voer de volgende scripts voor het instellen van Versleutelingsgegevens voor de herstelde OS-blob (deze stap is niet vereist voor gegevensblob). De $dekurl en $kekurl kunnen worden opgehaald uit de herstelde keyVault.

   Het onderstaande script moet worden uitgevoerd alleen als de bron keyVault/sleutels/geheimen niet beschikbaar is.

    ```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $kekUrl = "https://ContosoKeyVault.vault.azure.net/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      $encSetting = "{""encryptionEnabled"":true,""encryptionSettings"":[{""diskEncryptionKey"":{""sourceVault"":{""id"":""$keyVaultId""},""secretUrl"":""$dekUrl""},""keyEncryptionKey"":{""sourceVault"":{""id"":""$keyVaultId""},""keyUrl"":""$kekUrl""}}]}"
      $osBlobName = $obj.'properties.StorageProfile'.osDisk.name + ".vhd"
      $osBlob = Get-AzStorageBlob -Container $containerName -Blob $osBlobName
      $osBlob.ICloudBlob.Metadata["DiskEncryptionSettings"] = $encSetting
      $osBlob.ICloudBlob.SetMetadata()
      ```
   Na de **sleutel/geheimen zijn beschikbaar** en de details van de versleuteling worden ingesteld op de OS-Blob en koppel de schijven die met het onderstaande script.

    Als de bron keyVault/sleutel/geheimen beschikbaar zijn, moet klikt u vervolgens het bovenstaande script niet worden uitgevoerd.

    ```powershell
      Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
      foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
      {
      $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
      }
      ```

   * **Beheerde en niet-versleutelde VM's** - voor beheerde niet-versleutelde virtuele machines en koppel de herstelde beheerde schijven. Zie voor gedetailleerde informatie [een gegevensschijf koppelen aan een Windows-VM met behulp van PowerShell](../virtual-machines/windows/attach-disk-ps.md).

   * **Beheerd en versleutelde virtuele machines met Azure AD (alleen BEK)** : als u beheerde versleutelde virtuele machines in Azure AD (versleuteld met behulp van de BEK alleen), koppel de herstelde beheerde schijven. Zie voor gedetailleerde informatie [een gegevensschijf koppelen aan een Windows-VM met behulp van PowerShell](../virtual-machines/windows/attach-disk-ps.md).

   * **Beheerd en versleutelde virtuele machines met Azure AD (BEK en KEK)** : als u beheerde versleutelde virtuele machines in Azure AD (versleuteld met behulp van (bek) en KEK), koppel de herstelde beheerde schijven. Zie voor gedetailleerde informatie [een gegevensschijf koppelen aan een Windows-VM met behulp van PowerShell](../virtual-machines/windows/attach-disk-ps.md).

   * **Beheerd en versleutelde virtuele machines zonder Azure AD (alleen BEK)** -voor die worden beheerd, versleutelde virtuele machines zonder Azure AD (versleuteld met behulp van de BEK alleen), als bron **keyVault en-geheim zijn niet beschikbaar** herstellen de geheimen sleutelkluis met de de procedure in [een niet-versleutelde VM herstellen vanaf een herstelpunt voor Azure Backup](backup-azure-restore-key-secret.md). Voer de volgende scripts voor het instellen van Versleutelingsgegevens voor de herstelde OS-schijf (deze stap is niet vereist voor de gegevensschijf). De $dekurl kunnen worden opgehaald uit de herstelde keyVault.

     Het onderstaande script moet worden uitgevoerd alleen als de Key Vault en-geheim voor bron niet beschikbaar is.  

     ```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      $diskupdateconfig = New-AzDiskUpdateConfig -EncryptionSettingsEnabled $true
      $diskupdateconfig = Set-AzDiskUpdateDiskEncryptionKey -DiskUpdate $diskupdateconfig -SecretUrl $dekUrl -SourceVaultId $keyVaultId  
      Update-AzDisk -ResourceGroupName "testvault" -DiskName $obj.'properties.StorageProfile'.osDisk.name -DiskUpdate $diskupdateconfig
      ```

     Nadat de geheimen beschikbaar zijn en de details van de versleuteling worden ingesteld op de schijf met het besturingssysteem, als u wilt koppelen van de herstelde beheerde schijven, Zie [een gegevensschijf koppelen aan een Windows-VM met behulp van PowerShell](../virtual-machines/windows/attach-disk-ps.md).

   * **Beheerd en versleutelde virtuele machines zonder Azure AD (BEK en KEK)** : als u beheerde, versleutelde virtuele machines zonder Azure AD (versleuteld met behulp van de BEK & KEK), als bron **keyVault/sleutels/geheimen zijn niet beschikbaar** herstellen van de sleutel en geheimen op sleutel met behulp van de procedure in kluis [een niet-versleutelde VM herstellen vanaf een herstelpunt voor Azure Backup](backup-azure-restore-key-secret.md). Voer de volgende scripts voor het instellen van Versleutelingsgegevens voor de herstelde OS-schijf (deze stap is niet vereist voor de gegevensschijf). De $dekurl en $kekurl kunnen worden opgehaald uit de herstelde keyVault.

   Het onderstaande script moet worden uitgevoerd alleen als de bron keyVault/sleutels/geheimen niet beschikbaar is.

   ```powershell
     $dekUrl = "https://ContosoKeyVault.vault.azure.net/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
     $kekUrl = "https://ContosoKeyVault.vault.azure.net/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
     $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
     $diskupdateconfig = New-AzDiskUpdateConfig -EncryptionSettingsEnabled $true
     $diskupdateconfig = Set-AzDiskUpdateDiskEncryptionKey -DiskUpdate $diskupdateconfig -SecretUrl $dekUrl -SourceVaultId $keyVaultId  
     $diskupdateconfig = Set-AzDiskUpdateKeyEncryptionKey -DiskUpdate $diskupdateconfig -KeyUrl $kekUrl -SourceVaultId $keyVaultId  
     Update-AzDisk -ResourceGroupName "testvault" -DiskName $obj.'properties.StorageProfile'.osDisk.name -DiskUpdate $diskupdateconfig
    ```

    Nadat de sleutel-/ geheimen beschikbaar zijn en de details van de versleuteling worden ingesteld op de schijf met het besturingssysteem, als u wilt koppelen van de herstelde beheerde schijven, Zie [een gegevensschijf koppelen aan een Windows-VM met behulp van PowerShell](../virtual-machines/windows/attach-disk-ps.md).

5. Stel de netwerkinstellingen.

    ```powershell
    $nicName="p1234"
    $pip = New-AzPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName "test" -Location "WestUS" -Name "testvNET" -AddressPrefix 10.0.0.0/16
    $virtualNetwork | Set-AzVirtualNetwork
    $vnet = Get-AzVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    $subnetindex=0
    $nic = New-AzNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    $vm=Add-AzVMNetworkInterface -VM $vm -Id $nic.Id
    ```

6. Maak de virtuele machine.

    ```powershell  
    New-AzVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

7. Push-ade-extensie.

   * **Voor de virtuele machine met Azure AD** -gebruik de volgende opdracht handmatig kunnen versleuteling inschakelen voor de gegevensschijven  

     **Alleen de BEK**

      ```powershell  
      Set-AzVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -VolumeType Data
      ```

     **(Bek) en KEK-sleutel**

      ```powershell  
      Set-AzVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId  -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -VolumeType Data
      ```

   * **Voor de virtuele machine zonder Azure AD** -gebruik de volgende opdracht aan de versleuteling voor de gegevensschijven handmatig in te schakelen.

     Als incase tijdens de uitvoering van de opdracht het gevraagd AADClientID, moet u bijwerken van uw Azure-PowerShell.

     **Alleen de BEK**

      ```powershell  
      Set-AzVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -SkipVmBackup -VolumeType "All"
      ```

      **(Bek) en KEK-sleutel**

      ```powershell  
      Set-AzVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -SkipVmBackup -VolumeType "All"
      ```

## <a name="restore-files-from-an-azure-vm-backup"></a>Bestanden herstellen vanaf een back-up van virtuele Azure-machine

Naast het herstellen van schijven, kunt u ook afzonderlijke bestanden herstellen vanuit een Azure-VM back-up. De functionaliteit van de bestanden terugzetten biedt toegang tot alle bestanden in een herstelpunt. De bestanden via Verkenner beheren zoals u zou voor normale bestanden doen.

De eenvoudige stappen een bestand herstelt vanuit een Azure-VM back-up zijn:

* Selecteer de virtuele machine
* Een herstelpunt kiezen
* De schijven van het herstelpunt gekoppeld
* Kopieer de benodigde bestanden
* Ontkoppel de schijf

### <a name="select-the-vm"></a>Selecteer de virtuele machine

Als u de PowerShell-object waarmee de juiste back-artikel, starten van de container in de kluis en uw eigen manier omlaag in de objecthiërarchie werken. Selecteer de container die staat voor de virtuele machine, gebruikt u de [Get-AzRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupcontainer) cmdlet en doorgeven die u wilt de [Get-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupitem) cmdlet.

```powershell
$namedContainer = Get-AzRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
$backupitem = Get-AzRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Een herstelpunt kiezen

Gebruik de [Get-AzRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprecoverypoint) cmdlet om alle herstelpunten voor de back-upitem weer te geven. Kies vervolgens het herstelpunt te herstellen. Als u niet zeker welk herstelpunt weet te gebruiken, is het raadzaam om te kiezen van de meest recente RecoveryPointType = AppConsistent punt in de lijst.

In het volgende script, de variabele **$rp**, is een matrix van herstelpunten voor de geselecteerde back-upitem van de afgelopen zeven dagen. De matrix is in omgekeerde volgorde gesorteerd op tijd wordt opgelost met de meest recente herstelpunt bij index 0. Gebruik de standaard PowerShell matrix indexeren om op te halen van het herstelpunt. In het voorbeeld selecteert $rp [0] het meest recente herstelpunt.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
$rp[0]
```

De uitvoer lijkt op die in het volgende voorbeeld:

```
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

Gebruik de [Get-AzRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprpmountscript) cmdlet om op te halen van het script om alle schijven van het herstelpunt te koppelen.

> [!NOTE]
> De schijven zijn gekoppeld als iSCSI-gekoppelde schijven aan de machine waarop het script wordt uitgevoerd. Koppelen gebeurt onmiddellijk en u geen kosten.
>
>

```powershell
Get-AzRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]
```

De uitvoer lijkt op die in het volgende voorbeeld:

```powershell
OsType  Password        Filename
------  --------        --------
Windows e3632984e51f496 V2VM_wus2_8287309959960546283_451516692429_cbd6061f7fc543c489f1974d33659fed07a6e0c2e08740.exe
```

Voer het script op de computer waar u om de bestanden te herstellen. U kunt het opgegeven wachtwoord moet invoeren voor het uitvoeren van het script. Nadat de schijven zijn gekoppeld, gebruikt u Windows Verkenner om te bladeren naar de nieuwe volumes en bestanden. Raadpleeg voor meer informatie het artikel back-up [bestanden herstellen vanuit back-up van virtuele Azure-machine](backup-azure-restore-files-from-vm.md).

### <a name="unmount-the-disks"></a>De schijven ontkoppelen

Nadat de vereiste bestanden zijn gekopieerd, gebruikt u [uitschakelen AzRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackuprpmountscript) naar de schijven ontkoppelen. Zorg ervoor dat de schijven ontkoppelen, zodat de toegang tot de bestanden van het herstelpunt is verwijderd.

```powershell
Disable-AzRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]
```

## <a name="next-steps"></a>Volgende stappen

Als u liever PowerShell gebruiken om te communiceren met uw Azure-resources, Zie het artikel PowerShell [implementeren en beheren van back-up voor Windows Server](backup-client-automation.md). Als u DPM back-ups beheren, Zie het artikel [implementeren en beheren van Backup voor DPM](backup-dpm-automation.md). 
