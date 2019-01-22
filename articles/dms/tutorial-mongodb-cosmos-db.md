---
title: "Zelfstudie: MongoDB migreren naar Azure Cosmos DB's API voor offline MongoDB met behulp van Azure Database Migration Service | Microsoft Docs"
description: Leer hoe u on-premises MongoDB kunt migreren naar Azure Cosmos DB's API voor offline MongoDB met behulp van Azure Database Migration Service.
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: douglasl
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 12/11/2018
ms.openlocfilehash: b4f8d2bdbce20fc7a932280edc26cb3ddfbe6471
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54247602"
---
# <a name="tutorial-migrate-mongodb-to-azure-cosmos-dbs-api-for-mongodb-offline-using-dms"></a>Zelfstudie: MongoDB migreren naar Azure Cosmos DB's API voor offline MongoDB met behulp van DMS
Met Azure Database Migration Service kunt u een offline (eenmalige) migratie van databases uitvoeren van MongoDB on-premises of in de cloud naar Azure Cosmos DB's API voor MongoDB.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * De Azure-portal gebruiken om een Azure Database Migration Service-exemplaar te maken.
> * Een migratieproject maken met behulp van de Azure Database Migration Service.
> * De migratie uitvoeren.
> * De migratie controleren.

In deze zelfstudie migreert u een gegevensset die in MongoDB wordt gehost in een virtuele Azure-machine naar Azure Cosmos DB's API voor MongoDB met behulp van Azure Database Migration Service. Als u nog geen MongoDB-bron hebt ingesteld, raadpleegt u het artikel over het [installeren en configureren van MongoDB op een Windows-VM in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/install-mongodb).

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie hebt u het volgende nodig:
- [Maak een account voor Azure Cosmos DB's API voor MongoDB](https://ms.portal.azure.com/#create/Microsoft.DocumentDB).
- Maak een VNET voor de Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel. Dit geeft site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Zorg ervoor dat uw Azure Virtual Network (VNET) Network Security Group-regels de volgende communicatiepoorten niet blokkeren: 443, 53, 9354, 445 en 12000. Zie voor meer informatie over verkeer filteren van Azure VNET NSG het artikel [Netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Stel uw Windows-firewall open voor toegang van Azure Database Migration Service tot de bronserver van MongoDB. Standaard verloopt dit via TCP-poort 27017.
- Wanneer u een firewallapparaat gebruikt voor de brondatabase(s), moet u mogelijk firewallregels toevoegen om voor de Azure Database Migration Service toegang tot de brondatabase(s) voor de migratie toe te staan.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registreer de Microsoft.DataMigration-resourceprovider
1. Meld u aan bij de Azure-portal, selecteer **Alle services** en selecteer vervolgens **Abonnementen**.

   ![Portal-abonnementen weergeven](media/tutorial-mongodb-to-cosmosdb/portal-select-subscription1.png)
       
2. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken en selecteer vervolgens **Resourceproviders**.
 
    ![Resourceproviders weergeven](media/tutorial-mongodb-to-cosmosdb/portal-select-resource-provider.png)
    
3.  Zoek naar migratie en selecteer rechts van **Microsoft.DataMigration** de optie **Registreren**.
 
    ![Resourceprovider registreren](media/tutorial-mongodb-to-cosmosdb/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Een instantie maken
1.  Selecteer in de Azure-portal **Een resource maken**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service** uit de vervolgkeuzelijst.

    ![Azure Marketplace](media/tutorial-mongodb-to-cosmosdb/portal-marketplace.png)

2.  Selecteer in het scherm **Azure Database Migration Service** **Maken**.
 
    ![Azure Database Migration Service-exemplaar maken](media/tutorial-mongodb-to-cosmosdb/dms-create1.png)
  
3.  Geef in het scherm **Migratieservice maken** een naam op voor de service, het abonnement en een nieuwe of bestaande resourcegroep.

4. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken. 

5. Selecteer een bestaand virtueel netwerk (VNET) of maak een nieuwe.

    Het VNET biedt de Azure Database Migration Service toegang tot de broninstantie van MongoDB en het doelaccount van Azure Cosmos DB.

    Zie het artikel voor meer informatie over het maken van een VNET in de Azure-portal [Een virtueel netwerk maken met de Azure-portal](https://aka.ms/DMSVnet).

6. Selecteer een prijscategorie.

    Zie voor meer informatie over de kosten en prijscategorieën de [Pagina met prijzen](https://aka.ms/dms-pricing).

    Als u hulp nodig hebt bij het kiezen van de juiste Azure Database Migration Service-categorie, raadpleegt u de aanbevelingen in de blogpost [hier](https://go.microsoft.com/fwlink/?linkid=861067).  

     ![Instellingen configureren van een Azure Database Migration Service-exemplaar](media/tutorial-mongodb-to-cosmosdb/dms-settings2.png)

7.  Selecteer **Maken** om de dienst te maken.

## <a name="create-a-migration-project"></a>Maak een migratieproject
Nadat de service is gemaakt, zoek deze op in de Azure-portal, open hem en maak vervolgens een nieuw migratieproject.

1. Selecteer in de Azure-portal **Alle diensten**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service**.
 
      ![Zoek alle exemplaren van de Azure Database Migration Service](media/tutorial-mongodb-to-cosmosdb/dms-search.png)

2. Zoek in het scherm **Azure Database Migration service** naar de naam van de Azure Database Migration Service-instantie die u hebt gemaakt en selecteer vervolgens het exemplaar.

3. Selecteer + **Nieuw migratieproject**.

4. Geef in het scherm **Nieuw migratieproject** een naam op voor het project in het tekstvak **bronservertype**, selecteer **MongoDB**, selecteer in het tekstvak **Type doelserver** **CosmosDB (MongoDB-API)**, en selecteer bij **Type activiteit kiezen** de optie **Offline gegevensmigratie**. 

    ![Database Migration Service-project maken](media/tutorial-mongodb-to-cosmosdb/dms-create-project.png)

5.  Selecteer **Maken en uitvoeren van de activiteit** om het project te maken en de migratieactiviteit uit te voeren.

## <a name="specify-source-details"></a>Geef brondetails op
1. Geef in het scherm **Brondetails** de verbindingsgegevens op voor de MongoDB-bronserver.
    
   U kunt ook de verbindingsreeksmodus gebruiken en een locatie opgeven voor een weblog-archiefbestandscontainer met de dump van de verzamelingsgegevens die u wilt migreren.

   > [!NOTE]
   > Azure Database Migration Service kan ook bson- of json-documenten migreren naar Azure Cosmos DB's API voor MongoDB-verzamelingen.
    
   U kunt ook het IP-adres gebruiken voor situaties waarin DNS-naamomzetting niet mogelijk is.

   ![Geef brondetails op](media/tutorial-mongodb-to-cosmosdb/dms-specify-source.png)

2. Selecteer **Opslaan**.

## <a name="specify-target-details"></a>Doeldetails opgeven
1. Geef in het scherm **Details migratiedoel** de verbindingsgegevens op voor het Azure Cosmos DB-doelaccount, dat bestaat uit het vooraf ingerichte Azure Cosmos DB's API voor MongoDB-account waarnaar u de MongoDB-database migreert.

    ![Doeldetails opgeven](media/tutorial-mongodb-to-cosmosdb/dms-specify-target.png)

2. Selecteer **Opslaan**.

## <a name="map-to-target-databases"></a>Toewijzen aan doeldatabases
1. Wijs in het scherm **Toewijzen aan doeldatabases** de bron- en doeldatabase voor de migratie toe.

    Als de doeldatabase de naam van de dezelfde database als de bron-database bevat, wordt in de Azure Database Migration Service de doeldatabase standaard geselecteerd.

    Als de tekenreeks **Maken** wordt weergegeven naast de naam van de database, is de doeldatabase niet gevonden door Azure Database Migration Service en wordt de database voor u gemaakt.

    Op dit moment in de migratie kunt u [doorvoer inrichten](https://docs.microsoft.com/azure/cosmos-db/set-throughput). In Cosmos DB kunt u doorvoer inrichten op het niveau van de database of afzonderlijk voor elke verzameling. Doorvoer wordt gemeten in [Aanvraageenheden](https://docs.microsoft.com/azure/cosmos-db/request-units) (RU's). Meer informatie over de [prijzen van Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/).

    ![Toewijzen aan doeldatabases](media/tutorial-mongodb-to-cosmosdb/dms-map-target-databases.png)

2. Selecteer **Opslaan**.
3. Vouw in het scherm **Verzamelingsinstelling** de lijst met verzamelingen uit en bekijk welke verzamelingen worden gemigreerd.

    Houd er rekening mee dat in Azure Database Migration Service automatisch alle verzamelingen op het MongoDB-bronexemplaar worden geselecteerd die niet aanwezig zijn op het Azure Cosmos DB-doelaccount. Als u verzamelingen die al gegevens bevatten opnieuw wilt migreren, moet u die verzamelingen expliciet selecteren op deze blade.

    U kunt opgeven hoeveel RU's u voor de verzamelingen wilt gebruiken. In Azure Database Migration Service worden slimme standaardinstellingen voorgesteld op basis van de grootte van de verzameling.

    U kunt ook een shardsleutel opgeven om te profiteren van [partitionering in Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview) voor optimale schaalbaarheid. Lees de [best practices voor het selecteren van een shard-/partitiesleutel](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey).

    ![Verzamelingstabellen selecteren](media/tutorial-mongodb-to-cosmosdb/dms-collection-setting.png)

4. Selecteer **Opslaan**.

5. Geef in het tekstvak **Naam activiteit** van het scherm **Migratieoverzicht** een naam op voor de migratieactiviteit.

    ![Migratieoverzicht](media/tutorial-mongodb-to-cosmosdb/dms-migration-summary.png)

## <a name="run-the-migration"></a>De migratie uitvoeren
- Selecteer **Migratie uitvoeren**.

    Het venster van de migratieactiviteit wordt weergegeven en de **status** van de activiteit is **Niet gestart**.

    ![Activiteitsstatus](media/tutorial-mongodb-to-cosmosdb/dms-activity-status.png)

## <a name="monitor-the-migration"></a>Bewaak de migratie
- Selecteer op het scherm van de activiteitenmigratie **Vernieuwen** om de weergave bij te werken totdat de **Status** van de migratie als **Voltooid** wordt weergegeven.

   > [!NOTE]
   > U kunt de activiteit selecteren voor meer informatie over metrische migratiegegevens op het niveau van een database of verzameling.

    ![Activiteitsstatus Voltooid](media/tutorial-mongodb-to-cosmosdb/dms-activity-completed.png)

## <a name="verify-data-in-cosmos-db"></a>Gegevens controleren in Cosmos DB

- Nadat de migratie is voltooid, kunt u in uw Azure Cosmos DB-account controleren of alle verzamelingen met succes zijn gemigreerd.

    ![Activiteitsstatus Voltooid](media/tutorial-mongodb-to-cosmosdb/dms-cosmosdb-data-explorer.png)

## <a name="additional-resources"></a>Aanvullende resources

 * [Informatie over Cosmos DB-service](https://azure.microsoft.com/services/cosmos-db/)

## <a name="next-steps"></a>Volgende stappen
- Lees de Microsoft-[handleiding voor databasemigratie](https://datamigration.microsoft.com/) voor hulp bij andere migratiescenario's.