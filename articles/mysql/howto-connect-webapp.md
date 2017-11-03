---
title: Bestaande Azure App Service verbinden met Azure-Database voor MySQL | Microsoft Docs
description: Instructies voor het correct verbinding maken met een bestaande Azure App Service Azure Database voor MySQL
services: mysql
author: v-chenyh
ms.author: v-chenyh
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 0028b0c918b573f6884e6f63fa82dbe23079882a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="connect-an-existing-azure-app-service-to-azure-database-for-mysql-server"></a>Een bestaande Azure App Service verbinden met Azure-Database voor MySQL-server
Dit onderwerp wordt uitgelegd hoe u een bestaande Azure App Service verbinden met uw Azure-Database voor de MySQL-server.

## <a name="before-you-begin"></a>Voordat u begint
Meld u aan bij [Azure Portal](https://portal.azure.com). Maak een Azure-Database voor de MySQL-server. Raadpleeg voor meer informatie, [Azure-Database maken voor MySQL-server van de Portal](quickstart-create-mysql-server-database-using-azure-portal.md) of [Azure-Database maken voor MySQL-server met CLI](quickstart-create-mysql-server-database-using-azure-cli.md).

Er zijn momenteel twee oplossingen voor het inschakelen van een Azure App Service toegang heeft tot een Azure-Database voor MySQL. Beide oplossingen hebben betrekking op serverniveau firewallregels instellen.

## <a name="solution-1---create-a-firewall-rule-to-allow-all-ips"></a>1 - oplossing een firewallregel waarmee alle IP-adressen maken
Azure MySQL-Database biedt beveiliging van toegang tot uw gegevens beschermen met een firewall. Bij het verbinden van een Azure App Service met de Azure-Database voor de MySQL-server, houd er rekening mee dat de uitgaande IP-adressen van App Service dynamisch karakter. 

Om te controleren of de beschikbaarheid van uw Azure App Service, wordt u aangeraden deze oplossing waarmee alle IP-adressen.

> [!NOTE]
> Microsoft werkt aan een oplossing op lange termijn om te voorkomen dat alle IP-adressen voor Azure-services te maken met Azure Database MySQL toe.

1. Op de blade MySQL-server onder de instellingen voor de kop, klikt u op **verbindingsbeveiliging** de blade verbindingsbeveiliging voor Azure-Database voor MySQL openen.

   ![Azure-portal - Klik op de beveiliging van de verbinding](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Voer **REGELNAAM**, **eerste IP-**, en **END-IP**, en klik vervolgens op **opslaan**.
   - Regelnaam: toestaan-All-IP-adressen
   - Start-IP: 0.0.0.0
   - End-IP: 255.255.255.255

   ![Azure-portal - alle IP-adressen toevoegen](./media/howto-connect-webapp/1_2-add-all-ips.png)

## <a name="solution-2---create-a-firewall-rule-to-explicitly-allow-outbound-ips"></a>Oplossing 2: een firewallregel expliciet wilt toestaan uitgaande IP-adressen maken
U kunt alle de uitgaande IP-adressen van uw Azure App Service expliciet toevoegen.

1. Bekijk op de blade App Service-eigenschappen uw **uitgaande IP-adres**.

   ![Azure portal - weergave uitgaande IP-adressen](./media/howto-connect-webapp/2_1-outbound-ip-address.png)

2. Op de blade van de beveiliging MySQL-verbinding toevoegen uitgaande IP-adressen één voor één.

   ![Azure-portal - expliciete IP-adressen toevoegen](./media/howto-connect-webapp/2_2-add-explicit-ips.png)

3. Vergeet niet te **opslaan** uw firewallregels.

Hoewel Azure App service probeert om IP-adressen constante gedurende een bepaalde periode, zijn er ook gevallen waarbij de IP-adressen mag wijzigen. Bijvoorbeeld: dit kan gebeuren wanneer de app wordt gerecycled of een schaalaanpassing optreedt, of wanneer nieuwe computers worden toegevoegd in Azure regionale data centers om capaciteit te vergroten. Wanneer de IP-wijzigen adressen, krijgen de app kan uitvaltijd in het geval van deze niet langer verbinding kan maken met de MySQL-server. Houd rekening met deze overweging bij het kiezen van een van de voorgaande oplossingen.

## <a name="ssl-configuration"></a>SSL-configuratie
Azure MySQL-Database heeft SSL standaard ingeschakeld. Als uw toepassing geen van SSL gebruikmaakt verbinding maken met de database, moet u SSL op de MySQL-server uitschakelen. Zie voor meer informatie over het configureren van SSL [met behulp van SSL met Azure-Database voor MySQL](howto-configure-ssl.md).

## <a name="next-steps"></a>Volgende stappen
Raadpleeg voor meer informatie over verbindingsreeksen [verbindingsreeksen](howto-connection-string.md).
