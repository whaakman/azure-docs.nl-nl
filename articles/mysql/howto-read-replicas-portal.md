---
title: Maken en beheren van lezen-replica's in Azure Database for MySQL
description: Dit artikel wordt beschreven hoe u kunt instellen en lezen-replica's in Azure Database voor MySQL met behulp van de portal beheren.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 01/23/2019
ms.openlocfilehash: c7622252cd5b375e8c580bfcf7a45806d219d828
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54900558"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mysql-using-the-azure-portal"></a>Over het maken en beheren lezen-replica's in Azure Database for MySQL met behulp van de Azure portal


> [!IMPORTANT]
> De functie lezen replica is beschikbaar als openbare preview.

In dit artikel leert u hoe u kunt maken en beheren van meer replica's binnen dezelfde Azure-regio als het model in de Azure Database for MySQL-service met behulp van de Azure portal. De functie is momenteel in openbare preview.

## <a name="prerequisites"></a>Vereisten

- Een [Azure Database for MySQL-server](quickstart-create-mysql-server-database-using-azure-portal.md) die wordt gebruikt als de hoofd-server.

> [!IMPORTANT]
> De functie lezen replica is alleen beschikbaar voor Azure Database voor MySQL-servers in de Prijscategorieën voor algemeen gebruik of geoptimaliseerd voor geheugen. Controleer of de hoofd-server in een van deze Prijscategorieën.

## <a name="create-a-read-replica"></a>Maken van een replica lezen

Een lezen-replica-server kan worden gemaakt met behulp van de volgende stappen uit:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).

2. Selecteer de bestaande Azure Database for MySQL-server die u wilt gebruiken als een master. Deze actie opent u de **overzicht** pagina.

3. Selecteer **replicatie** in het menu onder **instellingen**.

4. Selecteer **-Replica toevoegen**.

   ![Azure Database voor MySQL - replicatie ](./media/howto-read-replica-portal/add-replica.png)

5. Voer een naam voor de replica-server en klikt u op **OK** om te bevestigen van het maken van de replica.

   ![Azure Database voor MySQL - replica maken ](./media/howto-read-replica-portal/create-replica.png)

> [!NOTE]
> Lezen-replica's worden gemaakt met de configuratie van de dezelfde server als de master. De configuratie van de replica-server kan worden gewijzigd nadat deze is gemaakt. Het wordt aanbevolen dat de configuratie van de replica-server moet worden opgeslagen op de waarden gelijk zijn aan of groter zijn dan het model om te controleren of dat de replica kan houden met de master.

Nadat de replica-server is gemaakt, kan het worden bekeken in de **replicatie** blade.

   ![Azure Database voor MySQL - lijst met replica 's ](./media/howto-read-replica-portal/list-replica.png)

## <a name="stop-replication-to-a-replica-server"></a>Replicatie naar een replica-server stoppen

> [!IMPORTANT]
> Replicatie naar een server stoppen is niet ongedaan worden gemaakt. Wanneer u replicatie tussen een model en de replica is gestopt, kunnen deze kan niet ongedaan worden gemaakt. De replica-server vervolgens wordt een zelfstandige server en biedt nu ondersteuning voor zowel lees- en schrijfbewerkingen. Deze server kan niet opnieuw worden gemaakt in een replica.

Als u wilt stoppen met replicatie tussen een hoofd- en een replica-server via de Azure-portal, gebruikt u de volgende stappen uit:

1. Selecteer uw master Azure Database voor MySQL-server in de Azure-portal. 

2. Selecteer **replicatie** in het menu onder **instellingen**.

3. Selecteer de replica-server die u wilt stoppen van replicatie voor.

   ![Azure Database voor MySQL - Selecteer stoppen replicatie van server ](./media/howto-read-replica-portal/stop-replication-select.png)

4. Selecteer **stoppen van de replicatie**.

   ![Azure Database voor MySQL - stoppen van de replicatie ](./media/howto-read-replica-portal/stop-replication.png)

5. Controleer of u wilt stoppen van replicatie door te klikken op **OK**.

   ![Azure Database voor MySQL - stoppen van de replicatie bevestigen ](./media/howto-read-replica-portal/stop-replication-confirm.png)

## <a name="delete-a-replica-server"></a>Een replica-server verwijderen

Als een lezen-replica-server verwijderen uit de Azure-portal, gebruikt u de volgende stappen uit:

1. Selecteer uw master Azure Database voor MySQL-server in de Azure-portal.

2. Selecteer **replicatie** in het menu onder **instellingen**.

3. Selecteer de replicaserver die u wilt verwijderen.

   ![Azure Database for MySQL - Selecteer replica-server verwijderen ](./media/howto-read-replica-portal/delete-replica-select.png)

4. Selecteer **replica verwijderen**

   ![Azure Database voor MySQL - replica verwijderen ](./media/howto-read-replica-portal/delete-replica.png)

5. Typ de naam van de replica en op **verwijderen** om verwijdering van de replica te bevestigen.  

   ![Azure Database voor MySQL - replica verwijderen bevestigen ](./media/howto-read-replica-portal/delete-replica-confirm.png)

## <a name="delete-a-master-server"></a>Een hoofd-server verwijderen

> [!IMPORTANT]
> Verwijderen van een hoofd-server-replicatie naar alle replicaservers stopt en Hiermee verwijdert u de hoofd-server zelf. Replica-servers worden zelfstandige servers die bieden nu ondersteuning voor zowel lees- en schrijfbewerkingen.

Als een hoofd-server verwijderen uit de Azure-portal, gebruikt u de volgende stappen uit:

1. Selecteer uw master Azure Database voor MySQL-server in de Azure-portal.

2. Uit de **overzicht**, selecteer **verwijderen**.

   ![Azure Database voor MySQL - model verwijderen ](./media/howto-read-replica-portal/delete-master-overview.png)

3. Typ de naam van de hoofd-server en klikt u op **verwijderen** verwijderen van de hoofd-server te bevestigen.  

   ![Azure Database voor MySQL - model verwijderen ](./media/howto-read-replica-portal/delete-master-confirm.png)

## <a name="monitor-replication"></a>Monitor voor replicatie

1. In de [Azure-portal](https://portal.azure.com/), selecteert u de replica Azure Database voor MySQL-server die u wilt bewaken.

2. Onder de **bewaking** sectie van de zijbalk Selecteer **metrische gegevens**:

3. Selecteer **vertraging van replicatie in een paar seconden** in de vervolgkeuzelijst met beschikbare metrische gegevens. 

   ![Selecteer de vertraging van replicatie ](./media/howto-read-replica-portal/monitor-select-replication-lag.png)

4. Selecteer het tijdsbereik dat u wilt weergeven. De onderstaande afbeelding selecteert een periode van 30 minuten.

   ![Tijdsbereik selecteren ](./media/howto-read-replica-portal/monitor-replication-lag-time-range.png)

5. Bekijk de vertraging van replicatie voor het geselecteerde tijdsbereik. De onderstaande afbeelding geeft de laatste 30 minuten.

   ![Tijdsbereik selecteren ](./media/howto-read-replica-portal/monitor-replication-lag-time-range-thirty-mins.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [lezen replica's](concepts-read-replicas.md)