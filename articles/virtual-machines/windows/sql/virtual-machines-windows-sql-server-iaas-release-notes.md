---
title: SQL Server op Azure VM-Release-opmerkingen | Microsoft Docs
description: Meer informatie over de nieuwe functies en verbeteringen van SQL Server op een Azure-VM
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/13/2018
ms.author: mathoma
ms.openlocfilehash: 7c854759e9a0ba13b698dd07dabb25100895bc85
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55766874"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>SQL Server op de opmerkingen bij de release van de virtuele Machine van Azure

Met Azure kunt u een virtuele machine met een installatiekopie van SQL Server ingebouwde implementeren. In dit artikel bevat de nieuwe functies en verbeteringen die u in de meest recente versie van SQL Server is geïmplementeerd op een virtuele machine van Azure verwachten kunt. 

## <a name="december-2018"></a>December 2018

| **Change** | Details |
| --- | --- |
| **Nieuwe SQL-cluster groep-resourceprovider** | Er is een nieuwe resourceprovider (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups) die de metagegevens van de Windows-failovercluster definieert. Lid worden van een SQL Server-VM naar de *SqlVirtualMachineGroups* bootstrapt van de Windows Failover Cluster-service en maakt de virtuele machine lid van het cluster.  |
|**Instellen van de implementatie van een beschikbaarheid van groep met Azure-Snelstartsjablonen automatiseren** |Het is nu mogelijk te maken van het Windows-failovercluster, SQL Server-VM's toevoegen aan het maken van de listener en de interne Load Balancer configureren met twee Azure Quickstart-sjablonen. Zie voor meer informatie, [WSFC maken, listener en ILB configureren voor de groep van een Always On-beschikbaarheid op een SQL Server-VM met de Azure Quickstart-sjabloon](virtual-machines-windows-sql-availability-group-quickstart-template.md). | 
| **Registratie van de Resourceprovider automatische SQL-VM** | SQL Server-VM's geïmplementeerd nadat deze maand zijn automatisch geregistreerd bij de nieuwe resourceprovider voor SQL Server. SQL Server-VM's geïmplementeerd voordat u deze maand wordt nog steeds moeten handmatig worden geregistreerd. Zie voor meer informatie, [bestaande SQL-VM met nieuwe resourceprovider registreren](virtual-machines-windows-sql-ahb.md#register-existing-sql-server-vm-with-sql-resource-provider).|
| &nbsp; | &nbsp; |


## <a name="november-2018"></a>November 2018

| **Change** | Details |
| --- | --- |
| **Nieuwe SQL-VM-resourceprovider** |  Er is een nieuwe resourceprovider voor SQL Server-VM's (Microsoft.SqlVirtualMachine) waarmee beter beheer van uw SQL Server-VM. Zie voor meer informatie over het registreren van uw virtuele machine [bestaande SQL-VM met nieuwe resourceprovider registreren](virtual-machines-windows-sql-ahb.md#register-existing-sql-server-vm-with-sql-resource-provider). |
|**Switch-licentiemodel** |U kunt nu schakelen tussen het licentiemodel met betalen per gebruik en bring-your-own-van uw SQL-VM met Azure CLI of Powershell. Zie [Het licentiemodel voor een SQL-VM wijzigen](virtual-machines-windows-sql-ahb.md) voor meer informatie. | 
| &nbsp; | &nbsp; |


## <a name="additional-resources"></a>Aanvullende resources

**Virtuele Windows-machines**:

* [Overzicht van SQL Server op een Windows VM](virtual-machines-windows-sql-server-iaas-overview.md).
* [Een SQL Server Windows VM inrichten](virtual-machines-windows-portal-sql-server-provision.md)
* [Een Database migreren naar SQL Server op een Azure-VM](virtual-machines-windows-migrate-sql.md)
* [Hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md)
* [Aanbevolen procedures voor prestaties voor SQL Server op virtuele machines van Azure](virtual-machines-windows-sql-performance.md)
* [Toepassingspatronen en ontwikkelingsstrategieën voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Virtuele Linux-machines**:

* [Overzicht van SQL Server op een virtuele Linux-machine](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Een SQL Server Linux-VM inrichten](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [Veelgestelde vragen over (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Documentatie voor SQL Server op Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
