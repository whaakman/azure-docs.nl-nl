---
title: 'Altijd versleuteld: Azure SQL Database - Windows-certificaatarchief | Microsoft Docs'
description: In dit artikel laat zien hoe het veiligstellen van gevoelige gegevens in een SQL-database waarvoor versleuteling van de database met behulp van Wizard altijd versleuteld in SQL Server Management Studio (SSMS). Deze ook ziet u hoe de versleutelingssleutels in het certificaatarchief van Windows.
keywords: versleutelen van gegevens, sql-versleuteling, versleuteling van de database, gevoelige gegevens altijd versleuteld.
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: ce7e052e-8bf6-4d7c-9204-4c6f4afeba4b
ms.service: sql-database
ms.custom: security
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: sstein
ms.openlocfilehash: d1fdfc4f739e65ff532b159eefaffe1622ad0963
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-the-windows-certificate-store"></a>Altijd versleuteld: Beveiligen van gevoelige gegevens in SQL-Database en opslaan van de versleutelingssleutels in het certificaatarchief van Windows

Dit artikel laat zien hoe het veiligstellen van gevoelige gegevens in een SQL-database waarvoor versleuteling van de database met behulp van de [Wizard altijd versleuteld](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Deze ook ziet u hoe de versleutelingssleutels in het certificaatarchief van Windows.

Altijd versleutelde is een nieuwe technologie voor het codering van gegevens in Azure SQL Database en als de gegevens in gebruik is, wordt nooit ervoor te zorgen dat gevoelige gegevens weergegeven als SQL Server die helpt beveiligen van gevoelige gegevens in rust op de server tijdens het verkeer tussen client en server de platte tekst binnen het databasesysteem. Nadat u gegevens worden versleuteld, alleen clienttoepassingen of appservers die toegang tot de sleutels hebben hebben toegang tot gecodeerde gegevens. Zie voor gedetailleerde informatie [altijd versleuteld (Database-Engine)](https://msdn.microsoft.com/library/mt163865.aspx).

Na het configureren van de database voor het gebruik van altijd versleuteld, maakt u een clienttoepassing in C# met Visual Studio werkt met de versleutelde gegevens.

Volg de stappen in dit artikel voor meer informatie over het instellen van altijd versleuteld voor een Azure SQL database. In dit artikel leert u hoe u de volgende taken uitvoeren:

* Gebruik van de wizard altijd versleuteld in SSMS maken [sleutels altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
  * Maak een [hoofdsleutel van kolom (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
  * Maak een [Kolomversleutelingssleutel (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
* Een databasetabel maken en kolommen te versleutelen.
* Maak een toepassing die wordt ingevoegd, selecteert en gegevens uit de versleutelde kolommen worden weergegeven.

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* Een Azure-account en -abonnement. Als u niet hebt, zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) versie 13.0.700.242 of hoger.
* [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) of hoger (op de clientcomputer).
* [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).

## <a name="create-a-blank-sql-database"></a>Een lege SQL-database maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **nieuwe** > **gegevens en opslag** > **SQL-Database**.
3. Maak een **leeg** database met de naam **kliniek** op een nieuwe of bestaande server. Zie voor gedetailleerde instructies over het maken van een database in de Azure portal [uw eerste Azure SQL database](sql-database-get-started-portal.md).
   
    ![Een lege database maken](./media/sql-database-always-encrypted/create-database.png)

U moet de verbindingsreeks later in de zelfstudie. Nadat de database is gemaakt, gaat u naar de nieuwe kliniek-database en kopieer de verbindingsreeks. U kunt de verbindingsreeks ophalen op elk gewenst moment, maar het is gemakkelijk om deze te kopiëren wanneer u zich in de Azure-portal.

1. Klik op **SQL-databases** > **kliniek** > **databaseverbindingsreeksen tonen**.
2. Kopieer de verbindingsreeks voor **ADO.NET**.
   
    ![Kopieer de verbindingsreeks](./media/sql-database-always-encrypted/connection-strings.png)

## <a name="connect-to-the-database-with-ssms"></a>Verbinding maken met de database met SSMS
SSMS opent en verbinding maken met de server met de database kliniek.

1. Open SSMS. (Klik op **Connect** > **Database-Engine** openen de **verbinding maken met Server** venster als dit niet geopend is).
2. Voer uw servernaam en referenties. Naam van de server kan worden gevonden op de blade SQL-database en in de verbindingsreeks u eerder hebt gekopieerd. Typ de volledige naam waaronder *database.windows.net*.
   
    ![Kopieer de verbindingsreeks](./media/sql-database-always-encrypted/ssms-connect.png)

Als de **nieuwe firewallregel** venster wordt geopend, aanmelding bij Azure en laat SSMS voor u een nieuwe firewallregel maken.

## <a name="create-a-table"></a>Een tabel maken
In deze sectie maakt u een tabel patiëntgegevens kan bevatten. Dit is een normale tabel in eerste instantie--configureert u versleuteling in de volgende sectie.

1. Vouw **Databases**.
2. Met de rechtermuisknop op de **kliniek** database en klik op **nieuwe Query**.
3. Plak de volgende Transact-SQL (T-SQL) in het nieuwe queryvenster en **Execute** deze.

        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO


## <a name="encrypt-columns-configure-always-encrypted"></a>Versleutelen van de kolommen (Configureer altijd versleuteld)
SSMS biedt een wizard voor het configureren van eenvoudig altijd versleuteld door het instellen van de CMK, CEK en versleutelde kolommen voor u.

1. Vouw **Databases** > **kliniek** > **tabellen**.
2. Met de rechtermuisknop op de **patiënten** tabel en selecteer **versleutelen kolommen** om de wizard altijd versleuteld te openen:
   
    ![Kolommen versleutelen](./media/sql-database-always-encrypted/encrypt-columns.png)

De wizard altijd versleuteld omvat de volgende secties: **kolomselectie**, **hoofdsleutel configuratie** (CMK) **validatie**, en **samenvatting** .

### <a name="column-selection"></a>Kolom selecteren
Klik op **volgende** op de **inleiding** pagina openen de **kolomselectie** pagina. Op deze pagina wordt u selecteren welke kolommen u versleutelen wilt, [het type versleuteling, en welke kolomversleutelingssleutel (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) te gebruiken.

Versleutelen **SSN** en **geboortedatum** informatie voor elke geduld. De **SSN** kolom deterministische versleuteling, die ondersteuning biedt voor gelijkheid zoekacties, samenvoegingen en gegroepeerd wordt gebruikt. De **geboortedatum** kolom willekeurige versleuteling, die geen ondersteuning biedt voor bewerkingen worden gebruikt.

Instellen de **versleutelingstype** voor de **SSN** kolom **Deterministic** en de **geboortedatum** kolom **Randomized** . Klik op **Volgende**.

![Kolommen versleutelen](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>Configuratie van de hoofdsleutel
De **hoofdsleutel configuratie** pagina is waar u uw CMK instellen en selecteren van de sleutelopslagplaatsprovider waar de CMK worden opgeslagen. Op dit moment kunt u een CMK opslaan in het certificaatarchief van de Windows Azure Key Vault of een hardware security module (HSM). Deze zelfstudie laat zien hoe uw sleutels opslaan in het certificaatarchief van Windows.

Controleer **Windows-certificaatarchief** is geselecteerd en klik op **volgende**.

![Configuratie van de hoofdsleutel](./media/sql-database-always-encrypted/master-key-configuration.png)

### <a name="validation"></a>Validatie
U kunt nu de kolommen versleutelen of opslaan van een PowerShell-script later uit te voeren. Selecteer voor deze zelfstudie **doorgaan voltooid nu** en klik op **volgende**.

### <a name="summary"></a>Samenvatting
Controleer of de instellingen juist zijn en klikt u op **voltooien** om de instellingen voor altijd versleuteld.

![Samenvatting](./media/sql-database-always-encrypted/summary.png)

### <a name="verify-the-wizards-actions"></a>Controleer of de acties van de wizard
Nadat de wizard voltooid is, wordt uw database instellen voor altijd versleuteld. De wizard zijn de volgende acties uitgevoerd:

* Een CMK gemaakt.
* Een CEK gemaakt.
* De geselecteerde kolommen voor codering geconfigureerd. Uw **patiënten** tabel momenteel geen gegevens bevat, maar alle bestaande gegevens in de geselecteerde kolommen nu versleuteld.

U kunt het maken van de sleutels in SSMS controleren door te gaan naar **kliniek** > **beveiliging** > **sleutels altijd versleuteld**. U ziet nu de nieuwe sleutels die de wizard voor u gegenereerd.

## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Een clienttoepassing die geschikt is voor de versleutelde gegevens maken
Nu dat altijd versleuteld is ingesteld, kunt u een toepassing die wordt uitgevoerd *voegt* en *selecteert* voor versleutelde kolommen. Als u wilt de voorbeeldtoepassing met succes uitvoeren, moet u het uitvoeren op dezelfde computer waarop u de wizard altijd versleuteld uitgevoerd. Als u wilt de toepassing op een andere computer uitvoert, moet u uw certificaten altijd versleuteld op de computer waarop de client-app implementeren.  

> [!IMPORTANT]
> Uw toepassing moet gebruiken [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objecten als tekst zonder opmaak gegevens doorgegeven aan de server met kolommen altijd versleuteld. Letterlijke waarden doorgeven zonder SqlParameter objecten leidt tot een uitzondering.
> 
> 

1. Open Visual Studio en maak een nieuwe C#-consoletoepassing. Zorg ervoor dat uw project is ingesteld op **.NET Framework 4.6** of hoger.
2. Noem het project **AlwaysEncryptedConsoleApp** en klik op **OK**.

![Nieuwe consoletoepassing](./media/sql-database-always-encrypted/console-app.png)

## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>De verbindingsreeks om in te schakelen altijd versleuteld wijzigen
Deze sectie wordt uitgelegd hoe altijd versleuteld inschakelen in de verbindingsreeks voor uw database. Wijzigt u de console-app die u zojuist hebt gemaakt in de volgende sectie, "Altijd versleuteld console voorbeeldtoepassing."

Om in te schakelen altijd versleuteld, moet u toevoegen de **Kolomversleutelingsinstelling** sleutelwoord naar uw verbinding tekenreeks en stel deze in op **ingeschakeld**.

U kunt dit rechtstreeks in de verbindingsreeks instellen of kunt u dit instellen met behulp van een [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). De voorbeeldtoepassing in de volgende sectie leest u hoe u **SqlConnectionStringBuilder**.

> [!NOTE]
> Dit is de enige wijziging vereist in een specifieke clienttoepassing altijd versleuteld. Als u een bestaande toepassing die wordt de verbindingsreeks extern opgeslagen (dat wil zeggen, in een configuratiebestand), u kunt mogelijk altijd versleuteld inschakelen zonder een code te wijzigen.
> 
> 

### <a name="enable-always-encrypted-in-the-connection-string"></a>Altijd versleuteld inschakelen in de verbindingsreeks
Het volgende trefwoord toevoegen aan uw verbindingsreeks:

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Altijd versleuteld met een SqlConnectionStringBuilder inschakelen
De volgende code toont het inschakelen van altijd versleuteld door in te stellen de [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) naar [ingeschakeld](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a>Altijd versleutelde console voorbeeldtoepassing
Dit voorbeeld laat zien hoe:

* Wijzig de verbindingsreeks om in te schakelen altijd versleuteld.
* Gegevens invoegen in het versleutelde kolommen.
* Een record met een filter voor een specifieke waarde in een versleutelde kolom selecteren.

Vervang de inhoud van **Program.cs** met de volgende code. De verbindingsreeks voor de globale connectionString-variabele in de regel direct boven de Main-methode vervangen door de geldige verbindingsreeks van de Azure-portal. Dit is de enige wijziging die u moet aanbrengen in deze code.

Voer de app om te zien altijd versleuteld in te grijpen.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;

    namespace AlwaysEncryptedConsoleApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Replace with your connection string";

        static void Main(string[] args)
        {
            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            InsertPatient(new Patient() {
                SSN = "999-99-0001", FirstName = "Orlando", LastName = "Gee", BirthDate = DateTime.Parse("01/04/1964") });
            InsertPatient(new Patient() {
                SSN = "999-99-0002", FirstName = "Keith", LastName = "Harris", BirthDate = DateTime.Parse("06/20/1977") });
            InsertPatient(new Patient() {
                SSN = "999-99-0003", FirstName = "Donna", LastName = "Carreras", BirthDate = DateTime.Parse("02/09/1973") });
            InsertPatient(new Patient() {
                SSN = "999-99-0004", FirstName = "Janet", LastName = "Gates", BirthDate = DateTime.Parse("08/31/1985") });
            InsertPatient(new Patient() {
                SSN = "999-99-0005", FirstName = "Lucy", LastName = "Harrington", BirthDate = DateTime.Parse("05/06/1993") });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
         VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }


## <a name="verify-that-the-data-is-encrypted"></a>Controleer of dat de gegevens worden versleuteld
U kunt snel controleren dat de werkelijke gegevens op de server worden gecodeerd door het uitvoeren van query's de **patiënten** gegevens met SSMS. (Gebruik de huidige verbinding waarbij de kolomversleutelingsinstelling nog niet is ingeschakeld.)

De volgende query uitvoeren op de kliniek-database.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

U kunt zien dat de versleutelde kolommen geen gecodeerde gegevens bevatten.

   ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted/ssms-encrypted.png)

U kunt toevoegen voor het gebruik van SSMS toegang tot de gegevens als leesbare tekst, de **Kolomversleutelingsinstelling = ingeschakeld** -parameter voor de verbinding.

1. SSMS, met de rechtermuisknop op uw server in **Objectverkenner**, en klik vervolgens op **Disconnect**.
2. Klik op **Connect** > **Database-Engine** openen de **verbinding maken met Server** venster en klik vervolgens op **opties**.
3. Klik op **extra verbindingsparameters** en het type **Kolomversleutelingsinstelling = ingeschakeld**.
   
    ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted/ssms-connection-parameter.png)
4. De volgende query uitvoeren op de **kliniek** database.
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     U ziet nu de gecodeerde gegevens in de versleutelde kolommen.

    ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [!NOTE]
> Als u verbinding met SSMS (of een client) vanaf een andere computer maakt, hebben geen toegang tot de versleutelingssleutels en niet mogelijk om de gegevens te ontsleutelen.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Nadat u een database die gebruikmaakt van altijd versleuteld maakt, wilt u mogelijk het volgende doen:

* Dit voorbeeld uitvoert vanaf een andere computer. Deze geen toegang tot de versleutelingssleutels zodat deze geen toegang tot de gecodeerde gegevens en kan niet worden uitgevoerd.
* [Draaien en opschonen van uw sleutels](https://msdn.microsoft.com/library/mt607048.aspx).
* [Migreren van gegevens die al is versleuteld met altijd versleuteld](https://msdn.microsoft.com/library/mt621539.aspx).
* [Altijd versleuteld certificaten implementeren op andere clientcomputers](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (Zie de sectie 'Waardoor certificaten beschikbaar voor toepassingen en-gebruikers').

## <a name="related-information"></a>Gerelateerde informatie
* [Altijd versleuteld (ontwikkeling client)](https://msdn.microsoft.com/library/mt147923.aspx)
* [Transparante gegevensversleuteling](https://msdn.microsoft.com/library/bb934049.aspx)
* [SQL Server-versleuteling](https://msdn.microsoft.com/library/bb510663.aspx)
* [Altijd versleutelde Wizard](https://msdn.microsoft.com/library/mt459280.aspx)
* [Altijd versleutelde Blog](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

