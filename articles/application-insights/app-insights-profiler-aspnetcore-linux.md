---
title: Profiel van ASP.NET Core Azure Linux web-apps met Application Insights Profiler | Microsoft Docs
description: Een conceptueel overzicht en stapsgewijze zelfstudie over het gebruik van Application Insights Profiler.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/23/2018
ms.author: mbullwin
ms.openlocfilehash: 5596c4efeba14e9d2bfdadd7ce92bb6b2c9fcbf0
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="profile-aspnet-core-azure-linux-web-apps-with-application-insights-profiler"></a>Profiel ASP.NET Core Azure Linux web-apps met Application Insights Profiler

Deze functie is momenteel beschikbaar als preview-product.

Na hoeveel tijd is besteed aan elke methode van uw live-webtoepassing wanneer u weten [Application Insights](app-insights-overview.md). Application Insights Profiler is nu beschikbaar voor ASP.NET Core web-apps die worden gehost in Linux op Azure App Service. Deze handleiding bevat stapsgewijze instructies over hoe de Profiler traceringen voor ASP.NET Core Linux web-apps kunnen worden verzameld.

Nadat u deze stapsgewijze Kennismaking hebt voltooid, kan uw app Profiler traceringen zoals die worden weergegeven in de afbeelding anders de traceringen verzamelen. In dit voorbeeld wordt de tracering Profiler geeft aan dat een bepaalde webaanvraag traag vanwege de tijd besteed wachten. De *hot pad* in de code die de app wordt vertraging is gemarkeerd als door een pictogram brandpreventie. De **over** methode in de **HomeController** sectie wordt de web-app vertragen, omdat het aanroepen van de methode de **Thread.Sleep** functie.

![Profiler traceringen](./media/app-insights-profiler-aspnetcore-linux/profiler-traces.png)

## <a name="prerequisites"></a>Vereisten
De volgende instructies gelden voor alle Windows, Linux en Mac-ontwikkelomgevingen:

