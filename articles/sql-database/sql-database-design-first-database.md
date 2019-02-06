---
title: 'Zelfstudie: Uw eerste individuele database in Azure SQL Database ontwerpen met SSMS | Microsoft Docs'
description: Lees hier alles over het ontwerpen van uw eerste Azure SQL database met SQL Server Management Studio.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.topic: tutorial
author: CarlRabeler
ms.author: carlrab
ms.reviewer: v-masebo
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: e7229a0816cf74fed08397a68dd34e305bf8c0ea
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55459533"
---
# <a name="tutorial-design-your-first-azure-sql-database-using-ssms"></a>Zelfstudie: Uw eerste Azure SQL-database ontwerpen met SSMS

Azure SQL Database is een relationele DBaaS (database-as-a-service) in Microsoft Cloud (Azure). In deze zelfstudie leert u hoe u met Azure Portal en [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) de volgende taken uitvoert:

> [!div class="checklist"]
> * Een database maken in Azure Portal*
> * Een serverfirewallregel instellen in Azure Portal
> * Verbinding maken met de database via SSMS
> * Tabellen maken met SSMS
> * Gegevens bulksgewijs laden met BCP
> * Query uitvoeren op gegevens met SSMS

*Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

> [!NOTE]
> Voor deze zelfstudie gebruiken we het [aankoopmodel op basis van DTU](sql-database-service-tiers-dtu.md), maar u kunt desgewenst ook het [aankoopmodel op basis van vCore](sql-database-service-tiers-vcore.md) kiezen.

## <a name="prerequisites"></a>Vereisten

Het volgende moet zijn geïnstalleerd om deze zelfstudie te voltooien:

