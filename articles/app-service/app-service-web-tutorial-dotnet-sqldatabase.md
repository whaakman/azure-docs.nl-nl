---
title: Een ASP.NET-app in Azure met SQL-Database bouwen | Microsoft Docs
description: Informatie over het ophalen van een ASP.NET-app in Azure, werkt met verbinding met een SQL-Database.
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 03c584f1-a93c-4e3d-ac1b-c82b50c75d3e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 06/09/2017
ms.author: cephalin
ms.custom: mvc, devcenter
ms.openlocfilehash: db3be8068ef9e560614daa0e7f0dcf62467fd338
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="build-an-aspnet-app-in-azure-with-sql-database"></a>Een ASP.NET-app in Azure met SQL-Database maken

[Azure Web Apps](app-service-web-overview.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie. Deze zelfstudie leert u hoe u een gegevensgestuurd ASP.NET web-app implementeren in Azure en verbindt deze met [Azure SQL Database](../sql-database/sql-database-technical-overview.md). Wanneer u klaar bent, kunt u een ASP.NET-app uitgevoerd in Azure hebt en verbonden met SQL-Database.

![Gepubliceerde ASP.NET-toepassing in Azure-web-app](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maken van een SQL-Database in Azure
> * Een ASP.NET-app verbinden met SQL Database
> * De app implementeren in Azure
> * Bijwerken van het gegevensmodel en de app implementeren
> * Logboeken van de stroom van Azure naar uw terminal
> * De app in de Azure portal beheren

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

* Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de volgende workloads:
  - **ASP.NET- en web-ontwikkeling**
  - **Azure-ontwikkeling**

  ![ASP.NET- en web-ontwikkeling en Azure-ontwikkeling (onder Web en cloud)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Het voorbeeld downloaden

[Download het voorbeeldproject](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).

Pak (uitpakken van) de *dotnet-sqldb-zelfstudie-master.zip* bestand.

Het voorbeeldproject bevat een eenvoudige [ASP.NET MVC](https://www.asp.net/mvc) CRUD (maken-lezen-update-verwijderen) met behulp van app [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

### <a name="run-the-app"></a>De app uitvoeren

Open de *dotnet-sqldb-zelfstudie-master/DotNetAppSqlDb.sln* bestand in Visual Studio. 

Type `Ctrl+F5` voor het uitvoeren van de app zonder foutopsporing. De app wordt weergegeven in de browser. Selecteer de **nieuw** koppelen en maken van een paar *taak* items. 

![Het dialoogvenster Nieuw ASP.NET-project](media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

Test de **bewerken**, **Details**, en **verwijderen** koppelingen.

De app gebruikmaakt van een databasecontext om te verbinden met de database. In dit voorbeeld gebruikt de databasecontext een verbindingsreeks met de naam `MyDbConnection`. De verbindingsreeks is ingesteld in de *Web.config* bestands- en waarnaar wordt verwezen in de *Models/MyDatabaseContext.cs* bestand. De naam van de tekenreeks wordt verderop in de zelfstudie gebruikt voor de Azure-web-app verbinden met een Azure SQL Database. 

## <a name="publish-to-azure-with-sql-database"></a>Publiceren naar Azure met SQL Database

In de **Solution Explorer**, met de rechtermuisknop op uw **DotNetAppSqlDb** project en selecteer **publiceren**.

![Publiceren vanuit Solution Explorer](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Zorg ervoor dat **Microsoft Azure App Service** is geselecteerd en klik op **Publiceren**.

![Publiceren vanaf de projectoverzichtspagina](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

Publiceren wordt geopend de **Create App Service** dialoogvenster dat helpt u bij het maken van alle Azure moet u uw ASP.NET-web-app uitvoeren in Azure-resources.

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Klik in het dialoogvenster **App Service maken** op **Een account toevoegen** en meld u vervolgens aan bij uw Azure-abonnement. Als u al bent aangemeld bij een Microsoft-account, moet u ervoor zorgen dat dit account uw Azure-abonnement bevat. Als het aangemelde Microsoft-account niet uw Azure-abonnement bevat, klikt u erop om het juiste account toe te voegen.
   
![Aanmelden bij Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

Als u bent aangemeld, kunt u in dit dialoogvenster alle resources gaan maken die u nodig hebt voor uw Azure-web-app.

### <a name="configure-the-web-app-name"></a>De naam van de web-app configureren

De naam van de gegenereerde web-app te houden of wijzig dit in een andere unieke naam (geldige tekens zijn `a-z`, `0-9`, en `-`). De naam van de web-app wordt gebruikt als onderdeel van de standaard-URL voor uw app (`<app_name>.azurewebsites.net`, waarbij `<app_name>` is de naam van uw web-app). De naam van de web-app moet uniek zijn in alle apps in Azure. 

![Het dialoogvenster app service maken](media/app-service-web-tutorial-dotnet-sqldatabase/wan.png)

> [!NOTE]
> Klik niet op **maken**. U moet eerst een SQL-Database in een later stadium instellen.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

[!INCLUDE [resource-group](../../includes/resource-group.md)]

Klik naast **Resourcegroep** op **Nieuw**.

![Naast de resourcegroep, klik op Nieuw.](media/app-service-web-tutorial-dotnet-sqldatabase/new_rg2.png)

De resourcegroep een naam **myResourceGroup**.

### <a name="create-an-app-service-plan"></a>Een App Service-plan maken

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Klik naast **App Service-plan** op **Nieuw**. 

Configureer in het dialoogvenster **App Service-plan configureren** het nieuwe App Service-plan met de volgende instellingen:

![Een App Service-plan maken](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

| Instelling  | Voorgestelde waarde | Voor meer informatie |
| ----------------- | ------------ | ----|
|**App Service-abonnement**| myAppServicePlan | [App Service-abonnementen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |
|**Locatie**| West-Europa | [Azure-regio's](https://azure.microsoft.com/regions/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) |
|**Grootte**| Gratis | [Prijscategorieën](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)|

### <a name="create-a-sql-server-instance"></a>Maak een SQL Server-exemplaar

Voordat u een database maakt, moet u een [logische Azure SQL Database-server](../sql-database/sql-database-features.md). Een logische server bevat een groep met databases die worden beheerd als groep.

Selecteer **verkennen extra Azure-services**.

![Naam van web-app configureren](media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

In de **Services** en klik op de  **+**  pictogram naast **SQL-Database**. 

![Klik op het tabblad Services op het pictogram naast SQL Database +.](media/app-service-web-tutorial-dotnet-sqldatabase/sql.png)

In de **SQL-Database configureren** dialoogvenster, klikt u op **nieuw** naast **SQL Server**. 

Er wordt een unieke naam gegenereerd. Deze naam wordt gebruikt als onderdeel van de standaard-URL voor de logische server `<server_name>.database.windows.net`. Het moet uniek zijn in alle exemplaren van logische server in Azure. U kunt de servernaam van de wijzigen, maar de gegenereerde waarde voor deze zelfstudie te behouden.

Toevoegen van een beheerder gebruikersnaam en wachtwoord. Zie voor de vereisten voor wachtwoordcomplexiteit, [wachtwoordbeleid](/sql/relational-databases/security/password-policy).

Deze gebruikersnaam en wachtwoord onthouden. U moet ze voor het beheren van de logische server-exemplaar later.

![SQL Server-exemplaar maken](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

Klik op **OK**. Sluit niet de **SQL-Database configureren** nog dialoogvenster.

### <a name="create-a-sql-database"></a>Een SQL-database maken

In de **SQL-Database configureren** dialoogvenster: 

* Gebruik de standaardwaarde gegenereerd **databasenaam**.
* In **Verbindingsreeksnaam**, type *MyDbConnection*. Deze naam moet overeenkomen met de verbindingsreeks waarnaar wordt verwezen in *Models/MyDatabaseContext.cs*.
* Selecteer **OK**.

![SQL-Database configureren](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

De **Create App Service** dialoogvenster resources weergegeven die u hebt gemaakt. Klik op **Create**. 

![de resources die u hebt gemaakt](media/app-service-web-tutorial-dotnet-sqldatabase/app_svc_plan_done.png)

Zodra de wizard is voltooid voor het maken van de Azure-resources, wordt uw ASP.NET-app gepubliceerd naar Azure. De standaardbrowser wordt gestart met de URL van de geïmplementeerde app. 

Enkele taakitems toevoegen.

![Gepubliceerde ASP.NET-toepassing in Azure-web-app](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

Gefeliciteerd. Uw ASP.NET-toepassing voor gegevensgestuurde wordt live in Azure App Service uitgevoerd.

## <a name="access-the-sql-database-locally"></a>Toegang tot de SQL-Database lokaal

Visual Studio kunt u bekijken en beheren van uw nieuwe SQL-Database eenvoudig in de **SQL Server Object Explorer**.

### <a name="create-a-database-connection"></a>Een databaseverbinding maken

Van de **weergave** selecteert u **SQL Server Object Explorer**.

Aan de bovenkant van **SQL Server Object Explorer**, klikt u op de **SQL-Server toevoegen** knop.

### <a name="configure-the-database-connection"></a>Verbinding met de database configureren

In de **Connect** dialoogvenster, vouw de **Azure** knooppunt. Uw SQL-Database-exemplaren in Azure worden hier weergegeven.

Selecteer de SQL-Database die u eerder hebt gemaakt. De verbinding die u eerder hebt gemaakt, wordt automatisch gevuld onderaan.

Typ het wachtwoord van de databasebeheerder u eerder hebt gemaakt en klik op **Connect**.

![Databaseverbinding vanuit Visual Studio configureren](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### <a name="allow-client-connection-from-your-computer"></a>Clientverbinding van uw computer toestaan

De **maken van een nieuwe firewallregel** dialoogvenster wordt geopend. Uw exemplaar van SQL-Database kunt standaard alleen verbindingen van Azure-services, zoals uw Azure-web-app. Voor verbinding met uw database, een firewallregel in het exemplaar van SQL-Database te maken. De firewallregel kan het openbare IP-adres van uw lokale computer.

Het dialoogvenster is al ingevuld met het openbare IP-adres van uw computer.

Zorg ervoor dat **mijn client-IP toevoegen** is geselecteerd en klik op **OK**. 

![Firewall voor SQL Database-instantie instellen](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

Zodra de Visual Studio is gemaakt van de firewallinstelling voor uw exemplaar van SQL-Database, de verbinding wordt weergegeven in **SQL Server Object Explorer**.

Hier kunt u de meest voorkomende uitvoeren databasebewerkingen, zoals het uitvoeren van query's maken, weergaven en opgeslagen procedures en meer. 

Vouw uw verbinding > **Databases** > **&lt;uw database >** > **tabellen**. Met de rechtermuisknop op de `Todoes` tabel en selecteer **weergavegegevens**. 

![Objecten van de SQL-Database verkennen](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## <a name="update-app-with-code-first-migrations"></a>App kunt bijwerken met Code First-migraties

U kunt de vertrouwde hulpprogramma's in Visual Studio gebruiken om bij te werken van uw database en web-app in Azure. In deze stap kunt u Code First-migraties in Entity Framework een wijziging aanbrengen in uw databaseschema en deze publiceren naar Azure.

Zie voor meer informatie over het gebruik van Entity Framework Code First-migraties [aan de slag met Entity Framework 6 Code First met MVC 5](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

### <a name="update-your-data-model"></a>Uw gegevensmodel bijwerken

Open _Models\Todo.cs_ code-editor. De volgende eigenschap toevoegen aan de `ToDo` klasse:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Code First-migraties lokaal uitvoeren

Voer enkele opdrachten om updates voor uw lokale database. 

Van de **extra** menu, klikt u op **NuGet Package Manager** > **Package Manager Console**.

Schakel in het venster Package Manager Console Code First-migraties:

```PowerShell
Enable-Migrations
```

Toevoegen van een migratie:

```PowerShell
Add-Migration AddProperty
```

De lokale database-update:

```PowerShell
Update-Database
```

Type `Ctrl+F5` de app uitvoert. Test de bewerken, details en koppelingen maken.

Als de toepassing wordt geladen zonder fouten, is de Code First-migraties van voltooid zijn. Echter uw pagina nog steeds ziet er hetzelfde omdat uw toepassingslogica wordt deze nieuwe eigenschap niet nog gebruikt. 

### <a name="use-the-new-property"></a>De nieuwe eigenschap te gebruiken

Enkele wijzigingen aanbrengen in uw code te gebruiken de `Done` eigenschap. Voor het gemak in deze zelfstudie alleen gaat u wijzigt de `Index` en `Create` weergaven om te zien van de eigenschap in te grijpen.

Open _Controllers\TodosController.cs_.

Zoek de `Create()` methode op regel 52 en voeg `Done` aan de lijst met eigenschappen in de `Bind` kenmerk. Wanneer u bent klaar, uw `Create()` methodehandtekening lijkt op de volgende code:

```csharp
public ActionResult Create([Bind(Include = "Description,CreatedDate,Done")] Todo todo)
```

Open _Views\Todos\Create.cshtml_.

In de code Razor ziet u een `<div class="form-group">` element die gebruikmaakt van `model.Description`, en vervolgens een andere `<div class="form-group">` element die gebruikmaakt van `model.CreatedDate`. Direct na deze twee elementen toevoegen `<div class="form-group">` element die gebruikmaakt van `model.Done`:

```csharp
<div class="form-group">
    @Html.LabelFor(model => model.Done, htmlAttributes: new { @class = "control-label col-md-2" })
    <div class="col-md-10">
        <div class="checkbox">
            @Html.EditorFor(model => model.Done)
            @Html.ValidationMessageFor(model => model.Done, "", new { @class = "text-danger" })
        </div>
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

Zoek de `<td>` element met de `Html.ActionLink()` Help-methoden. _Hierboven_ dit `<td>`, Voeg een andere `<td>` element met de volgende Razor code:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

Dit is alles wat u wilt zien van de wijzigingen in de `Index` en `Create` weergaven. 

Type `Ctrl+F5` de app uitvoert.

U kunt nu een taakitem toegevoegd en controleer of **gedaan**. Vervolgens moet het weergegeven in uw startpagina als een item voltooid. Houd er rekening mee dat de `Edit` weergave niet de `Done` veld, omdat u niet wijzigen de `Edit` weergeven.

### <a name="enable-code-first-migrations-in-azure"></a>Code First-migraties in Azure inschakelen

Nu dat uw code wijzigen werkt, met inbegrip van de database wilt migreren, moet u deze publiceren naar uw Azure-web-app en uw SQL-Database te werken met Code First-migraties.

Net zoals, met de rechtermuisknop op uw project en selecteer **publiceren**.

Klik op **instellingen** om de wizard publish te openen.

![Open publicatie-instellingen](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-settings.png)

Klik in de wizard op **volgende**.

Zorg ervoor dat de verbindingsreeks voor de SQL-Database is gevuld **MyDatabaseContext (MyDbConnection)**. Mogelijk moet u selecteert de **myToDoAppDb** database uit de vervolgkeuzelijst. 

Selecteer **uitvoeren Code First-migraties (wordt uitgevoerd op de start van toepassing)**, klikt u vervolgens op **opslaan**.

![Code First-migraties inschakelen in Azure-web-app](./media/app-service-web-tutorial-dotnet-sqldatabase/enable-migrations.png)

### <a name="publish-your-changes"></a>Uw wijzigingen publiceren

Nu dat u Code First-migraties in uw Azure-web-app hebt ingeschakeld, moet u uw codewijzigingen publiceren.

Klik op de publicatiepagina op **Publiceren**.

Selecteer en probeer het opnieuw toe te voegen taakitems **gedaan**, en ze moeten worden weergegeven in uw startpagina als een item voltooid.

![Azure-web-app na de eerste Code-migratie](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Uw bestaande taakitems nog steeds worden weergegeven. Wanneer u uw ASP.NET-toepassing opnieuw publiceren, is er geen bestaande gegevens in de SQL-Database verloren. Bovendien Code First-migraties alleen het schema van de wijzigingen en laat uw bestaande gegevens intact.


## <a name="stream-application-logs"></a>Toepassingslogboeken Stream

U kunt tracering berichten streamen rechtstreeks vanuit uw Azure-web-app met Visual Studio.

Open _Controllers\TodosController.cs_.

Elke actie begint met een `Trace.WriteLine()` methode. Deze code wordt toegevoegd aan hoe u traceringsberichten toevoegen aan uw Azure-web-app.

### <a name="open-server-explorer"></a>Open Server Explorer

Van de **weergave** selecteert u **Server Explorer**. U kunt logboekregistratie configureren voor uw Azure-web-app in **Server Explorer**. 

### <a name="enable-log-streaming"></a>Streaming-logboek inschakelen

In **Server Explorer**, vouw **Azure** > **App Service**.

Vouw de **myResourceGroup** resourcegroep, u hebt gemaakt tijdens het maken van de Azure-web-app.

Met de rechtermuisknop op uw Azure-web-app en selecteer **Streaming logboeken bekijken**.

![Streaming-logboek inschakelen](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

De logboeken worden nu gestreamd naar de **uitvoer** venster. 

![In het uitvoervenster streaming logboek](./media/app-service-web-tutorial-dotnet-sqldatabase/log-streaming-pane.png)

Echter, u Zie geen berichten traceren nog. Dat omdat wanneer u eerst selecteert **Streaming logboeken bekijken**, stelt uw Azure-web-app van de tracering naar `Error`, die alleen legt foutgebeurtenissen vast in (met de `Trace.TraceError()` methode).

### <a name="change-trace-levels"></a>De traceringsniveaus wijzigen

Als u wilt wijzigen van de traceringsniveaus voor andere traceringsberichten uitvoeren, gaat u terug naar **Server Explorer**.

Opnieuw met de rechtermuisknop op uw Azure-web-app en selecteer **weergave-instellingen**.

In de **toepassingslogboeken (bestandssysteem)** vervolgkeuzelijst **uitgebreid**. Klik op **Opslaan**.

![Wijzig het traceringsniveau in uitgebreid](./media/app-service-web-tutorial-dotnet-sqldatabase/trace-level-verbose.png)

> [!TIP]
> U kunt experimenteren met verschillende traceringsniveaus om te zien welke typen berichten worden weergegeven voor elk bedreigingsniveau. Bijvoorbeeld, de **informatie** niveau omvat alle logboeken die zijn gemaakt door `Trace.TraceInformation()`, `Trace.TraceWarning()`, en `Trace.TraceError()`, maar niet de logboeken die zijn gemaakt door `Trace.WriteLine()`.
>
>

Navigeer in uw browser naar uw web-app opnieuw op *http://&lt;uw app-naam >. azurewebsites.net*, probeer het vervolgens te klikken om de toepassing van de lijst met taken in Azure. Nu de traceringsberichten worden gestreamd naar de **uitvoer** venster in Visual Studio.

```console
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```



### <a name="stop-log-streaming"></a>Logboek streaming stoppen

Als u wilt de streaming-log service stopt, klikt u op de **bewaking stopt** knop in de **uitvoer** venster.

![Logboek streaming stoppen](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## <a name="manage-your-azure-web-app"></a>Uw Azure-web-app beheren

Ga naar de [Azure-portal](https://portal.azure.com) om te zien van de web-app die u hebt gemaakt. 



Klik vanuit het linkermenu op **App Service** en klik op de naam van uw Azure-web-app.

![Navigatie in de portal naar de Azure-web-app](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

U bevindt zich in uw web-app-pagina. 

Standaard ziet u de portal de **overzicht** pagina. Deze pagina geeft u een overzicht van hoe uw app presteert. Hier kunt u ook algemene beheertaken uitvoeren, zoals bladeren, stoppen, starten, opnieuw opstarten en verwijderen. De tabbladen aan de linkerkant van de pagina's worden weergegeven de andere configuratie dat kunt u openen. 

![App Service-pagina in Azure Portal](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

[!INCLUDE [Clean up section](../../includes/clean-up-section-portal-web-app.md)]

<a name="next"></a>

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Maken van een SQL-Database in Azure
> * Een ASP.NET-app verbinden met SQL Database
> * De app implementeren in Azure
> * Bijwerken van het gegevensmodel en de app implementeren
> * Logboeken van de stroom van Azure naar uw terminal
> * De app in de Azure portal beheren

Ga naar de volgende zelfstudie voor meer informatie over hoe u een aangepaste DNS-naam toegewezen aan de web-app.

> [!div class="nextstepaction"]
> [Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps](app-service-web-tutorial-custom-domain.md)
