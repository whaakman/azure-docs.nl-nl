---
title: Automatische back-up voor SQL Server 2014 Azure Virtual Machines | Microsoft Docs
description: Verklaart de functie voor automatische back-up voor SQL Server 2014 VM's die worden uitgevoerd in Azure. In dit artikel is specifiek voor virtuele machines met Resource Manager.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
tags: azure-resource-manager
ms.assetid: bdc63fd1-db49-4e76-87d5-b5c6a890e53c
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/03/2018
ms.author: jroth
ms.openlocfilehash: 0a1ad6d50c624115bab7ad09ff0e30a36e7df500
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51256609"
---
# <a name="automated-backup-for-sql-server-2014-virtual-machines-resource-manager"></a>Automatische back-up voor virtuele Machines (Resource Manager) voor SQL Server 2014

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016/2017](virtual-machines-windows-sql-automated-backup-v2.md)

Automatische back-up configureert automatisch [Managed Backup naar Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) voor alle bestaande en nieuwe databases op een Azure-VM met SQL Server 2014 Standard of Enterprise. Hiermee kunt u het configureren van regelmatige back-ups die gebruikmaken van duurzame Azure blob-opslag. Automatische back-up is afhankelijk van de [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van automatische back-up, houd rekening met de volgende vereisten:

**Besturingssysteem**:

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

**SQL Server-versie/editie**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

> [!IMPORTANT]
> Geautomatiseerde back-up werkt met SQL Server 2014. Als u van SQL Server 2016/2017 gebruikmaakt, kunt u automatische back-up v2 back-up van uw databases. Zie voor meer informatie, [v2 automatische back-up voor SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).

**Databaseconfiguratie**:

- Doeldatabase moeten het volledige herstelmodel gebruiken. Zie voor meer informatie over de impact van het volledige herstelmodel op back-ups, [back-up onder het volledige herstelmodel](https://technet.microsoft.com/library/ms190217.aspx).
- Doeldatabases zich op het standaardexemplaar van SQL Server. De SQL Server IaaS-extensie biedt geen ondersteuning voor benoemde exemplaren.

> [!NOTE]
> Automatische back-up, is afhankelijk van de SQL Server IaaS Agent-extensie. Huidige galerie met installatiekopieën van de SQL-machines met deze extensie standaard toegevoegd. Zie voor meer informatie, [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Instellingen

De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor automatische back-up. De werkelijke configuratiestappen variëren afhankelijk van of u de Azure portal of Azure Windows PowerShell-opdrachten gebruiken.

| Instelling | Bereik (standaard) | Beschrijving |
| --- | --- | --- |
| **Automatische back-up** | In-of uitschakelen (uitgeschakeld) | Hiermee schakelt automatische back-up voor een Azure-VM met SQL Server 2014 Standard of Enterprise of. |
| **Bewaarperiode** | 1-30 dagen (30 dagen) | Het aantal dagen dat een back-up behouden. |
| **Opslagaccount** | Azure Storage-account | Azure storage-account te gebruiken voor het opslaan van bestanden van geautomatiseerde back-ups in blob-opslag. Een container is gemaakt op deze locatie voor het opslaan van alle back-upbestanden. De naamconventie voor back-upbestand bevat de datum, tijd en de machinenaam van de. |
| **Versleuteling** | In-of uitschakelen (uitgeschakeld) | Hiermee schakelt versleuteling of. Als versleuteling is ingeschakeld, de certificaten voor het herstellen van de back-up bevinden zich in het opgegeven opslagaccount in dezelfde `automaticbackup` container met behulp van de dezelfde naamgevingsregel. Als het wachtwoord wordt gewijzigd, wordt een nieuw certificaat met dit wachtwoord wordt gegenereerd, maar wordt het oude certificaat voor het herstellen van eerdere back-ups blijft. |
| **Wachtwoord** | Wachtwoord-tekst | Een wachtwoord voor versleutelingssleutels. Dit is alleen vereist als versleuteling is ingeschakeld. Als u wilt een versleutelde back-ups hebt hersteld, moet u het juiste wachtwoord en het bijbehorende certificaat dat is gebruikt op het moment dat de back-up is gehaald hebben. |

## <a name="configure-in-the-portal"></a>Configureren in de portal

U kunt de Azure-portal gebruiken voor het configureren van automatische back-up tijdens het inrichten of voor bestaande SQL Server 2014-VM's.

## <a name="configure-new-vms"></a>Nieuwe virtuele machines configureren

De Azure portal gebruiken voor het configureren van automatische back-up wanneer u een nieuwe virtuele Machine voor SQL Server 2014 in het Resource Manager-implementatiemodel maakt.

In de **SQL Server-instellingen** venster **automatische back-up**. De volgende schermafbeelding van Azure portal bevat de **SQL automatische back-up** instellingen.

![Configuratie van SQL geautomatiseerde back-ups in Azure portal](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

## <a name="configure-existing-vms"></a>Bestaande virtuele machines configureren

Voor bestaande SQL Server virtuele machines, selecteert u uw virtuele machine van SQL Server. Selecteer vervolgens de **SQL Server-configuratie** sectie van de virtuele machine **instellingen**.

![SQL automatische back-up voor bestaande VM 's](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

In de **SQL Server-configuratie** deelvenster, klikt u op de **bewerken** knop in de automatische back-sectie.

![SQL automatische back-up voor bestaande VM's configureren](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Wanneer u klaar bent, klikt u op de **OK** knop aan de onderkant van de **SQL Server-configuratie** instellingen van uw wijzigingen op te slaan.

Als u automatische back-up voor de eerste keer inschakelt, configureert Azure de SQL Server IaaS Agent op de achtergrond. Gedurende deze tijd de Azure-portal mogelijk niet weergegeven of automatische back-up is geconfigureerd. Wacht enkele minuten voor de agent moet worden geïnstalleerd of geconfigureerd. Daarna wordt de Azure-portal de nieuwe instellingen weergegeven.

> [!NOTE]
> U kunt ook automatische Backup aan de hand van een sjabloon configureren. Zie voor meer informatie, [Azure quickstart-sjabloon voor automatische back-up](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configure-with-powershell"></a>Configureren met PowerShell

U kunt PowerShell gebruiken om te configureren van automatische back-up. Voordat u begint, moet u:

- [Download en installeer de nieuwste Azure PowerShell](https://aka.ms/webpi-azps).
- Open Windows PowerShell en koppel deze aan uw account met de **Connect-AzureRmAccount** opdracht.

### <a name="install-the-sql-iaas-extension"></a>De SQL IaaS-extensie installeren
Als u een SQL-Server-machine vanuit Azure portal hebt ingericht, wordt de SQL Server IaaS-extensie moet al geïnstalleerd. U kunt bepalen of het voor uw virtuele machine is geïnstalleerd door het aanroepen van **Get-AzureRmVM** opdracht en onderzoek de **extensies** eigenschap.

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions
```

Als de SQL Server IaaS Agent-extensie is geïnstalleerd, ziet u dat deze als 'SqlIaaSAgent' of 'SQLIaaSExtension' weergegeven. **ProvisioningState** voor de extensie moet ook "Geslaagd" weergegeven.

Als het is niet geïnstalleerd of kan niet worden ingericht, kunt u deze installeren met de volgende opdracht. Naast de groep van de naam en resourcegroep voor virtuele machine, moet u ook de regio opgeven (**$region**) die uw virtuele machine bevindt zich in.

```powershell
$region = "EASTUS2"
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region
```

> [!IMPORTANT]
> Als de extensie niet al is geïnstalleerd, opnieuw installeren van de extensie de SQL Server-service.

### <a id="verifysettings"></a> Controleer of de huidige instellingen

Als u automatische back-up ingeschakeld tijdens het inrichten, kunt u PowerShell gebruiken om uw huidige configuratie te controleren. Voer de **Get-AzureRmVMSqlServerExtension** opdracht en bekijk de **AutoBackupSettings** eigenschap:

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Deze krijgt u uitvoer die vergelijkbaar is met het volgende:

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

Als de uitvoer ziet u dat **inschakelen** is ingesteld op **False**, hebt u het inschakelen van automatische back-up. Het goede nieuws is dat u inschakelen en configureren van automatische back-up op dezelfde manier. Zie de volgende sectie voor deze informatie.

> [!NOTE] 
> Als u de instellingen controleren onmiddellijk na de wijziging, is het mogelijk dat u weer de oude configuratiewaarden krijgt. Wacht een paar minuten en controleer de instellingen opnieuw voor zorgen dat uw wijzigingen zijn toegepast.

### <a name="configure-automated-backup"></a>Automatische back-up configureren
U kunt PowerShell gebruiken om in te schakelen van automatische back-up evenals garantie voor de configuratie en het gedrag op elk gewenst moment wijzigen.

Eerst, selecteer of maak een opslagaccount voor de back-upbestanden. Het volgende script een storage-account geselecteerd of gemaakt als deze niet bestaat.

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
> Automatische back-up biedt geen ondersteuning voor back-ups van opslag in premium-opslag, maar het kan duren voordat de back-ups van VM-schijven die gebruikmaken van Premium Storage.

Gebruik vervolgens de **New-AzureRmVMSqlServerAutoBackupConfig** opdracht inschakelen en configureren van de automatische back-up-instellingen voor het opslaan van back-ups in de Azure storage-account. In dit voorbeeld worden de back-ups gedurende tien dagen bewaard. De tweede opdracht **Set-AzureRmVMSqlServerExtension**, updates van de opgegeven Azure-VM met deze instellingen.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Het kan enkele minuten om te installeren en configureren van de SQL Server IaaS Agent duren.

> [!NOTE]
> Er zijn andere instellingen voor **New-AzureRmVMSqlServerAutoBackupConfig** die alleen van toepassing op SQL Server 2016 en automatische back-up v2. SQL Server 2014 biedt geen ondersteuning voor de volgende instellingen: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**,  **FullBackupStartHour**, **FullBackupWindowInHours**, en **LogBackupFrequencyInMinutes**. Als u probeert deze instellingen configureren op een virtuele machine van SQL Server 2014, worden er geen fout is opgetreden, maar de instellingen kunnen niet worden toegepast. Als u gebruiken van deze instellingen op een virtuele machine van SQL Server 2016 wilt, Zie [v2 automatische back-up voor SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).

Als versleuteling wilt inschakelen, wijzigt u het vorige script om door te geven de **EnableEncryption** parameter samen met een wachtwoord (beveiligde tekenreeks) voor de **CertificatePassword** parameter. Het volgende script kunt de automatische back-up-instellingen in het vorige voorbeeld en versleuteling wordt toegevoegd.

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

Om te bevestigen van de instellingen worden toegepast, [controleren of de configuratie van de automatische back-up](#verifysettings).

### <a name="disable-automated-backup"></a>Automatische back-up uitschakelen

Als u wilt uitschakelen van automatische back-up, Voer hetzelfde script zonder de **-inschakelen** parameter voor de **New-AzureRmVMSqlServerAutoBackupConfig** opdracht. Het ontbreken van de **-inschakelen** parameter geeft u de opdracht uit om de functie uit te schakelen. Net als bij de installatie, kan het enkele minuten om uit te schakelen van automatische back-up duren.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a>Voorbeeldscript

Het volgende script geeft een reeks variabelen die u aanpassen kunt als u wilt inschakelen en configureren van automatische back-up voor uw virtuele machine. Mogelijk moet u in uw geval wordt het script op basis van uw behoeften aanpassen. U hoeft bijvoorbeeld wijzigingen aanbrengen, als u versleuteling in-of uitschakelen van de back-up van systeemdatabases.

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

## <a name="monitoring"></a>Bewaking

U hebt twee manieren voor het controleren van automatische back-up van SQL Server 2014. Omdat de functie voor SQL Server Managed Backup maakt gebruik van automatische back-up, gelden dezelfde controle technieken voor beide.

U kunt de status eerst opvragen door het aanroepen van [msdb.smart_admin.sp_get_backup_diagnostics](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-get-backup-diagnostics-transact-sql). Of een query is de [msdb.smart_admin.fn_get_health_status](https://docs.microsoft.com/sql/relational-databases/system-functions/managed-backup-fn-get-health-status-transact-sql) tabelfunctie.

> [!NOTE]
> Het schema voor Managed Backup in SQL Server 2014 is **msdb.smart_admin**. In SQL Server 2016 dit gewijzigd in **msdb.managed_backup**, en de onderwerpen met naslaginformatie gebruiken dit nieuwe schema. Maar voor SQL Server 2014, u moet echter ook doorgaan met de **smart_admin** schema voor alle back-up beheerde objecten.

Een andere optie is om te profiteren van de ingebouwde functie van Database Mail voor meldingen.

1. Roep de [msdb.smart_admin.sp_set_parameter](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-set-parameter-transact-sql) opgeslagen procedure voor het toewijzen van een e-mailadres voor de **SSMBackup2WANotificationEmailIds** parameter. 
1. Schakel [SendGrid](../../../sendgrid-dotnet-how-to-send-email.md) de e-mailberichten verzenden vanuit de Azure-VM.
1. Gebruik de naam van de SMTP-server en de gebruiker om te configureren van Database Mail. U kunt Database Mail configureren in SQL Server Management Studio of met Transact-SQL-opdrachten. Zie voor meer informatie, [Database Mail](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail).
1. [Configureren van SQL Server Agent voor het gebruik van Database Mail](https://docs.microsoft.com/sql/relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail).
1. Controleer of de SMTP-poort zowel via de firewall van de lokale VM en de netwerkbeveiligingsgroep voor de virtuele machine is toegestaan.

## <a name="next-steps"></a>Volgende stappen

Automatische back-up configureert u Managed Backup op Azure Virtual machines. Het is dus belangrijk om te [Raadpleeg de documentatie voor Managed Backup in SQL Server 2014](https://msdn.microsoft.com/library/dn449497(v=sql.120).aspx).

U vindt aanvullende back-up en herstellen van de richtlijnen voor het SQL Server op Azure Virtual machines in het volgende artikel: [back-up en herstel voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

Zie voor meer informatie over andere beschikbare automatiseringstaken [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md).

Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual machines [SQL Server op Azure Virtual Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md).
