---
title: Aan de slag met de WebJobs SDK - Azure
description: Inleiding tot de WebJobs SDK voor verwerking op de achtergrond gebeurtenisgestuurde. Leer hoe u toegang tot gegevens in Azure-services en services van derden.
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: jeconnoc
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/18/2019
ms.author: glenga
ms.openlocfilehash: b824c99a015cfa2c1d1c75e2a1257eff482e8dd6
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58087955"
---
# <a name="get-started-with-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Aan de slag met de Azure WebJobs-SDK voor verwerking op de achtergrond gebeurtenisgestuurde

Dit artikel wordt beschreven hoe u een Azure WebJobs SDK-project maken, lokaal uitvoeren, en vervolgens implementeert met Visual Studio 2017 [Azure App Service](overview.md). Het project dat u maakt, is een .NET Core-console-app die gebruikmaakt van versie 3.x van de WebJobs SDK. Als u geïnteresseerd in versie bent 2.x, die gebruikmaakt van .NET Framework, Zie [ontwikkelen en implementeren met Visual Studio - Azure App Service WebJobs](webjobs-dotnet-deploy-vs.md).

Zie voor meer informatie over het werken met de WebJobs SDK [over het gebruik van de Azure WebJobs SDK voor verwerking op de achtergrond gebeurtenisgestuurde](webjobs-sdk-how-to.md).

## <a name="prerequisites"></a>Vereisten

* [Installeer Visual Studio 2017](/visualstudio/install/) met de **Azure-ontwikkeling** werkbelasting. Als u al van Visual Studio hebt maar geen die werkbelasting, het toevoegen van de werkbelasting door te selecteren **Tools > hulpprogramma's en onderdelen**.

* U moet hebben [een Azure-account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) de WebJobs SDK-project kunt publiceren naar Azure.

## <a name="create-a-project"></a>Een project maken

1. Selecteer in Visual Studio, **bestand > Nieuw > Project**.

2. Selecteer **.NET Core > consoletoepassing (.NET Core)**.

3. Noem het project *WebJobsSDKSample*, en selecteer vervolgens **OK**.

   ![Het dialoogvenster Nieuw project](./media/webjobs-sdk-get-started/new-project.png)

## <a name="webjobs-nuget-packages"></a>WebJobs NuGet-pakketten

1. Installeer de nieuwste stabiele 3.x-versies van de volgende NuGet-pakketten:

   * `Microsoft.Azure.WebJobs`
   * `Microsoft.Azure.WebJobs.Extensions`

     Hier volgt de **Package Manager Console** opdrachten voor versie 3.0.4:

     ```powershell
     Install-Package Microsoft.Azure.WebJobs -version 3.0.4
     Install-Package Microsoft.Azure.WebJobs.Extensions -version 3.0.1
     ```

## <a name="create-the-host"></a>Maken van de Host

De host is de runtime-container voor functions die naar triggers en aanroepen van functies luistert. De volgende stappen maakt u een host die wordt geïmplementeerd [ `IHost` ](/dotnet/api/microsoft.extensions.hosting.ihost), dit is de algemene Host in ASP.NET Core.

1. In *Program.cs*, Voeg een `using` instructie:

    ```cs
    using Microsoft.Extensions.Hosting;
    ```

1. Vervang de `Main`-methode door de volgende code:

    ```cs
    static void Main(string[] args)
    {
        var builder = new HostBuilder();
        builder.ConfigureWebJobs(b =>
                {
                    b.AddAzureStorageCoreServices();
                });
        var host = builder.Build();
        using (host)
        {
            host.Run();
        }
    }
    ```

In de ASP.NET Core, host configuraties zijn ingesteld door het aanroepen van methoden voor het [ `HostBuilder` ](/dotnet/api/microsoft.extensions.hosting.hostbuilder) exemplaar. Zie voor meer informatie, [algemene Host .NET](/aspnet/core/fundamentals/host/generic-host). De `ConfigureWebJobs` uitbreidingsmethode initialiseert de WebJobs-host. In `ConfigureWebJobs`, u specifieke WebJobs extensies initialiseren en instellen van eigenschappen van de uitbreidingen.  

