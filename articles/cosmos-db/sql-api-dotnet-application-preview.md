---
title: Zelfstudie voor het ontwikkelen van een ASP.NET MVC-webtoepassing met Azure Cosmos DB met behulp van de .Net SDK (preview).
description: In deze zelfstudie wordt uitgelegd hoe u een ASP.NET MVC-webtoepassing kunt maken met Azure Cosmos DB. U gaat JSON-gegevens opslaan en openen vanuit een taken-app die wordt gehost op Azure.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 12/03/2018
ms.author: dech
ms.openlocfilehash: e3ad852246b4b78d5ed7ac938348e59e9b7e6ce0
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54037120"
---
# <a name="tutorial-develop-an-aspnet-mvc-web-application-with-azure-cosmos-db-by-using-net-preview-sdk"></a>Zelfstudie: een ASP.NET MVC-webtoepassing ontwikkelen met Azure Cosmos DB met behulp van de .NET SDK (preview) 

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [.NET (preview)](sql-api-dotnet-application-preview.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)


In deze zelfstudie wordt uitgelegd hoe u Azure Cosmos DB gebruikt om gegevens uit een ASP.NET MVC-toepassing die wordt gehost op Azure op te slaan en te openen. In deze zelfstudie gebruikt u de .Net SDK V3, momenteel in preview. De volgende afbeelding toont de webpagina die u gaat bouwen met behulp van het voorbeeld in dit artikel:
 
![Schermopname van de takenlijst MVC-webtoepassing die is gemaakt met deze zelfstudie - Stapsgewijze zelfstudie voor ASP NET MVC.](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-image01.png)

Als u geen tijd hebt om de zelfstudie te voltooien, kunt u het volledige voorbeeldproject uit [GitHub][GitHub] downloaden. 

In deze zelfstudie komt het volgende aan bod:

> [!div class="checklist"]
> * Een Azure Cosmos-account maken
> * Een ASP.NET MVC-app maken
> * De app met Azure Cosmos DB verbinden 
> * CRUD-bewerkingen op de gegevens uitvoeren

> [!TIP]
> Voor deze zelfstudie wordt ervan uitgegaan dat u ervaring hebt met ASP.NET MVC en Azure Websites. Als u niet bekend met ASP.NET of de [vereiste hulpprogramma's](#prerequisites) bent, raden we u aan het volledige voorbeeldproject uit [GitHub][GitHub] te downloaden, de vereiste NuGet-pakketten toe te voegen en het project uit te voeren. Zodra u het project gaat maken, kunt u dit artikel lezen voor meer informatie over de code in de context van het project.

## <a name="prerequisites"></a>Vereisten 

Voordat u de instructies in dit artikel uitvoert, moet u beschikken over de volgende resources:

* **Een actief Azure-account:** Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]  

* Microsoft Azure SDK voor .NET voor Visual Studio 2017 is beschikbaar via het Visual Studio-installatieprogramma.

Alle schermopnamen in dit artikel zijn gemaakt met Microsoft Visual Studio Community 2017. Als uw systeem is geconfigureerd met een andere versie, is het mogelijk dat de schermen en opties niet volledig overeenkomen. Als u echter aan de bovenstaande vereisten voldoet, moet deze oplossing werken.

## <a name="create-an-azure-cosmos-account"></a>Stap 1: Een Azure Cosmos-account maken

