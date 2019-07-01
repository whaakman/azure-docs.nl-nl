---
title: SQL Database-verbinding beveiligen met beheerde identiteit - Azure App Service | Microsoft Docs
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
ms.date: 06/21/2019
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 31535642526c608ad0ae29e5c0e3c93368e184ad
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67481015"
---
# <a name="tutorial-secure-azure-sql-database-connection-from-app-service-using-a-managed-identity"></a>Zelfstudie: Azure SQL Database-verbinding vanuit App Service beveiligen met een beheerde identiteit

[App Servicex](overview.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie in Azure. De service bevat ook een [beheerde identiteit](overview-managed-identity.md) voor uw app. Dit is een gebruiksklare oplossing voor het beveiligen van toegang tot [Azure SQL Database](/azure/sql-database/) en andere Azure-services. Beheerde identiteiten in App Service maken uw app veiliger doordat geheimen in uw app, zoals referenties in de verbindingsreeksen, worden verwijderd. In deze zelfstudie voegt u een beheerde identiteit toe aan het voorbeeld van de AS.NET-web-app dat u hebt gemaakt in [Zelfstudie: Een ASP.NET-app in Azure bouwen met behulp van SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md). Wanneer u klaar bent, maakt uw voorbeeld-app veilig verbinding met SQL Database zonder dat een gebruikersnaam en wachtwoorden zijn vereist.

> [!NOTE]
> In dit scenario wordt momenteel ondersteund door .NET Framework 4.7.2 en hoger. [.NET core 2.2](https://www.microsoft.com/net/download/dotnet-core/2.2) biedt weliswaar ondersteuning voor het scenario, maar is nog niet opgenomen in de standaardinstallatiekopieën in App Service. 
>

U leert het volgende:

> [!div class="checklist"]
> * Beheerde identiteiten inschakelen
> * SQL Database toegang verlenen tot de beheerde identiteit
> * Entity Framework voor het gebruik van Azure AD-verificatie met SQL Database configureren
> * Verbinding maken met SQL Database vanuit Visual Studio met behulp van Azure AD-verificatie

> [!NOTE]
>Azure AD-verificatie is _verschillende_ van [geïntegreerde Windows-verificatie](/previous-versions/windows/it-pro/windows-server-2003/cc758557(v=ws.10)) in on-premises Active Directory (AD DS). Gebruik de totaal verschillende verificatieprotocollen AD DS en Azure AD. Zie [Documentatie voor Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/) voor meer informatie.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Dit artikel gaat verder waar u bent gebleven in [Zelfstudie: Een ASP.NET-app in Azure bouwen met behulp van SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md). Als u dat nog niet gedaan hebt, volgt u eerst die zelfstudie. U kunt ook de stappen aanpassen voor uw eigen ASP.NET-app met SQL Database.

Als u wilt fouten opsporen in uw app met behulp van SQL Database als de back-end, zorg ervoor dat u hebt [clientverbinding vanaf uw computer toegestaan](app-service-web-tutorial-dotnet-sqldatabase.md#allow-client-connection-from-your-computer).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="grant-azure-ad-user-access-to-database"></a>Verleen Azure AD-gebruiker toegang tot de database

Azure AD-verificatie met SQL-Database eerst inschakelen door het toewijzen van een Azure AD-gebruiker als de Active Directory-beheerder van de SQL Database-server. Deze gebruiker wijkt af van het Microsoft-account dat u hebt gebruikt om u te registreren voor uw Azure-abonnement. Het moet een gebruiker die u hebt gemaakt, geïmporteerd, gesynchroniseerd of uitgenodigd voor Azure AD. Zie voor meer informatie over Azure AD-gebruikers toegestaan [Azure AD-functies en beperkingen in SQL-Database](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations). 

Vinden van de object-ID van de Azure AD-gebruiker via de [ `az ad user list` ](/cli/azure/ad/user?view=azure-cli-latest#az-ad-user-list) en vervang  *\<gebruiker-principal-naam >* . Het resultaat wordt opgeslagen in een variabele.

```azurecli-interactive
azureaduser=$(az ad user list --filter "userPrincipalName eq '<user-principal-name>'" --query [].objectId --output tsv)
```
> [!TIP]
> Als u wilt zien van de lijst met alle UPN-namen in Azure AD, `az ad user list --query [].userPrincipalName`.
>

Deze Azure AD-gebruiker toevoegen als een Active Directory-beheerder met [ `az sql server ad-admin create` ](/cli/azure/sql/server/ad-admin?view=azure-cli-latest#az-sql-server-ad-admin-create) opdracht in de Cloud Shell. In de volgende opdracht, vervangt u  *\<-servernaam >* .

```azurecli-interactive
az sql server ad-admin create --resource-group myResourceGroup --server-name <server-name> --display-name ADMIN --object-id $azureaduser
```

Zie voor meer informatie over het toevoegen van een Active Directory-beheerder [inrichten van een Azure Active Directory-beheerder voor uw Azure SQL Database-Server](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)

## <a name="set-up-visual-studio"></a>Visual Studio instellen

Om in te schakelen ontwikkeling en foutopsporing in Visual Studio, moet u uw Azure AD-gebruiker in Visual Studio toevoegen selecteren **bestand** > **Accountinstellingen** in het menu en klik op **toevoegen een account**.

Als u de Azure AD-gebruiker voor Azure service-verificatie, selecteert u **extra** > **opties** in het menu Selecteer **Azure Service-verificatie**  >  **Rekening selecteren**. Selecteer de Azure AD-gebruiker die u hebt toegevoegd en klik op **OK**.

U bent nu klaar om te ontwikkelen en fouten opsporen in uw app met de SQL-Database als de back-end, met behulp van Azure AD-verificatie.

## <a name="modify-aspnet-project"></a>ASP.NET-project wijzigen

Open Package Manager Console in Visual Studio en voeg het NuGet-pakket [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) toe:

```powershell
Install-Package Microsoft.Azure.Services.AppAuthentication -Version 1.2.0
```

In *Web.config*, werken vanaf de bovenkant van het bestand en de volgende wijzigingen aanbrengen:

- In `<configSections>`, de volgende sectie declaratie invoegtoepassing:

    ```xml
    <section name="SqlAuthenticationProviders" type="System.Data.SqlClient.SqlAuthenticationProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
    ```

- onder de afsluitende `</configSections>` tag, voeg de volgende XML-code voor `<SqlAuthenticationProviders>`.

    ```xml
    <SqlAuthenticationProviders>
      <providers>
        <add name="Active Directory Interactive" type="Microsoft.Azure.Services.AppAuthentication.SqlAppAuthenticationProvider, Microsoft.Azure.Services.AppAuthentication" />
      </providers>
    </SqlAuthenticationProviders>
    ```    

- De tekenreeks voor verbinding met de naam `MyDbConnection` en vervang de `connectionString` waarde met `"server=tcp:<server-name>.database.windows.net;database=<db-name>;UID=AnyString;Authentication=Active Directory Interactive"`. Vervang  _\<-servernaam >_ en  _\<db-naam >_ met de servernaam en databasenaam.

Type `Ctrl+F5` opnieuw uit te voeren de app. Dezelfde CRUD-app in uw browser is nu verbinding maken met de Azure SQL Database rechtstreeks met behulp van Azure AD-verificatie. Deze instelling kunt u databasemigraties uitvoeren. Als u uw wijzigingen implementeren in App Service, de dezelfde instellingen in combinatie met de beheerde identiteit van de app.

## <a name="use-managed-identity-connectivity"></a>Beheerde identiteit verbinding gebruiken

Vervolgens configureert u uw App Service-app verbinding maakt met SQL Database met een door het systeem toegewezen beheerde identiteit.

### <a name="enable-managed-identity-on-app"></a>Beheerde identiteit op de app inschakelen

Als u een beheerde identiteit voor uw Azure-app wilt inschakelen, gebruikt u de opdracht [az webapp identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) in de Cloud Shell. In de volgende opdracht, vervangt u  *\<app-naam >* .

```azurecli-interactive
az webapp identity assign --resource-group myResourceGroup --name <app-name>
```

Hier volgt een voorbeeld van de uitvoer:

```json
{
  "additionalProperties": {},
  "principalId": "21dfa71c-9e6f-4d17-9e90-1d28801c9735",
  "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47",
  "type": "SystemAssigned"
}
```

### <a name="add-managed-identity-to-an-azure-ad-group"></a>Beheerde identiteit toevoegen aan een Azure AD-groep

Deze identiteit om toegang te verlenen tot uw SQL-Database, moet u deze toevoegen aan een [Azure AD-groep](../active-directory/fundamentals/active-directory-manage-groups.md). In de Cloud Shell, toevoegen aan een nieuwe groep met de naam _myAzureSQLDBAccessGroup_, zoals weergegeven in het volgende script:

```azurecli-interactive
groupid=$(az ad group create --display-name myAzureSQLDBAccessGroup --mail-nickname myAzureSQLDBAccessGroup --query objectId --output tsv)
msiobjectid=$(az webapp identity show --resource-group myResourceGroup --name <app-name> --query principalId --output tsv)
az ad group member add --group $groupid --member-id $msiobjectid
az ad group member list -g $groupid
```

Als u de volledige JSON-uitvoer voor elke opdracht wilt zien, haal dan de `--query objectId --output tsv`-parameters weg.

### <a name="grant-permissions-to-azure-ad-group"></a>Machtigingen verlenen voor Azure AD-groep

Meld u in de Cloud Shell aan bij SQL Database met behulp van de SQLCMD-opdracht. Vervang  _\<-servernaam >_ met de naam van uw SQL-Database-server  _\<db-naam >_ met de database maakt gebruik van uw app, een naam en  _\< AAD-user-name >_ en  _\<aad-password >_ met de referenties van uw Azure AD-gebruiker.

```azurecli-interactive
sqlcmd -S <server-name>.database.windows.net -d <db-name> -U <aad-user-name> -P "<aad-password>" -G -l 30
```

In de SQL-prompt voor de database die u wilt, voer de volgende opdrachten om toe te voegen van de Azure AD groeperen en verleen de machtigingen van uw app nodig. Bijvoorbeeld: 

```sql
CREATE USER [myAzureSQLDBAccessGroup] FROM EXTERNAL PROVIDER;
ALTER ROLE db_datareader ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_datawriter ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_ddladmin ADD MEMBER [myAzureSQLDBAccessGroup];
GO
```

Typ `EXIT` om terug te keren naar de Cloud Shell-prompt.

### <a name="modify-connection-string"></a>De verbindingsreeks wijzigen

Houd er rekening mee dat dezelfde aangebracht wijzigingen in `Web.config` werkt met de beheerde identiteit, zodat het enige wat te doen is om te verwijderen van de bestaande verbindingsreeks in uw app, die Visual Studio heeft gemaakt implementeren van uw app de eerste keer. Gebruik de volgende opdracht uit, maar vervang  *\<app-naam >* met de naam van uw app.

```azurecli-interactive
az webapp config connection-string delete --resource-group myResourceGroup --name <app-name> --setting-names MyDbConnection
```

## <a name="publish-your-changes"></a>Uw wijzigingen publiceren

U hoeft nu alleen nog maar uw wijzigingen naar Azure te publiceren.

Klik in de **Solution Explorer** met de rechtermuisknop op uw project **DotNetAppSqlDb** en selecteer **Publiceren**.

![Publiceren vanuit Solution Explorer](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Klik op de publicatiepagina op **Publiceren**. Wanneer de nieuwe webpagina uw takenlijst weergeeft, maakt uw app verbinding met de database met behulp van de beheerde identiteit.

![Azure-app na Code First Migration](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

U zou nu de takenlijst moeten kunnen bewerken als voorheen.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Volgende stappen

Wat u hebt geleerd:

> [!div class="checklist"]
> * Beheerde identiteiten inschakelen
> * SQL Database toegang verlenen tot de beheerde identiteit
> * Entity Framework voor het gebruik van Azure AD-verificatie met SQL Database configureren
> * Verbinding maken met SQL Database vanuit Visual Studio met behulp van Azure AD-verificatie

Ga door naar de volgende zelfstudie om te leren hoe u een aangepaste DNS-naam aan uw web-app kunt toewijzen.

> [!div class="nextstepaction"]
> [Een bestaande aangepaste DNS-naam toewijzen aan Azure App Service](app-service-web-tutorial-custom-domain.md)
