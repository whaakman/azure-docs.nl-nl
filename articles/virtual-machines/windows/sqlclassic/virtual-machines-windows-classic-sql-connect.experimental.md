---
title: Verbinding maken met een virtuele Machine van SQL Server op Azure (klassiek) | Microsoft Docs
description: Informatie over het verbinding maken met SQL Server wordt uitgevoerd op een virtuele Machine in Azure. In dit onderwerp maakt gebruik van het klassieke implementatiemodel. De scenario's, is afhankelijk van de netwerkconfiguratie en de locatie van de client.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-service-management
ms.assetid: 416948af-454f-4cfe-8fd2-7cf971cbd3e9
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: jroth
experimental_id: d51f3cc6-753b-4e
ms.openlocfilehash: 8602decfa35a21e88885fda1a4c70264bb38444b
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Verbinding maken met een virtuele SQL Server-machine op Azure (klassieke implementatie)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-connect.md)
> * [Klassiek](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Overzicht
Dit onderwerp wordt beschreven hoe u verbinding maken met uw SQL Server-exemplaar dat is uitgevoerd op een virtuele machine van Azure. Dit omvat een aantal [algemene verbindingsproblemen scenario's](#connection-scenarios) en geeft vervolgens [gedetailleerde stappen voor het configureren van SQL-serververbindingen in een Azure VM](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Als u virtuele machines van Resource Manager gebruikt, raadpleegt u [Connect naar een virtuele Machine in Azure met Resource Manager van SQL Server](../sql/virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Scenario 's
De manier waarop die een client verbinding maakt met SQL Server op een virtuele Machine wordt uitgevoerd, is afhankelijk van de locatie van de client en de configuratie van de machine/netwerken. Deze scenario's omvatten:

* [Verbinding maken met SQL Server in dezelfde cloudservice](#connect-to-sql-server-in-the-same-cloud-service)
* [Via internet verbinding maken met SQL Server](#connect-to-sql-server-over-the-internet)
* [Verbinding maken met SQL Server in hetzelfde virtuele netwerk](#connect-to-sql-server-in-the-same-virtual-network)

> [!NOTE]
> Voordat u verbinding met een van deze methoden maakt, volgt u de [stappen in dit artikel voor het configureren van connectiviteit](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).
> 
> 

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Verbinding maken met SQL Server in dezelfde cloudservice
Meerdere virtuele machines kunnen worden gemaakt in dezelfde cloudservice. Zie voor informatie over dit scenario met virtuele machines, [verbinding maken tussen virtuele machines met een virtueel netwerk of cloud service](../classic/connect-vms-classic.md#connect-vms-in-a-standalone-cloud-service). In dit scenario wordt een client op een virtuele machine probeert te maken met SQL Server op een andere virtuele machine in dezelfde cloudservice wordt uitgevoerd.

In dit scenario kunt u verbinding maken met behulp van de virtuele machine **naam** (ook weergegeven als **computernaam** of **hostnaam** in de portal). Dit is de naam die u hebt opgegeven voor de virtuele machine tijdens het maken van. Bijvoorbeeld, als u met de naam van de VM SQL **mysqlvm**, een VM-client in dezelfde cloudservice kan de volgende verbindingsreeks gebruiken om verbinding te:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Verbinding maken met SQL Server via Internet
Als u wilt verbinding maken met uw SQL Server database-engine van het Internet, moet u een virtuele machine-eindpunt voor binnenkomende TCP-communicatie. Deze configuratiestap van Azure zorgt ervoor dat binnenkomend TCP-poortverkeer naar een TCP-poort wordt geleid die toegankelijk is voor de virtuele machine.

Als u wilt verbinden via het internet, moet u de VM DNS-naam en het poortnummer van de VM-eindpunt (geconfigureerd verderop in dit artikel). Als de DNS-naam, navigeer naar de Azure-portal en selecteert **virtuele machines (klassiek)**. Selecteer vervolgens de virtuele machine. De **DNS-naam** wordt weergegeven in de **overzicht** sectie.

Neem bijvoorbeeld een klassieke virtuele machine met de naam **mysqlvm** met een DNS-naam van **mysqlvm7777.cloudapp.net** en een VM-eindpunt van **57500**. Ervan uitgaande dat een correct geconfigureerde connectiviteit, de volgende verbindingsreeks kan worden gebruikt voor toegang tot de virtuele machine vanaf elke locatie op internet:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Hoewel deze verbindingsreeks wordt de connectiviteit mogelijk voor clients via internet, betekent dit niet dat iedereen verbinding kan maken met uw SQL-Server. Buiten hebben clients op de juiste gebruikersnaam en wachtwoord. Voor extra beveiliging, niet de standaardpoort 1433 gebruiken voor het eindpunt van de openbare virtuele machine. En, indien mogelijk, overweegt u het toevoegen van een ACL van uw eindpunt moet verkeer te beperken tot de clients alleen is toegestaan. Zie voor instructies over het gebruik van ACL's met eindpunten [beheren de ACL voor een eindpunt](../classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).

> [!NOTE]
> Wanneer u deze techniek gebruiken om te communiceren met SQL Server, alle uitgaande gegevens van de Azure-datacenter is onderworpen aan normaal [prijzen voor uitgaande gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/).
> 
> 

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Verbinding maken met SQL Server in hetzelfde virtuele netwerk
[Virtueel netwerk](../../../virtual-network/virtual-networks-overview.md) kunnen aanvullende scenario's. Zelfs als deze virtuele machines zijn opgenomen in andere cloudservices, kunt u virtuele machines in hetzelfde virtuele netwerk. En met een [site-naar-site VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), kunt u een hybride-architectuur die virtuele machines met on-premises netwerken en machines verbindt.

Virtuele netwerken kunnen u uw Azure VM's toevoegen aan een domein. Toevoegen aan een domein, is de enige manier om het gebruik van Windows-verificatie met SQL Server. De andere scenario's vereisen SQL-verificatie met gebruikersnamen en wachtwoorden.

Als u van plan bent om een domeinomgeving en de Windows-verificatie te configureren, hoeft u geen openbaar eindpunt of de SQL-verificatie en aanmeldingen configureren. In dit scenario kunt u verbinding met uw exemplaar van SQL Server door op te geven van de virtuele machine van SQL Server-naam in de verbindingstekenreeks. Het volgende voorbeeld wordt ervan uitgegaan dat de Windows-verificatie is geconfigureerd en dat de gebruiker toegang tot de SQL Server-exemplaar gekregen heeft.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Stappen voor het configureren van SQL-serververbindingen in een Azure VM
De volgende stappen laten zien hoe u verbinding maken met SQL Server-exemplaar via internet met behulp van SQL Server Management Studio (SSMS). Echter, zijn dezelfde stappen van toepassing op het toegankelijk is voor uw toepassingen, zowel on-premises uitgevoerd en in Azure maken van uw virtuele machine van SQL Server.

Voordat u verbinding met het exemplaar van SQL Server uit een andere virtuele machine of op het internet maken kunt, moet u de volgende taken uitvoeren:

* [Een TCP-eindpunt voor de virtuele machine maken](#create-a-tcp-endpoint-for-the-virtual-machine)
* [TCP-poorten openen in de Windows-firewall](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
* [SQL Server configureren voor het luisteren op het TCP-protocol](#configure-sql-server-to-listen-on-the-tcp-protocol)
* [SQL Server configureren voor verificatie in gemengde modus](#configure-sql-server-for-mixed-mode-authentication)
* [SQL Server-verificatie-aanmeldingen maken](#create-sql-server-authentication-logins)
* [Bepalen van de DNS-naam van de virtuele machine](#determine-the-dns-name-of-the-virtual-machine)
* [Verbinding maken met de Database-Engine vanaf een andere computer](#connect-to-the-database-engine-from-another-computer)

Het verbindingspad wordt met het volgende diagram samengevat:

![Verbinding maken met een virtuele machine van SQL Server](../../../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[!INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[!INCLUDE [Connect to SQL Server in a VM](../../../../includes/virtual-machines-sql-server-connection-steps.md)]

[!INCLUDE [Connect to SQL Server in a VM Classic Steps](../../../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Volgende stappen
Als u ook met AlwaysOn-beschikbaarheidsgroepen voor hoge beschikbaarheid en herstel na noodgevallen plant, kunt u overwegen een listener te implementeren. Database-clients verbinding maken voor de listener in plaats van rechtstreeks op een van de SQL Server-exemplaren. De listener routeert clients naar de primaire replica in de beschikbaarheidsgroep. Zie voor meer informatie [een ILB-listener voor AlwaysOn-beschikbaarheidsgroepen configureren in Azure](../classic/ps-sql-int-listener.md).

Het is belangrijk om te controleren van alle de aanbevolen beveiligingsprocedures voor SQL Server op Azure een virtuele machine wordt uitgevoerd. Zie [Veiligheidsoverwegingen voor SQL Server in virtuele Azure-machines](../sql/virtual-machines-windows-sql-security.md) voor meer informatie.

[Bekijk het leertraject](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) voor virtuele SQL Server-machines in Azure. 

Zie voor andere onderwerpen die betrekking hebben op SQL Server wordt uitgevoerd in Azure VM's [SQL Server op Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

