---
title: ActiveDirectoryInteractive maakt verbinding met SQL | Microsoft Docs
description: Voorbeeld van C#-Code, met uitleg voor de verbinding met Azure SQL Database met behulp van SqlAuthenticationMethod.ActiveDirectoryInteractive modus.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: active directory
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: MirekS
ms.reviewer: GeneMi
ms.date: 04/06/2018
manager: craigg
ms.openlocfilehash: 80944e73f21d75943d4fa71c7ac9500e47bab250
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47055523"
---
# <a name="use-activedirectoryinteractive-mode-to-connect-to-azure-sql-database"></a>ActiveDirectoryInteractive modus gebruiken om te verbinden met Azure SQL Database

In dit artikel biedt een uitvoerbaar C#-codevoorbeeld dat is verbonden met uw Microsoft Azure SQL-Database. De C#-programma maakt gebruik van de interactieve modus van verificatie, die ondersteuning biedt voor Azure AD multi-factor authentication (MFA). Een poging om verbinding te kan bijvoorbeeld een verificatiecode naar uw mobiele telefoon worden verzonden bevatten.

Zie voor meer informatie over MFA-ondersteuning voor SQL-hulpprogramma's, [ondersteuning van Azure Active Directory in SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/azure-active-directory).




## <a name="sqlauthenticationmethod-activedirectoryinteractive-enum-value"></a>SqlAuthenticationMethod. ActiveDirectoryInteractive enum-waarde

.NET Framework versie 4.7.2, de enum-waarde vanaf [ **SqlAuthenticationMethod** ](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlauthenticationmethod) heeft een nieuwe waarde **. ActiveDirectoryInteractive**. Bij gebruik door een client C#-programma, is deze enum-waarde zorgt ervoor dat het systeem de interactieve modus MFA ondersteuning van Azure AD gebruiken om te verifiëren met Azure SQL Database. De gebruiker die het programma vervolgens uitvoert, ziet de volgende dialoogvensters:

1. Een dialoogvenster waarin wordt weergegeven met de naam van een Azure AD-gebruiker en dat wordt gevraagd om het wachtwoord van de Azure AD-gebruiker.
    - Dit dialoogvenster wordt niet weergegeven als er geen wachtwoord vereist is. Er is geen wachtwoord is nodig als het domein van de gebruiker is gefedereerd met Azure AD.

    Als MFA is die zijn opgelegd voor de gebruiker door het beleid is ingesteld in Azure AD, worden vervolgens de volgende dialoogvensters weergegeven.

2. Alleen geeft de eerste keer dat de gebruiker optreedt in de MFA-scenario, het systeem een dialoogvenster met een extra. Het dialoogvenster wordt gevraagd voor een mobiel telefoonnummer waarop de SMS-berichten worden verzonden. Elk bericht bevat de *verificatiecode* die de gebruiker moet invoeren in het volgende dialoogvenster.

3. Een ander dialoogvenster waarin u wordt gevraagd de MFA-verificatiecode, die het systeem naar een mobiele telefoon is verzonden.

Zie voor meer informatie over het configureren van Azure AD om MFA te vereisen [aan de slag met Azure multi-factor Authentication in de cloud](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-cloud).

Zie voor schermafbeeldingen van deze dialoogvensters [meervoudige verificatie configureren voor SQL Server Management Studio en Azure AD](sql-database-ssms-mfa-authentication-configure.md).