Begin met het maken van een Azure Cosmos-account. Als u al een SQL-API-account voor Azure Cosmos DB hebt of de Azure Cosmos DB-emulator gebruikt voor deze zelfstudie, kunt u direct doorgaan naar het gedeelte [Een nieuwe ASP.NET MVS-toepassing maken](#create-a-new-mvc-application).

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

In het volgende gedeelte maakt u een nieuwe ASP.NET MVC-toepassing. 

## <a name="create-a-new-mvc-application"></a>Stap 2: nieuwe ASP.NET MVC-toepassing maken

1. Ga in Visual Studio in het menu **Bestand** naar **Nieuw** en selecteer vervolgens **Project**. Het dialoogvenster **Nieuw project** wordt weergegeven.

2. Vouw in het deelvenster **Nieuw project** achtereenvolgens **Geïnstalleerde** sjablonen, **Visual C#** en **Web** uit en selecteer vervolgens **ASP.NET-webtoepassing**. 

   ![Nieuw project voor ASP.NET-webtoepassing maken](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-new-project-dialog.png)

3. Typ in het vak **Naam** de naam van het project. In deze zelfstudie wordt de naam 'todo' gebruikt. Als u een andere naam gebruikt, moet u waar in deze zelfstudie over de naamruimte todo wordt gesproken, de codevoorbeelden aanpassen met de naam die u voor uw toepassing gebruikt. 

4. Selecteer **Bladeren** om naar de map te navigeren waarin u het project wilt maken en kies vervolgens **.Net Framework 4.6.1** of hoger. Selecteer **OK**. 

5. Het dialoogvenster **Nieuwe ASP.NET-webtoepassing** wordt weergegeven. Selecteer in het deelvenster met sjablonen **MVC**.

6. Selecteer **OK** om de scaffolding voor de lege ASP.NET MVC-sjabloon door Visual Studio uit te laten voeren. 

7. Zodra Visual Studio de standaard MVC-toepassing heeft gemaakt, beschikt u over een lege ASP.NET-toepassing die u lokaal kunt uitvoeren.

## <a name="add-nuget-packages"></a>Stap 3: Azure Cosmos DB NuGet-pakket aan het project toevoegen

Nu we de meeste code voor de ASP.NET MVC-framework hebben die nodig is voor deze oplossing, voegen we de NuGet-pakketten toe die zijn vereist voor het verbinden met Azure Cosmos DB.

1. De Azure Cosmos DB .NET SDK wordt verpakt en gedistribueerd als een NuGet-pakket. Als u het NuGet-pakket aan Visual Studio wilt toevoegen, gebruikt u NuGet-pakketbeheer in Visual Studio door in **Solution Explorer** met de rechtermuisknop op het project te klikken en vervolgens **NuGet-pakketten beheren** te selecteren.
   
   ![Schermopname van de opties voor klikken met de rechtermuisknop voor het webtoepassingsproject in Solution Explorer, met NuGet-pakketten beheren gemarkeerd.](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-manage-nuget.png)
   
2. Het dialoogvenster **NuGet-pakketten beheren** wordt weergegeven. Typ in het NuGet-vak **Bladeren** **Microsoft.Azure.Cosmos**. Installeer vanuit de resultaten de 3.0.0.1-previewversie van **Microsoft.Azure.Cosmos**. Hierbij worden het Azure Cosmos DB-pakket en de afhankelijkheden, zoals Newtonsoft.Json, gedownload en geïnstalleerd. Selecteer **OK** in het venster **Voorbeeld** en **I Accept** (Ik ga akkoord) in het venster **License Acceptance** (Licentie accepteren) om de installatie te voltooien.
   
   U kunt eventueel ook de console voor Pakketbeheer gebruiken om het NuGet-pakket te installeren. Hiervoor selecteert u in het menu **Extra** **NuGet Package Manager** (NuGet-pakketbeheer) en vervolgens **Package Manager Console** (Pakketbeheer-console). Typ achter de prompt de volgende opdracht:
   
   ```bash
   Install-Package Microsoft.Azure.Cosmos -Version 3.0.0.1-preview
   ```        

3. Nadat het pakket is geïnstalleerd, moet uw Visual Studio-oplossing de twee nieuwe bibliotheekverwijzingen naar Microsoft.Azure.Cosmos.Client en Newtonsoft.Json bevatten.
  
## <a name="set-up-the-mvc-application"></a>Stap 4: ASP.NET MVC-toepassing instellen

U kunt nu de modellen, weergaven en controllers toevoegen aan deze MVC-toepassing:

* [Een model toevoegen](#add-a-model).
* [Een controller toevoegen](#add-a-controller).
* [Weergaven toevoegen](#add-views).

### <a name="add-a-model"></a> Een model toevoegen

1. Klik in **Solution Explorer** met de rechtermuisknop op de map **Modellen** en selecteer achtereenvolgens **Toevoegen** en **Klasse**. Het dialoogvenster **Nieuw item toevoegen** wordt weergegeven.

1. Noem uw nieuwe klasse **TodoItem.cs** en selecteer **Toevoegen**. 

1. Vervang de code in de klasse "Todoitem" door de volgende code:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-web-app/src/Models/TodoItem.cs)]
   
   De gegevens die in Azure Cosmos DB zijn opgeslagen, worden doorgegeven via de kabel en opgeslagen als JSON. U kunt het kenmerk **JsonProperty** gebruiken, zoals wordt beschreven in de klasse **TodoItem** die u hebt gemaakt, om te bepalen hoe uw objecten door JSON.NET worden geserialiseerd/gedeserialiseerd. U kunt niet alleen de indeling van de eigenschapsnaam voor JSON bepalen, maar ook de naam van uw .NET-eigenschappen wijzigen, zoals u deed met de eigenschap **Description**. 

### <a name="add-a-controller"></a>Een controller toevoegen

1. Klik in **Solution Explorer** met de rechtermuisknop op de map **Controllers** en selecteer achtereenvolgens **Toevoegen** en **Controller**. Het dialoogvenster **Add Scaffold** (Scaffold toevoegen) wordt weergegeven.

1. Selecteer **MVC 5 Controller - Empty** (MVC 5-controller - Leeg) en selecteer vervolgens **Toevoegen**.

   ![Schermopname van het dialoogvenster Add Scaffold met de optie MVC 5 Controller - Empty gemarkeerd](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-controller-add-scaffold.png)

1. Noem uw nieuwe controller **ItemController, en vervang de code in het bestand door de volgende code:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-web-app/src/Controllers/ItemController.cs)]

   Het kenmerk **ValidateAntiForgeryToken** wordt hier gebruikt om deze toepassing te beschermen tegen aanvallen via aanvraagvervalsing op meerdere sites. Het volstaat echter niet om dit kenmerk alleen toe te voegen. Uw weergaven moeten ook samenwerken met dit anti-vervalsingstoken. Zie [Voorkomen van aanvraagvervalsing op meerdere sites][Preventing Cross-Site Request Forgery] voor meer informatie over dit onderwerp en voorbeelden van een juiste implementatie. De broncode op [GitHub][GitHub] beschikt over de volledige implementatie.
   
   We gebruiken ook het kenmerk **Bind** voor de methodeparameter om u te beschermen tegen over-postingaanvallen. Zie [Eenvoudige CRUD-bewerkingen in ASP.NET MVC][Basic CRUD Operations in ASP.NET MVC] voor meer informatie.
    
