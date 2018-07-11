---
title: Verbinding maken met een SQL Server-Machine in Azure (klassiek) | Microsoft Docs
description: Leer hoe u verbinding maken met SQL Server die wordt uitgevoerd op een virtuele Machine in Azure. In dit onderwerp wordt gebruikgemaakt van het klassieke implementatiemodel. De scenario's variëren, afhankelijk van de configuratie van netwerken en de locatie van de client.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
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
ms.openlocfilehash: c856c8c67d410a3b528c4f8b12b1225cf395bca4
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "29398366"
---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Verbinding maken met een virtuele SQL Server-machine op Azure (klassieke implementatie)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-connect.md)
> * [Klassiek](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Overzicht
In dit onderwerp wordt beschreven hoe u verbinding maken met uw exemplaar van SQL Server die wordt uitgevoerd op een virtuele machine van Azure. Deze staan enkele [algemene connectiviteitsscenario's](#connection-scenarios) en geeft vervolgens [gedetailleerde stappen voor het configureren van SQL Server-connectiviteit in een Azure-VM](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Als u virtuele machines Resource Manager gebruikt, Zie [verbinding maken met een SQL Server-VM op Azure met behulp van Resource Manager](../sql/virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Scenario 's
De manier waarop die een client verbinding maakt met SQL Server op een virtuele Machine wordt uitgevoerd, is afhankelijk van de locatie van de client en de configuratie van de machine/netwerken. Deze scenario's omvatten:

* [Verbinding maken met SQL Server in dezelfde cloudservice](#connect-to-sql-server-in-the-same-cloud-service)
* [Verbinding maken met SQL Server via internet](#connect-to-sql-server-over-the-internet)
* [Verbinding maken met SQL Server in hetzelfde virtuele netwerk](#connect-to-sql-server-in-the-same-virtual-network)

> [!NOTE]
> Voordat u verbinding met een van deze methoden maakt, u moet volgen de [stappen in dit artikel voor het configureren van connectiviteit](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).
> 
> 

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Verbinding maken met SQL Server in dezelfde cloudservice
Meerdere virtuele machines kunnen worden gemaakt in dezelfde cloudservice. Zie voor meer informatie over dit scenario virtuele machines, [verbinding maken tussen virtuele machines met een virtueel netwerk of cloudservice](../classic/connect-vms-classic.md#connect-vms-in-a-standalone-cloud-service). In dit scenario is wanneer een client op een virtuele machine verbinding maken met SQL Server die wordt uitgevoerd op een andere virtuele machine in dezelfde cloudservice.

In dit scenario kunt u verbinding maken met behulp van de virtuele machine **naam** (ook weergegeven als **computernaam** of **hostnaam** in de portal). Dit is de naam die u hebt opgegeven voor de virtuele machine tijdens het maken van. Bijvoorbeeld, als u de naam van uw SQL-VM **mysqlvm**, een VM-client in dezelfde cloudservice kan de volgende verbindingsreeks gebruiken om verbinding te maken:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Verbinding maken met SQL Server via Internet
Als u verbinding maken met uw SQL Server-database-engine van het Internet wilt, moet u het eindpunt van een virtuele machine voor de binnenkomende TCP-communicatie. Deze configuratiestap van Azure zorgt ervoor dat binnenkomend TCP-poortverkeer naar een TCP-poort wordt geleid die toegankelijk is voor de virtuele machine.

Als u wilt verbinden via het internet, moet u de DNS-naam van de virtuele machine en het poortnummer van de VM-eindpunt (geconfigureerd verderop in dit artikel). Als u de DNS-naam zoekt, gaat u naar de Azure-portal en selecteer **virtuele machines (klassiek)**. Selecteer vervolgens uw virtuele machine. De **DNS-naam** wordt weergegeven in de **overzicht** sectie.

Neem bijvoorbeeld een klassieke virtuele machine met de naam **mysqlvm** met een DNS-naam van **mysqlvm7777.cloudapp.net** en een VM-eindpunt van **57500**. Ervan uitgaande dat correct geconfigureerde connectiviteit, de volgende verbindingsreeks kan worden gebruikt voor toegang tot de virtuele machine vanaf elke locatie op internet:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Hoewel het hierdoor connectiviteit mogelijk voor clients via internet, betekent dit niet dat iedereen verbinding kan maken met uw SQL-Server. Buiten hebben clients op de juiste gebruikersnaam en wachtwoord. Voor extra beveiliging, gebruik niet de bekende poort 1433 voor het eindpunt openbare virtuele machine. En, indien mogelijk, overweegt u het toevoegen van een ACL op uw eindpunt te verkeer beperken tot de clients die u mogelijk maken. Zie voor instructies over het gebruik van ACL's met eindpunten [beheren de ACL voor een eindpunt](../classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).

> [!NOTE]
> Het is belangrijk te weten dat wanneer u deze techniek gebruiken om te communiceren met SQL Server, alle uitgaande gegevens van het Azure-datacenter is onderhevig aan normaal [prijzen voor uitgaande gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/).
> 
> 

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Verbinding maken met SQL Server in hetzelfde virtuele netwerk
[Virtueel netwerk](../../../virtual-network/virtual-networks-overview.md) aanvullende scenario's mogelijk maakt. U kunt virtuele machines in hetzelfde virtuele netwerk, verbinden, zelfs als deze virtuele machines die zijn opgenomen in andere cloud-services. En met een [site-naar-site VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), kunt u een hybride architectuur waarbij virtuele machines met on-premises netwerken en virtuele machines verbonden maken.

Virtuele netwerken kunt u uw Azure VM's toevoegen aan een domein. Dit is de enige manier om Windows-verificatie met SQL Server gebruiken. De andere verbinding-scenario's vereisen SQL-verificatie met gebruikersnamen en wachtwoorden.

Als u configureren van een domeinomgeving en Windows-verificatie wilt, hoeft u niet de stappen in dit artikel gebruiken om te configureren van het openbare eindpunt of de SQL-verificatie en -aanmeldingen. In dit scenario kunt u verbinding met uw SQL Server-exemplaar met de naam van de SQL Server-VM in de verbindingsreeks op te geven. Het volgende voorbeeld wordt ervan uitgegaan dat er ook Windows-verificatie is geconfigureerd en dat de gebruiker heeft toegang tot de SQL Server-exemplaar.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Stappen voor het configureren van SQL Server-connectiviteit in een Azure-VM
De volgende stappen laten zien hoe u verbinding maken met de SQL Server-exemplaar via internet met behulp van SQL Server Management Studio (SSMS). Dezelfde stappen zijn echter van toepassing op het maken van uw virtuele machine van SQL Server toegankelijk is voor uw toepassingen, zowel on-premises uitgevoerd en in Azure.

Voordat u verbinding met het exemplaar van SQL Server vanuit een andere virtuele machine of op het internet maken kunt, moet u de volgende taken uitvoeren zoals beschreven in de volgende secties:

* [Een TCP-eindpunt voor de virtuele machine maken](#create-a-tcp-endpoint-for-the-virtual-machine)
* [TCP-poorten openen in de Windows-firewall](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
* [Configureren van SQL Server om te luisteren op het TCP-protocol](#configure-sql-server-to-listen-on-the-tcp-protocol)
* [SQL Server configureren voor verificatie in gemengde modus](#configure-sql-server-for-mixed-mode-authentication)
* [Aanmeldingen voor SQL Server-verificatie maken](#create-sql-server-authentication-logins)
* [De DNS-naam van de virtuele machine vaststellen](#determine-the-dns-name-of-the-virtual-machine)
* [Verbinding maken met de Database-Engine vanaf een andere computer](#connect-to-the-database-engine-from-another-computer)

Het verbindingspad wordt samengevat in het volgende diagram:

![Verbinding maken met een SQL Server-machine](../../../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[!INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[!INCLUDE [Connect to SQL Server in a VM](../../../../includes/virtual-machines-sql-server-connection-steps.md)]

[!INCLUDE [Connect to SQL Server in a VM Classic Steps](../../../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Volgende stappen
Als u ook van plan bent te gebruiken van AlwaysOn-beschikbaarheidsgroepen voor hoge beschikbaarheid en herstel na noodgevallen, moet u rekening houden met het implementeren van een listener. Database-clients verbinding maken met de listener in plaats van rechtstreeks op een van de SQL Server-exemplaren. De listener routeert clients naar de primaire replica in de beschikbaarheidsgroep. Zie voor meer informatie, [een ILB-listener voor AlwaysOn-beschikbaarheidsgroepen configureren in Azure](../classic/ps-sql-int-listener.md).

Het is belangrijk om te controleren alle van de aanbevolen beveiligingsprocedures voor SQL Server die wordt uitgevoerd op een virtuele machine van Azure. Zie [Veiligheidsoverwegingen voor SQL Server in virtuele Azure-machines](../sql/virtual-machines-windows-sql-security.md) voor meer informatie.

[Bekijk het leertraject](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) voor virtuele SQL Server-machines in Azure. 

Zie voor andere onderwerpen met betrekking tot SQL Server wordt uitgevoerd in Azure VM's, [SQL Server op Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