## <a name="enable-console-logging"></a>Console-logboekregistratie inschakelen

In deze sectie maakt u een console-aanmelding die gebruikmaakt van instellen de [framework voor logboekregistratie van ASP.NET Core](/aspnet/core/fundamentals/logging).

1. Installeer de nieuwste stabiele versie van de volgende NuGet-pakketten:

   * `Microsoft.Extensions.Logging` -Het framework voor logboekregistratie.
   * `Microsoft.Extensions.Logging.Console` -De console-provider, waarmee Logboeken wordt verzonden naar de console.

   Hier volgen de **Package Manager Console** opdrachten voor versie 2.2.0:

   ```powershell
   Install-Package Microsoft.Extensions.Logging -version 2.2.0
   ```

   ```powershell
   Install-Package Microsoft.Extensions.Logging.Console -version 2.2.0
   ```

1. In *Program.cs*, Voeg een `using` instructie:

   ```cs
   using Microsoft.Extensions.Logging;
   ```

1. Roep de [ `ConfigureLogging` ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) methode voor [ `HostBuilder` ](/dotnet/api/microsoft.extensions.hosting.hostbuilder). De [ `AddConsole` ](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions.addconsole) methode console logboekregistratie wordt toegevoegd aan de configuratie.

    ```cs
    builder.ConfigureLogging((context, b) =>
    {
        b.AddConsole();
    });
    ```

    De `Main` methode nu er als volgt uitzien:

    ```cs
    static void Main(string[] args)
    {
        var builder = new HostBuilder();
        builder.ConfigureWebJobs(b =>
                {
                    b.AddAzureStorageCoreServices();
                });
        builder.ConfigureLogging((context, b) =>
                {
                    b.AddConsole();
                });
        var host = builder.Build();
        using (host)
        {
            host.Run();
        }
    }
    ```

    Deze update doet het volgende:

    * Schakelt [dashboard logboekregistratie](https://github.com/Azure/azure-webjobs-sdk/wiki/Queues#logs). Het dashboard is een oudere hulpprogramma voor het controleren en logboekregistratie van het dashboard wordt niet aanbevolen voor hoge doorvoer productiescenario's.
    * De console-provider met standaard toegevoegd [filteren](webjobs-sdk-how-to.md#log-filtering).

U kunt nu, een functie die wordt geactiveerd door berichten die in de toevoegen een [Azure Storage-wachtrij](../azure-functions/functions-bindings-storage-queue.md).

## <a name="install-the-storage-binding-extension"></a>De Storage-bindingextensie installeren

Vanaf versie 3.x, moet u expliciet de bindingsuitbreiding opslag is vereist door de WebJobs-SDK installeren. In eerdere versies van zijn de Storage-bindingen opgenomen in de SDK.

1. Installeer de nieuwste stabiele versie van de [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) NuGet-pakket versie 3.x. 

    Hier volgt de **Package Manager Console** voor versie 3.0.3 opdracht:

    ```powershell
    Install-Package Microsoft.Azure.WebJobs.Extensions.Storage -Version 3.0.3
    ```

2. In de `ConfigureWebJobs` uitbreidingsmethode, belt de `AddAzureStorage` methode voor het [ `HostBuilder` ](/dotnet/api/microsoft.extensions.hosting.hostbuilder) exemplaar te initialiseren van de opslag-extensie. Op dit moment de `ConfigureWebJobs` methode ziet eruit als in het volgende voorbeeld:

    ```cs
    builder.ConfigureWebJobs(b =>
                    {
                        b.AddAzureStorageCoreServices();
                        b.AddAzureStorage();
                    });
    ```

## <a name="create-a-function"></a>Een functie maken

1. Met de rechtermuisknop op het project, selecteer **toevoegen** > **Nieuw Item...** , kiest u **klasse**, de nieuwe naam C# klassebestand *Functions.cs*, en selecteer **toevoegen**.

1. In Functions.cs, kunt u de gegenereerde sjabloon vervangen door de volgende code:

   ```cs
   using Microsoft.Azure.WebJobs;
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

De Azure-opslagemulator die lokaal wordt uitgevoerd, beschikt niet over alle functies waarvoor de WebJobs SDK. Dus in deze sectie u een opslagaccount in Azure maken en configureren van het project om deze te gebruiken. Als u al een storage-account hebt, gaat u naar stap 6.

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

## <a name="configure-storage-to-run-locally"></a>Configureren van opslag aan het lokaal uitvoeren

De WebJobs SDK zoekt naar de verbindingsreeks voor opslag in de toepassingsinstellingen in Azure. Wanneer u lokaal uitvoert, zoekt deze waarde in het lokale configuratiebestand of omgevingsvariabelen.

1. Met de rechtermuisknop op het project, selecteer **toevoegen** > **Nieuw Item...** , kiest u **JavaScript-JSON-configuratiebestand**, Noem het nieuwe bestand *appsettings.json* en selecteer **toevoegen**. 

1. Voeg in het nieuwe bestand, een `AzureWebJobsStorage` veld, zoals in het volgende voorbeeld:

    ```json
    {
        "AzureWebJobsStorage": "{storage connection string}"
    }
    ```

1. Vervang *{opslagverbindingsreeks}* met de verbindingsreeks die u eerder hebt gekopieerd.

1. Selecteer de *appsettings.json* bestand in Solution Explorer en klik in de **eigenschappen** venster **naar uitvoermap kopiëren** naar **kopiëren indien nieuwer**.

Later, voegt u de dezelfde appinstelling voor de verbindingsreeks in uw app in Azure App Service.

## <a name="test-locally"></a>Lokaal testen

In deze sectie maakt u bouwen en lokaal uitvoeren van het project en activeer de functie door het maken van een wachtrijbericht.

1. Druk op **Ctrl + F5** het project wilt uitvoeren.

   De console wordt weergegeven dat de runtime uw functie gevonden en wordt gewacht tot Wachtrijberichten voor het activeren van het. De volgende uitvoer wordt gegenereerd door de host v3.x:

   ```console
    info: Microsoft.Azure.WebJobs.Hosting.JobHostService[0]
          Starting JobHost
    info: Host.Startup[0]
          Found the following functions:
          WebJobsSDKSample.Functions.ProcessQueueMessage

    info: Host.Startup[0]
          Job host started
    Application started. Press Ctrl+C to shut down.
    Hosting environment: Development
    Content root path: C:\WebJobsSDKSample\WebJobsSDKSample\bin\Debug\netcoreapp2.1\
   ```

1. Het consolevenster sluiten.

1. In **Server Explorer** in Visual Studio, vouw het knooppunt voor uw nieuwe opslagaccount en klik vervolgens met de rechtermuisknop op **wachtrijen**.

1. Selecteer **wachtrij maken**.

1. Voer *wachtrij* als de naam voor de wachtrij en selecteer vervolgens **OK**.

   ![Wachtrij maken](./media/webjobs-sdk-get-started/create-queue.png)

1. Met de rechtermuisknop op het knooppunt voor de nieuwe wachtrij en selecteer vervolgens **wachtrij weergeven**.

1. Selecteer de **bericht toevoegen** pictogram.

   ![Wachtrij maken](./media/webjobs-sdk-get-started/create-queue-message.png)

1. In de **bericht toevoegen** dialoogvenster Voer *Hello World!* Als de **berichttekst**, en selecteer vervolgens **OK**. Er is nu een bericht in de wachtrij.

   ![Wachtrij maken](./media/webjobs-sdk-get-started/hello-world-text.png)

1. Voer het project opnieuw uit.

   Omdat u gebruikt de `QueueTrigger` kenmerk in de `ProcessQueueMessage` functie, de runtime WeJobs SDK luistert naar berichten in wachtrij plaatsen wanneer deze opgestart wordt. Een nieuw wachtrijbericht is gevonden in de wachtrij met de naam *wachtrij* en de functie aanroept.

   Vanwege [wachtrij polling exponentieel uitstel](../azure-functions/functions-bindings-storage-queue.md#trigger---polling-algorithm), duurt het zo lang 2 minuten voor de runtime voor het vinden van het bericht en de functie aanroepen. Deze wachttijd kan worden teruggebracht door te voeren in [Ontwikkelingsmodus](webjobs-sdk-how-to.md#host-development-settings).

   De console-uitvoer ziet er zo uit:

   ```console
    info: Function.ProcessQueueMessage[0]
          Executing 'Functions.ProcessQueueMessage' (Reason='New queue message detected on 'queue'.', Id=2c319369-d381-43f3-aedf-ff538a4209b8)
    info: Function.ProcessQueueMessage[0]
          Trigger Details: MessageId: b00a86dc-298d-4cd2-811f-98ec39545539, DequeueCount: 1, InsertionTime: 1/18/2019 3:28:51 AM +00:00
    info: Function.ProcessQueueMessage.User[0]
          Hello World!
    info: Function.ProcessQueueMessage[0]
          Executed 'Functions.ProcessQueueMessage' (Succeeded, Id=2c319369-d381-43f3-aedf-ff538a4209b8)
   ```

1. Het consolevenster sluiten. 

1. Ga terug naar de wachtrij-venster en vernieuw deze. Het bericht is verwijderd, omdat deze is verwerkt door de functie lokaal worden uitgevoerd. 

## <a name="add-application-insights-logging"></a>Logboekregistratie voor Application Insights toevoegen

Wanneer het project wordt uitgevoerd in Azure, kunt u functie-uitvoering kan niet controleren door console-uitvoer weer te geven. Het is raadzaam de oplossing voor controle [Application Insights](../azure-monitor/app/app-insights-overview.md). Zie voor meer informatie, [Monitor Azure Functions](../azure-functions/functions-monitoring.md).

In deze sectie maakt uitvoeren u de volgende taken voor het instellen van Application Insights registreren voordat u naar Azure implementeert:

* Zorg ervoor dat u hebt een Application Insights-exemplaar om te werken met en een App Service-app.
* Configureer de App Service-app voor het gebruik van de Application Insights-exemplaar en het opslagaccount dat u eerder hebt gemaakt.
* Instellen van het project voor logboekregistratie in Application Insights.

### <a name="create-app-service-app-and-application-insights-instance"></a>App Service-app en Application Insights-exemplaar maken

1. Als u nog een App Service-app die u gebruiken kunt, hebt [maakt u er een](app-service-web-get-started-dotnet-framework.md). Wanneer u uw app maakt, kunt u ook een verbonden Application Insights-resource maken. Wanneer u dit doet, de `APPINSIGHTS_INSTRUMENTATIONKEY` voor u is ingesteld in uw app.

1. Als u nog een Application Insights-resource die u gebruiken kunt, hebt [maakt u er een](../azure-monitor/app/create-new-resource.md ). Stel **toepassingstype** naar **algemene**, en overslaan van de volgende secties **de instrumentatiesleutel kopiëren**.

1. Als u al een Application Insights-resource die u gebruiken wilt, [de instrumentatiesleutel kopiëren](../azure-monitor/app/create-new-resource.md#copy-the-instrumentation-key).

### <a name="configure-app-settings"></a>App-instellingen configureren 

1. In **Server Explorer** in Visual Studio, vouw de **App Service** onder het knooppunt **Azure**.

1. Vouw de resourcegroep die uw App Service-app in en klik vervolgens met de rechtermuisknop op uw App Service-app.

1. Selecteer **weergave-instellingen**.

1. In de **verbindingsreeksen** Voeg de volgende vermelding.

   |Name  |Connection String  |Databasetype|
   |---------|---------|------|
   |AzureWebJobsStorage | {de verbindingsreeks voor opslag die u eerder hebt gekopieerd}|Aangepast telefoonnummer|

1. Als de **toepassingsinstellingen** vak beschikt niet over de instrumentatie van een Application Insights-sleutel, voegt u de naam die u eerder hebt gekopieerd. (De instrumentatiesleutel kan al zijn er, afhankelijk van hoe u de App Service-app hebt gemaakt.)

   |Name  |Value  |
   |---------|---------|
   |APPINSIGHTS_INSTRUMENTATIONKEY | {instrumentatiesleutel} |

1. Vervang *{instrumentatiesleutel}* met de instrumentatiesleutel van de Application Insights-resource die u gebruikt.

1. Selecteer **Opslaan**.

1. Zodat u kunt deze lokaal uitvoeren, moet u de Application Insights-verbinding toevoegen aan het project. In de *appsettings.json* bestand, Voeg een `APPINSIGHTS_INSTRUMENTATIONKEY` veld, zoals in het volgende voorbeeld:

    ```json
    {
        "AzureWebJobsStorage": "{storage connection string}",
        "APPINSIGHTS_INSTRUMENTATIONKEY": "{instrumentation key}"
    }
    ```

    Vervang *{instrumentatiesleutel}* met de instrumentatiesleutel van de Application Insights-resource die u gebruikt.

1. Sla uw wijzigingen op.

### <a name="add-application-insights-logging-provider"></a>Toevoegen van Application Insights-provider voor logboekregistratie

Om te profiteren van [Application Insights](../azure-monitor/app/app-insights-overview.md) logboekregistratie, werk uw code logboekregistratie u het volgende doen:

* Toevoegen van een provider voor Application Insights-logboekregistratie met standaard [filteren](webjobs-sdk-how-to.md#log-filtering); alle gegevens en logboeken voor een hoger niveau gaat naar de console en de Application Insights wanneer u lokaal uitvoert.
* Plaats de `LoggerFactory` van het object in een `using` blokkeren om ervoor te zorgen dat de uitvoer moet worden leeggemaakt wanneer de host wordt afgesloten.

1. Installeer de nieuwste stabiele 3.x-versie van het NuGet-pakket voor de logboekregistratie voor Application Insights-provider: `Microsoft.Azure.WebJobs.Logging.ApplicationInsights`.

   Hier volgt de **Package Manager Console** voor versie 3.0.2 opdracht:

   ```powershell
   Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -Version 3.0.2
   ```

1. Open *Program.cs* en vervang de code in de `Main` methode met de volgende code:

    ```cs
    static void Main(string[] args)
    {
        var builder = new HostBuilder();
        builder.UseEnvironment(EnvironmentName.Development);
        builder.ConfigureWebJobs(b =>
                {
                    b.AddAzureStorageCoreServices();
                    b.AddAzureStorage();
                });
        builder.ConfigureLogging((context, b) =>
                {
                    b.AddConsole();

                    // If the key exists in settings, use it to enable Application Insights.
                    string instrumentationKey = context.Configuration["APPINSIGHTS_INSTRUMENTATIONKEY"];
                    if (!string.IsNullOrEmpty(instrumentationKey))
                    {
                        b.AddApplicationInsights(o => o.InstrumentationKey = instrumentationKey);
                    }
                });
        var host = builder.Build();
        using (host)
        {
            host.Run();
        }
    }
    ```

    Hiermee wordt de Application Insights-provider toegevoegd aan de logboekregistratie, met behulp van de sleutel die u eerder hebt toegevoegd aan uw app-instellingen.

## <a name="test-application-insights-logging"></a>Test Application Insights-logboekregistratie

In deze sectie maakt uitvoeren u lokaal opnieuw om te controleren dat logboekregistratie gegevens nu naar Application Insights evenals garantie voor de console zullen.

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

## <a name="deploy-as-a-webjob"></a>Implementeren naar Azure

Tijdens de implementatie, moet u een app service-exemplaar in voor het uitvoeren van uw functies maken. Wanneer u een .NET Core-console-app in App Service in Azure publiceert, wordt deze automatisch uitgevoerd als WebJob. Zie voor meer informatie over het publiceren, [ontwikkelen en WebJobs implementeren met Visual Studio](webjobs-dotnet-deploy-vs.md).

[!INCLUDE [webjobs-publish-net-core](../../includes/webjobs-publish-net-core.md)]

## <a name="trigger-the-function-in-azure"></a>Activeer de functie in Azure

1. Zorg ervoor dat u bent niet lokaal worden uitgevoerd (in het consolevenster sluiten als dit nog steeds geopend is). Anders mogelijk het lokale exemplaar van de eerste voor het verwerken van een Wachtrijberichten die u maakt.

1. In de **wachtrij** pagina in Visual Studio, een bericht naar de wachtrij toe te voegen als voorheen.

1. Vernieuw de **wachtrij** pagina en het nieuwe bericht verdwijnt omdat deze is verwerkt door de functie die wordt uitgevoerd in Azure.

   > [!TIP]
   > Wanneer u in Azure test, gebruikt u [Ontwikkelingsmodus](webjobs-sdk-how-to.md#host-development-settings) ervoor te zorgen dat een activeringsfunctie meteen wordt aangeroepen en voorkomen dat wordt vertraagd vanwege [wachtrij polling exponentieel uitstel](../azure-functions/functions-bindings-storage-queue.md#trigger---polling-algorithm).

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

   In deze code `queueTrigger` is een [binding expressie](../azure-functions/functions-bindings-expressions-patterns.md), wat betekent dat deze wordt omgezet naar een andere waarde tijdens runtime.  Tijdens runtime heeft de inhoud van het bericht uit de wachtrij.

1. Voeg een `using`:

   ```cs
   using System.IO;
   ```

1. Maak een blobcontainer in uw opslagaccount.

   a. In **Server Explorer** in Visual Studio, vouw het knooppunt voor uw opslagaccount, met de rechtermuisknop op **Blobs**, en selecteer vervolgens **Blob-Container maken**.

   b. In de **Blob-Container maken** dialoogvenster Voer *container* als containernaam van de en klik vervolgens op **OK**.

1. Upload de *Program.cs* -bestand naar de blob-container. (Dit bestand wordt hier gebruikt als voorbeeld; u kunt een tekstbestand uploaden en een wachtrijbericht te maken met de naam van het bestand.)

   a. In **Server Explorer**, dubbelklik op het knooppunt voor de container die u hebt gemaakt.

   b. In de **Container** venster de **uploaden** knop.

   ![Knop voor het uploaden van BLOB](./media/webjobs-sdk-get-started/blob-upload-button.png)

   c. Zoek en selecteer *Program.cs*, en selecteer vervolgens **OK**.

1. Een wachtrijbericht te maken in de wachtrij die u eerder hebt gemaakt, met *Program.cs* als de tekst van het bericht.

   ![Wachtrijbericht Program.cs](./media/webjobs-sdk-get-started/queue-msg-program-cs.png)

1. Het project lokaal uitvoeren.

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

Uitvoerbindingen vereenvoudigen code waarmee gegevens worden geschreven. In dit voorbeeld wijzigt de voorgaande build is door een kopie van de blob in plaats van logboekregistratie de grootte ervan te schrijven. BLOB storage-bindingen zijn opgenomen in het Azure Storage-uitbreidingspakket die we eerder hebt geïnstalleerd.

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

1. Het project lokaal uitvoeren.

   Bericht uit de wachtrij wordt de functie, die vervolgens wordt de blob gelezen, registreert de lengte en maakt u een nieuwe blob geactiveerd. De console-uitvoer is hetzelfde, maar wanneer u gaat u naar de blob-container-venster en selecteer **vernieuwen**, ziet u een nieuwe blob met de naam *kopie-Program.cs.*

## <a name="republish-the-updates-to-azure"></a>De updates naar Azure publiceren

1. Klik in **Solution Explorer** met de rechtermuisknop op het project en selecteer **Publiceren**.

1. In de **publiceren** dialoogvenster, zorg ervoor dat het huidige profiel is geselecteerd en kies vervolgens **publiceren**. Resultaten van het publiceren worden beschreven in de **uitvoer** venster.
 
1. De functie in Azure controleren door opnieuw een bestand uploaden naar de blob-container en het toevoegen van een bericht naar de wachtrij is de naam van het geüploade bestand. U ziet het bericht uit de wachtrij en een kopie van het bestand hebt gemaakt in de blob-container worden verwijderd. 

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd u maken, uitvoeren en implementeren van een WebJobs SDK 3.x-project.

> [!div class="nextstepaction"]
> [Meer informatie over de WebJobs SDK](webjobs-sdk-how-to.md)
