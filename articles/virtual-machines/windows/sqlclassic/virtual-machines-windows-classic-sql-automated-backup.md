---
title: Automatische back-up voor SQL Server virtuele Machines (klassiek) | Microsoft Docs
description: 'Verklaart de functie voor automatische back-up voor SQL Server wordt uitgevoerd in Azure Virtual Machines die met Resource Manager. '
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 3333e830-8a60-42f5-9f44-8e02e9868d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/23/2018
ms.author: jroth
ms.openlocfilehash: 81910dd9c2743484ca65bd21e400a1a52e961a19
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Automatische back-up voor SQL Server op Azure Virtual Machines (klassiek)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-automated-backup.md)
> * [Klassiek](../classic/sql-automated-backup.md)
> 
> 

Automatische back-up configureert automatisch [Managed Backup naar Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) voor alle bestaande en nieuwe databases op een virtuele machine in Azure met SQL Server 2014 Standard of Enterprise. Hiermee kunt u voor het configureren van regelmatige back-ups die gebruikmaken van duurzame Azure blob-opslag. Automatische back-up is afhankelijk van de [uitbreiding voor SQL Server IaaS-Agent](../classic/sql-server-agent-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. De Resource Manager-versie van dit artikel vindt u [automatische back-up voor SQL Server in Azure Resource Manager voor virtuele Machines](../sql/virtual-machines-windows-sql-automated-backup.md).

## <a name="prerequisites"></a>Vereisten
Houd rekening met de volgende vereisten voor het gebruik van automatische back-up:

**Besturingssysteem**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server-versie /-editie**:

* SQL Server 2014 Standard
* SQL Server 2014 Enterprise

> [!NOTE]
> Automatische back-up voor SQL Server 2016 wordt ondersteund met Resource Manager virtuele machines. Zie voor meer informatie [automatische back-up v2 voor SQL Server 2016 Azure Virtual Machines (Resource Manager)](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup-v2).

**Databaseconfiguratie**:

* Doeldatabases moeten het volledige herstelmodel gebruiken.

**Azure PowerShell**:

* [Installeer de nieuwste Azure PowerShell-opdrachten](/powershell/azure/overview).

**Uitbreiding van SQL Server IaaS**:

* [Installeer de SQL Server IaaS-extensie](../classic/sql-server-agent-extension.md).

## <a name="settings"></a>Instellingen
De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor automatische back-up. Voor klassieke virtuele machines, moet u PowerShell gebruiken om deze instellingen te configureren.

| Instelling | Bereik (standaard) | Beschrijving |
| --- | --- | --- |
| **Automatische back-up** |In-of uitschakelen (uitgeschakeld) |Hiermee schakelt automatische back-up voor een virtuele machine in Azure met SQL Server 2014 Standard of Enterprise of. |
| **Bewaarperiode** |1 tot 30 dagen (30 dagen) |Het aantal dagen wilt bewaren van een back-up. |
| **Storage-Account** |Azure storage-account (de storage-account voor de opgegeven virtuele machine gemaakt) |Een Azure storage-account moet worden gebruikt voor het opslaan van automatische back-up-bestanden in blob-opslag. Een container is gemaakt op deze locatie voor het opslaan van alle back-upbestanden. De naamconventie voor back-upbestand bevat de datum, tijd en de machinenaam van de. |
| **Versleuteling** |In-of uitschakelen (uitgeschakeld) |Hiermee schakelt versleuteling of. Bij versleuteling is ingeschakeld, worden de certificaten voor de back-up herstellen in het opgegeven opslagaccount in dezelfde automaticbackup container met de dezelfde naamgevingsregel gevonden. Als het wachtwoord wordt gewijzigd, wordt een nieuw certificaat wordt gegenereerd met dit wachtwoord, maar het oude certificaat blijft voor het herstellen van eerdere back-ups. |
| **Wachtwoord** |Wachtwoord tekst (geen) |Een wachtwoord voor versleutelingssleutels. Dit is alleen vereist als versleuteling is ingeschakeld. Om een versleutelde back-up herstellen, moet u het juiste wachtwoord en het bijbehorende certificaat dat is gebruikt op het moment dat de back-up is gehaald hebben. | **Back-upsysteem databases** | In-of uitschakelen (uitgeschakeld) | Volledige back-ups van Master, Model en MSDB duren |
| **Back-upschema configureren** | Handmatige/geautomatiseerde (geautomatiseerde) | Selecteer **automatisch** automatisch volledige en logboekback-ups op basis van het logboek groei. Selecteer **handmatige** om op te geven van de planning voor volledige en logboekback-ups. |

## <a name="configuration-with-powershell"></a>Configuratie met PowerShell
In het volgende PowerShell-voorbeeld is automatische back-up geconfigureerd voor een bestaande SQL Server 2014 VM. De **nieuw AzureVMSqlServerAutoBackupConfig** opdracht configureert u de instellingen voor automatische back-up voor het opslaan van back-ups in de Azure storage-account dat is opgegeven door de variabele $storageaccount. Deze back-ups voor 10 dagen bewaard. De **Set AzureVMSqlServerExtension** opdracht opgegeven Azure virtuele machine wordt bijgewerkt met deze instellingen.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Dit kan enige tijd duren om te installeren en configureren van de SQL Server IaaS-Agent.

Het inschakelen van versleuteling het vorige script wijzigen voor de parameter EnableEncryption samen met een wachtwoord (beveiligde tekenreeks) voor de parameter CertificatePassword doorgeven. Het volgende script kunt de instellingen voor automatische back-up in het vorige voorbeeld en versleuteling wordt toegevoegd.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Schakel automatische back-ups uitvoeren hetzelfde script zonder de **-inschakelen** -parameter voor de **nieuw AzureVMSqlServerAutoBackupConfig**. Net als bij de installatie, kan het enkele minuten uitschakelen van automatische back-up duren.

> [!NOTE]
> Uitschakelen en verwijderen van de SQL Server IaaS-Agent wordt niet verwijderd van de eerder geconfigureerde instellingen voor Managed Backup. U moet automatische back-up uitschakelen voordat u het uitschakelen of verwijderen van de SQL Server IaaS-Agent.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Automatische back-up configureert Managed Backup op Azure Virtual machines. Daarom is het belangrijk dat [Raadpleeg de documentatie voor Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) om te begrijpen van het gedrag en de gevolgen.

U vindt aanvullende back-up en herstellen van de richtlijnen voor het SQL Server op Azure VM's in het volgende onderwerp: [back-up en herstel voor SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-backup-recovery.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).

Zie voor meer informatie over andere beschikbare automatiseringstaken [uitbreiding voor SQL Server IaaS-Agent](../classic/sql-server-agent-extension.md).

Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual machines [SQL Server op Azure Virtual Machines-overzicht](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

