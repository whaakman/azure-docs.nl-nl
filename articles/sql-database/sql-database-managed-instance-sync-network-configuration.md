---
title: Azure App Service - sync-netwerkconfiguratie | Microsoft Docs
description: Dit artikel wordt beschreven hoe u wilt synchroniseren van uw netwerkconfiguratie voor Azure App Service plan hosten.
ms.service: sql-database
author: srdjan-bozovic
manager: craigg
ms.custom: managed instance
ms.topic: article
ms.date: 03/07/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 5b139b1279776acfca63def25a9fdae0f627a727
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan"></a>Netwerkconfiguratie voor Azure App Service plan hosting synchroniseren

Het gebeuren dat hoewel u [geïntegreerd van uw app met een Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md), u kan geen verbinding maken met beheerde-exemplaar. Wat die u kunt proberen is netwerkconfiguratie voor uw service-abonnement te vernieuwen. 

## <a name="sync-network-configuration-for-app-service-hosting-plan"></a>Synchronisatie-netwerkconfiguratie voor App Service-hostingplan plannen

Voer hiervoor de volgende stappen uit:  

1. Ga naar uw web-apps App Service-abonnement.
 
   ![App Service-plan](./media/sql-database-managed-instance-sync-networking/app-service-plan.png)

2. Klik op **Networking** en klik vervolgens op **Klik hier om te beheren**.
 
   ![service-abonnement beheren](./media/sql-database-managed-instance-sync-networking/manage-plan.png)

3. Selecteer uw **VNet** en klik op **Sync netwerk**. 
 
   ![synchronisatie-netwerk](./media/sql-database-managed-instance-sync-networking/sync.png)

4. Wacht totdat de synchronisatie wordt uitgevoerd.
  
   ![synchronisatie is uitgevoerd](./media/sql-database-managed-instance-sync-networking/sync-done.png)

U bent nu klaar om te proberen om de verbinding met uw exemplaar beheerd te herstellen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het configureren van uw VNet voor beheerde exemplaar [beheerd exemplaar VNet-configuratie](sql-database-managed-instance-vnet-configuration.md).
