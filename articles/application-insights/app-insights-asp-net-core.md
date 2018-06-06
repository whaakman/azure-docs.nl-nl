---
title: Azure Application Insights voor ASP.NET Core | Microsoft Docs
description: Bewaken van webtoepassingen voor beschikbaarheid, prestaties en het gebruik.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/03/2018
ms.author: mbullwin
ms.openlocfilehash: 459fbbe734577af474fae8de3acf9be732780d6c
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34736531"
---
# <a name="application-insights-for-aspnet-core"></a>Application Insights voor ASP.NET Core

Azure Application Insights biedt uitgebreide bewaking van uw webtoepassing op het codeniveau. U kunt eenvoudig uw webtoepassing voor beschikbaarheid, prestaties en gebruik bewaken. U kunt ook snel fouten in de toepassing identificeren en er een diagnose voor uitvoeren, zonder dat u hoeft te wachten totdat een gebruiker ze heeft gerapporteerd.

Dit artikel begeleidt u bij het maken van een voorbeeld van een ASP.NET Core [Razor pagina's](https://docs.microsoft.com/aspnet/core/mvc/razor-pages/?tabs=visual-studio) toepassing in Visual Studio en hoe u bewaking starten met Azure Application Insights.

## <a name="prerequisites"></a>Vereisten

- NET Core 2.0.0 SDK of hoger.
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) versie 15.7.3 of hoger, waarop de werkbelasting van de ontwikkeling van ASP.NET en Internet. 

## <a name="create-an-aspnet-core-project-in-visual-studio"></a>Een ASP.NET Core-project in Visual Studio maken

1. Met de rechtermuisknop op en start **Visual Studio 2017** als administrator.
2. Selecteer **bestand** > **nieuwe** > **Project** (Ctrl-Shift-N).

   ![Schermopname van nieuw Project Menu van Visual Studio-bestand](./media/app-insights-asp-net-core/001-new-project.png)

3. Vouw **Visual C#** > Selecteer **.NET Core** > **ASP.NET Core webtoepassing**. Voer een **naam** > **oplossingsnaam** > controleren **maken nieuwe Git-opslagplaats**.

   ![Schermafbeelding van de Wizard Nieuw Project bestand Visual Studio](./media/app-insights-asp-net-core/002-asp-net-core-web-application.png)

4. Selecteer **.Net Core** > **ASP.NET Core 2.0** **webtoepassing** > **OK**.

    ![Schermopname van nieuw Project selectie Menu van Visual Studio-bestand](./media/app-insights-asp-net-core/003-web-application.png)

## <a name="application-insights-search"></a>Application Insights-zoekopdracht

Standaard in Visual Studio 2015 Update 2 of hoger met een ASP.NET Core 2 + op basis van project u versie kunt profiteren van [Application Insights zoeken](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-visual-studio) zelfs nog voordat u expliciet Application Insights toevoegen aan uw project.

Deze functionaliteit testen:

1. Uw app uitvoeren door te klikken op IIS Express ![Schermopname van Visual Studio IIS Express-pictogram](./media/app-insights-asp-net-core/004-iis-express.png)

