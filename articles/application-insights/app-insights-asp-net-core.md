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
ms.date: 02/21/2018
ms.author: mbullwin
ms.openlocfilehash: 3f57eb3ede3b57e28c13e053b86ab022fdd21b7e
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="application-insights-for-aspnet-core"></a>Application Insights voor ASP.NET Core

Azure Application Insights biedt uitgebreide bewaking van uw webtoepassing op het codeniveau. U kunt eenvoudig uw webtoepassing voor beschikbaarheid, prestaties en gebruik bewaken. U kunt ook snel fouten in de toepassing identificeren en er een diagnose voor uitvoeren, zonder dat u hoeft te wachten totdat een gebruiker ze heeft gerapporteerd.

Dit artikel begeleidt u bij het maken van een voorbeeld van een ASP.NET Core [Razor pagina's](https://docs.microsoft.com/aspnet/core/mvc/razor-pages/?tabs=visual-studio) toepassing in Visual Studio en hoe u bewaking starten met Azure Application Insights.

## <a name="prerequisites"></a>Vereisten

- NET Core 2.0.0 SDK of hoger.
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) versie 15.3 of hoger, waarop de werkbelasting van de ontwikkeling van ASP.NET en Internet.

## <a name="create-an-aspnet-core-project-in-visual-studio"></a>Een ASP.NET Core-project in Visual Studio maken

1. Met de rechtermuisknop op en start **Visual Studio 2017** als administrator.
2. Selecteer **bestand** > **nieuwe** > **Project** (Ctrl-Shift-N).

   ![Schermopname van nieuw Project Menu van Visual Studio-bestand](./media/app-insights-asp-net-core/0001-file-new-project.png)

3. Vouw **Visual C#** > Selecteer **.NET Core** > **ASP.NET Core webtoepassing**. Voer een **naam** > **oplossingsnaam** > controleren **maken nieuwe Git-opslagplaats**.

   ![Schermafbeelding van de Wizard Nieuw Project bestand Visual Studio](./media/app-insights-asp-net-core/0002-new-project-web-application.png)

4. Selecteer **.Net Core** > **ASP.NET Core 2.0** **webtoepassing** > **OK**.

    ![Schermopname van nieuw Project selectie Menu van Visual Studio-bestand](./media/app-insights-asp-net-core/0003-dot-net-core.png)

## <a name="add-application-insights-telemetry"></a>Application Insights Telemetry toevoegen

1. Selecteer **Project** > **Application Insights Telemetry toevoegen...** (U kunt ook u kunt met de rechtermuisknop op **verbonden Services** > verbonden Service toevoegen.)

    ![Schermopname van nieuw Project selectie Menu van Visual Studio-bestand](./media/app-insights-asp-net-core/0004-add-application-insights-telemetry.png)

2. Selecteer **Gratis beginnen**.

    ![Schermopname van nieuw Project selectie Menu van Visual Studio-bestand](./media/app-insights-asp-net-core/0005-start-free.png)

3. Selecteer een geschikte **abonnement** > **Resource** > en al dan niet meer dan 1 GB per maand verzamelen toestaan > **registreren**.

    ![Schermopname van nieuw Project selectie Menu van Visual Studio-bestand](./media/app-insights-asp-net-core/0006-register.png)

## <a name="changes-made-to-your-project"></a>Wijzigingen maken aan uw project

Application Insights is weinig overhead. De aangebrachte door telemetrie voor Application Insights toe te voegen aan uw project bekijken:

Selecteer **weergave** > **Explorer Team** (Ctrl +\, Ctrl + M) > **Project** > **wijzigingen**

- Totaal vier wijzigingen:

  ![Schermopname van bestanden die zijn gewijzigd door Application Insights toe te voegen](./media/app-insights-asp-net-core/0007-changes.png)

- Een nieuw bestand wordt gemaakt:

   **ConnectedService.json**

  ![Schermopname van bestanden die zijn gewijzigd door Application Insights toe te voegen](./media/app-insights-asp-net-core/0008-connectedservice-json.png)

- Drie bestanden zijn gewijzigd:

  **appsettings.json**

   ![Schermopname van bestanden die zijn gewijzigd door Application Insights toe te voegen](./media/app-insights-asp-net-core/0009-appsettings-json.png)

  **ContosoDotNetCore.csproj**

   ![Schermopname van bestanden die zijn gewijzigd door Application Insights toe te voegen](./media/app-insights-asp-net-core/0010-contoso-netcore-csproj.png)

   **Program.cs**

   ![Schermopname van bestanden die zijn gewijzigd door Application Insights toe te voegen](./media/app-insights-asp-net-core/0011-program-cs.png)

