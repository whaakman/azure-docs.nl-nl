---
title: Verbinding maken met een virtuele Machine van SQL Server (Resource Manager) | Microsoft Docs
description: Informatie over het verbinding maken met SQL Server wordt uitgevoerd op een virtuele Machine in Azure. In dit onderwerp maakt gebruik van het klassieke implementatiemodel. De scenario's, is afhankelijk van de netwerkconfiguratie en de locatie van de client.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/14/2017
ms.author: jroth
ms.openlocfilehash: 67ba43f9456bbeffbf602067586143c4c68af672
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a>Verbinding maken met een virtuele SQL Server-machine op Azure (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-connect.md)
> * [Klassiek](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Overzicht

Dit onderwerp wordt beschreven hoe u verbinding maken met uw SQL Server-exemplaar dat is uitgevoerd op een virtuele machine van Azure. Dit omvat een aantal [algemene verbindingsproblemen scenario's](#connection-scenarios) en geeft vervolgens [gedetailleerde stappen voor het configureren van SQL-serververbindingen in een Azure VM](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Als u liever een volledig overzicht van beide, inrichten en connectiviteit, Zie [inrichten van een virtuele Machine van SQL Server op Azure](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Scenario 's

De manier waarop die een client verbinding maakt met SQL Server op een virtuele Machine wordt uitgevoerd, is afhankelijk van de locatie van de client en de netwerkconfiguratie.

Als u een virtuele SQL Server-machine in de Azure portal inricht, hebt u de optie voor het opgeven van het type **SQL-connectiviteit**.

![Openbare SQL verbindingsoptie tijdens het inrichten](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Uw opties voor connectiviteit zijn onder andere:

| Optie | Beschrijving |
|---|---|
| **Openbare** | Via internet verbinding maken met SQL Server |
| **Persoonlijke** | Verbinding maken met SQL Server in hetzelfde virtuele netwerk |
| **Lokale** | Verbinding maken met SQL Server lokaal op dezelfde virtuele machine | 

De volgende secties worden de **openbare** en **persoonlijke** opties in meer detail.

## <a name="connect-to-sql-server-over-the-internet"></a>Verbinding maken met SQL Server via Internet

Als u verbinding maken met uw SQL Server database-engine van het Internet wilt, selecteert u **openbare** voor de **SQL-connectiviteit** type in de portal tijdens het inrichten. De portal wordt automatisch de volgende stappen uit:

* Hiermee kunt het TCP/IP-protocol voor SQL Server.
* Hiermee configureert u een firewallregel in voor het openen van de SQL Server-TCP-poort (standaard 1433).
* Hiermee kunt SQL Server-verificatie vereist is voor openbare toegang.
* Hiermee configureert u de netwerkbeveiligingsgroep op de virtuele machine voor alle TCP-verkeer op de SQL Server-poort.

> [!IMPORTANT]
> Installatiekopieën van virtuele machines voor de SQL Server-ontwikkelaars en Express-edities het TCP/IP-protocol niet automatisch ingeschakeld. Voor ontwikkelaars en Express-edities, moet u SQL Server Configuration Manager te gebruiken [handmatig het TCP/IP-protocol inschakelen](#manualtcp) na het maken van de virtuele machine.

Een client met internettoegang kan verbinding maken met SQL Server-exemplaar door ofwel het openbare IP-adres van de virtuele machine of een DNS-label toegewezen aan dat IP-adres te geven. Als de SQL Server-poort 1433, hoeft u niet opgeven in de verbindingsreeks. De volgende verbindingsreeks verbinding maakt met een SQL-VM met een DNS-label `sqlvmlabel.eastus.cloudapp.azure.com` via SQL-verificatie (u kunt ook het openbare IP-adres).

```
Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>
```

Hoewel dit wordt de connectiviteit mogelijk voor clients via internet, betekent dit niet dat iedereen verbinding kan maken met uw SQL-Server. Buiten hebben clients op de juiste gebruikersnaam en wachtwoord. Voor extra beveiliging kunt u echter de standaardpoort 1433 voorkomen. Bijvoorbeeld, als u SQL Server om te luisteren op poort 1500 en tot stand gebrachte correcte firewall- en netwerkbeveiligingsgroepen hebt geconfigureerd, kan u door het poortnummer op de naam van de Server toe te voegen. Het volgende voorbeeld wijzigt u de vorige taak door een aangepast poortnummer toe te voegen **1500**, naar de server:

```
Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"
```

> [!NOTE]
> Als u een query's van SQL Server op een virtuele machine via het internet, alle uitgaande gegevens van de Azure-datacenter is onderworpen aan normaal [prijzen voor uitgaande gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="connect-to-sql-server-within-a-virtual-network"></a>Binnen een virtueel netwerk verbinding maken met SQL Server

Wanneer u de optie **persoonlijke** voor de **SQL-connectiviteit** type in de Azure-portal configureert u de instellingen die identiek is aan de meeste **openbare**. Een verschil is dat er geen groep van de netwerkbeveiligingsregel om toe te staan buiten verkeer op de SQL Server-poort (standaard 1433).

> [!IMPORTANT]
> Installatiekopieën van virtuele machines voor de SQL Server-ontwikkelaars en Express-edities het TCP/IP-protocol niet automatisch ingeschakeld. Voor ontwikkelaars en Express-edities, moet u SQL Server Configuration Manager te gebruiken [handmatig het TCP/IP-protocol inschakelen](#manualtcp) na het maken van de virtuele machine.

Particuliere connectiviteit wordt vaak gebruikt in combinatie met [virtueel netwerk](../../../virtual-network/virtual-networks-overview.md), waardoor verschillende scenario's. Zelfs als deze virtuele machines bestaan in verschillende resourcegroepen, kunt u virtuele machines in hetzelfde virtuele netwerk. En met een [site-naar-site VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), kunt u een hybride-architectuur die virtuele machines met on-premises netwerken en machines verbindt.

Virtuele netwerken kunnen u uw Azure VM's toevoegen aan een domein. Dit is de enige manier om Windows-verificatie met SQL Server gebruiken. De andere scenario's vereisen SQL-verificatie met gebruikersnamen en wachtwoorden.

Ervan uitgaande dat u DNS in het virtuele netwerk hebt geconfigureerd, kunt u verbinding met uw exemplaar van SQL Server door op te geven van de naam van de virtuele machine van SQL Server-computer in de verbindingstekenreeks. Het volgende voorbeeld wordt ervan uitgegaan dat ook Windows-verificatie is geconfigureerd en dat de gebruiker toegang tot de SQL Server-exemplaar gekregen heeft.

```
Server=mysqlvm;Integrated Security=true
```

## <a id="change"></a>SQL-connectiviteitsinstellingen wijzigen

U kunt de connectiviteitsinstellingen wijzigen voor uw virtuele machine van SQL Server in de Azure portal.

1. Selecteer in de Azure-portal **virtuele Machines**.

2. Selecteer uw SQL Server VM.

3. Onder **instellingen**, klikt u op **SQL Server-configuratiebestand**.

4. Wijzig de **niveau van de SQL-connectiviteit** naar de vereiste instelling. Desgewenst kunt u dit gebied kunt u de SQL Server-poort of de SQL-verificatie-instellingen wijzigen.

   ![Wijzigen van de SQL-connectiviteit](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity-change.png)

5. Wacht enkele minuten voor de update te voltooien.

   ![Updatebericht SQL VM](./media/virtual-machines-windows-sql-connect/sql-vm-updating-notification.png)

## <a id="manualtcp"></a>TCP/IP inschakelen voor ontwikkelaars en Express-edities

Bij het wijzigen van SQL Server-verbindingsinstellingen wordt Azure niet automatisch ingeschakeld het TCP/IP-protocol voor SQL Server-ontwikkelaars en Express-edities. In de onderstaande stappen wordt uitgelegd hoe u TCP/IP handmatig kunt inschakelen, zodat u op afstand via een IP-adres verbinding kunt maken.

Eerst verbinding met de SQL Server-machine via Extern bureaublad.

> [!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Schakel vervolgens het TCP/IP-protocol **SQL Server Configuration Manager**.

> [!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-with-ssms"></a>Verbinden met SSMS

De volgende stappen laten zien hoe een optionele DNS-Label maken voor uw virtuele machine van Azure en maak verbinding met SQL Server Management Studio (SSMS).

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Volgende stappen

Zie voor instructies samen met deze stappen connectiviteit inrichting [inrichten van een virtuele Machine van SQL Server op Azure](virtual-machines-windows-portal-sql-server-provision.md).

Zie voor andere onderwerpen die betrekking hebben op SQL Server wordt uitgevoerd in Azure VM's [SQL Server op Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).