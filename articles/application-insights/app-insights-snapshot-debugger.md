---
title: Azure Application Insights foutopsporingsprogramma momentopname voor de .NET-toepassingen | Microsoft Docs
description: Fouten opsporen in momentopnamen worden automatisch verzameld wanneer uitzonderingen worden veroorzaakt in productie .NET-toepassingen
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 05/08/2018
ms.reviewer: pharring
ms.author: mbullwin
ms.openlocfilehash: b180c7e8d26acc86aa1d1982ace92efafa85f9ef
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37115995"
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a>Fouten opsporen in momentopnamen op uitzonderingen in .NET-toepassingen

Wanneer er een uitzondering optreedt, kunt u automatisch een debug-momentopname verzamelen van uw live-webtoepassing. De momentopname toont de status van de broncode en variabelen op het moment dat de uitzondering is opgetreden. De momentopname-foutopsporing (preview) in [Azure Application Insights](app-insights-overview.md) uitzonderingstelemetrie van uw web-app wordt bewaakt. Deze verzamelt momentopnamen op uw eerste ArgumentOutOfRangeException uitzonderingen zodat u de informatie die u nodig hebt voor het vaststellen van problemen in productie hebt. Bevatten de [momentopname collector NuGet-pakket](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) in uw toepassing, en desgewenst verzameling parameters in configureren [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Momentopnamen worden weergegeven op [uitzonderingen](app-insights-asp-net-exceptions.md) in de Application Insights-portal.

U kunt foutopsporingsmomentopnamen in het portaal bekijken om de aanroepstack te zien en variabelen inspecteren bij elk aanroepstackframe. Als u een krachtigere foutopsporing ervaring met broncode, opent u momentopnamen met Visual Studio 2017 Enterprise door [downloaden van de momentopname Debugger-extensie voor Visual Studio](https://aka.ms/snapshotdebugger). In Visual Studio, kunt u ook [Snappoints om interactief momentopnamen te ingesteld](https://aka.ms/snappoint) zonder te wachten op een uitzondering.

Verzameling van de momentopname is beschikbaar voor:
* .NET framework en ASP.NET-toepassingen met .NET Framework 4.5 of hoger.
* .NET core 2.0 en ASP.NET Core 2.0-toepassingen worden uitgevoerd op Windows.

De volgende omgevingen worden ondersteund:
* Azure App Service.
* Azure Cloud Service OS-familie met 4 of hoger.
* Azure Service Fabric-services met Windows Server 2012 R2 of hoger.
* Virtuele Machines in Azure waarop Windows Server 2012 R2 of hoger.
* Lokale virtuele of fysieke machines met Windows Server 2012 R2 of hoger.

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
        <ThresholdForSnapshotting>1</ThresholdForSnapshotting>
        <!-- The maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- The maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often we reconnect to the stamp. The default value is 15 minutes.-->
        <ReconnectInterval>00:15:00</ReconnectInterval>
        <!-- How often to reset problem counters. -->
        <ProblemCounterResetInterval>1.00:00:00</ProblemCounterResetInterval>
        <!-- The maximum number of snapshots allowed in ten minutes.The default value is 1. -->
        <SnapshotsPerTenMinutesLimit>1</SnapshotsPerTenMinutesLimit>
        <!-- The maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>30</SnapshotsPerDayLimit>
        <!-- Whether or not to collect snapshot in low IO priority thread. The default value is true. -->
        <SnapshotInLowPriorityThread>true</SnapshotInLowPriorityThread>
        <!-- Agree to send anonymous data to Microsoft to make this product better. -->
        <ProvideAnonymousTelemetry>true</ProvideAnonymousTelemetry>
        <!-- The limit on the number of failed requests to request snapshots before the telemetry processor is disabled. -->
        <FailedRequestLimit>3</FailedRequestLimit>
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

    Voeg de volgende using-instructies naar `Startup.cs`

   ```csharp
   using Microsoft.ApplicationInsights.SnapshotCollector;
   using Microsoft.Extensions.Options;
   using Microsoft.ApplicationInsights.AspNetCore;
   using Microsoft.ApplicationInsights.Extensibility;
   ```

   Voeg de volgende `SnapshotCollectorTelemetryProcessorFactory` klasse naar `Startup` klasse.

   ```csharp
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
       ...
    ```
    Voeg de `SnapshotCollectorConfiguration` en `SnapshotCollectorTelemetryProcessorFactory` services voor het starten van de pijplijn:

    ```csharp
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
       "IsEnabledInDeveloperMode": false,
       "ThresholdForSnapshotting": 1,
       "MaximumSnapshotsRequired": 3,
       "MaximumCollectionPlanSize": 50,
       "ReconnectInterval": "00:15:00",
       "ProblemCounterResetInterval":"1.00:00:00",
       "SnapshotsPerTenMinutesLimit": 1,
       "SnapshotsPerDayLimit": 30,
       "SnapshotInLowPriorityThread": true,
       "ProvideAnonymousTelemetry": true,
       "FailedRequestLimit": 3
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

1. Navigeer naar de Application Insights-resource in de Azure portal.
1. Klik op **toegangsbeheer (IAM)**.
1. Klik op de **+ toevoegen** knop.
1. Selecteer **Application Insights momentopname foutopsporingsprogramma** van de **rollen** vervolgkeuzelijst.
1. Zoeken en geef een naam voor de gebruiker toe te voegen.
1. Klik op de **opslaan** om toe te voegen van de gebruiker aan de rol.


> [!IMPORTANT]
> Momentopnamen kunnen mogelijk persoonlijke en andere gevoelige gegevens in waarden van variabelen en parameter bevatten.

## <a name="debug-snapshots-in-the-application-insights-portal"></a>Fouten opsporen in momentopnamen in de Application Insights-portal

Als een momentopname beschikbaar voor een bepaalde uitzondering of een probleem-ID is, een **fouten opsporen in momentopname openen** knop wordt weergegeven op de [uitzondering](app-insights-asp-net-exceptions.md) in de Application Insights-portal.

![De knop openen fouten opsporen in momentopname op uitzondering](./media/app-insights-snapshot-debugger/snapshot-on-exception.png)

In de weergave fouten opsporen in momentopname ziet u een aanroepstack en een deelvenster Variabelen. Wanneer u frames aanroepstack in het deelvenster van de stack aanroep selecteert, kunt u lokale variabelen weergeven en parameters voor die functie-aanroep in het deelvenster Variabelen.

![Weergave fouten opsporen in momentopname in de portal](./media/app-insights-snapshot-debugger/open-snapshot-portal.png)

Momentopnamen kunnen gevoelige gegevens bevatten en standaard deze ook niet kan worden weergegeven. Momentopnamen kunnen weergeven, hebt u de `Application Insights Snapshot Debugger` rol aan u toegewezen.

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a>Fouten opsporen in momentopnamen met Visual Studio 2017 Enterprise
1. Klik op de **momentopname downloaden** knop voor het downloaden van een `.diagsession` bestand, dat kan worden geopend door Visual Studio 2017 Enterprise.

2. Openen van de `.diagsession` -bestand, moet u eerst [downloaden en installeren van de momentopname Debugger-extensie voor Visual Studio](https://aka.ms/snapshotdebugger).

3. Nadat u de momentopnamebestand opent, verschijnt de pagina met Mini-geheugendump foutopsporing in Visual Studio. Klik op **fouten opsporen in beheerde Code** momentopname van de foutopsporing te starten. De momentopname is geopend met de coderegel waar de uitzondering is opgetreden, zodat u fouten in de huidige status van het proces opsporen kunt.

    ![Weergave foutopsporing momentopname in Visual Studio](./media/app-insights-snapshot-debugger/open-snapshot-visualstudio.png)

De gedownloade momentopname bevat symboolbestanden die zijn gevonden op de webserver van de toepassing. Deze symboolbestanden zijn om momentopnamegegevens te koppelen met broncode vereist. Zorg dat u de implementatie van het symbool in te schakelen wanneer u uw web-apps publiceren voor App Service-apps.

## <a name="how-snapshots-work"></a>De werking van momentopnamen

De momentopname-Collector wordt geïmplementeerd als een [Application Insights telemetrie Processor](app-insights-configuration-with-applicationinsights-config.md#telemetry-processors-aspnet). Wanneer uw toepassing wordt uitgevoerd, wordt de momentopname Collector telemetrie Processor toegevoegd aan uw toepassing telemetrie pijplijn.
Elke keer dat de toepassing aanroepen [TrackException](app-insights-asp-net-exceptions.md#exceptions), de momentopname-Collector een probleem-ID van het type uitzondering wordt veroorzaakt en de methode activerend wordt berekend.
Elke keer dat uw toepassing TrackException roept, wordt een item verhoogd voor de juiste probleem-ID. Wanneer de teller de `ThresholdForSnapshotting` waarde, het probleem-ID wordt toegevoegd aan een verzameling van plan bent.

De momentopname-Collector bewaakt ook uitzonderingen als ze worden gegenereerd met een abonnement op de [AppDomain.CurrentDomain.FirstChanceException](https://docs.microsoft.com/dotnet/api/system.appdomain.firstchanceexception) gebeurtenis. Wanneer deze gebeurtenis wordt gestart, wordt het probleem-ID van de uitzondering berekend en vergeleken met de id van het probleem in de verzameling van plan bent.
Als er een overeenkomst is, wordt een momentopname van het actieve proces gemaakt. De momentopname is een unieke id toegewezen en de uitzondering wordt vermeld met die id. Nadat de handler FirstChanceException retourneert, is de uitzondering thrown normaal verwerkt. De uitzondering bereikt uiteindelijk de methode TrackException opnieuw waar, samen met de momentopname-id wordt gerapporteerd naar Application Insights.

Het hoofdproces blijft uitvoeren en verkeer leveren aan gebruikers met weinig onderbrekingen. Tegelijkertijd leert wordt de momentopname doorgegeven aan de momentopname Uploader-proces. De momentopname van Uploader maakt een minidump en geüpload naar Application Insights samen met eventuele relevante symboolbestanden (.pdb).

> [!TIP]
> - De momentopname van een proces is een onderbroken kloon van het proces dat wordt uitgevoerd.
> - Maken van een momentopname duurt ongeveer 10 tot 20 milliseconden.
> - De standaardwaarde voor `ThresholdForSnapshotting` is 1. Dit is ook de minimumwaarde. Daarom uw app heeft voor het activeren van dezelfde uitzondering **tweemaal** voordat een momentopname wordt gemaakt.
> - Stel `IsEnabledInDeveloperMode` op true als u genereren van momentopnamen wilt tijdens foutopsporing in Visual Studio.
> - De frequentie van momentopnamen maken wordt beperkt door de `SnapshotsPerTenMinutesLimit` instelling. De limiet is standaard een momentopname van elke tien minuten.
> - Niet meer dan 50 momentopnamen per dag kunnen worden geüpload.

## <a name="current-limitations"></a>Huidige beperkingen

### <a name="publish-symbols"></a>Symbolen publiceren
Het foutopsporingsprogramma momentopname vereist symboolbestanden op de productieserver variabelen decoderen en om een foutopsporing ervaring in Visual Studio te bieden. De 15.2 versie van Visual Studio 2017 publiceert symbolen voor release builds standaard wanneer deze wordt gepubliceerd naar App Service. In eerdere versies, moet u de volgende regel toevoegen aan uw publicatieprofiel `.pubxml` zodat symbolen zijn gepubliceerd in de releasemodus bestand:

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

Voor Azure Compute en andere typen, zorg ervoor dat de symboolbestanden in dezelfde map van de hoofdtoepassing .dll (meestal `wwwroot/bin`) of zijn beschikbaar op het huidige pad.

### <a name="optimized-builds"></a>Geoptimaliseerde builds
In sommige gevallen kunnen niet lokale variabelen worden weergegeven in de release builds vanwege optimalisaties die worden toegepast door het JIT-compileerprogramma.
De momentopname-Collector kan echter in Azure App Services deoptimize activerend methoden die deel van de verzameling van plan bent uitmaken.

> [!TIP]
> De extensie Application Insights-Site installeren in uw App Service deoptimization ondersteuning krijgen.

## <a name="troubleshooting"></a>Problemen oplossen

De volgende tips helpen u problemen oplossen met de momentopname-foutopsporing.

### <a name="use-the-snapshot-health-check"></a>Gebruik de momentopname-statuscontrole
Enkele veelvoorkomende problemen ertoe leiden dat de fouten opsporen in momentopname openen niet weergegeven. Met behulp van een verouderde momentopname Collector, bijvoorbeeld; de dagelijkse uploadlimiet; is bereikt of misschien de momentopname is net lang duurt om te uploaden. Gebruik de momentopname-Serverstatus controleren voor algemene problemen.

Er is een koppeling in het deelvenster met uitzondering van de end-to-end-trace-weergave krijgt u de momentopname-Serverstatus controleren.

![Voer de momentopname-statuscontrole](./media/app-insights-snapshot-debugger/enter-snapshot-health-check.png)

De interactieve, chat-achtige interface gezocht naar veelvoorkomende problemen en helpt u om te corrigeren.

![Statuscontrole](./media/app-insights-snapshot-debugger/healthcheck.png)

Als dit het probleem niet oplost, Raadpleeg de volgende handmatige stappen voor probleemoplossing.

### <a name="verify-the-instrumentation-key"></a>Controleer of de instrumentatiesleutel

Zorg ervoor dat u de juiste instrumentatiesleutel in uw gepubliceerde toepassing. Normaal gesproken de instrumentatiesleutel uit het bestand ApplicationInsights.config is gelezen. Controleer of de waarde is hetzelfde als de instrumentatiesleutel voor de Application Insights-resource die u in de portal ziet.

### <a name="upgrade-to-the-latest-version-of-the-nuget-package"></a>Een upgrade uitvoert naar de nieuwste versie van het NuGet-pakket

Visual Studio NuGet Package Manager gebruiken om ervoor te zorgen dat u de nieuwste versie van Microsoft.ApplicationInsights.SnapshotCollector. Opmerkingen bij de release kunnen worden gevonden op https://github.com/Microsoft/ApplicationInsights-Home/issues/167

### <a name="check-the-uploader-logs"></a>Raadpleeg de logboeken uploader

Nadat een momentopname is gemaakt, wordt een bestand met Mini-geheugendump (dmp) op schijf gemaakt. Een afzonderlijke uploader-proces dat minidump-bestand maakt en uploadt, samen met eventuele bijbehorende-PDB-bestanden naar Application Insights momentopname foutopsporingsprogramma opslag. Nadat de minidump heeft geüpload, wordt deze verwijderd van de schijf. De logboekbestanden voor het proces uploader zijn opgeslagen op schijf. In een App Service-omgeving vindt u deze logboeken in `D:\Home\LogFiles`. Met de site voor het beheer van Kudu voor App Service kunt u deze logboekbestanden.

1. Open uw App Service-toepassing in de Azure portal.
2. Klik op **geavanceerde hulpmiddelen**, of zoek naar **Kudu**.
3. Klik op **gaat**.
4. In de **-console voor foutopsporing** vervolgkeuzelijst de optie **CMD**.
5. Klik op **logboekbestanden**.

Er is ten minste één bestand met een naam die met begint `Uploader_` of `SnapshotUploader_` en een `.log` extensie. Klik op het juiste pictogram om te downloaden van alle logboekbestanden of in een browser openen.
De bestandsnaam bevat een uniek achtervoegsel waarmee het App Service-exemplaar. Als uw App Service-exemplaar wordt gehost op meer dan één computer, moet u er aparte logboekbestanden voor elke computer zijn. Wanneer de uploader een nieuw minidump-bestand detecteert, wordt deze vastgelegd in het logboekbestand. Hier volgt een voorbeeld van een geslaagde momentopname en uploaden:

```
SnapshotUploader.exe Information: 0 : Received Fork request ID 139e411a23934dc0b9ea08a626db16c5 from process 6368 (Low pri)
    DateTime=2018-03-09T01:42:41.8571711Z
SnapshotUploader.exe Information: 0 : Creating minidump from Fork request ID 139e411a23934dc0b9ea08a626db16c5 from process 6368 (Low pri)
    DateTime=2018-03-09T01:42:41.8571711Z
SnapshotUploader.exe Information: 0 : Dump placeholder file created: 139e411a23934dc0b9ea08a626db16c5.dm_
    DateTime=2018-03-09T01:42:41.8728496Z
SnapshotUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:45.7525022Z
SnapshotUploader.exe Information: 0 : Successfully wrote minidump to D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:45.7681360Z
SnapshotUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 214.42 MB (uncompressed)
    DateTime=2018-03-09T01:42:45.7681360Z
SnapshotUploader.exe Information: 0 : Upload successful. Compressed size 86.56 MB
    DateTime=2018-03-09T01:42:59.6184651Z
SnapshotUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2018-03-09T01:42:59.6184651Z
SnapshotUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2018-03-09T01:42:59.6809606Z
SnapshotUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2018-03-09T01:42:59.8059929Z
SnapshotUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:59.8530649Z
```

> [!NOTE]
> Het bovenstaande voorbeeld is van versie 1.2.0 van het Microsoft.ApplicationInsights.SnapshotCollector NuGet-pakket. In eerdere versies van het proces uploader heet `MinidumpUploader.exe` en het logboek minder gedetailleerd is.

In het vorige voorbeeld de instrumentatiesleutel wordt `c12a605e73c44346a984e00000000000`. Deze waarde moet overeenkomen met de instrumentatiesleutel voor uw toepassing.
De minidump is gekoppeld aan een momentopname met de ID `139e411a23934dc0b9ea08a626db16c5`. U kunt deze ID later te vinden van de bijbehorende uitzonderingstelemetrie in Application Insights Analytics gebruiken.

De uploader scant op nieuwe-PDB-bestanden over om de 15 minuten. Hier volgt een voorbeeld:

```
SnapshotUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2018-03-09T01:47:19.4457768Z
SnapshotUploader.exe Information: 0 : Scanning D:\home\site\wwwroot for local PDBs.
    DateTime=2018-03-09T01:47:19.4457768Z
SnapshotUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2018-03-09T01:47:19.4614027Z
SnapshotUploader.exe Information: 0 : Deleted PDB scan marker : D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\6368.pdbscan
    DateTime=2018-03-09T01:47:19.4614027Z
```

Voor toepassingen die zijn _niet_ gehost in App Service, de uploader-logboeken zijn in dezelfde map als de minidumps: `%TEMP%\Dumps\<ikey>` (waarbij `<ikey>` is uw instrumentatiesleutel).

### <a name="troubleshooting-cloud-services"></a>Problemen met Cloudservices oplossen
Voor rollen in Cloudservices, de tijdelijke standaardmap mogelijk te klein voor de bestanden met Mini-geheugendump, wat leidt tot verloren momentopnamen.
De ruimte die nodig zijn, is afhankelijk van de totale werkset van uw toepassing en het aantal gelijktijdige momentopnamen.
De werkset van een 32-bits ASP.NET-Webrol is meestal tussen 200 MB en 500 MB.
Wacht ten minste twee gelijktijdige momentopnamen.
Bijvoorbeeld, als uw toepassing gebruikmaakt van 1 GB van totale werkset, moet u zorgen dat er ten minste 2 GB aan schijfruimte voor het opslaan van momentopnamen is.
Volg deze stappen voor uw Cloudservice rol configureren met een specifieke lokale bron voor momentopnamen.

1. Een nieuwe lokale resource toevoegen aan uw Cloud-Service door de Service in de Cloud-definitiebestand (.csdef) te bewerken. Het volgende voorbeeld definieert een resource aangeroepen `SnapshotStore` met een grootte van 5 GB.
   ```xml
   <LocalResources>
     <LocalStorage name="SnapshotStore" cleanOnRoleRecycle="false" sizeInMB="5120" />
   </LocalResources>
   ```

2. Wijzigen van uw rol starten van de code voor het toevoegen van een omgevingsvariabele die naar verwijst de `SnapshotStore` lokale resource. Voor werkrollen, de code moet worden toegevoegd aan uw rol `OnStart` methode:
   ```csharp
   public override bool OnStart()
   {
       Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
       return base.OnStart();
   }
   ```
   Voor Web rollen (ASP.NET), de code moet worden toegevoegd aan uw webtoepassing `Application_Start` methode:
   ```csharp
   using Microsoft.WindowsAzure.ServiceRuntime;
   using System;

   namespace MyWebRoleApp
   {
       public class MyMvcApplication : System.Web.HttpApplication
       {
          protected void Application_Start()
          {
             Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
             // TODO: The rest of your application startup code
          }
       }
   }
   ```

3. Bijwerken van uw rol ApplicationInsights.config-bestand voor het overschrijven van de locatie van de tijdelijke map die wordt gebruikt door `SnapshotCollector`
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

Wanneer een momentopname wordt gemaakt, is de activerend uitzondering gemarkeerd met een momentopname-ID. Die ID momentopname wordt opgenomen als een aangepaste eigenschap wanneer de uitzonderingstelemetrie naar Application Insights wordt gemeld. Met behulp van **Search** in Application Insights, vindt u alle telemetrie met de `ai.snapshot.id` aangepaste eigenschap.

1. Blader naar uw Application Insights-resource in de Azure-portal.
2. Klik op **Search**.
3. Type `ai.snapshot.id` in het zoekvak en druk op Enter.

![Zoeken naar telemetrie met een momentopname-ID in de portal](./media/app-insights-snapshot-debugger/search-snapshot-portal.png)

Als deze zoekopdracht geen resultaten oplevert, zijn wordt er zijn geen momentopnamen gerapporteerd, naar Application Insights voor uw toepassing in de geselecteerde periode.

Als u wilt zoeken naar een specifieke momentopname-ID van de logboeken Uploader, typt u die ID in het zoekvak. Als u geen telemetrie voor een momentopname waarvan u weet dat is geüpload vinden, voert u de volgende stappen uit:

1. Controleer dat u op zoek bent op de juiste Application Insights-resource door te controleren of de instrumentatiesleutel.

2. Met behulp van de tijdstempel van het logboek Uploader, het filter tijdsbereik van de zoekopdracht omvatten die tijdsbereik aanpassen.

Als u een uitzondering met die ID momentopname niet ziet, is niet de uitzonderingstelemetrie gerapporteerd naar Application Insights. Deze situatie kan zich voordoen als uw toepassing is vastgelopen nadat de momentopname die het heeft, maar voordat de uitzonderingstelemetrie gemeld. In dit geval, controleert u de App Service-Logboeken onder `Diagnose and solve problems` om te zien of er onverwachte opnieuw wordt opgestart zijn of onverwerkte uitzonderingen.

### <a name="edit-network-proxy-or-firewall-rules"></a>Netwerk proxy of firewall-regels bewerken

Als uw toepassing verbinding met Internet via een proxy of firewall maakt, moet u wellicht de regels voor het toestaan van uw toepassing om te communiceren met de momentopname-foutopsporingsprogramma bewerken. Hier volgt [een lijst met IP-adressen en poorten gebruikt door het foutopsporingsprogramma momentopname](app-insights-ip-addresses.md#snapshot-debugger).

## <a name="next-steps"></a>Volgende stappen

* [Snappoints instellen in uw code](https://docs.microsoft.com/visualstudio/debugger/debug-live-azure-applications) momentopnamen ophalen zonder te wachten op een uitzondering.
* [Diagnose van uitzonderingen in uw web-apps](app-insights-asp-net-exceptions.md) wordt uitgelegd hoe u meer uitzonderingen zichtbaar maken voor Application Insights.
* [Detectie van smartcard](app-insights-proactive-diagnostics.md) detecteert automatisch afwijkingen.
