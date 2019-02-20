---
title: "Zelfstudie: MongoDB migreren naar Azure Cosmos DB's API voor online MongoDB met behulp van Azure Database Migration Service | Microsoft Docs"
description: Leer hoe u on-premises MongoDB kunt migreren naar Azure Cosmos DB's API voor online MongoDB met behulp van Azure Database Migration Service.
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: douglasl
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 02/12/2019
ms.openlocfilehash: 95286b7a63471ee07f76276d8b4b63ca5f2aecce
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56211996"
---
# <a name="tutorial-migrate-mongodb-to-azure-cosmos-dbs-api-for-mongodb-online-using-dms-preview"></a>Zelfstudie: MongoDB migreren naar Azure Cosmos DB's API voor online MongoDB met behulp van DMS (preview-versie)
Met Azure Database Migration Service kunt u een online migratie (met minimale downtime) van databases uitvoeren vanaf een on-premises of in de cloud aanwezig MongoDB -exemplaar naar Azure Cosmos DB's API voor MongoDB.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * De Azure-portal gebruiken om een Azure Database Migration Service-exemplaar te maken.
> * Een migratieproject maken met behulp van de Azure Database Migration Service.
> * De migratie uitvoeren.
> * De migratie controleren.
> * Voltooi de migratie als u klaar bent.

In deze zelfstudie migreert u met minimale downtime een gegevensset die in MongoDB wordt gehost in een virtuele Azure-machine naar Azure Cosmos DB's API voor MongoDB met behulp van Azure Database Migration Service. Als u nog geen MongoDB-bron hebt ingesteld, raadpleegt u het artikel over het [installeren en configureren van MongoDB op een Windows-VM in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/install-mongodb).

> [!NOTE]
> Als u Azure Database Migration Service gebruikt om een onlinemigratie uit te voeren, is het vereist dat u een exemplaar maakt op basis van de prijscategorie Premium.

> [!IMPORTANT]
> Voor een optimale migratie-ervaring raadt Microsoft u aan een exemplaar van de Azure Database Migration Service te maken in dezelfde Azure-regio als de doeldatabase. Het verplaatsen van gegevens naar regio's of geografieën kan het migratieproces vertragen.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

