---
title: Automatische back-v2 voor virtuele machines in SQL Server 2016/2017 Azure | Microsoft Docs
description: Verklaart de functie voor automatische back-up voor SQL Server 2016/2017 virtuele machines in Azure wordt uitgevoerd. In dit artikel is specifiek voor virtuele machines met behulp van de Resource Manager.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
tags: azure-resource-manager
ms.assetid: ebd23868-821c-475b-b867-06d4a2e310c7
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/03/2018
ms.author: jroth
ms.openlocfilehash: 4619c26e34c90f58702ad286f76a999f83f49cc4
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="automated-backup-v2-for-azure-virtual-machines-resource-manager"></a>Automatische back-v2 voor Azure Virtual Machines (Resource Manager)

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016/2017](virtual-machines-windows-sql-automated-backup-v2.md)

Automatische back-up v2 configureert automatisch [Managed Backup naar Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) voor alle bestaande en nieuwe databases op een virtuele machine in Azure met SQL Server 2016/2017 Standard, Enterprise of Developer-edities. Hiermee kunt u voor het configureren van regelmatige back-ups die gebruikmaken van duurzame Azure blob-opslag. Automatische back-up v2 is afhankelijk van de [uitbreiding voor SQL Server IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van automatische back-up v2, controleert u de volgende vereisten:

**Besturingssysteem**:

- Windows Server 2012 R2
- Windows Server 2016

**SQL Server-versie /-editie**:

- SQL Server 2016: Developer, Standard of Enterprise
- SQL Server 2017: Developer, Standard of Enterprise

> [!IMPORTANT]
> Automatische back-up v2 werkt met SQL Server 2016 of hoger. Als u SQL Server 2014 gebruikt, kunt u automatische back-up v1 back-up van uw databases. Zie voor meer informatie [automatische back-up-voor SQL Server 2014 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

**Databaseconfiguratie**:

- Doeldatabases moeten het volledige herstelmodel gebruiken. Zie voor meer informatie over de impact van het volledige herstelmodel op back-ups, [back-up onder het volledige herstelmodel](https://technet.microsoft.com/library/ms190217.aspx).
- Systeemdatabases hoeft geen volledig herstelmodel te gebruiken. Als u een logboekback-ups moeten worden genomen voor het Model of MSDB vereist, moet u volledig herstelmodel gebruiken.
- Doeldatabases moeten zich op het standaardexemplaar van SQL Server. Benoemde exemplaren wordt niet ondersteund door de SQL Server IaaS-extensie.

> [!NOTE]
> Automatische back-up is afhankelijk van de **uitbreiding voor SQL Server IaaS-Agent**. Huidige SQL-virtuele machine afbeeldingen deze extensie standaard toegevoegd. Zie voor meer informatie [uitbreiding voor SQL Server IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Instellingen
De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor automatische back-up v2. De werkelijke configuratiestappen variëren afhankelijk van of u de Azure portal of Azure Windows PowerShell-opdrachten gebruiken.

### <a name="basic-settings"></a>Basisinstellingen

| Instelling | Bereik (standaard) | Beschrijving |
| --- | --- | --- |
| **Automatische back-up** | In-of uitschakelen (uitgeschakeld) | Hiermee schakelt automatische back-up voor een virtuele machine in Azure met SQL Server 2016/2017 Developer, Standard of Enterprise of. |
| **Bewaarperiode** | 1 tot 30 dagen (30 dagen) | Het aantal dagen wilt bewaren van back-ups. |
| **Opslagaccount** | Azure Storage-account | Een Azure storage-account moet worden gebruikt voor het opslaan van automatische back-up-bestanden in blob-opslag. Een container is gemaakt op deze locatie voor het opslaan van alle back-upbestanden. De naamconventie voor back-upbestand bevat de datum en tijd GUID van database. |
| **Versleuteling** |In-of uitschakelen (uitgeschakeld) | Hiermee schakelt versleuteling of. Als versleuteling is ingeschakeld, worden de certificaten voor de back-up terugzetten bevinden zich in het opgegeven opslagaccount. Dit maakt gebruik van dezelfde **automaticbackup** container met de dezelfde naamgevingsregel. Als het wachtwoord wordt gewijzigd, wordt een nieuw certificaat wordt gegenereerd met dit wachtwoord, maar het oude certificaat blijft voor het herstellen van eerdere back-ups. |
| **Wachtwoord** |Wachtwoord tekst | Een wachtwoord voor versleutelingssleutels. Dit wachtwoord is alleen vereist als versleuteling is ingeschakeld. Om een versleutelde back-up herstellen, moet u het juiste wachtwoord en het bijbehorende certificaat dat is gebruikt op het moment dat de back-up is gehaald hebben. |

### <a name="advanced-settings"></a>Geavanceerde instellingen

| Instelling | Bereik (standaard) | Beschrijving |
| --- | --- | --- |
| **Back-ups van de Database** | In-of uitschakelen (uitgeschakeld) | Wanneer dit is ingeschakeld, deze functie ook een back-up van de systeemdatabases: Master, MSDB en Model. Voor de databases MSDB en het Model en controleert u of ze in de modus voor volledig herstel als u wilt dat de logboekback-ups moeten worden uitgevoerd. Logboekback-ups worden nooit genomen voor Master. En er is geen back-ups worden gemaakt voor TempDB. |
| **Back-upschema** | Handmatige/geautomatiseerde (geautomatiseerde) | Standaard wordt back-upschema automatisch bepaald op basis van de groei van het logboek. Handmatige back-upschema kan de gebruiker om op te geven het tijdvenster voor back-ups. In dit geval plaatsvinden back-ups alleen op de opgegeven frequentie en tijdens het opgegeven tijdvenster van een bepaalde dag. |
| **Volledige back-upfrequentie** | Dagelijks/wekelijks | De frequentie van volledige back-ups. In beide gevallen starten volledige back-ups tijdens het volgende geplande tijdstip-venster. Wanneer u wekelijks is ingeschakeld, kunnen back-ups van meerdere dagen totdat alle databases hebt met succes back-up gemaakt omvatten. |
| **Begintijd van volledige back-up** | 00:00 – 23:00 (01:00) | Begintijd van een bepaalde dag gedurende welke volledige back-ups kunnen worden uitgevoerd. |
| **Volledige back-up tijdvenster** | 1 – 23 uur (1 uur) | De duur van de periode van een bepaalde dag gedurende welke volledige back-ups kunnen worden uitgevoerd. |
| **Back-upfrequentie logboek** | 5 – 60 minuten (60 minuten) | De frequentie van logboekback-ups. |

## <a name="understanding-full-backup-frequency"></a>Volledige back-upfrequentie begrijpen
Het is belangrijk om het verschil tussen de dagelijkse en wekelijkse volledige back-ups van. Houd rekening met de volgende twee voorbeeldscenario's.

### <a name="scenario-1-weekly-backups"></a>Scenario 1: Wekelijkse back-ups
U hebt een SQL Server-VM met een aantal grote databases.

Op maandag, kunt u automatische back-up v2 inschakelen met de volgende instellingen:

- Back-upschema: **handmatig**
- Volledige back-upfrequentie: **wekelijks**
- Volledige back-begintijd: **01:00 uur**
- Volledige back-up tijdvenster: **1 uur**

Dit betekent dat het volgende beschikbare back-upvenster dinsdag op 1 uur 1 uur. Op dat moment wordt begint automatische back-up back-ups van uw databases een tegelijk. In dit scenario zijn uw databases groot genoeg of volledige back-ups worden uitgevoerd voor de eerste paar-databases. Echter na één uur zijn niet alle databases back-ups.

Als dit gebeurt, wordt automatische back-up begint back-ups van de resterende databases op de volgende dag, woensdag op 1 uur gedurende één uur. Als niet alle databases die back-ups zijn in die tijd, wordt geprobeerd opnieuw de volgende dag op hetzelfde moment. Dit gaat door totdat alle databases is back-ups zijn.

Nadat het dinsdag opnieuw bereikt, wordt er automatische back-up begint back-ups van alle databases opnieuw.

Dit scenario ziet u dat automatische back-up alleen binnen het opgegeven tijdvenster werkt en elke database back-up eenmaal per week. Dit betekent ook dat het mogelijk voor back-ups naar meerdere dagen in het geval waar het is niet mogelijk alle back-ups in een enkele dag voltooid is.

### <a name="scenario-2-daily-backups"></a>Scenario 2: Dagelijkse back-ups
U hebt een SQL Server-VM met een aantal grote databases.

Op maandag, kunt u automatische back-up v2 inschakelen met de volgende instellingen:

- Back-upschema: handmatige
- Volledige back-upfrequentie: per dag
- Volledige back-begintijd: 22:00
- Volledige back-up tijdvenster: 6 uur

Dit betekent dat het volgende beschikbare back-upvenster maandag om 10 uur gedurende 6 uur. Op dat moment wordt begint automatische back-up back-ups van uw databases een tegelijk.

Op dinsdag bij 10 gedurende 6 uur, volledige back-ups van alle databases start vervolgens opnieuw.

> [!IMPORTANT]
> Bij het plannen van dagelijkse back-ups, wordt het aanbevolen dat u een breed tijdvenster om te controleren of dat alle databases kunnen een back-up binnen deze tijd plannen. Dit is vooral belangrijk in het geval waar u een grote hoeveelheid gegevens back-up hebt.

## <a name="configure-in-the-portal"></a>Configureren in de portal

U kunt de Azure portal gebruiken voor het configureren van automatische back-up v2 tijdens het inrichten of voor een bestaande SQL Server 2016/2017 virtuele machines.

## <a name="configure-for-new-vms"></a>Voor de nieuwe virtuele machines configureren

De Azure portal gebruiken voor het configureren van automatische back-up v2 wanneer u een nieuwe SQL Server 2016 of 2017 virtuele Machine in het Resource Manager-implementatiemodel maakt.

In de **SQL Server-instellingen** deelvenster **automatische back-up**. De volgende schermopname van Azure portal bevat de **SQL automatische back-up** instellingen.

![Configuratie SQL automatische back-up in Azure-portal](./media/virtual-machines-windows-sql-automated-backup-v2/automated-backup-blade.png)

> [!NOTE]
> Automatische back-up v2 is standaard uitgeschakeld.

## <a name="configure-existing-vms"></a>Bestaande virtuele machines configureren

Selecteer de virtuele machine van SQL Server voor een bestaande SQL Server-virtuele machines. Selecteer vervolgens de **SQL Server-configuratiebestand** sectie van de virtuele machine **instellingen**.

![SQL automatische back-up voor de bestaande virtuele machines](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration.png)

In de **SQL Server-configuratiebestand** instellingen, klikt u op de **bewerken** knop in de automatische back-sectie.

![SQL automatische back-up voor de bestaande virtuele machines configureren](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration-edit.png)

Wanneer u klaar bent, klikt u op de **OK** knop aan de onderkant van de **SQL Server-configuratiebestand** instellingen uw wijzigingen op te slaan.

Als u automatische back-up voor het eerst inschakelt, configureert Azure de SQL Server IaaS-Agent op de achtergrond. Gedurende deze tijd de Azure-portal mogelijk niet weergegeven of automatische back-up is geconfigureerd. Wacht enkele minuten voor de agent moet worden geïnstalleerd of geconfigureerd. Daarna wordt de Azure-portal overeenkomen met de nieuwe instellingen.

## <a name="configure-with-powershell"></a>Configureer met PowerShell

U kunt PowerShell gebruiken voor het configureren van automatische back-up v2. Voordat u begint, moet u het volgende doen:

- [Download en installeer de nieuwste Azure PowerShell](http://aka.ms/webpi-azps).
- Open Windows PowerShell en deze koppelen aan uw account met de **Connect-AzureRmAccount** opdracht.

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

### <a id="verifysettings"></a> Controleer of de huidige instellingen
Als u automatische back-up ingeschakeld tijdens het inrichten, kunt u PowerShell gebruiken om te controleren van uw huidige configuratie. Voer de **Get-AzureRmVMSqlServerExtension** opdracht en bekijk de **AutoBackupSettings** eigenschap:

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Deze krijgt u uitvoer ziet er als volgt:

```
Enable                      : True
EnableEncryption            : False
RetentionPeriod             : 30
StorageUrl                  : https://test.blob.core.windows.net/
StorageAccessKey            :  
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : Manual
FullBackupFrequency         : WEEKLY
FullBackupStartTime         : 2
FullBackupWindowHours       : 2
LogBackupFrequency          : 60
```

Als u de uitvoer ziet dat **inschakelen** is ingesteld op **False**, hebt u automatische back-up inschakelen. Goed nieuws is dat u inschakelen en configureren van automatische back-up op dezelfde manier. Zie de volgende sectie voor deze informatie.

> [!NOTE] 
> Als u de instellingen onmiddellijk na een wijziging selectievakje, is het mogelijk dat u weer de oude configuratiewaarden krijgt. Wacht een paar minuten en controleer de instellingen opnieuw om er zeker van te zijn dat uw wijzigingen zijn toegepast.

### <a name="configure-automated-backup-v2"></a>Automatische back-v2 configureren
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

Gebruik vervolgens de **nieuw AzureRmVMSqlServerAutoBackupConfig** opdracht inschakelen en configureren van de automatische back-up-v2-instellingen voor het opslaan van back-ups in de Azure storage-account. In dit voorbeeld zijn de back-ups ingesteld op 10 dagen worden bewaard. Back-ups van de database zijn ingeschakeld. Volledige back-ups zijn gepland voor de per week met een tijdvenster op starten om 20:00 uur gedurende 2 uur. Logboekback-ups zijn gepland voor elke 30 minuten. De tweede opdracht **Set AzureRmVMSqlServerExtension**, updates van de opgegeven Azure-VM met deze instellingen.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname 
```

Dit kan enige tijd duren om te installeren en configureren van de SQL Server IaaS-Agent. 

Wijzig het vorige script om door te geven voor het inschakelen van versleuteling de **EnableEncryption** parameter samen met een wachtwoord (beveiligde tekenreeks) op voor de **CertificatePassword** parameter. Het volgende script kunt de instellingen voor automatische back-up in het vorige voorbeeld en versleuteling wordt toegevoegd.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

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
$backupscheduletype = "Manual"
$fullbackupfrequency = "Weekly"
$fullbackupstarthour = "20"
$fullbackupwindow = "2"
$logbackupfrequency = "30"

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
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType $backupscheduletype -FullBackupFrequency $fullbackupfrequency `
    -FullBackupStartHour $fullbackupstarthour -FullBackupWindowInHours $fullbackupwindow `
    -LogBackupFrequencyInMinutes $logbackupfrequency

# Apply the Automated Backup settings to the VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="monitoring"></a>Bewaking

U hebt twee mogelijkheden voor het bewaken van geautomatiseerde Backup op SQL Server 2016/2017. Omdat automatische back-up wordt de functie SQL Server Managed Backup gebruikt, zijn dezelfde technieken bewaking van toepassing op.

U kunt de status eerst opvragen door aan te roepen [msdb.managed_backup.sp_get_backup_diagnostics](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-get-backup-diagnostics-transact-sql). Of een query is de [msdb.managed_backup.fn_get_health_status](https://docs.microsoft.com/sql/relational-databases/system-functions/managed-backup-fn-get-health-status-transact-sql) functie met tabelwaarden.

Een andere optie is om te profiteren van de ingebouwde functie van Database Mail voor meldingen.

1. Roep de [msdb.managed_backup.sp_set_parameter](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-set-parameter-transact-sql) opgeslagen procedure voor het toewijzen van een e-mailadres voor de **SSMBackup2WANotificationEmailIds** parameter. 
1. Schakel [SendGrid](../../../sendgrid-dotnet-how-to-send-email.md) de e-mailberichten verzenden vanuit de Azure VM.
1. Gebruik de naam van de SMTP-server en de gebruiker voor het configureren van Database Mail. U kunt Database Mail configureren in SQL Server Management Studio of met Transact-SQL-opdrachten. Zie voor meer informatie [Database Mail](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail).
1. [Configureren van SQL Server Agent voor het gebruik van Database Mail](https://docs.microsoft.com/sql/relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail).
1. Controleer of de SMTP-poort zowel via de lokale firewall voor de virtuele machine en de netwerkbeveiligingsgroep voor de virtuele machine is toegestaan.

## <a name="next-steps"></a>Volgende stappen
Automatische back-up v2 configureert Managed Backup op Azure Virtual machines. Daarom is het belangrijk dat [Raadpleeg de documentatie voor Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) om te begrijpen van het gedrag en de gevolgen.

U vindt aanvullende back-up en herstellen van de richtlijnen voor het SQL Server op Azure VM's in het volgende artikel: [back-up en herstel voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

Zie voor meer informatie over andere beschikbare automatiseringstaken [uitbreiding voor SQL Server IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md).

Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual machines [SQL Server op Azure Virtual Machines-overzicht](virtual-machines-windows-sql-server-iaas-overview.md).

