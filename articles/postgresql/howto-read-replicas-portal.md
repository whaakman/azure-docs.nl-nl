---
title: Meer replica's voor Azure Database voor PostgreSQL beheren vanuit Azure portal
description: Informatie over het beheren van Azure Database for PostgreSQL lezen replica's van de Azure-portal.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/19/2019
ms.openlocfilehash: 24a37775298d6c6b40ec49f34158fcb77f26a379
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58113211"
---
# <a name="create-and-manage-read-replicas-from-the-azure-portal"></a>Maken en beheren van meer replica's van de Azure-portal

In dit artikel leert u hoe u maken en beheren van lezen-replica's in Azure Database for PostgreSQL via Azure portal. Zie voor meer informatie over meer replica's, de [overzicht](concepts-read-replicas.md).

> [!IMPORTANT]
> De functie lezen replica is beschikbaar als openbare preview.

## <a name="prerequisites"></a>Vereisten
Een [Azure Database for PostgreSQL-server](quickstart-create-server-database-portal.md) moet de hoofd-server.

## <a name="prepare-the-master-server"></a>De hoofd-server voorbereiden
Deze stappen moeten worden gebruikt om voor te bereiden van een hoofd-server in de lagen algemeen gebruik en geoptimaliseerd voor geheugen.

De `azure.replication_support` parameter moet worden ingesteld op **REPLICA** op de hoofd-server. Als deze parameter wordt gewijzigd, is een server opnieuw opstarten vereist voor de wijziging door te voeren.

1. Selecteer de bestaande Azure Database for PostgreSQL-server om te gebruiken als een model in de Azure-portal.

2. Selecteer in het menu links **serverparameters**.

3. Zoek de `azure.replication_support` parameter.

   ![Zoek naar de parameter azure.replication_support](./media/howto-read-replicas-portal/azure-replication-parameter.png)

4. Stel de `azure.replication_support` parameterwaarde **REPLICA**. Selecteer **opslaan** om uw wijzigingen te behouden.

   ![Stel de parameter in op de REPLICA en sla de wijzigingen](./media/howto-read-replicas-portal/save-parameter-replica.png)

5. Nadat u uw wijzigingen hebt opgeslagen, kunt u een melding ontvangen:

   ![Melding opslaan](./media/howto-read-replicas-portal/parameter-save-notification.png)

6. Start opnieuw op de server om uw wijzigingen te laten. Als u wilt weten hoe u een server opnieuw opstarten, Zie [opnieuw starten van een Azure Database for PostgreSQL-server](howto-restart-server-portal.md).


## <a name="create-a-read-replica"></a>Maken van een replica lezen
Volg deze stappen voor het maken van een replica lezen:

1. Selecteer de bestaande Azure Database for PostgreSQL-server om te gebruiken als de hoofd-server. 

2. In het servermenu onder **instellingen**, selecteer **replicatie**.

   Als u dit nog niet hebt ingesteld de `azure.replication_support` parameter **REPLICA** op een algemeen gebruik of geoptimaliseerd voor geheugen-master server en de server opnieuw opgestart, ontvangt u een melding. Deze stappen voltooien voordat u de replica maakt.

3. Selecteer **-Replica toevoegen**.

   ![Toevoegen van een replica](./media/howto-read-replicas-portal/add-replica.png)

4. Voer een naam voor de replica voor lezen. Selecteer **OK** om te bevestigen van het maken van de replica.

   ![De naam van de replica](./media/howto-read-replicas-portal/name-replica.png) 

Een replica wordt gemaakt met behulp van de configuratie van de dezelfde server als het model. Nadat een replica is gemaakt, verschillende instellingen van de hoofdserver onafhankelijk van elkaar kunnen worden gewijzigd: compute genereren, vCores, opslag en back-up maken van bewaarperiode. De prijscategorie kan ook afzonderlijk worden gewijzigd met uitzondering van of naar de Basic-laag.

> [!IMPORTANT]
> Werk de configuratie van de replica op gelijke of grotere waarden voordat een hoofd-server-configuratie is bijgewerkt met nieuwe waarden. Deze actie zorgt ervoor dat de replica kunt houden met eventuele wijzigingen in het model.

Nadat de lezen replica is gemaakt, kan het worden bekeken in de **replicatie** venster:

![De nieuwe replica in de replicatie-venster weergeven](./media/howto-read-replicas-portal/list-replica.png)
 

## <a name="stop-replication"></a>Replicatie stoppen
U kunt replicatie tussen een hoofd-server en een lezen replica stoppen.

> [!IMPORTANT]
> Na het stoppen van replicatie naar een hoofd-server en een replica lezen, kunnen deze kan niet ongedaan worden gemaakt. De replica voor lezen wordt een zelfstandige server die ondersteuning biedt voor zowel lees- en schrijfbewerkingen. De zelfstandige server kan niet opnieuw worden gemaakt in een replica.

