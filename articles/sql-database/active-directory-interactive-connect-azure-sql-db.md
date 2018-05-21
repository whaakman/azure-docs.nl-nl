---
title: ActiveDirectoryInteractive verbinding maakt met SQL | Microsoft Docs
description: C#-Code voorbeeld met uitleg voor het verbinden met Azure SQL Database met behulp van SqlAuthenticationMethod.ActiveDirectoryInteractive-modus.
services: sql-database
author: GithubMirek
manager: craigg
ms.service: sql-database
ms.custom: active directory
ms.topic: article
ms.date: 04/06/2018
ms.author: MirekS
ms.reviewer: GeneMi
ms.openlocfilehash: 6489fb5630e1990c942b461859650e2e469cda73
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="use-activedirectoryinteractive-mode-to-connect-to-azure-sql-database"></a>ActiveDirectoryInteractive modus gebruiken om te verbinden met Azure SQL Database

Dit artikel bevat een uitvoerbare C# codevoorbeeld die verbinding met uw Microsoft Azure SQL Database maakt. De C#-programma maakt gebruik van de interactieve modus van verificatie, die ondersteuning biedt voor Azure AD multi-factor authentication (MFA). Een poging om verbinding te kan bijvoorbeeld een verificatiecode naar uw mobiele telefoon verzonden bevatten.

Zie voor meer informatie over MFA-ondersteuning voor hulpprogramma's voor SQL [Azure Active Directory-ondersteuning in SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/azure-active-directory).




## <a name="sqlauthenticationmethod-activedirectoryinteractive-enum-value"></a>SqlAuthenticationMethod. ActiveDirectoryInteractive enum-waarde

Vanaf versie van .NET Framework 4.7.2, de enum [ **SqlAuthenticationMethod** ](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlauthenticationmethod) heeft een nieuwe waarde **. ActiveDirectoryInteractive**. Bij gebruik door een clientprogramma C#, deze enum-waarde zorgt ervoor dat de interactieve modus van Azure AD ondersteunen van MFA gebruiken om te verifiëren met Azure SQL Database. De gebruiker die het programma vervolgens voert ziet de volgende dialoogvensters:

1. Een dialoogvenster waarin wordt weergegeven met de naam van een Azure AD-gebruiker en die vraagt om het wachtwoord van de Azure AD-gebruiker.
    - Dit dialoogvenster wordt niet weergegeven als er geen wachtwoord is vereist. Er is geen wachtwoord is nodig als het domein van de gebruiker is gefedereerd met Azure AD.

    Als MFA is op de gebruiker die zijn opgelegd door het beleid dat is ingesteld in Azure AD, worden de volgende dialoogvensters naast weergegeven.

2. Alleen geeft de eerste keer dat de gebruiker merkt dat het MFA-scenario, het systeem een extra dialoogvenster. Het dialoogvenster gevraagd of u een mobiel telefoonnummer waarop de SMS-berichten worden verzonden. Elk bericht biedt de *verificatiecode* die de gebruiker moet invoeren in het volgende dialoogvenster.

3. Een ander dialoogvenster waarin u wordt gevraagd de verificatiecode MFA die het systeem is verzonden naar een mobiele telefoon.

Zie voor meer informatie over het configureren van Azure AD om MFA vereisen [aan de slag met Azure multi-factor Authentication in de cloud](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-cloud).

Zie voor schermafbeeldingen van deze dialoogvensters [configureren multi-factor authentication voor SQL Server Management Studio en Azure AD](sql-database-ssms-mfa-authentication-configure.md).

