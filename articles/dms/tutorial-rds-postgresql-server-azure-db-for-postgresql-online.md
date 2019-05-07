---
title: 'Zelfstudie: Azure Database Migration Service gebruiken voor een online migratie van de PostgreSQL van extern bureaublad-services met Azure Database voor PostgreSQL | Microsoft Docs'
description: Leer hoe u een online migratie uitvoeren van extern bureaublad-services PostgreSQL met Azure Database for PostgreSQL met behulp van de Azure Database Migration Service.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 05/01/2019
ms.openlocfilehash: 3f1ab5c2cb30dd4067c07833529e6a6a0c71e286
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65136649"
---
# <a name="tutorial-migrate-rds-postgresql-to-azure-database-for-postgresql-online-using-dms"></a>Zelfstudie: PostgreSQL van extern bureaublad-services migreren naar Azure Database for PostgreSQL online met behulp van DMS

U kunt Azure Database Migration Service gebruiken voor het migreren van databases uit een exemplaar van de PostgreSQL van extern bureaublad-services aan [Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/) terwijl de brondatabase online tijdens de migratie blijft. Met andere woorden, kan migratie worden bereikt met minimale downtime voor de toepassing. In deze zelfstudie, migreert u de **DVD verhuur** voorbeelddatabase van een exemplaar van extern bureaublad-services PostgreSQL 9.6 met Azure Database voor PostgreSQL met behulp van de online migratie-activiteit in Azure Database Migration Service.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
>
> * De voorbeeldschema met behulp van het hulpprogramma pg_dump migreren.
> * Maak een instantie van Azure Database Migration Service.
> * Een migratieproject maken met behulp van Azure Database Migration Service.
> * De migratie uitvoeren.
> * De migratie controleren.