## <a name="synthetic-transactions-with-powershell"></a>Synthetische transacties met PowerShell

Uw app opnieuw te starten en vervolgens te klikken om op koppelingen handmatig kunnen worden gebruikt voor het genereren van de test-verkeer. Het is echter vaak nuttig zijn voor het maken van een eenvoudige synthetische transactie in PowerShell.

1. Uw app uitvoeren door te klikken op IIS Express ![Schermopname van Visual Studio IIS Express-pictogram](./media/app-insights-asp-net-core/0012-iis-express.png)

2. Kopieer de url van de adresbalk van de browser. Het is in de notatie http://localhost:{random poortnummer}

   ![Schermafbeelding van de adresbalk van de browser-url](./media/app-insights-asp-net-core/0013-copy-url.png)

3. Voer de volgende PowerShell-lus voor het maken van 100 synthetische transacties op basis van uw app testen. Wijzig het poortnummer na **localhost:** overeenkomen met de url die u in de vorige stap hebt gekopieerd.

   ```PS
   for ($i = 0 ; $i -lt 100; $i++)
   {
    Invoke-WebRequest -uri http://localhost:50984/
   }
   ```

## <a name="open-application-insights-portal"></a>Open Application Insights-Portal

Start na het uitvoeren van de PowerShell uit de vorige sectie, Application Insights om de transacties bekijken en te bevestigen dat gegevens worden verzameld. 

Selecteer in het menu Visual Studio **Project** > **Application Insights** > **Open Application Insights-Portal**

   ![Schermopname van Application Insights-overzicht](./media/app-insights-asp-net-core/0014-portal-01.png)

> [!NOTE]
> In het bovenstaande voorbeeld **Live Stream**, **laadtijd voor paginaweergave**, en **mislukte aanvragen voor** worden momenteel niet worden verzameld. De volgende sectie begeleidt bij het toevoegen van elk. Als u al verzamelt **Live Stream**, en **laadtijd voor paginaweergave**, volgt u de stappen voor het alleen **mislukte aanvragen voor**.

## <a name="collect-failed-requests-live-stream--page-view-load-time"></a>Verzamelen van mislukte aanvragen, Live Stream & laadtijd voor paginaweergave

### <a name="failed-requests"></a>Mislukte aanvragen

Technisch **mislukte aanvragen voor** worden verzameld, maar geen nog hebben plaatsgevonden. Als u wilt versnellen langs een aangepaste uitzondering kunnen worden toegevoegd aan het bestaande project om te simuleren een echte-uitzondering. Als uw app wordt nog steeds uitgevoerd in Visual Studio voordat u doorgaat **Stop Debugging** (Shift + F5)

1. In **Solution Explorer** > Vouw **pagina's** > **About.cshtml** > open **About.cshtml.cs**.

   ![Schermafbeelding van de Visual Studio Solution Explorer](./media/app-insights-asp-net-core/0015-solution-explorer-about.png)

2. Toevoegen van een uitzondering onder ``Message=`` > opslaan van de wijziging in het bestand.

   ```C#
   throw new Exception("Test Exception");
   ```

   ![Schermopname van uitzonderingscode](./media/app-insights-asp-net-core/000016-exception.png)

### <a name="live-stream"></a>Live Stream

Voor toegang tot de Live Stream-functionaliteit van Application Insights met ASP.NET Core update om de **Microsoft.ApplicationInsights.AspNetCore 2.2.0** NuGet-pakketten.

Selecteer in Visual Studio **Project** > **NuGet-pakketten beheren** > **Microsoft.ApplicationInsights.AspNetCore** > versie **2.2.0** > **update**.

  ![Schermopname van het NuGet-Pakketbeheer](./media/app-insights-asp-net-core/0017-update-nuget.png)

Meerdere bevestiging vragen zal optreden, lezen en accepteren als u akkoord met de wijzigingen gaat.

### <a name="page-view-load-time"></a>Laadtijd voor paginaweergave

1. Ga in Visual Studio naar **Solution Explorer** > **pagina's** > twee bestanden moet worden gewijzigd: **_Layout.cshtml**, en **_ ViewImports.cshtml**

