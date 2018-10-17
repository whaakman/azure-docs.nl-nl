---
title: De Azure SQL Database-verbinding vanuit App Service beveiligen met een beheerde identiteit | Microsoft Docs
description: Informatie over hoe u databaseverbindingen veiliger kunt maken met behulp van een beheerde identiteit en over hoe u dit kunt toepassen op andere Azure-services.
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: syntaxc4
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 04/17/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 3125db03dc13f70524fd094736f50b563ef712a4
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44379924"
---
# <a name="tutorial-secure-azure-sql-database-connection-from-app-service-using-a-managed-identity"></a>Zelfstudie: De Azure SQL Database-verbinding vanuit App Service beveiligen met een beheerde identiteit

[App Servicex](app-service-web-overview.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie in Azure. De service bevat ook een [beheerde identiteit](app-service-managed-service-identity.md) voor uw app. Dit is een gebruiksklare oplossing voor het beveiligen van toegang tot [Azure SQL Database](/azure/sql-database/) en andere Azure-services. Beheerde identiteiten in App Service maken uw app veiliger doordat geheimen in uw app, zoals referenties in de verbindingsreeksen, worden verwijderd. In deze zelfstudie voegt u een beheerde identiteit toe aan het voorbeeld van de AS.NET-web-app dat u hebt gemaakt in [Zelfstudie: Een ASP.NET-app in Azure bouwen met behulp van SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md). Wanneer u klaar bent, maakt uw voorbeeld-app veilig verbinding met SQL Database zonder dat een gebruikersnaam en wachtwoorden zijn vereist.

U leert het volgende:

> [!div class="checklist"]
> * Beheerde identiteiten inschakelen
> * SQL Database toegang verlenen tot de beheerde identiteit
> * Toepassingscode configureren voor verificatie met SQL Database met behulp van Azure Active Directory-verificatie
> * Minimale bevoegdheden verlenen aan de beheerde identiteit in SQL Database

> [!NOTE]
> Azure Active Directory-verificatie _verschilt_ van [Geïntegreerde Windows-verificatie](/previous-versions/windows/it-pro/windows-server-2003/cc758557(v=ws.10)) in on-premises Active Directory (AD DS). AD DS en Azure Active Directory gebruiken totaal verschillende verificatieprotocollen. Zie voor meer informatie [The difference between Windows Server AD DS and Azure AD](../active-directory/fundamentals/understand-azure-identity-solutions.md#the-difference-between-windows-server-ad-ds-and-azure-ad) (Het verschil tussen Windows Server AD DS en Azure AD).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Dit artikel gaat verder waar u gebleven was in [Zelfstudie: Een ASP.NET-app in Azure bouwen met behulp van SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md). Als u dat nog niet gedaan hebt, volgt u eerst die zelfstudie. U kunt ook de stappen aanpassen voor uw eigen ASP.NET-app met SQL Database.

<!-- ![app running in App Service](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png) -->

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="enable-managed-identities"></a>Beheerde identiteiten inschakelen

Als u een beheerde identiteit voor uw Azure-app wilt inschakelen, gebruikt u de opdracht [az webapp identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) in de Cloud Shell. Vervang *\<app name>* in de volgende opdracht.

```azurecli-interactive
az webapp identity assign --resource-group myResourceGroup --name <app name>
```

Hier ziet u een voorbeeld van de uitvoer nadat de identiteit is gemaakt in Azure Active Directory:

```json
{
  "additionalProperties": {},
  "principalId": "21dfa71c-9e6f-4d17-9e90-1d28801c9735",
  "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47",
  "type": "SystemAssigned"
}
```

U gebruikt de waarde van `principalId` in de volgende stap. Als u de details van de nieuwe identiteit in Azure Active Directory wilt zien, voert u de volgende optionele opdracht uit met de waarde van `principalId`:

```azurecli-interactive
az ad sp show --id <principalid>
```

## <a name="grant-database-access-to-identity"></a>Database toegang verlenen tot identiteit

Vervolgens verleent u databasetoegang tot de beheerde identiteit van uw app, met behulp van de opdracht [`az sql server ad-admin create`](/cli/azure/sql/server/ad-admin?view=azure-cli-latest#az-sql-server-ad-admin_create) in de Cloud Shell. Vervang *\<server_name>* en <principalid_from_last_step> in de volgende opdracht. Typ de naam van een beheerder bij *\<admin_user>*.

```azurecli-interactive
az sql server ad-admin create --resource-group myResourceGroup --server-name <server_name> --display-name <admin_user> --object-id <principalid_from_last_step>
```

De beheerde identiteit heeft nu toegang tot uw Azure SQL Database-server.

## <a name="modify-connection-string"></a>De verbindingsreeks wijzigen

Wijzig de verbinding die u eerder voor uw app hebt ingesteld en doe dat met behulp van de opdracht [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) in de Cloud Shell. Vervang  *\<app name>* in de volgende opdracht door de naam van uw app en vervang *\<server_name>* en *\<db_name>*  door die van uw SQL Database.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='Server=tcp:<server_name>.database.windows.net,1433;Database=<db_name>;' --connection-string-type SQLAzure
```

## <a name="modify-aspnet-code"></a>ASP.NET-code wijzigen

In uw **DotNetAppSqlDb**-project in Visual Studio opent u _packages.config_ en voegt u de volgende regel in de lijst met pakketten toe.

```xml
<package id="Microsoft.Azure.Services.AppAuthentication" version="1.1.0-preview" targetFramework="net461" />
```

Open _Models\MyDatabaseContext.cs_ en voeg de volgende `using`-instructies toe aan het begin van het bestand:

```csharp
using System.Data.SqlClient;
using Microsoft.Azure.Services.AppAuthentication;
using System.Web.Configuration;
```

Voeg in de klasse `MyDatabaseContext` de volgende constructor toe:

```csharp
public MyDatabaseContext(SqlConnection conn) : base(conn, true)
{
    conn.ConnectionString = WebConfigurationManager.ConnectionStrings["MyDbConnection"].ConnectionString;
    // DataSource != LocalDB means app is running in Azure with the SQLDB connection string you configured
    if(conn.DataSource != "(localdb)\\MSSQLLocalDB")
        conn.AccessToken = (new AzureServiceTokenProvider()).GetAccessTokenAsync("https://database.windows.net/").Result;

    Database.SetInitializer<MyDatabaseContext>(null);
}
```

Deze constructor configureert een aangepast SqlConnection-object voor gebruik van een toegangstoken voor Azure SQL Database vanuit App Service. Met het toegangstoken verifieert uw App Service-app Azure SQL Database met de beheerde identiteit. Zie [Tokens voor Azure-resources verkrijgen](app-service-managed-service-identity.md#obtaining-tokens-for-azure-resources) voor meer informatie. De `if`-instructie laat u doorgaan met het lokaal testen van uw app met LocalDB.

> [!NOTE]
> `SqlConnection.AccessToken` wordt momenteel alleen ondersteund in .NET Framework 4.6 en hoger, niet in [.NET Core](https://www.microsoft.com/net/learn/get-started/windows).
>

Voor het gebruik van deze nieuwe constructor opent u `Controllers\TodosController.cs` en zoekt u de regel `private MyDatabaseContext db = new MyDatabaseContext();`. De bestaande code gebruikt de standaard `MyDatabaseContext`-controller om een database te maken met behulp van de standaardverbindingsreeks die een gebruikersnaam en wachtwoord in ongecodeerde tekst had voordat [u deze wijzigde](#modify-connection-string).

Vervang de gehele regel door de volgende code:

```csharp
private MyDatabaseContext db = new MyDatabaseContext(new SqlConnection());
```

### <a name="publish-your-changes"></a>Uw wijzigingen publiceren

U hoeft nu alleen nog maar uw wijzigingen naar Azure te publiceren.

Klik in de **Solution Explorer** met de rechtermuisknop op uw project **DotNetAppSqlDb** en selecteer **Publiceren**.

![Publiceren vanuit Solution Explorer](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Klik op de publicatiepagina op **Publiceren**. Wanneer de nieuwe webpagina uw takenlijst weergeeft, maakt uw app verbinding met de database met behulp van de beheerde identiteit.

![Azure-web-app na Code First Migration](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

U zou nu de takenlijst moeten kunnen bewerken als voorheen.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="grant-minimal-privileges-to-identity"></a>Minimale bevoegdheden verlenen aan identiteit

Tijdens de eerdere stappen hebt u waarschijnlijk gemerkt dat uw beheerde identiteit verbonden is met SQL Server als de Azure AD-beheerder. Voor het verlenen van minimale bevoegdheden aan uw beheerde identiteit moet u zich aanmelden bij de Azure SQL Database-server als Azure AD-beheerder en vervolgens een Azure Active Directory-groep toevoegen die de beheerde identiteit bevat. 

### <a name="add-managed-identity-to-an-azure-active-directory-group"></a>Beheerde identiteit toevoegen aan een Azure Active Directory-groep

Voeg in de Cloud Shell de beheerde identiteit voor uw app toe aan een nieuwe Azure Active Directory-groep met de naam _myAzureSQLDBAccessGroup_, zoals weergegeven in het volgende script:

```azurecli-interactive
groupid=$(az ad group create --display-name myAzureSQLDBAccessGroup --mail-nickname myAzureSQLDBAccessGroup --query objectId --output tsv)
msiobjectid=$(az webapp identity show --resource-group <group_name> --name <app_name> --query principalId --output tsv)
az ad group member add --group $groupid --member-id $msiobjectid
az ad group member list -g $groupid
```

Als u de volledige JSON-uitvoer voor elke opdracht wilt zien, haal dan de `--query objectId --output tsv`-parameters weg.

### <a name="reconfigure-azure-ad-administrator"></a>Azure AD-beheerder opnieuw configureren

Voorheen wees u de beheerde identiteit toe als de Azure AD-beheerder voor uw SQL Database. U kunt deze identiteit niet gebruiken voor interactief aanmelden (om databasegebruikers toe te voegen), dus moet u gebruikmaken van uw werkelijke Azure AD-gebruiker. Volg de stappen in [Een Azure Active Directory-beheerder voor uw Azure SQL Database Server inrichten](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) om uw Azure AD-gebruiker toe te voegen. 

### <a name="grant-permissions-to-azure-active-directory-group"></a>Machtigingen verlenen voor Azure Active Directory-groep

Meld u in de Cloud Shell aan bij SQL Database met behulp van de SQLCMD-opdracht. Vervang _\<servername>_ door uw SQL Database-servernaam en vervang _\<AADusername>_ en _\<AADpassword>_ door de referenties van uw Azure AD-gebruiker.

```azurecli-interactive
sqlcmd -S <server_name>.database.windows.net -U <AADuser_name> -P "<AADpassword>" -G -l 30
```

Voer bij de SQL-prompt de volgende opdrachten uit, waarmee de Azure Active Directory-groep wordt toegevoegd die u eerder als gebruiker hebt gemaakt.

```sql
CREATE USER [myAzureSQLDBAccessGroup] FROM EXTERNAL PROVIDER;
GO
```

Typ `EXIT` om terug te keren naar de Cloud Shell-prompt. Vervolgens voert u SQLCMD opnieuw uit, maar geeft u de naam van de database op in _\<dbname>_.

```azurecli-interactive
sqlcmd -S <server_name>.database.windows.net -d <db_name> -U <AADuser_name> -P "<AADpassword>" -G -l 30
```

Voer bij de SQL-prompt voor de gewenste database de volgende opdrachten uit om lees- en schrijfmachtigingen te verlenen aan de Azure Active Directory-groep.

```sql
ALTER ROLE db_datareader ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_datawriter ADD MEMBER [myAzureSQLDBAccessGroup];
GO
```

## <a name="next-steps"></a>Volgende stappen

Wat u hebt geleerd:

> [!div class="checklist"]
> * Beheerde identiteiten inschakelen
> * SQL Database toegang verlenen tot de beheerde identiteit
> * Toepassingscode configureren voor verificatie met SQL Database met behulp van Azure Active Directory-verificatie
> * Minimale bevoegdheden verlenen aan de beheerde identiteit in SQL Database

Ga door naar de volgende zelfstudie om te leren hoe u een aangepaste DNS-naam aan uw web-app kunt toewijzen.

> [!div class="nextstepaction"]
> [Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps](app-service-web-tutorial-custom-domain.md)
