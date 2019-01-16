---
title: Automatische patches voor SQL Server-VM's (klassiek) | Microsoft Docs
description: De functie automatisch patchen voor SQL Server Virtual Machines die worden uitgevoerd in Azure met behulp van de klassieke implementatiemodus uitgelegd.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 737b2f65-08b9-4f54-b867-e987730265a8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/07/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: aa912e3eb76d72e7a79c83d7e51d493310bd36b3
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54331313"
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Automatische patches voor SQL Server in Azure Virtual Machines (klassiek)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-automated-patching.md)
> * [Klassiek](../classic/sql-automated-patching.md)
> 
> 

Geautomatiseerde Patching het vastleggen van een onderhoudsvenster voor een virtuele Machine van Azure met SQL Server. Automatische Updates kunnen alleen worden geïnstalleerd tijdens deze periode. Voor SQL Server, dit het zorgt ervoor dat systeemupdates en alle bijbehorende opnieuw opstarten op de best mogelijke tijd voor de database moet worden uitgevoerd. 

> [!IMPORTANT]
> Alleen Windows-updates zijn gemarkeerd **belangrijk** zijn geïnstalleerd. Andere SQL Server-updates, zoals cumulatieve Updates moeten handmatig worden geïnstalleerd. 

Geautomatiseerde Patching is afhankelijk van de [SQL Server IaaS Agent-extensie](../classic/sql-server-agent-extension.md).

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Voor de Resource Manager-versie van dit artikel, raadpleegt u [automatisch patchen voor SQL Server in Azure Resource Manager voor virtuele Machines](../sql/virtual-machines-windows-sql-automated-patching.md).

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van geautomatiseerde Patching, houd rekening met de volgende vereisten:

**Besturingssysteem**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server-versie**:

* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

**Azure PowerShell**:

* [Installeer de nieuwste Azure PowerShell-opdrachten](/powershell/azure/overview).

**SQL Server IaaS-extensie**:

* [De SQL Server IaaS-extensie installeren](../classic/sql-server-agent-extension.md).

## <a name="settings"></a>Instellingen
De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor het automatisch patchen. Voor klassieke virtuele machines, moet u PowerShell gebruiken om deze instellingen te configureren.

| Instelling | Mogelijke waarden | Description |
| --- | --- | --- |
| **Automatisch patch toepassen** |In-of uitschakelen (uitgeschakeld) |Hiermee schakelt automatisch patchen voor een Azure-machine of. |
| **Onderhoudsplanning** |Elke dag, maandag, dinsdag, woensdag, donderdag, vrijdag, zaterdag, zondag |Het schema voor het downloaden en installeren van Windows, SQL Server en Microsoft-updates voor uw virtuele machine. |
| **Begintijd van onderhoud** |0-24 |De lokale tijd voor het bijwerken van de virtuele machine start. |
| **Duur van het onderhoudsvenster** |30-180 |Het aantal minuten dat is toegestaan om uit te voeren van het downloaden en installeren van updates. |
| **Patch-categorie** |Belangrijk |De categorie van updates te downloaden en installeren. |

## <a name="configuration-with-powershell"></a>Configuratie met PowerShell
In het volgende voorbeeld wordt PowerShell gebruikt voor het configureren van automatisch patchen op een bestaande SQL Server-VM. De **New-AzureVMSqlServerAutoPatchingConfig** opdracht configureert u een nieuw onderhoudsvenster voor automatische updates.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Op basis van dit voorbeeld, beschrijft de volgende tabel de praktische gevolgen zijn voor de doel-Azure-VM:

| Parameter | Effect |
| --- | --- |
| **DayOfWeek** |Patches elke donderdag geïnstalleerd. |
| **MaintenanceWindowStartingHour** |Begin updates om 11:00 uur. |
| **MaintenanceWindowDuration** |Patches moeten worden geïnstalleerd binnen 120 minuten. Op basis van de begintijd, moeten ze voltooien door 13:00 uur. |
| **PatchCategory** |De instelling alleen mogelijk voor deze parameter is "Belangrijk". |

Het kan enkele minuten om te installeren en configureren van de SQL Server IaaS Agent duren.

Als u wilt uitschakelen automatisch patchen, voert u de script zonder de - parameter inschakelen op de New-AzureVMSqlServerAutoPatchingConfig. Net als bij de installatie, kan het enkele minuten om uit te schakelen automatisch patchen duren.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over andere beschikbare automatiseringstaken [SQL Server IaaS Agent-extensie](../classic/sql-server-agent-extension.md).

Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual machines [SQL Server op Azure Virtual Machines overzicht](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

