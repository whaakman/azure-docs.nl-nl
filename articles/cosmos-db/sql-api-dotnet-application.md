---
title: 'ASP.NET MVC-zelfstudie voor Azure Cosmos DB: webtoepassingsontwikkeling | Microsoft Docs'
description: Dit is een zelfstudie voor ASP.NET MVC om een MVC-webtoepassing met Azure Cosmos DB te maken. JSON opslaan en gegevens benaderen via een takenlijst-app die wordt gehost op Azure Websites - Stapsgewijze zelfstudie voor ASP NET MVC.
keywords: asp.net mvc-zelfstudie, ontwikkelen van webtoepassingen, mvc-webtoepassing, asp net mvc zelfstudie stapsgewijs
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 52532d89-a40e-4fdf-9b38-aadb3a4cccbc
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/03/2017
ms.author: mimig
ms.custom: devcenter
ms.openlocfilehash: a403af0f31823f89cdc79d6769dff61aeaefc4ad
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="_Toc395809351"></a>ASP.NET MVC-zelfstudie: webtoepassingsontwikkeling met Azure Cosmos DB
> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Java](sql-api-java-application.md)
> * [Python](sql-api-python-application.md)
> 
> 

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Dit artikel biedt een end-to-end-overzicht waarin wordt getoond hoe u met Azure Cosmos DB een to-do-app kunt maken. Zo ziet u hoe u effectief kunt gebruikmaken van Azure Cosmos DB voor het opslaan van en uitvoeren van query's voor JSON-documenten. De taken worden opgeslagen als JSON-documenten in Azure Cosmos DB.

![Schermopname van de takenlijst MVC-webtoepassing die is gemaakt met deze zelfstudie - Stapsgewijze zelfstudie voor ASP NET MVC.](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-image01.png)

