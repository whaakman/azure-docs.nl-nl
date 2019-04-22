---
title: SQL Server op Azure VM-Release-opmerkingen | Microsoft Docs
description: Meer informatie over de nieuwe functies en verbeteringen van SQL Server op een Azure-VM
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
ms.openlocfilehash: 23e072369aa8ac6ca6ada5ec185df1a8d7e03c5b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59282539"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>SQL Server op de opmerkingen bij de release van de virtuele Machine van Azure

Met Azure kunt u een virtuele machine met een installatiekopie van SQL Server ingebouwde implementeren. In dit artikel bevat een overzicht van de nieuwe functies en verbeteringen in de recente versies van [SQL Server op Azure virtual machines](https://azure.microsoft.com/services/virtual-machines/sql-server/). Het artikel bevat ook belangrijke updates van inhoud die niet direct verband houden met de release maar gepubliceerd in dezelfde periode. Zie voor verbeteringen aan andere Azure-services, [Service-updates](https://azure.microsoft.com/updates)

## <a name="march-2019"></a>Maart 2019

### <a name="service-improvements"></a>Verbeteringen van de services

| Verbeteringen van de services | Details |
| --- | --- |
| **Ondersteuning voor aangepaste installatiekopie** | U kunt nu installeren de [SQL IaaS-extensie](virtual-machines-windows-sql-server-agent-extension.md#installation) op aangepaste installatiekopieën van besturingssysteem en SQL, die de beperkte functionaliteit van biedt [flexibele licentiëring](virtual-machines-windows-sql-ahb.md). Bij het registreren van uw aangepaste installatiekopie met de SQL-resourceprovider, het licentietype opgeven als 'AHUB' als anders mislukt de registratie.  | 
| **Benoemd exemplaar ondersteuning** | U kunt nu gebruikmaken van de [SQL IaaS-extensie](virtual-machines-windows-sql-server-agent-extension.md#installation) in een benoemd exemplaar als het standaardexemplaar correct is verwijderd. | 
| **Verbetering van de portal** | De Azure portal-ervaring voor het implementeren van een SQL Server-VM is vernieuwd om de bruikbaarheid verbeteren. Zie voor meer informatie, de korte [snelstartgids](quickstart-sql-vm-create-portal.md) en uitgebreider [procedures](virtual-machines-windows-portal-sql-server-provision.md) handleiding voor het implementeren van een SQL Server-VM. |
| &nbsp; | &nbsp; |

### <a name="documentation-improvements"></a>Verbeteringen in de documentatie bij

Geen


## <a name="february-2019"></a>Februari 2019

### <a name="service-improvements"></a>Verbeteringen van de services

| Verbeteringen van de services | Details |
| --- | --- |
| **Verbetering van de portal** | Het is nu mogelijk om te wijzigen van de licentiemodel voor een SQL Server-VM van betalen per gebruik voor het gebruik van bring-your-own-license de [Azure-portal](virtual-machines-windows-sql-ahb.md#with-the-azure-portal-1).|
|**Vereenvoudiging van AG-implementatie met Azure SQL-VM-CLI** | Het is nu eenvoudiger dan ooit implementeert een beschikbaarheidsgroep toevoegen aan een SQL Server-VM in Azure. [Azure SQL-VM-CLI](/cli/azure/sql/vm?view=azure-cli-2018-03-01-hybrid) kunt u de WSFC, ILB en AG-listener maken vanaf de opdrachtregel en in recordtijd wanneer! Zie voor meer informatie, [gebruik Azure SQL VM CLI voor AlwaysOn-beschikbaarheidsgroep configureren voor SQL Server op een Azure VM](virtual-machines-windows-sql-availability-group-cli.md). | 
| &nbsp; | &nbsp; |


## <a name="december-2018"></a>December 2018

| Verbeteringen van de services | Details |
| --- | --- |
| **Nieuwe SQL-cluster groep-resourceprovider** | Een nieuwe resourceprovider (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups) waarin de metagegevens van het Windows-failovercluster. Lid worden van een SQL Server-VM naar de *SqlVirtualMachineGroups* bootstrapt van de Windows Failover Cluster-service en maakt de virtuele machine lid van het cluster.  |
|**Instellen van de implementatie van een beschikbaarheid van groep met Azure-Snelstartsjablonen automatiseren** |Het is nu mogelijk te maken van het Windows-failovercluster, SQL Server-VM's toevoegen aan het maken van de listener en de interne Load Balancer configureren met twee Azure Quickstart-sjablonen. Zie voor meer informatie, [gebruik Azure-Snelstartsjabloon voor AlwaysOn-beschikbaarheidsgroep configureren voor SQL Server op een Azure VM](virtual-machines-windows-sql-availability-group-quickstart-template.md). | 
| **Registratie van de Resourceprovider automatische SQL-VM** | SQL Server-VM's geïmplementeerd nadat deze maand zijn automatisch geregistreerd bij de nieuwe resourceprovider voor SQL Server. SQL Server-VM's geïmplementeerd voordat u deze maand moeten nog steeds handmatig worden geregistreerd. Zie voor meer informatie, [bestaande SQL-VM met SQL-VM-resourceprovider registreren](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider).|
| &nbsp; | &nbsp; |


## <a name="november-2018"></a>November 2018

| Verbeteringen van de services | Details |
| --- | --- |
| **Nieuwe SQL-VM-resourceprovider** |  Een nieuwe resourceprovider voor SQL Server-VM's (Microsoft.SqlVirtualMachine) waarmee u beter beheer van uw SQL Server-VM. Zie voor meer informatie over het registreren van uw virtuele machine [bestaande SQL-VM met nieuwe resourceprovider registreren](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider). |
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
* [Een SQL Server Linux-Machine inrichten](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [Veelgestelde vragen over (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Documentatie voor SQL Server op Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
