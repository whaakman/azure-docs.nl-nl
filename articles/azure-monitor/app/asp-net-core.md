---
title: Azure Application Insights voor ASP.NET Core | Microsoft Docs
description: Bewaak ASP.NET Core web-apps voor beschikbaarheid, prestaties en gebruik.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/03/2018
ms.author: mbullwin
ms.openlocfilehash: 24132fdb23ff89045f2b497327997d95e4ceecac
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54054840"
---
# <a name="application-insights-for-aspnet-core"></a>Application Insights voor ASP.NET Core

Azure Application Insights biedt uitgebreide bewaking van uw webtoepassing op het codeniveau. U kunt eenvoudig uw web-App voor beschikbaarheid, prestaties en gebruik bewaken. U kunt ook snel fouten in de toepassing identificeren en er een diagnose voor uitvoeren, zonder dat u hoeft te wachten totdat een gebruiker ze heeft gerapporteerd.

In dit artikel leidt u door de stappen voor het maken van een voorbeeld van een ASP.NET Core [Razor Pages](https://docs.microsoft.com/aspnet/core/mvc/razor-pages/?tabs=visual-studio) toepassing in Visual Studio. Deze ook wordt beschreven hoe u voor het bewaken met behulp van Application Insights.

## <a name="prerequisites"></a>Vereisten

- .NET core 2.0.0 SDK of hoger
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) versie 15.7.3 of hoger, met de ASP.NET- en workload voor ontwikkeling

## <a name="create-an-aspnet-core-project-in-visual-studio"></a>Een ASP.NET Core-project in Visual Studio maken

1. Met de rechtermuisknop op **Visual Studio 2017**, en selecteer vervolgens **als administrator uitvoeren**.
2. Selecteer **bestand** > **nieuwe** > **Project** (Ctrl + Shift + N).

   ![Schermopname van Visual Studio-nieuw project-menu](./media/asp-net-core/001-new-project.png)

3. Vouw **Visual C#** uit. Selecteer **.NET Core** > **ASP.NET Core-webtoepassing**. Voer een naam van het project en de naam van een oplossing en selecteer vervolgens **maken nieuwe Git-opslagplaats**.

   ![Schermopname van Visual Studio-wizard voor nieuw project](./media/asp-net-core/002-asp-net-core-web-application.png)

4. Selecteer **.NET Core** > **ASP.NET Core 2.0** **webtoepassing** > **OK**.

    ![Schermopname van Visual Studio nieuwe project sjabloon selecteren](./media/asp-net-core/003-web-application.png)

## <a name="application-insights-search"></a>Application Insights-zoekopdracht

In Visual Studio 2015 Update 2 of hoger met een ASP.NET Core 2 + op basis van-project, kunt u profiteren van [Application Insights-zoekopdracht](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio), zelfs voordat u Application Insights expliciet aan uw project toevoegen.

Deze functionaliteit testen:

1. Uw app uitvoeren. Als u wilt uitvoeren van uw app, selecteer de **IIS Express** pictogram (![schermopname van Visual Studio IIS Express pictogram](./media/asp-net-core/004-iis-express.png)).

