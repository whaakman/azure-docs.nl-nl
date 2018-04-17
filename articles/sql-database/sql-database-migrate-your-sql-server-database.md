---
title: SQL Server-database migreren naar Azure SQL Database | Microsoft Docs
description: Informatie over het migreren van de SQL Server-database naar Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: mvc,migrate
ms.topic: tutorial
ms.date: 04/04/2018
ms.author: carlrab
ms.openlocfilehash: c4b4354cb1090b9abc388da9d1d1b135f135144d
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="migrate-your-sql-server-database-to-azure-sql-database"></a>Uw SQL Server-database migreren naar Azure SQL Database

Het verplaatsen van uw SQL Server-database naar Azure SQL Database erg eenvoudig. U maakt een lege SQL-database in Azure en met behulp van [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) importeert u de database in Azure. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een lege Azure SQL-database maken in Azure Portal (met behulp van een nieuwe of bestaande Azure SQL Database-server)
> * Een firewall op serverniveau maken in Azure Portal (indien nog niet eerder gemaakt)
> * [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) gebruiken om uw SQL Server-database te importeren in de lege Azure SQL-database 
> * Gebruik [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) om database-eigenschappen te wijzigen.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

- De nieuwste versie van [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) is geïnstalleerd.  
- De nieuwste versie van [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) is geïnstalleerd.
- U hebt een database geïdentificeerd en er toegang toe om te kunnen migreren. In deze zelfstudie wordt gebruikgemaakt van de [SQL Server 2008R2 AdventureWorks OLTP-database](https://msftdbprodsamples.codeplex.com/releases/view/59211) op een exemplaar van SQL Server 2008R2 of hoger, maar u kunt uw eigen database gebruiken. Gebruik [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) als u compatibiliteitsproblemen wilt oplossen

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-blank-sql-database"></a>Een lege SQL-database maken

Een Azure SQL-database wordt gemaakt met een gedefinieerde set [reken- en opslagresources](sql-database-service-tiers.md). De database is gemaakt in een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) en in een [logische Azure SQL Database-server](sql-database-features.md). 

Volg deze stappen om een lege SQL-database te maken. 

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.

2. Selecteer **Databases** op de pagina **Nieuw** en selecteer **Maken** onder **SQL-database** op de pagina **Nieuw**.

   ![lege database maken](./media/sql-database-design-first-database/create-empty-database.png)

