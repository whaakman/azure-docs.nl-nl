---
title: Een HTTP-trigger maken met een Azure Cosmos DB-invoerbinding | Microsoft Docs
description: Informatie over het gebruik van Azure Functions met HTTP-triggers voor het uitvoeren van query’s op Azure Cosmos DB.
services: cosmos-db
documentationcenter: ''
author: SnehaGunda
manager: kfile
ms.assetid: ''
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 85a9e66491513b016380913617d8e78cf5d82f6d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="create-an-azure-functions-http-trigger-with-an-azure-cosmos-db-input-binding"></a>Een Azure Functions HTTP-trigger maken met een Azure Cosmos DB-invoerbinding

Azure Cosmos DB is een globaal gedistribueerde, multi-model database die zowel schemaloos als serverloos is. Azure Function is een serverloze rekenservice waarmee u code op aanvraag uit kunt voeren. Koppel deze twee Azure-services en u hebt de basis voor een serverloze architectuur waarmee u zich kunt richten op het bouwen van geweldige apps en u geen zorgen hoeft te maken over het inrichten en onderhouden van servers voor uw reken- en databasebehoeften.

Deze zelfstudie bouwt voort op de code die is gemaakt in de [Graph-API-snelstart voor .NET](create-graph-dotnet.md). In deze zelfstudie wordt een Azure Function toegevoegd die een [HTTP-trigger](https://github.com/MicrosoftDocs/azure-docs-pr/azure-functions/functions-bindings-http-webhook.md#http-trigger) bevat. De HTTP-trigger gebruikt een Cosmos Azure DB- [invoerbinding](https://github.com/MicrosoftDocs/azure-docs-pr/azure-functions/functions-triggers-bindings.md) om gegevens op te halen uit de graafdatabase die u in de snelstartgids hebt gemaakt. Deze specifieke HTTP-trigger vraagt gegevens op uit Azure Cosmos DB, maar invoerbindingen van Azure Cosmos DB kunnen worden gebruikt voor het ophalen van gegevensinvoerwaarden voor wat uw functie maar nodig heeft.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een Azure Function-project maken 
> * Een HTTP-trigger maken
> * De Azure Function publiceren
> * De Azure Function verbinden met de Azure DB Cosmos-database

## <a name="prerequisites"></a>Vereisten

- [Visual Studio 2017 versie 15.3](https://www.visualstudio.com/vs/preview/), waaronder de **Azure-ontwikkelworkload**.

    ![Visual Studio 2017 installeren met de Azure-ontwikkelworkload](./media/tutorial-functions-http-trigger/functions-vs-workloads.png)
    
- Na het installeren van of upgraden naar Visual Studio 2017 versie 15.3, moet u handmatig de hulpprogramma’s van Visual Studio 2017 voor Azure Functions bijwerken. U kunt de hulpprogramma’s bijwerken vanuit het menu **Extra** onder **Extensie en updates...** > **Updates** > **Visual Studio Marketplace** > **Azure Functions en WebJobs-hulpprogramma’s** > **Bijwerken**.

- Voltooi de zelfstudie [Build a .NET application using the Graph API](tutorial-develop-graph-dotnet.md) (Een .NET-toepassing bouwen met behulp van de Graph-API) of download de voorbeeldcode uit de GitHub-repo [azure-cosmos-db-graph-dotnet-getting-started](https://github.com/Azure-Samples/azure-cosmos-db-graph-dotnet-getting-started) en bouw het project.
 
## <a name="build-a-function-in-visual-studio"></a>Een functie bouwen in Visual Studio

1. Voet een **Azure Functions**-project aan uw oplossing toe door in **Solution Explorer** met de rechtermuisknop op het knooppunt van de oplossing te klikken en **Toevoegen** >  **Nieuw Project** te kiezen. Kies **Azure Functions** in het dialoogvenster en noem het project **PeopleDataFunctions**.

   ![Een Azure Function-project toevoegen aan de oplossing](./media/tutorial-functions-http-trigger/01-add-function-project.png)

2. Nadat u het Azure Functions-project hebt gemaakt, moet u enkele NuGet-gerelateerde updates en installaties uitvoeren. 

    a. Zorg ervoor dat u de nieuwste Functions-SDK hebt door NuGet Manager te gebruiken om het **Microsoft.NET.Sdk.Functions**-pakket bij te werken. Klik in **Solution Explorer** met de rechtermuisknop op het project en klik op **NuGet-pakketten beheren**. Selecteer op het tabblad **Geïnstalleerd** Microsoft.NET.Sdk.Functions en klik op **Bijwerken**.

   ![NuGet-pakketten bijwerken](./media/tutorial-functions-http-trigger/02-update-functions-sdk.png)

    b. Voer **azure.graphs** in op het tabblad **Bladeren** om het pakket **Microsoft.Azure.Graphs** te zoeken en klik op **Installeren**. Dit pakket bevat de .NET-client-SDK van de Graph-API.

   ![De Graph-API installeren](./media/tutorial-functions-http-trigger/03-add-azure-graphs.png)

    c. Voer **mono.csharp** in op het tabblad **Bladeren** om het pakket **Mono.CSharp** te zoeken en klik op **Installeren**.

   ![Mono.CSharp installeren](./media/tutorial-functions-http-trigger/04-add-mono.png)

3. Uw Solution Explorer zou nu de pakketten moeten bevatten die u hebt geïnstalleerd, zoals hier wordt weergegeven. 
   
   Vervolgens moeten we wat code schrijven, dus gaan we een nieuw **Azure Function**-item toevoegen aan het project. 

    a. Klik op de rechtermuisknop op het projectknooppunt in **Solution Explorer** en kies  > **Nieuw item****Toevoegen**.   
    b. Selecteer **Visual C#-items** in het dialoogvenster **Nieuw item toevoegen**, selecteer **Azure Function**, typ **Search** als de naam voor uw project en klik op **Toevoegen**.  
 
   ![Een nieuwe functie met de naam Zoeken maken](./media/tutorial-functions-http-trigger/05-add-function.png)

4. De Azure Function reageert op HTTP-aanvragen, dus is de HTTP-triggersjabloon hier geschikt.
   
   Selecteer **HTTP-trigger** in het vak **Nieuwe Azure Function**. We willen deze Azure Function ook zo breed mogelijk toegankelijk maken, dus stellen we de **Toegangsrechten** in op **Anoniem**, zodat iedereen wordt toegelaten. Klik op **OK**.

   ![Toegangsrechten instellen op Anoniem](./media/tutorial-functions-http-trigger/06-http-trigger.png)

5. Nadat u Search.cs aan het Azure Function-project hebt toegevoegd, kopieert u deze **using**-instructies over de bestaande using-instructies heen:

   ```csharp
   using Microsoft.Azure.Documents;
   using Microsoft.Azure.Documents.Client;
   using Microsoft.Azure.Documents.Linq;
   using Microsoft.Azure.Graphs;
   using Microsoft.Azure.WebJobs;
   using Microsoft.Azure.WebJobs.Extensions.Http;
   using Microsoft.Azure.WebJobs.Host;
   using System;
   using System.Collections.Generic;
   using System.Configuration;
   using System.Linq;
   using System.Net;
   using System.Net.Http;
   using System.Threading.Tasks;
   ```

6. Vervolgens vervangt u de klassecode van de Azure Function door de onderstaande code. De code doorzoekt de Azure Cosmos DB-database met behulp van de Graph-API op alle personen of op de specifieke persoon die wordt geïdentificeerd door de querytekenreeksparameter `name`.

   ```csharp
   public static class Search
   {
       static string endpoint = ConfigurationManager.AppSettings["Endpoint"];
       static string authKey = ConfigurationManager.AppSettings["AuthKey"];

       [FunctionName("Search")]
       public static async Task<HttpResponseMessage> Run(
           [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req,
           TraceWriter log)
       {
           log.Info("C# HTTP trigger function processed a request.");

           // the person objects are free-form in structure
           List<dynamic> results = new List<dynamic>();

           // open the client's connection
           using (DocumentClient client = new DocumentClient(
               new Uri(endpoint),
               authKey,
               new ConnectionPolicy
               {
                   ConnectionMode = ConnectionMode.Direct,
                   ConnectionProtocol = Protocol.Tcp
               }))
           {
               // get a reference to the database the console app created
               Database database = await client.CreateDatabaseIfNotExistsAsync(
                   new Database
                   {
                       Id = "graphdb"
                   });

               // get an instance of the database's graph
               DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync(
                   UriFactory.CreateDatabaseUri("graphdb"),
                   new DocumentCollection { Id = "graphcollz" },
                   new RequestOptions { OfferThroughput = 1000 });

               // build a gremlin query based on the existence of a name parameter
               string name = req.GetQueryNameValuePairs()
                   .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                   .Value;

               IDocumentQuery<dynamic> query = (!String.IsNullOrEmpty(name))
                   ? client.CreateGremlinQuery<dynamic>(graph, string.Format("g.V('{0}')", name))
                   : client.CreateGremlinQuery<dynamic>(graph, "g.V()");

               // iterate over all the results and add them to the list
               while (query.HasMoreResults)
                   foreach (dynamic result in await query.ExecuteNextAsync())
                       results.Add(result);
           }

           // return the list with an OK response
           return req.CreateResponse<List<dynamic>>(HttpStatusCode.OK, results);
       }
   }
   ```

   De code is in feite dezelfde verbindingslogica als in de oorspronkelijke consoletoepassing waarmee de database aanvankelijk is gevuld, met een eenvoudige query voor het ophalen van de overeenkomende records.

## <a name="debug-the-azure-function-locally"></a>Fouten in de Azure-functie lokaal opsporen

Nu de code voltooid is, kunt u de lokale hulpprogramma’s voor foutopsporing en de emulator van Azure Function gebruiken om de code lokaal uit te voeren en te testen.

1. Voordat u de code correct kan worden uitgevoerd, moet u deze configureren voor lokale uitvoering met uw Azure Cosmos DB-verbindingsgegevens. U kunt het bestand local.settings.json gebruiken voor het configureren van de Azure Function voor lokale uitvoering, op soortgelijke wijze als u het bestand App.config zou gebruiken om de oorspronkelijke consoletoepassing voor uitvoering te configureren.

    U doet dit door de volgende regels code toe te voegen aan local.settings.json en uw Endpoint en AuthKey ernaar te kopiëren uit het bestand App.Config in het project GraphGetStarted, zoals weergegeven in de volgende afbeelding.

   ```json
    "Endpoint": "",
    "AuthKey": ""
    ```

   ![De eindpunt- en autorisatiesleutel instellen in het bestand local.settings.json](./media/tutorial-functions-http-trigger/07-local-functions-settings.png)

2. Wijzig het opstartproject in de nieuwe Functions-app. Klik in **Solution Explorer** met de rechtermuisknop op **PeopleDataFunctions** en selecteer **Instellen als opstartproject**.

3. Klik in **Solution Explorer** met de rechtermuisknop op **Afhankelijkheden** in het project **PeopleDataFunctions** en klik op **Verwijzing toevoegen**. Selecteer de System.Configuration uit de lijst en klik op **OK**.

3. Nu gaan we de app uitvoeren. Druk op F5 om het lokale foutopsporingsprogramma func.exe te starten, met de Azure Function-code gehost en klaar voor gebruik.

   Aan het einde van de eerste uitvoer van func.exe ziet u dat Azure Function wordt gehost op localhost:7071. Dit helpt bij het testen ervan in een client.

   ![De client testen](./media/tutorial-functions-http-trigger/08-functions-emulator.png)

4. Gebruik voor het testen van de Azure Function [Visual Studio Code](http://code.visualstudio.com/) met de extensie van Huachao Mao, [REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client). REST Client biedt de mogelijkheid lokale of externe HTTP-aanvragen uit te voeren met één klik met de rechtermuisknop. 

    U doet dit door een nieuw bestand met de naam test-function-locally.http maken en de volgende code toe te voegen:

    ```http
    get http://localhost:7071/api/Search

    get http://localhost:7071/api/Search?name=ben
   ```

    Klik nu met de rechtermuisknop op de eerste coderegel en selecteer vervolgens **Aanvraag verzenden**, zoals weergegeven in de volgende afbeelding.

   ![Een REST-aanvraag verzenden vanuit Visual Studio-code](./media/tutorial-functions-http-trigger/09-rest-client-in-vs-code.png)

   U krijgt het onbewerkte HTTP-antwoord uit de headers van de lokaal uitgevoerde Azure Functie te zien, met de inhoud van de JSON-hoofdtekst en al.

   ![REST-antwoord](./media/tutorial-functions-http-trigger/10-general-results.png)

5. Selecteer nu de tweede regel code en selecteer **Aanvraag verzenden**. Door de querytekenreeksparameter `name` toe te voegen met een waarde waarvan we weten dat die zich in de database bevindt, kunnen we de resultaten filteren die door de Azure Function worden geretourneerd.

   ![De resultaten van de Azure Function filteren](./media/tutorial-functions-http-trigger/11-search-for-ben.png)

Nadat de Azure Function is gevalideerd en goed lijkt te werken, is de laatste stap om deze te publiceren naar Azure App Service en te configureren voor uitvoering in de cloud.

## <a name="publish-the-azure-function"></a>De Azure-functie publiceren

1. Klik in **Solution Explorer** met de rechtermuisknop op het project en selecteer **Publiceren**.

   ![Het nieuwe project publiceren](./media/tutorial-functions-http-trigger/12-publish-function.png)

2. We zijn klaar om het naar de cloud te publiceren om het te testen in een openbaar beschikbaar scenario. Selecteer **Azure Function-app** op het tabblad **Publiceren**, selecteer **Nieuwe maken** om een Azure Function te maken in uw Azure-abonnement en klik op **Publiceren**.

   ![Een nieuwe Azure Function-app maken](./media/tutorial-functions-http-trigger/13-publish-panel.png)

3. Doe in het dialoogvenster **Publiceren** het volgende:
   
    a. Geef in **App-naam** een unieke naam voor de functie op.

    b. Selecteer in **Abonnement** het te gebruiken Azure-abonnement.
   
    c. Maak in **Resourcegroep** een nieuwe resourcegroep en gebruik dezelfde naam als de naam van de app.
   
    d. Klik voor **App Service-plan** op **Nieuw** om een nieuw App Service Plan op basis van verbruik te maken, omdat we van plan zijn de factureringsmethode voor betaling op basis van gebruik te gebruiken voor de serverloze Azure Function. Gebruik de standaardinstellingen op de pagina **App Service-plan configureren** en klik op **OK**.
   
    e. Klik bij **Opslagaccount** ook op **Nieuw** om een nieuw opslagaccount te maken om met de Azure Function te gebruiken, voor het geval dat we ooit ondersteuning voor blobs, tabellen of wachtrijen nodig hebben om de uitvoering of andere functionaliteit te activeren. Gebruik de standaardinstellingen op de pagina **Opslagaccount** en klik op **OK**.

    f. Klik vervolgens op de knop **Maken** in het dialoogvenster om alle resources in uw Azure-abonnement te maken. Visual Studio downloadt een publicatieprofiel (een eenvoudig XML-bestand) waarvan de volgende keer dat u de code van uw Azure Function publiceert gebruik wordt gemaakt.

   ![Het opslagaccount maken](./media/tutorial-functions-http-trigger/14-new-function-app.png)

    In Visual Studio wordt een pagina Publiceren weergegeven, die u kunt gebruiken als u wijzigingen in de Function aanbrengt en deze opnieuw moet publiceren. U hoeft nu geen actie te ondernemen op die pagina.

4. Nadat de Azure Function is gepubliceerd, kunt u naar de [Azure-portal](https://portal.azure.com/)-pagina voor uw Azure Function gaan. Daar ziet u een koppeling naar de **Toepassingsinstellingen** van de Azure Function. Open deze koppeling om de live Azure Function te configureren voor verbinding met de Azure Cosmos DB-database met uw persoonsgegevens.

   ![Toepassingsinstellingen controleren](./media/tutorial-functions-http-trigger/15-function-in-portal.png)

5. Net als u eerder in de het App.config-bestand van de consoletoepassing en in het bestand local.settings.json van de Azure Function hebt gedaan, moet u het Endpoint en de AuthKey voor de Azure Cosmos DB-database toevoegen aan de gepubliceerde functie. Op deze manier hoeft u nooit configuratiecode in te checken die uw sleutels bevat - u kunt ze configureren in de portal en er zeker van zijn dat ze niet worden opgeslagen in het broncodebeheer. Om elke waarde toe te voegen, klikt u op de knop **Nieuwe instelling toevoegen**, voegt u **Endpoint** toe en uw waarde uit app.config, klikt u vervolgens nogmaals op **Nieuwe instelling** en voegt u  **AuthKey** en uw aangepaste waarde toe. Nadat u de waarden hebt toegevoegd en opgeslagen, zouden uw instellingen er ongeveer als volgt uit moeten zien.

   ![Endpoint en AuthKey configureren](./media/tutorial-functions-http-trigger/16-app-settings.png)

6. Nadat de Azure Function correct is geconfigureerd in uw Azure-abonnement, kunt u de Visual Studio Code REST Client-extensie weer gebruiken om de URL van de openbaar beschikbare Azure Function op te vragen. Voeg deze twee regels code toe aan testfunctie-locally.http en voer vervolgens elke regel uit om deze functie te testen. Vervang de naam van de functie in de URL door de naam van uw functie.

    ```json
    get https://peoplesearchfunction.azurewebsites.net/api/Search

    get https://peoplesearchfunction.azurewebsites.net/api/Search?name=thomas
    ```

    De functie reageert met de gegevens die worden opgehaald uit Azure Cosmos DB.

    ![De REST-client gebruiken om een query uit te voeren op de Azure Function](./media/tutorial-functions-http-trigger/17-calling-function-from-code.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Een Azure Function-project gemaakt 
> * Een HTTP-trigger gemaakt
> * De Azure Function is gepubliceerd
> * De Function verbonden met de Azure DB Cosmos-database

U kunt nu doorgaan naar de sectie Concepten voor meer informatie over Cosmos DB.

> [!div class="nextstepaction"]
> [Wereldwijde distributie](distribute-data-globally.md) 

Dit artikel is gebaseerd op een blog uit [Brady Gaster's Schemaless & Serverless](http://www.bradygaster.com/category/%20Serverless%20&%20Schemaless) blogreeks. Ga naar zijn blog voor meer berichten in de reeks.
