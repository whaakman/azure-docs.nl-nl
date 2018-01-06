---
title: Automatiseren beheertaken op SQL-machines (Resource Manager) | Microsoft Docs
description: Dit artikel wordt beschreven hoe u voor het beheren van de SQL Server agent-extensie, die specifieke SQL Server-beheertaken worden geautomatiseerd. Het gaat hierbij om automatische back-up automatisch patchen en integratie van Azure Sleutelkluis.
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/05/2018
ms.author: jroth
ms.openlocfilehash: 1d2b681660ae6f59dec8a287baa853085c64ebeb
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-resource-manager"></a>Automatiseren beheertaken op Azure Virtual Machines met de SQL Server Agent-extensie (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klassiek](../classic/sql-server-agent-extension.md)
> 
> 

De extensie in SQL Server IaaS-Agent (SQLIaaSExtension) wordt uitgevoerd op Azure virtuele machines voor het automatiseren van beheertaken. In dit artikel biedt een overzicht van de services wordt ondersteund door de extensie, evenals de instructies voor installatie, status en verwijdering.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

De klassieke versie van dit artikel vindt u [SQL Server Agent-extensie voor SQL Server-machines klassieke](../classic/sql-server-agent-extension.md).

## <a name="supported-services"></a>Ondersteunde services
De uitbreiding met SQL Server IaaS-Agent ondersteunt de volgende beheertaken:

| Beheer-functie | Beschrijving |
| --- | --- |
| **Automatische back-up van SQL** |Automatiseert de planning van de back-ups voor alle databases voor het standaardexemplaar van SQL Server in de virtuele machine. Zie voor meer informatie [automatische back-up voor SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md). |
| **Automatisch patchen van SQL** |Hiermee configureert u een onderhoudsvenster waarbinnen updates voor uw virtuele machine kunnen worden uitgevoerd, zodat u voorkomen de updates tijdens piektijden voor uw workload dat kunt. Zie voor meer informatie [automatisch patchen voor SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md). |
| **Integratie van Azure Key Vault** |Hiermee kunt u automatisch installeren en configureren van Azure Sleutelkluis op uw SQL Server-VM. Zie voor meer informatie [configureren Azure Sleutelkluis-integratie voor SQL Server op Azure Virtual machines (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md). |

Eenmaal geïnstalleerd en uitgevoerd, maakt de uitbreiding met SQL Server IaaS-Agent deze beheerfuncties beschikbaar in het deelvenster SQL Server van de virtuele machine in de Azure-portal en via Azure PowerShell voor SQL Server marketplace-installatiekopieën en via Azure PowerShell voor handmatige installaties van de extensie. 

## <a name="prerequisites"></a>Vereisten
Vereisten voor het gebruik van de uitbreiding met SQL Server IaaS-Agent op de virtuele machine:

**Besturingssysteem**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server-versies**:

* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

**Azure PowerShell**:

* [Downloaden en configureren van de meest recente Azure PowerShell-opdrachten](/powershell/azure/overview)

## <a name="installation"></a>Installatie
De uitbreiding met SQL Server IaaS-Agent wordt automatisch geïnstalleerd wanneer u een van de SQL Server-installatiekopieën voor virtuele machine galerie inricht. Als u moet de uitbreiding handmatig installeren op een van deze VM's van SQL Server, gebruikt u de volgende PowerShell-opdracht:

```powershell
Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2" -Location "East US 2"
```

> [!IMPORTANT]
> Als de extensie niet al is geïnstalleerd, opnieuw installeren van de extensie de SQL Server-service.

Het is ook mogelijk de uitbreiding met SQL Server IaaS-Agent installeren op een virtuele machine van de OS-alleen Windows-Server. Dit wordt alleen ondersteund als u SQL Server ook handmatig hebt geïnstalleerd op deze machine. Installeer de uitbreiding handmatig met behulp van dezelfde **Set AzureVMSqlServerExtension** PowerShell-cmdlet.

> [!NOTE]
> Als u de uitbreiding met SQL Server IaaS-Agent handmatig op een alleen-besturingssysteem Windows Server-VM installeren, kunt u de SQL Server-configuratie-instellingen via de Azure portal niet beheren. In dit scenario moet u alle wijzigingen met PowerShell.

## <a name="status"></a>Status
Er is een manier om te controleren of de uitbreiding is geïnstalleerd om weer te geven van de agent-status in de Azure portal. Selecteer **alle instellingen** in het venster van de virtuele machine, en klik vervolgens op **extensies**. U ziet de **SQLIaaSExtension** extensie vermeld.

![SQL Server IaaS-agentextensie in de Azure-portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

U kunt ook de **Get-AzureVMSqlServerExtension** Azure PowerShell-cmdlet.

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

De vorige opdracht bevestigt de agent is geïnstalleerd en vindt u algemene statusinformatie. U kunt ook specifieke statusinformatie ophalen over automatische back-up en Patching met de volgende opdrachten.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Verwijdering
In de Azure-Portal kunt u de extensie verwijderen door te klikken op het weglatingsteken op de **extensies** venster van de eigenschappen van de virtuele machine. Klik vervolgens op **verwijderen**.

![De uitbreiding van SQL Server IaaS-Agent in Azure-portal verwijderen](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

U kunt ook de **verwijderen AzureRmVMSqlServerExtension** PowerShell-cmdlet.

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Volgende stappen
Beginnen met een van de services wordt ondersteund door de uitbreiding. Zie voor meer informatie de artikelen waarnaar wordt verwezen in de [ondersteunde services](#supported-services) sectie van dit artikel.

Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual Machines [SQL Server op Azure Virtual Machines-overzicht](virtual-machines-windows-sql-server-iaas-overview.md).

