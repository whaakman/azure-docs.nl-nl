---
title: 'Altijd versleuteld: SQL Database - Azure Sleutelkluis | Microsoft Docs'
description: In dit artikel leest u hoe ter beveiliging van gevoelige gegevens in een SQL-database met versleuteling van gegevens met de altijd versleuteld Wizard in SQL Server Management Studio.
keywords: versleuteling van gegevens, de versleutelingssleutel, cloud-versleuteling
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
ms.assetid: 6ca16644-5969-497b-a413-d28c3b835c9b
ms.service: sql-database
ms.custom: security
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: sstein
ms.openlocfilehash: 0f26ce26b8b33274291c115ae136d124d79ed349
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a>Altijd versleuteld: Beveiligen van gevoelige gegevens in SQL-Database en de versleutelingssleutels in Azure Sleutelkluis

Dit artikel ziet u het beveiligen van gevoelige gegevens in een SQL-database met gegevens versleuteling met de [Wizard altijd versleuteld](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Het bevat ook instructies die wordt beschreven hoe u voor het opslaan van de versleutelingssleutel in Azure Sleutelkluis.

Versleutelde is altijd een nieuwe technologie voor het codering van gegevens in Azure SQL Database en SQL Server die voorkomen dat gevoelige gegevens in rust op de server tijdens het verkeer tussen client en server, en als de gegevens in gebruik. Altijd versleutelde zorgt ervoor dat gevoelige gegevens nooit wordt weergegeven als leesbare tekst binnen het databasesysteem. Nadat u gegevensversleuteling configureert, alleen clienttoepassingen of appservers die toegang tot de sleutels hebben hebben toegang tot gecodeerde gegevens. Zie voor gedetailleerde informatie [altijd versleuteld (Database-Engine)](https://msdn.microsoft.com/library/mt163865.aspx).

Nadat u de database voor het gebruik van altijd versleuteld configureert, maakt u een clienttoepassing in C# met Visual Studio werkt met de versleutelde gegevens.

Volg de stappen in dit artikel en informatie over het instellen van altijd versleuteld voor een Azure SQL database. In dit artikel leert u hoe u de volgende taken uitvoeren:

* Gebruik van de wizard altijd versleuteld in SSMS maken [sleutels altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
  * Maak een [kolomhoofdsleutel (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
  * Maak een [kolomversleutelingssleutel (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
* Een databasetabel maken en kolommen te versleutelen.
* Maak een toepassing die wordt ingevoegd, selecteert en gegevens uit de versleutelde kolommen worden weergegeven.

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* Een Azure-account en -abonnement. Als u niet hebt, zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) versie 13.0.700.242 of hoger.
* [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) of hoger (op de clientcomputer).
* [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* [Azure PowerShell](/powershell/azure/overview), versie 1.0 of hoger. Type **(Get-Module azure - ListAvailable). Versie** wilt zien welke versie van PowerShell uitgevoerd.

## <a name="enable-your-client-application-to-access-the-sql-database-service"></a>De clienttoepassing toegang tot de SQL-Database-service inschakelen
U moet de clienttoepassing toegang heeft tot de service SQL Database met een Azure Active Directory (AAD)-toepassing instellen en kopiëren inschakelen de *toepassings-ID* en *sleutel* die u moet verificatie van uw toepassing.

Ophalen van de *toepassings-ID* en *sleutel*, volg de stappen in [maken van een Azure Active Directory-toepassing en service-principal die toegang bronnen tot](../azure-resource-manager/resource-group-create-service-principal-portal.md).

## <a name="create-a-key-vault-to-store-your-keys"></a>Een sleutelkluis voor het opslaan van uw sleutels maken
Nu dat uw client-app is geconfigureerd en u de ID van uw toepassing hebt, is het tijd om een sleutelkluis maken en configureren van het toegangsbeleid, zodat u en uw toepassing toegang krijgen de kluis geheimen (de sleutels altijd versleuteld tot). De *maken*, *ophalen*, *lijst*, *aanmelding*, *controleren*, *wrapKey*, en *unwrapKey* machtigingen zijn vereist voor het maken van een nieuwe hoofdsleutel voor kolom- en voor het instellen van versleuteling met SQL Server Management Studio.

U kunt snel een sleutelkluis maken het volgende script uit te voeren. Zie voor een gedetailleerde uitleg van deze cmdlets en meer informatie over het maken en configureren van een sleutelkluis [aan de slag met Azure Key Vault](../key-vault/key-vault-get-started.md).

    $subscriptionName = '<your Azure subscription name>'
    $userPrincipalName = '<username@domain.com>'
    $applicationId = '<application ID from your AAD application>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $vaultName = 'AeKeyVault'


    Login-AzureRmAccount
    $subscriptionId = (Get-AzureRmSubscription -SubscriptionName $subscriptionName).Id
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $resourceGroupName -Location $location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
    Set-AzureRmKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $applicationId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list




## <a name="create-a-blank-sql-database"></a>Een lege SQL-database maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Ga naar **nieuwe** > **gegevens en opslag** > **SQL-Database**.
3. Maak een **leeg** database met de naam **kliniek** op een nieuwe of bestaande server. Zie voor gedetailleerde instructies over het maken van een database in de Azure portal [uw eerste Azure SQL database](sql-database-get-started-portal.md).
   
    ![Een lege database maken](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

U moet de verbinding tekenreeks verderop in de zelfstudie, dus nadat u de database hebt gemaakt, blader naar de nieuwe kliniek-database en kopieer de verbindingsreeks. U kunt de verbindingsreeks ophalen op elk gewenst moment, maar het is gemakkelijk om deze te kopiëren in de Azure portal.

1. Ga naar **SQL-databases** > **kliniek** > **databaseverbindingsreeksen tonen**.
2. Kopieer de verbindingsreeks voor **ADO.NET**.
   
    ![Kopieer de verbindingsreeks](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)

## <a name="connect-to-the-database-with-ssms"></a>Verbinding maken met de database met SSMS
SSMS opent en verbinding maken met de server met de database kliniek.

1. Open SSMS. (Ga naar **Connect** > **Database-Engine** openen de **verbinding maken met Server** venster als deze niet geopend is.)
2. Voer uw servernaam en referenties. Naam van de server kan worden gevonden op de blade SQL-database en in de verbindingsreeks u eerder hebt gekopieerd. Typ de naam van de volledige server, met inbegrip van *database.windows.net*.
   
    ![Kopieer de verbindingsreeks](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

Als de **nieuwe firewallregel** venster wordt geopend, aanmelding bij Azure en laat SSMS voor u een nieuwe firewallregel maken.

## <a name="create-a-table"></a>Een tabel maken
In deze sectie maakt u een tabel patiëntgegevens kan bevatten. Het is niet in eerste instantie versleuteld--kunt u versleuteling wilt configureren in de volgende sectie.

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
SSMS biedt een wizard waarmee u eenvoudig configureren altijd versleuteld door het instellen van de hoofdsleutel van kolom, kolomversleutelingssleutel en versleutelde kolommen voor u.

1. Vouw **Databases** > **kliniek** > **tabellen**.
2. Met de rechtermuisknop op de **patiënten** tabel en selecteer **versleutelen kolommen** om de wizard altijd versleuteld te openen:
   
    ![Kolommen versleutelen](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

De wizard altijd versleuteld omvat de volgende secties: **kolomselectie**, **hoofdsleutel configuratie**, **validatie**, en **samenvatting**.

### <a name="column-selection"></a>Kolom selecteren
Klik op **volgende** op de **inleiding** pagina openen de **kolomselectie** pagina. Op deze pagina wordt u selecteren welke kolommen u versleutelen wilt, [het type versleuteling, en welke kolomversleutelingssleutel (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) te gebruiken.

Versleutelen **SSN** en **geboortedatum** informatie voor elke geduld. De kolom SSN gebruikt deterministische versleuteling, die ondersteuning biedt voor gelijkheid zoekacties, samenvoegingen en gegroepeerd. De kolom Geboortedatum gebruikt willekeurige versleuteling, die geen ondersteuning biedt voor bewerkingen.

Instellen de **versleutelingstype** voor de kolom SSN **Deterministic** en de kolom Geboortedatum **Randomized**. Klik op **Volgende**.

![Kolommen versleutelen](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a>Configuratie van de hoofdsleutel
De **hoofdsleutel configuratie** pagina is waar u uw CMK instellen en selecteren van de sleutelopslagplaatsprovider waar de CMK worden opgeslagen. Op dit moment kunt u een CMK opslaan in het certificaatarchief van de Windows Azure Key Vault of een hardware security module (HSM).

Deze zelfstudie laat zien hoe uw sleutels opslaan in Azure Sleutelkluis.

1. Selecteer **Azure Sleutelkluis**.
2. Selecteer de gewenste sleutelkluis in de vervolgkeuzelijst.
3. Klik op **Volgende**.

![Configuratie van de hoofdsleutel](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)

### <a name="validation"></a>Validatie
U kunt nu de kolommen versleutelen of opslaan van een PowerShell-script later uit te voeren. Selecteer voor deze zelfstudie **doorgaan voltooid nu** en klik op **volgende**.

### <a name="summary"></a>Samenvatting
Controleer of de instellingen juist zijn en klikt u op **voltooien** om de instellingen voor altijd versleuteld.

![Samenvatting](./media/sql-database-always-encrypted-azure-key-vault/summary.png)

### <a name="verify-the-wizards-actions"></a>Controleer of de acties van de wizard
Nadat de wizard voltooid is, wordt uw database instellen voor altijd versleuteld. De wizard zijn de volgende acties uitgevoerd:

* Een hoofdsleutel van kolom gemaakt en opgeslagen in Azure Sleutelkluis.
* Een kolomversleutelingssleutel gemaakt en opgeslagen in Azure Sleutelkluis.
* De geselecteerde kolommen voor codering geconfigureerd. De tabel Patiënten momenteel geen gegevens bevat, maar alle bestaande gegevens in de geselecteerde kolommen nu versleuteld.

U kunt het maken van de sleutels in SSMS controleren door het uitbreiden van **kliniek** > **beveiliging** > **sleutels altijd versleuteld**.

## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Een clienttoepassing die geschikt is voor de versleutelde gegevens maken
Nu dat altijd versleuteld is ingesteld, kunt u een toepassing die wordt uitgevoerd *voegt* en *selecteert* voor versleutelde kolommen.  

> [!IMPORTANT]
> Uw toepassing moet gebruiken [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objecten als tekst zonder opmaak gegevens doorgegeven aan de server met kolommen altijd versleuteld. Letterlijke waarden doorgeven zonder SqlParameter objecten leidt tot een uitzondering.
> 
> 

1. Open Visual Studio en maak een nieuwe C# **consoletoepassing** (Visual Studio 2015 en eerder) of **Console-App (.NET Framework)** (Visual Studio 2017 en hoger). Zorg ervoor dat uw project is ingesteld op **.NET Framework 4.6** of hoger.
2. Noem het project **AlwaysEncryptedConsoleAKVApp** en klik op **OK**.
3. De volgende NuGet-pakketten installeren door te gaan naar **extra** > **NuGet Package Manager** > **Package Manager Console**.

Deze twee regels code uitvoeren in de Package Manager-Console.

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>De verbindingsreeks om in te schakelen altijd versleuteld wijzigen
Deze sectie wordt uitgelegd hoe altijd versleuteld inschakelen in de verbindingsreeks voor uw database.

Om in te schakelen altijd versleuteld, moet u toevoegen de **Kolomversleutelingsinstelling** sleutelwoord naar uw verbinding tekenreeks en stel deze in op **ingeschakeld**.

U kunt dit rechtstreeks in de verbindingsreeks instellen of kunt u dit instellen met behulp van [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). De voorbeeldtoepassing in de volgende sectie leest u hoe u **SqlConnectionStringBuilder**.

### <a name="enable-always-encrypted-in-the-connection-string"></a>Altijd versleuteld inschakelen in de verbindingsreeks
Het volgende trefwoord toevoegen aan de verbindingsreeks.

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a>Altijd versleuteld met SqlConnectionStringBuilder inschakelen
De volgende code toont het inschakelen van altijd versleuteld door in te stellen [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) naar [ingeschakeld](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-the-azure-key-vault-provider"></a>Registreer de provider voor Azure Sleutelkluis
De volgende code laat zien hoe de Azure Sleutelkluis-provider geregistreerd met de ADO.NET-stuurprogramma.

    private static ClientCredential _clientCredential;

    static void InitializeAzureKeyVaultProvider()
    {
       _clientCredential = new ClientCredential(applicationId, clientKey);

       SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
          new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

       Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
          new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

       providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
       SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
    }



## <a name="always-encrypted-sample-console-application"></a>Altijd versleutelde console voorbeeldtoepassing
Dit voorbeeld laat zien hoe:

* Wijzig de verbindingsreeks om in te schakelen altijd versleuteld.
* Azure Key Vault registreren als sleutelopslagplaatsprovider van de toepassing.  
* Gegevens invoegen in het versleutelde kolommen.
* Een record met een filter voor een specifieke waarde in een versleutelde kolom selecteren.

Vervang de inhoud van **Program.cs** met de volgende code. Vervang de verbindingsreeks voor de globale connectionString-variabele in de regel dat direct vóór de Main-methode met de geldige verbindingsreeks van de Azure-portal. Dit is de enige wijziging die u moet aanbrengen in deze code.

Voer de app om te zien altijd versleuteld in te grijpen.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider;

    namespace AlwaysEncryptedConsoleAKVApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"<connection string from the portal>";
        static string applicationId = @"<application ID from your AAD application>";
        static string clientKey = "<key from your AAD application>";


        static void Main(string[] args)
        {
            InitializeAzureKeyVaultProvider();

            Console.WriteLine("Signed in as: " + _clientCredential.ClientId);

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

            InsertPatient(new Patient()
            {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993")
            });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now lets locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 999-99-0003):");
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


        private static ClientCredential _clientCredential;

        static void InitializeAzureKeyVaultProvider()
        {

            _clientCredential = new ClientCredential(applicationId, clientKey);

            SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
              new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

            Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
              new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

            providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
        }

        public async static Task<string> GetToken(string authority, string resource, string scope)
        {
            var authContext = new AuthenticationContext(authority);
            AuthenticationResult result = await authContext.AcquireTokenAsync(resource, _clientCredential);

            if (result == null)
                throw new InvalidOperationException("Failed to obtain the access token");
            return result.AccessToken;
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
U kunt snel controleren dat de werkelijke gegevens op de server worden gecodeerd door een query de patiënten met SSMS (met behulp van de huidige verbinding waarbij **Kolomversleutelingsinstelling** nog niet is ingeschakeld).

De volgende query uitvoeren op de kliniek-database.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

U kunt zien dat de versleutelde kolommen geen gecodeerde gegevens bevatten.

   ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)

U kunt toevoegen voor het gebruik van SSMS toegang tot de gegevens als leesbare tekst, de *Kolomversleutelingsinstelling = ingeschakeld* -parameter voor de verbinding.

1. SSMS, met de rechtermuisknop op uw server in **Objectverkenner** en kies **Disconnect**.
2. Klik op **Connect** > **Database-Engine** openen de **verbinding maken met Server** venster en klik op **opties**.
3. Klik op **extra verbindingsparameters** en het type **Kolomversleutelingsinstelling = ingeschakeld**.
   
    ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)
4. De volgende query uitvoeren op de kliniek-database.
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     U ziet nu de gecodeerde gegevens in de versleutelde kolommen.

    ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a>Volgende stappen
Nadat u een database die gebruikmaakt van altijd versleuteld maakt, wilt u mogelijk het volgende doen:

* [Draaien en opschonen van uw sleutels](https://msdn.microsoft.com/library/mt607048.aspx).
* [Migreren van gegevens die al is versleuteld met altijd versleuteld](https://msdn.microsoft.com/library/mt621539.aspx).

## <a name="related-information"></a>Gerelateerde informatie
* [Altijd versleuteld (ontwikkeling client)](https://msdn.microsoft.com/library/mt147923.aspx)
* [Transparante gegevensversleuteling](https://msdn.microsoft.com/library/bb934049.aspx)
* [SQL Server-versleuteling](https://msdn.microsoft.com/library/bb510663.aspx)
* [Altijd versleutelde wizard](https://msdn.microsoft.com/library/mt459280.aspx)
* [Altijd versleutelde blog](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

