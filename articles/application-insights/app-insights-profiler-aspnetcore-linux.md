---
title: Profiel van ASP.NET core Azure Linux web-apps met Application Insights Profiler | Microsoft Docs
description: Overzicht van het concept en stapsgewijze zelfstudie over het in te schakelen
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/23/2018
ms.author: mbullwin
ms.openlocfilehash: 2d7405baee84b53311f01e748ca7975147c107d8
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="profile-aspnet-core-azure-linux-web-apps-with-application-insights-profiler"></a>Profiel ASP.NET Core Azure Linux-Web-Apps met Application Insights Profiler

Deze functie is momenteel in preview

Na hoeveel tijd is besteed aan elke methode van uw live-webtoepassing wanneer u weten [Application Insights](app-insights-overview.md). Profiler is nu beschikbaar voor ASP.NET core web-apps die in Linux op App-Services worden gehost. Deze handleiding bevat stapsgewijze instructies over hoe de profiler traceringen voor ASP.NET core Linux web-apps kunnen worden verzameld.

Na het voltooien van deze procedure wordt uw app profiler traceringen vergelijkbaar met de onderstaande schermafbeelding verzamelen. In dit voorbeeld geeft de profiler tracering aan dat een bepaalde webaanvraag traag is, omdat de meeste tijd is besteed aan wachten. De hot pad in de code die de app wordt vertraagd wordt voorafgegaan door het pictogram brandpreventie. In dit voorbeeld ziet `About` methode in `HomeController` de web-app wordt vertraagd omdat dit is het aanroepen van `Thread.Sleep`.

![Profiler traceringen](./media/app-insights-profiler-aspnetcore-linux/profiler-traces.png)

## <a name="pre-requisites"></a>Vereisten
Instructies hieronder toegepast op alle Windows, Linux en Mac ontwikkelomgevingen:

* Installeer [.NET core SDK 2.1.2 of hoger](https://www.microsoft.com/net/download/windows/build)
* Installeer Git instructies te volgen op [aan de slag - Git installeren](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

## <a name="setup-project-locally"></a>Setup-project lokaal

1. Open een opdrachtprompt op uw computer. De onderstaande instructies werkt voor alle Windows, Linux en Mac-ontwikkelomgevingen.

2. De kern van een ASP.NET MVC-webtoepassing maken
    ```
    dotnet new mvc -n LinuxProfilerTest
    ```
3. De map wijzigen in de hoofdmap van het project via de opdrachtprompt

4. Nuget-pakket voor het verzamelen van traceringen profiler toevoegen.
    ```
    dotnet add package Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```
5. Een regel code willekeurige vertraging van enkele seconden in HomeController.cs toevoegen

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
6. Opslaan en uw wijzigingen aan de lokale opslagplaats

    ```
        git init
        git add .
        git commit -m "first commit"
    ```

## <a name="create-azure-app-service-for-hosting-your-project"></a>Azure App Service voor het hosten van uw project maken
1. Een omgeving van App Services Linux maken

    ![Linux-App Services maken](./media/app-insights-profiler-aspnetcore-linux/create-linux-appservice.png)

2. Implementatie-referentie maken. Noteer het wachtwoord als u deze later nodig bij het implementeren van uw app.

    ![Maak implementatiereferenties](./media/app-insights-profiler-aspnetcore-linux/create-deployment-credentials.png)

3. Optie voor implementatie. Instellen van een lokale Git-opslagplaats in de web-app in Azure portal instructies te volgen. Een Git-opslagplaats wordt automatisch gemaakt.

    ![Git-opslagplaats instellen](./media/app-insights-profiler-aspnetcore-linux/setup-git-repo.png)

Er zijn meer implementatieopties beschikbaar [hier](https://docs.microsoft.com/azure/app-service/containers/choose-deployment-type)

## <a name="deploy-your-project"></a>Uw project implementeren

1. Navigeer naar de hoofdmap van uw project in het opdrachtvenster. Externe Git-opslagplaats om te verwijzen naar de versie van App Services toevoegen:

    ```
    git remote add azure https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
    ```
    * Gebruik het 'username' van de stap 'implementatie referentie maken'.
    * Gebruik de 'appnaam' van de stap van de 'app service maken'.

2. Het project implementeren door de wijzigingen worden gepusht naar Azure

    ```
    git push azure master
    ```
Hier ziet u uitvoer die vergelijkbaar is met het volgende:

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
1. [Een Application Insights-resource maken](./app-insights-create-new-resource.md)
2. Kopieer de sleutel van de Application Insights-resource en stelt u de volgende instellingen in uw App-services

    ```
    APPINSIGHTS_INSTRUMENTATIONKEY: [YOUR_APPINSIGHTS_KEY]
    ASPNETCORE_HOSTINGSTARTUPASSEMBLIES: Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

    ![App-instellingen instellen](./media/app-insights-profiler-aspnetcore-linux/set-appsettings.png)

    App-instellingen wijzigen, wordt de site automatisch opnieuw opgestart. Nadat de nieuwe instellingen worden toegepast, worden de profiler wordt gestart voor twee minuten onmiddellijk uit te voeren. vervolgens wordt deze uitgevoerd gedurende 2 minuten om het uur.

3. Sommige verkeer naar uw website genereren. U kunt de site vernieuwen ```About``` pagina voor een paar keer.

4. 2-5 minuten wachten zodat de gebeurtenissen kunnen worden geaggregeerd naar Application Insights.

5. Ga naar Application Insights prestaties deelvenster in Azure-portal. U ziet de profiler traceringen beschikbaar in de rechterbenedenhoek.

    ![Weergave traceringen](./media/app-insights-profiler-aspnetcore-linux/view-traces.png)

## <a name="next-steps"></a>Volgende stappen
Als u aangepaste containers die worden gehost door App Services gebruikt, volg de instructies uit [ Serviceprofiler inschakelen voor beperkte ASP.NET Core toepassing](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/tree/master/examples/EnableServiceProfilerForContainerApp) App Insights Profiler inschakelen

Als u problemen of suggesties hebt, meld u aan onze github-opslagplaats: [ApplicationInsights-Profiler-AspNetCore: problemen](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/issues)
