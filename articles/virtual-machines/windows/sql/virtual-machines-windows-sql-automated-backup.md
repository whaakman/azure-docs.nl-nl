---
title: Automatische back-up voor SQL Server 2014 Azure Virtual Machines | Microsoft Docs
description: Verklaart de functie voor automatische back-up voor SQL Server 2014 virtuele machines in Azure wordt uitgevoerd. In dit artikel is specifiek voor virtuele machines met behulp van de Resource Manager.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: bdc63fd1-db49-4e76-87d5-b5c6a890e53c
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 91aab896dd5f06c950ee0ed8f36cc6a953d91611
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="automated-backup-for-sql-server-2014-virtual-machines-resource-manager"></a>Automatische back-up voor virtuele Machines (Resource Manager) voor SQL Server 2014

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016](virtual-machines-windows-sql-automated-backup-v2.md)

Automatische back-up configureert automatisch [Managed Backup naar Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) voor alle bestaande en nieuwe databases op een virtuele machine in Azure met SQL Server 2014 Standard of Enterprise. Hiermee kunt u voor het configureren van regelmatige back-ups die gebruikmaken van duurzame Azure blob-opslag. Automatische back-up is afhankelijk van de [uitbreiding voor SQL Server IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Vereisten
Houd rekening met de volgende vereisten voor het gebruik van automatische back-up:

**Besturingssysteem**:

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

**SQL Server-versie /-editie**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

> [!IMPORTANT]
> Automatische back-up werkt met SQL Server 2014. Als u SQL Server 2016 gebruikt, kunt u automatische back-up v2 back-up van uw databases. Zie voor meer informatie [automatische back-up v2 voor SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).

**Databaseconfiguratie**:

- Doeldatabases moeten het volledige herstelmodel gebruiken. Zie voor meer informatie over de impact van het volledige herstelmodel op back-ups, [back-up onder het volledige herstelmodel](https://technet.microsoft.com/library/ms190217.aspx).
- Doeldatabases moeten zich op het standaardexemplaar van SQL Server. Benoemde exemplaren wordt niet ondersteund door de SQL Server IaaS-extensie.

**Azure-implementatiemodel**:

- Resource Manager

**Azure PowerShell**:

- [Installeer de nieuwste Azure PowerShell-opdrachten](/powershell/azure/overview) als u van plan bent voor het configureren van automatische back-up met PowerShell.

> [!NOTE]
> Automatische back-up is afhankelijk van de SQL Server IaaS-Agent-extensie. Huidige SQL-virtuele machine afbeeldingen deze extensie standaard toegevoegd. Zie voor meer informatie [uitbreiding voor SQL Server IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Instellingen

De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor automatische back-up. De werkelijke configuratiestappen variëren afhankelijk van of u de Azure portal of Azure Windows PowerShell-opdrachten gebruiken.

| Instelling | Bereik (standaard) | Beschrijving |
| --- | --- | --- |
| **Automatische back-up** | In-of uitschakelen (uitgeschakeld) | Hiermee schakelt automatische back-up voor een virtuele machine in Azure met SQL Server 2014 Standard of Enterprise of. |
| **Bewaarperiode** | 1 tot 30 dagen (30 dagen) | Het aantal dagen wilt bewaren van een back-up. |
| **Storage-Account** | Azure Storage-account | Een Azure storage-account moet worden gebruikt voor het opslaan van automatische back-up-bestanden in blob-opslag. Een container is gemaakt op deze locatie voor het opslaan van alle back-upbestanden. De naamconventie voor back-upbestand bevat de datum, tijd en de machinenaam van de. |
| **Versleuteling** | In-of uitschakelen (uitgeschakeld) | Hiermee schakelt versleuteling of. Als versleuteling is ingeschakeld, de certificaten voor de back-up terugzetten bevinden zich in het opgegeven opslagaccount in dezelfde `automaticbackup` container met behulp van de dezelfde naamgevingsregel. Als het wachtwoord wordt gewijzigd, wordt een nieuw certificaat wordt gegenereerd met dit wachtwoord, maar het oude certificaat blijft voor het herstellen van eerdere back-ups. |
| **Wachtwoord** | Wachtwoord tekst | Een wachtwoord voor versleutelingssleutels. Dit is alleen vereist als versleuteling is ingeschakeld. Om een versleutelde back-up herstellen, moet u het juiste wachtwoord en het bijbehorende certificaat dat is gebruikt op het moment dat de back-up is gehaald hebben. |

## <a name="configuration-in-the-portal"></a>De configuratie in de Portal

U kunt de Azure portal gebruiken voor het configureren van automatische back-up tijdens het inrichten of voor een bestaande SQL Server 2014 virtuele machines.

### <a name="new-vms"></a>Nieuwe virtuele machines

De Azure portal gebruiken voor automatische back-up configureren wanneer u een nieuwe virtuele Machine van SQL Server 2014 in het Resource Manager-implementatiemodel maakt.

In de **SQL Server-instellingen** blade Selecteer **automatische back-up**. De volgende schermopname van Azure portal bevat de **SQL automatische back-up** blade.

![Configuratie SQL automatische back-up in Azure-portal](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

Zie voor context is het volledig onderwerp op [inrichten van een virtuele machine van SQL Server in Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Bestaande virtuele machines

Selecteer de virtuele machine van SQL Server voor een bestaande SQL Server-virtuele machines. Selecteer vervolgens de **SQL Server-configuratiebestand** sectie van de **instellingen** blade.

![SQL automatische back-up voor de bestaande virtuele machines](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

In de **SQL Server-configuratiebestand** blade, klikt u op de **bewerken** knop in de automatische back-sectie.

![SQL automatische back-up voor de bestaande virtuele machines configureren](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Wanneer u klaar bent, klikt u op de **OK** knop aan de onderkant van de **SQL Server-configuratiebestand** blade uw wijzigingen op te slaan.

Als u automatische back-up voor het eerst inschakelt, configureert Azure de SQL Server IaaS-Agent op de achtergrond. Gedurende deze tijd de Azure-portal mogelijk niet weergegeven of automatische back-up is geconfigureerd. Wacht enkele minuten voor de agent moet worden geïnstalleerd of geconfigureerd. Daarna wordt de Azure-portal overeenkomen met de nieuwe instellingen.

> [!NOTE]
> U kunt ook automatische back-up-met een sjabloon configureren. Zie voor meer informatie [Azure quickstart-sjabloon voor automatische back-up](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configuration-with-powershell"></a>Configuratie met PowerShell

U kunt PowerShell gebruiken voor het configureren van automatische back-up. Voordat u begint, moet u het volgende doen:

- [Download en installeer de nieuwste Azure PowerShell](http://aka.ms/webpi-azps).
- Open Windows PowerShell en deze koppelen aan uw account. U kunt dit doen door de stappen in de [configureren van uw abonnement](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) sectie van de inrichting onderwerp.

### <a name="install-the-sql-iaas-extension"></a>De uitbreiding voor de SQL-IaaS installeren
Als u een virtuele machine van SQL Server vanuit de Azure-portal hebt ingericht, de SQL Server IaaS-extensie al is geïnstalleerd. U kunt bepalen of het voor uw virtuele machine is geïnstalleerd door het aanroepen van **Get-AzureRmVM** opdracht en onderzoek de **extensies** eigenschap.

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions
```

Als de uitbreiding van de SQL Server IaaS-Agent is geïnstalleerd, ziet u dat het weergegeven als 'SqlIaaSAgent' of 'SQLIaaSExtension'. **ProvisioningState** voor de extensie 'Geslaagd' moet ook worden weergegeven.

Als dit is niet geïnstalleerd of niet ingericht, kunt u deze kunt installeren met de volgende opdracht. Naast de groep van de naam en de bron voor virtuele machine, moet u ook de regio opgeven (**$region**) waarin uw VM zich bevindt.

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region
```

### <a id="verifysettings"></a>Controleer of de huidige instellingen

Als u automatische back-up ingeschakeld tijdens het inrichten, kunt u PowerShell gebruiken om te controleren van uw huidige configuratie. Voer de **Get-AzureRmVMSqlServerExtension** opdracht en bekijk de **AutoBackupSettings** eigenschap:

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Deze krijgt u uitvoer ziet er als volgt:

```
Enable                      : False
EnableEncryption            : False
RetentionPeriod             : -1
StorageUrl                  : NOTSET
StorageAccessKey            : 
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : 
FullBackupFrequency         : 
FullBackupStartTime         : 
FullBackupWindowHours       : 
LogBackupFrequency          : 
```

Als u de uitvoer ziet dat **inschakelen** is ingesteld op **False**, hebt u automatische back-up inschakelen. Goed nieuws is dat u inschakelen en configureren van automatische back-up op dezelfde manier. Zie de volgende sectie voor deze informatie.

> [!NOTE] 
> Als u de instellingen onmiddellijk na een wijziging selectievakje, is het mogelijk dat u weer de oude configuratiewaarden krijgt. Wacht een paar minuten en controleer de instellingen opnieuw om er zeker van te zijn dat uw wijzigingen zijn toegepast.

### <a name="configure-automated-backup"></a>Automatische back-up configureren
U kunt PowerShell gebruiken om in te schakelen van automatische back-up ook garantie voor de configuratie en het gedrag op elk gewenst moment wijzigen.

Eerst, selecteer of maak een opslagaccount voor de back-upbestanden. Het volgende script selecteert een opslagaccount of gemaakt als deze niet bestaat.

```powershell
$storage_accountname = “yourstorageaccount”
$storage_resourcegroupname = $resourcegroupname

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }
```

> [!NOTE]
> Automatische back-up biedt geen ondersteuning voor back-ups van opslag in premium-opslag, maar het kan duren voordat de back-ups van VM-schijven die met Premium-opslag gebruiken.

Gebruik vervolgens de **nieuw AzureRmVMSqlServerAutoBackupConfig** opdracht inschakelen en configureren van de instellingen voor automatische back-up voor het opslaan van back-ups in de Azure storage-account. In dit voorbeeld zijn de back-ups ingesteld op 10 dagen worden bewaard. De tweede opdracht **Set AzureRmVMSqlServerExtension**, updates van de opgegeven Azure-VM met deze instellingen.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Dit kan enige tijd duren om te installeren en configureren van de SQL Server IaaS-Agent.

> [!NOTE]
> Er zijn andere instellingen voor **nieuw AzureRmVMSqlServerAutoBackupConfig** die alleen van toepassing op SQL Server 2016 en automatische back-up v2. SQL Server 2014 biedt geen ondersteuning voor de volgende instellingen: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**, **FullBackupStartHour**, **FullBackupWindowInHours**, en **LogBackupFrequencyInMinutes**. Als u probeert deze instellingen te configureren op een virtuele machine van SQL Server 2014, er is geen fout, maar kunnen de instellingen niet toegepast. Als u het gebruik van deze instellingen op een virtuele machine van SQL Server 2016 wilt, Zie [automatische back-up v2 voor SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).

Wijzig het vorige script om door te geven voor het inschakelen van versleuteling de **EnableEncryption** parameter samen met een wachtwoord (beveiligde tekenreeks) op voor de **CertificatePassword** parameter. Het volgende script kunt de instellingen voor automatische back-up in het vorige voorbeeld en versleuteling wordt toegevoegd.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Om te bevestigen dat de instellingen worden toegepast, [de configuratie van de automatische back-up](#verifysettings).

### <a name="disable-automated-backup"></a>Uitschakelen van automatische back-up

Schakel automatische back-up uitvoeren hetzelfde script zonder de **-inschakelen** -parameter voor de **nieuw AzureRmVMSqlServerAutoBackupConfig** opdracht. Het ontbreken van de **-inschakelen** parameter geeft u de opdracht uit om de functie uitschakelen. Net als bij de installatie, kan het enkele minuten uitschakelen van automatische back-up duren.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a>Voorbeeldscript

Het volgende script geeft een set van variabelen die u aanpassen kunt als u wilt inschakelen en configureren van automatische back-up voor uw virtuele machine. In het geval is moet u wellicht aanpassen van het script op basis van uw vereisten. U moet bijvoorbeeld wijzigen als u versleuteling in-of uitschakelen van de back-up van systeemdatabases.

```powershell
$vmname = "yourvmname"
$resourcegroupname = "vmresourcegroupname"
$region = “Azure region name such as EASTUS2”
$storage_accountname = “storageaccountname”
$storage_resourcegroupname = $resourcegroupname
$retentionperiod = 10

# ResourceGroupName is the resource group which is hosting the VM where you are deploying the SQL IaaS Extension

Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region

# Creates/use a storage account to store the backups

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }

# Configure Automated Backup settings

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays $retentionperiod -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

# Apply the Automated Backup settings to the VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Volgende stappen

Automatische back-up configureert Managed Backup op Azure Virtual machines. Daarom is het belangrijk dat [Raadpleeg de documentatie voor Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) om te begrijpen van het gedrag en de gevolgen.

U vindt aanvullende back-up en herstellen van de richtlijnen voor het SQL Server op Azure VM's in het volgende onderwerp: [back-up en herstel voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

Zie voor meer informatie over andere beschikbare automatiseringstaken [uitbreiding voor SQL Server IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md).

Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual machines [SQL Server op Azure Virtual Machines-overzicht](virtual-machines-windows-sql-server-iaas-overview.md).

