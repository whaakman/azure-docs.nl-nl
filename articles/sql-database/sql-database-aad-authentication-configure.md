---
title: Azure Active Directory - SQL-verificatie configureren | Microsoft Docs
description: Informatie over het verbinding maken met SQL-Database en SQL Data Warehouse met behulp van Azure Active Directory-verificatie - na het configureren van Azure AD.
services: sql-database
author: GithubMirek
manager: johammer
ms.assetid: 7e2508a1-347e-4f15-b060-d46602c5ce7e
ms.service: sql-database
ms.custom: security
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: Active
ms.date: 01/09/2018
ms.author: mireks
ms.openlocfilehash: 93fb39770a0b0c63011c05505be411c7470fea0a
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="configure-and-manage-azure-active-directory-authentication-with-sql-database-or-sql-data-warehouse"></a>Configureren en beheren van Azure Active Directory-verificatie met SQL-Database of SQL Data Warehouse

In dit artikel leest u hoe maken en Azure AD te vullen en vervolgens Azure AD te gebruiken met Azure SQL Database en SQL Data Warehouse. Zie voor een overzicht [Azure Active Directory-verificatie](sql-database-aad-authentication.md).

>  [!NOTE]  
>  Verbinding maken met SQL Server wordt uitgevoerd op een Azure VM wordt niet ondersteund met een Azure Active Directory-account. Gebruik in plaats daarvan de Active Directory-account van een domein.

## <a name="create-and-populate-an-azure-ad"></a>U maakt en vult u een Azure AD
Maak een Azure AD en deze vullen met gebruikers en groepen. Azure AD de eerste Azure AD kan worden beheerd domein. Azure AD kan ook worden voor een lokale Active Directory Domain Services die is gefedereerd met Azure AD.

