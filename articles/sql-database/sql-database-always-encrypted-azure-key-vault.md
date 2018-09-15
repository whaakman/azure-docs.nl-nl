---
title: 'Altijd versleuteld: SQL Database - Azure Key Vault | Microsoft Docs'
description: Dit artikel ziet u hoe u gevoelige gegevens in een SQL-database beveiligen met gegevensversleuteling met behulp van de Wizard altijd versleuteld in SQL Server Management Studio.
keywords: gegevensversleuteling, versleutelingssleutel, cloud-versleuteling
services: sql-database
author: VanMSFT
manager: craigg
ms.service: sql-database
ms.custom: security
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: vanto
ms.openlocfilehash: 5149b4cbd1e86bfca3ad7d628d129a08aff60a98
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45604299"
---
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a>Altijd versleuteld: Bescherming van gevoelige gegevens in SQL-Database en uw versleutelingssleutels opslaan in Azure Key Vault

Dit artikel leest u over het beveiligen van gevoelige gegevens in een SQL-database met gegevens met behulp van versleuteling de [Wizard altijd versleuteld](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Het bevat ook instructies die wordt beschreven hoe u voor het opslaan van de versleutelingssleutel in Azure Key Vault.

Altijd is versleuteld een nieuwe technologie voor het versleuteling van gegevens in Azure SQL Database en SQL Server die voorkomen dat gevoelige gegevens in rust op de server tijdens het verkeer tussen client en server, en als de gegevens worden gebruikt. Altijd versleutelde zorgt ervoor dat gevoelige gegevens nooit wordt weergegeven als tekst zonder opmaak in de database-systeem. Na het configureren van versleuteling van gegevens, alleen clienttoepassingen of servers met Apps die toegang tot de sleutels hebben kunnen toegang tot gegevens als tekst zonder opmaak. Zie voor gedetailleerde informatie [altijd versleuteld (Database-Engine)](https://msdn.microsoft.com/library/mt163865.aspx).

Nadat u de database voor het gebruik van altijd versleuteld hebt geconfigureerd, maakt u een clienttoepassing in C# met Visual Studio om te werken met de versleutelde gegevens.

Volg de stappen in dit artikel en meer informatie over het instellen van altijd versleuteld voor een Azure SQL database. In dit artikel leert u hoe u de volgende taken uitvoeren:

* De wizard Always Encrypted gebruiken in SSMS maken [Always Encrypted sleutels](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
  * Maak een [kolomhoofdsleutel (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
  * Maak een [kolomversleutelingssleutel (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
* Een databasetabel maken en kolommen te versleutelen.
* Maak een toepassing die wordt ingevoegd, selecteert en gegevens uit de versleutelde kolommen worden weergegeven.

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* Een Azure-account en -abonnement. Als u niet hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) versie 13.0.700.242 of hoger.
* [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) of hoger (op de clientcomputer).
* [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* [Azure PowerShell](/powershell/azure/overview), versie 1.0 of hoger. Type **(Get-Module azure - ListAvailable). Versie** om te zien welke versie van PowerShell u gebruikt.

## <a name="enable-your-client-application-to-access-the-sql-database-service"></a>Uw clienttoepassing toegang krijgt tot de SQL Database-service inschakelen
U moet zorgen dat uw clienttoepassing toegang tot de SQL-Database-service door het instellen van een Azure Active Directory (AAD)-toepassing en het kopiëren van de *toepassings-ID* en *sleutel* die u moet verifiëren van uw toepassing.

Aan de *toepassings-ID* en *sleutel*, volg de stappen in [maken van een Azure Active Directory-toepassing en service-principal die toegang hebben tot resources](../azure-resource-manager/resource-group-create-service-principal-portal.md).

## <a name="create-a-key-vault-to-store-your-keys"></a>Een key vault voor het opslaan van uw sleutels maken
Nu dat uw client-app is geconfigureerd en u de toepassings-ID hebt, is het tijd om te maken van een key vault en de toegangsbeleid configureren zodat u en uw toepassing toegang heeft tot een van de kluis geheimen (de sleutels altijd versleuteld). De *maken*, *ophalen*, *lijst*, *aanmelding*, *controleren*, *wrapKey*, en *unwrapKey* machtigingen zijn vereist voor het maken van een nieuwe hoofdsleutel voor de kolom en voor het instellen van versleuteling met SQL Server Management Studio.

U kunt snel een key vault maken het volgende script uit te voeren. Zie voor een gedetailleerde uitleg van deze cmdlets en meer informatie over het maken en configureren van een key vault [aan de slag met Azure Key Vault](../key-vault/key-vault-get-started.md).

    $subscriptionName = '<your Azure subscription name>'
    $userPrincipalName = '<username@domain.com>'
    $applicationId = '<application ID from your AAD application>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $vaultName = 'AeKeyVault'


    Connect-AzureRmAccount
    $subscriptionId = (Get-AzureRmSubscription -SubscriptionName $subscriptionName).Id
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $resourceGroupName -Location $location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
    Set-AzureRmKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $applicationId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list




## <a name="create-a-blank-sql-database"></a>Een lege SQL-database maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Ga naar **een resource maken** > **Databases** > **SQL-Database**.
3. Maak een **leeg** database met de naam **Clinic** op een nieuwe of bestaande server. Zie voor gedetailleerde instructies over het maken van een database in Azure portal [uw eerste Azure SQL-database](sql-database-get-started-portal.md).
   
    ![Een lege database maken](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

U moet de verbinding later in de zelfstudie reeks, dus wanneer u de database hebt gemaakt, blader naar de nieuwe Clinic-database en kopieer de verbindingsreeks. U kunt de verbindingsreeks ophalen op elk gewenst moment, maar het is gemakkelijk om deze te kopiëren in Azure portal.

1. Ga naar **SQL-databases** > **Clinic** > **databaseverbindingsreeksen tonen**.
2. Kopieer de verbindingsreeks voor **ADO.NET**.
   
    ![Kopieer de verbindingsreeks](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)

## <a name="connect-to-the-database-with-ssms"></a>Verbinding maken met de database via SQL Server Management Studio
Open SSMS en maak verbinding met de server met de Clinic-database.

1. Open SQL Server Management Studio. (Ga naar **Connect** > **Database-Engine** openen de **verbinding maken met Server** venster als deze niet geopend is.)
2. Voer uw servernaam en referenties. Naam van de server kan worden gevonden op de blade SQL-database en in de connection string u eerder hebt gekopieerd. Typ de naam van de volledige server, met inbegrip van *database.windows.net*.
   
    ![Kopieer de verbindingsreeks](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

Als de **nieuwe firewallregel** venster wordt geopend, aanmelden bij Azure en laat SSMS een nieuwe firewallregel maken voor u.

## <a name="create-a-table"></a>Een tabel maken
In deze sectie maakt u een tabel voor het opslaan van de gegevens. Het in eerste instantie niet is versleuteld, kunt u versleuteling wilt configureren in de volgende sectie.

1. Vouw **Databases**.
2. Met de rechtermuisknop op de **Clinic** database en klik op **nieuwe Query**.
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


## <a name="encrypt-columns-configure-always-encrypted"></a>Versleutelen van de kolommen (Always Encrypted configureren)
SSMS bevat een wizard waarmee u eenvoudig configureren altijd versleuteld door het instellen van de hoofdsleutel van de kolom, kolomversleutelingssleutel en versleutelde kolommen voor u.

1. Vouw **Databases** > **Clinic** > **tabellen**.
2. Met de rechtermuisknop op de **patiënten** tabel en selecteer **versleutelen kolommen** om de wizard Always Encrypted te openen:
   
    ![Versleutelen van kolommen](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

De wizard Always Encrypted bevat de volgende secties: **kolomselectie**, **hoofdsleutel configuratie**, **validatie**, en **samenvatting**.

### <a name="column-selection"></a>Kolom selecteren
Klik op **volgende** op de **inleiding** pagina wordt geopend de **kolomselectie** pagina. Op deze pagina, selecteert u welke kolommen u versleutelen wilt, [het type versleuteling, en welke kolomversleutelingssleutel (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) te gebruiken.

Versleutelen **SSN** en **geboortedatum** informatie voor elke patiënt. De kolom SSN gebruikt deterministische versleuteling, die ondersteuning biedt voor gelijkheid zoekacties, joins en groeperen op. De kolom Geboortedatum gebruikt willekeurige versleuteling, die geen ondersteuning biedt voor bewerkingen.

Instellen de **versleutelingstype** voor de kolom SSN **Deterministic** en de kolom Geboortedatum **Randomized**. Klik op **Volgende**.

![Versleutelen van kolommen](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a>Configuratie van de hoofdsleutel
De **hoofdsleutel configuratie** pagina is waar u uw CMK instellen en selecteert u de sleutel van de opslagprovider waar de CMK worden opgeslagen. U kunt op dit moment een CMK opslaan in het certificaatarchief van Windows, Azure Key Vault of een hardware security module (HSM).

Deze zelfstudie laat zien hoe u kunt uw sleutels in Azure Key Vault opslaan.

1. Selecteer **Azure Key Vault**.
2. Selecteer de gewenste key vault in de vervolgkeuzelijst.
3. Klik op **Volgende**.

![Configuratie van de hoofdsleutel](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)

### <a name="validation"></a>Validatie
U kunt nu de kolommen versleutelen of opslaan van een PowerShell-script later uit te voeren. Selecteer voor deze zelfstudie **gaat u verder met de nu voltooien** en klikt u op **volgende**.

### <a name="summary"></a>Samenvatting
Controleer of de instellingen juist zijn en klik op **voltooien** om de instellingen voor altijd versleuteld.

![Samenvatting](./media/sql-database-always-encrypted-azure-key-vault/summary.png)

### <a name="verify-the-wizards-actions"></a>Controleer of de acties van de wizard
Nadat de wizard voltooid is, wordt uw database instellen voor altijd versleuteld. De wizard zijn de volgende acties uitgevoerd:

* Een hoofdsleutel voor de kolom gemaakt en opgeslagen in Azure Key Vault.
* Een kolomversleutelingssleutel gemaakt en opgeslagen in Azure Key Vault.
* De geselecteerde kolommen voor de versleuteling wordt geconfigureerd. De tabel Patiënten heeft momenteel geen gegevens, maar alle bestaande gegevens in de geselecteerde kolommen nu is versleuteld.

U kunt controleren of het maken van de sleutels in SSMS door uit te vouwen **Clinic** > **Security** > **sleutels altijd versleuteld**.

## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Een clienttoepassing die geschikt is voor de versleutelde gegevens maken
Nu dat Always Encrypted is ingesteld, kunt u een toepassing die wordt uitgevoerd bouwen *voegt* en *selecteert* voor versleutelde kolommen.  

> [!IMPORTANT]
> Uw toepassing moet gebruikmaken van [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objecten als tekst zonder opmaak gegevens wordt doorgegeven aan de server met Always Encrypted kolommen. Doorgeven van letterlijke waarden zonder SqlParameter objecten leidt tot een uitzondering.
> 
> 

1. Open Visual Studio en maak een nieuwe C# **consoletoepassing** (Visual Studio 2015 en eerder) of **Console-App (.NET Framework)** (Visual Studio 2017 en hoger). Zorg ervoor dat uw project is ingesteld op **.NET Framework 4.6** of hoger.
2. Noem het project **AlwaysEncryptedConsoleAKVApp** en klikt u op **OK**.
3. De volgende NuGet-pakketten installeren door te gaan naar **extra** > **NuGet Package Manager** > **Package Manager Console**.

Deze twee regels met code uitvoeren in de Package Manager-Console.

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Wijzigen van de verbindingsreeks om in te schakelen Always Encrypted
Deze sectie wordt uitgelegd hoe u voor het inschakelen van altijd versleuteld in de verbindingsreeks van uw database.

Om in te schakelen Always Encrypted, dat u wilt toevoegen de **kolom Versleutelingsinstelling** trefwoord dat u wilt uw verbinding tekenreeks en stel deze in op **ingeschakeld**.

U kunt deze rechtstreeks in de verbindingsreeks instellen en kunt u dit instellen met behulp van [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). De voorbeeldtoepassing in de volgende sectie ziet u hoe u **SqlConnectionStringBuilder**.

### <a name="enable-always-encrypted-in-the-connection-string"></a>Inschakelen van altijd versleuteld in de connection string
De volgende trefwoord toevoegen aan uw verbindingsreeks.

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a>Altijd versleuteld met SqlConnectionStringBuilder inschakelen
De volgende code laat zien hoe u om in te schakelen van altijd versleuteld door in te stellen [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) naar [ingeschakeld](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-the-azure-key-vault-provider"></a>De Azure Key Vault-provider registreren
De volgende code laat zien hoe de Azure Key Vault-provider registreren met de ADO.NET-stuurprogramma.

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
Dit voorbeeld laat zien hoe u:

* Wijzig de verbindingsreeks om in te schakelen altijd versleuteld.
* Meld u aan Azure Key Vault als de sleutel opslagprovider van de toepassing.  
* Gegevens invoegen in de versleutelde kolommen.
* Selecteer een record door te filteren op een specifieke waarde in een versleutelde kolom.

Vervang de inhoud van **Program.cs** door de volgende code. Vervang de verbindingsreeks voor de globale connectionString-variabele in de regel die rechtstreeks voorafgaat aan de methode Main met uw geldige verbindingsreeks vanuit Azure portal. Dit is de enige wijziging die u moet aanbrengen in deze code.

De app om te zien van altijd versleuteld in actie uitvoeren.

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
U kunt snel controleren dat de werkelijke gegevens op de server is versleuteld door het opvragen van gegevens die patiënten met SSMS (met behulp van de huidige verbinding waar **kolom Versleutelingsinstelling** is nog niet ingeschakeld).

Voer de volgende query uit op de Clinic-database.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

U kunt zien dat de versleutelde kolommen geen gegevens als tekst zonder opmaak bevatten.

   ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)

Als u SSMS wilt krijgen tot de gegevens als tekst zonder opmaak, moet u eerst om ervoor te zorgen dat de gebruiker de juiste machtigingen voor de Azure Key Vault heeft: *ophalen*, *unwrapKey*, en *controleren*. Zie voor gedetailleerde informatie [maken en Store hoofdsleutels voor kolom (Always Encrypted)](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted?view=sql-server-2017).

Voeg de *Versleutelingsinstelling kolom = ingeschakeld* parameter tijdens de verbinding.

1. In SSMS, met de rechtermuisknop op uw server in **Objectverkenner** en kies **verbinding verbreken**.
2. Klik op **Connect** > **Database-Engine** te openen de **verbinding maken met Server** venster en klikt u op **opties**.
3. Klik op **extra verbindingsparameters** en het type **Versleutelingsinstelling kolom = ingeschakeld**.
   
    ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)
4. Voer de volgende query uit op de Clinic-database.
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     U ziet nu de gegevens als tekst zonder opmaak in het versleutelde kolommen.

    ![Nieuwe consoletoepassing](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a>Volgende stappen
Nadat u een database die gebruikmaakt van altijd versleuteld hebt gemaakt, kunt u het volgende doen:

* [Draaien en het opschonen van uw sleutels](https://msdn.microsoft.com/library/mt607048.aspx).
* [Migreren van gegevens die al is versleuteld met Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).

## <a name="related-information"></a>Gerelateerde informatie
* [Altijd versleuteld (clientontwikkeling)](https://msdn.microsoft.com/library/mt147923.aspx)
* [Transparante gegevensversleuteling](https://msdn.microsoft.com/library/bb934049.aspx)
* [SQL Server-versleuteling](https://msdn.microsoft.com/library/bb510663.aspx)
* [Altijd versleutelde wizard](https://msdn.microsoft.com/library/mt459280.aspx)
* [Blog van altijd versleuteld](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