Deze procedure ziet u hoe de Azure DB die Cosmos-service gebruiken voor het opslaan van en toegang tot gegevens uit een ASP.NET MVC-webtoepassing gehost op Azure. Zie [Een Azure Cosmos DB C#-consoletoepassing bouwen](sql-api-get-started.md) als u een zelfstudie zoekt die volledig is gericht op Azure Cosmos DB en niet op de ASP.NET MVC-onderdelen.

> [!TIP]
> Voor deze zelfstudie wordt ervan uitgegaan dat u ervaring hebt met ASP.NET MVC en Azure Websites. Als u niet bekend met ASP.NET of de [vereiste hulpprogramma's](#_Toc395637760) bent, is het raadzaam het volledige voorbeeldproject via [GitHub][GitHub] te downloaden en de instructies in dit voorbeeld te volgen. Zodra u klaar bent, kunt u dit artikel lezen voor meer informatie over de code in de context van het project.
> 
> 

## <a name="_Toc395637760"></a>Vereisten voor deze databasezelfstudie
Voordat u de instructies in dit artikel uitvoert, moet u beschikken over het volgende:

* Een actief Azure-account.  Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]  
* Microsoft Azure SDK voor .NET voor Visual Studio 2017 beschikbaar via de Visual Studio Installer.

Alle schermopnamen in dit artikel zijn gemaakt met behulp van Microsoft Visual Studio Community 2017. Als uw systeem is geconfigureerd met een andere versie is het mogelijk dat de schermen en opties niet volledig overeenkomen, maar als u voldoet aan de bovenstaande vereisten moet deze oplossing werken.

## <a name="_Toc395637761"></a>Stap 1: een Azure Cosmos DB-databaseaccount maken
Begin met het maken van een Azure Cosmos DB-account. Als u al een SQL-account voor Azure Cosmos DB of als u de Azure-Emulator Cosmos-database voor deze zelfstudie gebruikt, kunt u doorgaan met [Maak een nieuwe ASP.NET MVC-toepassing](#_Toc395637762).

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

<br/>
U kunt nu zien hoe u een compleet nieuwe ASP.NET MVC-toepassing maakt. 

## <a name="_Toc395637762"></a>Stap 2: Een nieuwe ASP.NET MVC-toepassing maken

1. Wijs in het menu **Bestand** van Visual Studio de optie **Nieuw** aan en klik vervolgens op **Project**. Het dialoogvenster **Nieuw project** wordt weergegeven.

2. Vouw in het deelvenster **Projecttypen** achtereenvolgens **Sjablonen**, **Visual C#** en **Web** uit en selecteer vervolgens**ASP.NET-webtoepassing**.

      ![Schermopname van het dialoogvenster met daarin het projecttype ASP.NET-webtoepassing gemarkeerd](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-new-project-dialog.png)

3. Typ in het vak **Naam** de naam van het project. In deze zelfstudie wordt de naam 'todo' gebruikt. Als u een andere naam gebruikt, moet u waar in deze zelfstudie over de naamruimte todo wordt gesproken, de codevoorbeelden aanpassen met de naam die u voor uw toepassing gebruikt. 
4. Klik op **Bladeren** om naar de map te navigeren waarin u het project wilt maken en klik vervolgens op **OK**.
   
      De **nieuwe ASP.NET-webtoepassing** dialoogvenster wordt weergegeven.
   
    ![Schermopname van het dialoogvenster Nieuw ASP.NET-webtoepassing met de MVC-toepassingssjabloon gemarkeerd](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-MVC.png)
5. Selecteer in het deelvenster met sjablonen **MVC**.

6. Klik op **OK** om de scaffolding van de lege ASP.NET MVC-sjabloon aan Visual Studio over te laten. 

          
7. Zodra de Visual Studio de standaard MVC-toepassing heeft gemaakt, beschikt u over een lege ASP.NET-toepassing die u lokaal kunt uitvoeren.
   
    We zullen het project niet lokaal uitvoeren, aangezien iedereen waarschijnlijk wel bekend is met de ASP.NET-toepassing Hello World. U gaat meteen Azure Cosmos DB aan dit project toevoegen en uw toepassing bouwen.

## <a name="_Toc395637767"></a>Stap 3: Azure Cosmos DB aan uw project met de MVC-webtoepassing toevoegen
Nu de meeste ASP.NET MVC-werkzaamheden voor deze oplossing zijn voltooid, kunt u zich richten op het werkelijke doel van deze zelfstudie, namelijk het toevoegen van Azure Cosmos DB aan de MVC-webtoepassing.

1. De Azure Cosmos DB .NET SDK wordt verpakt en gedistribueerd als een NuGet-pakket. Als u het NuGet-pakket aan Visual Studio wilt toevoegen, gebruikt u NuGet-pakketbeheer in Visual Studio door in **Solution Explorer** met de rechtermuisknop op het project te klikken en vervolgens op **NuGet-pakketten beheren** te klikken.
   
    ![Schermopname van de opties voor klikken met de rechtermuisknop voor het webtoepassingsproject in Solution Explorer, met NuGet-pakketten beheren gemarkeerd.](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-manage-nuget.png)
   
    Het dialoogvenster **NuGet-pakketten beheren** wordt weergegeven.
2. Typ in het NuGet-vak **Bladeren** ***Azure DocumentDB***. (Naam van het pakket niet is bijgewerkt naar Azure Cosmos DB.)
   
    Installeren van de resultaten de **Microsoft.Azure.DocumentDB door Microsoft** pakket. Dit wordt download en installeer het pakket Azure Cosmos DB, evenals alle afhankelijkheden, zoals Newtonsoft.Json. Klik op **OK** in het venster **Voorbeeld** en op **I Accept** (Ik ga akkoord) in het venster **License Acceptance** (Licentie accepteren) om de installatie te voltooien.
   
    ![Schermopname van het venster NuGet-pakketten beheren met de Microsoft Azure Cosmos DB clientbibliotheek gemarkeerd](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-install-nuget.png)
   
      U kunt eventueel ook de console voor Pakketbeheer gebruiken om het pakket te installeren. Hiervoor klikt u in het menu **Extra** op **NuGet Package Manager** (NuGet-pakketbeheer) en vervolgens op **Package Manager Console** (Pakketbeheer-console). Typ achter de prompt het volgende.
   
        Install-Package Microsoft.Azure.DocumentDB
        
3. Nadat het pakket is geïnstalleerd, moet uw Visual Studio-oplossing er ongeveer als volgt uitzien, met de twee nieuwe verwijzingen Microsoft.Azure.Documents.Client en Newtonsoft.Json.
   
    ![Schermopname van de twee verwijzingen die zijn toegevoegd aan het JSON-gegevensproject in Solution Explorer](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-added-references.png)

## <a name="_Toc395637763"></a>Stap 4: De ASP.NET MVC-toepassing instellen
U kunt nu de modellen, weergaven en controllers toevoegen aan deze MVC-toepassing:

* [Een model toevoegen](#_Toc395637764).
* [Een controller toevoegen](#_Toc395637765).
* [Weergaven toevoegen](#_Toc395637766).

### <a name="_Toc395637764"></a>Een JSON-gegevensmodel toevoegen
Als eerste wordt de **M** in MVC gemaakt, het model. 

1. Klik in **Solution Explorer** met de rechtermuisknop op de map **Modellen** en klik achtereenvolgens op **Toevoegen** en **Klasse**.
   
      Het dialoogvenster **Nieuw item toevoegen** wordt weergegeven.
2. Geef een naam op voor uw nieuwe klasse **Item.cs** en klik op **Toevoegen**. 
3. Voeg in het nieuwe bestand **Item.cs** achter de laatste *gebruiksinstructie* het volgende toe.
   
        using Newtonsoft.Json;
4. Vervang deze code nu 
   
        public class Item
        {
        }
   
    door de volgende code.
   
        public class Item
        {
            [JsonProperty(PropertyName = "id")]
            public string Id { get; set; }
   
            [JsonProperty(PropertyName = "name")]
            public string Name { get; set; }
   
            [JsonProperty(PropertyName = "description")]
            public string Description { get; set; }
   
            [JsonProperty(PropertyName = "isComplete")]
            public bool Completed { get; set; }
        }
   
    Alle gegevens in Azure Cosmos DB worden doorgegeven via de kabel en opgeslagen als JSON. U kunt het kenmerk **JsonProperty** gebruiken, zoals wordt beschreven in de klasse **Item** die we zojuist hebben gemaakt, om te bepalen hoe uw objecten door JSON.NET worden geserialiseerd/gedeserialiseerd. U **hoeft** dit niet te doen, maar ik wil ervoor zorgen dat mijn eigenschappen de naamgevingsconventie van JSON camelCase volgen. 
   
    U kunt niet alleen de indeling van de eigenschapsnaam voor JSON bepalen, maar ook de naam van uw .NET-eigenschappen volledig wijzigen, zoals ik deed met de **Description**. 

### <a name="_Toc395637765"></a>Een controller toevoegen
Nu we de **M** hebben gehad, kunnen we de **C** in MVC, een controllerklasse, maken.

1. Klik in **Solution Explorer** met de rechtermuisknop op de map **Controllers** en klik achtereenvolgens op **Toevoegen** en **Controller**.
   
    Het dialoogvenster **Add Scaffold** (Scaffold toevoegen) wordt weergegeven.
2. Selecteer **MVC 5 Controller - Empty** (MVC 5-controller - Leeg) en klik vervolgens op **Toevoegen**.
   
    ![Schermopname van het dialoogvenster Add Scaffold met de optie MVC 5 Controller - Empty gemarkeerd](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-controller-add-scaffold.png)
3. Noem uw nieuwe controller **ItemController**.
   
    ![Schermopname van het dialoogvenster Add Controller (Controller toevoegen)](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-add-controller.png)
   
    Zodra het bestand is gemaakt, wordt het nieuwe bestand ItemController.cs in **Solution Explorer** weergegeven en ziet uw Visual Studio-oplossing er ongeveer als volgt uit. Het nieuwe bestand Item.cs file, dat eerder is gemaakt, wordt ook weergegeven.
   
    ![Schermopname van de Visual Studio-oplossing - Solution Explorer met het nieuwe bestand ItemController.cs gemarkeerd](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-new-item-solution-explorer.png)
   
    U kunt ItemController.cs sluiten. Hier komen we later op terug. 

### <a name="_Toc395637766"></a>Weergaven toevoegen
Laten we nu de **V**, de weergaven, in MVC maken:

* [Een weergave toevoegen voor een itemindex ](#AddItemIndexView).
* [Een weergave toevoegen voor nieuwe items](#AddNewIndexView).
* [Een weergave toevoegen voor het bewerken van items](#_Toc395888515).

#### <a name="AddItemIndexView"></a>Een weergave toevoegen voor een itemindex
1. Vouw in **Solution Explorer** de map **Weergaven** uit en klik met de rechtermuisknop op de lege map **Item** die Visual Studio voor u heeft gemaakt toen u **ItemController** hebt toegevoegd. Klik vervolgens op **Toevoegen** en **Weergave**.
   
    ![Schermopname van Solution Explorer waarin de map Item wordt weergegeven die Visual Studio heeft gemaakt en waarin de opdrachten voor het toevoegen van een weergave zijn gemarkeerd](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-add-view.png)
2. Voer in het dialoogvenster **Weergave toevoegen** de volgende handelingen uit:
   
   * In het vak **Weergavenaam** typt u ***Index***.
   * Selecteer in het vak **Sjabloon** de optie ***Lijst***.
   * Selecteer in het vak **Modelklasse** de optie ***Item (todo.Models)***.
   * Typ in het veld voor de indelingspagina ***~/Views/Shared/_Layout.cshtml***.
     
   ![Schermopname van het dialoogvenster Weergave toevoegen](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-add-view-dialog.png)
3. Zodra al deze waarden zijn ingesteld, klikt u op **Toevoegen** en wordt er een nieuwe sjabloonweergave in Visual Studio gemaakt. Vervolgens wordt het cshtml-bestand geopend dat is gemaakt. Dit bestand in Visual Studio kan voorlopig worden gesloten, aangezien dit pas later aan bod komt.

#### <a name="AddNewIndexView"></a>Een weergave toevoegen voor nieuwe items
We kunnen op ongeveer dezelfde manier als voor de weergave **Itemindex** nu een nieuwe weergave voor het maken van nieuwe **Items** maken.

1. Klik in **Solution Explorer** met de rechtermuisknop nogmaals op de map **Item** en klik achtereenvolgens op **Toevoegen** en **Weergave**.
2. Voer in het dialoogvenster **Weergave toevoegen** de volgende handelingen uit:
   
   * Typ in het vak **Weergavenaam** ***Maken***.
   * Selecteer in het vak **Sjabloon** de optie ***Maken***.
   * Selecteer in het vak **Modelklasse** de optie ***Item (todo.Models)***.
   * Typ in het veld voor de indelingspagina ***~/Views/Shared/_Layout.cshtml***.
   * Klik op **Add**.
   
#### <a name="_Toc395888515"></a>Een weergave toevoegen voor het bewerken van items
Tot slot voegt u op dezelfde manier als hiervoor een weergave toe waarin u **items** kunt bewerken.

1. Klik in **Solution Explorer** met de rechtermuisknop nogmaals op de map **Item** en klik achtereenvolgens op **Toevoegen** en **Weergave**.
2. Voer in het dialoogvenster **Weergave toevoegen** de volgende handelingen uit:
   
   * Typ in het vak **Weergavenaam** ***Bewerken***.
   * Selecteer in het vak **Sjabloon** de optie ***Bewerken***.
   * Selecteer in het vak **Modelklasse** de optie ***Item (todo.Models)***.
   * Typ in het veld voor de indelingspagina ***~/Views/Shared/_Layout.cshtml***.
   * Klik op **Add**.

Zodra dit is gebeurd, sluit u alle cshtml-documenten in Visual Studio. We komen later op deze weergaven terug.

## <a name="_Toc395637769"></a>Stap 5: Azure Cosmos DB fysiek aansluiten
Nu de standaardwerkzaamheden voor MVC zijn voltooid, kunt u de code voor Azure Cosmos DB toevoegen. 

In deze sectie voegen we code toe voor de verwerking van het volgende:

* [Vermelden van onvolledige items](#_Toc395637770).
* [Items toevoegen](#_Toc395637771).
* [Items bewerken](#_Toc395637772).

### <a name="_Toc395637770"></a>Onvolledige objecten in uw MVC-webtoepassing vermelden
Allereerst moet u een klasse toevoegen die de logica bevat voor de verbinding met en het gebruik van Azure Cosmos DB. Voor deze zelfstudie voegen we alle logica toe aan een opslagplaatsklasse met de naam DocumentDBRepository. 

1. Klik in **Solution Explorer** met de rechtermuisknop op het project, klik op **Toevoegen** en klik vervolgens op **Klasse**. Geef een naam voor de nieuwe klasse **DocumentDBRepository** op en klik op **Toevoegen**.
2. Voeg de volgende *gebruiksinstructies* boven de *naamruimtedeclaratie* toe in de klasse **DocumentDBRepository** die we zojuist hebben gemaakt.
   
        using Microsoft.Azure.Documents; 
        using Microsoft.Azure.Documents.Client; 
        using Microsoft.Azure.Documents.Linq; 
        using System.Configuration;
        using System.Linq.Expressions;
        using System.Threading.Tasks;
        using System.Net
        
    Vervang deze code nu 
   
        public class DocumentDBRepository
        {
        }
   
    door de volgende code.
   
        public static class DocumentDBRepository<T> where T : class
        {
            private static readonly string DatabaseId = ConfigurationManager.AppSettings["database"];
            private static readonly string CollectionId = ConfigurationManager.AppSettings["collection"];
            private static DocumentClient client;
   
            public static void Initialize()
            {
                client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);
                CreateDatabaseIfNotExistsAsync().Wait();
                CreateCollectionIfNotExistsAsync().Wait();
            }
   
            private static async Task CreateDatabaseIfNotExistsAsync()
            {
                try
                {
                    await client.ReadDatabaseAsync(UriFactory.CreateDatabaseUri(DatabaseId));
                }
                catch (DocumentClientException e)
                {
                    if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
                    {
                        await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
                    }
                    else
                    {
                        throw;
                    }
                }
            }
   
            private static async Task CreateCollectionIfNotExistsAsync()
            {
                try
                {
                    await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId));
                }
                catch (DocumentClientException e)
                {
                    if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
                    {
                        await client.CreateDocumentCollectionAsync(
                            UriFactory.CreateDatabaseUri(DatabaseId),
                            new DocumentCollection { Id = CollectionId },
                            new RequestOptions { OfferThroughput = 1000 });
                    }
                    else
                    {
                        throw;
                    }
                }
            }
        }
   
    
3. Aangezien er enkele waarden uit de configuratie worden gelezen, opent u het bestand **Web.config** van de toepassing en voegt u de volgende regels onder de sectie `<AppSettings>` toe.
   
        <add key="endpoint" value="enter the URI from the Keys blade of the Azure Portal"/>
        <add key="authKey" value="enter the PRIMARY KEY, or the SECONDARY KEY, from the Keys blade of the Azure  Portal"/>
        <add key="database" value="ToDoList"/>
        <add key="collection" value="Items"/>
4. Werk nu de waarden voor *endpoint* en *authKey* bij door gebruik te maken van de blade Sleutels van Azure Portal. Gebruik de **URI** op de blade Sleutels als waarde voor de endpoint-instelling en gebruik de **PRIMAIRE SLEUTEL** of **SECUNDAIRE SLEUTEL** op de blade Sleutels als waarde voor authKey-instelling.

    Dat zorgt voor de bekabeling van de opslagplaats Azure Cosmos DB nu gaan we toepassingslogica toevoegen.

1. Op de eerste plaats willen we natuurlijk de onvolledige items kunnen weergeven met een takenlijsttoepassing.  Kopieer het volgende codefragment en plak dit ergens in de klasse **DocumentDBRepository**.
   
        public static async Task<IEnumerable<T>> GetItemsAsync(Expression<Func<T, bool>> predicate)
        {
            IDocumentQuery<T> query = client.CreateDocumentQuery<T>(
                UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId))
                .Where(predicate)
                .AsDocumentQuery();
   
            List<T> results = new List<T>();
            while (query.HasMoreResults)
            {
                results.AddRange(await query.ExecuteNextAsync<T>());
            }
   
            return results;
        }
2. Open de **ItemController** die eerder is toegevoegd en voeg de volgende *Using-instructies* toe boven de naamruimtedeclaratie.
   
        using System.Net;
        using System.Threading.Tasks;
        using todo.Models;
   
    Als u het project een andere naam dan 'todo' hebt gegeven, moet u 'todo.Models' bijwerken om hier de naam van uw project weer te geven.
   
    Vervang deze code nu
   
        //GET: Item
        public ActionResult Index()
        {
            return View();
        }
   
    door de volgende code.
   
        [ActionName("Index")]
        public async Task<ActionResult> IndexAsync()
        {
            var items = await DocumentDBRepository<Item>.GetItemsAsync(d => !d.Completed);
            return View(items);
        }
3. Open **Global.asax.cs** en voeg de volgende regel aan de methode **Application_Start** toe. 
   
        DocumentDBRepository<todo.Models.Item>.Initialize();

Op dit moment moet uw oplossing opbouwbewerking kunnen uitvoeren zonder dat er fouten optreden.

Als u de toepassing nu hebt uitgevoerd, gaat u naar de weergaven **HomeController** en **Index** van die controller. Dit is het standaardgedrag voor het MVC-sjabloonproject dat we aan het begin hebben gekozen, maar dit is niet het gewenste gedrag. U kunt de routering op deze MVC-toepassing wijzigen om dit gedrag te veranderen.

Open ***App\_Start\RouteConfig.cs*** en zoek de regel die begint met "defaults:" en pas deze als volgt aan.

        defaults: new { controller = "Item", action = "Index", id = UrlParameter.Optional }

Hiermee stelt u in dat ASP.NET MVC niet **Home** maar **Item** als controller gebruikt en **Index** als weergave gebruikt als u in de URL geen waarde hebt opgegeven voor het routeringsgedrag.

Als u de toepassing nu uitvoert, wordt uw **ItemController** aangeroepen, die vervolgens de opslagplaatsklasse aanroept en de methode GetItems gebruikt om alle onvolledige items naar de weergave **Weergaven**\\**Item**\\**Index** te retourneren. 

Als u dit project nu maakt en uitvoert, ziet u iets dat vergelijkbaar is met het volgende.    

![Schermopname van de takenlijstwebtoepassing die is gemaakt met deze databasezelfstudie](./media/sql-api-dotnet-application/build-and-run-the-project-now.png)

### <a name="_Toc395637771"></a>Items toevoegen
Laten we enkele items toevoegen aan de database zodat we niet tegen een leeg raster aankijken.

U kunt nu code toevoegen aan Azure Cosmos DBRepository en ItemController om de record in Azure Cosmos DB te houden.

1. Voeg de volgende methode toe aan uw klasse **DocumentDBRepository**.
   
       public static async Task<Document> CreateItemAsync(T item)
       {
           return await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId), item);
       }
   
   Deze methode wordt een object dat is doorgegeven aan deze gewoon en deze in Azure Cosmos DB zich blijft voordoen.
2. Open het bestand ItemController.cs en voeg het volgende codefragment toe binnen de klasse. Zodoende weet ASP.NET MVC wat er voor de actie **Create** moet worden gedaan. In dit geval geeft u de bijbehorende weergave Create.cshtml weer die eerder is gemaakt.
   
        [ActionName("Create")]
        public async Task<ActionResult> CreateAsync()
        {
            return View();
        }
   
    Er is nu meer code in deze controller nodig die de inzending vanuit de weergave **Create** accepteert.
3. Voeg het volgende codeblok toe aan de klasse ItemController.cs waarmee ASP.NET MVC wordt geïnstrueerd wat er moet gebeuren met een form POST voor deze controller.
   
        [HttpPost]
        [ActionName("Create")]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> CreateAsync([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
            if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.CreateItemAsync(item);
                return RedirectToAction("Index");
            }
   
            return View(item);
        }
   
    Deze code roept de DocumentDBRepository aan en gebruikt de methode CreateItemAsync om het nieuwe takenlijstitem door te geven aan de database. 
   
    **Opmerking over de beveiliging**: het kenmerk **ValidateAntiForgeryToken** wordt hier gebruikt om deze toepassing te beschermen tegen aanvallen via aanvraagvervalsing op meerdere sites. Het volstaat echter niet om dit kenmerk alleen toe te voegen. Uw weergaven moeten samenwerken met dit anti-vervalsingstoken. Zie [Voorkomen van aanvraagvervalsing op meerdere sites][Preventing Cross-Site Request Forgery] voor meer informatie over dit onderwerp en voorbeelden van een juiste implementatie. De broncode op [GitHub][GitHub] beschikt over de volledige implementatie.
   
    **Opmerking over de beveiliging**: we gebruiken ook het kenmerk **Bind** voor de methodeparameter om u te beschermen tegen over-postingaanvallen. Zie [Eenvoudige CRUD-bewerkingen in ASP.NET MVC][Basic CRUD Operations in ASP.NET MVC] voor meer informatie.

Hiermee is de benodigde code toegevoegd om nieuwe items aan de database toe te voegen.

### <a name="_Toc395637772"></a>Items bewerken
Tot slot moeten we ervoor zorgen dat we **Items** in de database kunnen bewerken en dat we ze kunnen markeren als voltooid. De weergave voor het bewerken van items was al toegevoegd aan het project. U hoeft daarom alleen code toe te voegen aan de controller en de klasse **DocumentDBRepository**.

1. Voeg het volgende toe aan de klasse **DocumentDBRepository**.
   
        public static async Task<Document> UpdateItemAsync(string id, T item)
        {
            return await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(DatabaseId, CollectionId, id), item);
        }
   
        public static async Task<T> GetItemAsync(string id)
        {
            try
            {
                Document document = await client.ReadDocumentAsync(UriFactory.CreateDocumentUri(DatabaseId, CollectionId, id));
                return (T)(dynamic)document;
            }
            catch (DocumentClientException e)
            {
                if (e.StatusCode == HttpStatusCode.NotFound)
                {
                    return null;
                }
                else
                {
                    throw;
                }
            }
        }
   
    Met de eerste methode, **GetItem**, wordt een item uit Azure Cosmos DB opgehaald dat wordt doorgegeven aan de **ItemController** en vervolgens aan de weergave **Bewerken**.
   
    Met de tweede methode die zojuist is toegevoegd, wordt het **document** in Azure Cosmos DB vervangen door de versie van het **document** dat is doorgegeven via de **ItemController**.
2. Voeg het volgende toe aan de klasse **ItemController**.
   
        [HttpPost]
        [ActionName("Edit")]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> EditAsync([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
            if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.UpdateItemAsync(item.Id, item);
                return RedirectToAction("Index");
            }
   
            return View(item);
        }
   
        [ActionName("Edit")]
        public async Task<ActionResult> EditAsync(string id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
   
            Item item = await DocumentDBRepository<Item>.GetItemAsync(id);
            if (item == null)
            {
                return HttpNotFound();
            }
   
            return View(item);
        }
   
    De eerste methode verwerkt de Http GET die plaatsvindt wanneer de gebruiker klikt op de koppeling **Bewerken** in de weergave **Index**. Met deze methode wordt er een [**document**](http://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) uit Azure Cosmos DB opgehaald en doorgegeven aan de weergave **Bewerken**.
   
    In de weergave **Bewerken** wordt vervolgens een Http POST naar de **IndexController** uitgevoerd. 
   
    Voor de tweede methode zijn er ingangen toegevoegd om het bijgewerkte object door te geven aan Azure Cosmos DB, zodat het object wordt bewaard in de database.

Meer hoeft u niet te doen om uw toepassing uit te voeren, onvolledige **Items** weer te geven, nieuwe **Items** toe te voegen en **Items** te bewerken.

## <a name="_Toc395637773"></a>Stap 6: De toepassing lokaal uitvoeren
Ga als volgt te werk als u de toepassing wilt testen op een lokale machine:

1. Druk in Visual Studio op F5 om de toepassing in de foutopsporingsmodus op te bouwen. De toepassing wordt opgebouwd en wordt er een browser gestart met het lege rasterpagina dat we eerder hebben gezien:
   
    ![Schermopname van de takenlijstwebtoepassing die is gemaakt met deze databasezelfstudie](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-an-item-a.png)
   
     
2. Klik op de koppeling **Nieuw maken** en voeg waarden toe aan de velden **Naam** en **Beschrijving**. Schakel het selectievakje **Voltooid** niet in, anders wordt het nieuwe **Item** toegevoegd met een onvoltooide status en wordt het niet weergegeven in de aanvankelijke lijst.
   
    ![Schermopname van de weergave Maken](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-new-item.png)
3. Als u op **Maken** klikt, keert u terug naar de weergaven **Index** en wordt uw **Item** weergegeven in de lijst.
   
    ![Schermopname van de weergave Index](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-an-item.png)
   
    U kunt gerust nog enkele **Items** aan uw takenlijst toevoegen.
    
4. Klik op **Bewerken** naast een **Item** in de lijst, zodat u wordt omgeleid naar de weergave **Bewerken**. Hier kunt u de eigenschappen van uw object bijwerken, inclusief de vlag **Voltooid**. Als u de vlag **Voltooid** markeert en op **Opslaan** klikt, wordt het **Item** verwijderd uit de lijst met onvolledige taken.
   
    ![Schermopname van de weergave Index met het selectievakje Voltooid ingeschakeld](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-completed-item.png)
5. Zodra u de app hebt getest, drukt u op Ctrl + F5 om de foutopsporing voor de app te stoppen. U kunt de app nu implementeren.

## <a name="_Toc395637774"></a>Stap 7: De toepassing in Azure App Service implementeren 
Nu dat u hebt de volledige toepassing correct werkt met Azure Cosmos DB gaan we deze web-app implementeren in Azure App Service.  

1. Als u deze toepassing wilt publiceren, hoeft u alleen maar met de rechtermuisknop op het project in **Solution Explorer** te klikken en vervolgens op **Publiceren** te klikken.
   
    ![Schermopname van de optie Publiceren in Solution Explorer](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-publish.png)

2. In de **publiceren** in het dialoogvenster klikt u op **Microsoft Azure App Service**, selecteer daarna **nieuw** een App Service-profiel maken of klik op **bestaande selecteren**  een bestaand profiel gebruiken.

    ![Het dialoogvenster Publish in Visual Studio](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-publish-to-existing.png)

3. Als u een bestaand Azure App Service-profiel, voert u de abonnementsnaam van uw. Gebruik de **weergave** filteren om te sorteren op resourcegroep of brontype en selecteer vervolgens uw Azure App Service. 
   
    ![In het dialoogvenster App Service in Visual Studio](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-app-service.png)

4. Klik op om een nieuw Azure App Service-profiel **nieuw** in de **publiceren** in het dialoogvenster. In de **Create App Service** dialoogvenster, Voer uw Web-App-naam en de juiste abonnement, de resourcegroep en de App Service-abonnement en klik vervolgens op **maken**.

    ![Het dialoogvenster App Service maken in Visual Studio](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-app-service.png)

Binnen een paar seconden zal Visual Studio publicatie van uw webtoepassing voltooien en een browser starten waarin u kunt zien uw werk in Azure wordt uitgevoerd!



## <a name="_Toc395637775"></a>Volgende stappen
Gefeliciteerd. U zojuist hebt gemaakt van uw eerste ASP.NET MVC-webtoepassing met Azure Cosmos DB en gepubliceerd naar Azure. De broncode voor de volledige toepassing, met inbegrip van de functionaliteit voor details en verwijderen die niet is opgenomen in deze zelfstudie, kan worden gedownload of gekloond via [GitHub][GitHub]. Als dit wilt toevoegen aan uw app, kunt u de code ophalen en toevoegen aan deze app.

Om extra functionaliteit toe te voegen aan uw toepassing, bekijkt u de API's beschikbaar zijn in de [Azure Cosmos DB .NET-bibliotheek](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) harte welkom om bij te dragen naar de Azure Cosmos DB .NET-bibliotheek op [GitHub] [GitHub]. 

[\*]: https://microsoft.sharepoint.com/teams/DocDB/Shared%20Documents/Documentation/Docs.LatestVersions/PicExportError
[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: http://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: http://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/documentdb-net-todo-app
