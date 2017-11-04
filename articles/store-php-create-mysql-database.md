---
title: Een MySQL-database in Azure maken en verbinden
description: Informatie over het gebruik van de Azure-portal een MySQL-database maken en vervolgens verbinding met deze van een PHP-web-app in Azure.
documentationcenter: php
services: app-service\web
author: cephalin
manager: erikre
editor: 
tags: mysql
ms.assetid: 55465a9a-7e65-4fd9-8a65-dd83ee41f3e5
ms.service: multiple
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;cephalin
ms.openlocfilehash: c072cb3a7d376d1e3c2b9f741f5410106e701256
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Een MySQL-database in Azure maken en verbinden
Deze zelfstudie ziet u hoe u een MySQL-database in de [Azure-portal](https://portal.azure.com) (provider [ClearDB](http://www.cleardb.com/)) en hoe u verbinding kunnen maken van een PHP-web-app uitgevoerd in de [Azure App Service](app-service/app-service-web-overview.md).

> [!NOTE]
> U kunt ook een MySQL-database maken als onderdeel van een <a href="https://portal.azure.com/#create/WordPress.WordPress" target="_blank">Marketplace-app-sjabloon</a>.
>
>

## <a name="create-a-mysql-database-in-azure-portal"></a>Een MySQL-database maken in Azure portal
Voor het maken van een MySQL-database in de Azure portal, het volgende doen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het menu links op **nieuw** > **gegevens en opslag** > **MySQL-Database**.

    ![Een MySQL-database maken in Azure - starten](./media/store-php-create-mysql-database/create-db-1-start.png)
3. In de nieuwe MySQL-Database [blade](azure-portal-overview.md), als volgt configureren van uw nieuwe MySQL-database (*blade*: een portal-pagina dat wordt geopend horizontaal):

   * **Databasenaam**: Typ een naam uniek identificeerbaar
   * **Abonnement**: Kies het abonnement te gebruiken
   * **Type van de database**: Selecteer **gedeelde** voor lage kosten of gratis lagen, of **toegewezen** ophalen van toegewijde bronnen.
   * **Resourcegroep**: de MySQL-database toevoegen aan een bestaande [resourcegroep](azure-resource-manager/resource-group-overview.md) of in een nieuwe plaatsen. Resources in dezelfde groep kunnen eenvoudig worden beheerd samen.
   * **Locatie**: Selecteer een locatie dicht bij u. Als u deze toevoegt aan een bestaande resourcegroep, bent u vergrendeld op de locatie van de resourcegroep.
   * **Prijscategorie**: klik op **prijscategorie**, selecteert u vervolgens een optie prijscategorie (**Mercury** is de laag gratis), en klik vervolgens op **selecteren**.
   * **Juridische voorwaarden**: klik op **juridische voorwaarden**, bekijk de details en klik op **kopen**.
   * **Vastmaken aan dashboard**: Selecteer deze optie als u wilt toegang hebt tot het rechtstreeks vanuit het dashboard. Dit is vooral handig als u niet bekend bent met de navigatie in de portal nog.

     De volgende schermafbeelding is slechts een voorbeeld van hoe u uw MySQL-database kunt configureren.  
     ![Een MySQL-database maken in Azure - configureren](./media/store-php-create-mysql-database/create-db-2-configure.png)
4. Wanneer u klaar bent configureren, klikt u op **maken**.

    ![Een MySQL-database maken in Azure - maken](./media/store-php-create-mysql-database/create-db-3-create.png)

    Hier ziet u een pop-upbericht waarin wordt aangegeven dat de implementatie is gestart.

    ![Maken van een MySQL-database in Azure - uitgevoerd](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Krijgt u een andere pop-up zodra de implementatie is geslaagd. De portal wordt de blade van uw MySQL-database ook automatisch geopend.

<a name="connect"></a>

## <a name="connect-to-your-mysql-database"></a>Verbinding maken met uw MySQL-database
Overzicht van de verbindingsgegevens voor de nieuwe MySQL-database, klikt u op **eigenschappen** in uw web-app-blade.

![Een MySQL-database maken in Azure - blade MySQL-database](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

U kunt nu verbindingsinformatie in een web-app. Een voorbeeld waarin het gebruik van de gegevens van een eenvoudige PHP-app is beschikbaar [hier](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie de [PHP-ontwikkelaarscentrum](/develop/php/).
