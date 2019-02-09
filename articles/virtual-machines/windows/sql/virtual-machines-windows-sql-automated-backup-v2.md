---
title: Automatische back-up v2 voor Azure VM's SQL Server 2016-2017 | Microsoft Docs
description: Verklaart de functie voor automatische back-up voor SQL Server 2016-2017-VM's die worden uitgevoerd in Azure. In dit artikel is specifiek voor virtuele machines met Resource Manager.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: ebd23868-821c-475b-b867-06d4a2e310c7
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/03/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: e20599833d3073e4819dbc974d4b2afe962ba18a
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984304"
---
# <a name="automated-backup-v2-for-azure-virtual-machines-resource-manager"></a>Automatische back-up v2 voor virtuele Azure-Machines (Resource Manager)

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016/2017](virtual-machines-windows-sql-automated-backup-v2.md)

Geautomatiseerde back-up v2 configureert automatisch [Managed Backup naar Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) voor alle bestaande en nieuwe databases op een virtuele machine van Azure SQL Server 2016-2017 Standard, Enterprise of Developer-edities. Hiermee kunt u het configureren van regelmatige back-ups die gebruikmaken van duurzame Azure blob-opslag. Geautomatiseerde back-up v2 is afhankelijk van de [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van automatische back-up v2, controleert u de volgende vereisten:

**Besturingssysteem**:

- Windows Server 2012 R2
- Windows Server 2016

**SQL Server-versie/editie**:

- SQL Server 2016: Developer, Standard of Enterprise
- SQL Server 2017: Developer, Standard of Enterprise

> [!IMPORTANT]
> Geautomatiseerde back-up v2 werkt met SQL Server 2016 of hoger. Als u SQL Server 2014 gebruikt, kunt u automatische back-up v1 naar back-up van uw databases. Zie voor meer informatie, [automatische Backup voor SQL Server 2014 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

**Databaseconfiguratie**:

- Doeldatabase moeten het volledige herstelmodel gebruiken. Zie voor meer informatie over de impact van het volledige herstelmodel op back-ups, [back-up onder het volledige herstelmodel](https://technet.microsoft.com/library/ms190217.aspx).
- Systeemdatabases geen volledig-herstelmodel te gebruiken. Als u logboekback-ups moeten worden uitgevoerd voor het Model of MSDB nodig hebt, moet u echter volledig-herstelmodel gebruiken.
- Doeldatabases zich op het standaardexemplaar van SQL Server. De SQL Server IaaS-extensie biedt geen ondersteuning voor benoemde exemplaren.

> [!NOTE]
> Automatische back-up is afhankelijk van de **SQL Server IaaS Agent-extensie**. Huidige galerie met installatiekopieën van de SQL-machines met deze extensie standaard toegevoegd. Zie voor meer informatie, [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Instellingen
De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor automatische back-up v2. De werkelijke configuratiestappen variëren afhankelijk van of u de Azure portal of Azure Windows PowerShell-opdrachten gebruiken.

### <a name="basic-settings"></a>Basisinstellingen

| Instelling | Bereik (standaard) | Description |
| --- | --- | --- |
| **Automatische back-up** | In-of uitschakelen (uitgeschakeld) | Hiermee schakelt automatische back-up voor een Azure-VM met SQL Server 2016/2017 Developer, Standard of Enterprise of. |
| **Bewaarperiode** | 1-30 dagen (30 dagen) | Het aantal dagen aan back-ups. |
| **Opslagaccount** | Azure Storage-account | Azure storage-account te gebruiken voor het opslaan van bestanden van geautomatiseerde back-ups in blob-opslag. Een container is gemaakt op deze locatie voor het opslaan van alle back-upbestanden. De naamconventie voor back-upbestand bevat de datum, tijd en GUID van database. |
| **Versleuteling** |In-of uitschakelen (uitgeschakeld) | Hiermee schakelt versleuteling of. Als versleuteling is ingeschakeld, worden de certificaten voor het herstellen van de back-up bevinden zich in het opgegeven opslagaccount. Het maakt gebruik van dezelfde **automaticbackup** container met de dezelfde naamgevingsregel. Als het wachtwoord wordt gewijzigd, wordt een nieuw certificaat met dit wachtwoord wordt gegenereerd, maar wordt het oude certificaat voor het herstellen van eerdere back-ups blijft. |
| **Wachtwoord** |Wachtwoord-tekst | Een wachtwoord voor versleutelingssleutels. Dit wachtwoord is alleen vereist als versleuteling is ingeschakeld. Als u wilt een versleutelde back-ups hebt hersteld, moet u het juiste wachtwoord en het bijbehorende certificaat dat is gebruikt op het moment dat de back-up is gehaald hebben. |

### <a name="advanced-settings"></a>Geavanceerde instellingen

| Instelling | Bereik (standaard) | Description |
| --- | --- | --- |
| **Back-ups van de Database** | In-of uitschakelen (uitgeschakeld) | Wanneer dit is ingeschakeld, deze functie ook back-ups van de systeemdatabases: Master, MSDB en Model. Voor de databases MSDB en het Model en Ga na of deze in de modus voor volledig herstel als u wilt dat de logboekback-ups moeten worden uitgevoerd. Logboekback-ups worden nooit genomen voor Master. En geen back-ups worden genomen voor TempDB. |
| **Back-upschema** | Handmatig/geautomatiseerde (automatisch) | Standaard de back-upschema automatisch bepaald op basis van de groei van het logboek. Handmatige back-upschema kan de gebruiker om op te geven van het tijdvenster voor back-ups. In dit geval plaatsvinden back-ups alleen op de opgegeven frequentie en tijdens de opgegeven periode van een bepaalde dag. |
| **Volledige back-upfrequentie** | Dagelijks/wekelijks | De frequentie van volledige back-ups. In beide gevallen moet beginnen met volledige back-ups tijdens de volgende geplande tijdstip. Wanneer u wekelijks is ingeschakeld, kunnen back-ups over meerdere dagen worden tot alle databases hebt is een back-up. |
| **Begintijd volledige back-up** | 00:00 – 23:00 (01:00) | Begintijd van een bepaalde dag, gedurende welke de volledige back-ups kunnen plaatsvinden. |
| **Tijdvenster volledige back-up** | 1: 23 uur (1 uur) | De duur van het tijdvenster van een bepaalde dag, gedurende welke de volledige back-ups kunnen plaatsvinden. |
| **Logboek-back-upfrequentie** | 5 – 60 minuten (60 minuten) | De frequentie van logboekback-ups. |

## <a name="understanding-full-backup-frequency"></a>Informatie over de volledige back-upfrequentie
Het is belangrijk om te begrijpen van het verschil tussen de dagelijkse en wekelijkse volledige back-ups van. Houd rekening met het volgende voorbeeld van de twee scenario's.

### <a name="scenario-1-weekly-backups"></a>Scenario 1: Wekelijkse back-ups
U hebt een SQL Server-VM met een aantal grote databases.

Op maandag, kunt u automatische back-up v2 inschakelen met de volgende instellingen:

- Back-upschema: **Handmatig**
- Frequentie van volledige back-up: **Weekly**
- Begintijd van volledige back-up: **01:00**
- Tijdvenster volledige back-up: **1 uur**

Dit betekent dat het volgende beschikbare back-upvenster dinsdag om 1 uur voor 1 uur. Op dat moment begint automatische back-up back-ups van uw één bewerking tegelijk. In dit scenario zijn uw databases groot genoeg volledige back-ups uit te voeren voor de eerste paar-databases. Echter na één uur niet alle databases back-ups zijn.

Als dit gebeurt, wordt automatische back-up begint back-ups van de resterende de volgende dag, woensdag om 1 uur gedurende één uur. Als niet alle databases hebt back-ups in die tijd, probeert het opnieuw de volgende dag op hetzelfde moment. Dit gaat door totdat alle databases is back-ups.

Nadat het dinsdag opnieuw hebt bereikt, begint de automatische back-up opnieuw back-ups van alle databases.

In dit scenario laat zien dat automatische back-up alleen binnen het opgegeven tijdvenster werkt en elke database een back-up eenmaal per week. Dit ook wordt aangegeven dat het mogelijk dat back-ups over meerdere dagen worden in het geval waarin het is niet mogelijk voor het voltooien van alle back-ups in een enkele dag.

### <a name="scenario-2-daily-backups"></a>Scenario 2: Dagelijkse back-ups
U hebt een SQL Server-VM met een aantal grote databases.

Op maandag, kunt u automatische back-up v2 inschakelen met de volgende instellingen:

- Back-upschema: Handmatig
- Frequentie van volledige back-up: Dagelijks
- Begintijd van volledige back-up: 22:00
- Tijdvenster volledige back-up: 6 uur

Dit betekent dat het volgende beschikbare back-upvenster maandag om 22 uur gedurende 6 uur. Op dat moment begint automatische back-up back-ups van uw één bewerking tegelijk.

Op dinsdag bij 10 gedurende 6 uur, volledige back-ups van alle databases start vervolgens opnieuw.

> [!IMPORTANT]
> Bij het plannen van dagelijkse back-ups, is het raadzaam dat u een breed tijdvenster om te controleren of dat alle databases kunnen een back-up binnen deze tijd plannen. Dit is vooral belangrijk in het geval waar u een grote hoeveelheid gegevens back-up hebt.

## <a name="configure-in-the-portal"></a>Configureren in de portal

U kunt de Azure-portal gebruiken voor het configureren van automatische back-up v2 tijdens het inrichten of voor bestaande SQL Server 2016-2017-VM's.

## <a name="configure-for-new-vms"></a>Voor nieuwe VM's configureren

De Azure portal gebruiken voor het configureren van automatische back-up v2 wanneer u een nieuwe SQL Server 2016 of 2017-Machine voor in het Resource Manager-implementatiemodel maakt.

In de **SQL Server-instellingen** venster **automatische back-up**. De volgende schermafbeelding van Azure portal bevat de **SQL automatische back-up** instellingen.

![Configuratie van SQL geautomatiseerde back-ups in Azure portal](./media/virtual-machines-windows-sql-automated-backup-v2/automated-backup-blade.png)

> [!NOTE]
> Geautomatiseerde back-up v2 is standaard uitgeschakeld.

## <a name="configure-existing-vms"></a>Bestaande virtuele machines configureren

Voor bestaande SQL Server virtuele machines, selecteert u uw virtuele machine van SQL Server. Selecteer vervolgens de **SQL Server-configuratie** sectie van de virtuele machine **instellingen**.

![SQL automatische back-up voor bestaande VM 's](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration.png)

In de **SQL Server-configuratie** instellingen, klikt u op de **bewerken** knop in de automatische back-sectie.

![SQL automatische back-up voor bestaande VM's configureren](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration-edit.png)

Wanneer u klaar bent, klikt u op de **OK** knop aan de onderkant van de **SQL Server-configuratie** instellingen van uw wijzigingen op te slaan.

Als u automatische back-up voor de eerste keer inschakelt, configureert Azure de SQL Server IaaS Agent op de achtergrond. Gedurende deze tijd de Azure-portal mogelijk niet weergegeven of automatische back-up is geconfigureerd. Wacht enkele minuten voor de agent moet worden geïnstalleerd of geconfigureerd. Daarna wordt de Azure-portal de nieuwe instellingen weergegeven.

## <a name="configure-with-powershell"></a>Configureren met PowerShell

U kunt PowerShell gebruiken om te configureren van automatische back-up v2. Voordat u begint, moet u:

- [Download en installeer de nieuwste Azure PowerShell](https://aka.ms/webpi-azps).
- Open Windows PowerShell en koppel deze aan uw account met de **Connect AzAccount** opdracht.

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

### <a name="install-the-sql-iaas-extension"></a>De SQL IaaS-extensie installeren
Als u een SQL-Server-machine vanuit Azure portal hebt ingericht, wordt de SQL Server IaaS-extensie moet al geïnstalleerd. U kunt bepalen of het voor uw virtuele machine is geïnstalleerd door het aanroepen van **Get-AzVM** opdracht en onderzoek de **extensies** eigenschap.

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions 
```

Als de SQL Server IaaS Agent-extensie is geïnstalleerd, ziet u dat deze als 'SqlIaaSAgent' of 'SQLIaaSExtension' weergegeven. **ProvisioningState** voor de extensie moet ook "Geslaagd" weergegeven. 

Als het is niet geïnstalleerd of kan niet worden ingericht, kunt u deze installeren met de volgende opdracht. Naast de groep van de naam en resourcegroep voor virtuele machine, moet u ook de regio opgeven (**$region**) die uw virtuele machine bevindt zich in.

```powershell
$region = “EASTUS2”
Set-AzVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region 
```

### <a id="verifysettings"></a> Controleer of de huidige instellingen
Als u automatische back-up ingeschakeld tijdens het inrichten, kunt u PowerShell gebruiken om uw huidige configuratie te controleren. Voer de **Get-AzVMSqlServerExtension** opdracht en bekijk de **AutoBackupSettings** eigenschap:

```powershell
(Get-AzVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Deze krijgt u uitvoer die vergelijkbaar is met het volgende:

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

Als de uitvoer ziet u dat **inschakelen** is ingesteld op **False**, hebt u het inschakelen van automatische back-up. Het goede nieuws is dat u inschakelen en configureren van automatische back-up op dezelfde manier. Zie de volgende sectie voor deze informatie.

> [!NOTE] 
> Als u de instellingen controleren onmiddellijk na de wijziging, is het mogelijk dat u weer de oude configuratiewaarden krijgt. Wacht een paar minuten en controleer de instellingen opnieuw voor zorgen dat uw wijzigingen zijn toegepast.

### <a name="configure-automated-backup-v2"></a>Configureren van automatische back-v2
U kunt PowerShell gebruiken om in te schakelen van automatische back-up evenals garantie voor de configuratie en het gedrag op elk gewenst moment wijzigen. 

Eerst, selecteer of maak een opslagaccount voor de back-upbestanden. Het volgende script een storage-account geselecteerd of gemaakt als deze niet bestaat.

```powershell
$storage_accountname = “yourstorageaccount”
$storage_resourcegroupname = $resourcegroupname

$storage = Get-AzStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region } 
```

> [!NOTE]
> Automatische back-up biedt geen ondersteuning voor back-ups van opslag in premium-opslag, maar het kan duren voordat de back-ups van VM-schijven die gebruikmaken van Premium Storage.

Gebruik vervolgens de **New-AzVMSqlServerAutoBackupConfig** opdracht inschakelen en configureren van de automatische back-up v2-instellingen voor het opslaan van back-ups in de Azure storage-account. In dit voorbeeld zijn de back-ups ingesteld op 10 dagen worden bewaard. Back-ups van de database zijn ingeschakeld. Volledige back-ups zijn gepland voor wekelijkse met een bepaalde periode vanaf 20:00 uur gedurende twee uur. Logboekback-ups zijn gepland voor elke 30 minuten. De tweede opdracht **Set AzVMSqlServerExtension**, updates van de opgegeven Azure-VM met deze instellingen.

```powershell
$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname 
```

Het kan enkele minuten om te installeren en configureren van de SQL Server IaaS Agent duren. 

Als versleuteling wilt inschakelen, wijzigt u het vorige script om door te geven de **EnableEncryption** parameter samen met een wachtwoord (beveiligde tekenreeks) voor de **CertificatePassword** parameter. Het volgende script kunt de automatische back-up-instellingen in het vorige voorbeeld en versleuteling wordt toegevoegd.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  

$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Om te bevestigen van de instellingen worden toegepast, [controleren of de configuratie van de automatische back-up](#verifysettings).

### <a name="disable-automated-backup"></a>Automatische back-up uitschakelen
Als u wilt uitschakelen van automatische back-up, Voer hetzelfde script zonder de **-inschakelen** parameter voor de **New-AzVMSqlServerAutoBackupConfig** opdracht. Het ontbreken van de **-inschakelen** parameter geeft u de opdracht uit om de functie uit te schakelen. Net als bij de installatie, kan het enkele minuten om uit te schakelen van automatische back-up duren.

```powershell
$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
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
$backupscheduletype = "Manual"
$fullbackupfrequency = "Weekly"
$fullbackupstarthour = "20"
$fullbackupwindow = "2"
$logbackupfrequency = "30"

# ResourceGroupName is the resource group which is hosting the VM where you are deploying the SQL IaaS Extension 

Set-AzVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region

# Creates/use a storage account to store the backups

$storage = Get-AzStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }

# Configure Automated Backup settings

$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays $retentionperiod -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType $backupscheduletype -FullBackupFrequency $fullbackupfrequency `
    -FullBackupStartHour $fullbackupstarthour -FullBackupWindowInHours $fullbackupwindow `
    -LogBackupFrequencyInMinutes $logbackupfrequency

# Apply the Automated Backup settings to the VM

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="monitoring"></a>Bewaking

U hebt twee manieren voor het controleren van automatische Backup voor SQL Server 2016/2017. Omdat de functie voor SQL Server Managed Backup maakt gebruik van automatische back-up, gelden dezelfde controle technieken voor beide.

U kunt de status eerst opvragen door het aanroepen van [msdb.managed_backup.sp_get_backup_diagnostics](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-get-backup-diagnostics-transact-sql). Of een query is de [msdb.managed_backup.fn_get_health_status](https://docs.microsoft.com/sql/relational-databases/system-functions/managed-backup-fn-get-health-status-transact-sql) tabelfunctie.

Een andere optie is om te profiteren van de ingebouwde functie van Database Mail voor meldingen.

1. Roep de [msdb.managed_backup.sp_set_parameter](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-set-parameter-transact-sql) opgeslagen procedure voor het toewijzen van een e-mailadres voor de **SSMBackup2WANotificationEmailIds** parameter. 
1. Schakel [SendGrid](../../../sendgrid-dotnet-how-to-send-email.md) de e-mailberichten verzenden vanuit de Azure-VM.
1. Gebruik de naam van de SMTP-server en de gebruiker om te configureren van Database Mail. U kunt Database Mail configureren in SQL Server Management Studio of met Transact-SQL-opdrachten. Zie voor meer informatie, [Database Mail](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail).
1. [Configureren van SQL Server Agent voor het gebruik van Database Mail](https://docs.microsoft.com/sql/relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail).
1. Controleer of de SMTP-poort zowel via de firewall van de lokale VM en de netwerkbeveiligingsgroep voor de virtuele machine is toegestaan.

## <a name="next-steps"></a>Volgende stappen
Geautomatiseerde back-up v2 Hiermee configureert u Managed Backup op Azure Virtual machines. Het is dus belangrijk om te [Raadpleeg de documentatie voor beheerde back-up](https://msdn.microsoft.com/library/dn449496.aspx) om te begrijpen van het gedrag en de gevolgen.

U vindt aanvullende back-up en herstellen van de richtlijnen voor het SQL Server op Azure Virtual machines in het volgende artikel: [Back-up en herstel voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

Zie voor meer informatie over andere beschikbare automatiseringstaken [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md).

Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual machines [SQL Server op Azure Virtual Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md).

