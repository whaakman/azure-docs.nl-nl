---
title: Beschikbaarheid van SQL Server - virtuele Machines in Azure - noodherstel groepen | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u een SQL Server-beschikbaarheidsgroep configureren op Azure virtuele machines met een replica in een andere regio.
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 388c464e-a16e-4c9d-a0d5-bb7cf5974689
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: 1ce90cf4bae66bfd6387a2698fd9b1ba7fc64595
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-an-always-on-availability-group-on-azure-virtual-machines-in-different-regions"></a>Een AlwaysOn-beschikbaarheidsgroep configureren op virtuele Azure-machines in verschillende regio 's

In dit artikel wordt uitgelegd hoe u een SQL Server Always On beschikbaarheidsgroepreplica's op virtuele Azure-machines op een externe locatie voor Azure configureren. Deze configuratie gebruiken ter ondersteuning van herstel na noodgevallen.

In dit artikel is van toepassing op Azure Virtual Machines in de modus Resource Manager.

De volgende afbeelding toont een algemene implementatie van een beschikbaarheidsgroep op Azure virtuele machines:

   ![Beschikbaarheidsgroep](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic.png)

In deze implementatie worden alle virtuele machines in een Azure-regio. De beschikbaarheidsgroepreplica's kunnen synchrone doorvoer met automatische failover op de SQL-1 en SQL-2 hebben. Als u wilt maken van deze architectuur, Zie [beschikbaarheidsgroep sjabloon of zelfstudie](virtual-machines-windows-portal-sql-availability-group-overview.md).

Deze architectuur is kwetsbaar voor downtime als de Azure-regio niet toegankelijk wordt. Toevoegen voor het opheffen van deze kwetsbaarheid, een replica in een andere Azure-regio. Het volgende diagram toont de nieuwe architectuur zou als volgt uitzien:

   ![Beschikbaarheid van groep DR](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic-dr.png)

De voorgaande diagram ziet u een nieuwe virtuele machine SQL 3 aangeroepen. SQL-3 is in een andere Azure-regio. SQL-3 wordt toegevoegd aan de Windows Server-failovercluster. SQL-3 kan een beschikbaarheidsreplica van de groep host. Ten slotte kunt u ziet dat de Azure-regio voor SQL-3 een nieuwe Azure load balancer heeft.

>[!NOTE]
> Een Azure beschikbaarheidsset is vereist wanneer meer dan één virtuele machine bevindt zich in dezelfde regio. De beschikbaarheidsset is niet vereist als er slechts één virtuele machine zich in de regio. U kunt alleen een virtuele machine in een beschikbaarheidsset tijdens de aanmaak plaatsen. Als de virtuele machine is al in een beschikbaarheidsset bevindt, kunt u een virtuele machine voor een extra replica later toevoegen.

In deze architectuur wordt de replica in de externe regio normaal gesproken geconfigureerd met asynchrone doorvoermodus voor beschikbaarheid en handmatige failover-modus.

Als replica's van beschikbaarheidsgroepen op Azure virtuele machines in verschillende Azure-regio's, moet elke regio:

* Een virtuele netwerkgateway
* Een verbinding met virtual network gateway

Het volgende diagram toont hoe de netwerken communiceren tussen datacenters.

   ![Beschikbaarheidsgroep](./media/virtual-machines-windows-portal-sql-availability-group-dr/01-vpngateway-example.png)

>[!IMPORTANT]
>Deze architectuur leidt ertoe dat kosten voor uitgaande gegevens voor gegevens gerepliceerd tussen Azure-regio's. Zie [bandbreedte prijzen](http://azure.microsoft.com/pricing/details/bandwidth/).  

## <a name="create-remote-replica"></a>Externe replica maken

Voor het maken van een replica in een externe Datacenter, voer de volgende stappen uit:

