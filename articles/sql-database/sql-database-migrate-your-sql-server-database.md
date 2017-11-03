---
title: SQL Server database migreren naar Azure SQL Database | Microsoft Docs
description: Meer informatie over uw SQL Server-database migreren naar Azure SQL Database.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,migrate
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 09/01/2017
ms.author: carlrab
ms.openlocfilehash: 526222944974c08f92aec2a8418e9b42401bc4d3
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="migrate-your-sql-server-database-to-azure-sql-database"></a>De SQL Server-database migreren naar Azure SQL Database

Het verplaatsen van uw SQL Server-database naar Azure SQL Database is net zo eenvoudig als het een lege SQL-database in Azure maken en vervolgens de [gegevens migratie-assistent](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) de database importeren in Azure. In deze zelfstudie leert hoe u u kunt:

> [!div class="checklist"]
> * Een lege Azure SQL-database maken in de Azure-portal (met behulp van een nieuwe of bestaande Azure SQL Database-server)
> * Maken van een firewall van het niveau van de server in de Azure portal (indien niet eerder hebt gemaakt)
> * Gebruik de [gegevens migratie-assistent](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) de SQL Server-database importeren in de lege Azure SQL database 
> * Gebruik [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) om database-eigenschappen te wijzigen.

Als u een Azure-abonnement geen [een gratis account maken](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

- De nieuwste versie van geïnstalleerd [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).  
- Geïnstalleerd van de nieuwste versie van de [gegevens migratie-assistent](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).
- U hebt geïdentificeerd en hebben toegang tot een database te migreren. Deze zelfstudie wordt gebruikgemaakt van de [SQL Server 2008 R2 AdventureWorks OLTP-database](https://msftdbprodsamples.codeplex.com/releases/view/59211) op een exemplaar van SQL Server 2008 R2 of hoger, maar u kunt een database van uw keuze. U kunt met het oplossen van compatibiliteitsproblemen door gebruik [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-blank-sql-database"></a>Een lege SQL-database maken

Een Azure SQL-database wordt gemaakt met een gedefinieerde set [reken- en opslagresources](sql-database-service-tiers.md). De database is gemaakt in een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) en in een [logische Azure SQL Database-server](sql-database-features.md). 

Volg deze stappen voor het maken van een lege SQL-database. 

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.

2. Selecteer **Databases** op de pagina **Nieuw** en selecteer **Maken** onder **SQL-database** op de pagina **Nieuw**.

   ![lege database maken](./media/sql-database-design-first-database/create-empty-database.png)

3. Vul het formulier SQL Database in met de volgende informatie, zoals in de voorgaande afbeelding wordt weergegeven:   

   | Instelling       | Voorgestelde waarde | Beschrijving | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Databasenaam** | mySampleDatabase | Zie [Database-id's](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) voor geldige databasenamen. | 
   | **Abonnement** | Uw abonnement  | Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen. |
   | **Resourcegroep** | myResourceGroup | Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige resourcegroepnamen. |
   | **Bron selecteren** | Lege database | Hiermee geeft u op dat een lege database moet worden gemaakt. |

4. Klik op **Server** als u een nieuwe server voor de nieuwe database wilt maken en configureren. Vul de **nieuwe serverformulier** met de volgende informatie: 

   | Instelling       | Voorgestelde waarde | Beschrijving | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servernaam** | Een wereldwijd unieke naam | Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige servernamen. | 
   | **Aanmeldgegevens van serverbeheerder** | Een geldige naam | Zie [Database-id's](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) voor geldige aanmeldingsnamen.|
   | **Wachtwoord** | Een geldig wachtwoord | Uw wachtwoord moet ten minste acht tekens bestaan en moet tekens bevatten uit drie van de volgende categorieën: hoofdletters, kleine letters, cijfers en niet-alfanumerieke tekens. |
   | **Locatie** | Een geldige locatie | Zie [Azure-regio's](https://azure.microsoft.com/regions/) voor informatie over regio's. |

   ![database-server maken](./media/sql-database-design-first-database/create-database-server.png)

5. Klik op **Selecteren**.

6. Klik op **Prijscategorie** om de servicelaag, het aantal DTU's en de hoeveelheid opslag op te geven. Bekijk de opties voor het aantal dtu's en opslag die u voor elke servicelaag beschikbaar is. 

7. Voor deze zelfstudie selecteert u de **standaard** servicelaag en gebruik vervolgens de schuifregelaar om te selecteren **100 dtu's (S3)** en **400** GB aan opslagruimte.

   ![database-s1 maken](./media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

8. Accepteer de gebruiksvoorwaarden voor de preview om de optie **Extra opslag** te gebruiken. 

   > [!IMPORTANT]
   > \* Opslagruimten groter dan de hoeveelheid inbegrepen opslagruimte zijn in preview en hiervoor gelden extra kosten. Zie [de prijsinformatie voor SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) voor meer informatie. 
   >
   >\* In de Premium-laag is op dit moment opslag van meer dan 1 TB beschikbaar in de volgende regio's: VS Oost 2, VS West, VS (overheid) Virginia, West-Europa, Duitsland Centraal, Zuidoost-Azië, Japan - oost, Australië - oost, Canada Centraal en Canada Oost. Zie [P11-P15: huidige beperkingen](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
   > 

9. Als u de servicelaag, het aantal DTU's en de hoeveelheid opslagruimte hebt geselecteerd, klikt u op **Toepassen**.  

10. Selecteer een **sortering** voor de lege database (Gebruik de standaardwaarde voor deze zelfstudie). Zie voor meer informatie over sorteringen [sorteringen](https://docs.microsoft.com/sql/t-sql/statements/collations)

11. Nu u het SQL Database-formulier hebt ingevuld, klikt u op **Maken** om de database in te richten. De inrichting duurt een paar minuten. 

12. Klik op de werkbalk op **Meldingen** om het implementatieproces te bewaken.
    
     ![melding](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Een serverfirewallregel maken

De service SQL Database maakt een firewall op serverniveau die voorkomt dat externe toepassingen en hulpmiddelen verbinding maken met de server of databases op de server, tenzij er een firewallregel is gemaakt om de firewall te openen voor specifieke IP-adressen. Volg deze stappen om een [SQL Database-firewallregel op serverniveau](sql-database-firewall-configure.md) te maken voor het IP-adres van de client en connectiviteit via de SQL Database-firewall alleen voor uw IP-adres toe te staan. 

> [!NOTE]
> SQL Database communiceert via poort 1433. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 1433 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u geen verbinding maken met uw Azure SQL Database-server, tenzij de IT-afdeling poort 1433 openstelt.
>

1. Wanneer de implementatie is voltooid, klikt u op **SQL Databases** in het menu aan de linkerkant. Klik vervolgens op de pagina **SQL Databases** op **mySampleDatabase**. De overzichtspagina voor uw database wordt geopend, met de volledig gekwalificeerde servernaam (zoals **mynewserver20170824.database.windows.net**) en opties voor verdere configuratie. 

2. Kopieer deze volledig gekwalificeerde servernaam om in volgende Quick Starts verbinding te maken met de server en de bijbehorende databases. 

   ![servernaam](./media/sql-database-get-started-portal/server-name.png) 

3. Klik op **serverfirewall ingesteld** op de werkbalk. De pagina **Firewallinstellingen** voor de SQL Database-server wordt geopend. 

   ![serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png) 

4. Klik op **IP van client toevoegen** op de werkbalk om uw huidige IP-adres aan een nieuwe firewallregel toe te voegen. Een firewallregel kan poort 1433 openen voor een afzonderlijk IP-adres of voor een aantal IP-adressen.

5. Klik op **Opslaan**. Er wordt een firewallregel op serverniveau gemaakt voor uw huidige IP-adres waarbij poort 1433 op de logische server wordt geopend.

6. Klik op **OK** en sluit de pagina **Firewallinstellingen**.

U kunt nu verbinding met de SQL-databaseserver en de databases met SQL Server Management Studio, migratie-assistent gegevens of een ander hulpprogramma naar keuze van deze IP-adres met de server admin-account in de vorige procedure hebt gemaakt.

> [!IMPORTANT]
> Voor alle Azure-services is toegang via de SQL Database-firewall standaard ingeschakeld. Klik op **UIT** op deze pagina om dit voor alle Azure-services uit te schakelen.

## <a name="sql-server-connection-information"></a>SQL Server-verbindingsgegevens

Haal de volledig gekwalificeerde servernaam van uw Azure SQL Database-server op uit Azure Portal. De volledig gekwalificeerde servernaam kunt u verbinding maken met uw Azure SQL-server met behulp van de client-hulpprogramma's, waaronder de hulp bij het migreren van gegevens en SQL Server Management Studio.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **SQL-databases** in het menu links en klik op uw database op de pagina **SQL-databases**. 
3. In het deelvenster **Essentials** van de Azure Portal-pagina van uw database kopieert u de **servernaam**.

   ![verbindingsgegevens](./media/sql-database-get-started-portal/server-name.png)

## <a name="migrate-your-database"></a>Uw database migreren

Volg deze stappen voor het gebruik van de  **[gegevens migratie-assistent](https://www.microsoft.com/download/details.aspx?id=53595)**  om te beoordelen van de gereedheid van de database voor migratie naar Azure SQL Database en de migratie te voltooien.

1. Open de **Data Migration Assistant**. U kunt DMA uitvoeren op elke computer met de verbinding met de SQL Server-exemplaar met de database die u wilt migreren en verbinding met internet. U hoeft niet te installeren op de computer die als host fungeert voor de SQL Server-exemplaar dat u wilt migreren. De firewallregel die u hebt gemaakt in een eerdere procedure moet zijn voor de computer waarop u werkt met de gegevens migratie-assistent.

     ![gegevens migreren assistent openen](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-open.png)

2. Klik in het menu links op **+ nieuw** maken een **Assessment** project. Vul in de gevraagde waarden en klik vervolgens op **maken**:

   | Instelling      | Voorgestelde waarde | Beschrijving | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Projecttype | Migratie | Kies uw database voor de migratie te beoordelen of ervoor kiezen om te beoordelen en migratie als onderdeel van dezelfde werkstroom |
   |Naam van het project|Migratie-zelfstudie| Een beschrijvende naam |
   |Brontype van de server| SQL Server | Dit is de enige bron is momenteel niet ondersteund |
   |Doeltype server| Azure SQL Database| U kunt kiezen uit: Azure SQL Database, SQL Server, SQL Server op virtuele machines in Azure |
   |Migratie-bereik| Schema en de gegevens| U kunt kiezen uit: Schema en de gegevens, alleen-schema-gegevens |
   
   ![nieuwe gegevens migratie assistent project](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-new-project.png)

3.  Op de **Select bron** pagina, vult u de gevraagde waarden en klik op **Connect**:

    | Instelling      | Voorgestelde waarde | Beschrijving | 
    | ------------ | ------------------ | ------------------------------------------------- | 
    | Servernaam | De servernaam of IP-adres | De servernaam of IP-adres |
    | Verificatietype | Uw voorkeur verificatietype| Keuzes: Windows-verificatie, SQL Server-verificatie, Active Directory geïntegreerde verificatie, Active Directory-wachtwoordverificatie |
    | Gebruikersnaam | De aanmeldingsnaam van uw | Uw aanmelding moet hebben **CONTROL SERVER** machtigingen |
    | Wachtwoord| uw wachtwoord | uw wachtwoord |
    | Verbindingseigenschappen| Selecteer **versleutelen verbinding** en **vertrouwensrelatie servercertificaat** afhankelijk van wat geschikt is voor uw omgeving. | Kies de eigenschappen die geschikt is voor het verbinding maken met de server |

    ![nieuwe gegevens selecteren migratiebron](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-source.png)

5. Selecteer een individuele database van de bronserver migreren naar Azure SQL Database en klik vervolgens op **volgende**. Er is slechts één database voor deze zelfstudie.

6. Op de **Selecteer doel** pagina, vult u de gevraagde waarden en klik op **Connect**:

    | Instelling      | Voorgestelde waarde | Beschrijving | 
    | ------------ | ------------------ | ------------------------------------------------- | 
    | Servernaam | De volledig gekwalificeerde naam van de Azure-Database-server | De volledig gekwalificeerde naam van de server Azure-Database van de vorige procedure |
    | Verificatietype | SQL Server-verificatie | SQL Server-verificatie is de enige optie als deze zelfstudie is geschreven, maar Active Directory Integrated Authentication en Active Directory-wachtwoordverificatie worden ook ondersteund door Azure SQL Database |
    | Gebruikersnaam | De aanmeldingsnaam van uw | Uw aanmelding moet hebben **beheer DATABASE** machtigingen voor de database |
    | Wachtwoord| uw wachtwoord | uw wachtwoord |
    | Verbindingseigenschappen| Selecteer **versleutelen verbinding** en **vertrouwensrelatie servercertificaat** afhankelijk van wat geschikt is voor uw omgeving. | Kies de eigenschappen die geschikt is voor het verbinding maken met de server |

    ![nieuwe migratie selecteert Gegevensdoel](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-target.png)

7. Selecteer de database van de doelserver die u in een vorige procedure hebt gemaakt en klik vervolgens op **volgende** om de bron database schema assessment proces te starten. Er is slechts één database voor deze zelfstudie. U ziet dat het compatibiliteitsniveau voor deze database is ingesteld op 140, namelijk het compatibiliteitsniveau van de standaard voor alle nieuwe databases in Azure SQL Database.

   > [!IMPORTANT] 
   > Nadat u de database naar Azure SQL Database migreert, kunt u de werking van de database op een opgegeven compatibiliteitsniveau voor compatibiliteit met oudere versies. Zie voor meer informatie over de gevolgen en opties voor de werking van een database op een specifieke compatibiliteitsniveau [databasecompatibiliteitsniveau ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Zie ook [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) voor meer informatie over aanvullende databaseniveau instellingen met betrekking tot compatibiliteitsniveaus.
   >

8. Op de **objecten selecteren** pagina nadat de bron van de database schema assessment proces is voltooid, de objecten die zijn geselecteerd voor migratie bekijken en de problemen met objecten bekijken. Controleer bijvoorbeeld de **dbo.uspSearchCandidateResumes** object voor **SERVERPROPERTY('LCID')** gedragswijzigingen en de **HumanResourcesJobCandidate** voor object Wijzigingen in de zoekopdracht in volledige tekst. 

   > [!IMPORTANT] 
   > Afhankelijk van de database-ontwerp en ontwerp van uw toepassing, wanneer u de brondatabase migreert, mogelijk moet u ofwel wijzigen of zowel de database of uw toepassing na de migratie (en in sommige gevallen vóór de migratie). Zie voor meer informatie over Transact-SQL-verschillen die invloed kunnen zijn op uw migratie [verschillen voor het oplossen van Transact-SQL tijdens de migratie naar de SQL-Database](sql-database-transact-sql-information.md).

     ![nieuwe migratie beoordeling en object gegevensselectie](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results.png)

9. Klik op **genereren SQL-script** de schemaobjecten in de brondatabase met een script. 
10. Bekijk het gegenereerde script en klik vervolgens op **naast uitgeven** zo nodig om te controleren van de geïdentificeerde assessment problemen en aanbevelingen. Bijvoorbeeld, voor zoeken in volledige tekst is de aanbeveling als u een upgrade uw gebruik van de functies van de volledige-tekstindex toepassingen testen. U kunt opslaan of kopieer het script als u wenst.

     ![nieuwe gegevens gegenereerd migratiescript](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-generated-script.png)

11. Klik op **implementeren schema** en bekijk hoe het migratieproces schema.

     ![nieuwe migratie schema gegevensmigratie](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-schema-migration.png)

12. Wanneer de schema-migratie is voltooid, bekijkt u de resultaten op fouten en, ervan uitgaande dat er zijn geen, klik op **gegevens migreren**.
13. Op de **tabellen selecteren** pagina, Controleer de tabellen die zijn geselecteerd voor migratie en klik vervolgens op **starten van de gegevensmigratie**.

     ![nieuwe gegevensmigratie voor migratie van gegevens](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-data-migration.png)

14. Bekijk het migratieproces.

     ![nieuwe gegevens migratie migratieproces](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-data-migration-process.png)

## <a name="connect-to-the-database-with-ssms"></a>Verbinding maken met de database met SSMS

Gebruik [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) geen verbinding met uw Azure SQL Database-server.

1. Open SQL Server Management Studio.

2. Voer in het dialoogvenster **Verbinding maken met server** de volgende informatie in:

   | Instelling       | Voorgestelde waarde | Beschrijving | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Servertype | Database-engine | Deze waarde is vereist |
   | Servernaam | De volledig gekwalificeerde servernaam | De naam moet er ongeveer als volgt: **mynewserver20170824.database.windows.net**. |
   | Authentication | SQL Server-verificatie | SQL-verificatie is het enige verificatietype dat we in deze zelfstudie hebben geconfigureerd. |
   | Aanmelden | Het beheerdersaccount voor de server | Dit is het account dat u hebt opgegeven tijdens het maken van de server. |
   | Wachtwoord | Het wachtwoord voor het beheerdersaccount voor de server | Dit is het wachtwoord dat u hebt opgegeven tijdens het maken van de server. |

   ![verbinding maken met server](./media/sql-database-connect-query-ssms/connect.png)

3. Klik op **Opties** in het dialoogvenster **Verbinding maken met server**. Voer in de sectie **Verbinding maken met database** **mySampleDatabase** in om verbinding te maken met deze database.

   ![verbinding maken met database op server](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. Klik op **Verbinden**. Het venster Objectverkenner wordt geopend in SSMS. 

5. In Objectverkenner vouwt u **Databases** en daarna **mySampleDatabase** uit om de objecten in de voorbeelddatabase weer te geven.

   ![database-objecten](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="change-database-properties"></a>Database-eigenschappen wijzigen

U kunt de servicelaag, prestatieniveau en compatibiliteitsniveau met SQL Server Management Studio wijzigen. Tijdens de fase importeren wordt aangeraden dat u importeert met een hogere prestaties laag database voor de beste prestaties, maar dat u omlaag schalen nadat het importeren is voltooid om geld besparen totdat u klaar bent voor de geïmporteerde database actief gebruiken. Het wijzigen van het compatibiliteitsniveau kan opleveren betere prestaties en toegang tot de nieuwste mogelijkheden van de Azure SQL Database-service. Wanneer u een oudere database migreert, wordt het compatibiliteitsniveau van de database behouden op het laagste ondersteunde niveau van die compatibel is met de database wordt geïmporteerd. Zie voor meer informatie [verbeterde prestaties van query's met de compatibiliteit van niveau 130 in Azure SQL Database](sql-database-compatibility-level-query-performance-130.md).

1. In Object Explorer met de rechtermuisknop op **mySampleDatabase** en klik vervolgens op **nieuwe Query**. Een query-venster wordt geopend is verbonden met uw database.

2. De volgende opdracht in te stellen van de servicetier op **standaard** en het prestatieniveau naar **S1**.

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
In deze zelfstudie hebt u geleerd:

> * Een lege Azure SQL-database maken in de Azure portal 
> * Maken van een firewall van het niveau van de server in de Azure portal 
> * Gebruik de [gegevens migratie-assistent](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) de SQL Server-database importeren in de lege Azure SQL database 
> * Gebruik [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) om database-eigenschappen te wijzigen.

Ga naar de volgende zelfstudie voor informatie over het beveiligen van uw database.

> [!div class="nextstepaction"]
> [Beveiligen van uw Azure SQL Database](sql-database-security-tutorial.md).


