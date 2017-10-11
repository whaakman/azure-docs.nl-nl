---
title: Automatisch patchen voor SQL Server-VM's (Resource Manager) | Microsoft Docs
description: Verklaart de functie automatisch patchen voor SQL Server Virtual Machines worden uitgevoerd in Azure Resource Manager gebruiken.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 58232e92-318f-456b-8f0a-2201a541e08d
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 7d501ab45a85010a8dbfd6135d77f18f1743354e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automated Patching voor SQL Server in virtuele machines van Azure (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-automated-patching.md)
> * [Klassiek](../classic/sql-automated-patching.md)
> 
> 

Geautomatiseerde Patching stelt u een onderhoudsvenster voor een virtuele Machine van Azure met SQL Server. Automatische Updates kunnen alleen worden geïnstalleerd tijdens deze periode. Voor SQL Server zorgt deze rescriction systeemupdates en eventuele bijbehorende opnieuw is opgestart bij de best mogelijke voor de database optreden. Geautomatiseerde Patching is afhankelijk van de [uitbreiding voor SQL Server IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

De klassieke versie van dit artikel vindt u [automatisch patchen voor SQL Server in Azure virtuele Machines klassieke](../classic/sql-automated-patching.md).

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

* [Installeer de nieuwste Azure PowerShell-opdrachten](/powershell/azure/overview) als u van plan bent automatisch patchen configureren met PowerShell.

> [!NOTE]
> Er is een geautomatiseerde Patching afhankelijk van de uitbreiding met SQL Server IaaS-Agent. Huidige SQL-virtuele machine afbeeldingen deze extensie standaard toegevoegd. Zie voor meer informatie [uitbreiding voor SQL Server IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md).
> 
> 

## <a name="settings"></a>Instellingen
De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor automatisch patchen. De werkelijke configuratiestappen variëren afhankelijk van of u de Azure portal of Azure Windows PowerShell-opdrachten gebruiken.

| Instelling | Mogelijke waarden | Beschrijving |
| --- | --- | --- |
| **Automatisch patch toepassen** |In-of uitschakelen (uitgeschakeld) |Hiermee schakelt automatisch patchen voor een virtuele machine van Azure of. |
| **Onderhoudsplanning** |Dagelijks, maandag, dinsdag, woensdag, donderdag, vrijdag, zaterdag, zondag |De planning voor het downloaden en installeren van Windows, SQL Server en de Microsoft-updates voor uw virtuele machine. |
| **Beginuur van het onderhoud** |0-24 |De lokale tijd voor het bijwerken van de virtuele machine start. |
| **Duur van het venster Onderhoud** |30-180 |Het aantal minuten over toegangsrechten voor het downloaden en installeren van updates te voltooien. |
| **Patch categorie** |Belangrijk |De categorie van updates te downloaden en installeren. |

## <a name="configuration-in-the-portal"></a>De configuratie in de Portal
U kunt de Azure-portal gebruiken voor het configureren van automatisch patchen tijdens het inrichten of voor een bestaande virtuele machines.

### <a name="new-vms"></a>Nieuwe virtuele machines
De Azure portal gebruiken voor het automatisch patchen te configureren wanneer u een nieuwe virtuele Machine van SQL Server in het Resource Manager-implementatiemodel maakt.

In de **SQL Server-instellingen** blade Selecteer **automatisch patchen**. De volgende schermopname van Azure portal bevat de **SQL automatisch patchen** blade.

![SQL automatisch patchen in Azure-portal](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Zie voor context is het volledig onderwerp op [inrichten van een virtuele machine van SQL Server in Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Bestaande virtuele machines
Selecteer de virtuele machine van SQL Server voor een bestaande SQL Server-virtuele machines. Selecteer vervolgens de **SQL Server-configuratiebestand** sectie van de **instellingen** blade.

![Automatisch patchen van SQL voor bestaande virtuele machines](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

In de **SQL Server-configuratiebestand** blade, klikt u op de **bewerken** knop in de sectie patchen automatisch.

![Configureer SQL automatisch patchen voor bestaande virtuele machines](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

Wanneer u klaar bent, klikt u op de **OK** knop aan de onderkant van de **SQL Server-configuratiebestand** blade uw wijzigingen op te slaan.

Als u automatisch patchen voor het eerst inschakelen wilt, configureert Azure de SQL Server IaaS-Agent op de achtergrond. Gedurende deze tijd de Azure-portal mogelijk niet weergegeven dat automatisch patchen is geconfigureerd. Wacht enkele minuten voor de agent moet worden geïnstalleerd of geconfigureerd. Daarna de Azure-portal duidt op de nieuwe instellingen.

> [!NOTE]
> U kunt ook configureren met behulp van een sjabloon automatisch patchen. Zie voor meer informatie [Azure quickstart-sjabloon voor het automatisch patchen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).
> 
> 

## <a name="configuration-with-powershell"></a>Configuratie met PowerShell
Na het inrichten van uw SQL VM PowerShell te gebruiken voor het configureren van automatisch patchen.

In het volgende voorbeeld wordt PowerShell gebruikt automatisch patchen op een bestaande SQL Server-VM configureren. De **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig** opdracht configureert u een nieuw onderhoudsvenster voor automatische updates.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

Op basis van dit voorbeeld, beschrijft de volgende tabel de praktische gevolgen voor de doel-virtuele machine van Azure:

| Parameter | Effect |
| --- | --- |
| **DayOfWeek** |Patches elke donderdag geïnstalleerd. |
| **MaintenanceWindowStartingHour** |Begin updates om 11:00 uur. |
| **MaintenanceWindowsDuration** |Patches moeten worden geïnstalleerd binnen 120 minuten. Op basis van de begintijd, moeten ze volledig 13:00 uur. |
| **PatchCategory** |De enige mogelijke-instelling voor deze parameter is **belangrijk**. |

Dit kan enige tijd duren om te installeren en configureren van de SQL Server IaaS-Agent.

Schakel automatisch patchen uitvoeren hetzelfde script zonder de **-inschakelen** -parameter voor de **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig**. Het ontbreken van de **-inschakelen** parameter geeft u de opdracht uit om de functie uitschakelen.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over andere beschikbare automatiseringstaken [uitbreiding voor SQL Server IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md).

Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual machines [SQL Server op Azure Virtual Machines-overzicht](virtual-machines-windows-sql-server-iaas-overview.md).

