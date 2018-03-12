---
title: Uw eerste Azure SQL-database ontwerpen | Microsoft Docs
description: Lees hier alles over het ontwerpen van uw eerste Azure SQL database in Azure Portal en met SQL Server Management Studio.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop databases
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 01/29/2018
ms.author: carlrab
ms.openlocfilehash: 52ec8afa693e6ac3c8c73f070f748a01116ec68f
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="design-your-first-azure-sql-database"></a>Uw eerste Azure SQL-database ontwerpen

Azure SQL Database is een relationele DBaaS (database-as-a-service) in de Microsoft Cloud (Azure). In deze zelfstudie leert u hoe u met Azure Portal en [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) de volgende taken uitvoert: 

> [!div class="checklist"]
> * Een database maken in Azure Portal
> * Een serverfirewallregel instellen in Azure Portal
> * Verbinding maken met de database via SSMS
> * Tabellen maken met SSMS
> * Gegevens bulksgewijs laden met BCP
> * Query uitvoeren op die gegevens met SSMS
> * De database [herstellen naar een eerder tijdstip](sql-database-recovery-using-backups.md#point-in-time-restore) in Azure Portal

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Het volgende moet zijn geïnstalleerd om deze zelfstudie te voltooien:
- De nieuwste versie van [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS).
- De nieuwste versie van [BCP en SQLCMD](https://www.microsoft.com/download/details.aspx?id=36433).

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
   >\* In de Premium-laag is momenteel ruim 1 TB opslag beschikbaar voor de volgende regio's: Australië - oost, Australië - zuidoost, Brazilië - zuid, Canada - centraal, Canada - oost, VS - midden, Frankrijk - centraal, Duitsland - centraal, Japan - oost, Japan - west, Korea - centraal, VS Noord-Centraal, Noord-Europa, VS Zuid-Centraal, Zuidoost-Azië, UK - zuid, UK - west, VS - oost2, VS - west, VS (overheid) - Virginia, en West-Europa. Zie [P11-P15: huidige beperkingen](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
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

U kunt nu verbinding maken met de SQL Database-server en de bijbehorende databases met behulp van SQL Server Management Studio of een ander hulpprogramma naar keuze. Dit doet u vanaf dit IP-adres via het serverbeheerdersaccount dat eerder is gemaakt.

> [!IMPORTANT]
> Voor alle Azure-services is toegang via de SQL Database-firewall standaard ingeschakeld. Klik op **UIT** op deze pagina om dit voor alle Azure-services uit te schakelen.

## <a name="sql-server-connection-information"></a>SQL Server-verbindingsgegevens

Haal de volledig gekwalificeerde servernaam van uw Azure SQL Database-server op uit Azure Portal. U gebuikt de volledig gekwalificeerde servernaam om verbinding met uw server te maken via SQL Server Management Studio.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **SQL-databases** in het menu links en klik op uw database op de pagina **SQL-databases**. 
3. In het deelvenster **Essentials** van de Azure Portal-pagina van uw database kopieert u de **servernaam**.

   ![verbindingsgegevens](./media/sql-database-get-started-portal/server-name.png)

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

## <a name="create-tables-in-the-database"></a>Tabellen maken in de database 

Maak met [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference) een databaseschema met vier tabellen die overeenkomen met belangrijke gegevens uit een studenteninformatiesysteem voor universiteiten:

- Person
- Course
- Student
- Credit

Het volgende diagram laat zien hoe deze tabellen aan elkaar zijn gerelateerd. Sommige van deze tabellen verwijzen naar kolommen in andere tabellen. Zo verwijst de tabel Student naar de kolom **PersonId** van de tabel **Person**. Bekijk het diagram goed om te begrijpen hoe de tabellen in deze zelfstudie aan elkaar zijn gerelateerd. Zie dit Engelstalige artikel over het [maken van effectieve databasetabellen](https://msdn.microsoft.com/library/cc505842.aspx) voor uitgebreide informatie over het maken van effectieve databasetabellen. Zie [Gegevenstypen](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) voor informatie over het kiezen van gegevenstypen.

> [!NOTE]
> U kunt ook de functie voor het [ontwerpen van tabellen in SQL Server Management Studio](https://msdn.microsoft.com/library/hh272695.aspx) gebruiken om tabellen te maken en ontwerpen. 

![Relaties tussen tabellen](./media/sql-database-design-first-database/tutorial-database-tables.png)

1. Klik in Objectverkenner met de rechtermuisknop op **mySampleDatabase** en klik vervolgens op **Nieuwe query**. Er wordt een leeg queryvenster geopend dat is verbonden met uw database.

2. Voer in het queryvenster de volgende query uit om vier tabellen te maken in uw database: 

   ```sql 
   -- Create Person table

   CREATE TABLE Person
   (
   PersonId   INT IDENTITY PRIMARY KEY,
   FirstName   NVARCHAR(128) NOT NULL,
   MiddelInitial NVARCHAR(10),
   LastName   NVARCHAR(128) NOT NULL,
   DateOfBirth   DATE NOT NULL
   )
   
   -- Create Student table
 
   CREATE TABLE Student
   (
   StudentId INT IDENTITY PRIMARY KEY,
   PersonId  INT REFERENCES Person (PersonId),
   Email   NVARCHAR(256)
   )
   
   -- Create Course table
 
   CREATE TABLE Course
   (
   CourseId  INT IDENTITY PRIMARY KEY,
   Name   NVARCHAR(50) NOT NULL,
   Teacher   NVARCHAR(256) NOT NULL
   ) 

   -- Create Credit table
 
   CREATE TABLE Credit
   (
   StudentId   INT REFERENCES Student (StudentId),
   CourseId   INT REFERENCES Course (CourseId),
   Grade   DECIMAL(5,2) CHECK (Grade <= 100.00),
   Attempt   TINYINT,
   CONSTRAINT  [UQ_studentgrades] UNIQUE CLUSTERED
   (
   StudentId, CourseId, Grade, Attempt
   )
   )
   ```

   ![Tabellen maken](./media/sql-database-design-first-database/create-tables.png)

3. Vouw het knooppunt 'Tables' in Object Explorer van SQL Server Management Studio uit om de tabellen te zien die u hebt gemaakt.

   ![Tabellen gemaakt in SSMS](./media/sql-database-design-first-database/ssms-tables-created.png)

## <a name="load-data-into-the-tables"></a>Gegevens laden in de tabellen

1. Maak een map **SampleTableData** in de map Downloads voor het opslaan van voorbeeldgegevens voor uw database. 

2. Klik met de rechtermuisknop op de volgende koppelingen en sla deze op in de map **SampleTableData**. 

   - [SampleCourseData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCourseData)
   - [SamplePersonData](https://sqldbtutorial.blob.core.windows.net/tutorials/SamplePersonData)
   - [SampleStudentData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleStudentData)
   - [SampleCreditData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCreditData)

3. Open een venster met een opdrachtprompt en ga naar de map SampleTableData.

4. Voer de volgende opdrachten uit om voorbeeldgegevens toe te voegen aan de tabellen. Vervang hierbij de waarden voor **ServerName**, **DatabaseName**, **UserName** en **Password** door de waarden voor uw omgeving.
  
   ```bcp
   bcp Course in SampleCourseData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Person in SamplePersonData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Student in SampleStudentData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Credit in SampleCreditData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   ```

U hebt nu voorbeeldgegevens geladen in de tabellen die u eerder hebt gemaakt.

## <a name="query-data"></a>Querygegevens

Voer de volgende query's uit om gegevens op te halen uit de databasetabellen. Zie [SQL-query's schrijven](https://technet.microsoft.com/library/bb264565.aspx) voor meer informatie over het schrijven van SQL-query's. Met de eerste query worden alle vier de tabellen gekoppeld om alle studenten te vinden geleerd die 'Dominick Pope' als docent hebben en die een cijfer hoger dan 75% van hem hebben gehad. Met de tweede query worden alle vier de tabellen gekoppeld en worden alle cursussen gevonden waarvoor 'Noe Coleman' zich ooit heeft ingeschreven.

1. Voer in een venster van SQL Server Management Studio de volgende query uit:

   ```sql 
   -- Find the students taught by Dominick Pope who have a grade higher than 75%

   SELECT  person.FirstName,
   person.LastName,
   course.Name,
   credit.Grade
   FROM  Person AS person
   INNER JOIN Student AS student ON person.PersonId = student.PersonId
   INNER JOIN Credit AS credit ON student.StudentId = credit.StudentId
   INNER JOIN Course AS course ON credit.CourseId = course.courseId
   WHERE course.Teacher = 'Dominick Pope' 
   AND Grade > 75
   ```

2. Voer in een venster van SQL Server Management Studio de volgende query uit:

   ```sql
   -- Find all the courses in which Noe Coleman has ever enrolled

   SELECT  course.Name,
   course.Teacher,
   credit.Grade
   FROM  Course AS course
   INNER JOIN Credit AS credit ON credit.CourseId = course.CourseId
   INNER JOIN Student AS student ON student.StudentId = credit.StudentId
   INNER JOIN Person AS person ON person.PersonId = student.PersonId
   WHERE person.FirstName = 'Noe'
   AND person.LastName = 'Coleman'
   ```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Een database herstellen naar een eerder tijdstip

Stel dat u een tabel per ongeluk hebt verwijderd. Dit is iets wat u niet eenvoudig kunt herstellen. In Azure SQL Database kunt u echter gedurende een periode van maximaal 35 dagen teruggaan naar een bepaald tijdstip om vanaf dat moment de gegevens te herstellen in een nieuwe database. U kunt deze database vervolgens gebruiken om de verwijderde gegevens terug te halen. Met de volgende stappen wordt de voorbeelddatabase hersteld naar een punt voordat de tabellen zijn toegevoegd.

1. Klik op de pagina SQL-database voor uw database op **Restore** op de werkbalk. De**gelijknamige**pagina wordt geopend.

   ![De pagina Restore](./media/sql-database-design-first-database/restore.png)

2. Vul in het formulier **Restore** de vereiste gegevens in:
    * Databasenaam: voer een naam in voor de database 
    * Point-in-time: selecteer het tabblad **Point-in-time** van het formulier Restore 
    * Restore point: selecteer een tijdstip voorafgaand aan het moment waarop de database is gewijzigd
    * Target server: u kunt deze waarde niet wijzigen als u een database gaat herstellen 
    * Elastic database pool: selecteer **None**  
    * Pricing tier: select **20 DTUs** en **40 GB** opslag.

   ![herstelpunt](./media/sql-database-design-first-database/restore-point.png)

3. Klik op **OK** om de database te [herstellen naar een eerder tijdstip](sql-database-recovery-using-backups.md#point-in-time-restore), voordat de tabellen werden toegevoegd. Als u een database herstelt naar een eerder tijdstip, wordt er op de server van de oorspronkelijke database een database gemaakt met de inhoud zoals die aanwezig was op het tijdstip dat u opgeeft. Dit tijdstip moet wel binnen de bewaarperiode liggen die wordt gehanteerd voor uw [servicelaag](sql-database-service-tiers.md).

## <a name="next-steps"></a>Volgende stappen 
In deze zelfstudie hebt u kennisgemaakt met eenvoudige databasetaken, zoals het maken van een database en tabellen, het laden en opvragen van gegevens, en het herstellen van een database naar een eerder tijdstip. U hebt geleerd hoe u:
> [!div class="checklist"]
> * Een database maken
> * Een firewallregel instellen
> * Verbinding maken met de database met behulp van [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS)
> * Tabellen maken
> * Bulksgewijs gegevens laden
> * Een query uitvoeren op deze gegevens
> * De database herstellen naar een eerder tijdstip met behulp van [SQL-Database](sql-database-recovery-using-backups.md#point-in-time-restore)

Ga naar de volgende zelfstudie als u alles wilt weten over het ontwerpen van een database met Visual Studio en C#.

> [!div class="nextstepaction"]
>[Een Azure SQL-database ontwerpen en verbinding maken met C# en ADO.NET](sql-database-design-first-database-csharp.md)