In dit artikel wordt een onlinemigratie beschreven van MongoDB naar Azure Cosmos DB's API voor MongoDB. Zie [MongoDB migreren naar Azure Cosmos DB's API voor offline MongoDB met behulp van DMS](tutorial-mongodb-cosmos-db.md) voor een offlinemigratie.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie hebt u het volgende nodig:
- [Maak een account voor Azure Cosmos DB's API voor MongoDB](https://ms.portal.azure.com/#create/Microsoft.DocumentDB).
- Maak een Azure Virtual Network (VNET) voor de Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel. Dit geeft site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Zorg ervoor dat uw VNET Network Security Group-regels de volgende communicatiepoorten niet blokkeren: 443, 53, 9354, 445 en 12000. Zie voor meer informatie over verkeer filteren van Azure VNET NSG het artikel [Netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Wijzig de firewall van de bronserver voor toegang van Azure Database Migration Service tot de bronserver van MongoDB. Standaard verloopt dit via TCP-poort 27017.
- Wanneer u een firewallapparaat gebruikt voor de brondatabase(s), moet u mogelijk firewallregels toevoegen om voor de Azure Database Migration Service toegang tot de brondatabase(s) voor de migratie toe te staan.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registreer de Microsoft.DataMigration-resourceprovider
1. Meld u aan bij de Azure-portal, selecteer **Alle services** en selecteer vervolgens **Abonnementen**.

   ![Portal-abonnementen weergeven](media/tutorial-mongodb-to-cosmosdb-online/portal-select-subscription1.png)
       
2. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken en selecteer vervolgens **Resourceproviders**.
 
    ![Resourceproviders weergeven](media/tutorial-mongodb-to-cosmosdb-online/portal-select-resource-provider.png)
    
3.  Zoek naar migratie en selecteer rechts van **Microsoft.DataMigration** de optie **Registreren**.
 
    ![Resourceprovider registreren](media/tutorial-mongodb-to-cosmosdb-online/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Een instantie maken
1.  Selecteer in de Azure-portal **Een resource maken**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service** uit de vervolgkeuzelijst.

    ![Azure Marketplace](media/tutorial-mongodb-to-cosmosdb-online/portal-marketplace.png)

2.  Selecteer in het scherm **Azure Database Migration Service** **Maken**.
 
    ![Azure Database Migration Service-exemplaar maken](media/tutorial-mongodb-to-cosmosdb-online/dms-create1.png)
  
3.  Geef in het scherm **Migratieservice maken** een naam op voor de service, het abonnement en een nieuwe of bestaande resourcegroep.

4. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken. 

5. Selecteer een bestaand virtueel netwerk (VNET) of maak een nieuwe.

    Het VNET biedt de Azure Database Migration Service toegang tot de broninstantie van MongoDB en het doelaccount van Azure Cosmos DB.

    Zie het artikel voor meer informatie over het maken van een VNET in de Azure-portal [Een virtueel netwerk maken met de Azure-portal](https://aka.ms/DMSVnet).

6. Selecteer een SKU in de prijscategorie Premium.

    > [!NOTE]
    > Onlinemigraties worden alleen ondersteund bij gebruik van deze categorie. Zie voor meer informatie over de kosten en prijscategorieën de [Pagina met prijzen](https://aka.ms/dms-pricing).

    Als u hulp nodig hebt bij het kiezen van de juiste Azure Database Migration Service-laag, raadpleegt u de aanbevelingen in de blogpost [hier](https://go.microsoft.com/fwlink/?linkid=861067).  

     ![Instellingen configureren van een Azure Database Migration Service-exemplaar](media/tutorial-mongodb-to-cosmosdb-online/dms-settings3.png)

7.  Selecteer **Maken** om de dienst te maken.

## <a name="create-a-migration-project"></a>Maak een migratieproject
Nadat de service is gemaakt, zoek deze op in de Azure-portal, open hem en maak vervolgens een nieuw migratieproject.

1. Selecteer in de Azure-portal **Alle diensten**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service**.
 
    ![Zoek alle exemplaren van de Azure Database Migration Service](media/tutorial-mongodb-to-cosmosdb-online/dms-search.png)

2. Zoek in het scherm **Azure Database Migration service** naar de naam van de Azure Database Migration Service-instantie die u hebt gemaakt en selecteer vervolgens het exemplaar.

    U kunt het service-exemplaar van Azure Database Migration ook vinden via het zoekvenster in de Azure-portal.

    ![Het zoekvenster gebruiken in de Azure-portal](media/tutorial-mongodb-to-cosmosdb-online/dms-search-portal.png)

3. Selecteer + **Nieuw migratieproject**.

4. Geef in het scherm **Nieuw migratieproject** een naam op voor het project in het tekstvak **Type bronserver**, selecteer **MongoDB**, selecteer in het tekstvak **Type doelserver** **CosmosDB (MongoDB-API)**, en selecteer bij **Type activiteit kiezen** de optie **Online gegevensmigratie [preview-versie]**.

    ![Database Migration Service-project maken](media/tutorial-mongodb-to-cosmosdb-online/dms-create-project1.png)

5.  Selecteer **Opslaan** en vervolgens **Create and run activity** om het project te maken en de migratieactiviteit uit te voeren.

## <a name="specify-source-details"></a>Geef brondetails op
1. Geef in het scherm **Brondetails** de verbindingsgegevens op voor de MongoDB-bronserver.

    Er zijn drie modi om verbinding te maken met een bron:
       * **Standaardmodus**: deze accepteert een Fully Qualified Domain Name of een IP-adres, poortnummer en verbindingsreferenties.
       * **Modus verbindingsreeks**: deze accepteert een MongoDB-verbindingsreeks, zoals beschreven in het artikel [Connection String URI Format](https://docs.mongodb.com/manual/reference/connection-string/) (URI-indeling van verbindingsreeks).
       * **Gegevens uit Azure-opslag**: deze accepteert een SAS-URL van de blob-container. Selecteer **Blob contains BSON dump** als de blob-container BSON-dumps bevat die zijn geproduceerd door het [bsondump-hulpprogramma](https://docs.mongodb.com/manual/reference/program/bsondump/) van MongoDB en deselecteer het als de container JSON-bestanden bevat.

    U kunt het IP-adres gebruiken voor situaties waarin DNS-naamomzetting niet mogelijk is.

   ![Geef brondetails op](media/tutorial-mongodb-to-cosmosdb-online/dms-specify-source1.png)

2. Selecteer **Opslaan**.

   > [!NOTE]
   > Het adres van de bronserver moet het adres zijn van de primaire server als de bron een replicaset is. Het moet het adres van de router zijn als de bron een shard-MongoDB-cluster is. Voor een shard-MongoDB-cluster moet Azure Database Migration Service verbinding kunnen maken met de afzonderlijke shards in het cluster. Hiervoor moet mogelijk de firewall op meerdere machines worden geopend.          

## <a name="specify-target-details"></a>Doeldetails opgeven
1. Geef in het scherm **Details migratiedoel** de verbindingsgegevens op voor het Azure Cosmos DB-doelaccount, dat bestaat uit het vooraf ingerichte Azure Cosmos DB's API voor MongoDB-account waarnaar u de MongoDB-database migreert.

    ![Doeldetails opgeven](media/tutorial-mongodb-to-cosmosdb-online/dms-specify-target1.png)

2. Selecteer **Opslaan**.

## <a name="map-to-target-databases"></a>Toewijzen aan doeldatabases
1. Wijs in het scherm **Toewijzen aan doeldatabases** de bron- en doeldatabase voor de migratie toe.

    Als de doeldatabase de naam van de dezelfde database als de bron-database bevat, wordt in de Azure Database Migration Service de doeldatabase standaard geselecteerd.

    Als de tekenreeks **Maken** wordt weergegeven naast de naam van de database, is de doeldatabase niet gevonden door Azure Database Migration Service en wordt de database voor u gemaakt.

    Op dit punt van de migratie geeft u een doorvoeraanvraageenheid op indien doorvoer van shares voor de database is gewenst. In Cosmos DB kunt u doorvoer inrichten op het niveau van de database of afzonderlijk voor elke verzameling. Doorvoer wordt gemeten in [Aanvraageenheden](https://docs.microsoft.com/azure/cosmos-db/request-units) (RU's). Meer informatie over de [prijzen van Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/).

    ![Toewijzen aan doeldatabases](media/tutorial-mongodb-to-cosmosdb-online/dms-map-target-databases1.png)

2. Selecteer **Opslaan**.

3. Vouw in het scherm **Verzamelingsinstelling** de lijst met verzamelingen uit en bekijk welke verzamelingen worden gemigreerd.

    Houd er rekening mee dat in Azure Database Migration Service automatisch alle verzamelingen op het MongoDB-bronexemplaar worden geselecteerd die niet aanwezig zijn op het Azure Cosmos DB-doelaccount. Als u verzamelingen die al gegevens bevatten, opnieuw wilt migreren, moet u die verzamelingen expliciet selecteren in dit scherm.

    U kunt opgeven hoeveel RU's u voor de verzamelingen wilt gebruiken. In de meeste gevallen moet een waarde tussen de 500 (minimaal 1000 voor shard-verzamelingen) en 4000 voldoende zijn. In Azure Database Migration Service worden slimme standaardinstellingen voorgesteld op basis van de grootte van de verzameling.

    U kunt ook een shardsleutel opgeven om te profiteren van [partitionering in Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview) voor optimale schaalbaarheid. Lees de [best practices voor het selecteren van een shard-/partitiesleutel](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey). Als u geen partitiesleutel hebt, kunt u altijd gebruikmaken van **_id** als de shardsleutel voor een betere doorvoer.

    ![Verzamelingstabellen selecteren](media/tutorial-mongodb-to-cosmosdb-online/dms-collection-setting1.png)

4. Selecteer **Opslaan**.

5. Geef in het tekstvak **Naam activiteit** van het scherm **Migratieoverzicht** een naam op voor de migratieactiviteit.

    ![Migratieoverzicht](media/tutorial-mongodb-to-cosmosdb-online/dms-migration-summary1.png)

## <a name="run-the-migration"></a>De migratie uitvoeren
- Selecteer **Migratie uitvoeren**.

   Het venster van de migratieactiviteit wordt weergegeven en de **status** van de activiteit wordt weergegeven.

   ![Activiteitsstatus](media/tutorial-mongodb-to-cosmosdb-online/dms-activity-status1.png)

## <a name="monitor-the-migration"></a>Bewaak de migratie
- Selecteer **Vernieuwen** in het scherm van de activiteitenmigratie om de weergave bij te werken totdat de **status** van de migratie als **Opnieuw afspelen** wordt weergegeven.

   > [!NOTE]
   > U kunt de activiteit selecteren voor meer informatie over metrische migratiegegevens op het niveau van een database of verzameling.

   ![Activiteitsstatus opnieuw afspelen](media/tutorial-mongodb-to-cosmosdb-online/dms-activity-replaying.png)

## <a name="verify-data-in-cosmos-db"></a>Gegevens controleren in Cosmos DB

1. Breng wijzigingen aan uw MongoDB-brondatabase aan.
2. Maak verbinding met COSMOS DB om te verifiëren of de gegevens vanuit de MongoDB-bronserver worden gerepliceerd.

    ![Activiteitsstatus opnieuw afspelen](media/tutorial-mongodb-to-cosmosdb-online/dms-verify-data.png)
 
## <a name="complete-the-migration"></a>Migratie voltooien

* Als alle documenten van de bron beschikbaar zijn op het COSMOS DB-doel, selecteert u **Voltooien** in het snelmenu van de migratieactiviteit om de migratie te voltooien.

    Met deze actie wordt het opnieuw afspelen van alle wijzigingen die in behandeling zijn, voltooid en tevens de migratie.

    ![Activiteitsstatus opnieuw afspelen](media/tutorial-mongodb-to-cosmosdb-online/dms-finish-migration.png)

## <a name="additional-resources"></a>Aanvullende resources

 * [Informatie over Cosmos DB-service](https://azure.microsoft.com/services/cosmos-db/)

## <a name="next-steps"></a>Volgende stappen
- Lees de Microsoft-[handleiding voor databasemigratie](https://datamigration.microsoft.com/) voor hulp bij andere migratiescenario's.