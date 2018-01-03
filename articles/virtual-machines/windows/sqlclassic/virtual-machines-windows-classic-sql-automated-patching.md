---
title: Automatisch patchen voor SQL Server-VM's (klassiek) | Microsoft Docs
description: De functie automatisch patchen voor SQL Server virtuele Machines die worden uitgevoerd in Azure met behulp van de klassieke implementatiemodus uitgelegd.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 737b2f65-08b9-4f54-b867-e987730265a8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 1959871141f196ba80ffd7b37e62e5ea5b42dba3
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Automatisch patchen voor SQL Server op Azure Virtual Machines (klassiek)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-automated-patching.md)
> * [Klassiek](../classic/sql-automated-patching.md)
> 
> 

Geautomatiseerde Patching stelt u een onderhoudsvenster voor een virtuele Machine van Azure met SQL Server. Automatische Updates kunnen alleen worden geïnstalleerd tijdens deze periode. Voor SQL Server, zorgt dit ervoor systeemupdates en eventuele bijbehorende opnieuw is opgestart bij de best mogelijke voor de database optreden. Geautomatiseerde Patching is afhankelijk van de [uitbreiding voor SQL Server IaaS-Agent](../classic/sql-server-agent-extension.md).

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. De Resource Manager-versie van dit artikel vindt u [automatisch patchen voor SQL Server in Azure Resource Manager voor virtuele Machines](../sql/virtual-machines-windows-sql-automated-patching.md).

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van automatisch patchen, kunt u overwegen de volgende vereisten:

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

**Uitbreiding van SQL Server IaaS**:

* [Installeer de SQL Server IaaS-extensie](../classic/sql-server-agent-extension.md).

## <a name="settings"></a>Instellingen
De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor automatisch patchen. Voor klassieke virtuele machines, moet u PowerShell gebruiken om deze instellingen te configureren.

| Instelling | Mogelijke waarden | Beschrijving |
| --- | --- | --- |
| **Automatisch patch toepassen** |In-of uitschakelen (uitgeschakeld) |Hiermee schakelt automatisch patchen voor een virtuele machine van Azure of. |
| **Onderhoudsplanning** |Dagelijks, maandag, dinsdag, woensdag, donderdag, vrijdag, zaterdag, zondag |De planning voor het downloaden en installeren van Windows, SQL Server en de Microsoft-updates voor uw virtuele machine. |
| **Beginuur van het onderhoud** |0-24 |De lokale tijd voor het bijwerken van de virtuele machine start. |
| **Duur van het venster Onderhoud** |30-180 |Het aantal minuten over toegangsrechten voor het downloaden en installeren van updates te voltooien. |
| **Patch categorie** |Belangrijk |De categorie van updates te downloaden en installeren. |

## <a name="configuration-with-powershell"></a>Configuratie met PowerShell
In het volgende voorbeeld wordt PowerShell gebruikt automatisch patchen op een bestaande SQL Server-VM configureren. De **nieuw AzureVMSqlServerAutoPatchingConfig** opdracht configureert u een nieuw onderhoudsvenster voor automatische updates.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Op basis van dit voorbeeld, beschrijft de volgende tabel de praktische gevolgen voor de doel-virtuele machine van Azure:

| Parameter | Effect |
| --- | --- |
| **DayOfWeek** |Patches elke donderdag geïnstalleerd. |
| **MaintenanceWindowStartingHour** |Begin updates om 11:00 uur. |
| **MaintenanceWindowsDuration** |Patches moeten worden geïnstalleerd binnen 120 minuten. Op basis van de begintijd, moeten ze volledig 13:00 uur. |
| **PatchCategory** |De instelling alleen mogelijk voor deze parameter is 'Belangrijk'. |

Dit kan enige tijd duren om te installeren en configureren van de SQL Server IaaS-Agent.

Als u wilt uitschakelen automatisch patchen, voert u de script zonder de - parameter inschakelen voor de nieuw-AzureVMSqlServerAutoPatchingConfig. Net als bij de installatie, kan het enkele minuten om uit te schakelen automatisch patchen duren.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over andere beschikbare automatiseringstaken [uitbreiding voor SQL Server IaaS-Agent](../classic/sql-server-agent-extension.md).

Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual machines [SQL Server op Azure Virtual Machines-overzicht](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

