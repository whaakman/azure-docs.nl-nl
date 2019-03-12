---
title: Beschikbaarheid van SQL Server - virtuele Machines van Azure - noodherstel groepen | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u een SQL Server-beschikbaarheidsgroep configureren op Azure virtual machines met een replica in een andere regio.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
manager: craigg
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
ms.openlocfilehash: 5e4b63d4fc8b437bd4d476c72d23f9dbf5242d5b
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57780555"
---
# <a name="configure-an-always-on-availability-group-on-azure-virtual-machines-in-different-regions"></a>Een AlwaysOn-beschikbaarheidsgroep configureren op Azure virtual machines in verschillende regio 's

In dit artikel wordt uitgelegd hoe u een SQL Server Always On availability group replica configureren op Azure virtual machines op een externe Azure-locatie. Deze configuratie gebruiken voor ondersteuning van herstel na noodgevallen.

In dit artikel is van toepassing op Azure Virtual Machines in Resource Manager-modus.

De volgende afbeelding toont een algemene implementatie van een beschikbaarheidsgroep op Azure virtual machines:

   ![Beschikbaarheidsgroep](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic.png)

In deze implementatie worden alle virtuele machines in een Azure-regio. De beschikbaarheidsgroepreplica's kunnen synchrone doorvoer met automatische failover op de SQL-1 en SQL-2 hebben. Deze architectuur, Raadpleeg [Availability Group-sjabloon of zelfstudie](virtual-machines-windows-portal-sql-availability-group-overview.md).

Deze architectuur is kwetsbaar voor downtime als de Azure-regio niet meer toegankelijk. Toevoegen om te strijden tegen dit beveiligingsprobleem, een replica in een andere Azure-regio. Het volgende diagram toont hoe de nieuwe architectuur eruit zouden zien:

   ![Beschikbaarheid van groep DR](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic-dr.png)

Het vorige diagram toont een nieuwe virtuele machine met de naam SQL-3. SQL-3 is in een andere Azure-regio. SQL-3 wordt toegevoegd aan de Windows Server-failovercluster. SQL-3 kan een beschikbaarheidsreplica van de groep host. Ten slotte u ziet dat de Azure-regio voor SQL-3 een nieuwe Azure load balancer.

>[!NOTE]
> Een Azure-beschikbaarheidsset is vereist wanneer meer dan één virtuele machine zich in dezelfde regio bevinden. Als er slechts één virtuele machine zich in de regio, klikt u vervolgens is de beschikbaarheidsset niet vereist. U kunt alleen een virtuele machine in een beschikbaarheidsset tijdens de aanmaak plaatsen. Als de virtuele machine al in een beschikbaarheidsset is, kunt u een virtuele machine voor een extra replica later toevoegen.

In deze architectuur is normaal gesproken de replica in de externe regio geconfigureerd met asynchrone doorvoermodus voor beschikbaarheid en handmatige failover-modus.

Wanneer de beschikbaarheidsgroepreplica's zijn op Azure virtual machines in verschillende Azure-regio's, moet elke regio:

* Een virtuele netwerkgateway
* Een virtuele netwerkgatewayverbinding

Het volgende diagram toont hoe de netwerken met elkaar communiceren tussen datacenters.

   ![Beschikbaarheidsgroep](./media/virtual-machines-windows-portal-sql-availability-group-dr/01-vpngateway-example.png)

>[!IMPORTANT]
>Deze architectuur leidt tot kosten voor uitgaande gegevens voor gegevens die worden gerepliceerd tussen Azure-regio's. Zie [bandbreedte prijzen](https://azure.microsoft.com/pricing/details/bandwidth/).  

## <a name="create-remote-replica"></a>Externe replica maken

Voor het maken van een replica in een extern Datacenter, voer de volgende stappen uit:

