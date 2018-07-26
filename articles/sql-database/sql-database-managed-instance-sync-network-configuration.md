---
title: Azure App Service - netwerkconfiguratie synchroniseren | Microsoft Docs
description: In dit artikel wordt beschreven hoe u uw netwerkconfiguratie voor Azure App Service-hostingabonnement synchroniseren.
ms.service: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.custom: managed instance
ms.topic: conceptual
ms.date: 03/07/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 57c4dd523a5dffc48a2d2d403d2a440a8d6cde95
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257887"
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan"></a>Configuratie van netwerken voor Azure App Service-hostingabonnement synchroniseren

Het gebeuren dat hoewel u [geïntegreerd van uw app met een Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md), u kunt geen verbinding maken met Managed Instance. Wat die kunt u proberen is om te vernieuwen voor de configuratie van netwerken voor uw service-plan. 

## <a name="sync-network-configuration-for-app-service-hosting-plan"></a>Netwerkconfiguratie synchroniseren voor App Service-hostingplan

Voer hiervoor de volgende stappen uit:  

1. Ga naar uw web-apps met App Service-plan.
 
   ![App Service-plan](./media/sql-database-managed-instance-sync-networking/app-service-plan.png)

2. Klik op **netwerken** en klik vervolgens op **Klik hier om te beheren**.
 
   ![beheren van service-plan](./media/sql-database-managed-instance-sync-networking/manage-plan.png)

3. Selecteer uw **VNet** en klikt u op **netwerk synchroniseren**. 
 
   ![netwerk synchroniseren](./media/sql-database-managed-instance-sync-networking/sync.png)

4. Wacht totdat de synchronisatie is voltooid.
  
   ![synchronisatie is uitgevoerd](./media/sql-database-managed-instance-sync-networking/sync-done.png)

U bent nu klaar om te proberen voor uw verbinding met uw beheerde exemplaar opnieuw tot stand te brengen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het configureren van uw VNet voor beheerd exemplaar [beheerd exemplaar VNet-configuratie](sql-database-managed-instance-vnet-configuration.md).