3. Vul het formulier SQL Database in met de volgende informatie, zoals in de voorgaande afbeelding wordt weergegeven:   

   | Instelling       | Voorgestelde waarde | Beschrijving | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Databasenaam** | mySampleDatabase | Zie [Database-id's](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) voor geldige databasenamen. | 
   | **Abonnement** | Uw abonnement  | Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen. |
   | **Resourcegroep** | myResourceGroup | Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige resourcegroepnamen. |
   | **Bron selecteren** | Lege database | Hiermee geeft u aan dat er een lege database moet worden gemaakt. |

4. Klik op **Server** als u een nieuwe server voor de nieuwe database wilt maken en configureren. Vul het **nieuwe serverformulier** in met de volgende gegevens: 

   | Instelling       | Voorgestelde waarde | Beschrijving | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servernaam** | Een wereldwijd unieke naam | Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige servernamen. | 
   | **Aanmeldgegevens van serverbeheerder** | Een geldige naam | Zie [Database-id's](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) voor geldige aanmeldingsnamen.|
   | **Wachtwoord** | Een geldig wachtwoord | Uw wachtwoord moet uit minstens acht tekens bestaan en moet tekens bevatten uit drie van de volgende categorieën: hoofdletters, kleine letters, cijfers en niet-alfanumerieke tekens. |
   | **Locatie** | Een geldige locatie | Zie [Azure-regio's](https://azure.microsoft.com/regions/) voor informatie over regio's. |

   ![database-server maken](./media/sql-database-design-first-database/create-database-server.png)

5. Klik op **Selecteren**.

6. Klik op **Prijscategorie** om de servicelaag, het aantal DTU's en de hoeveelheid opslag op te geven. Bekijk de opties voor de hoeveelheid DTU's en opslag die voor elke servicelaag beschikbaar zijn. 

7. Voor deze zelfstudie selecteert u de servicelaag **Standard** en gebruikt u vervolgens de schuifregelaar om **100 DTU's (S3)** en **400** GB aan opslagruimte te selecteren.

   ![database-s1 maken](./media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

8. Accepteer de gebruiksvoorwaarden voor de preview om de optie **Extra opslag** te gebruiken. 

   > [!IMPORTANT]
   > \* Opslagruimten groter dan de hoeveelheid inbegrepen opslagruimte zijn in preview en hiervoor gelden extra kosten. Zie [de prijsinformatie voor SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) voor meer informatie. 
   >
   >\* In de Premium-laag is momenteel ruim 1 TB opslag beschikbaar voor de volgende regio's: Brazilië - zuid, Canada - centraal, Canada - oost, VS - midden, Frankrijk - centraal, Duitsland - centraal, Japan - oost, Japan - west, Korea - centraal, VS Noord-Centraal, Noord-Europa, VS Zuid-Centraal, Zuidoost-Azië, UK - zuid, UK - west, VS - oost2, VS - west, VS (overheid) - Virginia, en West-Europa. Zie [P11-P15: huidige beperkingen](sql-database-dtu-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
   > 

9. Als u de servicelaag, het aantal DTU's en de hoeveelheid opslagruimte hebt geselecteerd, klikt u op **Toepassen**.  

10. Selecteer een **sortering** voor de lege database (gebruik de standaardwaarde in deze zelfstudie). Zie [Collations](https://docs.microsoft.com/sql/t-sql/statements/collations) (Sorteringen) voor meer informatie over sorteringen

11. Nu u het SQL Database-formulier hebt ingevuld, klikt u op **Maken** om de database in te richten. De inrichting duurt een paar minuten. 

12. Klik op de werkbalk op **Meldingen** om het implementatieproces te bewaken.
    
     ![melding](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Een serverfirewallregel maken

De service SQL Database maakt een firewall op serverniveau die voorkomt dat externe toepassingen en hulpmiddelen verbinding maken met de server of databases op de server, tenzij er een firewallregel is gemaakt om de firewall te openen voor specifieke IP-adressen. Volg deze stappen om een [SQL Database-firewallregel op serverniveau](sql-database-firewall-configure.md) te maken voor het IP-adres van de client en connectiviteit via de SQL Database-firewall alleen voor uw IP-adres toe te staan. 

> [!NOTE]
> SQL Database communiceert via poort 1433. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 1433 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u geen verbinding maken met uw Azure SQL Database-server, tenzij de IT-afdeling poort 1433 openstelt.
>

1. Wanneer de implementatie is voltooid, klikt u op **SQL Databases** in het menu aan de linkerkant. Klik vervolgens op de pagina **SQL Databases** op **mySampleDatabase**. De overzichtspagina voor uw database wordt geopend, met de volledig gekwalificeerde servernaam (zoals **mynewserver20170824.database.windows.net**) en opties voor verdere configuratie. 

2. Kopieer deze volledig gekwalificeerde servernaam om in volgende snelstarts verbinding te maken met de server en de bijbehorende databases. 

   ![servernaam](./media/sql-database-get-started-portal/server-name.png) 

3. Klik op de werkbalk op **Serverfirewall instellen**. De pagina **Firewallinstellingen** voor de SQL Database-server wordt geopend. 

   ![serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png) 

4. Klik op **IP van client toevoegen** op de werkbalk om uw huidige IP-adres aan een nieuwe firewallregel toe te voegen. Een firewallregel kan poort 1433 openen voor een afzonderlijk IP-adres of voor een aantal IP-adressen.

5. Klik op **Opslaan**. Er wordt een firewallregel op serverniveau gemaakt voor uw huidige IP-adres waarbij poort 1433 op de logische server wordt geopend.

6. Klik op **OK** en sluit de pagina **Firewallinstellingen**.

U kunt nu verbinding maken met de SQL Database-server en de bijbehorende databases met behulp van SQL Server Management Studio, Data Migration Assistant of een ander hulpprogramma naar keuze. Dit doet u vanaf dit IP-adres via het serverbeheerdersaccount dat in de vorige procedure is gemaakt.

> [!IMPORTANT]
> Voor alle Azure-services is toegang via de SQL Database-firewall standaard ingeschakeld. Klik op **UIT** op deze pagina om dit voor alle Azure-services uit te schakelen.

## <a name="sql-server-connection-information"></a>SQL Server-verbindingsgegevens

Haal de volledig gekwalificeerde servernaam van uw Azure SQL Database-server op uit Azure Portal. Gebruik de volledig gekwalificeerde servernaam om verbinding te maken met de Azure SQL-server met behulp van clienthulpprogramma's, waaronder Data Migration Assistance en SQL Server Management Studio.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **SQL-databases** in het menu links en klik op uw database op de pagina **SQL-databases**. 
3. In het deelvenster **Essentials** van de Azure Portal-pagina van uw database kopieert u de **servernaam**.

   ![verbindingsgegevens](./media/sql-database-get-started-portal/server-name.png)

## <a name="migrate-your-database"></a>Uw database migreren

Volg deze stappen om gebruik te maken van **[Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595)** om vast te stellen of de database gereed is voor migratie naar Azure SQL Database en om de migratie te voltooien.

1. Open **Data Migration Assistant**. U kunt DMA uitvoeren op elke computer die verbinding heeft met het SQL Server-exemplaar dat de database bevat die u wilt migreren, en die verbinding heeft met internet. U hoeft DMA niet te installeren op de computer die als host fungeert voor het SQL Server-exemplaar dat u migreert. De firewallregel die u in een vorige procedure hebt gemaakt, geldt voor de computer waarop u Data Migration Assistant uitvoert.

     ![dma openen](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-open.png)

2. Klik in het linkermenu op **+ Nieuwe** om een **evaluatieproject** te maken. Vul de gevraagde waarden in en klik op **Maken**:

   | Instelling      | Voorgestelde waarde | Beschrijving | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Projecttype | Migratie | Kies of u alleen uw database voor migratie wilt vaststellen of kies of u deze ook wilt migreren als onderdeel van dezelfde werkstroom |
   |Projectnaam|Zelfstudie Migratie| Een beschrijvende naam |
   |Bronservertype| SQL Server | Dit is de enige bron die momenteel wordt ondersteund |
   |Doelservertype| Azure SQL Database| De keuzes omvatten: SQL Database, SQL Server, SQL Server op virtuele machines in Azure |
   |Migratiebereik| Schema en gegevens| De keuzes omvatten: schema en gegevens, alleen schema, alleen gegevens |
   
   ![project voor nieuwe dma](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-new-project.png)

3.  Vul op de pagina **Bron selecteren** de vereiste waarden in en klik op **Verbinding maken**:

    | Instelling      | Voorgestelde waarde | Beschrijving | 
    | ------------ | ------------------ | ------------------------------------------------- | 
    | Servernaam | Servernaam of IP-adres | Servernaam of IP-adres |
    | Verificatietype | Verificatietype van voorkeur| Keuzes: Windows-verificatie, SQL Server-verificatie, geïntegreerde Active Directory-verificatie, wachtwoordverificatie voor Active Directory |
    | Gebruikersnaam | Aanmeldingsnaam | Uw aanmeldingsnaam dient machtigingen voor **CONTROL SERVER** te hebben |
    | Wachtwoord| Uw wachtwoord | Uw wachtwoord |
    | Verbindingseigenschappen| Selecteer de opties **Verbinding versleutelen** en **Servercertificaat vertrouwen** die geschikt zijn voor uw omgeving. | Kies de eigenschappen die geschikt zijn voor de verbinding met uw server |

    ![bron selecteren nieuwe gegevensmigratie](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-source.png)

5. Selecteer één database in uw bronserver die u wilt migreren naar Azure SQL Database en klik op **Volgende**. In deze zelfstudie is er slechts één database.

6. Vul op de pagina **Doel selecteren** de vereiste waarden in en klik op **Verbinding maken**:

    | Instelling      | Voorgestelde waarde | Beschrijving | 
    | ------------ | ------------------ | ------------------------------------------------- | 
    | Servernaam | De volledig gekwalificeerde naam van de Azure Database-server | De volledig gekwalificeerde naam van de Azure Database-server uit de vorige procedure |
    | Verificatietype | SQL Server-verificatie | SQL Server-verificatie was de enige optie toen deze zelfstudie werd geschreven, maar geïntegreerde Active Directory-verificatie en wachtwoordverificatie voor Active Directory worden ook door Azure SQL Database ondersteund |
    | Gebruikersnaam | Aanmeldingsnaam | Uw aanmeldingsnaam dient **CONTROL DATABASE**-machtigingen te hebben voor de doeldatabase |
    | Wachtwoord| Uw wachtwoord | Uw wachtwoord |
    | Verbindingseigenschappen| Selecteer de opties **Verbinding versleutelen** en **Servercertificaat vertrouwen** die geschikt zijn voor uw omgeving. | Kies de eigenschappen die geschikt zijn voor de verbinding met uw server |

    ![doel selecteren nieuwe gegevensmigratie](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-target.png)

7. Selecteer de database van de doelserver die u in een vorige procedure hebt gemaakt en klik vervolgens op **Volgende** om het evaluatieproces voor het doeldatabaseschema te starten. In deze zelfstudie is er slechts één database. Het compatibiliteitsniveau voor deze database wordt ingesteld op 140. Dit is het standaardcompatibiliteitsniveau voor alle nieuwe databases in Azure SQL Database.

   > [!IMPORTANT] 
   > Als u de database hebt gemigreerd naar Azure SQL Database, kunt u de database op een bepaald compatibiliteitsniveau hanteren voor compatibiliteit met eerdere versies. Zie [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level) (Compatibiliteitsniveau ALTER DATABASE) voor meer informatie over de implicaties en opties bij het hanteren van een database op een bepaald compatibiliteitsniveau. Zie ook [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) voor informatie over aanvullende instellingen op databaseniveau in verband met compatibiliteitsniveaus.
   >

8. Als het evaluatieproces voor het doeldatabaseschema is voltooid, controleert u op de pagina **Objecten selecteren** de objecten die zijn geselecteerd voor migratie en de objecten met problemen. Controleer bijvoorbeeld het object **dbo.uspSearchCandidateResumes** op wijzigingen in **SERVERPROPERTY('LCID')**-gedrag, en het object **HumanResourcesJobCandidate** op wijzigingen in Zoekopdracht in volledige tekst. 

   > [!IMPORTANT] 
   > Afhankelijk van het ontwerp van de database en dat van uw toepassing, dient u tijdens het migreren van de doeldatabase mogelijk de database en/of de toepassing na de migratie te wijzigen (en in sommige gevallen vóór de migratie). Zie [Transact-SQL-verschillen oplossen tijdens de migratie naar de SQL Database](sql-database-transact-sql-information.md) voor meer informatie over Transact-SQL-verschillen die de migratie negatief kunnen beïnvloeden.

     ![evaluatie van nieuwe gegevensmigratie en objectselectie](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results.png)

9. Klik op **SQL-script genereren** om een script te maken van de schemaobjecten in de doeldatabase. 
10. Beoordeel het gegenereerde script en klik zo nodig op **Next issue** om de vastgestelde evaluatieproblemen en aanbevelingen te beoordelen. Voor Zoekopdracht in volledige tekst bijvoorbeeld is de aanbeveling bij het uitvoeren van een upgrade dat u de toepassing test terwijl u gebruikmaakt van de volledige-tekstfuncties. U kunt het script desgewenst opslaan of kopiëren.

     ![gegenereerd script voor nieuwe gegevensmigratie](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-generated-script.png)

11. Klik op **Deploy schema** en bekijk het proces van de schemamigratie.

     ![schemamigratie nieuwe gegevensmigratie](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-schema-migration.png)

12. Als de schemamigratie is voltooid, controleert u de resultaten op fouten en als u die niet ziet, klikt u op **Migrate data**.
13. Controleer op de pagina **Tabellen selecteren** de tabellen die voor migratie zijn geselecteerd en klik op **Start data migration**.

     ![gegevensmigratie nieuwe gegevensmigratie](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-data-migration.png)

14. Bekijk het migratieproces.

     ![proces gegevensmigratie nieuwe gegevensmigratie](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-data-migration-process.png)

## <a name="connect-to-the-database-with-ssms"></a>Verbinding maken met de database via SSMS

Gebruik [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) om verbinding te maken met uw Azure SQL Database-server.

1. Open SQL Server Management Studio.

2. Voer in het dialoogvenster **Verbinding maken met server** de volgende informatie in:

   | Instelling       | Voorgestelde waarde | Beschrijving | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Servertype | Database-engine | Deze waarde is verplicht |
   | Servernaam | De volledig gekwalificeerde servernaam | De naam moet er ongeveer als volgt uitzien: **mynewserver20170824.database.windows.net**. |
   | Verificatie | SQL Server-verificatie | SQL-verificatie is het enige verificatietype dat we in deze zelfstudie hebben geconfigureerd. |
   | Aanmelden | Het beheerdersaccount voor de server | Dit is het account dat u hebt opgegeven tijdens het maken van de server. |
   | Wachtwoord | Het wachtwoord voor het beheerdersaccount voor de server | Dit is het wachtwoord dat u hebt opgegeven tijdens het maken van de server. |

   ![verbinding maken met server](./media/sql-database-connect-query-ssms/connect.png)

3. Klik op **Opties** in het dialoogvenster **Verbinding maken met server**. Voer in de sectie **Verbinding maken met database** **mySampleDatabase** in om verbinding te maken met deze database.

   ![verbinding maken met database op server](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. Klik op **Verbinden**. Het venster Objectverkenner wordt geopend in SSMS. 

5. In Objectverkenner vouwt u **Databases** en daarna **mySampleDatabase** uit om de objecten in de voorbeelddatabase weer te geven.

   ![databaseobjecten](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="change-database-properties"></a>Eigenschappen van de database wijzigen

U kunt de servicelaag, het prestatieniveau en het compatibiliteitsniveau wijzigen met behulp van SQL Server Management Studio. Voor de beste prestaties wordt u tijdens de importeerfase aangeraden naar een database met een hogere prestatielaag te importeren. Schaal echter omlaag nadat het importeren is voltooid om geld te besparen, totdat u klaar bent om de geïmporteerde database actief te gaan gebruiken. Het wijzigen van het compatibiliteitsniveau kan betere prestaties opleveren en toegang tot de nieuwste mogelijkheden van de Azure SQL Database-service. Wanneer u een oudere database migreert, wordt het compatibiliteitsniveau van de database gehandhaafd op het laagste ondersteunde niveau dat nog compatibel is met de database die wordt geïmporteerd. Zie [Improved query performance with compatibility Level 130 in Azure SQL Database](sql-database-compatibility-level-query-performance-130.md) (Verbeterde prestaties bij het uitvoeren van query's met compatibiliteitsniveau 130 in Azure SQL Database) voor meer informatie.

1. Klik in Objectverkenner met de rechtermuisknop op **mySampleDatabase** en klik vervolgens op **Nieuwe query**. Er wordt een queryvenster geopend dat is verbonden met uw database.

2. Voer de volgende opdracht uit om de servicelaag in te stellen op **Standaard** en het prestatieniveau op **S1**.

    ```sql
    ALTER DATABASE mySampleDatabase 
    MODIFY 
        (
        EDITION = 'Standard'
        , MAXSIZE = 250 GB
        , SERVICE_OBJECTIVE = 'S1'
    );
    ```

## <a name="next-steps"></a>Volgende stappen 
In deze zelfstudie hebt u het volgende geleerd:

> * Een lege Azure SQL-database maken in Azure Portal 
> * Een firewall op serverniveau maken in Azure Portal 
> * [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) gebruiken om uw SQL Server-database te importeren in de lege Azure SQL-database 
> * Gebruik [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) om database-eigenschappen te wijzigen.

Ga door naar de volgende zelfstudie om te leren hoe u de database beveiligd.

> [!div class="nextstepaction"]
> [Azure SQL Database beveiligen](sql-database-security-tutorial.md).


