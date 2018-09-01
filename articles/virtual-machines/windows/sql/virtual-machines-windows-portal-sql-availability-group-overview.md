---
title: Beschikbaarheid van SQL Server - virtuele Machines van Azure - overzicht groepen | Microsoft Docs
description: Dit artikel bevat beschikbaarheidsgroepen van SQL Server op Azure virtual machines.
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
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
ms.openlocfilehash: b71ba12f6c533d67b04366f05b9334e1993823fa
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/01/2018
ms.locfileid: "43382025"
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a>Maak kennis met SQL Server Always On-beschikbaarheidsgroepen op Azure virtual machines #

Dit artikel bevat beschikbaarheidsgroepen van SQL Server op Azure Virtual Machines. 

AlwaysOn-beschikbaarheidsgroepen op Azure Virtual Machines zijn vergelijkbaar met AlwaysOn-beschikbaarheidsgroepen on-premises. Zie voor meer informatie, [Always On Availability Groups (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx). 

Het diagram illustreert de onderdelen van een volledige SQL Server-beschikbaarheidsgroep in Azure Virtual Machines.

![Beschikbaarheidsgroep](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

Het belangrijkste verschil voor een beschikbaarheidsgroep in Azure Virtual Machines is dat de virtuele Azure-machines, vereist een [netwerktaakverdeler](../../../load-balancer/load-balancer-overview.md). De load balancer bevat de IP-adressen voor de beschikbaarheidsgroep-listener. Als u meer dan één beschikbaarheidsgroep hebt moet elke groep een listener. Een load balancer biedt ondersteuning voor meerdere listeners.

Wanneer u gereed voor het bouwen van een beschikbaarheidsgroep van SQL Server op Azure Virtual Machines bent, raadpleegt u deze zelfstudies.

## <a name="automatically-create-an-availability-group-from-a-template"></a>Automatisch een beschikbaarheidsgroep te maken van een sjabloon

[AlwaysOn-beschikbaarheidsgroep configureren voor de Resource Manager in Azure VM automatisch-](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)

## <a name="manually-create-an-availability-group-in-azure-portal"></a>Handmatig maken van een beschikbaarheidsgroep in Azure portal

U kunt ook de virtuele machines zelf maken zonder de sjabloon. Eerst, voldoen aan de vereisten en maakt u de beschikbaarheidsgroep. Zie de volgende onderwerpen: 

- [Vereisten voor SQL Server Always On-beschikbaarheidsgroepen configureren op Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [Maken van AlwaysOn-beschikbaarheidsgroep voor het verbeteren van de beschikbaarheid en herstel na noodgevallen](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a>Volgende stappen

[Een SQL Server Always On Availability Group op Azure Virtual Machines in verschillende regio's configureren](virtual-machines-windows-portal-sql-availability-group-dr.md).