2. In **_ViewImports.cshtml**, toevoegen:

   ```C#
   @using Microsoft.ApplicationInsights.AspNetCore
   @inject JavaScriptSnippet snippet
   ```
     ![Schermafbeelding van de codewijziging _ViewImports.cshtml](./media/app-insights-asp-net-core/00018-view-imports.png)

3. In **_Layout.cshtml** toevoegen van de regel onder voordat de ``</head>`` label, maar vóór alle andere scripts.

    ```C#
    @Html.Raw(snippet.FullScript)
    ```
    ![Schermafbeelding van de codewijziging layout.cshtml](./media/app-insights-asp-net-core/0018-layout-cshtml.png)

### <a name="test-failed-requests-page-view-load-time-live-stream"></a>Test is mislukt-aanvragen, laadtijd voor paginaweergave, Live Stream

U kunt testen en controleren of alles werkt nu dat u de vorige stappen hebt voltooid.

1. Uw app uitvoeren door te klikken op IIS Express ![Schermopname van Visual Studio IIS Express-pictogram](./media/app-insights-asp-net-core/0012-iis-express.png)

2. Navigeer naar de **over** pagina voor het activeren van de test-uitzondering. (Als u in de foutopsporingsmodus uitvoert, moet u klikken op **doorgaan** in Visual Studio voordat de uitzondering wordt verwerkt door de Application Insights.)

3. Voer het gesimuleerde PowerShell transactie-script uit eerdere (mogelijk moet u het poortnummer in het script aanpassen.)

4. Als het overzicht van de Insights toepassingen niet nog geopend vanuit Visual Studio menu Selecteer **Project** > **Application Insights** > **toepassing openen Insights-Portal**. 

   > [!TIP]
   > Als u uw nieuwe verkeer nog worden niet ziet, Controleer de **tijdsbereik** en klik op **vernieuwen**.

   ![Schermopname van overzicht](./media/app-insights-asp-net-core/0019-overview-updated.png)

5. Selecteer Live Stream

   ![Schermafbeelding van de Stream Live metrische gegevens](./media/app-insights-asp-net-core/0020-live-metrics-stream.png)

   (Als uw PowerShell-script nog steeds uitgevoerd is, u live metrische gegevens en ziet als is gestopt met het script opnieuw uitvoeren met Live Stream openen.)

## <a name="app-insights-sdk-comparison"></a>Vergelijking van App Insights-SDK

De Application Insights-productgroep werken hard aan bereiken zo dicht mogelijk bij functie pariteit mogelijk tussen de [.NET Framework SDK volledige](https://github.com/Microsoft/ApplicationInsights-dotnet) en de .net Core SDK. De 2.2.0 release van de [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore) voor de afstand van de functie grotendeels door Application Insights is gesloten.

Voor meer informatie over de verschillen en de wisselwerking tussen [.NET en .NET Core](https://docs.microsoft.com/en-us/dotnet/standard/choosing-core-framework-server).

   | Vergelijking van de SDK | ASP.NET        | ASP.NET Core 2.1.0    | ASP.NET Core 2.2.0 |
  |:-- | :-------------: |:------------------------:|:----------------------:|
   | **Live metrische gegevens**      | **+** |**-** | **+** |
   | **Telemetrie-serverkanaal** | **+** |**-** | **+**|
   |**Adaptieve steekproeven**| **+** | **-** | **+**|
   | **Aanroepen van de SQL-afhankelijkheden**     | **+** |**-** | **+**|
   | **Prestatiemeteritems*** | **+** | **-**| **-**|

_Prestatiemeteritems_ in deze context verwijst naar [serverzijde prestatiemeteritems](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-performance-counters) , zoals processors, geheugen en schijfgebruik.

## <a name="open-source-sdk"></a>Open-source SDK
[Lees- en bijdragen aan de code](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="next-steps"></a>Volgende stappen
* [Verken gebruikers loopt](app-insights-usage-flows.md) om te begrijpen hoe gebruikers navigeren door uw app.
* [Momentopname verzamelen configureren](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger#configure-snapshot-collection-for-aspnet-core-20-applications) om te zien van de status van de broncode en variabelen op het moment dat een uitzondering gegenereerd.
* [Gebruik de API](app-insights-api-custom-events-metrics.md) voor het verzenden van uw eigen gebeurtenissen en metrische gegevens voor een meer gedetailleerde weergave van de prestaties en het gebruik van uw app.
* [Beschikbaarheidstests](app-insights-monitor-web-app-availability.md) controleren van uw app continu uit de hele wereld.
