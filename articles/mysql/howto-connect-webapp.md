---
title: Bestaande Azure App Service verbinding met Azure Database for MySQL
description: Instructies voor het correct verbinding van een bestaande Azure App Service met Azure Database for MySQL
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 09/26/2018
ms.openlocfilehash: eb2fee7c76bcf29aee2dcd70d7975d7631bb23f6
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548949"
---
# <a name="connect-an-existing-azure-app-service-to-azure-database-for-mysql-server"></a>Verbinding maken met een bestaande Azure App Service met Azure Database voor MySQL-server
In dit onderwerp wordt uitgelegd hoe u een bestaande Azure App Service verbinden met uw Azure Database voor MySQL-server.

## <a name="before-you-begin"></a>Voordat u begint
Meld u aan bij [Azure Portal](https://portal.azure.com). Maak een Azure Database for MySQL-server. Raadpleeg voor meer informatie, [over het maken van Azure Database voor MySQL-server uit de Portal](quickstart-create-mysql-server-database-using-azure-portal.md) of [over het maken van Azure Database voor MySQL-server met behulp van CLI](quickstart-create-mysql-server-database-using-azure-cli.md).

Er zijn momenteel twee oplossingen voor het inschakelen van toegang van een Azure App Service met een Azure Database voor MySQL. Beide oplossingen hebben betrekking op het instellen van firewallregels op serverniveau.

## <a name="solution-1---allow-azure-services"></a>Oplossing 1: Azure-services toestaan
Azure Database voor MySQL biedt beveiliging met behulp van een firewall om uw gegevens te beveiligen. Bij het verbinden van een Azure App Service met Azure Database voor MySQL-server, houd er rekening mee dat de uitgaande IP-adressen van App Service dergelijke relays dynamisch zijn. Als u de optie 'Toegang tot Azure-services toestaan' kiest, kunnen de appservice verbinden met de MySQL-server.

1. Op de blade van de MySQL-server onder de instellingen voor kop, klikt u op **verbindingsbeveiliging** de beveiliging van de verbinding om blade te openen voor Azure Database voor MySQL.

   ![Azure-portal: klik op de beveiliging van de verbinding](./media/howto-connect-webapp/1-connection-security.png)

2. Selecteer **ON** in **toegang tot Azure-services toestaan**, klikt u vervolgens **opslaan**.
   ![Azure portal - toegang tot Azure toestaan](./media/howto-connect-webapp/allow-azure.png)

## <a name="solution-2---create-a-firewall-rule-to-explicitly-allow-outbound-ips"></a>Oplossing 2: Maak een firewallregel op expliciet uitgaande IP-adressen toestaan
U kunt alle de uitgaande IP-adressen van uw Azure App Service expliciet toevoegen.

1. Op de blade App Service-eigenschappen weergeven uw **uitgaande IP-adres**.

   ![Azure portal - weergave uitgaande IP-adressen](./media/howto-connect-webapp/2_1-outbound-ip-address.png)

2. Op de blade MySQL-verbinding, moet u uitgaand IP-adressen één voor één toevoegen.

   ![Azure portal - expliciete IP-adressen toevoegen](./media/howto-connect-webapp/2_2-add-explicit-ips.png)

3. Houd er rekening mee te **opslaan** uw firewall-regels.

Hoewel de Azure App service probeert te houden IP-adressen constante na verloop van tijd, zijn er gevallen waarbij de IP-adressen kunnen worden gewijzigd. Dit kan bijvoorbeeld gebeuren wanneer de app wordt gerecycled of een schaalbewerking optreedt, of wanneer nieuwe computers worden toegevoegd aan Azure regionale data centers capaciteit wilt vergroten. Als de IP-wijzigen adressen, kan de app tijdelijk zijn in het geval dat deze niet langer verbinding kan maken met de MySQL-server. Houd er rekening mee met deze overweging bij het kiezen van een van de voorgaande oplossingen.

## <a name="ssl-configuration"></a>SSL-configuratie
Azure Database voor MySQL is SSL standaard ingeschakeld. Als uw toepassing geen van SSL gebruikmaakt verbinding maakt met de database, moet u SSL op de MySQL-server uitschakelen. Zie voor meer informatie over het configureren van SSL [met behulp van SSL met Azure Database for MySQL](howto-configure-ssl.md).

## <a name="next-steps"></a>Volgende stappen
Raadpleeg voor meer informatie over verbindingsreeksen [verbindingsreeksen](howto-connection-string.md).