> [!TIP]
> Onze algemene zoekpagina voor alle soorten van .NET Framework-API's is beschikbaar op de volgende koppeling om onze handige **.NET API Browser** hulpprogramma:
>
> [https://docs.microsoft.com/dotnet/api/](https://docs.microsoft.com/dotnet/api/)
>
> Door de naam van het type toe te voegen aan de optionele toegevoegd **? term =** parameter, de zoekpagina kan hebben onze resultaat gereed en in afwachting van ons:
>
> [https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod](https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod)


## <a name="preparations-for-c-by-using-the-azure-portal"></a>Voorbereidingen voor C#, met behulp van de Azure-portal

We gaan ervan uit dat er al een [Azure SQL Database-server gemaakt](sql-database-get-started-portal.md) en beschikbaar zijn.


### <a name="a-create-an-app-registration"></a>A. De registratie van een app maken

Azure AD om verificatie te gebruiken, Geef uw C#-clientprogramma een GUID als een *clientId* wanneer uw programma verbinding probeert te maken. Voltooien van de registratie van een app wordt gegenereerd en de GUID weergegeven in de Azure portal, met het label **toepassings-ID**. De navigatiestappen zijn als volgt:

1. Azure-portal &gt; **Azure Active Directory** &gt; **App-registratie**

    ![App-registratie](media\active-directory-interactive-connect-azure-sql-db\sshot-create-app-registration-b20.png)

2. De **toepassings-ID** waarde gegenereerd en weergegeven.

    ![App-ID weergeven](media\active-directory-interactive-connect-azure-sql-db\sshot-application-id-app-regis-mk49.png)

3. **Geregistreerde app** &gt; **instellingen** &gt; **vereist machtigingen** &gt; **toevoegen**

    ![Machtigingsinstellingen voor de geregistreerde app](media\active-directory-interactive-connect-azure-sql-db\sshot-registered-app-settings-required-permissions-add-api-access-c32.png)

4. **Vereiste machtigingen** &gt; **API toevoegen toegang** &gt; **selecteert u een API** &gt; **Azure SQL Database**

    ![Toegang tot API voor Azure SQL Database toevoegen](media\active-directory-interactive-connect-azure-sql-db\sshot-registered-app-settings-required-permissions-add-api-access-Azure-sql-db-d11.png)

5. **API-toegang** &gt; **machtigingen selecteren** &gt; **overgedragen machtigingen**

    ![Machtigingen delegeren aan API voor Azure SQL Database](media\active-directory-interactive-connect-azure-sql-db\sshot-add-api-access-azure-sql-db-delegated-permissions-checkbox-e14.png)


### <a name="b-set-azure-ad-admin-on-your-sql-database-server"></a>B. Azure AD-beheerder instellen op de SQL-databaseserver

Elke Azure SQL Database-server heeft een eigen logische SQL-server van Azure AD. U moet een Azure AD-beheerder voor uw Azure SQL-server instellen voor ons scenario C#.

1. **SQL Server** &gt; **Active Directory-beheerder** &gt; **beheerder instellen**

    - Zie voor meer informatie over Azure AD-beheerders en gebruikers voor Azure SQL Database, de schermafbeeldingen in [configureren en beheren van Azure Active Directory-verificatie met SQL Database](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server), in de sectie **inrichten van een Azure Active Directory-beheerder voor uw Azure SQL Database-server**.


### <a name="c-prepare-an-azure-ad-user-to-connect-to-a-specific-database"></a>C. Een Azure AD-gebruiker verbinding maakt met een specifieke database voorbereiden

In de Azure AD die specifiek is voor uw Azure SQL Database-server, kunt u een gebruiker aan wie toegang tot een bepaalde database heeft toevoegen.

Zie voor meer informatie [Azure Active Directory-verificatie gebruiken voor verificatie bij SQL-Database, exemplaar beheerd of SQL Data Warehouse](sql-database-aad-authentication.md).


### <a name="d-add-a-non-admin-user-to-azure-ad"></a>D. Een gebruiker niet-beheerders toevoegen aan Azure AD

De Azure AD-beheerder van SQL Database-server kan worden gebruikt voor verbinding met uw SQL-Database-server. Een algemene aanvraag is echter een gebruiker niet-beheerders toevoegen aan de Azure AD. Wanneer de gebruiker niet-beheerders waarmee verbinding wordt gemaakt, wordt de MFA-volgorde aangeroepen als MFA is opgelegd voor deze gebruiker door Azure AD.




## <a name="azure-active-directory-authentication-library-adal"></a>Azure Active Directory Authentication Library (ADAL)

De C#-programma is afhankelijk van de naamruimte **Microsoft.IdentityModel.Clients.ActiveDirectory**. De klassen voor deze naamruimte zijn in de assembly met dezelfde naam.

- Gebruik NuGet om te downloaden en installeren van de ADAL-assembly.
    - [https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)

- Voeg een verwijzing naar de assembly, ter ondersteuning van een compileren van het C#-programma.




## <a name="sqlauthenticationmethod-enum"></a>SqlAuthenticationMethod enum

Een naamruimten die in het C#-voorbeeld is afhankelijk van is **System.Data.SqlClient**. Is van belang zijn speciale de enum **SqlAuthenticationMethod**. Deze opsomming heeft de volgende waarden:

- **SqlAuthenticationMethod.ActiveDirectory*interactief ***:&nbsp; Gebruik deze met de naam van een Azure AD-gebruiker, als u de multi-factor authentication, MFA.
    - Deze waarde is de focus van dit artikel. Het genereert een interactieve ervaring dialoogvensters voor het wachtwoord van de gebruiker en voor MFA-validatie wordt weergegeven als MFA is opgelegd voor deze gebruiker.
    - Deze waarde is beschikbaar vanaf met .NET Framework versie 4.7.2.

- **SqlAuthenticationMethod.ActiveDirectory*geïntegreerd ***:&nbsp; Gebruik deze optie voor een *federatieve* account. Voor een federatieve-account, is de gebruikersnaam bekend dat het Windows-domein. Deze methode biedt geen ondersteuning voor MFA.

- **SqlAuthenticationMethod.ActiveDirectory*wachtwoord ***:&nbsp; Gebruik deze voor verificatie met een Azure AD-gebruiker en het wachtwoord van de gebruiker vereist. De authenticatie wordt uitgevoerd in Azure SQL-Database. Deze methode biedt geen ondersteuning voor MFA.




## <a name="prepare-c-parameter-values-from-the-azure-portal"></a>Voorbereiden van C#-parameterwaarden vanuit de Azure-portal

Voor een geslaagde uitvoering van het C#-programma, moet u de juiste waarden toewijzen aan de volgende statische velden. Deze statische velden fungeren als parameters in het programma. De velden worden hier weergegeven met pretend waarden. Ook wordt weergegeven, zijn de locaties in de Azure-portal waar u de juiste waarden kunt verkrijgen:


| De naam van statisch veld | Stel de waarde | Waar in Azure-portal |
| :---------------- | :------------ | :-------------------- |
| Az_SQLDB_svrName | 'Mijn-favoriet-sqldb-svr.database.windows.net' | **SQL-servers** &gt; **filteren op naam** |
| AzureAD_UserID | "user9@abc.onmicrosoft.com" | **Azure Active Directory** &gt; **gebruiker** &gt; **nieuwe gastgebruiker** |
| Initial_DatabaseName | 'master' | **SQL-servers** &gt; **SQL-databases** |
| ClientApplicationID | 'a94f9c62-97fe-4d19-b06d-111111111111' | **Azure Active Directory** &gt; **App registraties**<br /> &nbsp; &nbsp; &gt; **Zoeken op naam** &gt; **toepassings-ID** |
| redirectUri | nieuwe Uri ('https://bing.com/') | **Azure Active Directory** &gt; **App registraties**<br /> &nbsp; &nbsp; &gt; **Zoeken op naam** &gt; *[uw-App-inschrijvingsrechten]* &gt;<br /> &nbsp; &nbsp; **Instellingen** &gt; **RedirectURIs**<br /><br />Voor dit artikel is geldige waarde voor RedirectUri fijn. De waarde is niet echt in ons geval gebruikt. |
| &nbsp; | &nbsp; | &nbsp; |


Afhankelijk van uw specifieke scenario moet u mogelijk geen waarden van de parameters in de voorgaande tabel.




## <a name="run-ssms-to-verify"></a>Uitvoeren van SSMS controleren

Het is nuttig om uit te voeren van SQL Server Management Studio (SSMS) voordat u de C#-programma uitvoert. De SSMS uitgevoerd wordt gecontroleerd of verschillende configuraties juist zijn. Een storing van de C#-programma kan vervolgens Overzicht alleen de broncode zijn.


#### <a name="verify-sql-database-firewall-ip-addresses"></a>Controleer of SQL-Database firewall IP-adressen

SSMS uitvoeren vanaf de computer, in hetzelfde gebouw, dat u later de C#-programma wordt uitgevoerd. U kunt gebruiken afhankelijk van wat **verificatie** modus u denkt dat is de eenvoudigste manier. Als er een aanwijzing dat de firewall van de database-server geen uw IP-adres accepteert, u kunt dit oplossen zoals weergegeven in [Azure SQL Database server- en databaseniveau firewallregels](sql-database-firewall-configure.md).


#### <a name="verify-multi-factor-authentication-mfa-for-azure-ad"></a>Controleer of de multi-factor authentication (MFA) voor Azure AD

SSMS nogmaals uitvoert, dit keer met **verificatie** ingesteld op **Active Directory - Universal met ondersteuning voor MFA**. Voor deze optie moet u SSMS 17,5 of hoger hebben.

Zie voor meer informatie [configureren multi-factor authentication voor SSMS en Azure AD](sql-database-ssms-mfa-authentication-configure.md).




## <a name="c-code-example"></a>Voorbeeld van C#-code

In dit voorbeeld C# compileren, moet u een verwijzing naar het DLL-assembly met de naam toevoegen **Microsoft.IdentityModel.Clients.ActiveDirectory**.


#### <a name="reference-documentation"></a>Referentiedocumentatie

- **System.Data.SqlClient** naamruimte:
    - Zoeken:&nbsp; [https://docs.microsoft.com/dotnet/api/?term=System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/?term=System.Data.SqlClient)
    - Directe:&nbsp; [System.Data.Client](https://docs.microsoft.com/dotnet/api/system.data.sqlclient)

- **Microsoft.IdentityModel.Clients.ActiveDirectory** naamruimte:
    - Zoeken:&nbsp; [https://docs.microsoft.com/dotnet/api/?term=Microsoft.IdentityModel.Clients.ActiveDirectory](https://docs.microsoft.com/dotnet/api/?term=Microsoft.IdentityModel.Clients.ActiveDirectory)
    - Directe:&nbsp; [Microsoft.IdentityModel.Clients.ActiveDirectory](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory)


#### <a name="c-source-code-in-two-parts"></a>C#-broncode, uit twee delen

&nbsp;

```csharp

using System;    // C# ,  part 1 of 2.

// Add a reference to assembly:  Microsoft.IdentityModel.Clients.ActiveDirectory.DLL
using Microsoft.IdentityModel.Clients.ActiveDirectory;

using DA = System.Data;
using SC = System.Data.SqlClient;
using AD = Microsoft.IdentityModel.Clients.ActiveDirectory;
using TX = System.Text;
using TT = System.Threading.Tasks;

namespace ADInteractive5
{
    class Program
    {
        // ASSIGN YOUR VALUES TO THESE STATIC FIELDS !!
        static public string Az_SQLDB_svrName = "<YOUR VALUE HERE>";
        static public string AzureAD_UserID = "<YOUR VALUE HERE>";
        static public string Initial_DatabaseName = "master";
        // Some scenarios do not need values for the following two fields:
        static public readonly string ClientApplicationID = "<YOUR VALUE HERE>";
        static public readonly Uri RedirectUri = new Uri("<YOUR VALUE HERE>");

        public static void Main(string[] args)
        {
            var provider = new ActiveDirectoryAuthProvider();

            SC.SqlAuthenticationProvider.SetProvider(
                SC.SqlAuthenticationMethod.ActiveDirectoryInteractive,
                //SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated,  // Alternatives.
                //SC.SqlAuthenticationMethod.ActiveDirectoryPassword,
                provider);

            Program.Connection();
        }

        public static void Connection()
        {
            SC.SqlConnectionStringBuilder builder = new SC.SqlConnectionStringBuilder();

            // Program._  static values that you set earlier.
            builder["Data Source"] = Program.Az_SQLDB_svrName;
            builder.UserID = Program.AzureAD_UserID;
            builder["Initial Catalog"] = Program.Initial_DatabaseName;

            // This "Password" is not used with .ActiveDirectoryInteractive.
            //builder["Password"] = "<YOUR PASSWORD HERE>";

            builder["Connect Timeout"] = 15;
            builder["TrustServerCertificate"] = true;
            builder.Pooling = false;

            // Assigned enum value must match the enum given to .SetProvider().
            builder.Authentication = SC.SqlAuthenticationMethod.ActiveDirectoryInteractive;
            SC.SqlConnection sqlConnection = new SC.SqlConnection(builder.ConnectionString);

            SC.SqlCommand cmd = new SC.SqlCommand(
                "SELECT '******** MY QUERY RAN SUCCESSFULLY!! ********';",
                sqlConnection);

            try
            {
                sqlConnection.Open();
                if (sqlConnection.State == DA.ConnectionState.Open)
                {
                    var rdr = cmd.ExecuteReader();
                    var msg = new TX.StringBuilder();
                    while (rdr.Read())
                    {
                        msg.AppendLine(rdr.GetString(0));
                    }
                    Console.WriteLine(msg.ToString());
                    Console.WriteLine(":Success");
                }
                else
                {
                    Console.WriteLine(":Failed");
                }
                sqlConnection.Close();
            }
            catch (Exception ex)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("Connection failed with the following exception...");
                Console.WriteLine(ex.ToString());
                Console.ResetColor();
            }
        }
    } // EOClass Program .

```

&nbsp;

#### <a name="second-half-of-the-c-program"></a>Tweede helft van het C#-programma

Voor betere visuele weergave, wordt de C#-programma opgesplitst in twee codeblokken. Om het programma uitvoert, de twee codeblokken samen te plakken.

&nbsp;

```csharp

    // C# ,  part 2 of 2 ,  to concatenate below part 1.

    /// <summary>
    /// SqlAuthenticationProvider - Is a public class that defines 3 different Azure AD
    /// authentication methods.  The methods are supported in the new .NET 4.7.2 .
    ///  . 
    /// 1. Interactive,  2. Integrated,  3. Password
    ///  . 
    /// All 3 authentication methods are based on the Azure
    /// Active Directory Authentication Library (ADAL) managed library.
    /// </summary>
    public class ActiveDirectoryAuthProvider : SC.SqlAuthenticationProvider
    {
        // Program._ more static values that you set!
        private readonly string _clientId = Program.ClientApplicationID;
        private readonly Uri _redirectUri = Program.RedirectUri;

        public override async TT.Task<SC.SqlAuthenticationToken>
            AcquireTokenAsync(SC.SqlAuthenticationParameters parameters)
        {
            AD.AuthenticationContext authContext =
                new AD.AuthenticationContext(parameters.Authority);
            authContext.CorrelationId = parameters.ConnectionId;
            AD.AuthenticationResult result;

            switch (parameters.AuthenticationMethod)
            {
                case SC.SqlAuthenticationMethod.ActiveDirectoryInteractive:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_0 == '.ActiveDirectoryInteractive'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,  // "https://database.windows.net/"
                        _clientId,
                        _redirectUri,
                        new AD.PlatformParameters(AD.PromptBehavior.Auto),
                        new AD.UserIdentifier(
                            parameters.UserId,
                            AD.UserIdentifierType.RequiredDisplayableId));
                    break;

                case SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_1 == '.ActiveDirectoryIntegrated'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,
                        _clientId,
                        new AD.UserCredential());
                    break;

                case SC.SqlAuthenticationMethod.ActiveDirectoryPassword:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_2 == '.ActiveDirectoryPassword'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,
                        _clientId,
                        new AD.UserPasswordCredential(
                            parameters.UserId,
                            parameters.Password));
                    break;

                default: throw new InvalidOperationException();
            }
            return new SC.SqlAuthenticationToken(result.AccessToken, result.ExpiresOn);
        }

        public override bool IsSupported(SC.SqlAuthenticationMethod authenticationMethod)
        {
            return authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated
                || authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryInteractive
                || authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryPassword;
        }
    } // EOClass ActiveDirectoryAuthProvider .
} // EONamespace.  End of entire program source code.

```

&nbsp;

#### <a name="actual-test-output-from-c"></a>Werkelijke testuitvoer van C#

```
[C:\Test\VSProj\ADInteractive5\ADInteractive5\bin\Debug\]
>> ADInteractive5.exe
In method 'AcquireTokenAsync', case_0 == '.ActiveDirectoryInteractive'.
******** MY QUERY RAN SUCCESSFULLY!! ********

:Success

[C:\Test\VSProj\ADInteractive5\ADInteractive5\bin\Debug\]
>>
```

&nbsp;


## <a name="next-steps"></a>Volgende stappen

- [Get-AzureRmSqlServerActiveDirectoryAdministrator](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlserveractivedirectoryadministrator)

