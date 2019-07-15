---
title: 'ASP.NET Core MVC-zelfstudie voor Azure Cosmos DB: Webtoepassingsontwikkeling'
description: ASP.NET Core MVC-zelfstudie voor het maken van een MVC-webtoepassing met Azure Cosmos DB. U JSON opslaan en toegang tot gegevens van een takenlijst-app die wordt gehost op Azure App Service - zelfstudie voor ASP NET Core MVC stap voor stap.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 06/24/2019
ms.author: sngun
ms.openlocfilehash: 85d9cbe7d0807ca0e7951e1e12d1edbbf7c921db
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67985906"
---
# <a name="tutorial-develop-an-aspnet-core-mvc-web-application-with-azure-cosmos-db-by-using-net-sdk"></a>Zelfstudie: Een ASP.NET Core MVC-webtoepassing met Azure Cosmos DB ontwikkelen met behulp van .NET SDK 

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)


In deze zelfstudie wordt uitgelegd hoe u Azure Cosmos DB gebruikt om gegevens uit een ASP.NET MVC-toepassing die wordt gehost op Azure op te slaan en te openen. In deze zelfstudie gebruikt u de .NET SDK V3. De volgende afbeelding toont de webpagina die u gaat bouwen met behulp van het voorbeeld in dit artikel:
 
![Schermafbeelding van de takenlijst MVC-webtoepassing die is gemaakt door deze zelfstudie - ASP NET Core MVC zelfstudie stapsgewijs](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-image01.png)

Als u geen tijd om de zelfstudie te voltooien, kunt u het volledige voorbeeldproject via downloaden [GitHub][GitHub].

In deze zelfstudie komt het volgende aan bod:

> [!div class="checklist"]
> * Een Azure Cosmos-account maken
> * Het maken van een ASP.NET Core MVC-app
> * De app met Azure Cosmos DB verbinden 
> * CRUD-bewerkingen op de gegevens uitvoeren

> [!TIP]
> In deze zelfstudie wordt ervan uitgegaan dat u ervaring met behulp van ASP.NET Core MVC en Azure App Service hebt. Als u niet bekend met ASP.NET Core of de [vereiste hulpprogramma's](#prerequisites), raden we u voor het downloaden van het volledige voorbeeldproject via [GitHub][GitHub], de vereiste NuGet-pakketten toevoegen en voer deze uit. Zodra u het project gaat maken, kunt u dit artikel lezen voor meer informatie over de code in de context van het project.

## <a name="prerequisites"></a>Vereisten 

Voordat u de instructies in dit artikel uitvoert, moet u beschikken over de volgende resources:

* **Een actief Azure-account:** Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]  

De schermafbeeldingen in dit artikel zijn gemaakt met behulp van Microsoft Visual Studio Community 2019. Als uw systeem is geconfigureerd met een andere versie, is het mogelijk dat de schermen en opties niet volledig overeenkomen. Als u echter aan de bovenstaande vereisten voldoet, moet deze oplossing werken.

## <a name="create-an-azure-cosmos-account"></a>Stap 1: Een Azure Cosmos-account maken

Begin met het maken van een Azure Cosmos-account. Als u al een SQL-API-account voor Azure Cosmos DB hebt of de Azure Cosmos DB-emulator gebruikt voor deze zelfstudie, kunt u direct doorgaan naar het gedeelte [Een nieuwe ASP.NET MVS-toepassing maken](#create-a-new-mvc-application).

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

In de volgende sectie maakt u een nieuwe ASP.NET Core MVC-toepassing. 

## <a name="create-a-new-mvc-application"></a>Stap 2: Maak een nieuwe ASP.NET Core MVC-toepassing

1. Ga in Visual Studio in het menu **Bestand** naar **Nieuw** en selecteer vervolgens **Project**. Het dialoogvenster **Nieuw project** wordt weergegeven.

2. In de **nieuw Project** venster, gebruik de **sjablonen zoeken** invoervak zoeken naar 'Web' en selecteer vervolgens **ASP.NET Core-webtoepassing**. 

   ![Nieuwe ASP.NET Core web application-project maken](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-new-project-dialog.png)

3. Typ in het vak **Naam** de naam van het project. In deze zelfstudie wordt de naam 'todo' gebruikt. Als u gebruiken dan deze naam wilt, waar deze zelfstudie wordt de naamruimte Todo wordt gesproken besproken pas vervolgens de codevoorbeelden voor het gebruik van alles wat u de naam van uw toepassing. 

4. Selecteer **Bladeren** om te navigeren naar de map waar u wilt maken van het project. Selecteer **Maken**. 

5. De **maken van een nieuwe ASP.NET Core-webtoepassing** in het dialoogvenster wordt weergegeven. Selecteer in de lijst met sjablonen, **webtoepassing (Model-View-Controller)** .

6. Selecteer **maken** en de opbouw rond de lege ASP.NET Core MVC-sjabloon in Visual Studio. 

7. Zodra Visual Studio de standaard MVC-toepassing heeft gemaakt, beschikt u over een lege ASP.NET-toepassing die u lokaal kunt uitvoeren.

## <a name="add-nuget-packages"></a>Stap 3: Azure Cosmos DB NuGet-pakket aan het project toevoegen

Nu we de meeste van de code van de ASP.NET Core MVC-framework die we nodig hebben voor deze oplossing hebt, gaat toevoegen de NuGet-pakketten dat is vereist voor het verbinding maken met Azure Cosmos DB.

1. De Azure Cosmos DB .NET SDK wordt verpakt en gedistribueerd als een NuGet-pakket. Als u het NuGet-pakket aan Visual Studio wilt toevoegen, gebruikt u NuGet-pakketbeheer in Visual Studio door in **Solution Explorer** met de rechtermuisknop op het project te klikken en vervolgens **NuGet-pakketten beheren** te selecteren.
   
   ![Schermopname van de opties voor klikken met de rechtermuisknop voor het webtoepassingsproject in Solution Explorer, met NuGet-pakketten beheren gemarkeerd.](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-manage-nuget.png)
   
2. Het dialoogvenster **NuGet-pakketten beheren** wordt weergegeven. Typ in het NuGet-vak **Bladeren** **Microsoft.Azure.Cosmos**. Installeer vanuit de resultaten de **Microsoft.Azure.Cosmos** pakket. Deze downloadt en installeert de Azure Cosmos DB-pakket en de bijbehorende afhankelijkheden. Selecteer **ik ga akkoord** in de **acceptatie van de licentie** venster om de installatie te voltooien.
   
   U kunt eventueel ook de console voor Pakketbeheer gebruiken om het NuGet-pakket te installeren. Hiervoor selecteert u in het menu **Extra** **NuGet Package Manager** (NuGet-pakketbeheer) en vervolgens **Package Manager Console** (Pakketbeheer-console). Typ achter de prompt de volgende opdracht:
   
   ```bash
   Install-Package Microsoft.Azure.Cosmos
   ```        

3. Nadat het pakket is geïnstalleerd, moet uw Visual Studio-project de bibliotheekverwijzing naar Microsoft.Azure.Cosmos bevatten.
  
## <a name="set-up-the-mvc-application"></a>Stap 4: De ASP.NET Core MVC-toepassing instellen

U kunt nu de modellen, weergaven en controllers toevoegen aan deze MVC-toepassing:

* [Een model toevoegen](#add-a-model).
* [Een controller toevoegen](#add-a-controller).
* [Weergaven toevoegen](#add-views).

### <a name="add-a-model"></a> Een model toevoegen

1. Klik in **Solution Explorer** met de rechtermuisknop op de map **Modellen** en selecteer achtereenvolgens **Toevoegen** en **Klasse**. Het dialoogvenster **Nieuw item toevoegen** wordt weergegeven.

1. Noem uw nieuwe klasse **Item.cs** en selecteer **toevoegen**. 

1. Vervang de code in de klasse 'Item.cs' vervolgens met de volgende code:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-core-web-app/src/Models/Item.cs)]
   
   De gegevens die in Azure Cosmos DB zijn opgeslagen, worden doorgegeven via de kabel en opgeslagen als JSON. Als u wilt bepalen hoe uw objecten geserialiseerd/gedeserialiseerd door JSON.NET worden, kunt u de **JsonProperty** kenmerk zoals geïllustreerd in de **Item** klasse die u hebt gemaakt. Niet alleen kunt u bepalen de indeling van de naam van de eigenschap dat gaan in JSON, u ook in uw .NET-eigenschappen wijzigen kunt zoals u dit hebt gedaan met de **voltooid** eigenschap. 

### <a name="add-a-controller"></a>Een controller toevoegen

1. Klik in **Solution Explorer** met de rechtermuisknop op de map **Controllers** en selecteer achtereenvolgens **Toevoegen** en **Controller**. Het dialoogvenster **Add Scaffold** (Scaffold toevoegen) wordt weergegeven.

1. Selecteer **MVC-Controller - leeg** en selecteer **toevoegen**.

   ![Schermafbeelding van het dialoogvenster Add Scaffold met de MVC-Controller - leeg-optie is gemarkeerd](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-controller-add-scaffold.png)

1. Noem uw nieuwe controller **ItemController**, en vervang de code in het bestand met de volgende code:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-core-web-app/src/Controllers/ItemController.cs)]

   Het kenmerk **ValidateAntiForgeryToken** wordt hier gebruikt om deze toepassing te beschermen tegen aanvallen via aanvraagvervalsing op meerdere sites. Het volstaat echter niet om dit kenmerk alleen toe te voegen. Uw weergaven moeten ook samenwerken met dit anti-vervalsingstoken. Zie voor meer informatie over dit onderwerp en voorbeelden van het juiste implementatie [Cross-Site te voorkomen dat aanvragen vervalsing][Preventing Cross-Site Request Forgery] . The source code provided on [GitHub][GitHub] beschikt over de volledige implementatie.

   We gebruiken ook het kenmerk **Bind** voor de methodeparameter om u te beschermen tegen over-postingaanvallen. Zie voor meer informatie, [eenvoudige CRUD-bewerkingen in ASP.NET MVC][Basic CRUD Operations in ASP.NET MVC].

### <a name="add-views"></a>Weergaven toevoegen

Vervolgens gaat u de volgende drie weergaven maken: 

* [Een weergave voor een lijst met items toevoegen](#AddItemIndexView).
* [Een weergave voor nieuwe items toevoegen](#AddNewIndexView).
* [Een weergave voor het bewerken van items toevoegen](#AddEditIndexView).

#### <a name="AddItemIndexView"></a>Een weergave voor een lijst met items toevoegen

1. Vouw in **Solution Explorer** de map **Weergaven** uit en klik met de rechtermuisknop op de lege map **Item** die Visual Studio voor u heeft gemaakt toen u **ItemController** hebt toegevoegd. Klik vervolgens op **Toevoegen** en **Weergave**.
   
   ![Schermopname van Solution Explorer met de map die Visual Studio heeft gemaakt met de opdrachten van de weergave toevoegen gemarkeerd](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-add-view.png)

2. Werk in het dialoogvenster **Weergave toevoegen** de volgende waarden bij:
   
   * In het vak **Weergavenaam** typt u ***Index***.
   * Selecteer in het vak **Sjabloon** de optie ***Lijst***.
   * Selecteer in het vak **Modelklasse** de optie ***Item (todo.Models)***.
   * Typ in het veld voor de indelingspagina ***~/Views/Shared/_Layout.cshtml***.
     
   ![Schermopname van het dialoogvenster weergave toevoegen](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-add-view-dialog.png)

3. Zodra u deze waarden hebt toegevoegd, klikt u op **Toevoegen** en wordt er een nieuwe sjabloonweergave in Visual Studio gemaakt. Zodra dit klaar is, wordt het nieuwe cshtml-bestand geopend. U kunt dit bestand in Visual Studio voorlopig sluiten, aangezien dit pas later aan bod komt.

#### <a name="AddNewIndexView"></a>Een weergave voor nieuwe items toevoegen

Maak een nieuwe weergave om items te maken, vergelijkbaar met hoe u een weergave hebt gemaakt voor het weergeven van items, door de volgende stappen te volgen:

1. Klik in **Solution Explorer** met de rechtermuisknop op de map **Item** en selecteer achtereenvolgens **Toevoegen** en **Weergave**.

1. Werk in het dialoogvenster **Weergave toevoegen** de volgende waarden bij:
   
   * Typ in het vak **Weergavenaam** ***Maken***.
   * Selecteer in het vak **Sjabloon** de optie ***Maken***.
   * Selecteer in het vak **Modelklasse** de optie ***Item (todo.Models)***.
   * Typ in het veld voor de indelingspagina ***~/Views/Shared/_Layout.cshtml***.
   * Selecteer **Toevoegen**.
   
#### <a name="AddEditIndexView"></a>Een weergave voor het bewerken van items toevoegen

En tot slot voegt u een weergave voor het bewerken van items toe met de volgende stappen:

1. Klik in **Solution Explorer** met de rechtermuisknop op de map **Item** en selecteer achtereenvolgens **Toevoegen** en **Weergave**.

1. Voer in het dialoogvenster **Weergave toevoegen** de volgende handelingen uit:
   
   * Typ in het vak **Weergavenaam** ***Bewerken***.
   * Selecteer in het vak **Sjabloon** de optie ***Bewerken***.
   * Selecteer in het vak **Modelklasse** de optie ***Item (todo.Models)***.
   * Typ in het veld voor de indelingspagina ***~/Views/Shared/_Layout.cshtml***.
   * Selecteer **Toevoegen**.

Zodra dit is gebeurd, sluit u alle cshtml-documenten in Visual Studio. U keert later naar deze weergaven terug.

## <a name="connect-to-cosmosdb"></a>Stap 5: Verbinding maken met Azure Cosmos DB 

Nu de standaardwerkzaamheden voor MVC zijn voltooid, kunt u de code voor het verbinding maken met Azure Cosmos DB toevoegen en CRUD-bewerkingen uitvoeren. 

### <a name="perform-crud-operations"></a> CRUD-bewerkingen op de gegevens uitvoeren

Allereerst moet u een klasse toevoegen die de logica bevat voor de verbinding met en het gebruik van Azure Cosmos DB. Voor deze zelfstudie voegen we deze logica in een klasse genaamd `CosmosDBService` en een interface met de naam `ICosmosDBService`. Deze service wordt uitgevoerd de CRUD en feed bewerkingen zoals het vermelden van onvolledige items, maken, bewerken en verwijderen van de artikelen te lezen. 

1. Maak in **Solution Explorer** een nieuwe map onder uw project met de naam **Services**.

1. Klik met de rechtermuisknop op de map **Services** en selecteer **Toevoegen** en **Klasse**. Noem de nieuwe klasse **CosmosDBService** en selecteer **toevoegen**.

1. Voeg de volgende code aan de **CosmosDBService** klasse en vervang de code in het bestand met de volgende code:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-core-web-app/src/Services/CosmosDbService.cs)]

1. Herhaal de stappen 2-3, maar deze keer, voor een klasse met de naam **ICosmosDBService**, en voeg de volgende code toe:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-core-web-app/src/Services/ICosmosDbService.cs)]
 