Als u wilt stoppen met replicatie tussen een hoofd-server en een lezen replica is vanaf de Azure-portal, de volgende stappen uit:

1. Selecteer uw master Azure Database for PostgreSQL-server in de Azure-portal.

2. In het servermenu onder **instellingen**, selecteer **replicatie**.

3. Selecteer de replica-server waarvoor om replicatie te stoppen.

   ![Selecteer de replica](./media/howto-read-replicas-portal/select-replica.png)
 
4. Selecteer **stoppen van de replicatie**.

   ![Selecteer stoppen van de replicatie](./media/howto-read-replicas-portal/select-stop-replication.png)
 
5. Selecteer **OK** om replicatie te stoppen.

   ![Bevestig dat u wilt stoppen van replicatie](./media/howto-read-replicas-portal/confirm-stop-replication.png)
 

## <a name="delete-a-master-server"></a>Een hoofd-server verwijderen
Als u wilt verwijderen een hoofd-server, moet u dezelfde stappen over het verwijderen van een zelfstandige Azure Database for PostgreSQL-server gebruiken. 

> [!IMPORTANT]
> Wanneer u een hoofdserver verwijdert, wordt replicatie naar alle lezen-replica's gestopt. De lezen-replica's worden zelfstandige servers die bieden nu ondersteuning voor zowel lees- en schrijfbewerkingen.

Als een server verwijderen uit de Azure-portal, de volgende stappen uit:

1. Selecteer uw master Azure Database for PostgreSQL-server in de Azure-portal.

2. Open de **overzicht** pagina voor de server. Selecteer **Verwijderen**.

   ![Selecteer op de pagina overzicht van server verwijderen van de hoofd-server](./media/howto-read-replicas-portal/delete-server.png)
 
3. Voer de naam van de hoofd-server te verwijderen. Selecteer **verwijderen** verwijderen van de hoofd-server te bevestigen.

   ![Bevestig dat u wilt verwijderen van de hoofd-server](./media/howto-read-replicas-portal/confirm-delete.png)
 

## <a name="delete-a-replica"></a>Een replica verwijderen
U kunt een vergelijkbaar met hoe u een hoofdserver verwijdert lezen replica verwijderen.

- Open in de Azure-portal, de **overzicht** pagina voor de replica voor lezen. Selecteer **Verwijderen**.

   ![Selecteer op de overzichtspagina van de replica om de replica te verwijderen](./media/howto-read-replicas-portal/delete-replica.png)
 
U kunt ook verwijderen met de lezen replica van de **replicatie** venster door de volgende stappen:

1. Selecteer uw master Azure Database for PostgreSQL-server in de Azure-portal.

2. In het servermenu onder **instellingen**, selecteer **replicatie**.

3. Selecteer de lezen-replica te verwijderen.

   ![Selecteer de replica verwijderen](./media/howto-read-replicas-portal/select-replica.png)
 
4. Selecteer **replica verwijderen**.

   ![Selecteer een replica verwijderen](./media/howto-read-replicas-portal/select-delete-replica.png)
 
5. Voer de naam van de replica te verwijderen. Selecteer **verwijderen** om verwijdering van de replica te bevestigen.

   ![Bevestig dat u wilt replica te verwijderen](./media/howto-read-replicas-portal/confirm-delete-replica.png)
 

## <a name="monitor-a-replica"></a>Een replica bewaken
Twee metrische gegevens zijn beschikbaar voor het bewaken van lezen replica's.

### <a name="max-lag-across-replicas-metric"></a>Maximale vertraging voor replica's metrische gegevens
De **maximale vertraging voor replica's** ziet u de vertraging in bytes tussen de hoofd-server en de meeste achtergebleven-replica. 

1.  Selecteer de master Azure Database for PostgreSQL-server in de Azure-portal.

2.  Selecteer **Metrische gegevens**. In de **metrische gegevens** venster **maximale vertraging voor replica's**.

    ![Monitor voor de maximale vertraging voor replica 's](./media/howto-read-replicas-portal/select-max-lag.png)
 
3.  Voor uw **aggregatie**, selecteer **Max**.


### <a name="replica-lag-metric"></a>Replica Lag metrische gegevens
De **Replica Lag** metrische gegevens geeft de tijd sinds de laatste replay transactie op een replica. Als er geen transacties plaatsvinden in uw model, geeft de metriek deze vertraging.

1. Selecteer in de Azure-portal, de Azure Database voor PostgreSQL replica lezen.

2. Selecteer **Metrische gegevens**. In de **metrische gegevens** venster **Replica Lag**.

   ![Monitor voor de vertraging replica](./media/howto-read-replicas-portal/select-replica-lag.png)
 
3. Voor uw **aggregatie**, selecteer **Max**. 
 
## <a name="next-steps"></a>Volgende stappen
Meer informatie over [lezen-replica's in Azure Database for PostgreSQL](concepts-read-replicas.md).