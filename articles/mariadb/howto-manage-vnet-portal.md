---
title: Maken en beheren van Azure Database voor MariaDB-VNet-service-eindpunten en regels met behulp van de Azure portal | Microsoft Docs
description: Maken en beheren van Azure Database voor MariaDB-VNet-service-eindpunten en regels met behulp van de Azure portal
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 11/19/2018
ms.openlocfilehash: 078c4253f990e3ad2759afb9f5b55832c78d9e73
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334088"
---
# <a name="create-and-manage-azure-database-for-mariadb-vnet-service-endpoints-and-vnet-rules-by-using-the-azure-portal"></a>Maken en beheren van Azure Database voor MariaDB-VNet-service-eindpunten en VNet-regels met behulp van Azure portal

> [!IMPORTANT]
> De VNet-service-eindpunten-functie is beschikbaar als openbare preview.

Virtueel netwerk (VNet) services-eindpunten en regels uitbreiden privé-adresruimte van een Virtueelnetwerk met uw Azure Database voor MariaDB-server. Zie voor een overzicht van Azure Database voor MariaDB-VNet-service-eindpunten, met inbegrip van beperkingen, [Azure Database voor MariaDB Server VNet service-eindpunten](concepts-data-access-security-vnet.md). VNet-service-eindpunten zijn beschikbaar in alle ondersteunde regio's voor Azure Database voor MariaDB.

> [!NOTE]
> Ondersteuning voor VNet-service-eindpunten is alleen voor algemeen gebruik en geoptimaliseerd voor geheugen-servers.

## <a name="create-a-vnet-rule-and-enable-service-endpoints"></a>Een VNet-regel maken en service-eindpunten inschakelen

1. Op de pagina MariaDB-server onder de instellingen voor kop, klikt u op **verbindingsbeveiliging** de beveiliging van de verbinding om deelvenster te openen voor Azure Database voor MariaDB. Klik op **+ bestaand virtueel netwerk toevoegen**. Als u een bestaand VNet niet hebt, kunt u **+ nieuw virtueel netwerk maken** een te maken. Zie [Snelstartgids: een virtueel netwerk met behulp van de Azure portal maken](../virtual-network/quick-create-portal.md)

   ![Azure-portal: klik op de beveiliging van de verbinding](./media/howto-manage-vnet-portal/1-connection-security.png)

2. Voer de naam van een VNet-regel, selecteer het abonnement, virtueel netwerk en subnetnaam en klik vervolgens op **inschakelen**. Hierdoor automatisch VNet-service-eindpunten op het subnet met behulp van de **Microsoft.SQL** servicetag.

   ![Azure portal - VNet configureren](./media/howto-manage-vnet-portal/2-configure-vnet.png)

   Het account moet de vereiste machtigingen om het maken van een virtueel netwerk en de service-eindpunt hebben.

   Service-eindpunten kunnen afzonderlijk worden geconfigureerd op de virtuele netwerken, door een gebruiker met schrijftoegang tot het virtuele netwerk.
    
   Voor het Azure-serviceresources aan een VNet, moet de gebruiker machtiging voor 'Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/' voor de subnetten die worden toegevoegd. Deze machtiging is standaard opgenomen in de ingebouwde service-beheerdersrollen en kan worden gewijzigd door aangepaste rollen te maken.
    
   Meer informatie over [ingebouwde rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) en het toewijzen van specifieke machtigingen voor [aangepaste rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).
    
   VNets en Azure-serviceresources kunnen in hetzelfde abonnement of in verschillende abonnementen zitten. Als het VNet en Azure-serviceresources in verschillende abonnementen zitten, moeten de resources onder dezelfde Active Directory (AD)-tenant.

   > [!IMPORTANT]
   > Het is raadzaam om te lezen in dit artikel over de service-eindpunt configuraties en overwegingen voor het configureren van service-eindpunten. **Service-eindpunt voor virtueel netwerk:** A [service-eindpunt voor Virtueelnetwerk](../virtual-network/virtual-network-service-endpoints-overview.md) is een subnet met eigenschappen die een of meer formele Azure-service typenamen bevatten. VNet-services-eindpunten gebruikt u de naam van de service type **Microsoft.Sql**, die verwijst naar de Azure-service met de naam SQL-Database. Deze servicetag geldt ook voor de Azure SQL Database, Azure Database voor MariaDB, PostgreSQL en MySQL-services. Het is belangrijk te weten bij het toepassen van de **Microsoft.Sql** servicetag naar een VNet-service-eindpunt configureert het verkeer van de service-eindpunt voor alle Azure-Database-services, met inbegrip van Azure SQL Database, Azure Database for PostgreSQL, Azure Database voor MariaDB en Azure Database for MySQL-servers op het subnet.
   > 

3. Eenmaal is ingeschakeld, klikt u op **OK** en u ziet dat de VNet-service-eindpunten zijn ingeschakeld, samen met een VNet-regel.

   ![VNet-service-eindpunten ingeschakeld en VNet-regel gemaakt](./media/howto-manage-vnet-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png)

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [SSL configureren op Azure Database voor MariaDB](howto-configure-ssl.md)
- Op dezelfde manier, u kunt een script op [inschakelen VNet service-eindpunten en een VNET-regel maken voor Azure Database voor MariaDB met behulp van Azure CLI](howto-manage-vnet-cli.md).