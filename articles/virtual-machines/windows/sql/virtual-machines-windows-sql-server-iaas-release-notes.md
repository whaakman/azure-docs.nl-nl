---
title: Opmerkingen bij de release van Azure-VM SQL Server | Microsoft Docs
description: Meer informatie over de nieuwe functies en verbeteringen van SQL Server op een Azure VM
services: virtual-machines-windows
author: MashaMSFT
ms.author: mathoma
manager: craigg
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 2/13/2019
ms.openlocfilehash: e7dea69b507117ba8a3765b5e9bc1fad46be4c53
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444195"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>Opmerkingen bij de release van Azure virtual machine SQL Server

Met Azure kunt u een virtuele machine implementeren met een installatie kopie van SQL Server ingebouwde. In dit artikel vindt u een overzicht van de nieuwe functies en verbeteringen in de recente releases van [SQL Server op virtuele machines van Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/). In dit artikel vindt u ook een overzicht van belang rijke inhouds updates die niet rechtstreeks verband houden met de release, maar die in hetzelfde tijds bestek zijn gepubliceerd. Zie [service-updates](https://azure.microsoft.com/updates) voor verbeteringen aan andere Azure-Services


## <a name="june-2019"></a>Juni 2019

### <a name="service-improvements"></a>Service verbeteringen

| Service verbeteringen | Details |
| --- | --- |
| **Nieuwe SQL IaaS-installatie modi** | Het is nu mogelijk om de SQL IaaS-uitbrei ding in de [Lightweight-modus](virtual-machines-windows-sql-server-agent-extension.md) te installeren om te voor komen dat SQL Server-service opnieuw wordt gestart.  |
| **Aanpassing van SQL Server-editie** | U kunt nu de [eigenschap Edition](virtual-machines-windows-sql-change-edition.md) voor uw SQL Server virtuele machine wijzigen. |
| **RP-wijzigingen van SQL-VM** | U kunt [uw SQL Server-VM registreren bij de resource provider van de SQL-VM](virtual-machines-windows-sql-register-with-resource-provider.md#register-with-sql-vm-resource-provider) , zelfs [Windows 2008-installatie kopieën](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-server-2008r2-on-windows-server-2008-vms) , met behulp van de nieuwe SQL IaaS-modi. |
| **BYOL-installatie kopieën maken met behulp van AHUB** | BYOL-installatie kopieën die via Marketplace worden geïmplementeerd, kunnen nu hun [licentie type overschakelen op ' payg '](virtual-machines-windows-sql-ahb.md#remarks).| 
| &nbsp; | &nbsp; |


## <a name="may-2019"></a>Mei 2019

### <a name="service-improvements"></a>Service verbeteringen

| Service verbeteringen | Details |
| --- | --- |
| **Nieuw SQL-VM-beheer in Azure Portal** | Er is nu een nieuwe manier om uw SQL Server-VM in de Azure Portal te beheren. Zie [SQL Server VM beheren in de Azure Portal](virtual-machines-windows-sql-manage-portal.md)voor meer informatie.  |
| &nbsp; | &nbsp; |

### <a name="documentation-improvements"></a>Verbeterde documentatie

| Documentatie | Details |
| --- | --- |
| **Nieuw beheer van de SQL-VM-Portal** | Over een dozijn artikel zijn de nieuwe functies van de SQL VM-beheer Portal bijgewerkt. | 
| &nbsp; | &nbsp; |




## <a name="april-2019"></a>April 2019

### <a name="service-improvements"></a>Service verbeteringen

| Service verbeteringen | Details |
| --- | --- |
| **Ondersteuning voor SQL Server 2008-2008R2 uitbreiden** | [Ondersteuning](virtual-machines-windows-sql-server-2008-eos-extend-support.md) voor SQL Server 2008 en SQL Server 2008 R2 uitbreiden *door te migreren* naar een virtuele machine van Azure. | 
| &nbsp; | &nbsp; |


## <a name="march-2019"></a>2019 maart

| Service verbeteringen | Details |
| --- | --- |
| **Ondersteuning voor aangepaste installatie kopieën** | U kunt de [SQL IaaS-extensie](virtual-machines-windows-sql-server-agent-extension.md#installation) nu installeren op aangepaste besturings systemen en SQL-installatie kopieën, die de beperkte functionaliteit van [flexibele licenties](virtual-machines-windows-sql-ahb.md)bieden. Wanneer u uw aangepaste installatie kopie met de SQL-resource provider registreert, geeft u het licentie type op als ' AHUB ', anders mislukt de registratie. | 
| **Ondersteunings ondersteuning voor benoemde instanties** | U kunt nu de [SQL IaaS-extensie](virtual-machines-windows-sql-server-agent-extension.md#installation) gebruiken met een benoemd exemplaar als het standaard exemplaar op de juiste manier is verwijderd. | 
| **Portal verbetering** | De Azure Portal ervaring voor het implementeren van een SQL Server VM is vernieuwd om de bruikbaarheid te verbeteren. Meer informatie vindt u in de korte [Snelstartgids](quickstart-sql-vm-create-portal.md) en uitgebreidere [hand leiding](virtual-machines-windows-portal-sql-server-provision.md) voor het implementeren van een SQL Server-VM.|
| &nbsp; | &nbsp; |


## <a name="february-2019"></a>Februari 2019

| Service verbeteringen | Details |
| --- | --- |
| **Portal verbetering** | Het is nu mogelijk om het licentie model voor een SQL Server-VM te wijzigen van betalen per gebruik naar uw eigen licentie met behulp van de [Azure Portal](virtual-machines-windows-sql-ahb.md#change-license-for-vms-already-registered-with-resource-provider)|
|**Implementatie vereenvoudiging van de AG met Azure SQL VM CLI** | Het is nu eenvoudiger dan ooit om een beschikbaarheids groep te implementeren op een SQL Server VM in Azure. Met [Azure SQL VM cli](/cli/azure/sql/vm?view=azure-cli-2018-03-01-hybrid) kunt u de WSFC-, ILB-en AG-listener maken vanaf de opdracht regel en in de record tijd. Zie voor meer informatie Azure SQL VM CLI gebruiken voor het configureren van AlwaysOn- [beschikbaarheids groep voor SQL Server op een virtuele machine van Azure](virtual-machines-windows-sql-availability-group-cli.md). | 
| &nbsp; | &nbsp; |


## <a name="december-2018"></a>December 2018

| Service verbeteringen | Details |
| --- | --- |
| **Nieuwe resource provider voor SQL-cluster groep** | Een nieuwe resource provider (micro soft. SqlVirtualMachine/SqlVirtualMachineGroups) die de meta gegevens van het Windows-failovercluster definieert. Als u een SQL Server virtuele machine aan de *SqlVirtualMachineGroups* toevoegt, worden de Windows Failover-clusterservice en wordt de virtuele machine aan het cluster toegevoegd.  |
|**Het instellen van een implementatie van een beschikbaarheids groep automatiseren met Azure Quick Start-sjablonen** |Het is nu mogelijk om het Windows-failovercluster te maken, SQL Server Vm's aan toe te voegen, de listener te maken en de interne Load Balancer met twee Azure Quick Start-sjablonen te configureren. Zie voor meer informatie Azure Quick Start-sjabloon gebruiken voor het configureren van AlwaysOn- [beschikbaarheids groep voor SQL Server op een virtuele machine van Azure](virtual-machines-windows-sql-availability-group-quickstart-template.md). | 
| **Automatische registratie van SQL-VM-resource provider** | SQL Server Vm's die na deze maand zijn geïmplementeerd, worden automatisch geregistreerd bij de resource provider van het nieuwe SQL Server. SQL Server Vm's die vóór deze maand zijn geïmplementeerd, moeten nog steeds hand matig worden geregistreerd. Zie [bestaande SQL-VM registreren bij SQL-VM-resource provider](virtual-machines-windows-sql-register-with-resource-provider.md)voor meer informatie.|
| &nbsp; | &nbsp; |


## <a name="november-2018"></a>November 2018

| Service verbeteringen | Details |
| --- | --- |
| **Nieuwe SQL-VM-resource provider** |  Een nieuwe resource provider voor SQL Server Vm's (micro soft. SqlVirtualMachine) waarmee u uw SQL Server virtuele machine beter kunt beheren. Zie voor meer informatie over het registreren van uw virtuele machine [bestaande SQL-VM registreren bij nieuwe resource provider](virtual-machines-windows-sql-register-with-resource-provider.md). |
|**Licentie model scha kelen** | U kunt nu scha kelen tussen het pay-per-gebruik en uw eigen licentie model voor uw virtuele SQL-machine met behulp van Azure CLI of Power shell. Zie [Het licentiemodel voor een SQL-VM wijzigen](virtual-machines-windows-sql-ahb.md) voor meer informatie. | 
| &nbsp; | &nbsp; |


## <a name="additional-resources"></a>Aanvullende resources

**Virtuele Windows-machines**:

* [Overzicht van SQL Server op een Windows-VM](virtual-machines-windows-sql-server-iaas-overview.md).
* [Een SQL Server Windows-VM inrichten](virtual-machines-windows-portal-sql-server-provision.md)
* [Een Data Base migreren naar SQL Server op een virtuele Azure-machine](virtual-machines-windows-migrate-sql.md)
* [Hoge Beschik baarheid en herstel na nood gevallen voor SQL Server in azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md)
* [Aanbevolen procedures voor prestaties voor SQL Server op virtuele machines van Azure](virtual-machines-windows-sql-performance.md)
* [Toepassings patronen en ontwikkelings strategieën voor SQL Server in azure Virtual Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Virtuele Linux-machines**:

* [Overzicht van SQL Server op een Linux-VM](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Een virtuele machine met SQL Server Linux inrichten](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [Veelgestelde vragen (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Documentatie voor SQL Server op Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
