---
title: Automatische back-up voor SQL Server Virtual Machines (klassiek) | Microsoft Docs
description: 'Verklaart de functie voor automatische back-up voor SQL Server in Azure Virtual Machines met Resource Manager wordt uitgevoerd. '
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 3333e830-8a60-42f5-9f44-8e02e9868d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/23/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: aeb97d661d330ed6afb3ca5e5e1eb924dacc4024
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58096296"
---
# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Automatische back-up voor SQL Server in Azure Virtual Machines (klassiek)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-automated-backup.md)
> * [Klassiek](../classic/sql-automated-backup.md)
> 
> 

Automatische back-up configureert automatisch [Managed Backup naar Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) voor alle bestaande en nieuwe databases op een Azure-VM met SQL Server 2014 Standard of Enterprise. Hiermee kunt u het configureren van regelmatige back-ups die gebruikmaken van duurzame Azure blob-opslag. Automatische back-up is afhankelijk van de [SQL Server IaaS Agent-extensie](../classic/sql-server-agent-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Voor de Resource Manager-versie van dit artikel, raadpleegt u [automatische back-up voor SQL Server in Azure Resource Manager voor virtuele Machines](../sql/virtual-machines-windows-sql-automated-backup.md).

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van automatische back-up, houd rekening met de volgende vereisten:

**Besturingssysteem**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server-versie/editie**:

* SQL Server 2014 Standard
* SQL Server 2014 Enterprise

> [!NOTE]
> Automatische back-up voor SQL Server 2016 wordt ondersteund met virtuele machines Resource Manager. Zie voor meer informatie, [v2 automatische back-up voor SQL Server 2016 Azure Virtual Machines (Resource Manager)](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup-v2).

**Databaseconfiguratie**:

* Doeldatabase moeten het volledige herstelmodel gebruiken.

**Azure PowerShell**:

* [Installeer de nieuwste Azure PowerShell-opdrachten](/powershell/azure/overview).

**SQL Server IaaS-extensie**:

* [De SQL Server IaaS-extensie installeren](../classic/sql-server-agent-extension.md).

## <a name="settings"></a>Instellingen
De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor automatische back-up. Voor klassieke virtuele machines, moet u PowerShell gebruiken om deze instellingen te configureren.

| Instelling | Bereik (standaard) | Description |
| --- | --- | --- |
| **Automatische back-up** |In-of uitschakelen (uitgeschakeld) |Hiermee schakelt automatische back-up voor een Azure-VM met SQL Server 2014 Standard of Enterprise of. |
| **Bewaarperiode** |1-30 dagen (30 dagen) |Het aantal dagen dat een back-up behouden. |
| **Opslagaccount** |Azure storage-account (de storage-account gemaakt voor de opgegeven virtuele machine) |Azure storage-account te gebruiken voor het opslaan van bestanden van geautomatiseerde back-ups in blob-opslag. Een container is gemaakt op deze locatie voor het opslaan van alle back-upbestanden. De naamconventie voor back-upbestand bevat de datum, tijd en de machinenaam van de. |
| **Versleuteling** |In-of uitschakelen (uitgeschakeld) |Hiermee schakelt versleuteling of. Als versleuteling is ingeschakeld, worden de certificaten voor het herstellen van de back-up bevinden zich in het opgegeven opslagaccount in dezelfde automaticbackup container met behulp van de dezelfde naamgevingsregel. Als het wachtwoord wordt gewijzigd, wordt een nieuw certificaat met dit wachtwoord wordt gegenereerd, maar wordt het oude certificaat voor het herstellen van eerdere back-ups blijft. |
| **Wachtwoord** |Wachtwoord-tekst (geen) |Een wachtwoord voor versleutelingssleutels. Dit is alleen vereist als versleuteling is ingeschakeld. Als u wilt een versleutelde back-ups hebt hersteld, moet u het juiste wachtwoord en het bijbehorende certificaat dat is gebruikt op het moment dat de back-up is gehaald hebben. |
| **Back-up van systeemdatabases** | In-of uitschakelen (uitgeschakeld) | Volledige back-ups maken van Master, Model en MSDB |
| **Back-upschema configureren** | Handmatig/geautomatiseerde (automatisch) | Selecteer **automatisch** automatisch volledige en logboekback-ups op basis van de groei van het logboek. Selecteer **handmatig** om op te geven van de planning voor volledige en logboekback-ups. |

## <a name="configuration-with-powershell"></a>Configuratie met PowerShell
In het volgende PowerShell-voorbeeld, is automatische back-up geconfigureerd voor een bestaande SQL Server 2014-VM. De **New-AzureVMSqlServerAutoBackupConfig** opdracht configureert de instellingen voor automatische back-up voor het opslaan van back-ups in de Azure storage-account dat is opgegeven door de variabele $storageaccount. Deze back-ups voor 10 dagen bewaard. De **Set AzureVMSqlServerExtension** opdracht werkt de opgegeven Azure-VM met deze instellingen.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Het kan enkele minuten om te installeren en configureren van de SQL Server IaaS Agent duren.

Wijzig het vorige script om door te geven van de parameter EnableEncryption samen met een wachtwoord (beveiligde tekenreeks) voor de parameter CertificatePassword voor het inschakelen van versleuteling. Het volgende script kunt de automatische back-up-instellingen in het vorige voorbeeld en versleuteling wordt toegevoegd.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Als u wilt uitschakelen van automatische back-up, Voer hetzelfde script zonder de **-inschakelen** parameter voor de **New-AzureVMSqlServerAutoBackupConfig**. Net als bij de installatie, kan het enkele minuten om uit te schakelen van automatische back-up duren.

> [!NOTE]
> Uitschakelen en verwijderen van de SQL Server IaaS Agent, wordt de eerder geconfigureerde instellingen voor beheerde back-up niet verwijderd. U moet automatische back-up uitschakelen voordat u het uitschakelen of verwijderen van de SQL Server IaaS Agent.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Automatische back-up configureert u Managed Backup op Azure Virtual machines. Het is dus belangrijk om te [Raadpleeg de documentatie voor beheerde back-up](https://msdn.microsoft.com/library/dn449496.aspx) om te begrijpen van het gedrag en de gevolgen.

U vindt aanvullende back-up en herstellen van de richtlijnen voor het SQL Server op Azure Virtual machines in het volgende onderwerp: [Back-up en herstel voor SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-backup-recovery.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).

Zie voor meer informatie over andere beschikbare automatiseringstaken [SQL Server IaaS Agent-extensie](../classic/sql-server-agent-extension.md).

Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual machines [SQL Server op Azure Virtual Machines overzicht](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

