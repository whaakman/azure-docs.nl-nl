---
title: Azure Application Insights Snapshot Debugger voor .NET-apps | Microsoft Docs
description: Fouten opsporen in momentopnamen worden automatisch verzameld wanneer er uitzonderingen zijn opgetreden in productie .NET-apps
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/08/2018
ms.reviewer: pharring
ms.author: mbullwin
ms.openlocfilehash: 50f5cf48838e5578dc94c61b4463f94e76f886e6
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54028370"
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a>Fouten opsporen in momentopnamen op uitzonderingen in .NET-apps

Wanneer er een uitzondering optreedt, kunt u automatisch een momentopname voor foutopsporing verzamelen van uw live webtoepassing. De momentopname toont de status van de broncode en variabelen op het moment dat de uitzondering is opgetreden. De Snapshot Debugger (preview) in [Azure Application Insights](../../application-insights/app-insights-overview.md) bewaakt uitzonderingstelemetrie van uw web-app. Deze verzamelt momentopnamen op uw uitzonderingen boven activerende zodat u de informatie die u nodig hebt om problemen in de productieomgeving te diagnosticeren. Bevatten de [Snapshot collector NuGet-pakket](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) in uw toepassing, en (optioneel) Configureer verzameling parameters in [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md). Momentopnamen worden weergegeven op [uitzonderingen](../../azure-monitor/app/asp-net-exceptions.md) in de Application Insights-portal.