> [!TIP]
> Onze pagina Algemeen zoeken voor alle soorten van .NET Framework-API is beschikbaar op de volgende koppeling naar onze handige **.NET API-Browser** hulpprogramma:
>
> [https://docs.microsoft.com/dotnet/api/](https://docs.microsoft.com/dotnet/api/)
>
> Door de naam van het toe te voegen aan de optionele toegevoegd **? term =** parameter, de zoekpagina kunt onze resultaat geschikt en gereed voor ons hebben:
>
> [https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod](https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod)


## <a name="preparations-for-c-by-using-the-azure-portal"></a>Voorbereidingen voor C#, met behulp van Azure portal

Gaan we ervan uit dat er al een [Azure SQL Database-server gemaakt](sql-database-get-started-portal.md) en beschikbaar is.


### <a name="a-create-an-app-registration"></a>A. Maken van een app-registratie

Voor het gebruik van Azure AD-verificatie, dient uw C#-clientprogramma een GUID als een *clientId* wanneer uw programma wordt geprobeerd om verbinding te maken. Voltooien van de registratie van een app genereert en toont de GUID in de Azure-portal met het label **toepassings-ID**. De navigatiestappen zijn als volgt:

1. Azure-portal &gt; **Azure Active Directory** &gt; **App-registratie**

    ![App-registratie](media\active-directory-interactive-connect-azure-sql-db\sshot-create-app-registration-b20.png)

2. De **toepassings-ID** waarde wordt gegenereerd en weergegeven.

    ![App-ID weergeven](media\active-directory-interactive-connect-azure-sql-db\sshot-application-id-app-regis-mk49.png)

3. **Geregistreerde app** &gt; **instellingen** &gt; **vereiste machtigingen** &gt; **toevoegen**

    ![Machtigingsinstellingen voor geregistreerde app](media\active-directory-interactive-connect-azure-sql-db\sshot-registered-app-settings-required-permissions-add-api-access-c32.png)

4. **Vereiste machtigingen** &gt; **API-toegang toevoegen** &gt; **Select an API** &gt; **Azure SQL Database**

    ![Toegang tot API voor Azure SQL Database toevoegen](media\active-directory-interactive-connect-azure-sql-db\sshot-registered-app-settings-required-permissions-add-api-access-Azure-sql-db-d11.png)

5. **API-toegang** &gt; **machtigingen selecteren** &gt; **gedelegeerde machtigingen**

    ![Het overdragen van machtigingen voor API voor Azure SQL Database](media\active-directory-interactive-connect-azure-sql-db\sshot-add-api-access-azure-sql-db-delegated-permissions-checkbox-e14.png)


### <a name="b-set-azure-ad-admin-on-your-sql-database-server"></a>B. Instellen van Azure AD-beheerder voor uw SQL Database-server

Elke Azure SQL Database-server heeft een eigen logische SQL-server van Azure AD. Voor onze C# scenario, moet u een Azure AD-beheerder voor uw Azure SQL-server instellen.

1. **SQL Server** &gt; **Active Directory-beheerder** &gt; **beheerder instellen**

    - Zie voor meer informatie over Azure AD-beheerders en gebruikers voor Azure SQL Database, de schermafbeeldingen in [configureren en beheren van Azure Active Directory-verificatie met SQL Database](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server), in de sectie **inrichten van een Azure Active Directory-beheerder voor uw Azure SQL Database-server**.


### <a name="c-prepare-an-azure-ad-user-to-connect-to-a-specific-database"></a>C. Een Azure AD-gebruiker verbinding maakt met een specifieke database voorbereiden

In de Azure AD die specifiek is voor uw Azure SQL Database-server, kunt u een gebruiker aan wie toegang tot een bepaalde database heeft toevoegen.

Zie voor meer informatie, [gebruik Azure Active Directory-verificatie voor verificatie met SQL Database Managed Instance of SQL Data Warehouse](sql-database-aad-authentication.md).


### <a name="d-add-a-non-admin-user-to-azure-ad"></a>D. Een gebruiker niet-beheerders toevoegen aan Azure AD

Azure AD-beheerder van SQL Database-server kan worden gebruikt om verbinding maken met uw SQL Database-server. Een algemene aanvraag is echter een niet-beheerdersaccount toevoegen aan de Azure AD. Wanneer de gebruiker die geen beheerder wordt gebruikt om verbinding te maken, wordt de MFA-reeks wordt aangeroepen als MFA is opgelegd voor deze gebruiker door Azure AD.




## <a name="azure-active-directory-authentication-library-adal"></a>Azure Active Directory Authentication Library (ADAL)

De C#-programma is afhankelijk van de naamruimte **Microsoft.IdentityModel.Clients.ActiveDirectory**. De klassen voor deze naamruimte zijn in de assembly met dezelfde naam.

- Gebruik NuGet te downloaden en installeren van de ADAL-assembly.
    - [https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)

- Voeg een verwijzing naar de assembly, ter ondersteuning van een compileren van de C#-programma.




## <a name="sqlauthenticationmethod-enum"></a>SqlAuthenticationMethod enum

Een naamruimten die afhankelijk van het C#-voorbeeld is **System.Data.SqlClient**. Is van belang zijn speciale de enum **SqlAuthenticationMethod**. Deze opsomming heeft de volgende waarden:

- **SqlAuthenticationMethod.ActiveDirectory \*interactief**\*:&nbsp; Gebruik dit met de naam van een Azure AD-gebruiker om multi-factor authentication, MFA.
    - Deze waarde is de focus van dit artikel. Het genereert een interactieve ervaring door het weergeven van dialoogvensters voor het wachtwoord van de gebruiker, en vervolgens voor MFA-validatie als MFA is ingesteld op deze gebruiker.
    - Deze waarde is beschikbaar vanaf .NET Framework versie 4.7.2.

- **SqlAuthenticationMethod.ActiveDirectory \*geïntegreerde**\*:&nbsp; Gebruik deze optie voor een *federatieve* account. Voor een federatieve-account is de naam van de gebruiker bekend aan het Windows-domein. Deze methode biedt geen ondersteuning voor MFA.

- **SqlAuthenticationMethod.ActiveDirectory \*wachtwoord**\*:&nbsp; dit gebruiken voor verificatie met een Azure AD-gebruiker en wachtwoord van de gebruiker vereist. De authenticatie wordt uitgevoerd in Azure SQL-Database. Deze methode biedt geen ondersteuning voor MFA.




## <a name="prepare-c-parameter-values-from-the-azure-portal"></a>Voorbereiden van C#-parameterwaarden vanuit Azure portal

Voor een geslaagde uitvoering van de C#-programma, moet u de juiste waarden toewijzen aan de volgende statische velden. Deze statische velden fungeren als parameters aan het programma. De velden worden hier weergegeven met pretend waarden. Ook wordt weergegeven, zijn de locaties in Azure portal vanaf waar u de juiste waarden kunt krijgen:


| Statische veldnaam | Stel de waarde | Waar in Azure portal |
| :---------------- | :------------ | :-------------------- |
| Az_SQLDB_svrName | "Mijn-favoriet-sqldb-svr.database.windows.net" | **SQL-servers** &gt; **filteren op naam** |
| AzureAD_UserID | "user9@abc.onmicrosoft.com" | **Azure Active Directory** &gt; **gebruiker** &gt; **nieuwe gastgebruiker** |
| Initial_DatabaseName | 'master' | **SQL-servers** &gt; **SQL-databases** |
| ClientApplicationID | "a94f9c62-97fe-4d19-b06d-111111111111" | **Azure Active Directory** &gt; **App-registraties**<br /> &nbsp; &nbsp; &gt; **Zoeken op naam** &gt; **toepassings-ID** |
| RedirectUri | nieuwe Uri ("https://bing.com/") | **Azure Active Directory** &gt; **App-registraties**<br /> &nbsp; &nbsp; &gt; **Zoeken op naam** &gt; *[Your-App-Gereg]* &gt;<br /> &nbsp; &nbsp; **Instellingen voor** &gt; **RedirectURIs**<br /><br />In dit artikel is geldige waarde prima voor RedirectUri. De waarde is niet echt in ons geval gebruikt. |
| &nbsp; | &nbsp; | &nbsp; |


Afhankelijk van uw specifieke scenario moet u mogelijk niet alle parameters in de voorgaande tabel waarden.




## <a name="run-ssms-to-verify"></a>Uitvoeren van SSMS om te controleren

Het is handig om uit te voeren van SQL Server Management Studio (SSMS) voordat de C#-programma wordt uitgevoerd. De SSMS uitvoeren wordt gecontroleerd of verschillende configuraties juist zijn. Een storing van de C#-programma kan vervolgens kluisdetails om alleen de broncode te zijn.


#### <a name="verify-sql-database-firewall-ip-addresses"></a>Controleer of IP-adressen voor SQL Database-firewall

SSMS uitvoeren vanaf dezelfde computer, in hetzelfde gebouw, dat u later de C#-programma wordt uitgevoerd. U kunt gebruiken afhankelijk van wat **verificatie** modus u denkt dat is de eenvoudigste manier. Als er een indicatie dat de firewall van de database-server wordt niet geaccepteerd voor uw IP-adres, kunt u die verhelpen zoals wordt weergegeven in [Azure SQL Database op serverniveau en databaseniveau firewallregels](sql-database-firewall-configure.md).


#### <a name="verify-multi-factor-authentication-mfa-for-azure-ad"></a>Controleer of de multi-factor authentication (MFA) voor Azure AD

SSMS opnieuw uitvoert, dit keer met **verificatie** ingesteld op **Active Directory - Universal met ondersteuning voor MFA**. Voor deze optie moet u SSMS 17,5 of hoger hebben.

Zie voor meer informatie, [configureren multi-factor authentication voor SSMS en Azure AD](sql-database-ssms-mfa-authentication-configure.md).




## <a name="c-code-example"></a>Voorbeeld van C#-code

In dit voorbeeld met C# compileren, moet u een verwijzing naar het DLL-assembly met de naam toevoegen **Microsoft.IdentityModel.Clients.ActiveDirectory**.


#### <a name="reference-documentation"></a>Referentiedocumentatie

- **System.Data.SqlClient** naamruimte:
    - Zoeken:&nbsp; [https://docs.microsoft.com/dotnet/api/?term=System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/?term=System.Data.SqlClient)
    - Direct:&nbsp; [System.Data.Client](https://docs.microsoft.com/dotnet/api/system.data.sqlclient)

- **Microsoft.IdentityModel.Clients.ActiveDirectory** naamruimte:
    - Zoeken:&nbsp; [https://docs.microsoft.com/dotnet/api/?term=Microsoft.IdentityModel.Clients.ActiveDirectory](https://docs.microsoft.com/dotnet/api/?term=Microsoft.IdentityModel.Clients.ActiveDirectory)
    - Direct:&nbsp; [Microsoft.IdentityModel.Clients.ActiveDirectory](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory)


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

#### <a name="second-half-of-the-c-program"></a>Tweede helft van de C#-programma

Voor betere visuals moeten worden weergegeven, is het C#-programma opgesplitst in twee codeblokken. Als u wilt het programma uitvoert, moet u de twee codeblokken samen plakken.

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