2. Selecteer **weergave** > **overige vensters** > **Application Insights zoeken**.

   ![Schermafbeelding van de diagnostische hulpprogramma's voor Visual Studio](./media/app-insights-asp-net-core/005-view-other-windows-search.png)

3. De telemetrie van de sessie foutopsporing is momenteel beschikbaar voor alleen lokale analyse. U kunt Application Insights, selecteer **telemetrie gereedheid** in de linkerbovenhoek rechts of de stappen in de volgende sectie.

   ![Schermopname van Visual Studio Application Insights zoeken](./media/app-insights-asp-net-core/006-search.png)

> [!NOTE]
> Voor meer informatie over hoe lichten Visual Studio om functies, zoals [Application Insights zoeken](app-insights-visual-studio.md) en [CodeLens](app-insights-visual-studio-codelens.md) lokaal voordat u hebt toegevoegd Application Insights aan uw project ASP.NET Core Bekijk de uitleg bij de [einde van dit artikel.](#Application-Insights-search-continued)

## <a name="add-application-insights-telemetry"></a>Application Insights Telemetry toevoegen

1. Selecteer **Project** > **Application Insights Telemetry toevoegen...** . (Of u kunt met de rechtermuisknop **verbonden Services** en selecteer verbonden Service toevoegen.)

    ![Schermopname van nieuw Project selectie Menu van Visual Studio-bestand](./media/app-insights-asp-net-core/007-project-add-telemetry.png)

2. Selecteer **aan de slag**. (Afhankelijk van uw versie van Visual Studio de tekst mogelijk enigszins afwijken. Sommige oudere versies hebben in plaats daarvan een **gratis Start** knop.)

    ![Schermopname van nieuw Project selectie Menu van Visual Studio-bestand](./media/app-insights-asp-net-core/008-get-started.png)

3. Selecteer een geschikte **abonnement** > **Resource** > **registreren**.

## <a name="changes-made-to-your-project"></a>Wijzigingen maken aan uw project

Application Insights is weinig overhead. De aangebrachte door telemetrie voor Application Insights toe te voegen aan uw project bekijken:

Selecteer **weergave** > **Explorer Team** (Ctrl +\, Ctrl + M) > **Project** > **wijzigingen**

- Totaal vier wijzigingen:

  ![Schermopname van bestanden die zijn gewijzigd door Application Insights toe te voegen](./media/app-insights-asp-net-core/009-changes.png)

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

Voor het automatiseren van aanvragen op basis van uw app met synthetische transacties.

1. Uw app uitvoeren door te klikken op IIS Express ![Schermopname van Visual Studio IIS Express-pictogram](./media/app-insights-asp-net-core/004-iis-express.png)

2. Kopieer de url van de adresbalk van de browser. Het is in de notatie http://localhost:{random poortnummer}

   ![Schermafbeelding van de adresbalk van de browser-url](./media/app-insights-asp-net-core/0013-copy-url.png)

3. Voer de volgende PowerShell-lus voor het maken van 100 synthetische transacties op basis van uw app testen. Wijzig het poortnummer na **localhost:** overeenkomen met de url die u in de vorige stap hebt gekopieerd.

   ```PowerShell
   for ($i = 0 ; $i -lt 100; $i++)
   {
    Invoke-WebRequest -uri http://localhost:50984/
   }
   ```

## <a name="open-application-insights-portal"></a>Open Application Insights-Portal

Start na het uitvoeren van de PowerShell uit de vorige sectie, Application Insights om de transacties bekijken en te bevestigen dat gegevens worden verzameld. 

Selecteer in het menu Visual Studio **Project** > **Application Insights** > **Open Application Insights-Portal**

   ![Schermopname van Application Insights-overzicht](./media/app-insights-asp-net-core/010-portal.png)

> [!NOTE]
> In het bovenstaande voorbeeld **Live Stream**, **laadtijd voor paginaweergave**, en **mislukte aanvragen voor** worden momenteel niet worden verzameld. De volgende sectie begeleidt bij het toevoegen van elk. Als u al verzamelt **Live Stream**, en **laadtijd voor paginaweergave**, volgt u de stappen voor het alleen **mislukte aanvragen voor**.

## <a name="collect-failed-requests-live-stream--page-view-load-time"></a>Verzamelen van mislukte aanvragen, Live Stream & laadtijd voor paginaweergave

### <a name="failed-requests"></a>Mislukte aanvragen

Technisch **mislukte aanvragen voor** worden verzameld, maar geen nog hebben plaatsgevonden. Als u wilt versnellen langs een aangepaste uitzondering kunnen worden toegevoegd aan het bestaande project om te simuleren een echte-uitzondering. Als uw app wordt nog steeds uitgevoerd in Visual Studio voordat u doorgaat **Stop Debugging** (Shift + F5)

1. In **Solution Explorer** > Vouw **pagina's** > **About.cshtml** > open **About.cshtml.cs**.

   ![Schermafbeelding van de Visual Studio Solution Explorer](./media/app-insights-asp-net-core/011-about.png)

2. Toevoegen van een uitzondering onder ``Message=`` en sla de wijziging in het bestand.

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

Voor toegang tot de Live Stream-functionaliteit van Application Insights met ASP.NET Core update om de **Microsoft.ApplicationInsights.AspNetCore 2.2.0** NuGet-pakketten.

Selecteer in Visual Studio **Project** > **NuGet-pakketten beheren** > **Microsoft.ApplicationInsights.AspNetCore** > versie **2.2.0** > **update**.

  ![Schermopname van het NuGet-Pakketbeheer](./media/app-insights-asp-net-core/012-nuget-update.png)

Meerdere bevestiging vragen uitgevoerd. Lees en accepteer als u akkoord met de wijzigingen gaat.

### <a name="page-view-load-time"></a>Laadtijd voor paginaweergave

1. Ga in Visual Studio naar **Solution Explorer** > **pagina's** > twee bestanden moet worden gewijzigd: _Layout.cshtml_, en  _ViewImports.cshtml_

2. In __ViewImports.cshtml_, toevoegen:

   ```csharp
   @using Microsoft.ApplicationInsights.AspNetCore
   @inject JavaScriptSnippet snippet
   ```

3. In **_Layout.cshtml** toevoegen van de regel onder voordat de ``</head>`` label, maar ook vóór alle andere scripts.

    ```csharp
    @Html.Raw(snippet.FullScript)
    ```

### <a name="test-failed-requests-page-view-load-time-live-stream"></a>Test is mislukt-aanvragen, laadtijd voor paginaweergave, Live Stream

Testen en controleren of alles werkt:

1. Uw app uitvoeren door te klikken op IIS Express ![Schermopname van Visual Studio IIS Express-pictogram](./media/app-insights-asp-net-core/0012-iis-express.png)

2. Navigeer naar de **over** pagina voor het activeren van de test-uitzondering. (Als u in de foutopsporingsmodus uitvoert, moet u klikken op **doorgaan** in Visual Studio voor de uitzondering worden weergegeven in Application Insights.)

3. Het gesimuleerde PowerShell transactie-script uit eerder opnieuw uitvoeren (mogelijk moet u het poortnummer in het script aanpassen.)

4. Als het overzicht van de Insights toepassingen niet nog geopend vanuit Visual Studio menu Selecteer **Project** > **Application Insights** > **toepassing openen Insights-Portal**. 

   > [!TIP]
   > Als u uw nieuwe verkeer nog worden niet ziet, Controleer de **tijdsbereik** en klik op **vernieuwen**.

   ![Schermopname van overzicht](./media/app-insights-asp-net-core/0019-overview-updated.png)

5. Selecteer Live Stream

   ![Schermafbeelding van de Stream Live metrische gegevens](./media/app-insights-asp-net-core/0020-live-metrics-stream.png)

   (Als uw PowerShell script wordt nog steeds uitgevoerd u ziet live metrische gegevens en als deze is gestopt uitvoeren voor het script opnieuw Live Stream openen)

## <a name="app-insights-sdk-comparison"></a>Vergelijking van App Insights-SDK

De Application Insights-productgroep werken hard aan bereiken functie pariteit tussen de [.NET Framework SDK volledige](https://github.com/Microsoft/ApplicationInsights-dotnet) en de .net Core SDK. De 2.2.0 release van de [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore) voor de afstand van de functie grotendeels door Application Insights is gesloten.

Voor meer informatie over de verschillen en de wisselwerking tussen [.NET en .NET Core](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).

   | Vergelijking van de SDK | ASP.NET        | ASP.NET Core 2.1.0    | ASP.NET Core 2.2.0 |
  |:-- | :-------------: |:------------------------:|:----------------------:|
   | **Live metrische gegevens**      | **+** |**-** | **+** |
   | **Telemetrie-serverkanaal** | **+** |**-** | **+**|
   |**Adaptieve steekproeven**| **+** | **-** | **+**|
   | **Aanroepen van de SQL-afhankelijkheden**     | **+** |**-** | **+**|
   | **Prestatiemeteritems*** | **+** | **-**| **-**|

_Prestatiemeteritems_ in deze context verwijst naar [serverzijde prestatiemeteritems](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) , zoals processors, geheugen en schijfgebruik.

## <a name="open-source-sdk"></a>Open-source SDK
[Lees- en bijdragen aan de code](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)

## <a name="application-insights-search-continued"></a>Application Insights zoeken voortgezet

Voor een beter begrip Application Insights zoeken werking in Visual Studio voor een project ASP.NET Core 2 zelfs wanneer een expliciete installatie van de Application Insights NuGet-pakketten nog niet hebt plaatsgevonden. Kan het handig zijn om te onderzoeken van de uitvoer voor foutopsporing.

Als u de uitvoer voor het woord zoekt _inzicht_ wordt deze lijkt op de volgende resultaten markeren:

```DebugOuput
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.aspnetcore.applicationinsights.hostingstartup\2.0.3\lib\netcoreapp2.0\Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll'.
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.applicationinsights.aspnetcore\2.1.1\lib\netstandard1.6\Microsoft.ApplicationInsights.AspNetCore.dll'.

Application Insights Telemetry (unconfigured): {"name":"Microsoft.ApplicationInsights.Dev.Message","time":"2018-06-03T17:32:38.2796801Z","tags":{"ai.location.ip":"127.0.0.1","ai.operation.name":"DEBUG /","ai.internal.sdkVersion":"aspnet5c:2.1.1","ai.application.ver":"1.0.0.0","ai.cloud.roleInstance":"CONTOSO-SERVER","ai.operation.id":"de85878e-4618b05bad11b5a6","ai.internal.nodeName":"CONTOSO-SERVER","ai.operation.parentId":"|de85878e-4618b05bad11b5a6."},"data":{"baseType":"MessageData","baseData":{"ver":2,"message":"Request starting HTTP/1.1 DEBUG http://localhost:53022/  0","severityLevel":"Information","properties":{"AspNetCoreEnvironment":"Development","Protocol":"HTTP/1.1","CategoryName":"Microsoft.AspNetCore.Hosting.Internal.WebHost","Host":"localhost:53022","Path":"/","Scheme":"http","ContentLength":"0","DeveloperMode":"true","Method":"DEBUG"}}}}
```

CoreCLR is twee assembly's laden: 

- _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_
- _Microsoft.ApplicationInsights.AspNetCore.dll_.

En de _configuratie_ in elk exemplaar van Application Insights telemetrie geeft aan dat deze toepassing is niet gekoppeld aan een ikey zodat de gegevens die is gegenereerd tijdens uw app wordt uitgevoerd naar Azure niet worden verzonden en alleen is beschikbaar voor lokaal zoeken en -analyse.

Onderdeel van de manier waarop dit mogelijk is is dat het NuGet-pakket _Microsoft.AspNetCore.All_ als een afhankelijkheid [ _Microsoft.ASPNetCoreApplicationInsights.HostingStartup_](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.applicationinsights.hostingstartup.applicationinsightshostingstartup?view=aspnetcore-2.1)

![Schermopname van het NuGet-afhankelijkheid grafiek voor Microsoft.AspNETCore.all](./media/app-insights-asp-net-core/013-dependency.png)

Buiten Visual Studio als u een project ASP.NET Core in VSCode of een andere editor zijn bewerken wouldn't deze assembly's automatisch geladen tijdens foutopsporing als u nog niet expliciet Application Insights hebt toegevoegd aan uw project.

Echter, deze licht up lokale Application Insights-functies van externe assembly's in Visual Studio wordt gerealiseerd via het gebruik van de [IHostingStartup Interface](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup?view=aspnetcore-2.1) zodat dynamisch Application Insights tijdens foutopsporing wordt toegevoegd.

Voor meer informatie over het verbeteren van een app van een [externe assembly in ASP.NET Core met IHostingStartup](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/platform-specific-configuration?view=aspnetcore-2.1). 

### <a name="how-to-disable-application-insights-in-visual-studio-net-core-projects"></a>Het uitschakelen van Application Insights in Visual Studio .NET Core-projecten

Tijdens de automatische licht van Application Insights-zoekfunctionaliteit nuttig voor sommige zijn kan, kan zien foutopsporing telemetrie gegenereerd wanneer u deze zijn niet verwacht verwarrend zijn.

Als het genereren van de telemetrie alleen uitschakelen is voldoende kunt u deze codeblok toevoegen aan de methode configureren van uw _Startup.cs_ bestand:

```csharp
  var configuration = app.ApplicationServices.GetService<Microsoft.ApplicationInsights.Extensibility.TelemetryConfiguration>();
            configuration.DisableTelemetry = true;
            if (env.IsDevelopment())
```

De CoreCLR wordt nog steeds geladen _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_ en _Microsoft.ApplicationInsights.AspNetCore.dll_, maar ze niet alles.

Als u volledig uitschakelen Application Insights in uw project voor Visual Studio .NET Core wilt, de aanbevolen methode is het selecteren van **extra** > **opties**  >   **Projecten en oplossingen** > **webprojecten** > en het selectievakje voor lokale Application Insights voor ASP.NET Core webprojecten uitschakelen. Deze functionaliteit is toegevoegd in Visual Studio 15,6.

![Schermopname van Visual Studio opties venster webprojecten scherm](./media/app-insights-asp-net-core/014-disable.png)

Als u een eerdere versie van Visual Studio uitvoert en u wilt dat voor het verwijderen van alle assembly's geladen via IHostingStartup kunt u toevoegen:

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

Het probleem met het gebruik van deze methoden is dat het niet alleen uitgeschakeld Application Insights alles in Visual Studio die is gebruik te maken van het lichte IHostingStartup up functionaliteit wordt uitgeschakeld.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="next-steps"></a>Volgende stappen
* [Verken gebruikers loopt](app-insights-usage-flows.md) om te begrijpen hoe gebruikers navigeren door uw app.
* [Momentopname verzamelen configureren](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger#configure-snapshot-collection-for-aspnet-core-20-applications) om te zien van de status van de broncode en variabelen op het moment dat een uitzondering gegenereerd.
* [Gebruik de API](app-insights-api-custom-events-metrics.md) voor het verzenden van uw eigen gebeurtenissen en metrische gegevens voor een meer gedetailleerde weergave van de prestaties en het gebruik van uw app.
* [Beschikbaarheidstests](app-insights-monitor-web-app-availability.md) controleren van uw app continu uit de hele wereld.