> [!NOTE]
> Als u Azure Database Migration Service gebruikt om een onlinemigratie uit te voeren, is het vereist dat u een exemplaar maakt op basis van de prijscategorie Premium. Zie de pagina met [prijzen](https://azure.microsoft.com/pricing/details/database-migration/) van Azure Database Migration Service voor meer informatie.

> [!IMPORTANT]
> Voor een optimale migratie-ervaring raadt Microsoft u aan een exemplaar van de Azure Database Migration Service te maken in dezelfde Azure-regio als de doeldatabase. Het verplaatsen van gegevens naar regio's of geografieën kan het migratieproces vertragen en fouten veroorzaken.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

In dit artikel wordt beschreven hoe u een online migratie uitvoeren van een on-premises exemplaar van de PostgreSQL met Azure Database voor PostgreSQL.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Download en installeer [PostgreSQL Community Edition](https://www.postgresql.org/download/) 9.5, 9.6 of 10. De bronversie van PostgreSQL Server moet versie 9.5.11, 9.6.7, 10 of hoger zijn. Zie voor meer informatie het artikel [Supported PostgreSQL Database Versions](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions) (Ondersteunde versies van de PostgreSQL-database).

    Bovendien hebben de extern bureaublad-services PostgreSQL-versie moet overeenkomen met de Azure Database for PostgreSQL-versie. Extern bureaublad-services PostgreSQL 9.5.11.5 kunt bijvoorbeeld alleen migreren naar Azure Database for PostgreSQL 9.5.11 en niet naar versie 9.6.7.

    > [!NOTE]
    > Voor PostgreSQL-versie 10 ondersteunt DMS momenteel alleen migratie van versie 10.3 met Azure Database voor PostgreSQL. We willen heel snel ondersteuning voor nieuwere versies van de PostgreSQL.

* Maak een instantie van [Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal). Raadpleeg dit [sectie](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal#connect-to-the-postgresql-server-using-pgadmin) van het document voor informatie over hoe u verbinding maakt met de PostgreSQL-Server met behulp van pgAdmin.
* Een Azure Virtual Network (VNet) maken voor Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel, waarmee u site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
* Zorg ervoor dat uw VNet netwerkbeveiligingsgroepsregels de volgende poorten voor binnenkomende communicatie naar Azure Database Migration Service niet blokkeren: 443, 53, 9354, 445 en 12000. Zie het artikel voor meer informatie over Azure VNet NSG wordt verkeer gefilterd, [netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
* Configureer uw [Windows Firewall voor toegang tot de database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Open uw Windows-firewall voor Azure Database Migration Service toegang tot de bron PostgreSQL-server, die standaard TCP-5432 poort.
* Wanneer u een firewallapparaat gebruikt voor de brondatabase(s), moet u mogelijk firewallregels toevoegen om voor de Azure Database Migration Service toegang tot de brondatabase(s) voor de migratie toe te staan.
* Maken van een op serverniveau [firewallregel](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voor de Azure Database for PostgreSQL-server voor Azure Database Migration Service toegang tot de doeldatabase. Geef het subnetbereik van het VNet gebruikt voor Azure Database Migration Service.

### <a name="set-up-aws-rds-postgresql-for-replication"></a>AWS RDS PostgreSQL ingesteld voor replicatie

1. Voor het maken van een nieuwe parametergroep, volg de instructies die worden geleverd door AWS in het artikel [werken met DB Parameter groepen](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithParamGroups.html).
2. Gebruik de naam van de hoofdgebruiker verbinding maken met de bron van Azure Database Migration Service. Als u een ander account dan het hoofd gebruikersaccount gebruikt, moet het account de rol rds_superuser en de rol rds_replication hebben. De rol rds_replication verleent machtigingen voor het beheren van logische sleuven en om gegevens te streamen met behulp van de logische sleuven.
3. Maak een nieuwe parametergroep met de volgende configuratie: een. Stel de parameter rds.logical_replication in uw groep DB parameter 1.
    b. max_wal_senders = [aantal gelijktijdige taken] - Hiermee stelt u de parameter max_wal_senders het aantal gelijktijdige taken die kunnen worden uitgevoerd, kunt het beste 10 taken.
    c. max_replication_slots: = [aantal sleuven], aanbevolen is ingesteld op 5 sleuven.
4. De parametergroep die u hebt gemaakt met de PostgreSQL van extern bureaublad-services-exemplaar koppelen.

## <a name="migrate-the-schema"></a>Het schema migreren

1. Het schema ophalen uit de brondatabase en van toepassing op de doeldatabase om de migratie van alle objecten in de database, zoals tabelschema, indexen en opgeslagen procedures te voltooien.

    De eenvoudigste manier voor het migreren van alleen het schema is pg_dump gebruiken met de optie -s. Zie voor meer informatie de [voorbeelden](https://www.postgresql.org/docs/9.6/app-pgdump.html#PG-DUMP-EXAMPLES) in de Postgres pg_dump zelfstudie.
    
    ```
    pg_dump -o -h hostname -U db_username -d db_name -s > your_schema.sql
    ```
    
    Om bijvoorbeeld te dumpbestand van een schema voor de **dvdrental** database, gebruikt u de volgende opdracht uit:
    
    ```
    pg_dump -o -h localhost -U postgres -d dvdrental -s  > dvdrentalSchema.sql
    ```

2. Een lege database maken in de doelservice, Azure Database for PostgreSQL. Verbinding maken en een database maken, verwijzen naar een van de volgende artikelen:

    * [Een Azure Database for PostgreSQL-server maken met behulp van Azure portal](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal)
    * [Een Azure Database for PostgreSQL maken met Azure CLI](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-azure-cli)

3. Importeer het schema in target-service, Azure Database for PostgreSQL. Als u wilt herstellen het dumpbestand schema, moet u de volgende opdracht uitvoeren:

    ```
    psql -h hostname -U db_username -d db_name < your_schema.sql
    ```

    Bijvoorbeeld:

    ```
    psql -h mypgserver-20170401.postgres.database.azure.com  -U postgres -d dvdrental < dvdrentalSchema.sql
    ```

4. Als u refererende sleutels in uw schema hebt, mislukken de eerste lading en doorlopende synchronisatie van de migratie. Uitpakken van het script drop refererende sleutel en refererende sleutels script op de bestemming (Azure Database for PostgreSQL), voer het volgende script in PgAdmin of psql toevoegen:

    ```
    SET group_concat_max_len = 8192;
        SELECT SchemaName, GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery, GROUP_CONCAT(AddQuery SEPARATOR ';\n') as AddQuery
        FROM
        (SELECT 
        KCU.REFERENCED_TABLE_SCHEMA as SchemaName,    
        KCU.TABLE_NAME,
        KCU.COLUMN_NAME,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' DROP FOREIGN KEY ', KCU.CONSTRAINT_NAME) AS DropQuery,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' ADD CONSTRAINT ', KCU.CONSTRAINT_NAME, ' FOREIGN KEY (`', KCU.COLUMN_NAME, '`) REFERENCES `', KCU.REFERENCED_TABLE_NAME, '` (`', KCU.REFERENCED_COLUMN_NAME, '`) ON UPDATE ',RC.UPDATE_RULE, ' ON DELETE ',RC.DELETE_RULE) AS AddQuery
        FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE KCU, information_schema.REFERENTIAL_CONSTRAINTS RC
        WHERE
          KCU.CONSTRAINT_NAME = RC.CONSTRAINT_NAME
          AND KCU.REFERENCED_TABLE_SCHEMA = RC.UNIQUE_CONSTRAINT_SCHEMA
      AND KCU.REFERENCED_TABLE_SCHEMA = 'sakila') Queries
      GROUP BY SchemaName;
    ```
        
5. Voer de drop refererende sleutel (dit is de tweede kolom) in het queryresultaat verwijderen van de refererende sleutel.

6. Als u triggers (insert of update trigger) in de gegevens hebt, wordt deze de integriteit van gegevens in de doel-afdwingen voordat het repliceren van gegevens van de bron. De aanbeveling is om uit te schakelen van triggers in alle tabellen *op de doelopslaglocatie* tijdens de migratie en schakelt u vervolgens de triggers nadat de migratie is voltooid.

    Triggers in doeldatabase uitschakelen:

    ```
    SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
    ```

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registreer de Microsoft.DataMigration-resourceprovider

1. Meld u aan bij de Azure-portal, selecteer **Alle services** en selecteer vervolgens **Abonnementen**.

   ![Portal-abonnementen weergeven](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/portal-select-subscription1.png)

2. Selecteer het abonnement waarin u wilt maken van het exemplaar van Azure Database Migration Service, en selecteer vervolgens **resourceproviders**.

    ![Resourceproviders weergeven](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/portal-select-resource-provider.png)

3. Zoek naar migratie en selecteer rechts van **Microsoft.DataMigration** de optie **Registreren**.

    ![Resourceprovider registreren](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/portal-register-resource-provider.png)

## <a name="create-an-instance-of-azure-database-migration-service"></a>Maak een instantie van Azure Database Migration Service

1. Selecteer in de Azure-portal **Een resource maken**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service** uit de vervolgkeuzelijst.

    ![Azure Marketplace](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/portal-marketplace.png)

2. Selecteer in het scherm **Azure Database Migration Service** **Maken**.

    ![Azure Database Migration Service-exemplaar maken](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-create1.png)
  
3. Geef in het scherm **Migratieservice maken** een naam op voor de service, het abonnement en een nieuwe of bestaande resourcegroep.

4. Selecteer de locatie waarin u wilt maken van het exemplaar van Azure Database Migration Service.

5. Selecteer een bestaand VNet of maak een nieuwe.

    Het VNet biedt Azure Database Migration Service toegang tot de bron-PostgreSQL-exemplaar en de doel-Azure Database voor PostgreSQL-exemplaar.

    Zie het artikel voor meer informatie over het maken van een VNet in Azure portal [een virtueel netwerk maken met de Azure-portal](https://aka.ms/DMSVnet).

6. Selecteer een prijscategorie; Zorg dat u selecteert u de Premium voor deze online migratie: 4vCores prijscategorie.

    ![Instellingen configureren van een Azure Database Migration Service-exemplaar](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-settings4.png)

7. Selecteer **Maken** om de dienst te maken.

## <a name="create-a-migration-project"></a>Maak een migratieproject

Nadat de service is gemaakt, zoek deze op in de Azure-portal, open hem en maak vervolgens een nieuw migratieproject.

1. Selecteer in de Azure-portal **Alle diensten**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service**.

      ![Zoek alle exemplaren van de Azure Database Migration Service](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-search.png)

2. Zoek in het scherm **Azure Database Migration service** naar de naam van de Azure Database Migration Service-instantie die u hebt gemaakt en selecteer vervolgens het exemplaar.

     ![Zoek uw exemplaar van de Azure Database Migration Service](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-instance-search.png)

3. Selecteer + **Nieuw migratieproject**.
4. Op de **nieuw migratieproject** scherm, Geef een naam voor het project in de **bronservertype** tekstvak, selecteer **AWS RDS voor PostgreSQL**, en klik vervolgens in de **Doelservertype** tekstvak, selecteer **Azure Database for PostgreSQL**.
5. Selecteer in de sectie **Het type activiteit kiezen** de optie **Onlinegegevensmigratie**.

    > [!IMPORTANT]
    > Zorg ervoor dat u selecteert **Online gegevensmigratie**; offline migraties worden niet ondersteund voor dit scenario.

    ![Azure Database Migration Service-project maken](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-create-project5.png)

    > [!NOTE]
    > U kunt ook **alleen maken in project** nu het migratieproject maken en uitvoeren van de migratie later opnieuw.

6. Selecteer **Opslaan**.

7. Selecteer **Maken en uitvoeren van de activiteit** om het project te maken en de migratieactiviteit uit te voeren.

    > [!NOTE]
    > Controleer of u een notitie van de vereisten die nodig zijn voor het instellen van online migratie in de blade voor het maken van project.

## <a name="specify-source-details"></a>Geef brondetails op

* Op de **migratiebron** scherm, geef de details van de verbinding voor de bron-PostgreSQL-exemplaar.

   ![Brondetails](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-source-details4.png)

## <a name="specify-target-details"></a>Doeldetails opgeven

1. Selecteer **opslaan**, en klik vervolgens op de **gericht op details** scherm, geef de details van de verbinding voor de doel-Azure Database for PostgreSQL-server, die vooraf is ingericht en is de **DVD Huur** geïmplementeerd met behulp van pg_dump schema.

    ![Doel selecteren](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-select-target4.png)

2. Selecteer **Opslaan**, en klik vervolgens in het scherm **Toewijzen aan doeldatabases**, wijs de bron- en de doeldatabase voor de migratie toe.

    Als de doel-database de naam van de dezelfde database als de bron-database bevat, selecteert Azure Database Migration Service de doeldatabase standaard.

    ![Toewijzen aan doeldatabases](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-map-targets-activity5.png)

3. Selecteer **Opslaan**, geef in het scherm **Migratieoverzicht** in het tekstvak **Naam activiteit** een naam op voor de migratieactiviteit en controleer het overzicht om ervoor te zorgen dat de bron- en doeldetails overeenkomen met wat u eerder hebt opgegeven.

    ![Migratieoverzicht](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-migration-summary1.png)

## <a name="run-the-migration"></a>De migratie uitvoeren

* Selecteer **Migratie uitvoeren**.

    Het venster van de migratieactiviteit wordt weergegeven en de **Status** van de activiteit is **Initialiseren**.

## <a name="monitor-the-migration"></a>Bewaak de migratie

1. Selecteer in het scherm van de activiteitenmigratie **Vernieuwen** om de weergave bij te werken totdat de **Status** van de migratie als **Actief** wordt weergegeven.

    ![De Status van de activiteit - uitgevoerd](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-activity-status3.png)

2. Onder **DATABASENAAM**, selecteert u een specifieke database om te gaan naar de migratiestatus van voor **alle gegevens worden geladen** en **incrementele gegevenssynchronisatie** bewerkingen.

    **Alle gegevens worden geladen** toont de status van de migratie laden terwijl **incrementele gegevenssynchronisatie** toont gegevens vastleggen (CDC) status wijzigen.

    ![Scherm van de inventaris - volledige gegevens laden](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-inventory-full-load.png)

    ![Inventaris scherm - incrementele gegevenssynchronisatie](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-inventory-incremental.png)

## <a name="perform-migration-cutover"></a>Migratie-cutover uitvoeren

Nadat de initiële volledig laden is voltooid, de databases zijn gemarkeerd **gereed voor Cutover**.

1. Wanneer u klaar bent om de databasemigratie te voltooien, selecteert u **Cutover starten**.

    ![Knippen opnieuw beginnen](media/tutorial-rds-postgresql-server-azure-db-for-postgresql-online/dms-inventory-start-cutover.png)
 
2. Zorg dat u alle transacties stopt die bij de brondatabase binnenkomen; wacht totdat de teller van **Wijzigingen in behandeling** op **0** staat.
3. Selecteer **Bevestigen** en selecteer vervolgens **Toepassen**.
4. Wanneer de status van de database-migratie voor de status **voltooid**, verbinding maken met uw toepassingen naar de nieuwe doel-Azure Database for PostgreSQL-database.

Uw online migratie van een on-premises exemplaar van de PostgreSQL met Azure Database for PostgreSQL is nu voltooid.

## <a name="next-steps"></a>Volgende stappen

- Zie het artikel [Wat is de Azure Database Migration Service?](https://docs.microsoft.com/azure/dms/dms-overview) voor informatie over de Azure Database Migration Service.
- Zie het artikel voor informatie over Azure Database for PostgreSQL, [wat is Azure Database for PostgreSQL?](https://docs.microsoft.com/azure/postgresql/overview).
- Voor andere vragen, e-de [Azure Databasemigraties vragen](mailto:AskAzureDatabaseMigrations@service.microsoft.com) alias.