1. De vorige code ontvangt een `CosmosClient` als onderdeel van de constructor. Na de ASP.NET Core-pijplijn moet van het project naar **Startup.cs** en initialiseren van de client op basis van de configuratie als een Singleton-instantie om te worden opgenomen via [Afhankelijkheidsinjectie](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). In de **ConfigureServices** handler, definiëren we:

    ```csharp
    services.AddSingleton<ICosmosDbService>(InitializeCosmosClientInstanceAsync(Configuration.GetSection("CosmosDb")).GetAwaiter().GetResult());
    ```

1. Binnen hetzelfde bestand, definiëren we onze Help-methode **InitializeCosmosClientInstanceAsync**, die wordt de configuratie niet lezen en initialiseren van de client.

    ```csharp
    private static async Task<CosmosDbService> InitializeCosmosClientInstanceAsync(IConfigurationSection configurationSection)
    {
        string databaseName = configurationSection.GetSection("DatabaseName").Value;
        string containerName = configurationSection.GetSection("ContainerName").Value;
        string account = configurationSection.GetSection("Account").Value;
        string key = configurationSection.GetSection("Key").Value;
        CosmosClientBuilder clientBuilder = new CosmosClientBuilder(account, key);
        CosmosClient client = clientBuilder
                            .WithConnectionModeDirect()
                            .Build();
        CosmosDbService cosmosDbService = new CosmosDbService(client, databaseName, containerName);
        Database database = await client.CreateDatabaseIfNotExistsAsync(databaseName);
        await database.CreateContainerIfNotExistsAsync(containerName, "/id");

        return cosmosDbService;
    }
    ```

1. De configuratie is gedefinieerd in het project tot **appsettings.json** bestand. Open het en toevoegen van een sectie met de naam **CosmosDb**:

   ```csharp
     "CosmosDb": {
        "Account": "<enter the URI from the Keys blade of the Azure Portal>",
        "Key": "<enter the PRIMARY KEY, or the SECONDARY KEY, from the Keys blade of the Azure  Portal>",
        "DatabaseName": "Tasks",
        "ContainerName": "Items"
      }
   ```
 
Nu als u de toepassing uitvoert, van ASP.NET Core pijplijn wordt geïnstantieerd **CosmosDbService** en onderhouden van een enkele instantie als Singleton; wanneer **ItemController** wordt gebruikt voor het verwerken van aanvragen van clients aan clientzijde, het deze één exemplaar ontvangt en kunnen deze gebruiken CRUD-bewerkingen uit te voeren.