2. Selecteer **weergave** > **andere Windows** > **Application Insights-zoekopdracht**.

   ![Schermopname van Visual Studio diagnostische hulpprogramma's voor selectie](./media/asp-net-core/005-view-other-windows-search.png)

3. De telemetrie van de sessie foutopsporing is momenteel alleen beschikbaar voor lokale analyse. Als volledig Application Insights, schakelt **telemetrie gereedheid** in de rechterbovenhoek rechterbovenhoek of volledig de stappen die worden vermeld in de volgende sectie.

   ![Schermopname van Visual Studio Application Insights zoeken](./media/asp-net-core/006-search.png)

> [!NOTE]
> Voor meer informatie over hoe Visual Studio verlichting van functies, zoals [Application Insights-zoekopdracht](../../azure-monitor/app/visual-studio.md) en [CodeLens](../../azure-monitor/app/visual-studio-codelens.md) lokaal voordat u Application Insights aan uw ASP.NET Core-project toevoegen, Zie [ Application Insights-zoekopdracht Vervolg](#application-insights-search-continued).

## <a name="add-application-insights-telemetry"></a>Application Insights Telemetry toevoegen

1. Selecteer **Project** > **Application Insights Telemetry toevoegen**. (Of u kunt met de rechtermuisknop op **Connected Services**, en selecteer vervolgens **Add Connected Service**.)

    ![Schermopname van Visual Studio-menu voor het selecteren van nieuw project](./media/asp-net-core/007-project-add-telemetry.png)

2. Selecteer **aan de slag**. (Afhankelijk van uw versie van Visual Studio, de tekst kan verschillen. Sommige oudere versies hebben een **gratis starten** knop in plaats daarvan.)

    ![Schermopname van Application Insights knop aan de slag](./media/asp-net-core/008-get-started.png)

3. Selecteer uw abonnement en selecteer vervolgens **Resource** > **registreren**.

## <a name="changes-made-to-your-project"></a>Wijzigingen aan uw project

Application Insights is weinig overhead. Om te controleren op wijzigingen aangebracht in uw project door Application Insights telemetry toevoegen:

Selecteer **weergave** > **Team Explorer** (Ctrl +\, Ctrl + M) > **Project** > **wijzigingen**

- Vier totaal aantal wijzigingen worden weergegeven:

  ![Schermafbeelding van de bestanden die zijn gewijzigd door het toevoegen van Application Insights](./media/asp-net-core/009-changes.png)

- Een nieuw bestand wordt gemaakt:

  -  _ConnectedService.json_

    ```json
    {
      "ProviderId": "Microsoft.ApplicationInsights.ConnectedService.ConnectedServiceProvider",
      "Version": "8.12.10405.1",
      "GettingStartedDocument": {
        "Uri": "https://go.microsoft.com/fwlink/?LinkID=798432"
      }
    }
    ```

- Drie bestanden zijn gewijzigd (aanvullende opmerkingen toegevoegd aan het markeren van wijzigingen):

  - _appSettings.JSON_:

    ```json
    {
      "Logging": {
        "IncludeScopes": false,
        "LogLevel": {
          "Default": "Warning"
        }
      },
    // Changes to file post adding Application Insights Telemetry:
      "ApplicationInsights": {
        "InstrumentationKey": "10101010-1010-1010-1010-101010101010"
      }
    }
    //
    ```

  - _ContosoDotNetCore.csproj_:

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
    <!--Changes to file post adding Application Insights Telemetry:-->
        <ApplicationInsightsResourceId>/subscriptions/2546c5a9-fa20-4de1-9f4a-62818b14b8aa/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/DotNetCore</ApplicationInsightsResourceId>
        <ApplicationInsightsAnnotationResourceId>/subscriptions/2546c5a9-fa20-4de1-9f4a-62818b14b8aa/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/DotNetCore</ApplicationInsightsAnnotationResourceId>
    <!---->
      </PropertyGroup>
      <ItemGroup>
    <!--Changes to file post adding Application Insights Telemetry:-->
        <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.1.1" />
    <!---->
        <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.8" />
      </ItemGroup>
      <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.4" />
      </ItemGroup>
    <!--Changes to file post adding Application Insights Telemetry:-->
      <ItemGroup>
        <WCFMetadata Include="Connected Services" />
      </ItemGroup>
    <!---->
    </Project>
    ```

  -  _Program.cs_:

      ```csharp
      using System;
      using System.Collections.Generic;
      using System.IO;
      using System.Linq;
      using System.Threading.Tasks;
      using Microsoft.AspNetCore;
      using Microsoft.AspNetCore.Hosting;
      using Microsoft.Extensions.Configuration;
      using Microsoft.Extensions.Logging;

      namespace DotNetCore
      {
          public class Program
          {
              public static void Main(string[] args)
              {
                  BuildWebHost(args).Run();
              }

              public static IWebHost BuildWebHost(string[] args) =>
                  WebHost.CreateDefaultBuilder(args)
      // Change to file post adding Application Insights Telemetry:
                      .UseApplicationInsights()
      //
                      .UseStartup<Startup>()
                      .Build();
          }
      }
      ```

## <a name="synthetic-transactions-with-powershell"></a>Synthetische transacties met PowerShell

Voor het automatiseren van aanvragen naar uw app met behulp van de synthetische transacties:

1. Als u wilt uitvoeren van uw app, selecteer de ![Schermopname van Visual Studio IIS Express-pictogram](./media/asp-net-core/004-iis-express.png) pictogram.

2. Kopieer de URL van de adresbalk van de browser. De URL heeft de indeling `http://localhost:<port number>`.

   ![Schermafbeelding van browser-URL in de adresbalk](./media/asp-net-core/0013-copy-url.png)

3. Voer de volgende PowerShell-lus voor het maken van 100 synthetische transacties met behulp van uw app testen. Wijzig het poortnummer na `localhost:` zodat deze overeenkomen met de URl die u in de vorige stap hebt gekopieerd. Bijvoorbeeld:

   ```PowerShell
   for ($i = 0 ; $i -lt 100; $i++)
   {
    Invoke-WebRequest -uri http://localhost:50984/
   }
   ```

## <a name="open-the-application-insights-portal"></a>Open de Application Insights-portal

Nadat u de PowerShell-opdrachten in de vorige sectie uitvoeren, opent u Application Insights om de transacties weer te geven en om te bevestigen dat gegevens worden verzameld. 

Selecteer in het menu van Visual Studio, **Project** > **Application Insights** > **Open Application Insights-Portal**.

   ![Schermopname van Application Insights-overzicht](./media/asp-net-core/010-portal.png)

> [!NOTE]
> In het voorgaande voorbeeld, **Live Stream**, **Paginalaadtijden**, en **mislukte aanvragen** worden niet verzameld. De volgende sectie helpt u met de stappen voor het toevoegen van elk van deze. Als u al verzamelt **Live Stream** en **Paginalaadtijden**, volg de stappen alleen voor **mislukte aanvragen**.

## <a name="collect-failed-requests-live-stream-and-page-view-load-time"></a>Mislukte aanvragen, Live Stream en laadtijd voor paginaweergave verzamelen

### <a name="failed-requests"></a>Mislukte aanvragen

In technisch opzicht kunnen mislukte aanvragen worden verzameld, maar er zijn geen mislukte aanvragen nog hebben plaatsgevonden. Als u wilt het proces versnellen, kunt u een aangepaste uitzondering toevoegen aan het bestaande project voor het simuleren van een echte-uitzondering. Als uw app wordt nog steeds uitgevoerd in Visual Studio, voordat u doorgaat, selecteert u **Stop Debugging** (Shift + F5).

1. In **Solution Explorer**, vouw **pagina's** > **About.cshtml**, en open vervolgens *About.cshtml.cs*.

   ![Schermopname van Visual Studio Solution Explorer](./media/asp-net-core/011-about.png)

2. Toevoegen van een uitzondering onder ``Message=``, en sla de wijziging naar het bestand.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc.RazorPages;

    namespace DotNetCore.Pages
    {
        public class AboutModel : PageModel
        {
            public string Message { get; set; }

            public void OnGet()
            {
                Message = "Your application description page.";
                throw new Exception("Test Exception");
            }
        }
    }
    ```

### <a name="live-stream"></a>Live Stream

Voor toegang tot de Live Stream-functionaliteit van Application Insights met ASP.NET Core, werken de Microsoft.ApplicationInsights.AspNetCore 2.2.0 NuGet-pakketten.

Selecteer in Visual Studio, **Project** > **NuGet-pakketten beheren** > **Microsoft.ApplicationInsights.AspNetCore** > (versie) **2.2.0** > **update**.

  ![Schermafbeelding van het NuGet-Pakketbeheer](./media/asp-net-core/012-nuget-update.png)

Meerdere vragen om bevestiging worden weergegeven. Lees en accepteer als u akkoord met de wijzigingen gaat.

### <a name="page-view-load-time"></a>Laadtijd voor paginaweergave

1. Ga in Visual Studio naar **Solution Explorer** > **pagina's**. U moet twee bestanden wijzigen: *Layout.cshtml* en *ViewImports.cshtml*.

2. In *ViewImports.cshtml*, voeg deze code toe:

   ```csharp
   @using Microsoft.ApplicationInsights.AspNetCore
   @inject JavaScriptSnippet snippet
   ```

3. In *Layout.cshtml*, voeg de volgende code voordat de ``</head>`` tag en voor eventuele andere scripts:

    ```csharp
    @Html.Raw(snippet.FullScript)
    ```

### <a name="test-failed-requests-page-view-load-time-and-live-stream"></a>Mislukte aanvragen, laadtijd voor paginaweergave, test- en Live Stream

Om te testen en controleren of alles werkt:

1. Uw app uitvoeren. Als u wilt uitvoeren van uw app, selecteer de ![Schermopname van Visual Studio IIS Express-pictogram](./media/asp-net-core/004-iis-express.png) pictogram.

2. Ga naar de **over** pagina voor het activeren van de test-uitzondering. (Als u in de foutopsporingsmodus in Visual Studio, selecteer **doorgaan** voor de uitzondering worden weergegeven in Application Insights.)

3. Opnieuw met het uitvoeren van de gesimuleerde PowerShell transactie-script, dat u eerder hebt gebruikt. (Mogelijk moet u het poortnummer in het script aanpassen.)

4. Als de **overzicht** pagina in Applications Insights nog niet is geopend, zich in het menu van Visual Studio, selecteer **Project** > **Application Insights**  >  **Application Insights-Portal openen**. 

   > [!TIP]
   > Als u uw nieuwe verkeer niet ziet, controleert u de waarde voor **tijdsbereik**, en selecteer vervolgens **vernieuwen**.

   ![Schermopname van het overzichtsvenster voor de](./media/asp-net-core/0019-overview-updated.png)

5. Selecteer **Live Stream**.

   ![Schermafbeelding van Live Metrics Stream](./media/asp-net-core/0020-live-metrics-stream.png)

   (Als uw PowerShell-script wordt nog steeds uitgevoerd, ziet u live metrische gegevens. Als uw PowerShell-script is gestopt, voer het script opnieuw uit met open Live Stream.)

## <a name="application-insights-sdk-comparison"></a>Vergelijking van Application Insights-SDK

De Application Insights-productgroep werkt hard aan bereiken functiepariteit tussen de [volledige .NET Framework SDK](https://github.com/Microsoft/ApplicationInsights-dotnet) en de .NET Core SDK. De 2.2.0 release van de [ASP.NET Core-SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore) voor Application Insights grotendeels de verschillen in functies steeds wordt gesloten.

De volgende tabel beschrijft meer van de verschillen en wisselwerking tussen [.NET en .NET Core](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server):

   | Vergelijking van de SDK | ASP.NET        | ASP.NET Core 2.1.0    | ASP.NET Core 2.2.0 |
  |:-- | :-------------: |:------------------------:|:----------------------:|
   | **Live metrics Stream**      | **+** |**-** | **+** |
   | **Telemetrie-kanaal van server** | **+** |**-** | **+**|
   |**Adaptieve steekproeven**| **+** | **-** | **+**|
   | **SQL-afhankelijkheidsaanroepen**     | **+** |**-** | **+**|
   | **Prestatiemeteritems*** | **+** | **-**| **-**|

Prestatiemeteritems in deze context verwijzen naar [serverzijde prestatiemeteritems](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) , zoals processor, geheugen en schijfgebruik.

## <a name="open-source-sdk"></a>Open-source-SDK
[Lees- en dragen bij aan de code](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates).

## <a name="application-insights-search-continued"></a>Vervolg van Application Insights-zoekopdracht

In deze sectie kunt u beter te begrijpen hoe Application Insights-zoekopdracht in Visual Studio werkt voor een ASP.NET Core 2-project. Het werkt op deze manier, zelfs wanneer u de Application Insights NuGet-pakketten nog expliciet nog niet hebt geïnstalleerd. Het kan ook handig om te controleren van de foutopsporingsuitvoer zijn.

Als u de uitvoer voor het woord zoekt _inzicht_, zijn gemarkeerd met het resultaat is vergelijkbaar met het volgende:

```DebugOutput
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.aspnetcore.applicationinsights.hostingstartup\2.0.3\lib\netcoreapp2.0\Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll'.
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.applicationinsights.aspnetcore\2.1.1\lib\netstandard1.6\Microsoft.ApplicationInsights.AspNetCore.dll'.

Application Insights Telemetry (unconfigured): {"name":"Microsoft.ApplicationInsights.Dev.Message","time":"2018-06-03T17:32:38.2796801Z","tags":{"ai.location.ip":"127.0.0.1","ai.operation.name":"DEBUG /","ai.internal.sdkVersion":"aspnet5c:2.1.1","ai.application.ver":"1.0.0.0","ai.cloud.roleInstance":"CONTOSO-SERVER","ai.operation.id":"de85878e-4618b05bad11b5a6","ai.internal.nodeName":"CONTOSO-SERVER","ai.operation.parentId":"|de85878e-4618b05bad11b5a6."},"data":{"baseType":"MessageData","baseData":{"ver":2,"message":"Request starting HTTP/1.1 DEBUG http://localhost:53022/  0","severityLevel":"Information","properties":{"AspNetCoreEnvironment":"Development","Protocol":"HTTP/1.1","CategoryName":"Microsoft.AspNetCore.Hosting.Internal.WebHost","Host":"localhost:53022","Path":"/","Scheme":"http","ContentLength":"0","DeveloperMode":"true","Method":"DEBUG"}}}}
```

In de uitvoer van de geladen CoreCLR twee assembly's: 

- _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_
- _Microsoft.ApplicationInsights.AspNetCore.dll_.

De _niet-geconfigureerd_ verwijzing in elk exemplaar van Application Insights telemetry geeft aan dat deze toepassing is niet gekoppeld aan een ikey. De gegevens die gegenereerd tijdens het uitvoeren van uw app is niet verzonden naar Azure. De gegevens zijn alleen beschikbaar voor lokaal zoeken en analyse.

De functionaliteit is mogelijk deels omdat het NuGet-pakket _Microsoft.AspNetCore.All_ duurt [ _Microsoft.ASPNetCoreApplicationInsights.HostingStartup_ ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.applicationinsights.hostingstartup.applicationinsightshostingstartup?view=aspnetcore-2.1) Als een afhankelijkheid.

![Schermafbeelding van NuGet afhankelijkheidsgrafiek voor Microsoft.AspNETCore.all](./media/asp-net-core/013-dependency.png)

Buiten Visual Studio, als u een ASP.NET Core-project in VSCode, of een andere editor die zijn bewerkt wouldn't deze assembly's automatisch laden tijdens de foutopsporing als u Application Insights expliciet hebt toegevoegd aan uw project.

Echter deze verlichting van lokale Application Insights-functies van externe assembly's in Visual Studio wordt gerealiseerd met behulp van de [IHostingStartup Interface](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup?view=aspnetcore-2.1). Application Insights de interface dynamisch toegevoegd tijdens de foutopsporing.

Meer informatie over het verbeteren van een app uit een [externe assembly in ASP.NET Core met IHostingStartup](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/platform-specific-configuration?view=aspnetcore-2.1). 

### <a name="disable-application-insights-in-visual-studio-net-core-projects"></a>Uitschakelen van Application Insights in Visual Studio .NET Core-projecten

Hoewel de automatische licht-up van Application Insights zoeken functionaliteit mogelijk handig ziet foutopsporing telemetrie wordt gegenereerd wanneer u het kan verwarrend zijn niet verwacht.

Als alleen uitschakelen telemetrie genereren voldoende is, kunt u deze code blokkeren als u wilt toevoegen de **configureren** -methode van uw _Startup.cs_ bestand:

```csharp
  var configuration = app.ApplicationServices.GetService<Microsoft.ApplicationInsights.Extensibility.TelemetryConfiguration>();
            configuration.DisableTelemetry = true;
            if (env.IsDevelopment())
```

CoreCLR nog steeds geladen _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_ en _Microsoft.ApplicationInsights.AspNetCore.dll_, maar de bestanden niet iets doen.

Als u uitschakelen volledig Application Insights in Visual Studio .NET Core-project wilt, de aanbevolen methode is om te selecteren **extra** > **opties**  >   **Projecten en oplossingen** > **Web-projecten**. Selecteer de **uitschakelen lokale Application Insights voor ASP.NET Core web-projecten** selectievakje. Deze functionaliteit is toegevoegd in Visual Studio 15,6.

![Schermopname van Visual Studio-opties venster webprojecten scherm](./media/asp-net-core/014-disable.png)

Als u een eerdere versie van Visual Studio uitvoert en u wilt voor het verwijderen van alle assembly's die zijn geladen via *IHostingStartup*, hebt u twee opties:

* Voeg `.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")` naar _Program.cs_:

  ```csharp
  using System;
  using System.Collections.Generic;
  using System.IO;
  using System.Linq;
  using System.Threading.Tasks;
  using Microsoft.AspNetCore;
  using Microsoft.AspNetCore.Hosting;
  using Microsoft.Extensions.Configuration;
  using Microsoft.Extensions.Logging;

  namespace DotNetCore
  {
      public class Program
      {
          public static void Main(string[] args)
          {
              BuildWebHost(args).Run();
          }

          public static IWebHost BuildWebHost(string[] args) =>
              WebHost.CreateDefaultBuilder(args)
                  .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
                  .UseStartup<Startup>()
                  .Build();
      }
  }
  ```

* Voeg ``"ASPNETCORE_preventHostingStartup": "True"`` naar _launchSettings.json_ omgevingsvariabelen.

Het probleem met het gebruik van een van deze methoden is dat ze alleen Application Insights niet uitschakelen. De methoden uitschakelen in Visual Studio die werd gebruikt ook de *IHostingStartup* licht-up-functionaliteit.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="next-steps"></a>Volgende stappen
* [Verken Gebruikersstromen](../../azure-monitor/app/usage-flows.md) om te begrijpen hoe gebruikers navigeren via uw app.
* [Momentopname verzamelen configureren](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger#configure-snapshot-collection-for-aspnet-core-20-applications) om te zien van de status van de broncode en variabelen op dit moment wordt er een uitzondering gegenereerd.
* [Gebruik de API](../../azure-monitor/app/api-custom-events-metrics.md) voor het verzenden van uw eigen gebeurtenissen en metrische gegevens voor een gedetailleerdere weergave van de prestaties en het gebruik van uw app.
* Gebruik [beschikbaarheidstests](../../azure-monitor/app/monitor-web-app-availability.md) om te controleren of uw app continu van over de hele wereld.