### <a name="add-views"></a>Weergaven toevoegen

Vervolgens gaat u de volgende drie weergaven maken: 

* [Een weergave voor een lijst met items toevoegen](#AddItemIndexView).
* [Een weergave voor nieuwe items toevoegen](#AddNewIndexView).
* [Een weergave voor het bewerken van items toevoegen](#AddEditIndexView).

#### <a name="AddItemIndexView"></a>Een weergave voor een lijst met items toevoegen

1. Vouw in **Solution Explorer** de map **Weergaven** uit en klik met de rechtermuisknop op de lege map **Item** die Visual Studio voor u heeft gemaakt toen u **ItemController** hebt toegevoegd. Klik vervolgens op **Toevoegen** en **Weergave**.
   
   ![Schermopname van Solution Explorer waarin de map Item wordt weergegeven die Visual Studio heeft gemaakt en waarin de opdrachten voor het toevoegen van een weergave zijn gemarkeerd](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-add-view.png)

2. Werk in het dialoogvenster **Weergave toevoegen** de volgende waarden bij:
   
   * In het vak **Weergavenaam** typt u ***Index***.
   * Selecteer in het vak **Sjabloon** de optie ***Lijst***.
   * Selecteer in het vak **Modelklasse** de optie ***Item (todo.Models)***.
   * Typ in het veld voor de indelingspagina ***~/Views/Shared/_Layout.cshtml***.
     
   ![Schermopname van het dialoogvenster Weergave toevoegen](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-add-view-dialog.png)

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

Allereerst moet u een klasse toevoegen die de logica bevat voor de verbinding met en het gebruik van Azure Cosmos DB. Voor deze zelfstudie kapselen we deze logica in een klasse genaamd TodoItemService.cs in. Deze code leest de Azure Cosmos DB-eindpuntwaarden uit het configuratiebestand en voert CRUD-bewerkingen uit zoals het weergeven van onvolledige items en het maken, bewerken en verwijderen van de items. 

1. Maak in **Solution Explorer** een nieuwe map onder uw project met de naam **Services**.

1. Klik met de rechtermuisknop op de map **Services** en selecteer **Toevoegen** en **Klasse**. Noem de nieuwe klasse **TodoItemService** en selecteer **Toevoegen**.

1. Voeg de volgende code toe aan de klasse **TodoItemService** en vervang de code in het bestand door de volgende code:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-web-app/src/Services/TodoItemService.cs)]
 
1. De vorige code leest de verbindingstekenreekswaarden uit het configuratiebestand. Als u de waarden van de verbindingstekenreeks van uw Azure Cosmos-account wilt bijwerken, opent u het bestand **Web.config** in uw project en voegt u de volgende regels toe onder het gedeelte `<AppSettings>`:

   ```csharp
    <add key="endpoint" value="<enter the URI from the Keys blade of the Azure Portal>" />
    <add key="primaryKey" value="<enter the PRIMARY KEY, or the SECONDARY KEY, from the Keys blade of the Azure  Portal>" />
    <add key="database" value="Tasks" />
    <add key="container" value="Items" />
   ```
 
1. Werk de waarden voor endpoint en primarykey bij met de waarden die zijn opgehaald uit de blade **Sleutels** in de Azure-portal. Gebruik de **URI** op de blade Sleutels als waarde voor de endpoint-instelling en gebruik de **PRIMAIRE SLEUTEL** of **SECUNDAIRE SLEUTEL** op de blade Sleutels voor de sleutelinstelling. Hiermee is de Azure Cosmos DB aan de toepassing gekoppeld en kunnen we de toepassingslogica toevoegen.

1. Open **Global.asax.cs** en voeg de volgende regel aan de methode **Application_Start** toe. 
   
   ```csharp
   TodoItemService.Initialize().GetAwaiter().GetResult();
   ```

   Op dit moment moet de oplossing uw project kunnen maken zonder dat er fouten optreden. Als u de toepassing nu uitvoert, moeten de weergaven **HomeController** en **Index** van die controller worden geopend. Dit is het standaardgedrag voor het MVC-sjabloonproject dat we aan het begin hebben gekozen. U kunt de routering op deze MVC-toepassing wijzigen om dit gedrag te veranderen.

1. Open ***App\_Start\RouteConfig.cs***, zoek de regel die begint met "defaults:" en werk deze bij met de volgende code:

   ```csharp
   defaults: new { controller = "Item", action = "Index", id = UrlParameter.Optional }
   ```

  Met deze code stelt u in dat ASP.NET MVC niet **Home** maar **Item** als controller gebruikt en **Index** als weergave gebruikt als u in de URL geen waarde hebt opgegeven voor het routeringsgedrag.

Als u de toepassing nu uitvoert, wordt uw **ItemController** aangeroepen die de GetItems-methoden aanroepen vanaf de TodoItemService-klasse die u in het volgende gedeelte definieert. 

Als u dit project nu maakt en uitvoert, ziet u iets dat vergelijkbaar is met het volgende.    

![Schermopname van de takenlijstwebtoepassing die is gemaakt met deze databasezelfstudie](./media/sql-api-dotnet-application-preview/build-and-run-the-project-now.png)


## <a name="run-the-application"></a>Stap 6: De toepassing lokaal uitvoeren

Volg deze stappen als u de toepassing wilt testen op een lokale machine:

1. Druk in Visual Studio op F5 om de toepassing in de foutopsporingsmodus op te bouwen. De toepassing wordt opgebouwd en wordt er een browser gestart met het lege rasterpagina dat we eerder hebben gezien:
   
   ![Schermopname van de takenlijstwebtoepassing die is gemaakt met deze databasezelfstudie](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-create-an-item-a.png)
       
2. Klik op de koppeling **Nieuw maken** en voeg waarden toe aan de velden **Naam** en **Beschrijving**. Schakel het selectievakje **Voltooid** niet in, anders wordt het nieuwe item toegevoegd met een onvoltooide status en wordt het niet weergegeven in de aanvankelijke lijst.
   
3. Als u op **Maken** klikt, keert u terug naar de weergave **Index** en wordt uw item in de lijst weergegeven. U kunt eventueel nog enkele items aan uw takenlijst toevoegen.

    ![Schermopname van de weergave Index](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-create-an-item.png)
  
4. Klik op **Bewerken** naast een **Item** in de lijst, zodat u wordt omgeleid naar de weergave **Bewerken**. Hier kunt u de eigenschappen van uw object bijwerken, inclusief de vlag **Voltooid**. Als u de vlag **Voltooid** markeert en op **Opslaan** klikt, wordt het **Item** verwijderd uit de lijst met onvolledige taken.
   
   ![Schermopname van de weergave Index met het selectievakje Voltooid ingeschakeld](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-completed-item.png)

5. Zodra u de app hebt getest, drukt u op Ctrl + F5 om de foutopsporing voor de app te stoppen. U kunt de app nu implementeren.

## <a name="deploy-the-application-to-azure"></a>Stap 7: De toepassing implementeren 
Nu de volledige toepassing correct werkt met Azure Cosmos DB, kunt u de web-app implementeren in Azure App Service.  

1. Klik in **Solution Explorer** met de rechtermuisknop op het project en selecteer **Publiceren** om de toepassing te publiceren.
   
2. Selecteer in het dialoogvenster **Publiceren** **Microsoft Azure App Service** en selecteer daarna **Nieuwe maken** om een App Service-profiel te maken of kies **Bestaand profiel selecteren** om een bestaand profiel te gebruiken.

3. Als u al een Azure App Service-profiel hebt, selecteert u **Abonnement** in de vervolgkeuzelijst. Sorteer met de filter **Weergave** op resourcegroep of resourcetype. Zoek vervolgens de vereiste Azure App Service en selecteer **OK**. 
   
   ![Dialoogvenster App Service in Visual Studio](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-app-service.png)

4. Klik op **Nieuw profiel maken** in het dialoogvenster **Publiceren** om een nieuw Azure App Service-profiel te maken. Voer in het dialoogvenster **App Service maken** de naam van uw webtoepassing en het abonnement, de resourcegroep en het App Service-abonnement in en selecteer vervolgens **Maken**.

   ![Dialoogvenster App-service maken in Visual Studio](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-create-app-service.png)

Over een paar seconden zal Visual Studio uw webtoepassing publiceren en een browser starten waarin u kunt zien hoe uw project in Azure wordt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u een ASP.NET MVC-webtoepassing maakt die toegang tot gegevens die zijn opgeslagen in Azure Cosmos DB kan krijgen. U kunt nu verdergaan met het volgende artikel:

> [!div class="nextstepaction"]
> [Een Java-toepassing maken voor toegang tot gegevens die zijn opgeslagen in een SQL-API-account van Azure Cosmos DB]( sql-api-java-application.md)


[Visual Studio Express]: https://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: https://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: https://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: https://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/cosmos-dotnet-todo-app
