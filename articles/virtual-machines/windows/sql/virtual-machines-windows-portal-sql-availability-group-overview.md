---
title: Beschikbaarheid van SQL Server - virtuele Azure-Machines - overzicht groepen | Microsoft Docs
description: Dit artikel bevat een SQL Server-beschikbaarheidsgroepen op virtuele machines in Azure.
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
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
ms.openlocfilehash: 2cbb9ff3b2d13996b1b8dc64aa833807c264c877
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a>Inleiding tot SQL Server altijd op beschikbaarheidsgroepen op virtuele machines in Azure #

Dit artikel bevat een SQL Server-beschikbaarheidsgroepen op Azure Virtual Machines. 

AlwaysOn-beschikbaarheidsgroepen op Azure Virtual Machines zijn vergelijkbaar met on-premises altijd op beschikbaarheidsgroepen. Zie voor meer informatie [altijd op beschikbaarheidsgroepen (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx). 

Het diagram illustreert de onderdelen van een volledige SQL Server-beschikbaarheidsgroep in Azure Virtual Machines.

![Beschikbaarheidsgroep](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

Het belangrijkste verschil voor een beschikbaarheidsgroep in Azure Virtual Machines is dat de virtuele machines van Azure, vereist een [netwerktaakverdeler](../../../load-balancer/load-balancer-overview.md). De load balancer bevat de IP-adressen voor de beschikbaarheidsgroeplistener. Als er meer dan één beschikbaarheidsgroep moet elke groep een listener. Één load balancer biedt ondersteuning voor meerdere listeners.

Wanneer u gereed bent voor het bouwen van een aroup van de beschikbaarheid van SQL Server op Azure Virtual Machines, verwijzen naar deze zelfstudies.

## <a name="automatically-create-an-availability-group-from-a-template"></a>Een beschikbaarheidsgroep automatisch maken van een sjabloon

[AlwaysOn-beschikbaarheidsgroep configureren voor de Resource Manager in Azure VM automatisch-](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)

## <a name="manually-create-an-availability-group-in-azure-portal"></a>Een beschikbaarheidsgroep handmatig maken in Azure portal

U kunt ook de virtuele machines zelf maken zonder de sjabloon. Eerst, voldoen aan de vereisten en de beschikbaarheidsgroep te maken. Zie de volgende onderwerpen: 

- [Vereisten voor SQL Server AlwaysOn-beschikbaarheidsgroepen configureren op Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [Maak altijd op beschikbaarheidsgroep voor het verbeteren van de beschikbaarheid en herstel na noodgevallen](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a>Volgende stappen

[Configureren van een SQL Server altijd op beschikbaarheidsgroep op Azure virtuele Machines in verschillende regio's](virtual-machines-windows-portal-sql-availability-group-dr.md).
