---
title: Azure Application Insights voor ASP.NET Core | Microsoft Docs
description: Bewaak web-apps voor beschikbaarheid, prestaties en gebruik.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/03/2018
ms.author: mbullwin
ms.openlocfilehash: f9ab9b9af81bf1827c2da646908e204bd051706b
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970931"
---
# <a name="application-insights-for-aspnet-core"></a>Application Insights voor ASP.NET Core

Azure Application Insights biedt uitgebreide bewaking van uw webtoepassing op het codeniveau. U kunt eenvoudig uw web-App voor beschikbaarheid, prestaties en gebruik bewaken. U kunt ook snel fouten in de toepassing identificeren en er een diagnose voor uitvoeren, zonder dat u hoeft te wachten totdat een gebruiker ze heeft gerapporteerd.

In dit artikel wordt beschreven hoe u een voorbeeld van een ASP.NET Core [Razor Pages](https://docs.microsoft.com/aspnet/core/mvc/razor-pages/?tabs=visual-studio) toepassing in Visual Studio en over het bewaken met Azure Application Insights.

## <a name="prerequisites"></a>Vereisten

- NET Core 2.0.0 SDK of hoger.
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) versie 15.7.3 of hoger met de ASP.NET- en workload voor ontwikkeling. 

## <a name="create-an-aspnet-core-project-in-visual-studio"></a>Een ASP.NET Core-project in Visual Studio maken

1. Met de rechtermuisknop op en start **Visual Studio 2017** als administrator.
2. Selecteer **bestand** > **nieuwe** > **Project** (Ctrl-Shift-N).

   ![Schermopname van het nieuwe Projectmenu van Visual Studio-bestand](./media/app-insights-asp-net-core/001-new-project.png)

3. Vouw **Visual C#** > Selecteer **.NET Core** > **ASP.NET Core-webtoepassing**. Voer een **naam** > **oplossingsnaam** > Controleer **maken nieuwe Git-opslagplaats**.

   ![Schermafbeelding van de Wizard Nieuw Project bestand Visual Studio](./media/app-insights-asp-net-core/002-asp-net-core-web-application.png)

4. Selecteer **.Net Core** > **ASP.NET Core 2.0** **webtoepassing** > **OK**.

    ![Schermopname van het nieuwe Project selectie Menu van Visual Studio-bestand](./media/app-insights-asp-net-core/003-web-application.png)

## <a name="application-insights-search"></a>Application Insights-zoekopdracht

Standaard in Visual Studio 2015 Update 2 of hoger met een ASP.NET Core 2 + op basis van consoletoepassingsproject dat u versie kan profiteren van [Application Insights-zoekopdracht](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio) zelfs nog voordat u expliciet Application Insights toevoegen aan uw project.

Deze functionaliteit testen:

1. Uw app uitvoeren door te klikken op IIS Express ![Schermopname van Visual Studio IIS Express-pictogram](./media/app-insights-asp-net-core/004-iis-express.png)

