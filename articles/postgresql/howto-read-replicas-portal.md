---
title: Meer replica's voor Azure Database voor PostgreSQL beheren vanuit Azure portal
description: Informatie over het beheren van Azure Database for PostgreSQL lezen replica's van de Azure-portal.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 04/01/2019
ms.openlocfilehash: bf1fb1c1343173949ecb6348284cb537282b277b
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58846972"
---
# <a name="create-and-manage-read-replicas-from-the-azure-portal"></a>Maken en beheren van meer replica's van de Azure-portal

In dit artikel leert u hoe u maken en beheren van lezen-replica's in Azure Database for PostgreSQL via Azure portal. Zie voor meer informatie over meer replica's, de [overzicht](concepts-read-replicas.md).


## <a name="prerequisites"></a>Vereisten
Een [Azure Database for PostgreSQL-server](quickstart-create-server-database-portal.md) moet de hoofd-server.

## <a name="prepare-the-master-server"></a>De hoofd-server voorbereiden
Deze stappen moeten worden gebruikt om voor te bereiden van een hoofd-server in de lagen algemeen gebruik en geoptimaliseerd voor geheugen. De hoofd-server wordt voorbereid voor replicatie door de parameter azure.replication_support. Als de replicatie-parameter wordt gewijzigd, is een server opnieuw opstarten vereist voor de wijziging door te voeren. Deze twee stappen zijn in Azure portal, ingekapseld in één knop, **ondersteuning voor replicatie inschakelen**.

1. Selecteer de bestaande Azure Database for PostgreSQL-server om te gebruiken als een model in de Azure-portal.

2. Op de zijbalk server onder **instellingen**, selecteer **replicatie**.

3. Selecteer **Replicatieondersteuning voor inschakelen**. 

   ![Replicatieondersteuning voor inschakelen](./media/howto-read-replicas-portal/enable-replication-support.png)

4. Controleer of dat u wilt inschakelen Replicatieondersteuning. Met deze bewerking wordt opnieuw opgestart de hoofd-server. 

   ![Ondersteuning voor replicatie inschakelen bevestigen](./media/howto-read-replicas-portal/confirm-enable-replication.png)
   
5. U ontvangt twee Azure portalmeldingen zodra de bewerking voltooid is. Er is een melding voor het bijwerken van de parameter-server. Er is een andere melding voor de server opnieuw is gestart, die meteen volgt.

   ![Succes meldingen - inschakelen](./media/howto-read-replicas-portal/success-notifications-enable.png)

6. Vernieuwen van de Azure portal-pagina voor het bijwerken van de replicatie-werkbalk. U kunt nu meer replica's voor deze server maken.

   ![Bijgewerkte werkbalk](./media/howto-read-replicas-portal/updated-toolbar.png)
   
Replicatieondersteuning in te schakelen, is een eenmalige bewerking per hoofd-server. Een **Replicatieondersteuning uitschakelen** knop is opgegeven voor uw gemak. Wordt niet aanbevolen uitschakelen van de Replicatieondersteuning, tenzij u er zeker van te zijn dat u wordt nooit een replica maken op deze master-server. U kunt Replicatieondersteuning voor niet uitschakelen terwijl uw hoofd-server heeft de bestaande replica's.


## <a name="create-a-read-replica"></a>Maken van een replica lezen
Volg deze stappen voor het maken van een replica lezen:

1. Selecteer de bestaande Azure Database for PostgreSQL-server om te gebruiken als de hoofd-server. 

2. Op de zijbalk server onder **instellingen**, selecteer **replicatie**.

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