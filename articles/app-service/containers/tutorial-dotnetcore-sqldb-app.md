---
title: Een .NET Core en SQL Database web-app in Azure App Service maken op Linux | Microsoft Docs
description: Informatie over het ophalen van een .NET Core-app in Azure App Service op Linux, werken met verbinding met een SQL-Database.
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: syntaxc4
editor: 
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 10/10/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: ef68f64437935f08f76c29ecf15d574279cca7f1
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="build-a-net-core-and-sql-database-web-app-in-azure-app-service-on-linux"></a>Een .NET Core en SQL Database web-app in Azure App Service maken op Linux

[App-Service op Linux](app-service-linux-intro.md) biedt een zeer schaalbaar, zelf patch webhosting-service met het Linux-besturingssysteem. Deze zelfstudie laat zien hoe een web-app voor .NET Core maken en te verbinden met een SQL-Database. Wanneer u bent klaar, hebt u een .NET Core MVC-app in App Service wordt uitgevoerd op Linux.

![App in App Service wordt uitgevoerd op Linux](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

Wat u leert hoe:

> [!div class="checklist"]
> * Maken van een SQL-Database in Azure
> * Verbinding maken met een .NET Core app met SQL-Database
> * De app implementeren in Azure
> * Bijwerken van het gegevensmodel en de app implementeren
> * Diagnostische logboeken van de stroom van Azure
> * De app in de Azure portal beheren

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

1. [Git installeren](https://git-scm.com/)
1. [.NET Core SDK 1.1.2 installeren](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.1.2-download.md)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-local-net-core-app"></a>Lokale .NET Core-app maken

In deze stap moet u het lokale .NET Core project instellen.

### <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

In het terminalvenster `cd` in een werkmap.

Voer de volgende opdrachten de voorbeeld-opslagplaats klonen en wijzig in de hoofdmap.

```bash
git clone https://github.com/azure-samples/dotnetcore-sqldb-tutorial
cd dotnetcore-sqldb-tutorial
```

Het voorbeeldproject bevat een eenvoudige CRUD (maken-lezen-update-verwijderen) app met [Entity Framework Core](https://docs.microsoft.com/ef/core/).

### <a name="run-the-application"></a>De toepassing uitvoeren

Voer de volgende opdrachten voor het installeren van de vereiste pakketten, database migraties worden uitgevoerd en de toepassing niet starten.

```bash
dotnet restore
dotnet ef database update
dotnet run
```

Ga naar `http://localhost:5000` in een browser. Selecteer de **nieuw** koppelen en maken van een paar _taak_ items.

![wel verbinding kunt maken met SQL-Database](./media/tutorial-dotnetcore-sqldb-app/local-app-in-browser.png)

Als u wilt stoppen .NET Core op elk gewenst moment, drukt u op `Ctrl+C` in de terminal.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-production-sql-database"></a>SQL-Database maken

In deze stap maakt u een SQL-Database in Azure. Wanneer uw app wordt geïmplementeerd naar Azure, wordt deze cloud-database gebruikt.

Voor SQL-Database in deze zelfstudie wordt [Azure SQL Database](/azure/sql-database/).

### <a name="create-a-resource-group"></a>Een resourcegroep maken

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)]

### <a name="create-a-sql-database-logical-server"></a>Maak een logische SQL Database-server

In de Cloud-Shell, maakt u een logische SQL Database-server met de [az sql server maken](/cli/azure/sql/server#create) opdracht.

Vervang de  *\<servernaam >* aanduiding voor items met een unieke naam voor de SQL-Database. Deze naam wordt gebruikt als het onderdeel van het eindpunt van de SQL-Database, `<server_name>.database.windows.net`, zodat de naam moet uniek zijn in alle logische servers in Azure. De naam mag alleen kleine letters, cijfers en het koppelteken (-) en moet tussen 3 en 50 tekens bevatten. Vervang ook  *\<db_username >* en  *\<db_password >* met een gebruikersnaam en wachtwoord van uw keuze. 


```azurecli-interactive
az sql server create --name <server_name> --resource-group myResourceGroup --location "West Europe" --admin-user <db_username> --admin-password <db_password>
```

Wanneer de logische SQL Database-server is gemaakt, toont de Azure CLI informatie vergelijkbaar met het volgende voorbeeld:

```json
{
  "administratorLogin": "sqladmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/<server_name>",
  "identity": null,
  "kind": "v12.0",
  "location": "westeurope",
  "name": "<server_name>",
  "resourceGroup": "myResourceGroup",
  "state": "Ready",
  "tags": null,
  "type": "Microsoft.Sql/servers",
  "version": "12.0"
}
```

### <a name="configure-a-server-firewall-rule"></a>Een serverfirewallregel configureren

Maak een [Azure SQL Database-firewallregel op serverniveau](../../sql-database/sql-database-firewall-configure.md) met de opdracht [az sql server firewall create](/cli/azure/sql/server#create). Als zowel de IP-beginadres en de laatste IP-zijn ingesteld op 0.0.0.0, wordt de firewall alleen geopend voor andere Azure-resources. 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server <server_name> --name AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

### <a name="create-a-database"></a>Een database maken

Maak een database met een [prestatieniveau van S0](../../sql-database/sql-database-service-tiers.md) op de server met de opdracht [az sql db create](/cli/azure/sql/db#create).

```azurecli-interactive
az sql db create --resource-group myResourceGroup --server <server_name> --name coreDB --service-objective S0
```

### <a name="create-connection-string"></a>Verbindingstekenreeks maken

Vervang de volgende tekenreeks met de  *\<servernaam >*,  *\<db_username >*, en  *\<db_password >* u eerder is gebruikt.

```
Server=tcp:<server_name>.database.windows.net,1433;Initial Catalog=coreDB;Persist Security Info=False;User ID=<db_username>;Password=<db_password>;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

Dit is de verbindingsreeks voor uw app .NET Core. Kopieer de verbindingsreeks voor gebruik later.

## <a name="deploy-app-to-azure"></a>App implementeren in Azure

In deze stap maakt implementeren u uw toepassing verbinding maken met SQL Database .NET Core op Linux-App Service.

### <a name="configure-local-git-deployment"></a>Lokale Git-implementatie configureren

[!INCLUDE [Configure a deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Een App Service-plan maken

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Een webtoepassing maken

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-dotnetcore-no-h.md)] 

### <a name="configure-an-environment-variable"></a>Configureren van een omgevingsvariabele

U stelt tekenreeksen voor databaseverbindingen voor uw app in Azure met de [az webapp config appsettings bijwerken](/cli/azure/webapp/config/appsettings#update) opdracht in de Cloud-Shell. Vervang in de volgende opdracht  *\<app-naam >*, evenals de  *\<koppeling-reeks >* parameter met de verbindingsreeks die u eerder hebt gemaakt.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='<connection_string>' --connection-string-type SQLServer
```

Vervolgens stelt `ASPNETCORE_ENVIRONMENT` app-instelling op _productie_. Deze instelling laat u weten of er worden uitgevoerd in Azure, omdat u SQLLite voor uw lokale ontwikkelingsomgeving en SQL-Database voor uw Azure-omgeving gebruiken.

Het volgende voorbeeld wordt een `ASPNETCORE_ENVIRONMENT` app-instelling in uw Azure-web-app. Vervang de  *\<app_naam >* tijdelijke aanduiding.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings ASPNETCORE_ENVIRONMENT="Production"
```

### <a name="connect-to-sql-database-in-production"></a>Verbinding maken met SQL Database in productie

In de lokale opslagplaats Startup.cs open en zoek de volgende code:

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
```

Vervang deze door de volgende code, die gebruikmaakt van de omgevingsvariabelen die u eerder hebt geconfigureerd.

```csharp
// Use SQL Database if in Azure, otherwise, use SQLLite
if(Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") == "Production")
    services.AddDbContext<DotNetCoreSqlDbContext>(options =>
            options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
else
    services.AddDbContext<DotNetCoreSqlDbContext>(options =>
            options.UseSqlite("Data Source=MvcMovie.db"));

// Automatically perform database migration
services.BuildServiceProvider().GetService<DotNetCoreSqlDbContext>().Database.Migrate();
```

Als deze code wordt gedetecteerd dat deze wordt uitgevoerd in productie (dit geeft aan dat de Azure-omgeving) en de verbindingsreeks wordt gebruikt u geconfigureerd voor verbinding met de SQL-Database.

De `Database.Migrate()` aanroep helpt u wanneer deze wordt uitgevoerd in Azure, omdat deze automatisch de databases die maakt de app behoeften van uw .NET Core, op basis van de migratieconfiguratie van de. 

Sla uw wijzigingen op.

### <a name="push-to-azure-from-git"></a>Pushen naar Azure vanaf Git

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash
Counting objects: 98, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (92/92), done.
Writing objects: 100% (98/98), 524.98 KiB | 5.58 MiB/s, done.
Total 98 (delta 8), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: .
remote: Updating submodules.
remote: Preparing deployment for commit id '0c497633b8'.
remote: Generating deployment script.
remote: Project file path: ./DotNetCoreSqlDb.csproj
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling ASP.NET Core Web Application deployment.
remote: .
remote: .
remote: .
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

### <a name="browse-to-the-azure-web-app"></a>Blader naar de Azure-web-app

Blader naar de geïmplementeerde web-app met behulp van uw webbrowser.

```bash
http://<app_name>.azurewebsites.net
```

Enkele taakitems toevoegen.

![App in App Service wordt uitgevoerd op Linux](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

**Gefeliciteerd!** U bent een gegevensgestuurde .NET Core-app in App Service uitgevoerd op Linux.

## <a name="update-locally-and-redeploy"></a>Lokaal bijwerken en implementeren

In deze stap maakt u een wijziging aanbrengt in het schema van uw database en deze publiceren naar Azure.

### <a name="update-your-data-model"></a>Uw gegevensmodel bijwerken

Open _Models\Todo.cs_ code-editor. De volgende eigenschap toevoegen aan de `ToDo` klasse:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Code First-migraties lokaal uitvoeren

Voer enkele opdrachten om updates voor uw lokale database.

```bash
dotnet ef migrations add AddProperty
```

De lokale database-update:

```bash
dotnet ef database update
```

### <a name="use-the-new-property"></a>De nieuwe eigenschap te gebruiken

Enkele wijzigingen aanbrengen in uw code te gebruiken de `Done` eigenschap. Voor het gemak in deze zelfstudie alleen gaat u wijzigt de `Index` en `Create` weergaven om te zien van de eigenschap in te grijpen.

Open _Controllers\TodosController.cs_.

Zoek de `Create()` methode en voeg `Done` aan de lijst met eigenschappen in de `Bind` kenmerk. Wanneer u bent klaar, uw `Create()` methodehandtekening lijkt op de volgende code:

```csharp
public async Task<IActionResult> Create([Bind("ID,Description,CreatedDate,Done")] Todo todo)
```

Open _Views\Todos\Create.cshtml_.

In de code Razor ziet u een `<div class="form-group">` element voor `Description`, en vervolgens een andere `<div class="form-group">` element voor `CreatedDate`. Direct na deze twee elementen toevoegen `<div class="form-group">` element voor `Done`:

```csharp
<div class="form-group">
    <label asp-for="Done" class="col-md-2 control-label"></label>
    <div class="col-md-10">
        <input asp-for="Done" class="form-control" />
        <span asp-validation-for="Done" class="text-danger"></span>
    </div>
</div>
```

Open _Views\Todos\Index.cshtml_.

Zoeken naar de lege `<th></th>` element. Voeg de volgende Razor-code net boven dit element:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Zoek de `<td>` element met de `asp-action` tag helpers. Voeg de volgende Razor-code net boven dit element:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.CreatedDate)
</td>
```

Dit is alles wat u wilt zien van de wijzigingen in de `Index` en `Create` weergaven.

### <a name="test-your-changes-locally"></a>Uw wijzigingen lokaal testen

De app lokaal uitvoeren.

```bash
dotnet run
```

Navigeer in uw browser naar `http://localhost:5000/`. U kunt nu een taakitem toegevoegd en controleer of **gedaan**. Vervolgens moet het weergegeven in uw startpagina als een item voltooid. Houd er rekening mee dat de `Edit` weergave niet de `Done` veld, omdat u niet wijzigen de `Edit` weergeven.

### <a name="publish-changes-to-azure"></a>Wijzigingen publiceren naar Azure

```bash

git commit -am "added done field"
git push azure master
```

Eenmaal de `git push` is voltooid, gaat u naar uw Azure-web-app en de nieuwe functionaliteit uitproberen.

![Azure-web-app na de eerste Code-migratie](./media/tutorial-dotnetcore-sqldb-app/this-one-is-done.png)

Uw bestaande taakitems nog steeds worden weergegeven. Wanneer u uw app .NET Core opnieuw publiceert, is er geen bestaande gegevens in de SQL-Database verloren. Bovendien Entity Framework Core migraties alleen het schema van de wijzigingen en laat uw bestaande gegevens intact.

## <a name="manage-your-azure-web-app"></a>Uw Azure-web-app beheren

Ga naar de [Azure-portal](https://portal.azure.com) om te zien van de web-app die u hebt gemaakt.

Klik vanuit het linkermenu op **App Services** en klik op de naam van uw Azure-web-app.

![Navigatie in de portal naar de Azure-web-app](./media/tutorial-dotnetcore-sqldb-app/access-portal.png)

Standaard ziet u de portal voor uw web-app **overzicht** pagina. Deze pagina geeft u een overzicht van hoe uw app presteert. Hier kunt u ook algemene beheertaken uitvoeren, zoals bladeren, stoppen, starten, opnieuw opstarten en verwijderen. De tabbladen aan de linkerkant van de pagina's worden weergegeven de andere configuratie dat kunt u openen.

![App Service-pagina in Azure Portal](./media/tutorial-dotnetcore-sqldb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Volgende stappen

Wat u hebt geleerd:

> [!div class="checklist"]
> * Maken van een SQL-Database in Azure
> * Verbinding maken met een .NET Core app met SQL-Database
> * De app implementeren in Azure
> * Bijwerken van het gegevensmodel en de app implementeren
> * Logboeken van de stroom van Azure naar uw terminal
> * De app in de Azure portal beheren

Ga naar de volgende zelfstudie voor informatie over het toewijzen van een aangepaste DNS-naam aan uw web-app.

> [!div class="nextstepaction"]
> [Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps](../app-service-web-tutorial-custom-domain.md)