---
title: 'Zelfstudie: Azure Database Migration Service gebruiken voor het uitvoeren van een online migratie van Oracle naar Azure Database for PostgreSQL | Microsoft Docs'
description: Leer hoe u een online migratie uitvoeren van Oracle on-premises of op virtuele machines met Azure Database for PostgreSQL met behulp van Azure Database Migration Service.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: b73249a9f72e4616f6d36e16f110913278f04590
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415604"
---
# <a name="tutorial-migrate-oracle-to-azure-database-for-postgresql-online-using-dms-preview"></a>Zelfstudie: Migreren van Oracle naar Azure Database for PostgreSQL online met behulp van DMS (Preview)

U kunt Azure Database Migration Service gebruiken voor het migreren van de databases van Oracle-databases die worden gehost on-premises of op virtuele machines om [Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/) met minimale downtime. Met andere woorden, kunt u de migratie met minimale downtime voor de toepassing uitvoeren. In deze zelfstudie, migreert u de **HR** voorbeelddatabase van een on-premises of virtuele machine-exemplaar van Oracle 11 g met Azure Database voor PostgreSQL met behulp van de online migratie-activiteit in Azure Database Migration Service.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * De migratie zonder veel moeite met het hulpprogramma ora2pg te beoordelen.
> * De voorbeeldschema met behulp van het hulpprogramma ora2pg migreren.
> * Maak een instantie van Azure Database Migration Service.
> * Een migratieproject maken met behulp van Azure Database Migration Service.
> * De migratie uitvoeren.
> * De migratie controleren.

> [!NOTE]
> Met Azure Database Migration Service voor het uitvoeren van een online migratie vereist het maken van een exemplaar op basis van de prijscategorie Premium.

> [!IMPORTANT]
> Voor een optimale migratie-ervaring wordt aangeraden het maken van een exemplaar van Azure Database Migration Service in dezelfde Azure-regio als de doeldatabase. Het verplaatsen van gegevens naar regio's of geografieën kan het migratieproces vertragen en fouten veroorzaken.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