Zie [Uw on-premises identiteiten integreren met Azure Active Directory](../active-directory/active-directory-aadconnect.md), [Uw domeinnaam toevoegen in Azure AD](../active-directory/active-directory-domains-add-azure-portal.md), [Microsoft Azure ondersteunt nu federatie met Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Uw Azure AD-adreslijst beheren](../active-directory/active-directory-administer.md), [Azure AD beheren met Windows PowerShell](/powershell/azure/overview?view=azureadps-2.0) en [Poorten en protocollen waarvoor hybride identiteit is vereist](..//active-directory/connect/active-directory-aadconnect-ports.md) voor meer informatie.

## <a name="associate-or-add-an-azure-subscription-to-azure-active-directory"></a>Koppelen of een Azure-abonnement toevoegen aan Azure Active Directory

1. Koppel uw Azure-abonnement met Azure Active Directory door het maken van de map een vertrouwde map voor het hosten van de database Azure-abonnement. Zie voor meer informatie [hoe Azure-abonnementen worden gekoppeld aan Azure AD](../active-directory/active-directory-how-subscriptions-associated-directory.md).
2. Gebruik de schakelbaar directory in de Azure-portal overschakelen naar het abonnement is gekoppeld aan een domein.

   **Aanvullende informatie:** om Azure-abonnement heeft een vertrouwensrelatie met een Azure AD-exemplaar. Dit betekent dat er op die directory wordt vertrouwd voor het verifiëren van gebruikers, services en apparaten. Meerdere abonnementen kunnen dezelfde directory vertrouwen, maar een abonnement vertrouwt slechts één directory. De vertrouwensrelatie die een abonnement heeft met een directory is anders dan de relatie die een abonnement heeft met andere resources in Azure (websites, databases, enzovoort); deze resources lijken meer op onderliggende resources van een abonnement. Als een abonnement is verlopen, wordt toegang tot de andere resources die zijn gekoppeld aan het abonnement ook geblokkeerd. De directory blijft echter wel aanwezig in Azure, en u kunt er een ander abonnement aan koppelen om de directorygebruikers te blijven beheren. Zie voor meer informatie over bronnen [informatie over toegang tot bronnen in Azure](../active-directory/active-directory-b2b-admin-add-users.md). Voor meer informatie over deze vertrouwde relatie Zie [koppelen of een Azure-abonnement toevoegen aan Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="create-an-azure-ad-administrator-for-azure-sql-server"></a>Een Azure AD-beheerder voor Azure SQL-server maken
Elke Azure SQL-server (die als host fungeert voor een SQL-Database of SQL Data Warehouse) begint met een administrator-account voor één server die de beheerder van de hele Azure SQL-server. Een tweede SQL Server-beheerder moet worden gemaakt, die een Azure AD-account. Deze principal is gemaakt als de gebruiker van een ingesloten database in de database master. Als beheerder, het server-beheerder gebruikersaccounts lid zijn van de **db_owner** rol in elke gebruiker van de database en voert u de gebruikersdatabase van elke als de **dbo** gebruiker. Zie voor meer informatie over de beheerdersaccounts server [het beheren van Databases en aanmeldingen in Azure SQL Database](sql-database-manage-logins.md).

Als u Azure Active Directory met geo-replicatie, moet de Azure Active Directory-beheerder voor zowel de primaire en secundaire servers worden geconfigureerd. Als een server geen beheerder van de Azure Active Directory heeft, ontvangt Azure Active Directory-aanmeldingen en gebruikers een ' kan geen verbinding maken' op serverfout.

> [!NOTE]
> Gebruikers die niet zijn gebaseerd op een Azure AD-account (inclusief het administrator-account van de Azure SQL-server), kan Azure AD-gebruikers, maken, omdat ze geen machtiging voor het valideren van de voorgestelde databasegebruikers met Azure AD.
> 

## <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-server"></a>Een Azure Active Directory-beheerder voor uw Azure SQL-server inrichten

De volgende twee procedures laten zien hoe een Azure Active Directory-beheerder voor uw Azure SQL-server in de Azure-portal en met behulp van PowerShell inrichten.

### <a name="azure-portal"></a>Azure Portal
1. In de [Azure-portal](https://portal.azure.com/), klik op de verbinding met een vervolgkeuzelijst met mogelijke Active Directory's in de rechterbovenhoek. Kies de juiste Active Directory als de standaard Azure AD. Deze stap koppelt u de koppeling van het abonnement met Active Directory met Azure SQL-server om ervoor te zorgen dat hetzelfde abonnement wordt gebruikt voor zowel Azure AD en SQL Server. (De Azure SQL-server kan worden gehost, Azure SQL Database of Azure SQL Data Warehouse.)   
    ![Kies ad][8]   
    
2. Selecteer in het linkerdeelvenster banner **SQL-servers**, selecteer uw **SQL server**, en klik vervolgens in de **SQL Server** blade, klikt u op **Active Directory-beheerder** .   
3. In de **Active Directory-beheerder** blade, klikt u op **beheerder instellen**.   
    ![Selecteer active directory](./media/sql-database-aad-authentication/select-active-directory.png)  
    
4. In de **admin toevoegen** blade, zoekt u naar een gebruiker selecteert de gebruiker of groep een beheerder en klik vervolgens op **Selecteer**. (De Active Directory-beheerder blade ziet u alle leden en groepen van Active Directory. Gebruikers of groepen die grijs worden weergegeven, kunnen niet worden geselecteerd omdat ze worden niet ondersteund als Azure AD-beheerders. (Zie de lijst met ondersteunde beheerders in de **Azure AD-functies en beperkingen** sectie van [Azure Active Directory-verificatie gebruiken voor verificatie bij SQL-Database of SQL Data Warehouse](sql-database-aad-authentication.md).) Op rollen gebaseerde toegangsbeheer (RBAC) geldt alleen voor de portal en niet wordt doorgegeven naar SQL Server.   
    ![Selecteer beheerder](./media/sql-database-aad-authentication/select-admin.png)  
    
5. Aan de bovenkant van de **Active Directory-beheerder** blade, klikt u op **opslaan**.   
    ![beheerder opslaan](./media/sql-database-aad-authentication/save-admin.png)   

Het proces van het wijzigen van de beheerder kan enkele minuten duren. En vervolgens de nieuwe beheerder wordt weergegeven in de **Active Directory-beheerder** vak.

   > [!NOTE]
   > Bij het instellen van de Azure AD-beheerder, kan niet de nieuwe admin-naam (gebruiker of groep) al aanwezig zijn in de virtuele-hoofddatabase als de gebruiker van een SQL Server-verificatie. Indien aanwezig, mislukt de installatie van Azure AD-beheerder; het maken ervan worden teruggedraaid en waarmee wordt aangegeven dat deze een beheerder (naam) al bestaat. Omdat deze een SQL Server authentication-gebruiker geen deel uit van de Azure AD maakt, worden alle inspanningen verbinding maken met de server met Azure AD-verificatie mislukt.
   > 


Later verwijderen van een beheerder, aan de bovenkant van de **Active Directory-beheerder** blade, klikt u op **admin verwijderen**, en klik vervolgens op **opslaan**.

### <a name="powershell"></a>PowerShell
Voor het uitvoeren van PowerShell-cmdlets, moet u Azure PowerShell is geïnstalleerd en actief zijn. Zie voor gedetailleerde informatie [Installeren en configureren van Azure PowerShell](/powershell/azure/overview).

Voer de volgende Azure PowerShell-opdrachten voor het inrichten van een Azure AD-beheerder:

* Add-AzureRmAccount
* SELECT-AzureRmSubscription

Cmdlets gebruikt voor het inrichten en beheren van Azure AD-beheerder:

| Naam van cmdlet | Beschrijving |
| --- | --- |
| [Set-AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/set-azurermsqlserveractivedirectoryadministrator) |Voorziet in een Azure Active Directory-beheerder voor Azure SQL-server of Azure SQL Data Warehouse. (Moet van het huidige abonnement.) |
| [Verwijder AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/remove-azurermsqlserveractivedirectoryadministrator) |Hiermee verwijdert u een Azure Active Directory-beheerder voor Azure SQL-server of Azure SQL Data Warehouse. |
| [Get-AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/get-azurermsqlserveractivedirectoryadministrator) |Retourneert informatie over een Azure Active Directory-beheerder die momenteel zijn geconfigureerd voor de Azure SQL-server of Azure SQL Data Warehouse. |

Gebruik PowerShell-opdracht get-help voor meer informatie voor elk van deze opdrachten, bijvoorbeeld ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

Het volgende script bepalingen beheerder Azure AD-groep met de naam **DBA_Group** (object-id `40b79501-b343-44ed-9ce7-da4c8cc7353f`) voor de **demo_server** server in een resourcegroep met de naam **groep-23**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23"
-ServerName "demo_server" -DisplayName "DBA_Group"
```

De **DisplayName** invoerparameter accepteert u de weergavenaam van Azure AD of de UPN-naam. Bijvoorbeeld: ``DisplayName="John Smith"`` en ``DisplayName="johns@contoso.com"``. Voor Azure AD-groepen alleen de Azure AD wordt weergavenaam ondersteund.

> [!NOTE]
> De Azure PowerShell-opdracht ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` voorkomt niet dat u Azure AD-beheerders voor niet-ondersteunde gebruikers inrichten. Een niet-ondersteunde gebruiker kan worden ingericht, maar kan geen verbinding maken met een database. 
> 

Het volgende voorbeeld wordt de optionele **ObjectID**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23"
-ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [!NOTE]
> De Azure AD **ObjectID** is vereist wanneer de **DisplayName** is niet uniek. Voor het ophalen van de **ObjectID** en **DisplayName** waarden, gebruikt u de Active Directory-sectie van de klassieke Azure-Portal en bekijk de eigenschappen van een gebruiker of groep.
> 

Het volgende voorbeeld retourneert informatie over de huidige Azure AD-beheerder voor Azure SQL-server:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" | Format-List
```

Het volgende voorbeeld wordt een Azure AD-beheerder verwijderd.

```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server"
```

U kunt ook een Azure Active Directory-beheerder inrichten met behulp van de REST API's. Zie voor meer informatie [Service Management REST API-verwijzing en bewerkingen voor Azure SQL-Databases bewerkingen voor Azure SQL-Databases](https://msdn.microsoft.com/library/azure/dn505719.aspx)

### <a name="cli"></a>CLI  
U kunt ook een Azure AD-beheerder inrichten door het aanroepen van de volgende CLI-opdrachten:
| Opdracht | Beschrijving |
| --- | --- |
|[AZ sql server ad-beheerder maken](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az_sql_server_ad_admin_create) |Voorziet in een Azure Active Directory-beheerder voor Azure SQL-server of Azure SQL Data Warehouse. (Moet van het huidige abonnement.) |
|[AZ sql server ad-beheerder verwijderen](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az_sql_server_ad_admin_delete) |Hiermee verwijdert u een Azure Active Directory-beheerder voor Azure SQL-server of Azure SQL Data Warehouse. |
|[lijst met AZ sql server ad-beheerder](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az_sql_server_ad_admin_list) |Retourneert informatie over een Azure Active Directory-beheerder die momenteel zijn geconfigureerd voor de Azure SQL-server of Azure SQL Data Warehouse. |
|[update van AZ sql server ad-beheerder](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az_sql_server_ad_admin_update) |De Active Directory-beheerder voor een Azure SQL-server of Azure SQL Data Warehouse-updates. |

Zie voor meer informatie over de CLI-opdrachten, [SQL - az sql](https://docs.microsoft.com/cli/azure/sql/server).  


## <a name="configure-your-client-computers"></a>De clientcomputers configureren
U moet de volgende software installeren op alle clientcomputers waarvan uw toepassingen of gebruikers verbinding maken met Azure SQL Database of Azure SQL Data Warehouse met behulp van Azure AD-identiteiten:

* .NET framework 4.6 of hoger van [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
* Azure Active Directory Authentication Library voor SQL Server (**ADALSQL. DLL-bestand**) is beschikbaar in meerdere talen (x x86- en amd64) uit het Downloadcentrum op [Microsoft Active Directory Authentication Library voor Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742).

U kunt voldoen aan deze voorwaarden voldoet:

* Één installeert [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) of [SQL Server Data Tools voor Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) voldoet aan de vereisten voor .NET Framework 4.6.
* Versie van SSMS installeert de x86 van **ADALSQL. DLL-bestand**.
* SSDT installeert de amd64-versie van **ADALSQL. DLL-bestand**.
* De meest recente Visual Studio van [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) voldoet aan de vereisten voor .NET Framework 4.6, maar wordt niet geïnstalleerd voor de vereiste amd64-versie van **ADALSQL. DLL-bestand**.

## <a name="create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>Ingesloten databasegebruikers in de database die is toegewezen aan Azure AD-identiteiten maken

Azure Active Directory-verificatie vereist databasegebruikers moet worden gemaakt als ingesloten databasegebruikers. Een ingesloten database-gebruiker op basis van een Azure AD-identiteit is een databasegebruiker die geen een aanmelding in de database master en die wordt toegewezen aan een identiteit in de Azure AD-directory die is gekoppeld aan de database. De identiteit van de Azure AD kan worden een afzonderlijk gebruikersaccount of een groep. Zie voor meer informatie over ingesloten databasegebruikers [opgenomen databasegebruikers maken uw Database draagbare](https://msdn.microsoft.com/library/ff929188.aspx).

> [!NOTE]
> Database-gebruikers (met uitzondering van beheerders) kunnen niet worden gemaakt met behulp van de portal. RBAC-rollen worden niet doorgegeven aan SQL Server, SQL Database of SQL Data Warehouse. Azure RBAC-rollen worden gebruikt voor het beheren van Azure-Resources en niet van toepassing op de databasemachtigingen. Bijvoorbeeld, de **SQL Server Inzender** rol verleent geen toegang tot het verbinding maken met de SQL-Database of SQL Data Warehouse. De toegangsmachtiging moet rechtstreeks in de database met behulp van Transact-SQL-instructies worden toegekend.
>

Voor het maken van een Azure AD-gebruiker op basis van die database (met uitzondering van de beheerder van de server die eigenaar is van de database), verbinding maken met de database met de identiteit van een Azure AD, als een gebruiker met ten minste de **elke gebruiker ALTER** machtiging. Gebruik vervolgens de volgende Transact-SQL-syntaxis:

```
CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
```

*Azure_AD_principal_name* mag de UPN van een Azure AD-gebruiker of de weergavenaam voor een Azure AD-groep.

**Voorbeelden:** voor het maken van een ingesloten database gebruiker voor een Azure AD federatief of beheerd domeingebruiker:
```
CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;
```

Geef de weergavenaam van een beveiligingsgroep voor het maken van een ingesloten database-gebruiker die een Azure AD of federatieve domeingroep:
```
CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;
```

Een ingesloten database-gebruiker die een toepassing die verbinding maakt met behulp van een Azure AD-token maken:

```
CREATE USER [appName] FROM EXTERNAL PROVIDER;
```

>  [!TIP]
>  U kunt een gebruiker rechtstreeks vanuit een Azure Active Directory dan de Azure Active Directory die is gekoppeld aan uw Azure-abonnement maken. Leden van andere Active Directory's die geïmporteerde gebruikers in de bijbehorende Active Directory zijn (ook wel externe gebruikers) kunnen echter worden toegevoegd aan een Active Directory-groep in de Active Directory-tenant. Door de gebruiker van een ingesloten database voor die AD-groep maken, kunnen de gebruikers uit Active Directory externe toegang tot SQL-Database.   

Voor meer informatie over het maken van die gebruikers op basis van identiteiten met Azure Active Directory-database, raadpleegt u [gebruiker maken (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx).

> [!NOTE]
> Verwijderen van de Azure Active Directory-beheerder voor Azure SQL-server voorkomt u dat alle Azure AD authentication-gebruiker verbinding met de server. Indien nodig, onbruikbaar Azure AD-gebruikers kunnen handmatig worden verwijderd door de beheerder van een SQL-Database.   

>  [!NOTE]
>  Als u krijgt een **verbinding time-out verstreken**, moet u instellen de `TransparentNetworkIPResolution` parameter van de verbindingsreeks op false. Zie voor meer informatie [time-out verbindingsprobleem met .NET Framework 4.6.1 - wordt met TransparentNetworkIPResolution](https://blogs.msdn.microsoft.com/dataaccesstechnologies/2016/05/07/connection-timeout-issue-with-net-framework-4-6-1-transparentnetworkipresolution/).   

   
Wanneer u een databasegebruiker maakt, wordt die gebruiker ontvangt de **CONNECT** machtiging en verbinding kunnen maken met die database als lid van de **openbare** rol. In eerste instantie de beschikbaar voor de gebruiker alleen machtigingen worden alle machtigingen te krijgen tot de **openbare** rol of machtigingen toegekend aan een Azure AD-groepen dat ze lid zijn van zijn. Wanneer u een Azure AD-gebruiker op basis van opgenomen database inricht, verleent u de gebruiker extra machtigingen dezelfde manier als u toestemming aan een andere gebruiker. Doorgaans machtigingen verlenen voor databaserollen en gebruikers toevoegen aan functies. Zie voor meer informatie [Database-Engine machtiging Basics](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Zie voor meer informatie over specifieke functies van de SQL-Database, [het beheren van Databases en aanmeldingen in Azure SQL Database](sql-database-manage-logins.md).
Een federatieve domeingebruikersaccount die is geïmporteerd in een beheerd domein als een externe gebruiker moet de identiteit van de beheerde domein gebruiken.

> [!NOTE]
> Azure AD-gebruikers zijn gemarkeerd in de databasemetagegevens van de met het type E (EXTERNAL_USER) en voor groepen met type X (EXTERNAL_GROUPS). Zie voor meer informatie [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx). 
>

## <a name="connect-to-the-user-database-or-data-warehouse-by-using-ssms-or-ssdt"></a>Verbinding maken met de gebruiker database of de data warehouse met behulp van SSMS of SSDT  
Om te bevestigen op de Azure AD-beheerder correct is ingesteld, verbinding maken met de **master** met de administrator-account voor Azure AD-database.
Verbinding maken met de database met een Azure AD-identiteit die toegang tot de database heeft voor het inrichten van een Azure AD-gebruiker op basis van die database (met uitzondering van de beheerder van de server die eigenaar is van de database).

> [!IMPORTANT]
> Ondersteuning voor Azure Active Directory-verificatie is beschikbaar met [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) en [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) in Visual Studio 2015. De augustus 2016-versie van SSMS biedt ook ondersteuning voor universele verificatie van Active Directory, zodat beheerders meervoudige authenticatie met behulp van een telefoongesprek, tekstbericht, smartcards en pincode of mobiele-appmelding.
 
## <a name="using-an-azure-ad-identity-to-connect-using-ssms-or-ssdt"></a>Met behulp van een Azure AD-identiteit verbinding maken met behulp van SSMS of SSDT  

De volgende procedures laten zien hoe verbinding maken met een SQL-database met een Azure AD-identiteit met behulp van SQL Server Management Studio of SQL Server-Database Tools.

### <a name="active-directory-integrated-authentication"></a>Geïntegreerde verificatie van Active Directory

Gebruik deze methode als u bent aangemeld bij Windows met uw Azure Active Directory-referenties van een federatieve domein.

1. Start Management Studio of Data Tools en in de **verbinding maken met Server** (of **verbinding maken met Database-Engine**) het dialoogvenster de **verificatie** de optie  **Geïntegreerd met Active Directory -**. Er is geen wachtwoord nodig is of omdat de referenties van uw bestaande worden weergegeven voor de verbinding kan worden ingevoerd.   

    ![Selecteer AD-geïntegreerde verificatie][11]
2. Klik op de **opties** knop, en klik op de **verbindingseigenschappen** pagina in de **verbinding maken met database** typt u de naam van de gebruikersdatabase u verbinding wilt maken. (De **AD-domein naam of het tenant-ID**' optie wordt alleen ondersteund voor **Universal met MFA verbinding** opties, anders het wordt grijs.)  

    ![Selecteer de databasenaam][13]

## <a name="active-directory-password-authentication"></a>Wachtwoordverificatie voor Active Directory

Gebruik deze methode als het domein te koppelen aan een Azure AD principal-naam met behulp van de Azure AD worden beheerd. U kunt deze ook gebruiken voor federatieve account zonder toegang tot het domein, bijvoorbeeld bij het op afstand werkt.

Gebruik deze methode als u bent aangemeld bij Windows met referenties van een domein dat niet is gefedereerd met Azure of met behulp van Azure AD-verificatie met behulp van Azure AD op basis van de eerste of het domein van de client.

1. Start Management Studio of Data Tools en in de **verbinding maken met Server** (of **verbinding maken met Database-Engine**) het dialoogvenster de **verificatie** de optie  **Active Directory - wachtwoord**.
2. In de **gebruikersnaam** typt u de naam van uw Azure Active Directory-gebruiker in de notatie  **username@domain.com** . Dit moet een account van de Azure Active Directory of gefedereerd als u een account van een domein met de Azure Active Directory.
3. In de **wachtwoord** vak, typ het wachtwoord van de gebruiker voor de Azure Active Directory-account of federatieve domeinaccount.

    ![Selecteer verificatie van AD-wachtwoord][12]
4. Klik op de **opties** knop, en klik op de **verbindingseigenschappen** pagina in de **verbinding maken met database** typt u de naam van de gebruikersdatabase u verbinding wilt maken. (Zie de afbeelding in de vorige optie.)

## <a name="using-an-azure-ad-identity-to-connect-from-a-client-application"></a>Met behulp van een Azure AD-identiteit verbinding maken vanuit een clienttoepassing

De volgende procedures wordt uitgelegd hoe u verbinding maakt met een SQL-database met een Azure AD-identiteit van een clienttoepassing.

###  <a name="active-directory-integrated-authentication"></a>Geïntegreerde verificatie van Active Directory

Voor het gebruik van geïntegreerde Windows-verificatie, moet uw domein Active Directory worden gefedereerd met Azure Active Directory. De clienttoepassing (of een service) verbinden met de database moet worden uitgevoerd op een machine domein onder de domeinreferenties van een gebruiker.

Voor verbinding met een database met behulp van geïntegreerde verificatie en de identiteit van een Azure AD, moet het sleutelwoord verificatie in de verbindingsreeks voor de database zijn ingesteld op Active Directory Integrated. De volgende C#-voorbeeldcode maakt gebruik van ADO .NET.

```
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Het sleutelwoord van de verbindingsreeks ``Integrated Security=True`` wordt niet ondersteund voor het verbinden met Azure SQL Database. Bij het maken van een ODBC-verbinding, moet u spaties verwijderen en verificatie van 'ActiveDirectoryIntegrated' ingesteld.

### <a name="active-directory-password-authentication"></a>Wachtwoordverificatie voor Active Directory

Voor verbinding met een database met behulp van geïntegreerde verificatie en de identiteit van een Azure AD, moet het sleutelwoord verificatie worden ingesteld op Active Directory-wachtwoord. De verbindingsreeks moet gebruikers-ID/UID en het wachtwoord/PWD sleutelwoorden en waarden bevatten. De volgende C#-voorbeeldcode maakt gebruik van ADO .NET.

```
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Meer informatie over Azure AD-verificatiemethoden met behulp van de demo-codevoorbeelden die beschikbaar zijn op [Azure AD Authentication GitHub Demo](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth).

## <a name="azure-ad-token"></a>Azure AD-token
Deze verificatiemethode kan middelste laag services verbinding maken met Azure SQL Database- of Azure SQL Data Warehouse met het verkrijgen van een token van Azure Active Directory (AAD). Hiermee kunt geavanceerde scenario's zoals verificatie op basis van certificaten. U moet vier eenvoudige stappen voor het gebruik van Azure AD-tokenverificatie uitvoeren:

1. Uw toepassing registreren met Azure Active Directory en de client-id ophalen voor uw code. 
2. Maak een databasegebruiker die de toepassing vertegenwoordigt. (Voltooid eerder in stap 6).
3. Een certificaat maken op de client-computer wordt uitgevoerd de toepassing.
4. Het certificaat als een sleutel voor uw toepassing toevoegen.

Voorbeeld-verbindingsreeks:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

Zie voor meer informatie [SQL Server Security-Blog](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/). Zie voor meer informatie over het toevoegen van een certificaat [aan de slag met verificatie op basis van certificaten in Azure Active Directory](../active-directory/active-directory-certificate-based-authentication-get-started.md).

### <a name="sqlcmd"></a>sqlcmd

De volgende instructies verbinding maken met versie 13,1 van sqlcmd die beschikbaar via is de [Downloadcentrum](http://go.microsoft.com/fwlink/?LinkID=825643).

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="next-steps"></a>Volgende stappen
- Zie [SQL Database-toegang en -beheer](sql-database-control-access.md) voor een overzicht van toegang en beheer in SQL Database.
- Zie [Aanmeldingen, gebruikers en databaserollen](sql-database-manage-logins.md) voor een overzicht van aanmeldingen, gebruikers en databaserollen in SQL Database.
- Zie [Principals](https://msdn.microsoft.com/library/ms181127.aspx) voor meer informatie over database-principals.
- Zie [Databaserollen](https://msdn.microsoft.com/library/ms189121.aspx) voor meer informatie over databaserollen.
- Zie [SQL Database-firewallregels](sql-database-firewall-configure.md) voor meer informatie over de firewallregels in SQL Database.

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/active-directory-integrated.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth2.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db2.png