Als u maken en dit project nu uitvoeren, moet u nu iets zien er als volgt uitzien:

![Schermafbeelding van de takenlijstwebtoepassing die zijn gemaakt door deze databasezelfstudie](./media/sql-api-dotnet-application/build-and-run-the-project-now.png)


## <a name="run-the-application"></a>Stap 6: De toepassing lokaal uitvoeren

Volg deze stappen als u de toepassing wilt testen op een lokale machine:

1. Druk in Visual Studio op F5 om de toepassing in de foutopsporingsmodus op te bouwen. De toepassing wordt opgebouwd en wordt er een browser gestart met het lege rasterpagina dat we eerder hebben gezien:
   
   ![Schermafbeelding van de takenlijstwebtoepassing die zijn gemaakt door deze zelfstudie](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-an-item-a.png)
       
2. Klik op de koppeling **Nieuw maken** en voeg waarden toe aan de velden **Naam** en **Beschrijving**. Schakel het selectievakje **Voltooid** niet in, anders wordt het nieuwe item toegevoegd met een onvoltooide status en wordt het niet weergegeven in de aanvankelijke lijst.
   
3. Als u op **Maken** klikt, keert u terug naar de weergave **Index** en wordt uw item in de lijst weergegeven. U kunt eventueel nog enkele items aan uw takenlijst toevoegen.

    ![Schermafbeelding van de weergave Index](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-an-item.png)
  
4. Klik op **Bewerken** naast een **Item** in de lijst, zodat u wordt omgeleid naar de weergave **Bewerken**. Hier kunt u de eigenschappen van uw object bijwerken, inclusief de vlag **Voltooid**. Als u markeert de **voltooid** markeren en op **opslaan**, wordt de **Item** wordt weergegeven als voltooid in de lijst.
   
   ![Schermafbeelding van de weergave Index met het selectievakje voltooid ingeschakeld](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-completed-item.png)

5. U kunt controleren op elk gewenst moment de status van de gegevens in de Azure Cosmos DB-service met [Cosmos Explorer](https://cosmos.azure.com) of de Azure Cosmos DB Emulator Data Explorer.

6. Zodra u de app hebt getest, drukt u op Ctrl + F5 om de foutopsporing voor de app te stoppen. U kunt de app nu implementeren.

## <a name="deploy-the-application-to-azure"></a>Stap 7: De toepassing implementeren 
Nu de volledige toepassing correct werkt met Azure Cosmos DB, kunt u de web-app implementeren in Azure App Service.  

1. Klik in **Solution Explorer** met de rechtermuisknop op het project en selecteer **Publiceren** om de toepassing te publiceren.
   
2. In de **publiceren** in het dialoogvenster, selecteer **App Service**en selecteer vervolgens **nieuw** voor het maken van een App Service-profiel of kies **bestaande selecteren**om een bestaand profiel te gebruiken.

3. Als u al een Azure App Service-profiel hebt, selecteert u **Abonnement** in de vervolgkeuzelijst. Sorteer met de filter **Weergave** op resourcegroep of resourcetype. Zoek vervolgens de vereiste Azure App Service en selecteer **OK**.
   
   ![Dialoogvenster App Service in Visual Studio](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-app-service.png)

4. Klik op **Nieuw profiel maken** in het dialoogvenster **Publiceren** om een nieuw Azure App Service-profiel te maken. Voer in het dialoogvenster **App Service maken** de naam van uw webtoepassing en het abonnement, de resourcegroep en het App Service-abonnement in en selecteer vervolgens **Maken**.

   ![Dialoogvenster App-service maken in Visual Studio](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-app-service.png)

Binnen een paar seconden, Visual Studio uw web-App publiceert en start een browser waarin u uw project uitvoeren in Azure kunt zien.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd over het bouwen van een ASP.NET Core MVC-webtoepassing die toegang gegevens die zijn opgeslagen in Azure Cosmos DB tot krijgen. U kunt nu verdergaan met het volgende artikel:

* [Meer informatie over het partitioneren van uw gegevens in Azure Cosmos DB](./partitioning-overview.md)
* [Meer informatie over hoe u kunt meer geavanceerde query's in Azure Cosmos DB](./how-to-sql-query.md)
* [Meer informatie over hoe u uw gegevens modelleren in een complexer scenario](./how-to-model-partition-example.md)


[Visual Studio Express]: https://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: https://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: https://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: https://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/cosmos-dotnet-core-todo-app
