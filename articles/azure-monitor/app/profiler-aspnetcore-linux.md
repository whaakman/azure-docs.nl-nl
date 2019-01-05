---
title: Profileren van ASP.NET Core Azure Linux-web-apps met Application Insights Profiler | Microsoft Docs
description: Een conceptueel overzicht en een stapsgewijze zelfstudie over het gebruik van Application Insights Profiler.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: mbullwin
ms.openlocfilehash: db2972019e81ab413857d49ffa65eade3ff061ec
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54051183"
---
# <a name="profile-aspnet-core-azure-linux-web-apps-with-application-insights-profiler"></a>Profileren van ASP.NET Core Azure Linux-web-apps met Application Insights Profiler

Deze functie is momenteel beschikbaar als preview-product.

Ontdek hoeveel tijd is besteed in elke methode van uw live webtoepassing wanneer u [Application Insights](../../application-insights/app-insights-overview.md). Application Insights Profiler is nu beschikbaar voor ASP.NET Core web-apps die worden gehost in Linux op Azure App Service. Deze handleiding bevat stapsgewijze instructies over hoe de Profiler-traces voor ASP.NET Core-Linux WebApps kunnen worden verzameld.

Nadat u alle stappen hebt voltooid, kan uw app Profiler-traceringen, zoals de traceringen die worden weergegeven in de afbeelding te verzamelen. In dit voorbeeld wordt de Profiler-tracering geeft aan dat een bepaalde webaanvraag trage vanwege tijd wachten. De *snelpad* in de code die de app wordt traag is gemarkeerd met een pictogram vuur. De **over** methode in de **HomeController** sectie de web-app is vertraagd omdat het aanroepen van de methode de **Thread.Sleep** functie.

![Profilertraces](./media/profiler-aspnetcore-linux/profiler-traces.png)

## <a name="prerequisites"></a>Vereisten
De volgende instructies zijn van toepassing op alle omgevingen voor Windows, Linux en Mac-ontwikkeling:

* Installeer de [.NET Core SDK 2.1.2 of hoger](https://dotnet.microsoft.com/download/archives).
* Git installeren door de instructies op [aan de slag - installeren van Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

## <a name="set-up-the-project-locally"></a>Het project lokaal instellen

1. Open een opdrachtpromptvenster op uw computer. De volgende instructies werken voor alle omgevingen voor Windows, Linux en Mac-ontwikkeling.

2. Een ASP.NET Core MVC-webtoepassing maken:

    ```
    dotnet new mvc -n LinuxProfilerTest
    ```

3. Wijzig de werkmap in de hoofdmap voor het project.

4. NuGet-pakket voor het verzamelen van de Profiler-traceringen toevoegen:

    ```
    dotnet add package Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

5. Toevoegen van een regel met code in de **HomeController.cs** sectie willekeurige vertraging een paar seconden:

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

6. Opslaan en uw wijzigingen aan in de lokale opslagplaats:

    ```
        git init
        git add .
        git commit -m "first commit"
    ```

## <a name="create-the-linux-web-app-to-host-your-project"></a>De Linux-web-app voor het hosten van uw project maken

1. De web-app-omgeving maken met behulp van App Service op Linux:

    ![De Linux-web-app maken](./media/profiler-aspnetcore-linux/create-linux-appservice.png)

2. Maak de referenties voor implementatie:

    > [!NOTE]
    > Noteer het wachtwoord voor later gebruik bij het implementeren van uw web-app.

    ![De referenties voor implementatie maken](./media/profiler-aspnetcore-linux/create-deployment-credentials.png)

3. Kies de implementatie-opties. Instellen van een lokale Git-opslagplaats in de web-app door de instructies in de Azure portal. Een Git-opslagplaats wordt automatisch gemaakt.

    ![De Git-opslagplaats instellen](./media/profiler-aspnetcore-linux/setup-git-repo.png)

Zie voor meer implementatieopties, [in dit artikel](https://docs.microsoft.com/azure/app-service/containers/choose-deployment-type).

## <a name="deploy-your-project"></a>Uw project implementeren

1. Blader naar de hoofdmap voor uw project in het opdrachtpromptvenster. Toevoegen van een externe Git-opslagplaats om te verwijzen naar de opslagplaats op App Service:

    ```
    git remote add azure https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
    ```

    * Gebruik de **gebruikersnaam** die u voor het maken van de referenties voor implementatie gebruikt.
    * Gebruik de **appnaam** dat u gebruikt de web-app maken met behulp van App Service op Linux.

2. Het project implementeren door simpelweg de wijzigingen naar Azure te pushen:

    ```
    git push azure master
    ```

Hier ziet u uitvoer die vergelijkbaar is met het volgende voorbeeld:

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

1. [Maak een Application Insights-resource](./../../application-insights/app-insights-create-new-resource.md).

2. Kopieer de **iKey** waarde van de Application Insights-resource en stel de volgende instellingen in uw web-apps:

    ```
    APPINSIGHTS_INSTRUMENTATIONKEY: [YOUR_APPINSIGHTS_KEY]
    ASPNETCORE_HOSTINGSTARTUPASSEMBLIES: Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

    ![De app-instellingen configureren](./media/profiler-aspnetcore-linux/set-appsettings.png)

    Wanneer de app-instellingen worden gewijzigd, wordt de site automatisch opnieuw opgestart. Nadat de nieuwe instellingen worden toegepast, voert u de Profiler onmiddellijk voor twee minuten. Vervolgens wordt de Profiler gedurende twee minuten om het uur uitgevoerd.

3. Bepaald verkeer naar uw website genereren. U kunt netwerkverkeer genereren door het vernieuwen van de site **over** pagina een paar keer.

4. De gebeurtenissen die moeten worden twee tot vijf minuten wachten tot cumulatieve naar Application Insights.

5. Blader naar de Application Insights **prestaties** deelvenster in de Azure-portal. U kunt de Profiler-traceringen in de rechterbenedenhoek van het deelvenster weergeven.

    ![Profiler-traceringen weergeven](./media/profiler-aspnetcore-linux/view-traces.png)

## <a name="known-issues"></a>Bekende problemen

### <a name="the-enable-action-in-the-profiler-configuration-pane-doesnt-work"></a>De actie inschakelen in het deelvenster Profiler configuratie werkt niet

> [!NOTE]
> Als u uw app met behulp van App Service op Linux hosten, moet u niet opnieuw in de Profiler in te schakelen de **prestaties** deelvenster in de Application Insights-portal. U kunt het NuGet-pakket opnemen in uw project en de Application Insights instellen **iKey** waarde in de instellingen van uw web-app zodat de Profiler.

Als u de werkstroom inschakelen voor volgt [Application Insights Profiler voor Windows](./profiler.md) en selecteer **inschakelen** in de **Profiler configureren** deelvenster, ontvangt u een foutbericht. De actie inschakelen probeert de Windows-versie van de Profiler-agent installeren op de Linux-omgeving.

We werken aan een oplossing voor dit probleem.

![Probeer niet opnieuw inschakelen van de Profiler in het deelvenster prestaties](./media/profiler-aspnetcore-linux/issue-enable-profiler.png)


## <a name="next-steps"></a>Volgende stappen
Als u aangepaste containers die worden gehost in Azure App Service gebruiken, volgt u de instructies in [ Service Profiler inschakelen voor een beperkte ASP.NET Core-toepassing](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/tree/master/examples/EnableServiceProfilerForContainerApp) Application Insights Profiler inschakelen.

Rapport eventuele problemen of suggesties voor de Application Insights-GitHub-opslagplaats: [Application Insights-Profiler-AspNetCore: Problemen met](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/issues).
