---
title: Azure Application Insights foutopsporingsprogramma momentopname voor de .NET-toepassingen | Microsoft Docs
description: Fouten opsporen in momentopnamen worden automatisch verzameld wanneer uitzonderingen worden veroorzaakt in productie .NET-toepassingen
services: application-insights
documentationcenter: 
author: pharring
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: mbullwin
ms.openlocfilehash: 8d6f2347e06e58ec2b506aa9eaf716b3f71f3a77
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a>Fouten opsporen in momentopnamen op uitzonderingen in .NET-toepassingen

Wanneer er een uitzondering optreedt, kunt u automatisch een debug-momentopname verzamelen van uw live-webtoepassing. De momentopname toont de status van de broncode en variabelen op het moment dat de uitzondering is opgetreden. De momentopname-foutopsporing (preview) in [Azure Application Insights](app-insights-overview.md) uitzonderingstelemetrie van uw web-app wordt bewaakt. Deze verzamelt momentopnamen op uw eerste ArgumentOutOfRangeException uitzonderingen zodat u de informatie die u nodig hebt voor het vaststellen van problemen in productie hebt. Bevatten de [momentopname collector NuGet-pakket](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) in uw toepassing, en desgewenst verzameling parameters in configureren [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Momentopnamen worden weergegeven op [uitzonderingen](app-insights-asp-net-exceptions.md) in de Application Insights-portal.

U kunt momentopnamen foutopsporing weergeven in de portal om te zien van de aanroep stapelen en variabelen op elke aanroepstackframe controleren. Als u een krachtigere foutopsporing ervaring met broncode, opent u momentopnamen met Visual Studio 2017 Enterprise door [downloaden van de momentopname Debugger-extensie voor Visual Studio](https://aka.ms/snapshotdebugger). In Visual Studio kunt u ook [Snappoints om interactief momentopnamen te ingesteld](https://aka.ms/snappoint) zonder te wachten op een uitzondering.

Verzameling van de momentopname is beschikbaar voor:
* .NET framework en ASP.NET-toepassingen met .NET Framework 4.5 of hoger.
* .NET core 2.0 en ASP.NET Core 2.0-toepassingen worden uitgevoerd op Windows.

De volgende omgevingen worden ondersteund:
* Azure App Service.
* Azure Cloud Service OS-familie met 4 of hoger.
* Azure Service Fabric-services met Windows Server 2012 R2 of hoger.
* Virtuele Machines in Azure waarop Windows Server 2012 R2 of hoger.
* On-premises virtuele of fysieke machines met Windows Server 2012 R2 of hoger.

> [!NOTE]
> Client-toepassingen (bijvoorbeeld WPF, Windows Forms of UWP) worden niet ondersteund.

### <a name="configure-snapshot-collection-for-aspnet-applications"></a>Momentopname verzamelen voor ASP.NET-toepassingen configureren

1. [Application Insights inschakelen in uw web-app](app-insights-asp-net.md), als u dit nog niet hebt gedaan.

2. Bevatten de [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-pakket in uw app. 

3. Bekijk de standaardopties te gebruiken die het pakket worden toegevoegd aan [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- The default is true, but you can disable Snapshot Debugging by setting it to false -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this to true. -->
        <!-- DeveloperMode is a property on the active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need to see an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>5</ThresholdForSnapshotting>
        <!-- The maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- The maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often to reset problem counters. -->
        <ProblemCounterResetInterval>06:00:00</ProblemCounterResetInterval>
        <!-- The maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>50</SnapshotsPerDayLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. Momentopnamen worden verzameld alleen op uitzonderingen die worden gerapporteerd aan de Application Insights. In sommige gevallen (bijvoorbeeld oudere versies van het .NET-platform), moet u mogelijk [uitzondering verzamelen configureren](app-insights-asp-net-exceptions.md#exceptions) om uitzonderingen met momentopnamen in de portal te bekijken.


### <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a>Momentopname verzamelen voor ASP.NET Core 2.0-toepassingen configureren

1. [Application Insights inschakelen in uw web-app van ASP.NET Core](app-insights-asp-net-core.md), als u dit nog niet hebt gedaan.

    > [!NOTE]
    > Niet zeker van te zijn dat uw toepassing verwijst naar versie 2.1.1 of nieuwere van het pakket Microsoft.ApplicationInsights.AspNetCore.

2. Bevatten de [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-pakket in uw app.

3. Wijzigen van uw toepassing `Startup` klasse toevoegen en configureren van de momentopname-Collector telemetrie processor.

   ```csharp
   using Microsoft.ApplicationInsights.SnapshotCollector;
   using Microsoft.Extensions.Options;
   ...
   class Startup
   {
       private class SnapshotCollectorTelemetryProcessorFactory : ITelemetryProcessorFactory
       {
           private readonly IServiceProvider _serviceProvider;

           public SnapshotCollectorTelemetryProcessorFactory(IServiceProvider serviceProvider) =>
               _serviceProvider = serviceProvider;

           public ITelemetryProcessor Create(ITelemetryProcessor next)
           {
               var snapshotConfigurationOptions = _serviceProvider.GetService<IOptions<SnapshotCollectorConfiguration>>();
               return new SnapshotCollectorTelemetryProcessor(next, configuration: snapshotConfigurationOptions.Value);
           }
       }

       public Startup(IConfiguration configuration) => Configuration = configuration;

       public IConfiguration Configuration { get; }

       // This method gets called by the runtime. Use this method to add services to the container.
       public void ConfigureServices(IServiceCollection services)
       {
           // Configure SnapshotCollector from application settings
           services.Configure<SnapshotCollectorConfiguration>(Configuration.GetSection(nameof(SnapshotCollectorConfiguration)));

           // Add SnapshotCollector telemetry processor.
           services.AddSingleton<ITelemetryProcessorFactory>(sp => new SnapshotCollectorTelemetryProcessorFactory(sp));

           // TODO: Add other services your application needs here.
       }
   }
   ```

4. Configureer de Collector momentopname met het toevoegen van een sectie SnapshotCollectorConfiguration aan appsettings.json. Bijvoorbeeld:

   ```json
   {
     "ApplicationInsights": {
       "InstrumentationKey": "<your instrumentation key>"
     },
     "SnapshotCollectorConfiguration": {
       "IsEnabledInDeveloperMode": true
     }
   }
   ```

### <a name="configure-snapshot-collection-for-other-net-applications"></a>Momentopname verzamelen voor andere .NET-toepassingen configureren

1. Als uw toepassing is niet al zijn geïnstrumenteerd met Application Insights, aan de slag door [Application Insights inschakelen en instellen van de instrumentatiesleutel](app-insights-windows-desktop.md).

2. Voeg de [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-pakket in uw app.

3. Momentopnamen worden verzameld alleen op uitzonderingen die worden gerapporteerd aan de Application Insights. U wilt wijzigen van uw code om te rapporteren. De verwerking van de code van uitzonderingen, is afhankelijk van de structuur van uw toepassing, maar een voorbeeld lager is dan:
    ```csharp
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle the request.
        }
        catch (Exception ex)
        {
            // Report the exception to Application Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow the exception if desired.
        }
   }
    ```
    
## <a name="grant-permissions"></a>Machtigingen verlenen

Eigenaars van het Azure-abonnement kunnen inspecteren momentopnamen. Andere gebruikers moet machtiging worden verleend door een eigenaar.

Als u wilt machtigen, wijzen de `Application Insights Snapshot Debugger` rol aan gebruikers die momentopnamen inspecteert. Deze rol kan worden toegewezen aan individuele gebruikers of groepen door abonnementseigenaren voor de Application Insights-resource van het doel of de resourcegroep of abonnement.

1. Open de blade Access Control (IAM).
1. Klik op de knop toevoegen +.
1. Selecteer Application Insights momentopname foutopsporingsprogramma in de vervolgkeuzelijst rollen.
1. Zoeken en geef een naam voor de gebruiker toe te voegen.
1. Klik op de knop Opslaan om de gebruiker toevoegen aan de rol.


> [!IMPORTANT]
> Momentopnamen kunnen mogelijk persoonlijke en andere gevoelige gegevens in waarden van variabelen en parameter bevatten.

## <a name="debug-snapshots-in-the-application-insights-portal"></a>Fouten opsporen in momentopnamen in de Application Insights-portal

Als een momentopname beschikbaar voor een bepaalde uitzondering of een probleem-ID is, een **fouten opsporen in momentopname openen** knop wordt weergegeven op de [uitzondering](app-insights-asp-net-exceptions.md) in de Application Insights-portal.

![De knop openen fouten opsporen in momentopname op uitzondering](./media/app-insights-snapshot-debugger/snapshot-on-exception.png)

In de weergave fouten opsporen in momentopname ziet u een aanroepstack en een deelvenster Variabelen. Wanneer u frames aanroepstack in het deelvenster van de stack aanroep selecteert, kunt u lokale variabelen weergeven en parameters voor die functie-aanroep in het deelvenster Variabelen.

![Weergave fouten opsporen in momentopname in de portal](./media/app-insights-snapshot-debugger/open-snapshot-portal.png)

Momentopnamen kunnen gevoelige gegevens bevatten, en ze zijn standaard niet zichtbaar. Momentopnamen kunnen weergeven, hebt u de `Application Insights Snapshot Debugger` rol aan u toegewezen.

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a>Fouten opsporen in momentopnamen met Visual Studio 2017 Enterprise
1. Klik op de **momentopname downloaden** knop voor het downloaden van een `.diagsession` bestand, dat kan worden geopend door Visual Studio 2017 Enterprise. 

2. Openen van de `.diagsession` -bestand, moet u eerst [downloaden en installeren van de momentopname Debugger-extensie voor Visual Studio](https://aka.ms/snapshotdebugger).

3. Nadat u de momentopnamebestand opent, verschijnt de pagina met Mini-geheugendump foutopsporing in Visual Studio. Klik op **fouten opsporen in beheerde Code** momentopname van de foutopsporing te starten. De momentopname is geopend met de coderegel waar de uitzondering is opgetreden, zodat u fouten in de huidige status van het proces opsporen kunt.

    ![Weergave foutopsporing momentopname in Visual Studio](./media/app-insights-snapshot-debugger/open-snapshot-visualstudio.png)

De gedownloade momentopname bevat symboolbestanden die zijn gevonden op de webserver van de toepassing. Deze symboolbestanden zijn om momentopnamegegevens te koppelen met broncode vereist. Zorg dat u de implementatie van het symbool in te schakelen wanneer u uw web-apps publiceren voor App Service-apps.

## <a name="how-snapshots-work"></a>De werking van momentopnamen

Wanneer uw toepassing wordt gestart, moet op een afzonderlijke momentopname uploader proces dat uw toepassing voor aanvragen van de momentopname bewaakt wordt gemaakt. Wanneer een momentopname wordt aangevraagd, wordt een schaduwkopie van het actieve proces aangebracht in milliseconden ongeveer 10 tot 20. Het proces van de schaduw wordt vervolgens geanalyseerd en een momentopname wordt gemaakt terwijl het hoofdproces nog steeds worden uitgevoerd en verkeer leveren aan gebruikers. De momentopname wordt vervolgens geüpload naar Application Insights samen met eventuele relevante (.pdb)-symboolbestanden die nodig zijn om de momentopname weer te geven.

## <a name="current-limitations"></a>Huidige beperkingen

### <a name="publish-symbols"></a>Symbolen publiceren
Het foutopsporingsprogramma momentopname vereist symboolbestanden op de productieserver variabelen decoderen en om een foutopsporing ervaring in Visual Studio te bieden. De 15.2 versie van Visual Studio 2017 publiceert symbolen voor release builds standaard wanneer deze wordt gepubliceerd naar App Service. In eerdere versies, moet u de volgende regel toevoegen aan uw publicatieprofiel `.pubxml` zodat symbolen zijn gepubliceerd in de releasemodus bestand:

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

Voor Azure Compute en andere typen, zorg ervoor dat de symboolbestanden in dezelfde map van de hoofdtoepassing .dll (meestal `wwwroot/bin`) of zijn beschikbaar op het huidige pad.

### <a name="optimized-builds"></a>Geoptimaliseerde builds
In sommige gevallen kunnen niet lokale variabelen worden weergegeven in de release builds vanwege optimalisaties die worden toegepast tijdens het maken.

## <a name="troubleshooting"></a>Problemen oplossen

De volgende tips helpen u problemen oplossen met de momentopname-foutopsporing.

### <a name="verify-the-instrumentation-key"></a>Controleer of de instrumentatiesleutel

Zorg ervoor dat u de juiste instrumentatiesleutel in uw gepubliceerde toepassing. Application Insights wordt normaal gesproken de instrumentatiesleutel gelezen uit het bestand ApplicationInsights.config. Controleer of de waarde is hetzelfde als de instrumentatiesleutel voor de Application Insights-resource die u in de portal ziet.

### <a name="check-the-uploader-logs"></a>Raadpleeg de logboeken uploader

Nadat een momentopname is gemaakt, wordt een bestand met Mini-geheugendump (dmp) op schijf gemaakt. Een afzonderlijke uploader proces duurt dat minidump-bestand en uploadt, samen met eventuele bijbehorende-PDB-bestanden naar Application Insights momentopname foutopsporingsprogramma opslag. Nadat de minidump heeft geüpload, wordt deze verwijderd van de schijf. De logboekbestanden voor de uploader minidump worden bewaard op de schijf. In een App Service-omgeving vindt u deze logboeken in `D:\Home\LogFiles\Uploader_*.log`. Met de site voor het beheer van Kudu voor App Service kunt u deze logboekbestanden.

1. Open uw App Service-toepassing in de Azure portal.

2. Selecteer de **geavanceerde hulpmiddelen** blade of zoeken naar **Kudu**.
3. Klik op **gaat**.
4. In de **-console voor foutopsporing** vervolgkeuzelijst de optie **CMD**.
5. Klik op **logboekbestanden**.

Er is ten minste één bestand met een naam die met begint `Uploader_` en een `.log` extensie. Klik op het juiste pictogram om te downloaden van alle logboekbestanden of in een browser openen.
De bestandsnaam bevat de naam van de machine. Als uw App Service-exemplaar wordt gehost op meer dan één computer, moet u er aparte logboekbestanden voor elke computer zijn. Wanneer de uploader een nieuw minidump-bestand detecteert, wordt deze vastgelegd in het logboekbestand. Hier volgt een voorbeeld van een geslaagde upload:

```
MinidumpUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:08.0349846Z
MinidumpUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 329.12 MB
    DateTime=2017-05-25T14:25:16.0145444Z
MinidumpUploader.exe Information: 0 : Upload successful.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2017-05-25T14:25:44.2310982Z
MinidumpUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2017-05-25T14:25:44.5435948Z
MinidumpUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:44.6095821Z
```

In het vorige voorbeeld de instrumentatiesleutel wordt `c12a605e73c44346a984e00000000000`. Deze waarde moet overeenkomen met de instrumentatiesleutel voor uw toepassing.
De minidump is gekoppeld aan een momentopname met de ID `139e411a23934dc0b9ea08a626db16c5`. U kunt deze ID later te vinden van de bijbehorende uitzonderingstelemetrie in Application Insights Analytics gebruiken.

De uploader scant op nieuwe-PDB-bestanden over om de 15 minuten. Hier volgt een voorbeeld:

```
MinidumpUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\home\site\wwwroot\ for local PDBs.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\local\Temporary ASP.NET Files\root\a6554c94\e3ad6f22\assembly\dl3\81d5008b\00b93cc8_dec5d201 for local PDBs.
    DateTime=2017-05-25T15:11:38.8160276Z
MinidumpUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2017-05-25T15:11:38.8316450Z
MinidumpUploader.exe Information: 0 : Deleted PDB scan marker D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\.pdbscan.
    DateTime=2017-05-25T15:11:38.8316450Z
```

Voor toepassingen die zijn _niet_ gehost in App Service, de uploader-logboeken zijn in dezelfde map als de minidumps: `%TEMP%\Dumps\<ikey>` (waarbij `<ikey>` is uw instrumentatiesleutel).

### <a name="troubleshooting-cloud-services"></a>Problemen met Cloudservices oplossen
Voor rollen in Cloudservices, de tijdelijke standaardmap mogelijk te klein voor de bestanden met Mini-geheugendump, wat leidt tot verloren momentopnamen.
De ruimte die nodig zijn, is afhankelijk van de totale werkset van uw toepassing en het aantal gelijktijdige momentopnamen.
De werkset van een 32-bits ASP.NET-Webrol is meestal tussen 200 MB en 500 MB.
U moet voor ten minste twee gelijktijdige momentopnamen toestaan.
Bijvoorbeeld, als uw toepassing gebruikmaakt van 1 GB van totale werkset, moet u zorgen dat er ten minste 2 GB aan schijfruimte voor het opslaan van momentopnamen is.
Volg deze stappen voor uw Cloudservice rol configureren met een specifieke lokale bron voor momentopnamen.

1. Een nieuwe lokale resource toevoegen aan uw Cloud-Service door de Service in de Cloud-definitiebestand (.csdf) te bewerken. Het volgende voorbeeld definieert een resource aangeroepen `SnapshotStore` met een grootte van 5 GB.
```xml
   <LocalResources>
     <LocalStorage name="SnapshotStore" cleanOnRoleRecycle="false" sizeInMB="5120" />
   </LocalResources>
```

2. Wijzigen van uw rol `OnStart` methode voor het toevoegen van een omgevingsvariabele die naar verwijst de `SnapshotStore` lokale resource.
```csharp
   public override bool OnStart()
   {
       Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
       return base.OnStart();
   }
```

3. Bijwerken van uw rol ApplicationInsights.config-bestand voor het overschrijven van de locatie van de tijdelijke map die wordt gebruikt door`SnapshotCollector`
```xml
  <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Use the SnapshotStore local resource for snapshots -->
      <TempFolder>%SNAPSHOTSTORE%</TempFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
  </TelemetryProcessors>
```

### <a name="use-application-insights-search-to-find-exceptions-with-snapshots"></a>Gebruik Application Insights zoeken uitzonderingen met momentopnamen

Wanneer een momentopname wordt gemaakt, is de activerend uitzondering gemarkeerd met een momentopname-ID. Wanneer de uitzonderingstelemetrie wordt gemeld naar Application Insights dat momentopname-ID is opgenomen als een aangepaste eigenschap. Met de Search-blade in Application Insights en u vindt alle telemetrie met de `ai.snapshot.id` aangepaste eigenschap.

1. Blader naar uw Application Insights-resource in de Azure-portal.
2. Klik op **Search**.
3. Type `ai.snapshot.id` in het zoekvak en druk op Enter.

![Zoeken naar telemetrie met een momentopname-ID in de portal](./media/app-insights-snapshot-debugger/search-snapshot-portal.png)

Als deze zoekopdracht geen resultaten oplevert, zijn wordt er zijn geen momentopnamen gerapporteerd, naar Application Insights voor uw toepassing in de geselecteerde periode.

Als u wilt zoeken naar een specifieke momentopname-ID van de logboeken Uploader, typt u die ID in het zoekvak. Als u geen telemetrie voor een momentopname waarvan u weet dat is geüpload vinden, voert u de volgende stappen uit:

1. Controleer dat u op zoek bent op de juiste Application Insights-resource door te controleren of de instrumentatiesleutel.

2. Met behulp van de tijdstempel van het logboek Uploader, het filter tijdsbereik van de zoekopdracht omvatten die tijdsbereik aanpassen.

Als u een uitzondering met die ID momentopname niet ziet, is niet de uitzonderingstelemetrie gerapporteerd naar Application Insights. Deze situatie kan zich voordoen als uw toepassing is vastgelopen nadat de momentopname die het heeft, maar voordat de uitzonderingstelemetrie gemeld. In dit geval, controleert u de App Service-Logboeken onder `Diagnose and solve problems` om te zien of er onverwachte opnieuw wordt opgestart zijn of onverwerkte uitzonderingen.

## <a name="next-steps"></a>Volgende stappen

* [Snappoints instellen in uw code](https://docs.microsoft.com/visualstudio/debugger/debug-live-azure-applications) momentopnamen ophalen zonder te wachten op een uitzondering.
* [Diagnose van uitzonderingen in uw web-apps](app-insights-asp-net-exceptions.md) wordt uitgelegd hoe u meer uitzonderingen zichtbaar maken voor Application Insights. 
* [Detectie van smartcard](app-insights-proactive-diagnostics.md) detecteert automatisch afwijkingen.