- [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (nieuwste versie)
- [BCP en SQLCMD](https://www.microsoft.com/download/details.aspx?id=36433) (nieuwste versie)

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-blank-database"></a>Een lege database maken

Een Azure SQL-database wordt gemaakt met een gedefinieerde set [reken- en opslagresources](sql-database-service-tiers-dtu.md). De database is gemaakt in een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) en in een [Azure SQL Database-server](sql-database-features.md).

Volg deze stappen om een lege SQL-database te maken.

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.

1. Selecteer op de pagina **Nieuw** **Databases** in de sectie Azure Marketplace en klik vervolgens op **SQL Database** in de sectie **Aanbevolen**.

   ![lege database maken](./media/sql-database-design-first-database/create-empty-database.png)

   1. Vul het formulier **SQL Database** in met de volgende informatie, zoals in de voorgaande afbeelding wordt weergegeven:

      | Instelling       | Voorgestelde waarde | Beschrijving |
      | ------------ | ------------------ | ------------------------------------------------- |
      | **Databasenaam** | *yourDatabase* | Zie [Database-id's](/sql/relational-databases/databases/database-identifiers) voor geldige databasenamen. |
      | **Abonnement** | *yourSubscription*  | Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen. |
      | **Resourcegroep** | *yourResourceGroup* | Zie [Naming conventions](/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige resourcegroepnamen. |
      | **Bron selecteren** | Lege database | Hiermee geeft u aan dat er een lege database moet worden gemaakt. |

   1. Klik op **Server** als u een bestaande server wilt gebruiken of een nieuwe server voor de database wilt maken en configureren. Selecteer de server of klik op **Een nieuwe server maken** en vul het formulier **Nieuwe server** in met de volgende gegevens:

      | Instelling       | Voorgestelde waarde | Beschrijving |
      | ------------ | ------------------ | ------------------------------------------------- |
      | **Servernaam** | Een wereldwijd unieke naam | Zie [Naming conventions](/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige servernamen. |
      | **Aanmeldgegevens van serverbeheerder** | Een geldige naam | Zie [Database-id's](/sql/relational-databases/databases/database-identifiers) voor geldige aanmeldingsnamen. |
      | **Wachtwoord** | Een geldig wachtwoord | Uw wachtwoord moet uit minstens acht tekens bestaan en moet tekens bevatten uit drie van de volgende categorieën: hoofdletters, kleine letters, cijfers en niet-alfanumerieke tekens. |
      | **Locatie** | Een geldige locatie | Zie [Azure-regio's](https://azure.microsoft.com/regions/) voor informatie over regio's. |

      ![database-server maken](./media/sql-database-design-first-database/create-database-server.png)

      Klik op **Selecteren**.

   1. Klik op **Prijscategorie** om de servicelaag, het aantal DTU's of vCores en de hoeveelheid opslag op te geven. U kunt de opties bekijken voor de hoeveelheid DTU's/vCores en opslag die voor elke servicelaag beschikbaar zijn. Standaard wordt het **Standard**-[aankoopmodel op basis van DTU](sql-database-service-tiers-dtu.md) geselecteerd, maar u kunt desgewenst ook het [aankoopmodel op basis van vCore](sql-database-service-tiers-vcore.md) kiezen.

      > [!IMPORTANT]
      > Voor de Premium-laag is er meer dan 1 TB aan opslagruimte beschikbaar in alle regio's met uitzondering van: UK - noord, US - west-centraal, US - zuid 2, China - oost, USDoD - centraal, Duitsland - centraal, US DoD - oost, US Gov - zuid-west, US Gov - zuid-centraal, Duitsland - noordoost, China - noord, US Gov - oost. In andere regio’s is de maximale opslagruimte in de Premium-laag beperkt tot 1 TB. Zie [P11-P15: huidige beperkingen]( sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).

      Als u de servicelaag, het aantal DTU's en de hoeveelheid opslagruimte hebt geselecteerd, klikt u op **Toepassen**.

   1. Voer een **sortering** in voor de lege database (gebruik de standaardwaarde in deze zelfstudie). Zie [Collations](/sql/t-sql/statements/collations) (Sorteringen) voor meer informatie over sorteringen

1. Nu u het **SQL Database**-formulier hebt ingevuld, klikt u op **Maken** om de database in te richten. Deze stap kan enkele minuten duren.

1. Klik op de werkbalk op **Meldingen** om het implementatieproces te bewaken.

     ![melding](./media/sql-database-design-first-database/notification.png)

## <a name="create-a-firewall-rule"></a>Een firewallregel maken

De service SQL Database maakt een firewall op serverniveau. De firewall voorkomt dat externe toepassingen en hulpprogramma's verbinding maken met de server of databases op de server. Als u externe connectiviteit voor uw database wilt inschakelen, moet u eerst een regel voor uw IP-adres aan de firewall toevoegen. Volg deze stappen als u een [firewallregel op SQL Database-serverniveau](sql-database-firewall-configure.md) wilt maken.

> [!NOTE]
> SQL Database communiceert via poort 1433. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 1433 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u geen verbinding maken met uw Azure SQL Database-server, tenzij de beheerder poort 1433 openstelt.

1. Wanneer de implementatie is voltooid, klikt u op **SQL Databases** in het menu aan de linkerkant. Klik vervolgens op de pagina **SQL Databases** op *yourDatabase*. De overzichtspagina voor de database wordt geopend, met de volledig gekwalificeerde **servernaam** (bijvoorbeeld *yourserver.database.windows.net*) en opties voor verdere configuratie.

1. Kopieer vanuit SQL Server Management Studio deze volledig gekwalificeerde servernaam om verbinding te maken met de server en de databases.

   ![servernaam](./media/sql-database-design-first-database/server-name.png)

1. Klik op de werkbalk op **Serverfirewall instellen**. De pagina **Firewallinstellingen** voor de SQL Database-server wordt geopend.

   ![serverfirewallregel](./media/sql-database-design-first-database/server-firewall-rule.png)

   1. Klik op **IP van client toevoegen** op de werkbalk om uw huidige IP-adres aan een nieuwe firewallregel toe te voegen. Een firewallregel kan poort 1433 openen voor een afzonderlijk IP-adres of voor een aantal IP-adressen.

   1. Klik op **Opslaan**. Er wordt een firewallregel op serverniveau gemaakt voor uw huidige IP-adres waarbij poort 1433 op de SQL Database-server wordt geopend.

   1. Klik op **OK** en sluit de pagina **Firewallinstellingen**.

Uw IP-adres wordt niet meer door de firewall geblokkeerd. U kunt nu verbinding maken met de SQL Database-server en de bijbehorende databases met behulp van SQL Server Management Studio of een ander hulpprogramma naar keuze. Gebruik het beheerdersaccount voor de server dat u eerder hebt gemaakt.

> [!IMPORTANT]
> Voor alle Azure-services is toegang via de SQL-databasefirewall standaard ingeschakeld. Klik op **UIT** op deze pagina om dit voor alle Azure-services uit te schakelen.

## <a name="connect-to-the-database"></a>Verbinding maken met de database

Gebruik [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) om verbinding te maken met uw Azure SQL Database-server.

1. Open SQL Server Management Studio.

1. Voer in het dialoogvenster **Verbinding maken met server** de volgende informatie in:

   | Instelling       | Voorgestelde waarde | Beschrijving |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Servertype** | Database-engine | Deze waarde is verplicht. |
   | **Servernaam** | De volledig gekwalificeerde servernaam | Bijvoorbeeld *yourserver.database.windows.net*. |
   | **Verificatie** | SQL Server-verificatie | SQL-verificatie is het enige verificatietype dat we in deze zelfstudie hebben geconfigureerd. |
   | **Aanmelding** | Het beheerdersaccount voor de server | Het account dat u hebt opgegeven tijdens het maken van de server. |
   | **Wachtwoord** | Het wachtwoord voor het beheerdersaccount voor de server | Het wachtwoord dat u hebt opgegeven tijdens het maken van de server. |

   ![verbinding maken met server](./media/sql-database-design-first-database/connect.png)

   1. Klik op **Opties** in het dialoogvenster **Verbinding maken met server**. Voer in de sectie **Verbinding maken met database** *yourDatabase* in om verbinding te maken met deze database.

      ![verbinding maken met database op server](./media/sql-database-design-first-database/options-connect-to-db.png)  

   1. Klik op **Verbinden**. Het venster **Objectverkenner** wordt geopend in SSMS.

1. In **Objectverkenner** vouwt u **Databases** en daarna *yourDatabase* uit om de objecten in de voorbeelddatabase weer te geven.

   ![databaseobjecten](./media/sql-database-design-first-database/connected.png)  

## <a name="create-tables-in-the-database"></a>Tabellen maken in de database

Maak met [Transact-SQL](/sql/t-sql/language-reference) een databaseschema met vier tabellen die overeenkomen met belangrijke gegevens uit een studenteninformatiesysteem voor universiteiten:

- Person
- Course
- Student
- Tegoed

Het volgende diagram laat zien hoe deze tabellen aan elkaar zijn gerelateerd. Sommige van deze tabellen verwijzen naar kolommen in andere tabellen. Zo verwijst de tabel *Student* naar de kolom *PersonId* van de tabel *Person*. Bekijk het diagram goed om te begrijpen hoe de tabellen in deze zelfstudie aan elkaar zijn gerelateerd. Zie dit Engelstalige artikel over het [maken van effectieve databasetabellen](https://msdn.microsoft.com/library/cc505842.aspx) voor uitgebreide informatie over het maken van effectieve databasetabellen. Zie [Gegevenstypen](/sql/t-sql/data-types/data-types-transact-sql) voor informatie over het kiezen van gegevenstypen.

> [!NOTE]
> U kunt ook de functie voor het [ontwerpen van tabellen in SQL Server Management Studio](/sql/ssms/visual-db-tools/design-database-diagrams-visual-database-tools) gebruiken om tabellen te maken en ontwerpen.

![Relaties tussen tabellen](./media/sql-database-design-first-database/tutorial-database-tables.png)

1. Klik in **Objectverkenner** met de rechtermuisknop op *yourDatabase* en selecteer **Nieuwe query**. Er wordt een leeg queryvenster geopend dat is verbonden met uw database.

1. Voer in het queryvenster de volgende query uit om vier tabellen te maken in uw database:

   ```sql
   -- Create Person table
   CREATE TABLE Person
   (
       PersonId INT IDENTITY PRIMARY KEY,
       FirstName NVARCHAR(128) NOT NULL,
       MiddelInitial NVARCHAR(10),
       LastName NVARCHAR(128) NOT NULL,
       DateOfBirth DATE NOT NULL
   )

   -- Create Student table
   CREATE TABLE Student
   (
       StudentId INT IDENTITY PRIMARY KEY,
       PersonId INT REFERENCES Person (PersonId),
       Email NVARCHAR(256)
   )

   -- Create Course table
   CREATE TABLE Course
   (
       CourseId INT IDENTITY PRIMARY KEY,
       Name NVARCHAR(50) NOT NULL,
       Teacher NVARCHAR(256) NOT NULL
   )

   -- Create Credit table
   CREATE TABLE Credit
   (
       StudentId INT REFERENCES Student (StudentId),
       CourseId INT REFERENCES Course (CourseId),
       Grade DECIMAL(5,2) CHECK (Grade <= 100.00),
       Attempt TINYINT,
       CONSTRAINT [UQ_studentgrades] UNIQUE CLUSTERED
       (
           StudentId, CourseId, Grade, Attempt
       )
   )
   ```

   ![Tabellen maken](./media/sql-database-design-first-database/create-tables.png)

1. Vouw in de **Objectverkenner**, onder *yourDatabase*, het knooppunt **Tables** uit om de tabellen te zien die u hebt gemaakt.

   ![Tabellen gemaakt in SSMS](./media/sql-database-design-first-database/ssms-tables-created.png)

## <a name="load-data-into-the-tables"></a>Gegevens laden in de tabellen

1. Maak een map *sampleData* in de map Downloads voor het opslaan van voorbeeldgegevens voor uw database.

1. Klik met de rechtermuisknop op de volgende koppelingen en sla deze op in de map *sampleData*.

   - [SampleCourseData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCourseData)
   - [SamplePersonData](https://sqldbtutorial.blob.core.windows.net/tutorials/SamplePersonData)
   - [SampleStudentData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleStudentData)
   - [SampleCreditData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCreditData)

1. Open een venster met een opdrachtprompt en ga naar de map *sampleData*.

1. Voer de volgende opdrachten uit om voorbeeldgegevens toe te voegen aan de tabellen. Vervang hierbij de waarden voor *server*, *database*, *user* en *password* door de waarden voor uw omgeving.
  
   ```cmd
   bcp Course in SampleCourseData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   bcp Person in SamplePersonData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   bcp Student in SampleStudentData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   bcp Credit in SampleCreditData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   ```

U hebt nu voorbeeldgegevens geladen in de tabellen die u eerder hebt gemaakt.

## <a name="query-data"></a>Querygegevens

Voer de volgende query's uit om gegevens op te halen uit de databasetabellen. Zie [Write SQL queries](https://technet.microsoft.com/library/bb264565.aspx) (SQL-query's schrijven) voor meer informatie over het schrijven van SQL-query's. Met de eerste query worden alle vier de tabellen gekoppeld om de studenten te vinden die 'Dominick Pope' als docent hebben en een rang hoger dan 75% hebben. Met de tweede query worden alle vier de tabellen gekoppeld en worden de cursussen gevonden waarvoor 'Noe Coleman' zich ooit heeft ingeschreven.

1. Voer in een venster van SQL Server Management Studio de volgende query uit:

   ```sql
   -- Find the students taught by Dominick Pope who have a grade higher than 75%
   SELECT  person.FirstName, person.LastName, course.Name, credit.Grade
   FROM  Person AS person
       INNER JOIN Student AS student ON person.PersonId = student.PersonId
       INNER JOIN Credit AS credit ON student.StudentId = credit.StudentId
       INNER JOIN Course AS course ON credit.CourseId = course.courseId
   WHERE course.Teacher = 'Dominick Pope'
       AND Grade > 75
   ```

1. Voer in een queryvenster de volgende query uit:

   ```sql
   -- Find all the courses in which Noe Coleman has ever enrolled
   SELECT  course.Name, course.Teacher, credit.Grade
   FROM  Course AS course
       INNER JOIN Credit AS credit ON credit.CourseId = course.CourseId
       INNER JOIN Student AS student ON student.StudentId = credit.StudentId
       INNER JOIN Person AS person ON person.PersonId = student.PersonId
   WHERE person.FirstName = 'Noe'
       AND person.LastName = 'Coleman'
   ```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een aantal eenvoudige databasetaken geleerd. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een database maken
> * Een firewallregel instellen
> * Verbinding maken met de database met behulp van [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS)
> * Tabellen maken
> * Bulksgewijs gegevens laden
> * Een query uitvoeren op deze gegevens

Ga naar de volgende zelfstudie als u alles wilt weten over het ontwerpen van een database met Visual Studio en C#.

> [!div class="nextstepaction"]
> [Een Azure SQL-database ontwerpen en verbinding maken met C# en ADO.NET](sql-database-design-first-database-csharp.md)