1. [Een virtueel netwerk maken in de nieuwe regio](../../../virtual-network/manage-virtual-network.md#create-a-virtual-network).

1. [Configureren van een VNet-naar-VNet-verbinding met de Azure portal](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).

   >[!NOTE]
   >In sommige gevallen mogelijk hebt u PowerShell gebruikt om de VNet-naar-VNet-verbinding te maken. Bijvoorbeeld, als u verschillende Azure-accounts gebruikt configureren u niet de verbinding in de portal. In dit geval ziet, [configureren van een VNet-naar-VNet-verbinding met de Azure portal](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

1. [Maken van een domeincontroller in de nieuwe regio](../../../active-directory/active-directory-new-forest-virtual-machine.md).

   Deze domeincontroller biedt verificatie als de domeincontroller in de primaire site niet beschikbaar is.

1. [Een SQL Server-machine maken in de nieuwe regio](virtual-machines-windows-portal-sql-server-provision.md).

1. [Maak een Azure load balancer in het netwerk op de nieuwe regio](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

   Deze load balancer moet:

   - Zich bevinden in hetzelfde netwerk en subnet als de nieuwe virtuele machine.
   - Een statisch IP-adres voor de beschikbaarheidsgroep-listener hebben.
   - Een back-endpool die bestaan uit alleen de virtuele machines in dezelfde regio als de load balancer bevatten.
   - Gebruik een TCP-poort test die specifiek zijn voor het IP-adres.
   - Een load balancing-regel die specifiek zijn voor de SQL-Server in dezelfde regio hebben.  
   - Alleen een Standard Load Balancer als de virtuele machines in de back-endpool geen deel uit van een enkele beschikbaarheidsset of virtuele-machineschaalset maken. Voor aanvullende informatie revisie [overzicht van Azure Load Balancer Standard](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).

1. [Functie Failover Clustering toevoegen aan de nieuwe SQL-Server](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

1. [De nieuwe SQL-Server toevoegen aan het domein](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

1. [Instellen van de nieuwe SQL Server-serviceaccount voor een domeinaccount gebruiken](virtual-machines-windows-portal-sql-availability-group-prereq.md#setServiceAccount).

1. [De nieuwe SQL-Server toevoegen aan Windows Server Failover Cluster](virtual-machines-windows-portal-sql-availability-group-tutorial.md#addNode).

1. Een IP-adresresource maken op het cluster.

   U kunt de IP-adresresource maken in Failoverclusterbeheer. Met de rechtermuisknop op de rol van de groep beschikbaarheid, klikt u op **Resource toevoegen**, **meer bronnen**, en klikt u op **IP-adres**.

   ![IP-adres maken](./media/virtual-machines-windows-portal-sql-availability-group-dr/20-add-ip-resource.png)

   Dit IP-adres als volgt configureren:

   - Het netwerk van het externe Datacenter gebruiken.
   - Het IP-adres van de nieuwe Azure load balancer toewijzen. 

1. Op de nieuwe SQL-Server in SQL Server Configuration Manager [inschakelen Always On Availability Groups](https://msdn.microsoft.com/library/ff878259.aspx).

1. [Open firewallpoorten op de nieuwe SQL-Server](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).

   De poortnummers die u wilt openen, is afhankelijk van uw omgeving. Open poorten voor de mirroring-eindpunt en de Azure load balancer-statustest.

1. [Een replica toevoegen aan de beschikbaarheidsgroep op de nieuwe SQL-Server](https://msdn.microsoft.com/library/hh213239.aspx).

   Voor een replica in een externe Azure-regio, moet u deze voor asynchrone replicatie met handmatige failover instellen.  

1. De resource van het IP-adres toevoegen als een afhankelijkheid voor de listener client access point (netwerknaam) cluster.

   De volgende schermafbeelding ziet u een correct geconfigureerde clusterbron van de IP-adres:

   ![Beschikbaarheidsgroep](./media/virtual-machines-windows-portal-sql-availability-group-dr/50-configure-dependency-multiple-ip.png)

   >[!IMPORTANT]
   >De clusterbrongroep bevat beide IP-adressen. Beide IP-adressen zijn afhankelijkheden voor de listener client access point. Gebruik de **of** operator in de configuratie van het cluster-afhankelijkheid.

1. [De clusterparameters instellen in PowerShell](virtual-machines-windows-portal-sql-availability-group-tutorial.md#setparam).

Voer de PowerShell-script met de naam van clusternetwerk, het IP-adres en de testpoort die u hebt geconfigureerd op de load balancer in de nieuwe regio.

   ```PowerShell
   $ClusterNetworkName = "<MyClusterNetworkName>" # The cluster name for the network in the new region (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name).
   $IPResourceName = "<IPResourceName>" # The cluster name for the new IP Address resource.
   $ILBIP = “<n.n.n.n>” # The IP Address of the Internal Load Balancer (ILB) in the new region. This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ProbePort = <nnnnn> # The probe port you set on the ILB.

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

## <a name="set-connection-for-multiple-subnets"></a>Set-verbinding voor meerdere subnetten

De replica in het externe Datacenter maakt deel uit van de beschikbaarheidsgroep, maar het is in een ander subnet. Als deze replica de primaire replica de verbindingstime-outs toepassing optreden wordt. Dit gedrag is hetzelfde als een on-premises beschikbaarheidsgroep in een implementatie met meerdere subnetten. Als u wilt toestaan verbindingen van client-toepassingen, verbinding met de client bijwerken of naamomzetting opslaan in cache op de netwerknaam van cluster configureren.

Bij voorkeur, werk de client-verbindingsreeksen om in te stellen `MultiSubnetFailover=Yes`. Zie [verbinding te maken met MultiSubnetFailover](https://msdn.microsoft.com/library/gg471494#Anchor_0).

Als u de verbindingsreeksen niet wijzigen, configureert u de naam van het probleem zou moeten opslaan in cache. Zie [verbindingstime-outs in meerdere subnetten beschikbaarheidsgroep](https://blogs.msdn.microsoft.com/alwaysonpro/2014/06/03/connection-timeouts-in-multi-subnet-availability-group/).

## <a name="fail-over-to-remote-region"></a>Failover naar een externe regio

Als u wilt testen listener de verbinding met de externe regio, kunt u voor de replica naar de externe regio failover. De replica is asynchroon, is failover kwetsbaar voor mogelijk gegevensverlies. Wijzig de beschikbaarheidsmodus in synchrone failover zonder verlies van gegevens, en de failover-modus ingesteld op automatisch. Voer de volgende stappen uit:

1. In **Objectverkenner**, verbinding met het exemplaar van SQL Server die als host fungeert voor de primaire replica.
1. Onder **AlwaysOn Availability Groups**, **beschikbaarheidsgroepen**, met de rechtermuisknop op de beschikbaarheidsgroep en klikt u op **eigenschappen**.
1. Op de **algemene** pagina onder **Beschikbaarheidsreplica**, instellen van de secundaire replica in de DR-site moet gebruiken **synchroon doorvoeren** beschikbaarheidsmodus en  **Automatische** failover-modus.
1. Als u een secundaire replica in dezelfde site als de primaire replica voor hoge beschikbaarheid hebt, stelt u deze replica op **asynchrone doorvoeren** en **handmatig**.
1. Klik op OK.
1. In **Objectverkenner**, met de rechtermuisknop op de beschikbaarheidsgroep en klikt u op **Dashboard weergeven**.
1. Controleer op het dashboard, of dat de replica op de DR-site wordt gesynchroniseerd.
1. In **Objectverkenner**, met de rechtermuisknop op de beschikbaarheidsgroep en klikt u op **Failover...** . SQL Server Management Studio's Hiermee opent u een wizard voor failover van SQL Server.  
1. Klik op **volgende**, en selecteer het SQL Server-exemplaar in de DR-site. Klik op **volgende** opnieuw.
1. Verbinding maken met de SQL Server-exemplaar in de DR-site en op **volgende**.
1. Op de **samenvatting** pagina, Controleer de instellingen en klik op **voltooien**.

Nadat het testen van de connectiviteit, de primaire replica terug verplaatsen naar uw primaire datacenter en stel de beschikbaarheidsmodus terug naar de normale operationele instellingen. De volgende tabel ziet u de normale operationele instellingen voor de architectuur die worden beschreven in dit document:

| Locatie | Server-exemplaar | Rol | Modus voor beschikbaarheid | Failover-modus
| ----- | ----- | ----- | ----- | -----
| Primaire Datacenter | SQL-1 | Primair | Synchrone | Automatisch
| Primaire Datacenter | SQL-2 | Secundair | Synchrone | Automatisch
| Secundaire of externe Datacenter | SQL-3 | Secundair | Asynchrone | Handmatig


### <a name="more-information-about-planned-and-forced-manual-failover"></a>Meer informatie over geplande en geforceerde handmatige failover

Zie de volgende onderwerpen voor meer informatie:

- [Voer een geplande handmatige Failover van een beschikbaarheidsgroep (SQL Server)](https://msdn.microsoft.com/library/hh231018.aspx)
- [Voer een geforceerde handmatige Failover van een beschikbaarheidsgroep (SQL Server)](https://msdn.microsoft.com/library/ff877957.aspx)

## <a name="additional-links"></a>Aanvullende koppelingen

* [Always On Availability Groups](https://msdn.microsoft.com/library/hh510230.aspx)
* [Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/windows/)
* [Azure Load Balancers](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer)
* [Azure-Beschikbaarheidssets](../manage-availability.md)
