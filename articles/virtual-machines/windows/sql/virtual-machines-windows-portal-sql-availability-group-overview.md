---
title: Beschikbaarheid van SQL Server - virtuele Machines van Azure - overzicht groepen | Microsoft Docs
description: Dit artikel bevat beschikbaarheidsgroepen van SQL Server op Azure virtual machines.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/13/2017
ms.author: mikeray
ms.openlocfilehash: e11e8c6dbad049b7efea373944155748b9703d57
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57781133"
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a>Maak kennis met SQL Server Always On-beschikbaarheidsgroepen op Azure virtual machines #

Dit artikel bevat beschikbaarheidsgroepen van SQL Server op Azure Virtual Machines. 

AlwaysOn-beschikbaarheidsgroepen op Azure Virtual Machines zijn vergelijkbaar met AlwaysOn-beschikbaarheidsgroepen on-premises. Zie voor meer informatie, [Always On Availability Groups (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx). 

Het diagram illustreert de onderdelen van een volledige SQL Server-beschikbaarheidsgroep in Azure Virtual Machines.

![Beschikbaarheidsgroep](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

Het belangrijkste verschil voor een beschikbaarheidsgroep in Azure Virtual Machines is dat de virtuele Azure-machines, vereist een [netwerktaakverdeler](../../../load-balancer/load-balancer-overview.md). De load balancer bevat de IP-adressen voor de beschikbaarheidsgroep-listener. Als u meer dan één beschikbaarheidsgroep hebt moet elke groep een listener. Een load balancer biedt ondersteuning voor meerdere listeners.

Bovendien op een failovercluster voor Azure IaaS VM-Gast wordt aangeraden één NIC per server (clusterknooppunt) en één subnet. Azure-netwerken heeft fysieke redundantie extra NIC's en subnetten onnodige maakt op een Azure IaaS VM-gastcluster. Hoewel het clustervalidatierapport wordt een waarschuwing dat de knooppunten alleen bereikbaar is op één netwerk zijn uitgeeft, kan deze waarschuwing veilig worden genegeerd op Azure IaaS VM-gastclusters voor failover. 

|  | Windows Server-versie | SQL Server-versie | SQL Server-editie | WSFC Quorum Config | Herstel na Noodgevallen met meerdere regio 's | Ondersteuning voor meerdere subnetten | Ondersteuning voor een bestaande AD-Tenant | Herstel na Noodgevallen met meerdere zones dezelfde regio | Ondersteuning voor dist AG er is geen AD-domein | Dist AG-ondersteuning met niet-cluster |  
| :------ | :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----|
| [SQL VM-CLI](virtual-machines-windows-sql-availability-group-cli.md) | 2016 | 2017 </br>2016   | Ent | Cloud-witness | Nee | Ja | Ja | Ja | Nee | Nee |
| [QuickStart-sjablonen](virtual-machines-windows-sql-availability-group-quickstart-template.md) | 2016 | 2017</br>2016  | Ent | Cloud-witness | Nee | Ja | Ja | Ja | Nee | Nee |
| [Sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) | 2016 </br>2012 R2 | 2016</br>2014 | Ent | Bestandsshare | Nee | Nee | Nee | Nee | Nee | Nee |
| [Handmatig](virtual-machines-windows-portal-sql-availability-group-prereq.md) | Alle | Alle | Alle | Alle | Ja | Ja | Ja | Ja | Ja | Ja |
| &nbsp; | &nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |

Wanneer u gereed voor het bouwen van een beschikbaarheidsgroep van SQL Server op Azure Virtual Machines bent, raadpleegt u deze zelfstudies.

## <a name="manually-with-azure-cli"></a>Handmatig met Azure CLI
Met behulp van Azure CLI voor het configureren en implementeren van een beschikbaarheidsgroep is de aanbevolen optie, omdat deze het beste in termen van eenvoud en snelheid van de implementatie. Met Azure CLI, het maken van het Windows-failovercluster, SQL Server-VM's toevoegen aan het cluster, evenals het maken van de listener en een interne Load Balancer kan alle worden bereikt in minder dan 30 minuten. Deze optie nog steeds een handmatig maken van de beschikbaarheidsgroep vereist, maar alle andere vereiste configuratiestappen worden geautomatiseerd. 

Zie voor meer informatie, [gebruik Azure SQL VM CLI voor AlwaysOn-beschikbaarheidsgroep configureren voor SQL Server op een Azure VM](virtual-machines-windows-sql-availability-group-cli.md). 

## <a name="automatically-with-azure-quickstart-templates"></a>Automatisch met Azure-Quickstart-sjablonen
De Azure Quickstart-sjablonen maken gebruik van de SQL-VM-resourceprovider voor het implementeren van het Windows-failovercluster, SQL Server-VM's toevoegen aan het maken van de listener en de interne Load Balancer configureren. Deze optie nog steeds vereist een handmatige maken van de beschikbaarheidsgroep en de interne Load Balancer (ILB), maar automatiseert en alle andere vereiste configuratiestappen (met inbegrip van de configuratie van de ILB) vereenvoudigt. 

Zie voor meer informatie, [gebruik Azure-Snelstartsjabloon voor AlwaysOn-beschikbaarheidsgroep configureren voor SQL Server op een Azure VM](virtual-machines-windows-sql-availability-group-quickstart-template.md).


## <a name="automatically-with-an-azure-portal-template"></a>Automatisch met een Azure-Portal sjabloon

[AlwaysOn-beschikbaarheidsgroep configureren voor de Resource Manager in Azure VM automatisch-](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)


## <a name="manually-in-azure-portal"></a>Handmatig in Azure portal

U kunt ook de virtuele machines zelf maken zonder de sjabloon. Eerst, voldoen aan de vereisten en maakt u de beschikbaarheidsgroep. Zie de volgende onderwerpen: 

- [Vereisten voor SQL Server Always On-beschikbaarheidsgroepen configureren op Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [Maken van AlwaysOn-beschikbaarheidsgroep voor het verbeteren van de beschikbaarheid en herstel na noodgevallen](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a>Volgende stappen

[Een SQL Server Always On Availability Group op Azure Virtual Machines in verschillende regio's configureren](virtual-machines-windows-portal-sql-availability-group-dr.md)
