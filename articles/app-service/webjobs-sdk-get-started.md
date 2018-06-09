---
title: Aan de slag met de Azure WebJobs SDK
description: Inleiding tot de WebJobs SDK voor gebeurtenisafhankelijke achtergrondverwerking. Ontdek hoe u toegang tot gegevens in Azure-services en services van derden.
services: app-service\web, storage
documentationcenter: .net
author: tdykstra
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/27/2018
ms.author: tdykstra
ms.openlocfilehash: d0afc8b6f8e0b7ef73e5d1c3cbabf2e1542f47f4
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234513"
---
# <a name="get-started-with-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Aan de slag met de Azure WebJobs SDK voor gebeurtenisafhankelijke achtergrondverwerking

In dit artikel laat zien hoe een Azure WebJobs SDK-project maken en deze implementeren in Azure App Service lokaal uitvoeren.

De instructies zijn voor [Visual Studio 2017](https://www.visualstudio.com/vs/), maar dezelfde taken kunnen worden bewerkstelligd met andere hulpprogramma's, zoals [Visual Studio Code](https://code.visualstudio.com/).

## <a name="what-is-the-azure-webjobs-sdk"></a>Wat is de Azure WebJobs SDK?

De Azure WebJobs SDK is een raamwerk dat de taak vereenvoudigt van het schrijven van achtergrond verwerkingscode die toegang heeft tot gegevens in Azure-services. De SDK biedt een declaratieve syntaxis voor het opgeven van gebeurtenissen die moeten worden geactiveerd door een functie, zoals een nieuw bericht is toegevoegd aan een wachtrij. Hiermee bepaalt u vergelijkbare declaratieve syntaxis lezen en schrijven van gegevens als een functie is geactiveerd. Dit systeem van triggers en bindingen zorgt voor de meeste op laag niveau codering taken die zijn gekoppeld aan het openen van Azure en services van derden.

### <a name="functions-triggers-and-bindings"></a>Functies, triggers en bindingen

Een project WebJobs SDK definieert een of meer *functies*. Een functie is een methode die een trigger-kenmerk in de methodehandtekening heeft. Specificeren voorwaarden voor het aanroepen van een functie van triggers en bindingen aan te geven wat te lezen en schrijven. Bijvoorbeeld de trigger-kenmerk in de volgende functie weergegeven in de runtime voor de functie aanroepen wanneer een wachtrijbericht wordt weergegeven in de `items` wachtrij. De `Blob` kenmerk instrueert de runtime bericht uit de wachtrij gebruiken om te lezen van een blob in de *workitems* container. De inhoud van het bericht uit de wachtrij &mdash; opgegeven in de `queueTrigger` parameter &mdash; is de naam van de blob.

```cs
public static void Run(
    [QueueTrigger("items")] string myQueueItem,
    [Blob("workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    TraceWriter log)
{
    log.Info($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

### <a name="versions-2x-and-3x"></a>Versies 2.x en 3.x

De instructies hoe een WebJobs SDK versie 2.x-project maken met opmerkingen over wat verschillend voor 3.x (in preview is). De belangrijkste wijziging geïntroduceerd door 3.x is het gebruik van .NET Core in plaats van .NET Framework.

### <a name="azure-functions"></a>Azure Functions

[Azure Functions](../azure-functions/functions-overview.md) is gebaseerd op de WebJobs SDK en kan worden gebruikt wanneer u niet wilt dat de WebJobs SDK rechtstreeks gebruiken. Azure Functions 1.x gebruikt de WebJobs SDK 2.x. Zie voor meer informatie [vergelijking van Azure Functions met de WebJobs SDK](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs).

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u hebt [een Azure-account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) en ervaring met [apps in Azure App Service](app-service-web-overview.md). De stappen in dit artikel:

* [Installeer Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/) met de **ontwikkelen van Azure** werkbelasting. Als u al Visual Studio hebt, maar geen werkbelasting, de werkbelasting toevoegen door te selecteren **Extra > ophalen's en onderdelen**.
* [Maken van een App Service-app](app-service-web-get-started-dotnet-framework.md). Als u al die u een webtaak te implementeren hebt kunt, kunt u die kunt gebruiken in plaats van een nieuwe maken.

## <a name="create-a-project"></a>Een project maken

1. Selecteer in Visual Studio **bestand > nieuw project**.

2. Selecteer **Classic Windows Desktop > Console-App (.NET Framework)**.

   Selecteer voor het maken van een project 3.x **.NET Core > Console-App (.NET Core)**.

3. Noem het project *WebJobsSDKSample*, en selecteer vervolgens **OK**.

   ![Het dialoogvenster Nieuw project](./media/webjobs-sdk-get-started/new-project.png)

## <a name="add-webjobs-nuget-package"></a>WebJobs NuGet-pakket toevoegen

1. Installeer de nieuwste stabiele 2.x-versie van het NuGet-pakket `Microsoft.Azure.WebJobs`. (Voor de WebJobs SDK 3.x, kiest u de meest recente versie van de 3.x.)
 
   Hier volgt de **Package Manager Console** voor versie 2.2.0 opdracht:

   ```powershell
   Install-Package Microsoft.Azure.WebJobs -version 2.2.0
   ``` 

## <a name="create-the-jobhost"></a>De JobHost maken

De `JobHost` object bevat de runtime voor functies: luistert deze naar de functies triggers en aanroepen. 

1. In *Program.cs*, Voeg een `using` instructie:

   ```cs
   using Microsoft.Azure.WebJobs;
   ```

1. Vervang de `Main`-methode door de volgende code:

   ```cs
   static void Main()
   {
       var config = new JobHostConfiguration();
       var host = new JobHost(config);
       host.RunAndBlock();
   }
   ```

## <a name="enable-console-logging"></a>Console logboekregistratie inschakelen

Er zijn verschillende opties voor logboekregistratie in WebJobs SDK-project. Het is raadzaam de een de [framework voor logboekregistratie die is ontwikkeld voor ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging). Dit framework biedt betere prestaties en meer flexibiliteit bij het opslagmedium en filteren. 

In deze sectie maakt instellen u console logboekregistratie die gebruikmaakt van de nieuwe structuur.

1. Installeer de nieuwste stabiele versie van de volgende NuGet-pakketten:

   * `Microsoft.Extensions.Logging` -Het framework voor logboekregistratie.
   * `Microsoft.Extensions.Logging.Console` -De-console *provider*. Een provider verzendt logboeken naar de doellocatie in dit geval naar de console. 
 
   Hier volgen de **Package Manager Console** -opdrachten voor versie 2.0.1:

   ```powershell
   Install-Package Microsoft.Extensions.Logging -version 2.0.1
   ``` 

   ```powershell
   Install-Package Microsoft.Extensions.Logging.Console -version 2.0.1
   ``` 

2. In *Program.cs*, Voeg een `using` instructie:

   ```cs
   using Microsoft.Extensions.Logging;
   ```

3. In de `Main` methode, voeg code toe aan het bijwerken van de `JobHostConfiguration` voordat u de `JobHost`:
 
   ```
   config.DashboardConnectionString = "";
   var loggerFactory = new LoggerFactory();
   config.LoggerFactory = loggerFactory
       .AddConsole();
   ```

   Deze code maakt de volgende wijzigingen:

   * Schakelt [dashboard logboekregistratie](https://github.com/Azure/azure-webjobs-sdk/wiki/Queues#logs). Het dashboard is een verouderde hulpprogramma bewaking en dashboard logboekregistratie wordt niet aanbevolen voor productiescenario hoge gegevensdoorvoer.
   * De provider van de console met standaard toegevoegd [filteren](webjobs-sdk-how-to.md#log-filtering). 

   De `Main` methode is nu ziet er als volgt:

   ```
   var config = new JobHostConfiguration();
   config.DashboardConnectionString = "";
   var loggerFactory = new LoggerFactory();
   config.LoggerFactory = loggerFactory
       .AddConsole();
   var host = new JobHost(config);
   host.RunAndBlock();
   ```
   
## <a name="create-a-function"></a>Een functie maken

1. Maak *Functions.cs* in de projectmap en vervang de sjablooncode met deze code:

   ```cs
   using Microsoft.Azure.WebJobs;
   using Microsoft.Azure.WebJobs.Host;
   using Microsoft.Extensions.Logging;

   namespace WebJobsSDKSample
   {
       public class Functions
       {
           public static void ProcessQueueMessage([QueueTrigger("queue")] string message, ILogger logger)
           {
               logger.LogInformation(message);
           }
       }
   }
   ```

   De `QueueTrigger` kenmerk instrueert de runtime aan deze functie aanroepen wanneer een nieuw bericht is geschreven in een Azure Storage-wachtrij aangeroepen `queue`. De inhoud van het bericht uit de wachtrij worden geleverd aan de methode code in de `message` parameter. De hoofdtekst van de methode is waar u de triggergegevens verwerken. In dit voorbeeld registreert de code alleen het bericht.

   De `message` parameter hoeft te zijn van een tekenreeks. U kunt ook verbinden met een JSON-object, een bytematrix of een [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage) object. [Zie wachtrij trigger gebruik](../azure-functions/functions-bindings-storage-queue.md#trigger---usage). Elk bindingstype (zoals wachtrijen, blobs of tabellen) heeft een andere set parametertypen die u kunt koppelen aan.

## <a name="create-a-storage-account"></a>Create a storage account

De Azure-opslagemulator wordt lokaal uitgevoerd beschikt niet over alle functies waarvoor de WebJobs SDK. Dus in deze sectie u een opslagaccount in Azure maken en configureren van het project voor het gebruik van deze.

1. Open **Server Explorer** en aanmelden bij Azure. Met de rechtermuisknop op de **Azure** knooppunt en selecteer vervolgens **verbinding maken met Microsoft Azure-abonnement**.

   ![Aanmelden bij Azure](./media/webjobs-sdk-get-started/sign-in.png)

1. Onder de **Azure** knooppunt in **Server Explorer**, met de rechtermuisknop op **opslag**, en selecteer vervolgens **opslagaccount maken**.

   ![Menu voor Storage-account maken](./media/webjobs-sdk-get-started/create-storage-account-menu.png)

2. In de **Storage-Account maken** dialoogvenster Voer een unieke naam voor het opslagaccount.

3. Kies dezelfde **regio** dat u hebt gemaakt in uw App Service-app of een regio om u te sluiten.

1. Selecteer **Maken**.

   ![Storage-account maken](./media/webjobs-sdk-get-started/create-storage-account.png)

1. Onder de **opslag** knooppunt in **Server Explorer**, selecteer het nieuwe opslagaccount. In de **eigenschappen** venster, selecteer het weglatingsteken (**...** ) aan de rechterkant van de **verbindingsreeks** waardeveld.

   ![Verbinding tekenreeks weglatingsteken](./media/webjobs-sdk-get-started/conn-string-ellipsis.png)

2. Kopieer de verbindingsreeks en sla deze waarde ergens in dat u deze direct opnieuw kopiëren kunt.

   ![Kopieer de verbindingsreeks](./media/webjobs-sdk-get-started/copy-key.png)

## <a name="configure-storage-for-running-locally"></a>Configureren van opslag voor die lokaal wordt uitgevoerd

De WebJobs SDK wordt gezocht naar de verbindingsreeks voor opslag in de instellingen van de App-verzameling. Als u lokaal hebt uitgevoerd, wordt gezocht naar deze waarde in de *App.config* bestand of de omgeving variabelen.

1. Voeg het volgende XML-bestand naar de *App.config* bestand, onmiddellijk na de opening `<configuration>` tag.

   ```xml
   <connectionStrings>
     <add name="AzureWebJobsStorage" connectionString="{storage connection string}" />
   </connectionStrings>
   ```

2. Vervang *{opslagverbindingsreeks}* met de verbindingsreeks die u eerder hebt gekopieerd.

   Later hebt u de verbindingsreeks opnieuw gebruiken bij het configureren van de App Service-app in Azure.

## <a name="test-locally"></a>Lokaal testen

In deze sectie u bouwen en lokaal uitvoeren van het project en de functie activeren door het maken van een wachtrijbericht.

1. Druk op Ctrl + F5 het project wilt uitvoeren.

   De console ziet u dat de runtime vinden van uw functie en Wachtrijberichten wacht voor het activeren van het.

   ```console
   Found the following functions:
   WebJobsSDKSample.Functions.ProcessQueueMessage
   info: Host.Startup[0]
         Found the following functions:
         WebJobsSDKSample.Functions.ProcessQueueMessage
   Job host started
   info: Host.Startup[0]
         Job host started
   ```

   Ziet u een waarschuwing over een `ServicePointManager` instelling. Voor het testen van dat u met dit project doet, kunt u de waarschuwing negeren. Zie voor meer informatie over de waarschuwing [het gebruik van de WebJobs SDK](webjobs-sdk-how-to.md#jobhost-servicepointmanager-settings).

2. Sluit het consolevenster.

1. In **Server Explorer**, vouw het knooppunt voor uw nieuwe opslagaccount en klik vervolgens met de rechtermuisknop op **wachtrijen**. 

2. Selecteer **wachtrij maken**. 

3. Voer *wachtrij* als de naam voor de wachtrij en selecteer vervolgens **OK**.

   ![Wachtrij maken](./media/webjobs-sdk-get-started/create-queue.png)

4. Met de rechtermuisknop op het knooppunt voor de nieuwe wachtrij, en selecteer vervolgens **wachtrij weergeven**.

5. Selecteer de **bericht toevoegen** pictogram.

   ![Wachtrij maken](./media/webjobs-sdk-get-started/create-queue-message.png)

6. In de **bericht toevoegen** dialoogvenster Voer *Hello World!* Als de **berichttekst**, en selecteer vervolgens **OK**.

   ![Wachtrij maken](./media/webjobs-sdk-get-started/hello-world-text.png)

7. Voer het project opnieuw uit.

   Omdat u gebruikt de `QueueTrigger` kenmerk in de `ProcessQueueMessage` functie, de runtime WeJobs SDK luistert naar berichten in wachtrij plaatsen wanneer deze opgestart wordt. Er is een nieuwe wachtrijbericht gevonden in de wachtrij met de naam *wachtrij* en de functie aanroept.

   Vanwege [wachtrij polling exponentieel uitstel](../azure-functions/functions-bindings-storage-queue.md#trigger---polling-algorithm), het twee minuten om de runtime vinden van het bericht en roep de functie zo lang kan duren. Deze wachttijd kan worden verkleind door te voeren in [Ontwikkelingsmodus](webjobs-sdk-how-to.md#jobhost-development-settings).

  De console-uitvoer ziet er als volgt:

   ```console
   Found the following functions:
   WebJobsSDKSample.Functions.ProcessQueueMessage
   info: Host.Startup[0]
         Found the following functions:
         WebJobsSDKSample.Functions.ProcessQueueMessage
   Job host started
   info: Host.Startup[0]
         Job host started
   Executing 'Functions.ProcessQueueMessage' (Reason='New queue message detected on 'queue'.', Id=ebcb275d-0d7c-4293-a1af-93e0804b9e49)
   info: Function[0]
         Hello World!
   info: Host.Results[0]
         Executed 'Functions.ProcessQueueMessage' (Succeeded, Id=ebcb275d-0d7c-4293-a1af-93e0804b9e49)
   Executed 'Functions.ProcessQueueMessage' (Succeeded, Id=ebcb275d-0d7c-4293-a1af-93e0804b9e49)
   ```

8. Sluit het consolevenster.

## <a name="add-application-insights-logging"></a>Logboekregistratie van Application Insights toevoegen

Wanneer het project wordt uitgevoerd in Azure, kunt u een functie wordt uitgevoerd kan niet controleren door console-uitvoer weer te geven. Het is raadzaam de bewakingsoplossing [Application Insights](../application-insights/app-insights-overview.md). Zie voor meer informatie [Monitor Azure Functions](../azure-functions/functions-monitoring.md).

In deze sectie kunt u de volgende taken voor het instellen van Application Insights logboekregistratie voordat u naar Azure implementeert doen:

* Zorg ervoor dat u hebt een App Service-app en een exemplaar van de Application Insights werken met.
* Configureer de App Service-app voor het gebruik van de Application Insights-exemplaar en het opslagaccount dat u eerder hebt gemaakt.
* Het project voor logboekregistratie in Application Insights instellen.

### <a name="create-app-service-app-and-application-insights-instance"></a>App Service-app en Application Insights-exemplaar maken

1. Als u nog een App Service-app die u gebruiken kunt, hebt [maken van een](app-service-web-get-started-dotnet-framework.md).

2. Als u nog een Application Insights-resource die u gebruiken kunt, hebt [maken van een](../application-insights/app-insights-create-new-resource.md). Stel **toepassingstype** naar **algemene**, en overslaan van de volgende secties **de instrumentatiesleutel kopiëren**.

3. Als u al een Application Insights-resource die u gebruiken wilt, [de instrumentatiesleutel kopiëren](../application-insights/app-insights-create-new-resource.md#copy-the-instrumentation-key).

### <a name="configure-app-settings"></a>App-instellingen configureren 

1. In **Server Explorer**, vouw de **App Service** knooppunt onder **Azure**.

1. Vouw de resourcegroep die uw App Service-app in en klik met de rechtermuisknop op uw App Service-app.

3. Selecteer **instellingen weergeven**.

4. In de **verbindingsreeksen** Voeg de volgende vermelding.

   |Naam  |Verbindingsreeks  |Databasetype|
   |---------|---------|------|
   |AzureWebJobsStorage | {de verbindingsreeks voor opslag die u eerder hebt gekopieerd}|Aangepast telefoonnummer|
   
6. Als de **toepassingsinstellingen** vak beschikt niet over een Application Insights instrumentation sleutel, voegt de naam die u eerder hebt gekopieerd. (De instrumentatiesleutel mogelijk al zijn er, afhankelijk van hoe u de App Service-app hebt gemaakt.)

   |Naam  |Waarde  |
   |---------|---------|
   |APPINSIGHTS_INSTRUMENTATIONKEY | {instrumentatiesleutel} |

2. Vervang *{instrumentatiesleutel}* met de instrumentatiesleutel van de Application Insights-resource die u gebruikt.

2. Selecteer **Opslaan**.

1. Voeg het volgende XML-bestand naar de *App.config* -bestand, onmiddellijk na de verbinding tekenreeksen.

   ```xml
   <appSettings>
     <add key="APPINSIGHTS_INSTRUMENTATIONKEY" value="{instrumentation key}" />
   </appSettings>
   ```

2. Vervang *{instrumentatiesleutel}* met de instrumentatiesleutel van de Application Insights-resource die u gebruikt.

   Toevoegen van deze gegevens naar de *App.config* bestand kunt u de Application Insights om verbinding te testen wanneer u het project lokaal uitvoeren. 

3. Sla uw wijzigingen op.

### <a name="add-application-insights-logging-provider"></a>Application Insights logboekregistratie provider toevoegen

1. Installeer de nieuwste stabiele 2.x-versie van het NuGet-pakket voor de logboekregistratie van Application Insights-provider: `Microsoft.Azure.WebJobs.Logging.ApplicationInsights`. (Voor de WebJobs SDK 3.x, kiest u de meest recente 3.x-versie van het pakket.)

   Hier volgt de **Package Manager Console** voor versie 2.2.0 opdracht:

   ```powershell
   Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -version 2.2.0
   ``` 

1. Installeer de nieuwste stabiele 4.x-versie van het NuGet-pakket voor de .NET configuration manager: `System.Configuration.ConfigurationManager`.

   Hier volgt de **Package Manager Console** voor versie 4.4.1 opdracht:

   ```powershell
   Install-Package System.Configuration.ConfigurationManager -version 4.4.1
   ``` 

2. Open *Program.cs* en voeg een `using` -instructie voor de configuration manager:

   ```csharp
   using System.Configuration;
   ```

2. Vervang de code in de `Main` methode met de volgende code:

   ```csharp
   using (var loggerFactory = new LoggerFactory())
   {
       var config = new JobHostConfiguration();
       var instrumentationKey =
           ConfigurationManager.AppSettings["APPINSIGHTS_INSTRUMENTATIONKEY"];
       config.DashboardConnectionString = "";
       config.LoggerFactory = loggerFactory
           .AddApplicationInsights(instrumentationKey, null)
           .AddConsole();
       var host = new JobHost(config);
       host.RunAndBlock();
   }
   ```

   Deze code maakt de volgende wijzigingen:

   * Voegt een provider Application Insights logboekregistratie met standaard [filteren](webjobs-sdk-how-to.md#log-filtering); alle informatie en een hoger niveau Logboeken wordt nu gaat u naar de console en de Application Insights wanneer u lokaal uitvoert. 
   * Plaatst de `LoggerFactory` object in een `using` blok om ervoor te zorgen dat de uitvoer wordt leeggemaakt wanneer de host wordt afgesloten. 

## <a name="test-application-insights-logging"></a>Test Application Insights-logboekregistratie

In deze sectie uitvoeren u lokaal opnieuw om te controleren dat logboekregistratie gegevens nu zullen ook Application Insights garantie voor de console.

1. Gebruik **Server Explorer** voor het maken van een wachtrijbericht dezelfde manier als u dit hebt gedaan [eerdere](#trigger-the-function), behalve Voer *Hello App Insights!* Als de berichttekst.

1. Voer het project.

   De WebJobs SDK verwerkt bericht uit de wachtrij en ziet u de logboeken in het consolevenster.

1. Sluit het consolevenster.

1. Open de [Azure-portal](https://portal.azure.com/), en Ga naar uw Application Insights-resource.

2. Selecteer **Search**.

   ![Selecteer zoeken](./media/webjobs-sdk-get-started/select-search.png)

1. Als er geen de *Hello App Insights!* bericht, selecteer **vernieuwen** regelmatig of er enkele minuten. (Logboeken niet onmiddellijk omdat het duurt lang voordat de client Application Insights leegmaken van de logboeken worden verwerkt.)

   ![Logboeken in Application Insights](./media/webjobs-sdk-get-started/logs-in-ai.png)

3. Sluit het consolevenster.

## <a name="deploy-as-a-webjob"></a>Als een webtaak implementeren

In deze sectie kunt u het project implementeren als een webtaak. U deze implementeren op een App Service-app die u hebt [eerder gemaakte](#create-app-service-app-and-application-insights-instance). Als u wilt testen uw code terwijl deze wordt uitgevoerd in Azure, moet u een functie-aanroep door het maken van een wachtrijbericht activeren.

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer vervolgens **publiceren als Azure webtaak**.

1. In de **toevoegen Azure webtaak** dialoogvenster Selecteer **OK**.

   ![Azure webtaak toevoegen](./media/webjobs-sdk-get-started/add-azure-webjob.png)

   Visual Studio installeert automatisch een NuGet-pakket voor webtaak publiceren.

1. In de **profiel** stap van de **publiceren** wizard selecteert u **Microsoft Azure App Service**.

   ![Dialoogvenster publiceren](./media/webjobs-sdk-get-started/publish-dialog.png)

1. In de **App Service** dialoogvenster Selecteer **uw resourcegroep > uw App Service-app**, en selecteer vervolgens **OK**.

   ![Het dialoogvenster App Service](./media/webjobs-sdk-get-started/app-service-dialog.png)

1. In de **verbinding** stap van de wizard de optie **publiceren**.

## <a name="trigger-the-function-in-azure"></a>Activeren van de functie in Azure

1. Zorg ervoor dat u bent niet lokaal uitgevoerd (het consolevenster sluiten als deze nog steeds geopend). Anders mogelijk het lokale exemplaar van de eerste voor het verwerken van een Wachtrijberichten die u maakt.

1. Gebruik **Server Explorer** voor het maken van een wachtrijbericht dezelfde manier als u dit hebt gedaan [eerdere](#trigger-the-function), behalve Voer *Hello Azure!*.

7. Vernieuw de **wachtrij** pagina in Visual Studio en het nieuwe bericht is verdwenen omdat de functie die wordt uitgevoerd in Azure App Service is verwerkt.

   > [!TIP]
   > Wanneer u in Azure test, gebruiken [Ontwikkelingsmodus](webjobs-sdk-how-to.md#jobhost-development-settings) om te controleren of een wachtrij-activeringsfunctie meteen wordt aangeroepen en te voorkomen dat vertragingen veroorzaakt [wachtrij polling exponentieel uitstel](../azure-functions/functions-bindings-storage-queue.md#trigger---polling-algorithm).

### <a name="view-logs-in-application-insights"></a>Logboeken bekijken in Application Insights

1. Open de [Azure-portal](https://portal.azure.com/), en Ga naar uw Application Insights-resource.

2. Selecteer **Search**.

1. Als er geen de *Hello Azure!* bericht, selecteer **vernieuwen** regelmatig of er enkele minuten.

   U ziet de logboeken van de functie die wordt uitgevoerd in een webtaak met inbegrip van de *Hello Azure!* de tekst die u hebt ingevoerd in de vorige sectie.

## <a name="add-an-input-binding"></a>Toevoegen van een invoer-binding

Invoer bindingen vereenvoudigen code die gegevens leest. Bericht uit de wachtrij is een blob-naam voor dit voorbeeld en u de blob-naam om te zoeken en te lezen van een blob in Azure Storage.

1. In *Functions.cs*, vervang de `ProcessQueueMessage` methode met de volgende code:

   ```cs
   public static void ProcessQueueMessage(
       [QueueTrigger("queue")] string message,
       [Blob("container/{queueTrigger}", FileAccess.Read)] Stream myBlob,
       ILogger logger)
   {
       logger.LogInformation($"Blob name:{message} \n Size: {myBlob.Length} bytes");
   }
   ```

   In deze code `queueTrigger` is een [bindende expressie](../azure-functions/functions-triggers-bindings.md#binding-expressions-and-patterns), hetgeen betekent dat wordt omgezet naar een andere waarde tijdens runtime.  De inhoud van het bericht uit de wachtrij heeft tijdens runtime.

2. Voeg een `using`:

   ```cs
   using System.IO;
   ```

3. Maak een blobcontainer in uw opslagaccount.

   a. In **Server Explorer**, vouw het knooppunt voor uw opslagaccount, met de rechtermuisknop op **Blobs**, en selecteer vervolgens **Blob-Container maken**.

   b. In de **Blob-Container maken** dialoogvenster Voer *container* als de naam van de container en klik vervolgens op **OK**.

4. Upload de *Program.cs* bestand naar de blob-container. (Dit bestand wordt hier gebruikt als voorbeeld; u kunt uploaden van een tekstbestand en een wachtrijbericht maken met de naam van het bestand.)

   a. In **Server Explorer**, dubbelklik op het knooppunt voor de container die u zojuist hebt gemaakt.

   b. In de **Container** Selecteer de **uploaden** knop.

   ![Knop voor BLOB uploaden](./media/webjobs-sdk-get-started/blob-upload-button.png)

   c. Zoek en selecteer *Program.cs*, en selecteer vervolgens **OK**.

5. Maken van een wachtrijbericht in de wachtrij die u eerder hebt gemaakt, met *Program.cs* als de tekst van het bericht.

   ![Wachtrijbericht Program.cs](./media/webjobs-sdk-get-started/queue-msg-program-cs.png)

6. Voer het project.

   De functie, die vervolgens de blob leest en hiermee de duur voor bericht uit de wachtrij wordt geactiveerd. De console-uitvoer ziet er als volgt:

   ```console
   Found the following functions:
   ConsoleApp1.Functions.ProcessQueueMessage
   Job host started
   Executing 'Functions.ProcessQueueMessage' (Reason='New queue message detected on 'queue'.', Id=5a2ac479-de13-4f41-aae9-1361f291ff88)
   Blob name:Program.cs
   Size: 532 bytes
   Executed 'Functions.ProcessQueueMessage' (Succeeded, Id=5a2ac479-de13-4f41-aae9-1361f291ff88)
   ```

## <a name="add-an-output-binding"></a>Een uitvoerbinding toevoegen

Uitvoer bindingen vereenvoudigen code die gegevens worden geschreven. In dit voorbeeld wijzigt de vorige taak door het schrijven van een kopie van de blob in plaats van logboekregistratie de grootte.

1. Vervang de `ProcessQueueMessage`-methode door de volgende code:

   ```cs
   public static void ProcessQueueMessage(
       [QueueTrigger("queue")] string message,
       [Blob("container/{queueTrigger}", FileAccess.Read)] Stream myBlob,
       [Blob("container/copy-{queueTrigger}", FileAccess.Write)] Stream outputBlob,
       ILogger logger)
   {
       logger.LogInformation($"Blob name:{message} \n Size: {myBlob.Length} bytes");
       myBlob.CopyTo(outputBlob);
   }
   ```

5. Maken van een andere wachtrijbericht met *Program.cs* als de tekst van het bericht.

6. Voer het project.

   Bericht uit de wachtrij activeert de functie, die vervolgens leest de blob, registreert de lengte en maakt u een nieuwe blob. De console-uitvoer is hetzelfde, maar wanneer u gaat u naar de blob-container-venster en selecteer **vernieuwen**, ziet u een nieuwe blob met de naam *kopie Program.cs.*

## <a name="next-steps"></a>Volgende stappen

Deze handleiding leert maken, uitvoeren en een WebJobs SDK-project implementeert.

Zodat alles wat u een project WebJobs SDK probeert moest de instructies u een project maken vanaf het begin. Echter, wanneer u uw volgende project maakt, kunt u overwegen de **Azure webtaak** sjabloon in de **Cloud** categorie. Deze sjabloon maakt een project met NuGet-pakketten en voorbeeldcode al ingesteld. Houd er rekening mee dat de voorbeeldcode worden gewijzigd moeten mogelijk voor het gebruik van de nieuwe framework voor logboekregistratie.

Zie voor meer informatie [het gebruik van de WebJobs SDK](webjobs-sdk-how-to.md).