* Installeer de [.NET Core SDK 2.1.2 of hoger](https://www.microsoft.com/net/download/windows/build).
* Installeer Git volgens de instructies op [aan de slag - installeren van Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

## <a name="set-up-the-project-locally"></a>Het project lokaal instellen

1. Open een opdrachtpromptvenster op uw computer. De volgende instructies werken voor alle Windows, Linux en Mac-ontwikkelomgevingen.

2. Een ASP.NET Core MVC-webtoepassing maken:

    ```
    dotnet new mvc -n LinuxProfilerTest
    ```

3. Wijzig de werkmap in de hoofdmap voor het project.

4. Het NuGet-pakket voor het verzamelen van de Profiler traceringen toevoegen:

    ```
    dotnet add package Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

5. Voeg een regel code in de **HomeController.cs** sectie willekeurige vertraging van enkele seconden:

    ```csharp
        using System.Threading;
        ...

        public IActionResult About()
            {
                Random r = new Random();
                int delay = r.Next(5000, 10000);
                Thread.Sleep(delay);
                return View();
            }
    ```

6. Opslaan en uw wijzigingen aan de lokale opslagplaats:

    ```
        git init
        git add .
        git commit -m "first commit"
    ```

## <a name="create-the-linux-web-app-to-host-your-project"></a>De Linux-web-app voor het hosten van uw project maken

1. De web-app-omgeving maken met behulp van App Service op Linux:

    ![De Linux-web-app maken](./media/app-insights-profiler-aspnetcore-linux/create-linux-appservice.png)

2. Maak de referenties voor implementatie:

    > [!NOTE]
    > Leg uw wachtwoord voor later gebruik bij het implementeren van uw web-app.

    ![De referenties voor implementatie maken](./media/app-insights-profiler-aspnetcore-linux/create-deployment-credentials.png)

3. Kies de implementatie-opties. Een lokale Git-opslagplaats in de web-app instellen door de instructies in de Azure portal. Een Git-opslagplaats, wordt automatisch gemaakt.

    ![De Git-opslagplaats instellen](./media/app-insights-profiler-aspnetcore-linux/setup-git-repo.png)

Zie voor meer implementatieopties [in dit artikel](https://docs.microsoft.com/azure/app-service/containers/choose-deployment-type).

## <a name="deploy-your-project"></a>Uw project implementeren

1. Blader naar de hoofdmap voor uw project in het opdrachtpromptvenster. Toevoegen van een externe Git-opslagplaats om te verwijzen naar de opslagplaats op App Service:

    ```
    git remote add azure https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
    ```

    * Gebruik de **gebruikersnaam** die u hebt gebruikt voor het maken van de referenties voor implementatie.
    * Gebruik de **appnaam** dat u gebruikt voor het maken van de web-app met behulp van App-Service op Linux.

2. Het project implementeren door de wijzigingen naar Azure te pushen:

    ```
    git push azure master
    ```

U ziet de uitvoer ziet er als volgt uitzien:

    ```
    Counting objects: 9, done.
    Delta compression using up to 8 threads.
    Compressing objects: 100% (8/8), done.
    Writing objects: 100% (9/9), 1.78 KiB | 911.00 KiB/s, done.
    Total 9 (delta 3), reused 0 (delta 0)
    remote: Updating branch 'master'.
    remote: Updating submodules.
    remote: Preparing deployment for commit id 'd7369a99d7'.
    remote: Generating deployment script.
    remote: Running deployment command...
    remote: Handling ASP.NET Core Web Application deployment.
    remote: ......
    remote:   Restoring packages for /home/site/repository/EventPipeExampleLinux.csproj...
    remote: .
    remote:   Installing Newtonsoft.Json 10.0.3.
    remote:   Installing Microsoft.ApplicationInsights.Profiler.Core 1.1.0-LKG
    …

    ```

## <a name="add-application-insights-to-monitor-your-web-apps"></a>Application Insights voor het bewaken van uw web-apps toevoegen

1. [Een Application Insights-resource maken](./app-insights-create-new-resource.md).

2. Kopieer de **iKey** waarde van de Application Insights-resource en stelt u de volgende instellingen in uw web-apps:

    ```
    APPINSIGHTS_INSTRUMENTATIONKEY: [YOUR_APPINSIGHTS_KEY]
    ASPNETCORE_HOSTINGSTARTUPASSEMBLIES: Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

    ![De app-instellingen configureren](./media/app-insights-profiler-aspnetcore-linux/set-appsettings.png)

    Wanneer de app-instellingen worden gewijzigd, wordt de site automatisch opnieuw opgestart. Nadat de nieuwe instellingen worden toegepast, voert u de Profiler onmiddellijk twee minuten. De Profiler vervolgens twee minuten om het uur wordt uitgevoerd.

3. Sommige verkeer naar uw website genereren. U kunt verkeer genereren door de site vernieuwen **over** pagina een paar keer.

4. Wacht twee tot vijf minuten om de gebeurtenissen te cumulatieve naar Application Insights.

5. Blader naar de Application Insights **prestaties** deelvenster in de Azure-portal. U kunt de Profiler traceringen in de rechterbenedenhoek van het deelvenster weergeven.

    ![Profiler traceringen weergeven](./media/app-insights-profiler-aspnetcore-linux/view-traces.png)

## <a name="known-issues"></a>Bekende problemen

### <a name="the-enable-action-in-the-profiler-configuration-pane-doesnt-work"></a>De actie inschakelen in het deelvenster Profiler configuratie werkt niet

> [!NOTE]
> Als u uw app met behulp van App Service op Linux host, u hoeft niet opnieuw inschakelen van de Profiler in de **prestaties** deelvenster in de Application Insights-portal. U kunt het NuGet-pakket opnemen in uw project en instellen van de Application Insights **iKey** waarde in de instellingen van uw web-app zodat de Profiler.

Als u de werkstroom inschakelen voor volgt [Application Insights Profiler voor Windows](./app-insights-profiler.md) en selecteer **inschakelen** in de **Profiler configureren** deelvenster er een foutbericht. De actie inschakelen probeert te installeren van de Windows-versie van de Profiler-agent op de Linux-omgeving.

We proberen op een oplossing voor dit probleem.

![Probeer niet de Profiler in het deelvenster prestaties opnieuw inschakelen](./media/app-insights-profiler-aspnetcore-linux/issue-enable-profiler.png)


## <a name="next-steps"></a>Volgende stappen
Als u aangepaste containers die worden gehost in Azure App Service gebruikt, volg de instructies in [ Serviceprofiler inschakelen voor een beperkte ASP.NET Core toepassing](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/tree/master/examples/EnableServiceProfilerForContainerApp) Application Insights Profiler inschakelen.

Eventuele problemen of suggesties om de Application Insights GitHub-opslagplaats te rapporteren: [ApplicationInsights-Profiler-AspNetCore: problemen](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/issues).
