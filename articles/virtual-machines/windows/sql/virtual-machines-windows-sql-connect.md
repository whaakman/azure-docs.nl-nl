---
title: Verbinding maken met een SQL Server-Machine (Resource Manager) | Microsoft Docs
description: Leer hoe u verbinding maken met SQL Server die wordt uitgevoerd op een virtuele Machine in Azure. In dit onderwerp wordt gebruikgemaakt van het klassieke implementatiemodel. De scenario's variëren, afhankelijk van de configuratie van netwerken en de locatie van de client.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/12/2017
ms.author: jroth
ms.openlocfilehash: 522ece2528e43c1037dc6bb707201ecda8074dd9
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38705959"
---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure"></a>Verbinding maken met een SQL Server-Machine in Azure

## <a name="overview"></a>Overzicht

In dit onderwerp wordt beschreven hoe u verbinding maken met uw exemplaar van SQL Server die wordt uitgevoerd op een virtuele machine van Azure. Deze staan enkele [algemene connectiviteitsscenario's](#connection-scenarios) en geeft vervolgens [stappen in de portal voor het wijzigen van instellingen voor netwerkconnectiviteit](#change). Als u wilt oplossen of connectiviteit buiten de portal configureren, Zie de [handmatige configuratie](#manual) aan het einde van dit onderwerp. 

Als u liever een volledig overzicht van de inrichting en connectiviteit, raadpleegt u [inrichten van een SQL Server-Machine in Azure](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Scenario 's

De manier waarop die een client verbinding maakt met SQL Server op een virtuele Machine wordt uitgevoerd, is afhankelijk van de locatie van de client en de netwerkconfiguratie.

Als u een SQL Server-VM in Azure portal inricht, hebt u de optie voor het opgeven van het type **SQL-verbinding**.

![Openbare SQL connectiviteitsoptie tijdens het inrichten](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Uw opties voor connectiviteit zijn onder andere:

| Optie | Beschrijving |
|---|---|
| **Openbare** | Verbinding maken met SQL Server via internet |
| **Persoonlijke** | Verbinding maken met SQL Server in hetzelfde virtuele netwerk |
| **lokale** | Verbinding maken met SQL Server lokaal op dezelfde virtuele machine | 

De volgende secties worden de **openbare** en **persoonlijke** opties in meer detail.

## <a name="connect-to-sql-server-over-the-internet"></a>Verbinding maken met SQL Server via Internet

Als u verbinding maken met uw SQL Server-database-engine van het Internet wilt, selecteert u **openbare** voor de **SQL-verbinding** type in de portal tijdens het inrichten. De portal wordt automatisch de volgende stappen uit:

* Hiermee kunt het TCP/IP-protocol voor SQL Server.
* Hiermee configureert u een firewallregel voor het openen van de SQL Server-TCP-poort (standaard 1433).
* Hiermee kunt SQL Server-verificatie, die vereist zijn voor openbare toegang.
* Hiermee configureert u de netwerkbeveiligingsgroep op de virtuele machine voor alle TCP-verkeer op de SQL Server-poort.

> [!IMPORTANT]
> De installatiekopieën van virtuele machines voor de SQL Server Developer en Express-edities het TCP/IP-protocol niet automatisch ingeschakeld. Voor Developer en Express editions, moet u SQL Server Configuration Manager te gebruiken [handmatig het TCP/IP-protocol ingeschakeld](#manualtcp) na het maken van de virtuele machine.

Een client met toegang tot het internet worden verbonden met het SQL Server-exemplaar door ofwel het openbare IP-adres van de virtuele machine of een DNS-label dat is toegewezen aan dat IP-adres op te geven. Als de SQL Server-poort 1433, hoeft u niet opgeven in de verbindingsreeks. De volgende verbindingsreeks verbinding maakt met een SQL-VM met een DNS-label van `sqlvmlabel.eastus.cloudapp.azure.com` met behulp van SQL-verificatie (u kunt ook het openbare IP-adres).

```
Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>
```

Hoewel het hierdoor connectiviteit mogelijk voor clients via internet, betekent dit niet dat iedereen verbinding kan maken met uw SQL-Server. Buiten hebben clients op de juiste gebruikersnaam en wachtwoord. Voor extra beveiliging kunt kunt u de bekende poort 1433 voorkomen. Bijvoorbeeld, als u SQL Server om te luisteren op poort 1500 en tot stand gebrachte correcte firewall- en regels voor netwerkbeveiligingsgroepen hebt geconfigureerd, u kan verbinding maken met het poortnummer toe te voegen aan de naam van de Server. Het volgende voorbeeld wijzigt u de voorgaande build is door het toevoegen van een aangepast poortnummer **1500**, aan de servernaam:

```
Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"
```

> [!NOTE]
> Wanneer u een query uitvoeren van SQL Server in een virtuele machine via internet, alle uitgaande gegevens van het Azure-datacenter is onderworpen aan normaal [prijzen voor uitgaande gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="connect-to-sql-server-within-a-virtual-network"></a>Verbinding maken met SQL Server binnen een virtueel netwerk

Als u ervoor kiest **persoonlijke** voor de **SQL-verbinding** type in de Azure-portal configureert u de instellingen die identiek is aan de meeste **openbare**. Een verschil is dat er geen regel voor netwerkbeveiligingsgroep om toe te staan buiten verkeer op de SQL Server-poort (standaard 1433).

> [!IMPORTANT]
> De installatiekopieën van virtuele machines voor de SQL Server Developer en Express-edities het TCP/IP-protocol niet automatisch ingeschakeld. Voor Developer en Express editions, moet u SQL Server Configuration Manager te gebruiken [handmatig het TCP/IP-protocol ingeschakeld](#manualtcp) na het maken van de virtuele machine.

Particuliere connectiviteit wordt vaak gebruikt in combinatie met [Virtueelnetwerk](../../../virtual-network/virtual-networks-overview.md), waardoor verschillende scenario's. U kunt virtuele machines in hetzelfde virtuele netwerk, verbinden, zelfs als deze virtuele machines aanwezig zijn in verschillende resourcegroepen bevinden. En met een [site-naar-site VPN](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), kunt u een hybride architectuur waarbij virtuele machines met on-premises netwerken en virtuele machines verbonden maken.

Virtuele netwerken kunnen u uw Azure VM's toevoegen aan een domein. Dit is de enige manier om Windows-verificatie met SQL Server gebruiken. De andere verbinding-scenario's vereisen SQL-verificatie met gebruikersnamen en wachtwoorden.

Ervan uitgaande dat u DNS in het virtuele netwerk hebt geconfigureerd, kunt u verbinding met uw SQL Server-exemplaar met de naam van de virtuele machine met SQL Server-computer in de verbindingsreeks op te geven. Het volgende voorbeeld wordt ook van uitgegaan dat er ook Windows-verificatie is geconfigureerd en dat de gebruiker heeft toegang tot de SQL Server-exemplaar.

```
Server=mysqlvm;Integrated Security=true
```

## <a id="change"></a> SQL-verbindingsinstellingen wijzigen

U kunt de connectiviteitsinstellingen voor uw virtuele machine van SQL Server in Azure portal.

1. Selecteer in de Azure portal, **virtuele Machines**.

2. Selecteer uw SQL Server-machine.

3. Onder **instellingen**, klikt u op **SQL Server-configuratie**.

4. Wijzig de **SQL-verbindingsniveau** naar de vereiste instelling. U kunt dit gebied (optioneel) gebruiken om de SQL Server-poort of de SQL-verificatie-instellingen te wijzigen.

   ![Wijzigen van de SQL-verbinding](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity-change.png)

5. Wacht enkele minuten voor de update te voltooien.

   ![Updatemelding voor SQL-VM](./media/virtual-machines-windows-sql-connect/sql-vm-updating-notification.png)

## <a id="manualtcp"></a> Schakel TCP/IP in voor de Developer en Express editions

Bij het wijzigen van instellingen voor SQL Server-verbindingen, Azure niet automatisch ingeschakeld het TCP/IP-protocol voor SQL Server Developer en Express-edities. In de onderstaande stappen wordt uitgelegd hoe u TCP/IP handmatig kunt inschakelen, zodat u op afstand via een IP-adres verbinding kunt maken.

Eerst verbinding maken met de SQL Server-machine met extern bureaublad.

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Schakel vervolgens het TCP/IP-protocol met **SQL Server Configuration Manager**.

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-with-ssms"></a>Verbinden met SSMS

De volgende stappen laten zien hoe een optioneel DNS-Label maken voor uw Azure-VM en maak verbinding met SQL Server Management Studio (SSMS).

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a id="manual"></a> Handmatige configuratie en probleemoplossing

Hoewel de portal opties biedt voor het automatisch verbinding te configureren, is het handig om te weten hoe u handmatig configureert connectiviteit. Informatie over de vereisten kan ook helpen oplossen van problemen.

De volgende tabel bevat de vereisten voor het verbinding maken met SQL Server in een Azure-VM wordt uitgevoerd.

| Vereiste | Beschrijving |
|---|---|
| [Modus van SQL Server-verificatie inschakelen](https://docs.microsoft.com/sql/database-engine/configure-windows/change-server-authentication-mode#SSMSProcedure) | SQL Server-verificatie is vereist voor het verbinding maken met de virtuele machine op afstand, tenzij u Active Directory op een Virtueelnetwerk hebt geconfigureerd. |
| [Maken van een SQL-aanmelding](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/create-a-login) | Als u SQL-verificatie gebruikt, moet u een SQL-aanmelding met een gebruikersnaam en wachtwoord die ook beschikt over machtigingen voor de doeldatabase. |
| [TCP/IP-protocol inschakelen](#manualTCP) | SQL Server moet verbindingen toestaan via TCP. |
| [Firewall-regel voor de SQL Server-poort inschakelen](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) | De firewall op de virtuele machine moet binnenkomend verkeer op de SQL Server-poort (standaard 1433) toestaan. |
| [Een regel voor netwerkbeveiligingsgroep maken voor TCP 1433](../../../virtual-network/manage-network-security-group.md#create-a-security-rule) | U moet de virtuele machine voor het ontvangen van verkeer op de SQL Server-poort (standaard 1433) als u wilt verbinding maken via internet toestaan. Lokale en virtuele-netwerk-only-verbindingen hebben deze niet nodig. Dit is de enige stap in de Azure-portal. |

> [!TIP]
> De stappen in de tabel hierboven worden uitgevoerd voor u wanneer u verbinding in de portal configureert. Deze stappen om uw configuratie te bevestigen of om in te stellen verbinding handmatig alleen gebruiken voor SQL Server.

## <a name="next-steps"></a>Volgende stappen

Zie voor instructies, samen met stappen van de verbinding wordt ingericht [inrichten van een SQL Server-Machine in Azure](virtual-machines-windows-portal-sql-server-provision.md).

Zie voor andere onderwerpen met betrekking tot SQL Server wordt uitgevoerd in Azure VM's, [SQL Server op Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).