---
title: Load Balancer voor SQL Server altijd configureren op | Microsoft Docs
description: Load Balancer om te werken met SQL Server Always On en informatie over het gebruik van PowerShell voor het maken van een load balancer voor de uitvoering van SQL Server configureren
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
ms.assetid: d7bc3790-47d3-4e95-887c-c533011e4afd
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 5e890f8314c8f191dbfa6c6818d810b91d0e829d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="configure-a-load-balancer-for-sql-server-always-on"></a>Een load balancer configureren voor SQL Server Always On

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

SQL Server AlwaysOn-beschikbaarheidsgroepen nu uitvoert met een interne load balancer. Een beschikbaarheidsgroep is SQL-Server vlaggenschip oplossing voor hoge beschikbaarheid en herstel na noodgevallen. De beschikbaarheidsgroep-listener kan client toepassingen naadloos verbinding kunnen maken met de primaire replica, ongeacht het aantal replica's in de configuratie.

De naam van de listener (DNS) is toegewezen aan een load balanced IP-adres. Azure Load Balancer stuurt het binnenkomende verkeer naar alleen de primaire server in de replicaset.

U kunt interne load balancer-ondersteuning voor SQL Server Always On (listener)-eindpunten. U hebt nu de controle over de toegankelijkheid van de listener. U kunt het netwerktaakverdeling IP-adres van een specifiek subnet in het virtuele netwerk.

Met behulp van een interne load balancer op de listener, het eindpunt van de SQL Server (bijvoorbeeld Server = tcp:ListenerName, 1433; Database = DatabaseName) is alleen toegankelijk is voor:

* Services en virtuele machines in hetzelfde virtuele netwerk.
* Services en virtuele machines van de verbonden on-premises netwerken.
* Services en virtuele machines van onderling verbonden virtuele netwerken.

![Interne load balancer SQL Server Always On](./media/load-balancer-configure-sqlao/sqlao1.png)

## <a name="add-an-internal-load-balancer-to-the-service"></a>Een interne load balancer toevoegen aan de service

1. In het volgende voorbeeld kunt u een virtueel netwerk met een subnet met de naam Subnet-1 configureren:

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. Taakverdeling eindpunten voor een interne load balancer toevoegen op elke virtuele machine.

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    In het vorige voorbeeld hebt u twee virtuele machines 'sqlsvc1' en 'sqlsvc2' genoemd en die worden uitgevoerd in de cloudservice 'Sqlsvc'. Nadat het maken van de interne load balancer met de `DirectServerReturn` overschakelen, u Netwerktaakverdeling eindpunten toevoegen aan de interne load balancer. De eindpunten van taakverdeling toestaan listeners configureren voor de beschikbaarheidsgroepen van SQL Server.

Zie voor meer informatie over SQL Server Always On [een interne load balancer voor een AlwaysOn-beschikbaarheidsgroep configureren in Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Zie ook
* [Configureren van een openbare load balancer aan de slag](load-balancer-get-started-internet-arm-ps.md)
* [Aan de slag met het configureren van een interne load balancer](load-balancer-get-started-ilb-arm-ps.md)
* [Een distributiemodus voor de load balancer configureren](load-balancer-distribution-mode.md)
* [TCP-time-outinstellingen voor inactiviteit voor de load balancer configureren](load-balancer-tcp-idle-timeout.md)