In dit artikel wordt beschreven hoe u een online migratie uitvoeren van Oracle naar Azure Database voor PostgreSQL.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Download en installeer [Oracle 11g versie 2 (Standard Edition, Standard Edition One of Enterprise Edition)](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html).
* Het voorbeeld downloaden **HR** uit de database [hier](https://docs.oracle.com/database/121/COMSC/installation.htm#COMSC00002).
* Download en installeer ora2pg op een [Windows](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Steps%20to%20Install%20ora2pg%20on%20Windows.pdf) of [Linux](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Steps%20to%20Install%20ora2pg%20on%20Linux.pdf).
* [Een exemplaar maken in Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/quickstart-create-server-database-portal).
* Verbinding maken met het exemplaar en een database maken met de instructies in deze [document](https://docs.microsoft.com/azure/postgresql/tutorial-design-database-using-azure-portal).
* Een Azure Virtual Network (VNet) maken voor Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel, waarmee u site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Zie voor meer informatie over het maken van een VNet, de [documentatie voor Virtual Network](https://docs.microsoft.com/azure/virtual-network/), en met name de artikelen met vindt u meer details.

  > [!NOTE]
  > Tijdens de installatie van de VNet, als u ExpressRoute gebruikt met het naar Microsoft-netwerkpeering, voeg de volgende service [eindpunten](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) aan het subnet waarin de service worden ingericht:
  > * Doel-database-eindpunt (bijvoorbeeld SQL-eindpunt, Cosmos DB-eindpunt, enzovoort)
  > * Opslageindpunt
  > * Service bus-eindpunt
  >
  > Deze configuratie is nodig omdat Azure Database Migration Service beschikt niet over de verbinding met internet.

* Zorg ervoor dat uw regels VNet Netwerkbeveiligingsgroep (NSG) blokkeren niet de volgende poorten voor binnenkomende communicatie naar Azure Database Migration Service: 443, 53, 9354, 445, 12000. Zie het artikel voor meer informatie over Azure VNet NSG wordt verkeer gefilterd, [netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm).
* Configureer uw [Windows Firewall voor toegang tot de database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Uw Windows-firewall voor Azure Database Migration Service toegang tot de bron Oracle-server, die standaard TCP-poort 1521 wordt geopend.
* Wanneer u een apparaat voor een firewall voor de brondatabase (s), moet u mogelijk toevoegen van firewallregels om toe te staan van Azure Database Migration Service voor toegang tot de brondatabase (s) voor de migratie.
* Maken van een op serverniveau [firewallregel](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voor Azure Database for PostgreSQL voor Azure Database Migration Service toegang tot de doeldatabase. Geef het subnetbereik van het VNet gebruikt voor Azure Database Migration Service.
* Toegang tot de bron Oracle-databases inschakelen.

  > [!NOTE]
  > De DBA-rol is vereist voor een gebruiker verbinding maken met de Oracle-bron.

  * Archief opnieuw Logboeken is vereist voor incrementele synchronisatie in Azure Database Migration Service voor het vastleggen van gegevens. Volg deze stappen voor het configureren van de Oracle-bron:
    * Meld u aan met behulp van SYSDBA bevoegdheden door het uitvoeren van de volgende opdracht uit:

      ```
      sqlplus (user)/(password) as sysdba
      ```

    * Het database-exemplaar afgesloten met de volgende opdracht.

      ```
      SHUTDOWN IMMEDIATE;
      ```

      Wachten op de bevestiging `'ORACLE instance shut down'`.

    * Start de nieuwe instantie en koppelpunten (maar niet openen) op de database archiveren bu met de volgende opdracht uit te schakelen:

      ```
      STARTUP MOUNT;
      ```

      De database moet worden gekoppeld; Wacht op bevestiging 'Oracle-exemplaar aan de slag'.

    * Wijzigen van de database archiveren modus met de volgende opdracht:

      ```
      ALTER DATABASE ARCHIVELOG;
      ```

    * Openen van de database voor normale bewerkingen met de volgende opdracht:

      ```
      ALTER DATABASE OPEN;
      ```

      U moet mogelijk opnieuw opstarten van het bestand BOOG worden weergegeven.

    * Als u wilt controleren, moet u de volgende opdracht uitvoeren:

      ```
      SELECT log_mode FROM v$database;
      ```

      U ontvangt een reactie `'ARCHIVELOG'`. Als het antwoord is `'NOARCHIVELOG'`, en vervolgens niet aan de vereiste wordt voldaan.

  * Aanvullende logboekregistratie voor replicatie met behulp van een van de volgende opties inschakelen

    * **Optie 1**.
      De database aanvullende logboekregistratie op voor de tabellen met PK en unieke index wijzigen. Retourneert de query detectie `'IMPLICIT'`.

      ```
      ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (PRIMARY KEY, UNIQUE) COLUMNS;
      ```

      Wijzig het tabelniveau van de aanvullende logboekregistratie. Alleen voor tabellen die gegevens manipuleren en geen PKs of unieke indexen worden uitgevoerd.

      ```
      ALTER TABLE [TABLENAME] ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;
      ```

    * **Optie 2**.
      Wijzigen van de database aanvullende logboekregistratie op aan zodat alle tabellen en de detectie-query retourneert `'YES'`.

      ```
      ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
      ```

      Wijzig het tabelniveau van de aanvullende logboekregistratie. Volg de onderstaande om uit te voeren van slechts één instructie voor elke tabel logica.

      Als de tabel een primaire sleutel heeft:

      ```
      ALTER TABLE xxx ADD SUPPLEMENTAL LOG DATA (PRIMARY KEY) COLUMNS;
      ```

      Als de tabel een unieke index heeft:

      ```
      ALTER TABLE xxx ADD SUPPLEMENTAL LOG GROUP (first unique index columns) ALWAYS;
      ```

      Voer anders de volgende opdracht:

      ```
      ALTER TABLE xxx ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;
      ```

    Als u wilt controleren, moet u de volgende opdracht uitvoeren:

      ```
      SELECT supplemental_log_data_min FROM v$database;
      ```

    U ontvangt een reactie `'YES'`.

> [!IMPORTANT]
> Voor de openbare preview-versie van dit scenario, Azure Database Migration Service biedt ondersteuning voor Oracle-versie 10g of 11g. Klanten met Oracle-versie 12 c of hoger Houd er rekening mee dat moet het minimum-verificatieprotocol dat is toegestaan voor ODBC-stuurprogramma verbinding maken met Oracle 8 zijn. Voor een Oracle-bron is die versie 12c of hoger, moet u het verificatieprotocol als volgt configureren:
>
> * SQLNET bijwerken. ORA:
>
>    ```
>    SQLNET.ALLOWED_LOGON_VERSION_CLIENT = 8
>    SQLNET.ALLOWED_LOGON_VERSION_SERVER = 8
>    ```
>
> * Start de computer voor de nieuwe instellingen toe te passen.
> * Wijzig het wachtwoord voor bestaande gebruikers:
>
>    ```
>    ALTER USER system IDENTIFIED BY {pswd}
>    ```
>
>   Zie voor meer informatie de pagina [hier](http://www.dba-oracle.com/t_allowed_login_version_server.htm).
>
> Let op dat het wijzigen van het verificatieprotocol clientverificatie kan beïnvloeden.

## <a name="assess-the-effort-for-an-oracle-to-azure-database-for-postgresql-migration"></a>De inspanningen voor een Oracle naar Azure Database voor PostgreSQL-migratie beoordelen

We adviseren ora2pg gebruiken om het werk dat nodig is om te migreren van Oracle naar Azure Database for PostgreSQL vast te stellen. Gebruik de `ora2pg -t SHOW_REPORT` -richtlijn voor het maken van een overzicht van alle objecten van de Oracle, de migratie van de geschatte kosten (in dagen van de ontwikkelaar) en bepaalde databaseobjecten die speciale aandacht nodig als onderdeel van de conversie hebben.

De meeste klanten zullen een aanzienlijke hoeveelheid tijd controleren van het evaluatierapport en overweegt de automatische en handmatige conversie zonder veel moeite besteden.

Als u wilt configureren en uitvoeren van ora2pg voor het maken van een rapport over de beoordeling, Zie de **voor de migratie: Evaluatie** sectie van de [Oracle met Azure Database for PostgreSQL Cookbook](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20Azure%20PostgreSQL%20Migration%20Cookbook.pdf). Een voorbeeld ora2pg-evaluatierapport is beschikbaar voor verwijzing [hier](http://ora2pg.darold.net/report.html).

## <a name="export-the-oracle-schema"></a>Het Oracle-schema exporteren

U wordt aangeraden dat u het schema Oracle en andere Oracle-objecten (typen, procedures, functies, enzovoort) converteren naar een schema dat compatibel is met Azure Database for PostgreSQL ora2pg gebruiken. ora2pg bevat veel richtlijnen kunt u vooraf bepaalde gegevenstypen definiëren. Bijvoorbeeld, kunt u de `DATA_TYPE` -richtlijn voor het vervangen van alle NUMBER(*,0) met bigint in plaats van NUMERIC(38).

U kunt ora2pg voor het exporteren van elk van de database-objecten in .sql-bestanden uitvoeren. U kunt vervolgens de .sql-bestanden bekijken voordat u met Azure Database for PostgreSQL met behulp van psql importeren of kunt u uitvoeren de. SQL-script in PgAdmin.

```
psql -f [FILENAME] -h [AzurePostgreConnection] -p 5432 -U [AzurePostgreUser] -d database 
```

Bijvoorbeeld:

```
psql -f %namespace%\schema\sequences\sequence.sql -h server1-server.postgres.database.azure.com -p 5432 -U username@server1-server -d database
```

Als u wilt configureren en ora2pg voor schemaconversie uitvoeren, Zie de **migratie: Schema en gegevens** sectie van de [Oracle met Azure Database for PostgreSQL Cookbook](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20Azure%20PostgreSQL%20Migration%20Cookbook.pdf).

## <a name="set-up-the-schema-in-azure-database-for-postgresql"></a>Instellen van het schema in Azure Database for PostgreSQL

Standaard houdt Oracle de schema.table.column in alle gevallen van hoofdletters, terwijl PostgreSQL schema.table.column in kleine letters blijft. Azure Database Migration Service om te beginnen verplaatsing van gegevens van Oracle met Azure Database voor PostgreSQL, moet de schema.table.column case dezelfde indeling als de Oracle-bron.

Bijvoorbeeld, als de Oracle-bron heeft als het schema van "HR"." WERKNEMERS'.' Werknemer', en vervolgens de PostgreSQL-schema moet dezelfde indeling gebruiken.

Om ervoor te zorgen dat de hoofdletters en kleine letters van de schema.table.column hetzelfde zijn voor Oracle- en Azure Database for PostgreSQL is, raden wij aan dat u de volgende stappen uit.

> [!NOTE]
> U kunt een andere benadering voor het afleiden van het schema hoofdletters. We werken om te verbeteren en het automatiseren van deze stap.

1. Schema's met ora2pg met kleine aanvragen exporteren. In de tabel maken van het sql-script maakt u een schema met hoofdletters 'SCHEMA' handmatig.
2. Importeer de rest van de Oracle-objecten, zoals triggers en -reeksen, procedures, typen en functies in Azure Database for PostgreSQL.
3. Als u de tabel en kolom bovenste aanvraag, voer het volgende script:

   ```
   -- INPUT: schema name
   set schema.var = “HR”;

   -- Generate statements to rename tables and columns
   SELECT 1, 'SET search_path = "' ||current_setting('schema.var')||'";'
   UNION ALL 
   SELECT 2, 'alter table "'||c.relname||'" rename '||a.attname||' to "'||upper(a.attname)||'";'
   FROM pg_class c
   JOIN pg_attribute a ON a.attrelid = c.oid
   JOIN pg_type t ON a.atttypid = t.oid
   LEFT JOIN pg_catalog.pg_constraint r ON c.oid = r.conrelid
    AND r.conname = a.attname
   WHERE c.relnamespace = (select oid from pg_namespace where nspname=current_setting('schema.var')) AND a.attnum > 0 AND c.relkind ='r'
   UNION ALL
   SELECT 3, 'alter table '||c.relname||' rename to "'||upper(c.relname)||'";'
   FROM pg_catalog.pg_class c
    LEFT JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
   WHERE c.relkind ='r' AND n.nspname=current_setting('schema.var')
   ORDER BY 1;
   ```

* De refererende sleutel verwijderen in de doeldatabase voor de volledige belasting om uit te voeren. Raadpleeg de **de voorbeeldschema migreren** gedeelte van het artikel [hier](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online) voor een script dat u gebruiken kunt voor het verwijderen van de refererende sleutel.
* Azure Database Migration Service gebruiken om uit te voeren voor volledig laden en sync.
* Wanneer de gegevens in de doel-Azure Database voor PostgreSQL-exemplaar is bijgewerkt met de bron, database in Azure Database Migration Service cutover uitvoeren.
* Om te maken van SCHEMA-, tabel- en kleine letters (als het schema voor Azure Database for PostgreSQL deze manier voor query van toepassing moeten), voer het volgende script:

  ```
  -- INPUT: schema name
  set schema.var = hr;
  
  -- Generate statements to rename tables and columns
  SELECT 1, 'SET search_path = "' ||current_setting('schema.var')||'";'
  UNION ALL
  SELECT 2, 'alter table "'||c.relname||'" rename "'||a.attname||'" to '||lower(a.attname)||';'
  FROM pg_class c
  JOIN pg_attribute a ON a.attrelid = c.oid
  JOIN pg_type t ON a.atttypid = t.oid
  LEFT JOIN pg_catalog.pg_constraint r ON c.oid = r.conrelid
     AND r.conname = a.attname
  WHERE c.relnamespace = (select oid from pg_namespace where nspname=current_setting('schema.var')) AND a.attnum > 0 AND c.relkind ='r'
  UNION ALL
  SELECT 3, 'alter table "'||c.relname||'" rename to '||lower(c.relname)||';'
  FROM pg_catalog.pg_class c
     LEFT JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
  WHERE c.relkind ='r' AND n.nspname=current_setting('schema.var')
  ORDER BY 1;
  ```

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registreer de Microsoft.DataMigration-resourceprovider

1. Meld u aan bij de Azure-portal, selecteer **Alle services** en selecteer vervolgens **Abonnementen**.

   ![Portal-abonnementen weergeven](media/tutorial-oracle-azure-postgresql-online/portal-select-subscriptions.png)

2. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken en selecteer vervolgens **Resourceproviders**.

    ![Resourceproviders weergeven](media/tutorial-oracle-azure-postgresql-online/portal-select-resource-provider.png)

3. Zoek naar migratie en selecteer rechts van **Microsoft.DataMigration** de optie **Registreren**.

    ![Resourceprovider registreren](media/tutorial-oracle-azure-postgresql-online/portal-register-resource-provider.png)

## <a name="create-a-dms-instance"></a>Een DMS-exemplaar maken

1. Selecteer in de Azure-portal **Een resource maken**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service** uit de vervolgkeuzelijst.

    ![Azure Marketplace](media/tutorial-oracle-azure-postgresql-online/portal-marketplace.png)

2. Selecteer in het scherm **Azure Database Migration Service** **Maken**.

    ![Azure Database Migration Service-exemplaar maken](media/tutorial-oracle-azure-postgresql-online/dms-create2.png)
  
3. Geef in het scherm **Migratieservice maken** een naam op voor de service, het abonnement en een nieuwe of bestaande resourcegroep.

4. Selecteer een bestaand VNet of maak een nieuwe.

    Het VNet biedt Azure Database Migration Service toegang tot de bron Oracle en de doel-Azure Database voor PostgreSQL-instantie.

    Zie het artikel voor meer informatie over het maken van een VNet in Azure portal [een virtueel netwerk maken met de Azure-portal](https://aka.ms/DMSVnet).

5. Selecteer een prijscategorie.

    Zie voor meer informatie over de kosten en prijscategorieën de [Pagina met prijzen](https://aka.ms/dms-pricing).

    ![Instellingen configureren van een Azure Database Migration Service-exemplaar](media/tutorial-oracle-azure-postgresql-online/dms-settings5.png)

6. Selecteer **Maken** om de dienst te maken.

## <a name="create-a-migration-project"></a>Maak een migratieproject

Nadat de service is gemaakt, zoek deze op in de Azure-portal, open hem en maak vervolgens een nieuw migratieproject.

1. Selecteer in de Azure-portal **Alle diensten**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service**.

    ![Zoek alle exemplaren van de Azure Database Migration Service](media/tutorial-oracle-azure-postgresql-online/dms-search.png)

2. Zoek in het scherm **Azure Database Migration service** naar de naam van de Azure Database Migration Service-instantie die u hebt gemaakt en selecteer vervolgens het exemplaar.

    ![Zoek uw exemplaar van de Azure Database Migration Service](media/tutorial-oracle-azure-postgresql-online/dms-instance-search.png)

3. Selecteer + **Nieuw migratieproject**.
4. Op de **nieuw migratieproject** scherm, Geef een naam voor het project in de **bronservertype** tekstvak, selecteer **Oracle**, in de **doelservertype**  tekstvak, selecteer **Azure Database for PostgreSQL**.
5. Selecteer in de sectie **Het type activiteit kiezen** de optie **Onlinegegevensmigratie**.

   ![Azure Database Migration Service-project maken](media/tutorial-oracle-azure-postgresql-online/dms-create-project5.png)

   > [!NOTE]
   > U kunt ook **alleen maken in project** nu het migratieproject maken en uitvoeren van de migratie later opnieuw.

6. Selecteer **opslaan**, houd er rekening mee de vereisten voor het gebruik van Azure Database Migration Service is moet een online migratie uit te voeren, en selecteer vervolgens **maken en de uitvoering van de activiteit**.

## <a name="specify-source-details"></a>Geef brondetails op

* Op de **Brondetails toevoegen** scherm, geef de details van de verbinding voor de bron-Oracle-exemplaar.

  ![Scherm Brondetails toevoegen](media/tutorial-oracle-azure-postgresql-online/dms-add-source-details.png)

## <a name="upload-oracle-oci-driver"></a>Oracle OCI stuurprogramma uploaden

1. Selecteer **opslaan**, en klik vervolgens op de **OCI installeren stuurprogramma** scherm, meld u aan bij uw account Oracle en download het stuurprogramma **instantclient-basiclite-windows.x64-12.2.0.1.0.zip**(37,128,586 Byte(s)) (SHA1 controlesom: 865082268) van [hier](https://www.oracle.com/technetwork/topics/winx64soft-089540.html#ic_winx64_inst).
2. Download het stuurprogramma naar een gedeelde map.

   Zorg ervoor dat de map wordt gedeeld met de gebruikersnaam die u hebt opgegeven met ten minste alleen-lezen toegang. Azure Database Migration Service toegang heeft tot en leest van de share voor het stuurprogramma OCI uploaden naar Azure door te imiteren van de gebruikersnaam die u opgeeft.

   De gebruikersnaam die u opgeeft moet een Windows-gebruikersaccount.

   ![OCI stuurprogramma installeren](media/tutorial-oracle-azure-postgresql-online/dms-oci-driver-install.png)

## <a name="specify-target-details"></a>Doeldetails opgeven

1. Selecteer **opslaan**, en klik vervolgens op de **gericht op details** scherm, geef de details van de verbinding voor de doel-Azure Database for PostgreSQL-server, is het vooraf ingerichte exemplaar van Azure Database for PostgreSQL waaraan de **HR** schema is geïmplementeerd.

    ![Scherm Doeldetails](media/tutorial-oracle-azure-postgresql-online/dms-add-target-details1.png)

2. Selecteer **Opslaan**, en klik vervolgens in het scherm **Toewijzen aan doeldatabases**, wijs de bron- en de doeldatabase voor de migratie toe.

    Als de doel-database de naam van de dezelfde database als de bron-database bevat, selecteert Azure Database Migration Service de doeldatabase standaard.

    ![Toewijzen aan doeldatabases](media/tutorial-oracle-azure-postgresql-online/dms-map-target-details.png)

3. Selecteer **Opslaan**, geef in het scherm **Migratieoverzicht** in het tekstvak **Naam activiteit** een naam op voor de migratieactiviteit en controleer het overzicht om ervoor te zorgen dat de bron- en doeldetails overeenkomen met wat u eerder hebt opgegeven.

    ![Migratieoverzicht](media/tutorial-oracle-azure-postgresql-online/dms-migration-summary.png)

## <a name="run-the-migration"></a>De migratie uitvoeren

* Selecteer **Migratie uitvoeren**.

  Het venster van de migratieactiviteit wordt weergegeven en de **Status** van de activiteit is **Initialiseren**.

## <a name="monitor-the-migration"></a>Bewaak de migratie

1. Selecteer in het scherm van de activiteitenmigratie **Vernieuwen** om de weergave bij te werken totdat de **Status** van de migratie als **Actief** wordt weergegeven.

     ![De Status van de activiteit - uitgevoerd](media/tutorial-oracle-azure-postgresql-online/dms-activity-running.png)

2. Onder **databasenaam**, selecteert u een specifieke database om te gaan naar de migratiestatus van voor **alle gegevens worden geladen** en **incrementele gegevenssynchronisatie** bewerkingen.

    ‘Alle gegevens worden geladen’ toont de migratiestatus van de eerste lading terwijl ‘Incrementele gegevenssynchronisatie’ de CDC-status (Change Data Capture) toont.

     ![Activiteitenstatus: Volledig geladen](media/tutorial-oracle-azure-postgresql-online/dms-activity-full-load-completed.png)

     ![Activiteitenstatus: Incrementele gegevenssynchronisatie](media/tutorial-oracle-azure-postgresql-online/dms-activity-incremental-data-sync.png)

## <a name="perform-migration-cutover"></a>Migratie-cutover uitvoeren

Nadat de eerste volledige lading is voltooid, worden de databases gemarkeerd als **Gereed voor cutover**.

1. Wanneer u klaar bent om de databasemigratie te voltooien, selecteert u **Cutover starten**.

2. Zorg dat u alle transacties stopt die bij de brondatabase binnenkomen; wacht totdat de teller van **Wijzigingen in behandeling** op **0** staat.

   ![Cutover starten](media/tutorial-oracle-azure-postgresql-online/dms-start-cutover.png)

3. Selecteer **Bevestigen** en selecteer vervolgens **Toepassen**.
4. Wanneer de status van de database-migratie voor de status **voltooid**, verbinding maken met uw toepassingen naar de nieuwe doel-Azure Database voor PostgreSQL-instantie.

 > [!NOTE]
 > Aangezien PostgreSQL standaard schema.table.column in kleine letters heeft, kunt u terugkeren van hoofdletters naar kleine letters met behulp van het script in de **instellen van het schema in Azure Database for PostgreSQL** sectie eerder in dit artikel.

## <a name="next-steps"></a>Volgende stappen

* Raadpleeg het artikel [Bekende problemen/beperkingen met online migraties naar Azure Database for PostgreSQL](known-issues-azure-postgresql-online.md) voor informatie over bekende problemen en beperkingen bij het uitvoeren van online migraties naar Azure Database for PostgreSQL.
* Zie het artikel [Wat is de Azure Database Migration Service?](https://docs.microsoft.com/azure/dms/dms-overview) voor informatie over de Azure Database Migration Service.
* Zie het artikel voor informatie over Azure Database for PostgreSQL, [wat is Azure Database for PostgreSQL?](https://docs.microsoft.com/azure/postgresql/overview).