U kunt foutopsporingsmomentopnamen in het portaal bekijken om de aanroepstack te zien en variabelen inspecteren bij elk aanroepstackframe. Als u een krachtigere foutopsporing ervaring met de broncode, opent u momentopnamen met Visual Studio 2017 Enterprise. In Visual Studio, kunt u ook [Snappoints om interactief momentopnamen te instellen](https://aka.ms/snappoint) zonder te wachten op een uitzondering.

Fouten opsporen in momentopnamen worden opgeslagen voor de zeven dagen. Deze bewaarbeleid is ingesteld op basis van de per toepassing. Als u nodig hebt om deze waarde te verhogen, kunt u een toename van aanvragen door een ondersteuningsaanvraag opent in de Azure-portal.

Verzameling van de momentopname is beschikbaar voor:
* .NET framework en ASP.NET-toepassingen met .NET Framework 4.5 of hoger.
* .NET core 2.0 en ASP.NET Core 2.0-toepassingen worden uitgevoerd op Windows.

De volgende omgevingen worden ondersteund:
* Azure App Service.
* Azure Cloud Service OS family 4 of hoger uitvoeren.
* Azure Service Fabric-services wordt uitgevoerd op Windows Server 2012 R2 of hoger.
* Virtuele Azure-Machines met Windows Server 2012 R2 of hoger.
* On-premises virtuele of fysieke machines met Windows Server 2012 R2 of hoger.

> [!NOTE]
> Client-toepassingen (bijvoorbeeld WPF, Windows Forms of UWP) worden niet ondersteund.

### <a name="configure-snapshot-collection-for-aspnet-applications"></a>Momentopname verzamelen voor ASP.NET-toepassingen configureren

1. [Application Insights inschakelen in uw web-app](../../azure-monitor/app/asp-net.md), als u dit nog niet hebt gedaan.

2. Bevatten de [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-pakket in uw app.

3. Bekijk de standaardopties die het pakket wordt toegevoegd aan [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md):

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
        <SnapshotsPerTenMinutesLimit>3</SnapshotsPerTenMinutesLimit>
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

4. Momentopnamen worden verzameld alleen op uitzonderingen die worden gerapporteerd aan Application Insights. In sommige gevallen (bijvoorbeeld oudere versies van het .NET-platform), moet u wellicht te [uitzondering verzamelen configureren](../../azure-monitor/app/asp-net-exceptions.md#exceptions) om te zien welke uitzonderingen met momentopnamen in de portal.


### <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a>Momentopname verzamelen voor ASP.NET Core 2.0-toepassingen configureren

1. [Application Insights inschakelen in uw ASP.NET Core web-app](../../azure-monitor/app/asp-net-core.md), als u dit nog niet hebt gedaan.

    > [!NOTE]
    > Ervoor dat uw toepassing verwijst naar versie 2.1.1 of hoger, van het pakket Microsoft.ApplicationInsights.AspNetCore zijn.

2. Bevatten de [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-pakket in uw app.

3. Wijzigen van uw toepassing `Startup` klasse die u wilt toevoegen en configureren van de Snapshot Collector telemetrie processor.

    Voeg de volgende using-instructies toe aan `Startup.cs`

   ```csharp
   using Microsoft.ApplicationInsights.SnapshotCollector;
   using Microsoft.Extensions.Options;
   using Microsoft.ApplicationInsights.AspNetCore;
   using Microsoft.ApplicationInsights.Extensibility;
   ```

   Voeg de volgende `SnapshotCollectorTelemetryProcessorFactory` klasse aan `Startup` klasse.

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
    Voeg de `SnapshotCollectorConfiguration` en `SnapshotCollectorTelemetryProcessorFactory` services aan de pijplijn starten:

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

4. De Snapshot Collector configureren door het toevoegen van een sectie SnapshotCollectorConfiguration aan appsettings.json. Bijvoorbeeld:

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

1. Als uw toepassing is niet al zijn geïnstrumenteerd met Application Insights, aan de slag met [Application Insights inschakelen en het instellen van de instrumentatiesleutel](../../application-insights/app-insights-windows-desktop.md).

2. Voeg de [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-pakket in uw app.

3. Momentopnamen worden verzameld alleen op uitzonderingen die worden gerapporteerd aan Application Insights. Mogelijk moet u uw code meldt deze wijzigen. De code van afhandelingsservice voor uitzondering, is afhankelijk van de structuur van uw toepassing, maar een voorbeeld is lager dan:
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

Toegang tot momentopnamen is beveiligd met op rollen gebaseerd toegangsbeheer (RBAC). Als u wilt controleren van een momentopname, moet u eerst worden toegevoegd aan de benodigde rol door de eigenaar van een abonnement.

> [!NOTE]
> Eigenaren en medewerkers automatisch geen deze rol. Als ze momentopnamen weergeven willen, moeten ze zelf toevoegen aan de rol.

Eigenaars van abonnementen toe te kennen de `Application Insights Snapshot Debugger` rol voor gebruikers die zullen momentopnamen inspecteren. Deze rol kan worden toegewezen aan individuele gebruikers of groepen door abonnementseigenaren voor Application Insights-resource van het doel of de resourcegroep of abonnement.

1. Navigeer naar de Application Insights-resource in Azure portal.
1. Klik op **toegangsbeheer (IAM)**.
1. Klik op de **+ roltoewijzing toevoegen** knop.
1. Selecteer **Application Insights Snapshot Debugger** uit de **rollen** vervolgkeuzelijst.
1. Zoek en voer een naam voor de gebruiker toe te voegen.
1. Klik op de **opslaan** om toe te voegen van de gebruiker aan de rol.


> [!IMPORTANT]
> Momentopnamen kunnen mogelijk persoonlijke en andere gevoelige gegevens in waarden van variabelen en parameter bevatten.

## <a name="debug-snapshots-in-the-application-insights-portal"></a>Fouten opsporen in momentopnamen in de Application Insights-portal

Als een momentopname beschikbaar voor een bepaalde uitzonderingen of een probleem-ID is, een **fouten opsporen in momentopname openen** knop wordt weergegeven op de [uitzondering](../../azure-monitor/app/asp-net-exceptions.md) in de Application Insights-portal.

![De knop openen fouten opsporen in momentopname van uitzondering](./media/snapshot-debugger/snapshot-on-exception.png)

In de weergave fouten opsporen in momentopname ziet u een aanroepstack en een deelvenster Variabelen. Wanneer u frames van de aanroepstack in het deelvenster met call stack selecteert, vindt u lokale variabelen en parameters voor deze functie aanroepen in het deelvenster Variabelen.

![Weergave fouten opsporen in momentopname in de portal](./media/snapshot-debugger/open-snapshot-portal.png)

Momentopnamen kunnen gevoelige gegevens bevatten, en standaard ze niet kan worden weergegeven. Als u momentopnamen, hebt u de `Application Insights Snapshot Debugger` rol die aan u zijn toegewezen.

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a>Fouten opsporen in momentopnamen met Visual Studio 2017 Enterprise
1. Klik op de **momentopname downloaden** knop voor het downloaden van een `.diagsession` -bestand, dat kan worden geopend door Visual Studio 2017 Enterprise.

2. Om te openen de `.diagsession` -bestand, moet u beschikken over de Snapshot Debugger VS zijn geïnstalleerd. De Snapshot Debugger-component is een vereist onderdeel van de ASP.net-werkbelasting in Visual Studio en kan worden geselecteerd in de lijst met afzonderlijke onderdelen in het installatieprogramma van Visual Studio. Als u met behulp van een eerdere versie van Visual Studio dan 15.5 moet u voor het installeren van de extensie van de [Visual Studio marketplace](https://aka.ms/snapshotdebugger).

3. Nadat u de momentopnamebestand opent, wordt de pagina Minidump foutopsporing in Visual Studio weergegeven. Klik op **fouten opsporen in beheerde Code** foutopsporing van de momentopname te starten. De momentopname wordt geopend aan de coderegel waar de uitzondering is opgetreden, zodat u kunt fouten opsporen in de huidige status van het proces.

    ![Weergave-momentopname voor foutopsporing in Visual Studio](./media/snapshot-debugger/open-snapshot-visualstudio.png)

De momentopname van het gedownloade bevat symboolbestanden die zijn gevonden op uw webserver van de toepassing. Deze symboolbestanden moet momentopname van de gegevens koppelen met de broncode. Voor App Service-apps, zorg ervoor dat u het symbool implementatie in te schakelen wanneer u uw web-apps publiceren.

## <a name="how-snapshots-work"></a>De werking van momentopnamen

De Snapshot Collector wordt geïmplementeerd als een [Application Insights Telemetry Processor](../../azure-monitor/app/configuration-with-applicationinsights-config.md#telemetry-processors-aspnet). Wanneer uw toepassing wordt uitgevoerd, wordt de telemetrie-Processor van Snapshot Collector wordt toegevoegd aan van de toepassing telemetrie pijplijn.
Telkens wanneer die uw toepassing aanroepen [TrackException](../../azure-monitor/app/asp-net-exceptions.md#exceptions), de Snapshot Collector berekent een probleem-ID van het type uitzondering en de activerend methode.
Telkens wanneer uw toepassing TrackException, roept wordt een item verhoogd voor de juiste probleem-ID. Wanneer de teller de `ThresholdForSnapshotting` waarde, de probleem-ID wordt toegevoegd aan een verzameling van plan bent.

De Snapshot Collector bewaakt ook uitzonderingen als ze zijn gegenereerd met een abonnement op de [AppDomain.CurrentDomain.FirstChanceException](https://docs.microsoft.com/dotnet/api/system.appdomain.firstchanceexception) gebeurtenis. Wanneer die gebeurtenis wordt geactiveerd, wordt de probleem-ID van de uitzondering berekend en vergeleken met de probleem-id's in de verzameling van plan bent.
Als er een overeenkomst is, wordt een momentopname van het actieve proces gemaakt. De momentopname is een unieke id toegewezen en de uitzondering is een factureringslabel die id. Nadat de handler FirstChanceException worden geretourneerd, is de thrown uitzondering wordt verwerkt die normaal werken. De uitzondering bereikt uiteindelijk de methode TrackException opnieuw waar, samen met de id van de momentopname, wordt dit apparaat gerapporteerd naar Application Insights.

De belangrijkste proces wordt voortgezet uit te voeren en verkeer voor gebruikers met weinig onderbreking fungeren. De momentopname wordt ondertussen doorgegeven aan de momentopname Uploader-proces. De momentopname van Uploader maakt u een minidump en geüpload naar Application Insights, samen met eventuele relevante (.pdb)-symboolbestanden.

> [!TIP]
> - Een momentopname van een proces is een onderbroken kloon van het proces dat wordt uitgevoerd.
> - Het maken van de momentopname van het duurt ongeveer 10 tot 20 milliseconden.
> - De standaardwaarde voor `ThresholdForSnapshotting` is 1. Dit is ook de minimale waarde. Daarom uw app heeft voor het activeren van de dezelfde uitzondering **tweemaal** voordat er een momentopname wordt gemaakt.
> - Stel `IsEnabledInDeveloperMode` in op true als u genereren van momentopnamen wilt bij foutopsporing in Visual Studio.
> - De frequentie voor het maken van momentopnamen wordt beperkt door de `SnapshotsPerTenMinutesLimit` instelling. Standaard heeft de limiet is een momentopname van elke tien minuten.
> - Niet meer dan 50 momentopnamen per dag kunnen worden geüpload.

## <a name="current-limitations"></a>Huidige beperkingen

### <a name="publish-symbols"></a>Symbolen publiceren
De Snapshot Debugger vereist symboolbestanden op de productieserver moet worden gedecodeerd variabelen en om een probleemoplossingservaring in Visual Studio.
Versie 15.2 (of hoger) van Visual Studio 2017 publiceert symbolen voor release standaard bouwt wanneer deze wordt gepubliceerd naar App Service. In eerdere versies, moet u de volgende regel toevoegen aan uw publicatieprofiel `.pubxml` bestand, zodat de symbolen in de releasemodus worden gepubliceerd:

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

Voor Azure Compute en andere typen, zorg ervoor dat het symboolbestanden zich in dezelfde map van de hoofdtoepassing dll-bestanden (normaal gesproken `wwwroot/bin`) of beschikbaar zijn op het huidige pad.

### <a name="optimized-builds"></a>Geoptimaliseerde builds
In sommige gevallen kunnen geen lokale variabelen worden weergegeven in de release builds vanwege optimalisaties die worden toegepast door de JIT-compiler.
De Snapshot Collector kan echter in Azure App Services deoptimize activerend methoden die deel van de verzameling van plan bent uitmaken.

> [!TIP]
> Installeer de Application Insights-Site-extensie in uw App Service deoptimization ondersteuning krijgen.

## <a name="troubleshooting"></a>Problemen oplossen

De volgende tips helpen bij het oplossen van problemen met de Snapshot Debugger.

### <a name="use-the-snapshot-health-check"></a>Gebruik de momentopname-statuscontrole
Enkele veelvoorkomende problemen resulteren in de Open fouten opsporen in momentopname niet weergegeven. Met behulp van een verouderde Snapshot Collector, bijvoorbeeld; de dagelijkse uploadlimiet; is bereikt of misschien de momentopname is net lang duurt om te uploaden. Gebruik de momentopname-statuscontrole veelvoorkomende problemen op te lossen.

Er is een koppeling in het deelvenster met uitzondering van de end-to-end tracering weergave die u naar de momentopname-Serverstatus controleren gaat.

![Statuscontrole van de momentopname invoeren](./media/snapshot-debugger/enter-snapshot-health-check.png)

De interactieve, chat-achtige interface uiterlijk voor veelvoorkomende problemen en helpt u om ze te corrigeren.

![Statuscontrole](./media/snapshot-debugger/healthcheck.png)

Als dat niet het probleem is opgelost, klikt u vervolgens verwijzen naar de volgende handmatige stappen voor probleemoplossing.

### <a name="verify-the-instrumentation-key"></a>Controleer of de instrumentatiesleutel

Zorg ervoor dat u de juiste instrumentatiesleutel in uw gepubliceerde toepassing. Normaal gesproken wordt de instrumentatiesleutel gelezen uit het bestand ApplicationInsights.config. Controleer of de waarde is hetzelfde als de instrumentatiesleutel voor de Application Insights-resource die u in de portal ziet.

### <a name="upgrade-to-the-latest-version-of-the-nuget-package"></a>Een upgrade uitvoert naar de nieuwste versie van het NuGet-pakket

Gebruik Visual Studio NuGet Package Manager om te controleren of u de nieuwste versie van Microsoft.ApplicationInsights.SnapshotCollector. Opmerkingen bij de release kunnen u vinden op https://github.com/Microsoft/ApplicationInsights-Home/issues/167

### <a name="check-the-uploader-logs"></a>Raadpleeg de logboeken uploader

Nadat een momentopname is gemaakt, wordt een minidump-bestand (dmp) wordt gemaakt op de schijf. Een afzonderlijke uploader-proces dat minidump-bestand maakt en uploadt, samen met eventuele gekoppelde PDB-bestanden, naar Application Insights Snapshot Debugger opslag. Nadat de minidump heeft geüpload, wordt deze verwijderd van schijf. De logboekbestanden voor het proces uploader zijn opgeslagen op schijf. In een App Service-omgeving, vindt u deze logboeken in `D:\Home\LogFiles`. De Kudu-site voor beheer voor App Service gebruiken om deze logboekbestanden.

1. Open uw App Service-toepassing in Azure portal.
2. Klik op **geavanceerde hulpmiddelen**, of zoek naar **Kudu**.
3. Klik op **gaat**.
4. In de **Foutopsporingsconsole** vervolgkeuzelijst Schakel **CMD**.
5. Klik op **logboekbestanden**.

Er is ten minste één bestand met een naam die met begint `Uploader_` of `SnapshotUploader_` en een `.log` extensie. Klik op het juiste pictogram om te downloaden van de logboekbestanden of in een browser openen.
De bestandsnaam bevat een uniek achtervoegsel waarmee het App Service-exemplaar. Als uw App Service-exemplaar wordt gehost op meer dan één machine, zijn er afzonderlijke logboekbestanden voor elke machine. Wanneer u de uploader detecteert een nieuw minidump-bestand, wordt deze in het logboekbestand vastgelegd. Hier volgt een voorbeeld van een geslaagde momentopname en te uploaden:

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
> Het bovenstaande voorbeeld is versie 1.2.0 van het Microsoft.ApplicationInsights.SnapshotCollector NuGet-pakket. In eerdere versies, de uploader-proces wordt genoemd `MinidumpUploader.exe` en het logboek is minder gedetailleerd.

In het vorige voorbeeld de instrumentatiesleutel is `c12a605e73c44346a984e00000000000`. Deze waarde moet overeenkomen met de instrumentatiesleutel voor uw toepassing.
De minidump is gekoppeld aan een momentopname met de ID `139e411a23934dc0b9ea08a626db16c5`. U kunt deze ID later gebruiken vinden van de bijbehorende uitzonderingstelemetrie in Application Insights Analytics.

De uploader scant op nieuwe PDB-bestanden over om de 15 minuten. Hier volgt een voorbeeld:

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

Voor toepassingen die _worden niet_ gehost in App Service, de uploader-logboeken worden in dezelfde map als de minidumps: `%TEMP%\Dumps\<ikey>` (waarbij `<ikey>` uw instrumentatiesleutel is).

### <a name="troubleshooting-cloud-services"></a>Problemen met Cloudservices oplossen
Voor de rollen in Cloud Services zijn de tijdelijke standaardmap te klein voor het opslaan van de minidump-bestanden, waardoor er verloren momentopnamen.
De ruimte die nodig zijn, is afhankelijk van de totale werkset van uw toepassing en het aantal gelijktijdige momentopnamen.
De werkset van een 32-bits ASP.NET-Webrol is meestal tussen 200 MB en 500 MB.
Wacht ten minste twee gelijktijdige momentopnamen.
Bijvoorbeeld, als uw toepassing gebruikmaakt van 1 GB aan totale werkset, moet u ervoor dat er ten minste 2 GB aan schijfruimte voor het opslaan van momentopnamen.
Volg deze stappen voor het configureren van uw rol Service in de Cloud met een eigen lokale resources voor momentopnamen.

1. Een nieuwe lokale resource toevoegen aan uw Service in de Cloud door de Service in de Cloud-definitie (.csdef)-bestand te bewerken. Het volgende voorbeeld definieert een resource met de naam `SnapshotStore` met een grootte van 5 GB.
   ```xml
   <LocalResources>
     <LocalStorage name="SnapshotStore" cleanOnRoleRecycle="false" sizeInMB="5120" />
   </LocalResources>
   ```

2. Wijzigen van uw rol opstartcode voor het toevoegen van een omgevingsvariabele die naar verwijst de `SnapshotStore` lokale bron. Voor werkrollen, de code moet worden toegevoegd aan van uw rol `OnStart` methode:
   ```csharp
   public override bool OnStart()
   {
       Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
       return base.OnStart();
   }
   ```
   Voor Web-rollen (ASP.NET), de code moet worden toegevoegd aan uw webtoepassing `Application_Start` methode:
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

3. Bijwerken van uw rol ApplicationInsights.config-bestand als u wilt overschrijven van de locatie van de tijdelijke map die wordt gebruikt door `SnapshotCollector`
   ```xml
   <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Use the SnapshotStore local resource for snapshots -->
      <TempFolder>%SNAPSHOTSTORE%</TempFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
   </TelemetryProcessors>
   ```

### <a name="overriding-the-shadow-copy-folder"></a>De schaduwkopie-map te overschrijven

Wanneer de momentopname-Collector wordt gestart, probeert het vinden van een map op de schijf die geschikt is voor het uitvoeren van de momentopname Uploader-proces. De door u gekozen map staat bekend als de Shadow Copy-map.

De Snapshot Collector controleert enkele bekende locaties, te zorgen dat deze machtigingen heeft om te kopiëren van de momentopname Uploader binaire bestanden. De volgende omgevingsvariabelen worden gebruikt:
- Fabric_Folder_App_Temp
- LOCALAPPDATA
- APPDATA
- TEMP

Als een geschikte map kan niet worden gevonden, Snapshot Collector meldt een fout met de melding _"Kan een geschikte shadow copy-map niet vinden."_

Rapporten van Snapshot Collector als het kopiëren mislukt, een `ShadowCopyFailed` fout.

Als de uploader kan niet worden gestart, Snapshot Collector rapporteert een `UploaderCannotStartFromShadowCopy` fout. De hoofdtekst van het bericht bevat vaak `System.UnauthorizedAccessException`. Deze fout treedt meestal op omdat de toepassing wordt uitgevoerd onder een account met beperkte machtigingen. Het account heeft machtigingen voor schrijven naar de shadow copy-map, maar het is niet gemachtigd voor het uitvoeren van code.

Omdat deze fouten worden gewoonlijk tijdens het opstarten optreden, ze meestal moeten worden gevolgd door een `ExceptionDuringConnect` fout uitspraak _"Uploader kan niet worden gestart."_

Als tijdelijke oplossing voor deze fouten, kunt u de shadow copy-map handmatig via de `ShadowCopyFolder` configuratie-optie. Bijvoorbeeld, u ApplicationInsights.config gebruikt:

   ```xml
   <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Override the default shadow copy folder. -->
      <ShadowCopyFolder>D:\SnapshotUploader</ShadowCopyFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
   </TelemetryProcessors>
   ```

Of, als u appsettings.json met een .NET Core-toepassing:

   ```json
   {
     "ApplicationInsights": {
       "InstrumentationKey": "<your instrumentation key>"
     },
     "SnapshotCollectorConfiguration": {
       "ShadowCopyFolder": "D:\\SnapshotUploader"
     }
   }
   ```

### <a name="use-application-insights-search-to-find-exceptions-with-snapshots"></a>Gebruik Application Insights zoeken naar uitzonderingen met momentopnamen

Wanneer een momentopname wordt gemaakt, is de activerend uitzondering gemarkeerd met een momentopname-ID. Deze momentopname-ID is opgenomen als een aangepaste eigenschap wanneer de uitzonderingstelemetrie naar Application Insights wordt gemeld. Met behulp van **zoeken** in Application Insights, vindt u alle telemetrie met de `ai.snapshot.id` aangepaste eigenschap.

1. Blader naar uw Application Insights-resource in Azure portal.
2. Klik op **Zoeken**.
3. Type `ai.snapshot.id` in het zoekvak en druk op Enter.

![Zoek telemetrie met een momentopname-ID in de portal](./media/snapshot-debugger/search-snapshot-portal.png)

Als deze zoekopdracht geen resultaten oplevert, zijn wordt er zijn geen momentopnamen gerapporteerd naar Application Insights voor uw toepassing in het geselecteerde tijdsbereik.

Als u wilt zoeken naar een specifieke momentopname-ID van de Uploader-Logboeken, typt u deze ID in het zoekvak in. Als u geen telemetrie voor een momentopname waarvan u weet dat is geüpload vinden, volgt u deze stappen:

1. Controleer dat u geïnteresseerd bent in de juiste Application Insights-resource door te controleren of de instrumentatiesleutel.

2. Met behulp van de tijdstempel van het logboek Uploader, pas het filter tijdsbereik van de zoekopdracht om te kunnen krijgen die tijdsbereik.

Als u een uitzondering met deze momentopname-ID nog steeds niet ziet, is niet naar Application Insights-uitzonderingstelemetrie de gerapporteerd. Deze situatie kan zich voordoen als uw toepassing is vastgelopen nadat de momentopname heeft geduurd, maar voordat deze de uitzonderingstelemetrie gemeld. In dit geval, controleert u de App Service-Logboeken onder `Diagnose and solve problems` om te zien of er onverwacht opnieuw wordt opgestart zijn of er is een onverwerkte uitzonderingen.

### <a name="edit-network-proxy-or-firewall-rules"></a>De netwerk-proxy of firewall-regels bewerken

Als uw toepassing verbinding met Internet via een proxy of firewall maakt, moet u mogelijk de regels voor het toestaan van uw toepassing om te communiceren met de service Snapshot Debugger bewerken. Hier volgt [een lijst met IP-adressen en poorten die worden gebruikt door de Snapshot Debugger](../../azure-monitor/app/ip-addresses.md#snapshot-debugger).

## <a name="next-steps"></a>Volgende stappen

* [Snappoints instellen in uw code](https://docs.microsoft.com/visualstudio/debugger/debug-live-azure-applications) momentopnamen ophalen zonder te wachten op een uitzondering.
* [Diagnose-uitzonderingen in uw web-apps](../../azure-monitor/app/asp-net-exceptions.md) wordt uitgelegd hoe u meer uitzonderingen zichtbaar maken voor Application Insights.
* [Slimme detectie](../../application-insights/app-insights-proactive-diagnostics.md) automatisch detecteert afwijkende prestaties.
