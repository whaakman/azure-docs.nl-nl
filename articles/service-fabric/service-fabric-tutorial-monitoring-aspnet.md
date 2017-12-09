---
title: Controle en diagnostische gegevens voor ASP.NET Core services in Azure Service Fabric | Microsoft Docs
description: Informatie over het instellen van controle en diagnostische gegevens voor een toepassing met Azure Service Fabric ASP.NET Core.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/14/2017
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: ce854a3dc41dec69c3f8de245a03d55a2354335f
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="monitor-and-diagnose-an-aspnet-core-application-on-service-fabric"></a>Controle en diagnose van een toepassing ASP.NET Core in Service Fabric
Deze zelfstudie is een vierde deel van een reeks. Deze doorloopt stappen voor het instellen van controle en diagnostische gegevens voor een ASP.NET Core-toepassing uitgevoerd op een Service Fabric-cluster met behulp van Application Insights. Telemetrie van de toepassing die is ontwikkeld in het eerste deel van de zelfstudie wordt verzameld [een .NET-Service Fabric-toepassing bouwen](service-fabric-tutorial-create-dotnet-app.md). 

In de vierde deel van de reeks zelfstudie leert u hoe:
> [!div class="checklist"]
> * Application Insights configureren voor uw toepassing
> * Antwoord telemetrie te traceren op basis van HTTP-communicatie tussen services verzamelen
> * Gebruik de functie App-kaart in Application Insights
> * Aangepaste gebeurtenissen met de Application Insights-API toevoegen

