---
title: Maken en beheren van Azure-Database voor de service-eindpunten MySQL VNet en regels met de Azure portal | Microsoft Docs
description: Maken en beheren van Azure-Database voor de service-eindpunten MySQL VNet en regels met de Azure portal
services: mysql
author: mbolz
ms.author: mbolz
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/01/2018
ms.openlocfilehash: 7520868fd6bd349043ad2c53e62de5db978db8b1
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35267217"
---
# <a name="create-and-manage-azure-database-for-mysql-vnet-service-endpoints-and-vnet-rules-by-using-the-azure-portal"></a>Maken en beheren van Azure-Database voor de service-eindpunten MySQL VNet en regels voor VNet met behulp van de Azure-portal
Virtueel netwerk (VNet) services-eindpunten en de regels van de persoonlijke adresruimte van een virtueel netwerk uitbreiden naar uw Azure-Database voor de MySQL-server. Zie voor een overzicht van Azure-Database voor MySQL VNet service-eindpunten, met inbegrip van beperkingen, [Azure Database voor de service-eindpunten MySQL Server VNet](concepts-data-access-and-security-vnet.md). VNet-service-eindpunten zijn beschikbaar in public preview in alle ondersteunde regio's voor Azure-Database voor MySQL.

> [!NOTE]
> Ondersteuning voor VNet-service-eindpunten is alleen voor algemene doeleinden en geoptimaliseerd voor geheugen-servers.

## <a name="create-a-vnet-rule-and-enable-service-endpoints-in-the-azure-portal"></a>Een VNet-regel maken en inschakelen van de service-eindpunten in de Azure portal

1. Op de pagina MySQL-server onder de instellingen voor de kop, klikt u op **verbindingsbeveiliging** het deelvenster beveiliging van de verbinding voor de Azure-Database voor MySQL openen. Klik op **+ bestaand virtueel netwerk toe te voegen**. Als u geen bestaand VNet kunt u **+ nieuw virtueel netwerk maken** een maken. Zie [Snelstartgids: een virtueel netwerk maken met de Azure-portal](../virtual-network/quick-create-portal.md)

   ![Azure-portal - Klik op de beveiliging van de verbinding](./media/howto-manage-vnet-using-portal/1-connection-security.png)

2. Voer een naam in VNet regel, selecteer het abonnement, het virtuele netwerk en de naam van Subnet en klik op **inschakelen**. Hierdoor automatisch VNet service-eindpunten op het subnet met behulp van de **Microsoft.SQL** service-tag.

   ![Azure-portal - VNet configureren](./media/howto-manage-vnet-using-portal/2-configure-vnet.png)

   > [!IMPORTANT]
   > Het is raadzaam om te lezen van dit artikel over service-eindpuntconfiguraties en overwegingen voordat de service-eindpunten configureren. **Virtueel netwerk service-eindpunt:** A [Virtual Network service-eindpunt](../virtual-network/virtual-network-service-endpoints-overview.md) een subnet waarvan de eigenschapwaarden een of meer namen van formele Azure-service bevatten. VNet-services-eindpunten gebruiken de naam van de service type **Microsoft.Sql**, die verwijst naar de Azure-service met de naam SQL-Database. Deze servicetag geldt ook voor de Azure SQL Database, Azure-Database voor PostgreSQL en MySQL-services. Het is belangrijk te weten bij het toepassen van de **Microsoft.Sql** service-tag met de service-eindpunt van een VNet configureert het netwerkverkeer voor de service-eindpunt voor alle Azure-Database-services, met inbegrip van Azure SQL Database, Azure-Database voor PostgreSQL en Azure-Database voor de MySQL-servers op het subnet. 
   > 

3. Eenmaal is ingeschakeld, klikt u op **OK** en u ziet dat de service-eindpunten VNet samen met een VNet-regel zijn ingeschakeld.

   ![VNet-service-eindpunten is ingeschakeld en Vnnet-regel is gemaakt](./media/howto-manage-vnet-using-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png)

## <a name="next-steps"></a>Volgende stappen
- Op deze manier kunt script naar [VNet inschakelen service-eindpunten en maakt u een VNET-regel voor de Azure-Database voor MySQL met Azure CLI](howto-manage-vnet-using-cli.md).
- Voor hulp bij het verbinding maken met een Azure-Database voor de MySQL-server, Zie [verbindingsbibliotheken voor Azure-Database voor MySQL](./concepts-connection-libraries.md)
