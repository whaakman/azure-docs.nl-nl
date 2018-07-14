---
title: Automatiseer beheertaken op SQL-VM's (klassiek) | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u voor het beheren van de SQL Server agent-extensie, welke specifieke SQL Server-beheertaken worden geautomatiseerd. Het gaat hierbij om automatische back-up, automatisch patchen en integratie van Azure Sleutelkluis. In dit onderwerp wordt gebruikgemaakt van de klassieke implementatiemodus.
services: virtual-machines-windows
documentationcenter: ''
author: rothja
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a9bda2e7-cdba-427c-bc30-77cde4376f3a
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/12/2018
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 55a31d7a0ab603dd7fe7de514d11d003e044240a
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008328"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-classic"></a>Automatiseer beheertaken op Azure Virtual Machines met de SQL Server Agent-extensie (klassiek)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md)
> * [Klassiek](../classic/sql-server-agent-extension.md)
> 
>
 
De SQL Server IaaS Agent-extensie (SQLIaaSAgent) wordt uitgevoerd op Azure virtual machines voor het automatiseren van beheertaken. Dit onderwerp bevat een overzicht van de services die worden ondersteund door de extensie, evenals de instructies voor installatie, status en verwijdering.

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Voor de Resource Manager-versie van dit artikel, raadpleegt u [SQL Server Agent-extensie voor SQL Server VM's Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md).

## <a name="supported-services"></a>Ondersteunde services
De SQL Server IaaS Agent-extensie ondersteunt de volgende beheertaken:

| Beheer-functie | Beschrijving |
| --- | --- |
| **Automatische back-up van SQL** |Automatiseert de planning van de back-ups voor alle databases voor het standaardexemplaar van SQL Server in de virtuele machine. Zie voor meer informatie, [automatische back-up voor SQL Server in Azure Virtual Machines (klassiek)](../classic/sql-automated-backup.md). |
| **Automatisch patchen van SQL** |Hiermee configureert u een onderhoudsvenster waarin belangrijke updates voor Windows met uw virtuele machine plaatsvinden kunnen, zodat u dat updates tijdens piektijden voor uw workload voorkomen kunt. Zie voor meer informatie, [automatische patches voor SQL Server in Azure Virtual Machines (klassiek)](../classic/sql-automated-patching.md). |
| **Integratie van Azure Key Vault** |Hiermee kunt u automatisch installeren en configureren van Azure Key Vault op uw SQL Server-VM. Zie voor meer informatie, [configureren van Azure Key Vault-integratie voor SQL Server op Azure Virtual machines (klassiek)](../classic/ps-sql-keyvault.md). |

## <a name="prerequisites"></a>Vereisten
Vereisten voor het gebruik van de SQL Server IaaS Agent-extensie op de virtuele machine:

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

Als u meerdere abonnementen hebt, gebruikt u **Select-AzureSubscription** om te selecteren van het abonnement met het doel-klassieke VM.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Op dit moment krijgt u een lijst van de klassieke virtuele machines en hun namen van de gekoppelde service met de **Get-AzureVM** opdracht.

    Get-AzureVM

## <a name="installation"></a>Installatie
Voor klassieke virtuele machines, moet u PowerShell gebruiken voor de SQL Server IaaS Agent-extensie installeren en configureren van de gekoppelde services. Gebruik de **Set AzureVMSqlServerExtension** PowerShell-cmdlet voor het installeren van de extensie. Bijvoorbeeld, de volgende opdracht wordt de extensie wordt geïnstalleerd op een Windows Server VM (klassiek) en naam 'SQLIaaSExtension'.

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Als u naar de nieuwste versie van de SQL IaaS Agent-extensie bijwerkt, moet u uw virtuele machine opnieuw opstarten na het bijwerken van de extensie.

> [!NOTE]
> Klassieke virtuele machines beschikt niet over een optie voor het installeren en configureren van de SQL Agent-extensie voor IaaS via de portal.

> [!NOTE]
> De SQL Server IaaS Agent-extensie wordt alleen ondersteund op [installatiekopieën van SQL Server-VM-galerie](../sql/virtual-machines-windows-sql-server-iaas-overview.md#get-started-with-sql-vms) (betalen per gebruik of bring-your-own-license). Het wordt niet ondersteund als u SQL Server voor het handmatig op een alleen-besturingssysteem Windows Server-machine installeren of als u een aangepaste SQL Server-VM VHD implementeren. In dergelijke gevallen is het mogelijk om te installeren en beheren van de extensie handmatig met behulp van PowerShell, maar het wordt sterk aanbevolen om te installeren in plaats daarvan een installatiekopie van SQL Server-VM-galerie en vervolgens aan te passen.

## <a name="status"></a>Status
Er is één manier om te controleren of de extensie is geïnstalleerd om de agentstatus in Azure Portal weer te geven. Selecteer een virtuele machine die worden vermeld in de blade van de virtuele machine en klik vervolgens op **extensies**. U ziet de **SQLIaaSAgent** extensie is die weergegeven.

![SQL Server IaaS Agent-extensie in Azure Portal](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

U kunt ook de **Get-AzureVMSqlServerExtension** Azure Powershell-cmdlet.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Verwijdering
In de Azure-Portal, kunt u de extensie verwijderen door te klikken op het beletselteken op de **extensies** blade van de eigenschappen van uw virtuele machine. Klik vervolgens op **verwijderen**.

![Verwijderen van de SQL Server IaaS Agent-extensie in Azure Portal](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

U kunt ook de **Remove-AzureVMSqlServerExtension** Powershell-cmdlet.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Volgende stappen
Beginnen met een van de services die worden ondersteund door de extensie. Voor meer informatie, Zie de onderwerpen waarnaar wordt verwezen in de [ondersteunde services](#supported-services) sectie van dit artikel.

Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual Machines [SQL Server op Azure Virtual Machines overzicht](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

