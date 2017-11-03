---
title: Load balancer voor de SQL AlwaysOn configureren op | Microsoft Docs
description: Configureer te werken met SQL altijd op Load balancer en hoe u powershell als u wilt maken van de load balancer voor de SQL-implementatie
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
ms.openlocfilehash: 3ebbf1c4009d89b1f18b2ff8ff5dd243c456dff8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-load-balancer-for-sql-always-on"></a>Load balancer voor SQL altijd configureren op

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

SQL Server AlwaysOn-beschikbaarheidsgroepen kan nu worden uitgevoerd met ILB. Beschikbaarheidsgroep is SQL-Server vlaggenschip oplossing voor hoge beschikbaarheid en herstel na noodgevallen. De beschikbaarheidsgroep-Listener kan client toepassingen naadloos verbinding kunnen maken met de primaire replica, ongeacht het aantal van de replica's in de configuratie.

De naam van de listener (DNS) is toegewezen aan een load balanced IP-adres en de Azure load balancer stuurt het binnenkomende verkeer naar alleen de primaire server in de replicaset.

U kunt de ILB ondersteuning voor SQL Server AlwaysOn (listener)-eindpunten. U nu hebben controle over de toegankelijkheid van de listener en de netwerktaakverdeling IP-adres kunt kiezen uit een specifiek subnet in het virtuele netwerk (VNet).

Met behulp van ILB op de listener, de SQL server-eindpunt (bijvoorbeeld Server = tcp:ListenerName, 1433; Database = DatabaseName) is alleen toegankelijk is voor:

* Services en virtuele machines in hetzelfde virtuele netwerk
* Services en virtuele machines van de verbonden on-premises netwerk
* Services en virtuele machines van de VNets met elkaar verbonden

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Afbeelding 1: de SQL AlwaysOn is geconfigureerd met Internet gerichte load balancer

## <a name="add-internal-load-balancer-to-the-service"></a>Interne Load Balancer toevoegen aan de service

1. Er wordt een virtueel netwerk met een subnet met de naam Subnet-1 geconfigureerd in het volgende voorbeeld:

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. Eindpunten van taakverdeling voor de ILB op elke virtuele machine toevoegen

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    In het bovenstaande voorbeeld hebt u 2 VM genoemd 'sqlsvc1' en 'sqlsvc2' wordt uitgevoerd in de cloud service 'Sqlsvc'. Na het maken van de ILB met `DirectServerReturn` switch u taakverdeling eindpunten toevoegen aan de ILB waarmee SQL voor listeners configureren voor de beschikbaarheidsgroepen.

Zie voor meer informatie over SQL AlwaysOn, [een interne load balancer voor een AlwaysOn-beschikbaarheidsgroep configureren in Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Zie ook
[De load balancer van een Internetgericht configureren aan de slag](load-balancer-get-started-internet-arm-ps.md)

[Een interne load balancer configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een distributiemodus voor de load balancer configureren](load-balancer-distribution-mode.md)

[TCP-time-outinstellingen voor inactiviteit voor de load balancer configureren](load-balancer-tcp-idle-timeout.md)
