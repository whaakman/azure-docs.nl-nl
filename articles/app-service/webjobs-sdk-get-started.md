---
title: Aan de slag met de WebJobs SDK - Azure
description: Inleiding tot de WebJobs SDK voor verwerking op de achtergrond gebeurtenisgestuurde. Leer hoe u toegang tot gegevens in Azure-services en services van derden.
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/27/2018
ms.author: glenga
ms.openlocfilehash: d9f3685e0a3961631f1577827cc415cb91d38a15
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54074237"
---
# <a name="get-started-with-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Aan de slag met de Azure WebJobs-SDK voor verwerking op de achtergrond gebeurtenisgestuurde

In dit artikel laat zien hoe een Azure WebJobs SDK-project maken en deze implementeren in Azure App Service lokaal uitvoeren.

De instructies zijn voor [Visual Studio 2017](https://www.visualstudio.com/vs/), maar dezelfde taken kunnen worden bewerkstelligd met andere hulpprogramma's, zoals [Visual Studio Code](https://code.visualstudio.com/).

## <a name="what-is-the-azure-webjobs-sdk"></a>Wat is de Azure WebJobs SDK?

De Azure WebJobs SDK is een structuur waarmee achtergrondverwerking eenvoudig van het schrijven van de achtergrond verwerkingscode die toegang heeft tot gegevens in Azure-services. De SDK biedt een declaratieve syntaxis voor het opgeven van gebeurtenissen die een functie, zoals een nieuw bericht is toegevoegd aan een wachtrij moeten worden geactiveerd. Vergelijkbare declaratieve syntaxis bepaalt lezen en schrijven van gegevens als een functie is geactiveerd. Dit systeem van triggers en bindingen zorgt dat de meeste van de laag niveau codering taken die zijn gekoppeld aan het openen van Azure en services van derden.

### <a name="functions-triggers-and-bindings"></a>Functies, triggers en bindingen

Een WebJobs SDK-project definieert een of meer *functies*. Een functie is een methode die een triggerkenmerk in de methodehandtekening heeft. Geeft u de voorwaarden voor het aanroepen van een functie voor triggers en bindingen aan te geven wat te lezen en schrijven. Bijvoorbeeld, de triggerkenmerk in de volgende functie vertelt de runtime voor het aanroepen van de functie wanneer er een wachtrijbericht wordt weergegeven in de `items` wachtrij. De `Blob` kenmerk geeft aan dat de runtime het wachtrijbericht gebruiken om te lezen van een blob in de *workitems* container. De inhoud van het wachtrijbericht &mdash; opgegeven in de `queueTrigger` parameter &mdash; is de naam van de blob.

```cs
public static void Run(
    [QueueTrigger("items")] string myQueueItem,
    [Blob("workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    TraceWriter log)
{
    log.Info($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

### <a name="versions-2x-and-3x"></a>Versie 2.x en 3.x

De instructies vertellen hoe ze moeten een WebJobs SDK versie 2.x-project maken. De meest recente versie van de WebJobs SDK is 3.x, maar is momenteel in preview en in dit artikel biedt geen instructies voor die versie nog hebt. De belangrijkste wijziging die is geïntroduceerd in versie 3.x is het gebruik van .NET Core in plaats van .NET Framework.

### <a name="azure-functions"></a>Azure Functions

[Azure Functions](../azure-functions/functions-overview.md) is gebaseerd op de WebJobs SDK en is een optie wanneer u niet nodig hebt voor rechtstreeks gebruik van de WebJobs SDK. Azure Functions 1.x maakt gebruik van de WebJobs SDK 2.x. Zie voor meer informatie, [vergelijking van Azure Functions en de WebJobs SDK](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs).

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u hebt [een Azure-account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) en ervaring met [apps in Azure App Service](overview.md). Voltooi de stappen in dit artikel:

* [Installeer Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/) met de **Azure-ontwikkeling** werkbelasting. Als u al van Visual Studio hebt maar geen die werkbelasting, het toevoegen van de werkbelasting door te selecteren **Tools > hulpprogramma's en onderdelen**.
* [Een App Service-app maken](app-service-web-get-started-dotnet-framework.md). Als u al die u een webtaak te implementeren hebt kunt, kunt u die gebruiken in plaats van een nieuwe maken.

## <a name="create-a-project"></a>Een project maken

1. Selecteer in Visual Studio, **bestand > nieuw project**.

2. Selecteer **klassieke bureaublad van Windows > consoletoepassing (.NET Framework)**.

3. Noem het project *WebJobsSDKSample*, en selecteer vervolgens **OK**.

   ![Het dialoogvenster Nieuw project](./media/webjobs-sdk-get-started/new-project.png)

## <a name="add-webjobs-nuget-package"></a>WebJobs NuGet-pakket toevoegen

1. Installeer de nieuwste stabiele 2.x-versie van het NuGet-pakket `Microsoft.Azure.WebJobs`.
 
   Hier volgt de **Package Manager Console** voor versie 2.2.0 opdracht:

   ```powershell
   Install-Package Microsoft.Azure.WebJobs -version 2.2.0
   ``` 

## <a name="create-the-jobhost"></a>De JobHost maken

De `JobHost` -object is de container runtime van functions: er wordt geluisterd naar triggers en aanroepen van functies. 

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

## <a name="enable-console-logging"></a>Console-logboekregistratie inschakelen

Er zijn verschillende opties voor logboekregistratie in de WebJobs SDK-project. Het is raadzaam een de [framework voor logboekregistratie die is ontwikkeld voor ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging). Dit framework biedt betere prestaties en meer flexibiliteit in de opslagmedia en filteren. 

In deze sectie maakt instellen u console-aanmelding die gebruikmaakt van de nieuwe framework.

1. Installeer de nieuwste stabiele versie van de volgende NuGet-pakketten:

   * `Microsoft.Extensions.Logging` -Het framework voor logboekregistratie.
   * `Microsoft.Extensions.Logging.Console` -De-console *provider*. Een provider verzendt logboeken naar een bepaald doel, in dit geval naar de console. 
 
   Hier volgen de **Package Manager Console** -opdrachten voor versie 2.0.1:

   ```powershell
   Install-Package Microsoft.Extensions.Logging -version 2.0.1
   ``` 

   ```powershell
   Install-Package Microsoft.Extensions.Logging.Console -version 2.0.1
   ``` 

1. In *Program.cs*, Voeg een `using` instructie:

   ```cs
   using Microsoft.Extensions.Logging;
   ```

1. In de `Main` methode, voeg code toe aan het bijwerken van de `JobHostConfiguration` voordat u de `JobHost`:
 
   ```
   config.DashboardConnectionString = "";
   var loggerFactory = new LoggerFactory();
   config.LoggerFactory = loggerFactory
       .AddConsole();
   ```

   Deze code brengt de volgende wijzigingen:

   * Schakelt [dashboard logboekregistratie](https://github.com/Azure/azure-webjobs-sdk/wiki/Queues#logs). Het dashboard is een oudere hulpprogramma voor het controleren en logboekregistratie van het dashboard wordt niet aanbevolen voor hoge doorvoer productiescenario's.
   * De console-provider met standaard toegevoegd [filteren](webjobs-sdk-how-to.md#log-filtering). 

   De `Main` methode nu er als volgt uitzien:

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

   De `QueueTrigger` kenmerk geeft aan dat de runtime voor het aanroepen van deze functie wanneer een nieuw bericht is geschreven in een Azure Storage-wachtrij met de naam `queue`. De inhoud van het bericht uit de wachtrij worden geleverd aan de methode code in de `message` parameter. De hoofdtekst van de methode is waar u de triggergegevens te verwerken. In dit voorbeeld registreert de code alleen het bericht.

   De `message` parameter hoeft te zijn van een tekenreeks. U kunt ook verbinden met een JSON-object, een bytematrix of een [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage) object. [Zie wachtrij trigger gebruik](../azure-functions/functions-bindings-storage-queue.md#trigger---usage). Elk bindingstype (zoals wachtrijen, blobs en tabellen) heeft een andere set parametertypen die u kunt koppelen aan.

## <a name="create-a-storage-account"></a>Create a storage account

De Azure-opslagemulator die lokaal wordt uitgevoerd, beschikt niet over alle functies waarvoor de WebJobs SDK. Dus in deze sectie u een opslagaccount in Azure maken en configureren van het project om deze te gebruiken.

1. Open **Server Explorer** in Visual studio en aanmelding bij Azure. Met de rechtermuisknop op de **Azure** knooppunt en selecteert u vervolgens **verbinding maken met Microsoft Azure-abonnement**.

   ![Aanmelden bij Azure](./media/webjobs-sdk-get-started/sign-in.png)

1. Onder de **Azure** knooppunt in **Server Explorer**, met de rechtermuisknop op **opslag**, en selecteer vervolgens **maken opslagaccount**.

   ![Menu voor Storage-account maken](./media/webjobs-sdk-get-started/create-storage-account-menu.png)

1. In de **Storage-Account maken** dialoogvenster vak, voer een unieke naam voor het opslagaccount.

1. Kies dezelfde **regio** dat u hebt gemaakt in uw App Service-app of een regio om u te sluiten.

1. Selecteer **Maken**.

   ![Storage-account maken](./media/webjobs-sdk-get-started/create-storage-account.png)

1. Onder de **opslag** knooppunt in **Server Explorer**, selecteert u het nieuwe opslagaccount. In de **eigenschappen** venster, selecteer het weglatingsteken (**...** ) aan de rechterkant van de **Connection String** waardeveld.

   ![Connection String beletselteken](./media/webjobs-sdk-get-started/conn-string-ellipsis.png)

1. Kopieer de verbindingsreeks, waarde en sla deze ergens dat u deze opnieuw gemakkelijk kopiëren kunt.

   ![Verbindingsreeks kopiëren](./media/webjobs-sdk-get-started/copy-key.png)

## <a name="configure-storage-for-running-locally"></a>Configureren van opslag voor het lokaal uitvoeren

De WebJobs SDK zoekt naar de verbindingsreeks voor opslag in de instellingen van de App-verzameling. Wanneer u lokaal uitvoert, wordt er gezocht naar deze waarde in de *App.config* bestand of de omgeving variabelen.

1. Voeg het volgende XML-bestand naar de *App.config* -bestand, onmiddellijk na het openen `<configuration>` tag.

   ```xml
   <connectionStrings>
     <add name="AzureWebJobsStorage" connectionString="{storage connection string}" />
   </connectionStrings>
   ```

1. Vervang *{opslagverbindingsreeks}* met de verbindingsreeks die u eerder hebt gekopieerd.

   Later moet u de verbindingsreeks opnieuw gebruiken bij het configureren van de App Service-app in Azure.

## <a name="test-locally"></a>Lokaal testen

In deze sectie maakt u bouwen en lokaal uitvoeren van het project en activeer de functie door het maken van een wachtrijbericht.

1. Druk op Ctrl + F5 om uit te voeren van het project.

   De console wordt weergegeven dat de runtime uw functie gevonden en wordt gewacht tot Wachtrijberichten voor het activeren van het.

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

   Ziet u een waarschuwingsbericht weergegeven over een `ServicePointManager` instelling. Voor het testen van dat u aan dit project gaat doen, kunt u de waarschuwing negeren. Zie voor meer informatie over de waarschuwing [over het gebruik van de WebJobs SDK](webjobs-sdk-how-to.md#jobhost-servicepointmanager-settings).

1. Het consolevenster sluiten.

1. In **Server Explorer** in Visual Studio, vouw het knooppunt voor uw nieuwe opslagaccount en klik vervolgens met de rechtermuisknop op **wachtrijen**. 

1. Selecteer **wachtrij maken**. 

1. Voer *wachtrij* als de naam voor de wachtrij en selecteer vervolgens **OK**.

   ![Wachtrij maken](./media/webjobs-sdk-get-started/create-queue.png)

1. Met de rechtermuisknop op het knooppunt voor de nieuwe wachtrij en selecteer vervolgens **wachtrij weergeven**.

1. Selecteer de **bericht toevoegen** pictogram.

   ![Wachtrij maken](./media/webjobs-sdk-get-started/create-queue-message.png)

1. In de **bericht toevoegen** dialoogvenster Voer *Hello World!* Als de **berichttekst**, en selecteer vervolgens **OK**.

   ![Wachtrij maken](./media/webjobs-sdk-get-started/hello-world-text.png)

1. Voer het project opnieuw uit.

   Omdat u gebruikt de `QueueTrigger` kenmerk in de `ProcessQueueMessage` functie, de runtime WeJobs SDK luistert naar berichten in wachtrij plaatsen wanneer deze opgestart wordt. Een nieuw wachtrijbericht is gevonden in de wachtrij met de naam *wachtrij* en de functie aanroept.

   Vanwege [wachtrij polling exponentieel uitstel](../azure-functions/functions-bindings-storage-queue.md#trigger---polling-algorithm), duurt het zo lang 2 minuten voor de runtime voor het vinden van het bericht en de functie aanroepen. Deze wachttijd kan worden teruggebracht door te voeren in [Ontwikkelingsmodus](webjobs-sdk-how-to.md#jobhost-development-settings).

  De console-uitvoer ziet er zo uit:

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

1. Het consolevenster sluiten.

## <a name="add-application-insights-logging"></a>Logboekregistratie voor Application Insights toevoegen

Wanneer het project wordt uitgevoerd in Azure, kunt u functie-uitvoering kan niet controleren door console-uitvoer weer te geven. Het is raadzaam de oplossing voor controle [Application Insights](../application-insights/app-insights-overview.md). Zie voor meer informatie, [Monitor Azure Functions](../azure-functions/functions-monitoring.md).

In deze sectie maakt uitvoeren u de volgende taken voor het instellen van Application Insights registreren voordat u naar Azure implementeert:

* Zorg ervoor dat u hebt een Application Insights-exemplaar om te werken met en een App Service-app.
* Configureer de App Service-app voor het gebruik van de Application Insights-exemplaar en het opslagaccount dat u eerder hebt gemaakt.
* Instellen van het project voor logboekregistratie in Application Insights.

### <a name="create-app-service-app-and-application-insights-instance"></a>App Service-app en Application Insights-exemplaar maken

1. Als u nog een App Service-app die u gebruiken kunt, hebt [maakt u er een](app-service-web-get-started-dotnet-framework.md).

1. Als u nog een Application Insights-resource die u gebruiken kunt, hebt [maakt u er een](../azure-monitor/app/create-new-resource.md ). Stel **toepassingstype** naar **algemene**, en overslaan van de volgende secties **de instrumentatiesleutel kopiëren**.

1. Als u al een Application Insights-resource die u gebruiken wilt, [de instrumentatiesleutel kopiëren](../azure-monitor/app/create-new-resource.md #copy-the-instrumentation-key).

### <a name="configure-app-settings"></a>App-instellingen configureren 

1. In **Server Explorer** in Visual Studio, vouw de **App Service** onder het knooppunt **Azure**.

1. Vouw de resourcegroep die uw App Service-app in en klik vervolgens met de rechtermuisknop op uw App Service-app.

1. Selecteer **weergave-instellingen**.

1. In de **verbindingsreeksen** Voeg de volgende vermelding.

   |Name  |Connection String  |Databasetype|
   |---------|---------|------|
   |AzureWebJobsStorage | {de verbindingsreeks voor opslag die u eerder hebt gekopieerd}|Aangepast telefoonnummer|
   
1. Als de **toepassingsinstellingen** vak beschikt niet over de instrumentatie van een Application Insights-sleutel, voegt u de naam die u eerder hebt gekopieerd. (De instrumentatiesleutel kan al zijn er, afhankelijk van hoe u de App Service-app hebt gemaakt.)

   |Name  |Waarde  |
   |---------|---------|
   |APPINSIGHTS_INSTRUMENTATIONKEY | {instrumentatiesleutel} |

1. Vervang *{instrumentatiesleutel}* met de instrumentatiesleutel van de Application Insights-resource die u gebruikt.

1. Selecteer **Opslaan**.

1. Voeg het volgende XML-bestand naar de *App.config* bestand, onmiddellijk na de verzameling van de tekenreeksen verbinding.

   ```xml
   <appSettings>
     <add key="APPINSIGHTS_INSTRUMENTATIONKEY" value="{instrumentation key}" />
   </appSettings>
   ```

1. Vervang *{instrumentatiesleutel}* met de instrumentatiesleutel van de Application Insights-resource die u gebruikt.

   Toevoegen van deze gegevens naar de *App.config* bestand kunt u de Application Insights-verbinding testen wanneer u het project lokaal uitvoeren. 

1. Sla uw wijzigingen op.

### <a name="add-application-insights-logging-provider"></a>Toevoegen van Application Insights-provider voor logboekregistratie

1. Installeer de nieuwste stabiele 2.x-versie van het NuGet-pakket voor de logboekregistratie voor Application Insights-provider: `Microsoft.Azure.WebJobs.Logging.ApplicationInsights`.

   Hier volgt de **Package Manager Console** voor versie 2.2.0 opdracht:

   ```powershell
   Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -version 2.2.0
   ``` 

1. Installeer de nieuwste stabiele 4.x-versie van het NuGet-pakket voor de configuration manager voor .NET: `System.Configuration.ConfigurationManager`.

   Hier volgt de **Package Manager Console** voor versie 4.4.1 opdracht:

   ```powershell
   Install-Package System.Configuration.ConfigurationManager -version 4.4.1
   ``` 

1. Open *Program.cs* en voeg een `using` -instructie voor de configuration manager:

   ```csharp
   using System.Configuration;
   ```

1. Vervang de code in de `Main` methode met de volgende code:

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

   Deze code brengt de volgende wijzigingen:

   * Voegt een logboekregistratie voor Application Insights-provider met standaard [filteren](webjobs-sdk-how-to.md#log-filtering); alle gegevens en logboeken van hoger niveau wordt nu omgeleid naar de console en de Application Insights wanneer u lokaal uitvoert. 
   * Hebt u de `LoggerFactory` van het object in een `using` blokkeren om ervoor te zorgen dat de uitvoer moet worden leeggemaakt wanneer de host wordt afgesloten. 

## <a name="test-application-insights-logging"></a>Test Application Insights-logboekregistratie

In deze sectie uitvoeren u lokaal opnieuw om te controleren dat logboekregistratie gegevens nu naar Application Insights evenals garantie voor de console zullen.

1. Gebruik **Server Explorer** in Visual Studio om een wachtrijbericht te maken, de dezelfde manier waarop u hebt [eerdere](#trigger-the-function-in-azure), met uitzondering van Voer *Hello App Insights!* Als de berichttekst.

1. Voer het project.

   De WebJobs SDK verwerkt bericht uit de wachtrij en ziet u de logboeken in het consolevenster.

1. Het consolevenster sluiten.

1. Open de [Azure-portal](https://portal.azure.com/), en Ga naar uw Application Insights-resource.

1. Selecteer **zoeken**.

   ![Selecteer zoeken](./media/webjobs-sdk-get-started/select-search.png)

1. Als er geen de *Hello App Insights!* bericht, selecteer **vernieuwen** periodiek voor enkele minuten. (Logboeken worden niet weergegeven onmiddellijk omdat het enige tijd duren voor de client Application Insights leegmaken van de logboeken worden verwerkt.)

   ![Logboeken in Application Insights](./media/webjobs-sdk-get-started/logs-in-ai.png)

1. Het consolevenster sluiten.

## <a name="deploy-as-a-webjob"></a>Als een WebJob implementeren

In deze sectie kunt u het project implementeren als een WebJob. U deze implementeren in een App Service-app die u [eerder hebt gemaakt](#create-app-service-app-and-application-insights-instance). Als u wilt testen van uw code tijdens het uitvoeren in Azure, zult u een functie-aanroep te maken van een wachtrijbericht activeren.

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer vervolgens **publiceren als Azure WebJob**.

1. In de **Azure-webtaak toevoegen** dialoogvenster, selecteer **OK**.

   ![Azure-webtaak toevoegen](./media/webjobs-sdk-get-started/add-azure-webjob.png)

   Visual Studio installeert automatisch een NuGet-pakket voor het publiceren van de webtaak.

1. In de **profiel** stap van de **publiceren** wizard de optie **Microsoft Azure App Service**.

   ![Dialoogvenster publiceren](./media/webjobs-sdk-get-started/publish-dialog.png)

1. In de **App Service** dialoogvenster, selecteer **uw resourcegroep > uw App Service-app**, en selecteer vervolgens **OK**.

   ![Dialoogvenster App Service](./media/webjobs-sdk-get-started/app-service-dialog.png)

1. In de **verbinding** stap van de wizard de optie **publiceren**.

## <a name="trigger-the-function-in-azure"></a>Activeer de functie in Azure

1. Zorg ervoor dat u bent niet lokaal worden uitgevoerd (in het consolevenster sluiten als dit nog steeds geopend is). Anders mogelijk het lokale exemplaar van de eerste voor het verwerken van een Wachtrijberichten die u maakt.


1. Vernieuw de **wachtrij** pagina in Visual Studio en het nieuwe bericht is verdwenen omdat de functie die wordt uitgevoerd in Azure App Service is verwerkt.

   > [!TIP]
   > Wanneer u in Azure test, gebruikt u [Ontwikkelingsmodus](webjobs-sdk-how-to.md#jobhost-development-settings) ervoor te zorgen dat een activeringsfunctie meteen wordt aangeroepen en voorkomen dat wordt vertraagd vanwege [wachtrij polling exponentieel uitstel](../azure-functions/functions-bindings-storage-queue.md#trigger---polling-algorithm).

### <a name="view-logs-in-application-insights"></a>Logboeken weergeven in Application Insights

1. Open de [Azure-portal](https://portal.azure.com/), en Ga naar uw Application Insights-resource.

1. Selecteer **zoeken**.

1. Als er geen de *Hello Azure!* bericht, selecteer **vernieuwen** periodiek voor enkele minuten.

   U ziet dat de logboeken van de functie die wordt uitgevoerd in een webtaak, met inbegrip van de *Hello Azure!* de tekst die u hebt ingevoerd in de vorige sectie.

## <a name="add-an-input-binding"></a>Een Invoerbinding toevoegen

Invoerbindingen vereenvoudigen code die gegevens leest. Bericht uit de wachtrij is een blobnaam voor dit voorbeeld, en u gebruikt de blob-naam om te zoeken en lezen van een blob in Azure Storage.

1. In *Functions.cs*, vervangen de `ProcessQueueMessage` methode met de volgende code:

   ```cs
   public static void ProcessQueueMessage(
       [QueueTrigger("queue")] string message,
       [Blob("container/{queueTrigger}", FileAccess.Read)] Stream myBlob,
       ILogger logger)
   {
       logger.LogInformation($"Blob name:{message} \n Size: {myBlob.Length} bytes");
   }
   ```

   In deze code `queueTrigger` is een [binding expressie](../azure-functions/functions-triggers-bindings.md#binding-expressions-and-patterns), wat betekent dat deze wordt omgezet naar een andere waarde tijdens runtime.  Deze heeft de inhoud van het wachtrijbericht tijdens runtime.

1. Voeg een `using`:

   ```cs
   using System.IO;
   ```

1. Maak een blobcontainer in uw opslagaccount.

   a. In **Server Explorer** in Visual Studio, vouw het knooppunt voor uw opslagaccount, met de rechtermuisknop op **Blobs**, en selecteer vervolgens **Blob-Container maken**.

   b. In de **Blob-Container maken** dialoogvenster Voer *container* als containernaam van de en klik vervolgens op **OK**.

1. Upload de *Program.cs* -bestand naar de blob-container. (Dit bestand wordt hier gebruikt als voorbeeld; u kunt een tekstbestand uploaden en een wachtrijbericht te maken met de naam van het bestand.)

   a. In **Server Explorer**, dubbelklik op het knooppunt voor de container die u zojuist hebt gemaakt.

   b. In de **Container** venster de **uploaden** knop.

   ![Knop voor het uploaden van BLOB](./media/webjobs-sdk-get-started/blob-upload-button.png)

   c. Zoek en selecteer *Program.cs*, en selecteer vervolgens **OK**.

1. Een wachtrijbericht te maken in de wachtrij die u eerder hebt gemaakt, met *Program.cs* als de tekst van het bericht.

   ![Wachtrijbericht Program.cs](./media/webjobs-sdk-get-started/queue-msg-program-cs.png)

1. Voer het project.

   Bericht uit de wachtrij wordt geactiveerd voor de functie, die vervolgens wordt de blob gelezen en de lengte-Logboeken. De console-uitvoer ziet er zo uit:

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

Uitvoerbindingen vereenvoudigen code waarmee gegevens worden geschreven. In dit voorbeeld wijzigt de voorgaande build is door een kopie van de blob in plaats van logboekregistratie de grootte ervan te schrijven.

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

1. Maken van een andere wachtrijbericht met *Program.cs* als de tekst van het bericht.

1. Voer het project.

   Bericht uit de wachtrij wordt de functie, die vervolgens wordt de blob gelezen, registreert de lengte en maakt u een nieuwe blob geactiveerd. De console-uitvoer is hetzelfde, maar wanneer u gaat u naar de blob-container-venster en selecteer **vernieuwen**, ziet u een nieuwe blob met de naam *kopie-Program.cs.*

## <a name="next-steps"></a>Volgende stappen

Deze handleiding heeft laten zien hoe u kunt maken, uitvoeren en implementeren van een WebJobs SDK-project.

Om weer te geven alles wat u een WebJobs SDK-project krijgt, had de instructies u bij het maken van een project maken. Echter, wanneer u uw volgende project maakt, kunt u overwegen de **Azure-webtaak** sjabloon in de **Cloud** categorie. Deze sjabloon maakt een project met NuGet-pakketten en voorbeeldcode al ingesteld. Houd er rekening mee dat de voorbeeldcode worden gewijzigd moeten mogelijk voor het gebruik van de nieuwe framework voor logboekregistratie.

Zie voor meer informatie, [over het gebruik van de WebJobs SDK](webjobs-sdk-how-to.md).