2. Selecteer **weergave** > **andere Windows** > **Application Insights-zoekopdracht**.

   ![Schermafbeelding van diagnostische hulpprogramma's voor Visual Studio](./media/app-insights-asp-net-core/005-view-other-windows-search.png)

3. De telemetrie van de sessie foutopsporing is momenteel beschikbaar voor alleen lokale analyse. Als volledig Application Insights, schakelt **telemetrie gereedheid** in de rechterbovenhoek rechts of volg de stappen in de volgende sectie.

   ![Schermopname van Visual Studio Application Insights zoeken](./media/app-insights-asp-net-core/006-search.png)

> [!NOTE]
> Voor meer informatie over hoe Visual Studio verlichting van functies, zoals [Application Insights-zoekopdracht](app-insights-visual-studio.md) en [CodeLens](app-insights-visual-studio-codelens.md) lokaal voordat u hebt toegevoegd Application Insights aan uw ASP.NET Core-project uitchecken de uitleg bij de [einde van dit artikel.](#Application-Insights-search-continued)

## <a name="add-application-insights-telemetry"></a>Application Insights Telemetry toevoegen

1. Selecteer **Project** > **Application Insights Telemetry toevoegen...** . (Of u kunt met de rechtermuisknop op **Connected Services** en selecteer Add Connected Service.)

    ![Schermopname van het nieuwe Project selectie Menu van Visual Studio-bestand](./media/app-insights-asp-net-core/007-project-add-telemetry.png)

2. Selecteer **aan de slag**. (Afhankelijk van uw versie van Visual Studio de tekst mogelijk enigszins afwijken. Sommige oudere versies hebben in plaats daarvan een **gratis starten** knop.)

    ![Schermopname van het nieuwe Project selectie Menu van Visual Studio-bestand](./media/app-insights-asp-net-core/008-get-started.png)

3. Selecteer een geschikte **abonnement** > **Resource** > **registreren**.

## <a name="changes-made-to-your-project"></a>Wijzigingen maken aan uw project

Application Insights is weinig overhead. Om te controleren op wijzigingen aangebracht in uw project door Application Insights telemetry toevoegen:

Selecteer **weergave** > **Team Explorer** (Ctrl +\, Ctrl + M) > **Project** > **wijzigingen**

- Totaal aantal vier wijzigingen:

  ![Schermafbeelding van de bestanden die zijn gewijzigd door het toevoegen van Application Insights](./media/app-insights-asp-net-core/009-changes.png)

- Een nieuw bestand wordt gemaakt:

   _ConnectedService.json_

```json
{
  "ProviderId": "Microsoft.ApplicationInsights.ConnectedService.ConnectedServiceProvider",
  "Version": "8.12.10405.1",
  "GettingStartedDocument": {
    "Uri": "https://go.microsoft.com/fwlink/?LinkID=798432"
  }
}
```

- Drie bestanden zijn gewijzigd: (aanvullende opmerkingen toegevoegd aan het markeren van wijzigingen)

  _appsettings.json_

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

  _ContosoDotNetCore.csproj_

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

   _Program.cs_

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

Voor het automatiseren van aanvragen voor uw app met synthetische transacties.

1. Uw app uitvoeren door te klikken op IIS Express ![Schermopname van Visual Studio IIS Express-pictogram](./media/app-insights-asp-net-core/004-iis-express.png)

2. Kopieer de url van de adresbalk van de browser. Het is in de indeling http://localhost:{random poortnummer}

   ![Schermafbeelding van de adresbalk van de browser-url](./media/app-insights-asp-net-core/0013-copy-url.png)

3. Voer de volgende PowerShell-lus om 100 synthetische transacties op basis van uw test-app te maken. Wijzig het poortnummer na **localhost:** zodat deze overeenkomen met de url die u in de vorige stap hebt gekopieerd.

   ```PowerShell
   for ($i = 0 ; $i -lt 100; $i++)
   {
    Invoke-WebRequest -uri http://localhost:50984/
   }
   ```

## <a name="open-application-insights-portal"></a>Open Application Insights-Portal

Start na het uitvoeren van de PowerShell uit de vorige sectie, Application Insights voor het weergeven van de transacties en bevestigt u dat gegevens worden verzameld. 

Selecteer in het menu van Visual Studio, **Project** > **Application Insights** > **Open Application Insights-Portal**

   ![Schermopname van Application Insights-overzicht](./media/app-insights-asp-net-core/010-portal.png)

> [!NOTE]
> In het bovenstaande voorbeeld **Live Stream**, **Paginalaadtijden**, en **mislukte aanvragen** worden momenteel niet worden verzameld. De volgende sectie helpt bij het toevoegen van elk. Als u al zijn verzameld **Live Stream**, en **Paginalaadtijden**, volgt u de stappen voor het alleen **mislukte aanvragen**.

## <a name="collect-failed-requests-live-stream--page-view-load-time"></a>Verzamelen van mislukte aanvragen, Live Stream & laadtijd voor paginaweergave

### <a name="failed-requests"></a>Mislukte aanvragen

Technisch **mislukte aanvragen** worden verzameld, maar er zijn geen nog zijn opgetreden. Als u wilt versnellen langs een aangepaste uitzondering kan worden toegevoegd aan het bestaande project voor het simuleren van een echte-uitzondering. Als uw app wordt nog steeds uitgevoerd in Visual Studio voordat u doorgaat **Stop Debugging** (Shift + F5)

1. In **Solution Explorer** > Vouw **pagina's** > **About.cshtml** > open **About.cshtml.cs**.

   ![Schermopname van Visual Studio Solution Explorer](./media/app-insights-asp-net-core/011-about.png)

2. Toevoegen van een uitzondering onder ``Message=`` en opslaan van de wijziging in het bestand.

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

Toegang tot de Live Stream-functionaliteit van Application Insights met ASP.NET Core-update voor de **Microsoft.ApplicationInsights.AspNetCore 2.2.0** NuGet-pakketten.

Selecteer in Visual Studio, **Project** > **NuGet-pakketten beheren** > **Microsoft.ApplicationInsights.AspNetCore** > versie **2.2.0** > **update**.

  ![Schermafbeelding van het NuGet-Pakketbeheer](./media/app-insights-asp-net-core/012-nuget-update.png)

Meerdere vragen om bevestiging wordt uitgevoerd. Lees en accepteer als u akkoord met de wijzigingen gaat.

### <a name="page-view-load-time"></a>Laadtijd voor paginaweergave

1. Ga in Visual Studio naar **Solution Explorer** > **pagina's** > twee bestanden moet worden gewijzigd: _Layout.cshtml_, en  _ViewImports.cshtml_

2. In __ViewImports.cshtml_, toevoegen:

   ```csharp
   @using Microsoft.ApplicationInsights.AspNetCore
   @inject JavaScriptSnippet snippet
   ```

3. In **_Layout.cshtml** toevoegen van de regel onder voordat de ``</head>`` tag, maar ook vóór alle andere scripts.

    ```csharp
    @Html.Raw(snippet.FullScript)
    ```

### <a name="test-failed-requests-page-view-load-time-live-stream"></a>De test is mislukt voor aanvragen, laadtijd voor paginaweergave, Live Stream

Te testen en te bevestigen dat alles werkt:

1. Uw app uitvoeren door te klikken op IIS Express ![Schermopname van Visual Studio IIS Express-pictogram](./media/app-insights-asp-net-core/0012-iis-express.png)

2. Navigeer naar de **over** pagina voor het activeren van de test-uitzondering. (Als u in de foutopsporingsmodus uitvoert, moet u klikken op **doorgaan** in Visual Studio voor de uitzondering worden weergegeven in Application Insights.)

3. Opnieuw uitvoeren van het gesimuleerde PowerShell transactie-script uit eerdere (mogelijk moet u het poortnummer in het script aanpassen.)

4. Als het overzicht van Applications Insights niet nog steeds geopend vanuit Visual Studio menu Selecteer is **Project** > **Application Insights** > **toepassing openen Insights-Portal**. 

   > [!TIP]
   > Als u uw nieuwe verkeer nog niet ziet, controleert u de **tijdsbereik** en klikt u op **vernieuwen**.

   ![Schermafbeelding van het overzichtsvenster](./media/app-insights-asp-net-core/0019-overview-updated.png)

5. Live Stream selecteren

   ![Schermafbeelding van Live Metrics Stream](./media/app-insights-asp-net-core/0020-live-metrics-stream.png)

   (Als uw PowerShell script wordt nog steeds uitgevoerd u ziet in live metrics stream, als deze is gestopt uitvoeren een het script opnieuw uit met Live Stream dat geopend.)

## <a name="app-insights-sdk-comparison"></a>Vergelijking van App Insights-SDK

De Application Insights-productgroep werkt hard aan bereiken functiepariteit tussen de [volledige .NET Framework SDK](https://github.com/Microsoft/ApplicationInsights-dotnet) en de .net Core-SDK. De 2.2.0 release van de [ASP.NET Core-SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore) voor Application Insights heeft de verschillen in functies steeds grotendeels gesloten.

Meer informatie geven over de verschillen en zorgen voor een balans tussen [.NET en .NET Core](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).

   | Vergelijking van de SDK | ASP.NET        | ASP.NET Core 2.1.0    | ASP.NET Core 2.2.0 |
  |:-- | :-------------: |:------------------------:|:----------------------:|
   | **Live metrics Stream**      | **+** |**-** | **+** |
   | **Telemetrie-kanaal van server** | **+** |**-** | **+**|
   |**Adaptieve steekproeven**| **+** | **-** | **+**|
   | **SQL-Afhankelijkheidsaanroepen**     | **+** |**-** | **+**|
   | **Prestatiemeteritems*** | **+** | **-**| **-**|

_Prestatiemeteritems_ in deze context verwijst naar [serverzijde prestatiemeteritems](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) , zoals processor, geheugen en schijfgebruik.

## <a name="open-source-sdk"></a>Open-source-SDK
[Lees- en dragen bij aan de code](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)

## <a name="application-insights-search-continued"></a>Application Insights-zoekopdracht vervolg

Voor een beter te begrijpen hoe Application Insights-zoekopdracht werkt in Visual Studio voor een ASP.NET Core 2-project, zelfs wanneer een expliciete installatie van de Application Insights NuGet-pakketten is nog niet hebt opgetreden. Kan het handig zijn om te controleren van de uitvoer voor foutopsporing.

Als u de uitvoer voor het woord zoekt _inzicht_ wordt het resultaat is vergelijkbaar met de volgende markeren:

```DebugOuput
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.aspnetcore.applicationinsights.hostingstartup\2.0.3\lib\netcoreapp2.0\Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll'.
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.applicationinsights.aspnetcore\2.1.1\lib\netstandard1.6\Microsoft.ApplicationInsights.AspNetCore.dll'.

Application Insights Telemetry (unconfigured): {"name":"Microsoft.ApplicationInsights.Dev.Message","time":"2018-06-03T17:32:38.2796801Z","tags":{"ai.location.ip":"127.0.0.1","ai.operation.name":"DEBUG /","ai.internal.sdkVersion":"aspnet5c:2.1.1","ai.application.ver":"1.0.0.0","ai.cloud.roleInstance":"CONTOSO-SERVER","ai.operation.id":"de85878e-4618b05bad11b5a6","ai.internal.nodeName":"CONTOSO-SERVER","ai.operation.parentId":"|de85878e-4618b05bad11b5a6."},"data":{"baseType":"MessageData","baseData":{"ver":2,"message":"Request starting HTTP/1.1 DEBUG http://localhost:53022/  0","severityLevel":"Information","properties":{"AspNetCoreEnvironment":"Development","Protocol":"HTTP/1.1","CategoryName":"Microsoft.AspNetCore.Hosting.Internal.WebHost","Host":"localhost:53022","Path":"/","Scheme":"http","ContentLength":"0","DeveloperMode":"true","Method":"DEBUG"}}}}
```

CoreCLR is het laden van twee assembly's: 

- _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_
- _Microsoft.ApplicationInsights.AspNetCore.dll_.

En de _niet-geconfigureerd_ in elk exemplaar van Application Insights telemetrie wordt aangegeven dat deze toepassing niet gekoppeld aan een ikey, zodat de gegevens die is gegenereerd tijdens het uitvoeren van uw app wordt niet naar Azure worden verzonden en alleen is beschikbaar voor lokaal zoeken en analyseren.

Onderdeel van hoe dit mogelijk is dat het NuGet-pakket _Microsoft.AspNetCore.All_ wordt gebruikt als een afhankelijkheid [ _Microsoft.ASPNetCoreApplicationInsights.HostingStartup_](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.applicationinsights.hostingstartup.applicationinsightshostingstartup?view=aspnetcore-2.1)

![Schermafbeelding van NuGet afhankelijkheid Graph voor Microsoft.AspNETCore.all](./media/app-insights-asp-net-core/013-dependency.png)

Buiten Visual Studio als u een ASP.NET Core-project in VSCode of een andere editor hebt bewerkt wouldn't deze assembly's automatisch laden tijdens de foutopsporing als u Application Insights expliciet hebt toegevoegd aan uw project.

Maar deze verlichting van lokale Application Insights-functies van externe assembly's in Visual Studio wordt bereikt via het gebruik van de [IHostingStartup Interface](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup?view=aspnetcore-2.1) die Application Insights dynamisch worden toegevoegd tijdens de foutopsporing.

Voor meer informatie over het verbeteren van een app uit een [externe assembly in ASP.NET Core met IHostingStartup](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/platform-specific-configuration?view=aspnetcore-2.1). 

### <a name="how-to-disable-application-insights-in-visual-studio-net-core-projects"></a>Het uitschakelen van Application Insights in Visual Studio .NET Core-projecten

Tijdens de automatische licht van Application Insights-zoekfunctionaliteit nuttig voor sommige zijn kan, kan zien foutopsporing telemetrie gegenereerd wanneer u deze niet waren verwacht verwarrend zijn.

Als alleen uitschakelen telemetrie genereren voldoende is, kunt u dit codeblok toevoegen aan de methode configureren van uw _Startup.cs_ bestand:

```csharp
  var configuration = app.ApplicationServices.GetService<Microsoft.ApplicationInsights.Extensibility.TelemetryConfiguration>();
            configuration.DisableTelemetry = true;
            if (env.IsDevelopment())
```

De CoreCLR wordt nog steeds geladen _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_ en _Microsoft.ApplicationInsights.AspNetCore.dll_, maar ze niet alles.

Als u uitschakelen volledig Application Insights in Visual Studio .NET Core-project wilt, de aanbevolen methode is om te selecteren **extra** > **opties**  >   **Projecten en oplossingen** > **webprojecten** > en schakel het selectievakje in als u wilt uitschakelen, lokale Application Insights voor ASP.NET Core web-projecten. Deze functionaliteit is toegevoegd in Visual Studio 15,6.

![Schermopname van Visual Studio-opties venster webprojecten scherm](./media/app-insights-asp-net-core/014-disable.png)

Als u een eerdere versie van Visual Studio uitvoert en u wilt voor het verwijderen van alle assembly's geladen via IHostingStartup kunt u toevoegen:

`.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")`

naar _Program.cs_:

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

Of u kunt ook zou u ``"ASPNETCORE_preventHostingStartup": "True"`` naar _launchSettings.json_ omgevingsvariabelen.

Het probleem met het gebruik van een van deze methoden is dat het niet alleen Application Insights wordt uitgeschakeld in Visual Studio die is gebruik te maken van het licht IHostingStartup van functionaliteit uitgeschakeld.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="next-steps"></a>Volgende stappen
* [Verken gebruikers stromen](app-insights-usage-flows.md) om te begrijpen hoe gebruikers navigeren via uw app.
* [Momentopname verzamelen configureren](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger#configure-snapshot-collection-for-aspnet-core-20-applications) om te zien van de status van de broncode en variabelen op dit moment wordt er een uitzondering gegenereerd.
* [Gebruik de API](app-insights-api-custom-events-metrics.md) voor het verzenden van uw eigen gebeurtenissen en metrische gegevens voor een gedetailleerdere weergave van de prestaties en het gebruik van uw app.
* [Beschikbaarheidstests](app-insights-monitor-web-app-availability.md) controleren van uw app continu van over de hele wereld.
