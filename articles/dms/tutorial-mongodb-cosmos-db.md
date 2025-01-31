---
title: 'Zelfstudie: Azure Database Migration Service gebruiken voor het migreren van MongoDB naar Azure Cosmos DB-API voor MongoDB offline | Microsoft Docs'
description: Informatie over het migreren van MongoDB on-premises naar Azure Cosmos DB-API voor MongoDB offline met behulp van Azure Database Migration Service.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 07/04/2019
ms.openlocfilehash: 2ff5ebefbe379edda94dcf8ac066027398e2f3f4
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565550"
---
# <a name="tutorial-migrate-mongodb-to-azure-cosmos-dbs-api-for-mongodb-offline-using-dms"></a>Zelfstudie: MongoDB migreren naar Azure Cosmos DB's API voor offline MongoDB met behulp van DMS

Azure Database Migration Service kunt u een offline (eenmalig) worden gemigreerd van databases uitvoeren vanaf een on-premises of in de cloud van exemplaar van MongoDB naar Azure Cosmos DB-API voor MongoDB.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
>
> * Maak een instantie van Azure Database Migration Service.
> * Een migratieproject maken met behulp van Azure Database Migration Service.
> * De migratie uitvoeren.
> * De migratie controleren.

In deze zelfstudie maakt migreren u een gegevensset in die wordt gehost in een virtuele Machine van Azure naar Azure Cosmos DB-API voor MongoDB met Azure Database Migration Service MongoDB. Als u nog geen MongoDB-bron hebt ingesteld, raadpleegt u het artikel over het [installeren en configureren van MongoDB op een Windows-VM in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/install-mongodb).

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* [De vóór de migratie voltooien](../cosmos-db/mongodb-pre-migration.md) stappen zoals het schatten van de doorvoer, een partitiesleutel en het indexeringsbeleid te kiezen.
* [Maak een account voor Azure Cosmos DB's API voor MongoDB](https://ms.portal.azure.com/#create/Microsoft.DocumentDB).
* Een Azure Virtual Network (VNet) maken voor Azure Database Migration Service met behulp van Azure Resource Manager-implementatiemodel, waarmee u site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction)of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Zie voor meer informatie over het maken van een VNet, de [documentatie voor Virtual Network](https://docs.microsoft.com/azure/virtual-network/), en met name de artikelen met vindt u meer details.

    > [!NOTE]
    > Tijdens de installatie van de VNet, als u ExpressRoute gebruikt met het naar Microsoft-netwerkpeering, voeg de volgende service [eindpunten](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) aan het subnet waarin de service worden ingericht:
    >
    > * Doel-database-eindpunt (bijvoorbeeld SQL-eindpunt, Cosmos DB-eindpunt, enzovoort)
    > * Opslageindpunt
    > * Service bus-eindpunt
    >
    > Deze configuratie is nodig omdat Azure Database Migration Service beschikt niet over de verbinding met internet.

* Zorg ervoor dat uw regels VNet Netwerkbeveiligingsgroep (NSG) de volgende communicatiepoorten niet blokkeren: 53, 443, 445, 9354 en 10000 20000. Zie het artikel voor meer informatie over Azure VNet NSG wordt verkeer gefilterd, [netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
* Open uw Windows-firewall voor Azure Database Migration Service toegang tot de bron MongoDB-server, die standaard TCP-poort 27017 is.
* Wanneer u een apparaat voor een firewall voor de brondatabase (s), moet u mogelijk toevoegen van firewallregels om toe te staan van Azure Database Migration Service voor toegang tot de brondatabase (s) voor de migratie.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registreer de Microsoft.DataMigration-resourceprovider

1. Meld u aan bij de Azure-portal, selecteer **Alle services** en selecteer vervolgens **Abonnementen**.

   ![Portal-abonnementen weergeven](media/tutorial-mongodb-to-cosmosdb/portal-select-subscription1.png)

2. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken en selecteer vervolgens **Resourceproviders**.

    ![Resourceproviders weergeven](media/tutorial-mongodb-to-cosmosdb/portal-select-resource-provider.png)

3. Zoek naar migratie en selecteer rechts van **Microsoft.DataMigration** de optie **Registreren**.

    ![Resourceprovider registreren](media/tutorial-mongodb-to-cosmosdb/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Een instantie maken

1. Selecteer in de Azure-portal **Een resource maken**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service** uit de vervolgkeuzelijst.

    ![Azure Marketplace](media/tutorial-mongodb-to-cosmosdb/portal-marketplace.png)

2. Selecteer in het scherm **Azure Database Migration Service** **Maken**.

    ![Azure Database Migration Service-exemplaar maken](media/tutorial-mongodb-to-cosmosdb/dms-create1.png)
  
3. Geef in het scherm **Migratieservice maken** een naam op voor de service, het abonnement en een nieuwe of bestaande resourcegroep.

4. Selecteer de locatie waarin u wilt maken van het exemplaar van Azure Database Migration Service. 

5. Selecteer een bestaand VNet of maak een nieuwe.

    Het VNet biedt Azure Database Migration Service toegang tot de bron-MongoDB-exemplaar en de doel-Azure Cosmos DB-account.

    Zie het artikel voor meer informatie over het maken van een VNet in Azure portal [een virtueel netwerk maken met de Azure-portal](https://aka.ms/DMSVnet).

6. Selecteer een prijscategorie.

    Zie voor meer informatie over de kosten en prijscategorieën de [Pagina met prijzen](https://aka.ms/dms-pricing).

    ![Instellingen configureren van een Azure Database Migration Service-exemplaar](media/tutorial-mongodb-to-cosmosdb/dms-settings2.png)

7. Selecteer **Maken** om de dienst te maken.

## <a name="create-a-migration-project"></a>Maak een migratieproject

Nadat de service is gemaakt, zoek deze op in de Azure-portal, open hem en maak vervolgens een nieuw migratieproject.

1. Selecteer in de Azure-portal **Alle diensten**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service**.

      ![Zoeken naar alle exemplaren van Azure Database Migration Service](media/tutorial-mongodb-to-cosmosdb/dms-search.png)

2. Op de **Azure Database Migration service** scherm, zoek naar de naam van de Azure Database Migration Service-instantie die u hebt gemaakt en selecteer vervolgens het exemplaar.

3. Selecteer + **Nieuw migratieproject**.

4. Geef in het scherm **Nieuw migratieproject** een naam op voor het project in het tekstvak **bronservertype**, selecteer **MongoDB**, selecteer in het tekstvak **Type doelserver** **CosmosDB (MongoDB-API)** , en selecteer bij **Type activiteit kiezen** de optie **Offline gegevensmigratie**. 

    ![Database Migration Service-project maken](media/tutorial-mongodb-to-cosmosdb/dms-create-project.png)

5. Selecteer **Maken en uitvoeren van de activiteit** om het project te maken en de migratieactiviteit uit te voeren.

## <a name="specify-source-details"></a>Geef brondetails op

1. Geef in het scherm **Brondetails** de verbindingsgegevens op voor de MongoDB-bronserver.

    Er zijn drie modi om verbinding te maken met een bron:
   * **Standaardmodus**: deze accepteert een Fully Qualified Domain Name of een IP-adres, poortnummer en verbindingsreferenties.
   * **Modus verbindingsreeks**: deze accepteert een MongoDB-verbindingsreeks, zoals beschreven in het artikel [Connection String URI Format](https://docs.mongodb.com/manual/reference/connection-string/) (URI-indeling van verbindingsreeks).
   * **Gegevens uit Azure-opslag**: deze accepteert een SAS-URL van de blob-container. Selecteer **Blob contains BSON dump** als de blob-container BSON-dumps bevat die zijn geproduceerd door het [bsondump-hulpprogramma](https://docs.mongodb.com/manual/reference/program/bsondump/) van MongoDB en deselecteer het als de container JSON-bestanden bevat.

     Als u deze optie selecteert, controleer dan of de verbindingsreeks van het opslagaccount wordt weergegeven in de volgende indeling:

     ```
     https://blobnameurl/container?SASKEY
     ```

     Vanwege het type dumpgegevens in Azure-opslag moet u ook rekening houden met het volgende.

     * Voor BSON-dumps moeten de gegevens in de blob-container de bsondump-indeling hebben, zodat de gegevensbestanden worden geplaatst in mappen die worden genoemd naar de omvattende databases in de collection.bson-indeling. Metagegevensbestanden (indien aanwezig) moeten een naam krijgen op basis van de indeling *verzameling*.metadata.json.

     * Voor JSON-dumps moeten de bestanden in de blob-container worden geplaatst in mappen die zijn genoemd naar de omvattende databases. In elke databasemap moeten gegevensbestanden worden geplaatst in een submap met de naam 'data' en ze moeten een naam krijgen op basis van de indeling *verzameling*.json. Metagegevensbestanden (indien aanwezig) moeten worden geplaatst in een submap met de naam 'metadata' en ze moeten een naam krijgen op basis van dezelfde indeling *verzameling*.json. De metagegevensbestanden moeten de indeling hebben die wordt geproduceerd door het MongoDB-hulpprogramma bsondump.

    > [!IMPORTANT]
    > Het wordt afgeraden een zelfondertekend certificaat gebruiken op de mongo-server. Echter, als dit wordt gebruikt, maak verbinding met de server met **modus verbindingsreeks** en zorg ervoor dat de verbindingsreeks heeft ""
    >
    >```
    >&sslVerifyCertificate=false
    >```

   U kunt ook het IP-adres gebruiken voor situaties waarin DNS-naamomzetting niet mogelijk is.

   ![Geef brondetails op](media/tutorial-mongodb-to-cosmosdb/dms-specify-source.png)

2. Selecteer **Opslaan**.

## <a name="specify-target-details"></a>Doeldetails opgeven

1. Geef in het scherm **Details migratiedoel** de verbindingsgegevens op voor het Azure Cosmos DB-doelaccount, dat bestaat uit het vooraf ingerichte Azure Cosmos DB's API voor MongoDB-account waarnaar u de MongoDB-database migreert.

    ![Doeldetails opgeven](media/tutorial-mongodb-to-cosmosdb/dms-specify-target.png)

2. Selecteer **Opslaan**.

## <a name="map-to-target-databases"></a>Toewijzen aan doeldatabases

1. Wijs in het scherm **Toewijzen aan doeldatabases** de bron- en doeldatabase voor de migratie toe.

    Als de doel-database de naam van de dezelfde database als de bron-database bevat, selecteert Azure Database Migration Service de doeldatabase standaard.

    Als de tekenreeks **maken** wordt weergegeven naast de naam van de database, betekent dit dat Azure Database Migration Service de doeldatabase niet vinden en de service worden de database voor u gemaakt.

    Op dit moment in de migratie kunt u [doorvoer inrichten](https://docs.microsoft.com/azure/cosmos-db/set-throughput). In Cosmos DB kunt u doorvoer inrichten op het niveau van de database of afzonderlijk voor elke verzameling. Doorvoer wordt gemeten in [Aanvraageenheden](https://docs.microsoft.com/azure/cosmos-db/request-units) (RU's). Meer informatie over de [prijzen van Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/).

    ![Toewijzen aan doeldatabases](media/tutorial-mongodb-to-cosmosdb/dms-map-target-databases.png)

2. Selecteer **Opslaan**.
3. Vouw in het scherm **Verzamelingsinstelling** de lijst met verzamelingen uit en bekijk welke verzamelingen worden gemigreerd.

    Azure Database Migration Service automatisch selecteert alle verzamelingen die aanwezig zijn op de bron-MongoDB-exemplaar dat niet aanwezig zijn op de doel-Azure Cosmos DB-account. Als u verzamelingen die al gegevens bevatten opnieuw wilt migreren, moet u die verzamelingen expliciet selecteren op deze blade.

    U kunt opgeven hoeveel RU's u voor de verzamelingen wilt gebruiken. Azure Database Migration Service stelt voor slimme standaardinstellingen op basis van de grootte van de verzameling.

    > [!NOTE]
    > Voer de migratie van de database en verzameling parallel met behulp van meerdere exemplaren van Azure Database Migration Service, indien nodig, om de snelheid van de uitvoering.

    U kunt ook een shardsleutel opgeven om te profiteren van [partitionering in Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview) voor optimale schaalbaarheid. Lees de [best practices voor het selecteren van een shard-/partitiesleutel](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey).

    ![Verzamelingstabellen selecteren](media/tutorial-mongodb-to-cosmosdb/dms-collection-setting.png)

4. Selecteer **Opslaan**.

5. Geef in het tekstvak **Naam activiteit** van het scherm **Migratieoverzicht** een naam op voor de migratieactiviteit.

    ![Migratieoverzicht](media/tutorial-mongodb-to-cosmosdb/dms-migration-summary.png)

## <a name="run-the-migration"></a>De migratie uitvoeren

* Selecteer **Migratie uitvoeren**.

    Het venster van de migratieactiviteit wordt weergegeven en de **status** van de activiteit is **Niet gestart**.

    ![Activiteitsstatus](media/tutorial-mongodb-to-cosmosdb/dms-activity-status.png)

## <a name="monitor-the-migration"></a>Bewaak de migratie

* Selecteer op het scherm van de activiteitenmigratie **Vernieuwen** om de weergave bij te werken totdat de **Status** van de migratie als **Voltooid** wordt weergegeven.

   > [!NOTE]
   > U kunt de activiteit selecteren voor meer informatie over metrische migratiegegevens op het niveau van een database of verzameling.

    ![Activiteitsstatus Voltooid](media/tutorial-mongodb-to-cosmosdb/dms-activity-completed.png)

## <a name="verify-data-in-cosmos-db"></a>Gegevens controleren in Cosmos DB

* Nadat de migratie is voltooid, kunt u in uw Azure Cosmos DB-account controleren of alle verzamelingen met succes zijn gemigreerd.

    ![Activiteitsstatus Voltooid](media/tutorial-mongodb-to-cosmosdb/dms-cosmosdb-data-explorer.png)

## <a name="post-migration-optimization"></a>Na de migratie-optimalisatie

Nadat u de gegevens die zijn opgeslagen in de MongoDB-database naar Azure Cosmos DB-API voor MongoDB migreert, kunt u verbinding maken met Azure Cosmos DB en beheren van de gegevens. U kunt ook andere stappen na de migratie optimalisatie zoals de optimalisering van het indexeringsbeleid uitvoeren, het standaardconsistentieniveau bijwerken of globale distributie configureren voor uw Azure Cosmos DB-account. Zie voor meer informatie de [na de migratie optimalisatie](../cosmos-db/mongodb-post-migration.md) artikel.

## <a name="additional-resources"></a>Aanvullende resources

* [Informatie over Cosmos DB-service](https://azure.microsoft.com/services/cosmos-db/)

## <a name="next-steps"></a>Volgende stappen

* Lees de Microsoft-[handleiding voor databasemigratie](https://datamigration.microsoft.com/) voor hulp bij andere migratiescenario's.
