---
title: Automatiseren beheertaken op SQL-machines (klassiek) | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe voor het beheren van de SQL Server agent-extensie, die specifieke SQL Server-beheertaken worden geautomatiseerd. Het gaat hierbij om automatische back-up automatisch patchen en integratie van Azure Sleutelkluis. In dit onderwerp maakt gebruik van de klassieke implementatiemodus.
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a9bda2e7-cdba-427c-bc30-77cde4376f3a
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 30fa9128cd51a7498449c991b58500ad9acdd3d4
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-classic"></a>Automatiseren beheertaken op Azure Virtual Machines met de SQL Server Agent-extensie (klassiek)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md)
> * [Klassiek](../classic/sql-server-agent-extension.md)
> 
>
 
De extensie in SQL Server IaaS-Agent (SQLIaaSAgent) wordt uitgevoerd op Azure virtuele machines voor het automatiseren van beheertaken. In dit onderwerp biedt een overzicht van de services wordt ondersteund door de extensie, evenals de instructies voor installatie, status en verwijdering.

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. De Resource Manager-versie van dit artikel vindt u [SQL Server Agent-extensie voor SQL Server VM's Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md).

## <a name="supported-services"></a>Ondersteunde services
De uitbreiding met SQL Server IaaS-Agent ondersteunt de volgende beheertaken:

| Beheer-functie | Beschrijving |
| --- | --- |
| **Automatische back-up van SQL** |Automatiseert de planning van de back-ups voor alle databases voor het standaardexemplaar van SQL Server in de virtuele machine. Zie voor meer informatie [automatische back-up voor SQL Server in Azure Virtual Machines (klassiek)](../classic/sql-automated-backup.md). |
| **Automatisch patchen van SQL** |Hiermee configureert u een onderhoudsvenster waarbinnen updates voor uw virtuele machine kunnen worden uitgevoerd, zodat u voorkomen de updates tijdens piektijden voor uw workload dat kunt. Zie voor meer informatie [automatisch patchen voor SQL Server in Azure Virtual Machines (klassiek)](../classic/sql-automated-patching.md). |
| **Integratie van Azure Key Vault** |Hiermee kunt u automatisch installeren en configureren van Azure Sleutelkluis op uw SQL Server-VM. Zie voor meer informatie [configureren Azure Sleutelkluis-integratie voor SQL Server op Azure Virtual machines (klassiek)](../classic/ps-sql-keyvault.md). |

## <a name="prerequisites"></a>Vereisten
Vereisten voor het gebruik van de uitbreiding met SQL Server IaaS-Agent op de virtuele machine:

### <a name="operating-system"></a>Besturingssysteem:
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

### <a name="sql-server-versions"></a>SQL Server-versies:
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

### <a name="azure-powershell"></a>Azure PowerShell:
[Downloaden en configureren van de meest recente Azure PowerShell-opdrachten](/powershell/azure/overview).

Start Windows PowerShell en verbinden met uw Azure-abonnement met de **Add-AzureAccount** opdracht.

    Add-AzureAccount

Als u meerdere abonnementen hebt, gebruikt u **Select-AzureSubscription** selecteren van het abonnement waarin het doel-klassieke VM.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Op dit moment krijgt u een lijst met de klassieke virtuele machines en hun namen van de gekoppelde service met de **Get-AzureVM** opdracht.

    Get-AzureVM

## <a name="installation"></a>Installatie
Voor klassieke virtuele machines, moet u PowerShell gebruiken voor de uitbreiding voor de SQL Server IaaS-Agent installeren en configureren van de gekoppelde services. Gebruik de **Set AzureVMSqlServerExtension** PowerShell-cmdlet voor het installeren van de extensie. De volgende opdracht wordt bijvoorbeeld de uitbreiding wordt geïnstalleerd op een Windows Server-VM (klassiek) en naam 'SQLIaaSExtension'.

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Als u naar de nieuwste versie van de SQL-uitbreiding voor IaaS-Agent bijwerkt, moet u uw virtuele machine opnieuw opstarten na het bijwerken van de extensie.

> [!NOTE]
> Klassieke virtuele machines beschikt niet over een optie voor het installeren en configureren van de SQL Agent-uitbreiding voor IaaS via de portal.
> 
> 

## <a name="status"></a>Status
Er is een manier om te controleren of de uitbreiding is geïnstalleerd om weer te geven van de agent-status in de Azure Portal. Selecteer een virtuele machine die worden vermeld in de blade van de virtuele machine en klik vervolgens op **extensies**. U ziet de **SQLIaaSAgent** extensie vermeld.

![SQL Server IaaS-Agent-extensie in de Azure-Portal](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

U kunt ook de **Get-AzureVMSqlServerExtension** Azure Powershell-cmdlet.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Verwijdering
In de Azure-Portal kunt u de extensie verwijderen door te klikken op het weglatingsteken op de **extensies** blade van de eigenschappen van de virtuele machine. Klik vervolgens op **verwijderen**.

![De SQL Server Agent IaaS-uitbreiding in Azure-Portal verwijderen](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

U kunt ook de **verwijderen AzureVMSqlServerExtension** Powershell-cmdlet.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Volgende stappen
Beginnen met een van de services wordt ondersteund door de uitbreiding. Voor meer informatie, Zie de onderwerpen waarnaar wordt verwezen in de [ondersteunde services](#supported-services) sectie van dit artikel.

Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual Machines [SQL Server op Azure Virtual Machines-overzicht](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