In deze zelfstudie reeks leert u hoe:
> [!div class="checklist"]
> * [Een .NET-Service Fabric-toepassing bouwen](service-fabric-tutorial-create-dotnet-app.md)
> * [De toepassing naar een externe-cluster implementeren](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [CI/CD met behulp van Visual Studio Team Services configureren](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * Controle en diagnostische gegevens voor de toepassing instellen

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint:
- Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Installeer Visual Studio 2017](https://www.visualstudio.com/) en installeer de **ontwikkelen van Azure** en **ASP.NET en web ontwikkeling** werkbelastingen.
- [De Service Fabric SDK installeren](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a>De voorbeeldtoepassing Voting downloaden
Als u hebt niet de voorbeeldtoepassing Voting in [deel uitmaken van een van deze zelfstudie reeks](service-fabric-tutorial-create-dotnet-app.md), u kunt dit downloaden. Voer de volgende opdracht om een kloon de opslagplaats van de voorbeeld-app op uw lokale computer in een opdrachtvenster of terminal.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="set-up-an-application-insights-resource"></a>Een Application Insights-resource instellen
Application Insights verkeert momenteel platform voor prestaties van Azure-toepassing en Service-Fabric aanbevolen platform voor bewaking en diagnostische gegevens. Voor het maken van een Application Insights-resource, gaat u naar [Azure-portal](https://portal.azure.com). Klik op **nieuw** in het linkernavigatievenster-menu om Azure Marketplace te openen. Klik op **bewaking + Management** en vervolgens **Application Insights**.

![Nieuwe AI-resource maken](./media/service-fabric-tutorial-monitoring-aspnet/new-ai-resource.png)

U moet nu om de vereiste informatie over de kenmerken van de bron moet worden gemaakt in te vullen. Voer een juiste *naam*, *resourcegroep*, en *abonnement*. Stel de *locatie* waar u in de toekomst Service Fabric-cluster wilt implementeren. In deze zelfstudie wordt we de app implementeren op een lokaal cluster zodat de *locatie* veld is niet relevant. De *toepassingstype* te laten als 'ASP.NET web application'. 

![AI resourcekenmerken](./media/service-fabric-tutorial-monitoring-aspnet/new-ai-resource-attrib.png)

Nadat u de vereiste gegevens hebt ingevuld, klikt u op **maken** voor het inrichten van de resource - het duurt ongeveer een minuut. 
<!-- When completed, navigate to the newly deployed resource, and find the "Instrumentation Key" (visible in the "Essentials" drop down section). Copy it to clipboard, since we will need it in the next step. -->

## <a name="add-application-insights-to-the-applications-services"></a>Application Insights toevoegen aan de toepassing services

Start Visual Studio 2017 met verhoogde bevoegdheden. U kunt dit doen door met de rechtermuisknop op het pictogram Visual Studio in het Menu Start en kiezen **als administrator uitvoeren**. Klik op **bestand** > **Open** > **Project/oplossing** en navigeer naar de toepassing Voting (ofwel gemaakt in deel een van de zelfstudie of git gekloonde). Open *Voting.sln* als u wordt gevraagd om terug te zetten van de toepassing NuGet-pakketten, klikt u op **Ja**.

Volg deze stappen voor het configureren van Application Insights voor VotingWeb- en VotingData services:
1. Met de rechtermuisknop op de naam van de service en klik op **Application Insights configureren...** .

    ![AI configureren](./media/service-fabric-tutorial-monitoring-aspnet/configure-ai.png)

2. Klik op **Start gratis**.
3. Aanmelden bij uw account (waarmee u ook instellen uw Azure-abonnement) en selecteer het abonnement waarin u de Application Insights-resource gemaakt. De bron onder vinden *bestaande Application Insights-resource* in de vervolgkeuzelijst 'Resource'. Klik op **registreren** Application Insights toevoegen aan uw service.

    ![AI registreren](./media/service-fabric-tutorial-monitoring-aspnet/register-ai.png)

4. Klik op **voltooien** zodra het dialoogvenster dat verschijnt de bewerking is voltooid.
    
Zorg ervoor dat te doen in de bovenstaande stappen **beide** van de services in de toepassing Application Insights configureren voor de toepassing. Dezelfde Application Insights-resource wordt gebruikt voor zowel de services om te zien binnenkomende en uitgaande aanvragen en communicatie tussen de services.

## <a name="add-the-microsoftapplicationinsightsservicefabricnative-nuget-to-the-services"></a>Het Microsoft.ApplicationInsights.ServiceFabric.Native NuGet toevoegen aan de services

Application Insights heeft twee Service Fabric specifieke NuGets die kunnen worden gebruikt, afhankelijk van het scenario. Een wordt met systeemeigen van Service Fabric-services en de andere met containers en Gast uitvoerbare bestanden gebruikt. In dit geval worden gebruikt het Microsoft.ApplicationInsights.ServiceFabric.Native NuGet gebruikmaken van de kennis van de servicecontext die wordt gebracht. Voor meer informatie over de Application Insights-SDK en de Service Fabric-specifieke NuGets, Zie [Microsoft Application Insights voor Service Fabric](https://github.com/Microsoft/ApplicationInsights-ServiceFabric/blob/develop/README.md). 

Hier volgen de stappen voor het instellen van het NuGet:
1. Met de rechtermuisknop op de **oplossing 'Stem'** aan de bovenkant van uw Solution Explorer en klik op **NuGet-pakketten beheren voor oplossing...** .
2. Klik op **Bladeren** in het menu van de bovenste navigatiebalk van het venster 'NuGet--oplossing' en controleer de **Include prerelease** in naast de zoekbalk.
3. Zoeken naar `Microsoft.ApplicationInsights.ServiceFabric.Native` en klik op het juiste NuGet-pakket.
4. Klik op het recht van de twee selectievakjes naast de twee services in de toepassing **VotingWeb** en **VotingData** en klik op **installeren**.
    ![AI-inschrijving is voltooid](./media/service-fabric-tutorial-monitoring-aspnet/aisdk-sf-nuget.png)
5. Klik op **OK** op de *wijzigingen* dialoogvenster dat verschijnt en accepteer de *licentie acceptatie*. Dit voltooit de NuGet toe te voegen aan de services.
6. Nu moet u voor het instellen van de initialisatiefunctie telemetrie in de twee services. Hiertoe opent u *VotingWeb.cs* en *VotingData.cs*. Voor beide parameters, volgt u de volgende twee stappen:
    1. Deze twee toevoegen *met* instructies aan het begin van elke  *\<ServiceName > .cs*:
    
    ```csharp
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.ServiceFabric;
    ```
    
    2. In de geneste *retourneren* statusverklaring *CreateServiceInstanceListeners()* of *CreateServiceReplicaListeners()*onder *ConfigureServices*  >  *services*, tussen de twee Singleton-services gedeclareerd, toevoegen: `.AddSingleton<ITelemetryInitializer>((serviceProvider) => FabricTelemetryInitializerExtension.CreateFabricTelemetryInitializer(serviceContext))`.
    Hiermee voegt u toe de *Service Context* naar uw telemetrie, zodat u beter inzicht in de bron van uw telemetrie in Application Insights. De geneste *retourneren* -instructie in *VotingWeb.cs* ziet er als volgt:
    
    ```csharp
    return new WebHostBuilder().UseWebListener()
        .ConfigureServices(
            services => services
                .AddSingleton<StatelessServiceContext>(serviceContext)
                .AddSingleton<ITelemetryInitializer>((serviceProvider) => FabricTelemetryInitializerExtension.CreateFabricTelemetryInitializer(serviceContext))
                .AddSingleton<HttpClient>())
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseStartup<Startup>()
        .UseApplicationInsights()
        .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
        .UseUrls(url)
        .Build();
    ```

    Op vergelijkbare wijze *VotingData.cs*, u moet beschikken over:

    ```csharp
    return new WebHostBuilder()
        .UseKestrel()
        .ConfigureServices(
            services => services
                .AddSingleton<StatefulServiceContext>(serviceContext)
                .AddSingleton<ITelemetryInitializer>((serviceProvider) => FabricTelemetryInitializerExtension.CreateFabricTelemetryInitializer(serviceContext))
                .AddSingleton<IReliableStateManager>(this.StateManager))
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseStartup<Startup>()
        .UseApplicationInsights()
        .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
        .UseUrls(url)
        .Build();
    ```

Op dit moment bent u klaar om de toepassing te implementeren. Klik op **Start** boven (of **F5**), Visual Studio bouwt en de toepassing van het pakket, instellen van uw lokale cluster, en de toepassing te implementeren. 

Als de toepassing is voltooid implementeren, head via naar [localhost:8080](localhost:8080)waar u zich kunt zien, de stemmen één pagina voorbeeldtoepassing. Voor enkele andere items die van uw keuze voor het maken van sommige sample data en telemetrie - ik heb voor desserts stemmen!

![AI voorbeeld stemmen](./media/service-fabric-tutorial-monitoring-aspnet/vote-sample.png)

Gerust *verwijderen* enkele van de stemmende opties ook wanneer u klaar bent voor het toevoegen van een paar stemmen.

## <a name="view-telemetry-and-the-app-map-in-application-insights"></a>Telemetrie van paginaweergaven en de kaart App in Application Insights 

HEAD over naar uw Application Insights-resource in Azure-portal en in de linkernavigatiebalk van de resource, klikt u op **Previews** onder *configureren*. Schakel **op** de *toepassing van meerdere rollenoverzicht* in de lijst met beschikbare previews.

![AI inschakelen AppMap](./media/service-fabric-tutorial-monitoring-aspnet/ai-appmap-enable.png)

Klik op **overzicht** om terug te gaan naar de landingspagina van de bron. Klik vervolgens op **Search** in de rechterbovenhoek om te zien van de traceringen binnenkort in. Het duurt enkele minuten duren voordat de traceringen in Application Insights weergegeven. In het geval dat u niet een, even wacht en klik op de **vernieuwen** bovenaan op de knop.
![AI Zie traceringen](./media/service-fabric-tutorial-monitoring-aspnet/ai-search.png)

Schuiven naar beneden op het *Search* venster ziet u de binnenkomende telemetrie krijgt u een out-of-the-box met Application Insights. Voor elke actie die u in de toepassing Voting duurde, moet er een uitgaande PUT-aanvraag van *VotingWeb* (plaatsen stemmen/Put [name]), een binnenkomende PUT-aanvraag van *VotingData* (plaatsen VoteData/Put [naam]), gevolgd door een combinatie van GET-aanvragen voor het vernieuwen van de gegevens worden weergegeven. Bovendien moet er een afhankelijkheid tracering voor HTTP op localhost, aangezien die HTTP-aanvragen. Hier volgt een voorbeeld van wat u voor hoe een stem ziet is toegevoegd: ![aanvraagtracering AI-voorbeeld](./media/service-fabric-tutorial-monitoring-aspnet/sample-request.png)

U kunt klikken op een van de traceringen om meer informatie over deze weer te geven. Er is nuttige informatie over de aanvraag die is opgegeven door de Application Insights, met inbegrip van de *reactietijd* en de *aanvraag-URL*. Bovendien, omdat u het Service Fabric-specifieke NuGet hebt toegevoegd, ook krijgt u gegevens over uw toepassing in de context van een Service Fabric-cluster in de *aangepaste gegevens* hieronder. Dit omvat de servicecontext, zodat u ziet de *PartitionID* en *ReplicaId* lokaliseren van de bron van de aanvraag en betere problemen bij het opsporen van fouten in uw toepassing.

![AI trace-details](./media/service-fabric-tutorial-monitoring-aspnet/trace-details.png)

Bovendien, aangezien we de App-kaart ingeschakeld op de *overzicht* pagina te klikken op de **App kaart** pictogram ziet u zowel uw services verbonden.

![AI trace-details](./media/service-fabric-tutorial-monitoring-aspnet/app-map.png)

De kaart App kunt u beter inzicht in de Toepassingstopologie van uw vooral als u begint met het toevoegen van meerdere verschillende services die samenwerken. Ook krijgt u basisgegevens op aanvraag succespercentages en kunt u diagnosticeren van mislukte aanvragen om te begrijpen wanneer dingen kunnen hebben een fout opgetreden. Zie voor meer informatie over het gebruik van de App-kaart, [toepassingstoewijzing in Application Insights](../application-insights/app-insights-app-map.md).

## <a name="add-custom-instrumentation-to-your-application"></a>Aangepaste instrumentation aan uw toepassing toevoegen

Hoewel Application Insights veel telemetrie uit de verpakking biedt, moet u kunt aangepaste instrumentation meer toevoegen. Dit kan op basis van de behoeften van uw bedrijf of voor het verbeteren van diagnostische gegevens bij problemen in uw toepassing. Application Insights is een API om op te nemen aangepaste gebeurtenissen en metrische gegevens vindt u meer over [hier](../application-insights/app-insights-api-custom-events-metrics.md).

Laten we een aantal aangepaste gebeurtenissen naar toevoegen *VoteDataController.cs* (onder *VotingData* > *domeincontrollers*) om bij te houden wanneer stemmen worden toegevoegd en verwijderd uit de onderliggende *votesDictionary*. 
1. Voeg `using Microsoft.ApplicationInsights;` aan het einde van de andere using-instructies.
2. Een nieuwe declareren *TelemetryClient* aan het begin van de klasse, onder het maken van de *IReliableStateManager*: `private TelemetryClient telemetry = new TelemetryClient();`.
3. In de *Put()* werken, het toevoegen van een gebeurtenis die wordt bevestigd een stem dat is toegevoegd. Voeg `telemetry.TrackEvent($"Added a vote for {name}");` nadat de transactie is voltooid, voordat de retour-rechts *OkResult* instructie.
4. In *Delete()*, er is een ' if/else' gebaseerd op voorwaarde dat de *votesDictionary* stemmen voor een bepaalde optie bevat. 
    1. Toevoegen van een gebeurtenis dat de verwijdering van een stem in de *als* instructie na de *await tx.CommitAsync()*:`telemetry.TrackEvent($"Deleted votes for {name}");`
    2. Toevoegen van een gebeurtenis om weer te geven dat de verwijdering niet heeft plaatsgevonden de *anders* voordat de instructie return-instructie:`telemetry.TrackEvent($"Unable to delete votes for {name}, voting option not found");`

Hier volgt een voorbeeld van wat uw *Put()* en *Delete()* functies eruit als na het toevoegen van de gebeurtenissen:

```csharp
// PUT api/VoteData/name
[HttpPut("{name}")]
public async Task<IActionResult> Put(string name)
{
    var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

    using (ITransaction tx = this.stateManager.CreateTransaction())
    {
        await votesDictionary.AddOrUpdateAsync(tx, name, 1, (key, oldvalue) => oldvalue + 1);
        await tx.CommitAsync();
    }

    telemetry.TrackEvent($"Added a vote for {name}");
    return new OkResult();
}

// DELETE api/VoteData/name
[HttpDelete("{name}")]
public async Task<IActionResult> Delete(string name)
{
    var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

    using (ITransaction tx = this.stateManager.CreateTransaction())
    {
        if (await votesDictionary.ContainsKeyAsync(tx, name))
        {
            await votesDictionary.TryRemoveAsync(tx, name);
            await tx.CommitAsync();
            telemetry.TrackEvent($"Deleted votes for {name}");
            return new OkResult();
        }
        else
        {
            telemetry.TrackEvent($"Unable to delete votes for {name}, voting option not found");
            return new NotFoundResult();
        }
    }
}
```

Als u klaar bent u deze wijzigingen aanbrengt, **Start** de toepassing zo dat deze maakt en implementeert de nieuwste versie van het. Als de toepassing is voltooid implementeren, head via naar [localhost:8080](localhost:8080), toevoegen en verwijderen van een aantal opties voor stemmen. Ga vervolgens terug naar uw Application Insights-resource voor een overzicht van de traceringen voor de meest recente uitvoeren (zoals voordat, traceringen min 1-2 kunnen worden weergegeven in Application Insights). Voor alle de stemmen u toegevoegd en verwijderd, ziet u nu een 'aangepaste gebeurtenis * samen met alle telemetrie van het antwoord. 

![aangepaste gebeurtenissen](./media/service-fabric-tutorial-monitoring-aspnet/custom-events.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:
> [!div class="checklist"]
> * Application Insights configureren voor uw toepassing
> * Antwoord telemetrie te traceren op basis van HTTP-communicatie tussen services verzamelen
> * Gebruik de functie App-kaart in Application Insights
> * Aangepaste gebeurtenissen met de Application Insights-API toevoegen

Nu dat u instellen van controle en diagnostische gegevens voor uw ASP.NET-toepassing hebt voltooid, probeert u het volgende:
- [Controle en diagnostische gegevens in Service Fabric verkennen](service-fabric-diagnostics-overview.md)
- [Analyse van de service Fabric-gebeurtenis met Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
- Zie voor meer informatie over Application Insights, [Application Insights-documentatie](https://docs.microsoft.com/azure/application-insights/)