1. [Een virtueel netwerk maken in de nieuwe regio](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

1. [Configureren van een VNet-naar-VNet-verbinding met de Azure portal](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).

   >[!NOTE]
   >In sommige gevallen moet u wellicht PowerShell gebruiken om de VNet-naar-VNet-verbinding te maken. Als u verschillende Azure-accounts gebruiken niet kunt u de verbinding configureren in de portal. In dit geval ziet, [configureren van een VNet-naar-VNet-verbinding met de Azure portal](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

1. [Maken van een domeincontroller in het nieuwe gebied](../../../active-directory/active-directory-new-forest-virtual-machine.md).

   Deze domeincontroller biedt verificatie als de domeincontroller in de primaire site niet beschikbaar is.

1. [Maken van een virtuele machine van SQL Server in de nieuwe regio](virtual-machines-windows-portal-sql-server-provision.md).

1. [Maak een Azure load balancer in het netwerk op de nieuwe regio](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

   Deze load balancer moet:

   - Zich bevinden in hetzelfde netwerk en subnet als de nieuwe virtuele machine.
   - Een statisch IP-adres hebben voor de beschikbaarheidsgroeplistener.
   - Een back-endpool die bestaan uit alleen de virtuele machines in dezelfde regio bevinden als de load balancer bevatten.
   - Gebruik een TCP-poort test die specifiek zijn voor het IP-adres.
   - Een taakverdelingsregel specifiek zijn voor de SQL-Server in dezelfde regio hebben.  

1. [Functie Failover Clustering toevoegen aan de nieuwe SQL-Server](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

1. [De nieuwe SQL-Server toevoegen aan het domein](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

1. [Instellen van de nieuwe SQL Server-serviceaccount voor een domeinaccount gebruiken](virtual-machines-windows-portal-sql-availability-group-prereq.md#setServiceAccount).

1. [De nieuwe SQL-Server toevoegen aan Windows Server Failover Cluster](virtual-machines-windows-portal-sql-availability-group-tutorial.md#addNode).

1. Maak een bron van de IP-adres op het cluster.

   U kunt de IP-adres-resource maken in Failoverclusterbeheer. Met de rechtermuisknop op de rol van de groep beschikbaarheid, klikt u op **Resource toevoegen**, **meer bronnen**, en klik op **IP-adres**.

   ![IP-adres maken](./media/virtual-machines-windows-portal-sql-availability-group-dr/20-add-ip-resource.png)

   Dit IP-adres als volgt configureren:

   - Het netwerk van het externe Datacenter gebruiken.
   - Wijs het IP-adres van de nieuwe Azure load balancer. 

1. Op de nieuwe SQL-Server in SQL Server Configuration Manager [inschakelen altijd op beschikbaarheidsgroepen](http://msdn.microsoft.com/library/ff878259.aspx).

1. [Open firewallpoorten op de nieuwe SQL-Server](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).

   De poortnummers die u wilt openen, is afhankelijk van uw omgeving. Open poorten voor de spiegeling eindpunt en Azure load balancer health test.

1. [Een replica toevoegen aan de beschikbaarheidsgroep op de nieuwe SQL-Server](http://msdn.microsoft.com/library/hh213239.aspx).

   Voor een replica in een externe Azure-regio, moet u deze voor asynchrone replicatie met handmatige failover instellen.  

1. De bron van het IP-adres toevoegen als een afhankelijkheid voor de listener client access point (netwerknaam) cluster.

   De volgende schermafbeelding ziet een correct geconfigureerde clusterbron voor IP-adres:

   ![Beschikbaarheidsgroep](./media/virtual-machines-windows-portal-sql-availability-group-dr/50-configure-dependency-multiple-ip.png)

   >[!IMPORTANT]
   >De clusterbrongroep bevat beide IP-adressen. Afhankelijkheden voor de listener client access point-beide IP-adressen zijn. Gebruik de **of** -operator in de configuratie van het cluster-afhankelijkheid.

1. [Stel de clusterparameters in PowerShell](virtual-machines-windows-portal-sql-availability-group-tutorial.md#setparam).

Voer de PowerShell-script met de clusternetwerknaam, IP-adres en testpoort die u hebt geconfigureerd op de load balancer in de nieuwe regio.

   ```PowerShell
   $ClusterNetworkName = "<MyClusterNetworkName>" # The cluster name for the network in the new region (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name).
   $IPResourceName = "<IPResourceName>" # The cluster name for the new IP Address resource.
   $ILBIP = “<n.n.n.n>” # The IP Address of the Internal Load Balancer (ILB) in the new region. This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ProbePort = <nnnnn> # The probe port you set on the ILB.

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

## <a name="set-connection-for-multiple-subnets"></a>Set-verbinding voor meerdere subnetten

De replica in het externe Datacenter maakt deel uit van de beschikbaarheidsgroep, maar het is in een ander subnet. Als deze replica de primaire replica wordt, kunnen de time-outs van de toepassing optreden. Dit gedrag is hetzelfde als een lokale beschikbaarheidsgroep in een implementatie met meerdere subnetten. Als u wilt toestaan verbindingen vanaf client toepassingen, verbinding met de client bijwerken of opslaan in cache op de cluster-netwerknaam naamomzetting configureren.

Bij voorkeur, werk de verbindingsreeksen van de client in te stellen `MultiSubnetFailover=Yes`. Zie [verbinding te maken met MultiSubnetFailover](http://msdn.microsoft.com/library/gg471494#Anchor_0).

Als u de verbindingsreeksen niet wijzigen, kunt u de naam resolutie caching kunt configureren. Zie [time-outs voor verbindingen in meerdere subnetten beschikbaarheidsgroep](http://blogs.msdn.microsoft.com/alwaysonpro/2014/06/03/connection-timeouts-in-multi-subnet-availability-group/).

## <a name="fail-over-to-remote-region"></a>Failover naar externe regio

Als u wilt testen listener-verbindingsproblemen met de externe regio, kunt u de replica voor de externe regio failover. Terwijl de replica asynchroon is, is failover kwetsbaar voor mogelijk gegevensverlies. Wijzig de beschikbaarheidsmodus in synchrone failover zonder verlies van gegevens, en de failover-modus ingesteld op automatisch. Voer de volgende stappen uit:

1. In **Objectverkenner**, verbinding met het exemplaar van SQL Server die als host fungeert voor de primaire replica.
1. Onder **AlwaysOn Availability Groups**, **beschikbaarheidsgroepen**, met de rechtermuisknop op de beschikbaarheidsgroep en klik op **eigenschappen**.
1. Op de **algemene** pagina onder **Beschikbaarheidsreplica**, instellen van de secundaire replica in de DR-site te gebruiken **synchroon doorvoeren** beschikbaarheidsmodus en **automatische** failover-modus.
1. Als u een secundaire replica in dezelfde site als de primaire replica voor hoge beschikbaarheid hebt, stelt u deze replica op **asynchrone doorvoeren** en **handmatige**.
1. Klik op OK.
1. In **Objectverkenner**, met de rechtermuisknop op de beschikbaarheidsgroep en klikt u op **Dashboard weergeven**.
1. Controleren of de replica op de DR-site is gesynchroniseerd op het dashboard.
1. In **Objectverkenner**, met de rechtermuisknop op de beschikbaarheidsgroep en klikt u op **Failover...** . SQL Server Management Studios Hiermee opent u de wizard een failover van SQL Server.  
1. Klik op **volgende**, en selecteer het SQL Server-exemplaar in de DR-site. Klik op **volgende** opnieuw.
1. Verbinding maken met de SQL Server-exemplaar in de DR-site en klik op **volgende**.
1. Op de **samenvatting** pagina, Controleer de instellingen en klik op **voltooien**.

Na de verbinding test, de primaire replica naar uw primaire Datacenter gaan en de beschikbaarheidsmodus weer instellen op hun normale operationele instellingen. De volgende tabel ziet u de normale operationele instellingen voor de architectuur die in dit document beschreven:

| Locatie | Server-exemplaar | Rol | De Beschikbaarheidsmodus | Failover-modus
| ----- | ----- | ----- | ----- | -----
| Primaire Datacenter | SQL-1 | Primair | Synchrone | Automatisch
| Primaire Datacenter | SQL-2 | Secundair | Synchrone | Automatisch
| Secundaire of door de externe Datacenter | SQL-3 | Secundair | Asynchrone | Handmatig


### <a name="more-information-about-planned-and-forced-manual-failover"></a>Meer informatie over de geplande en geforceerde handmatige failover

Zie de volgende onderwerpen voor meer informatie:

- [Voer een geplande handmatige Failover van een beschikbaarheidsgroep (SQL Server)](http://msdn.microsoft.com/library/hh231018.aspx)
- [Voer een geforceerde handmatige Failover van een beschikbaarheidsgroep (SQL Server)](http://msdn.microsoft.com/library/ff877957.aspx)

## <a name="additional-links"></a>Aanvullende koppelingen

* [Altijd op beschikbaarheidsgroepen](http://msdn.microsoft.com/library/hh510230.aspx)
* [Virtuele Machines in Azure](http://docs.microsoft.com/azure/virtual-machines/windows/)
* [Azure Load Balancers](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer)
* [Azure Beschikbaarheidssets](../manage-availability.md)
