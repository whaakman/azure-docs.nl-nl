---
title: Apps in een lokale Docker-container foutopsporing | Microsoft Docs
description: Informatie over het aanpassen van een app die wordt uitgevoerd in een lokale Docker-container, het vernieuwen van de container met Edit- and vernieuwen en foutopsporing onderbrekingspunten instellen
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 480e3062-aae7-48ef-9701-e4f9ea041382
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/22/2016
ms.author: mlearned
ms.openlocfilehash: 7df8bb2dd61466fd9af6492277d54cf0ffdfeb0b
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="debugging-apps-in-a-local-docker-container"></a>Fouten in apps in een lokale Docker-container opsporen
## <a name="overview"></a>Overzicht
Visual Studio Tools for Docker biedt een consistente manier te ontwikkelen en uw toepassing lokaal in een Linux-Docker-container te valideren.
U moet niet opnieuw starten van de container telkens wanneer u een code wijzigen.
In dit artikel ziet u hoe u met de functie 'Bewerken en vernieuwen' een ASP.NET Core Web-app te starten in een lokale Docker-container, breng eventueel benodigde wijzigingen en vernieuw de browser om deze wijzigingen te bekijken.
Dit artikel ziet u ook het instellen van onderbrekingspunten voor foutopsporing.

> [!NOTE]
> Ondersteuning voor Windows-Container wordt binnenkort in een toekomstige release
>
>

## <a name="prerequisites"></a>Vereisten
De volgende hulpprogramma's moeten worden geïnstalleerd.

* [Meest recente versie van Visual Studio](https://www.visualstudio.com/downloads/)
* [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)

Als u wilt uitvoeren, Docker-containers lokaal, moet u een lokale docker-client.
U kunt de [Docker-werkset](https://www.docker.com/products/docker-toolbox), die vereist dat Hyper-V moet worden uitgeschakeld of kunt u [Docker voor Windows](https://www.docker.com/get-docker), die gebruikmaakt van Hyper-V en Windows 10 is vereist.

Als Docker-werkset wordt gebruikt, moet u [de Docker-client configureren](vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. Een webtoepassing maken
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Docker-ondersteuning toevoegen
[!INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-edit-your-code-and-refresh"></a>3. Bewerken van uw code en vernieuwen
Als u wilt herhalen snel wijzigingen, kunt u uw toepassing binnen een container start en wijzigingen aanbrengen, gaan ze weer te geven u net als bij IIS Express.

1. Stel de oplossing op `Debug` en druk op  **&lt;CTRL + F5 >** naar uw docker-installatiekopie bouwen en lokaal uitvoeren.

    Nadat de container-installatiekopie is gemaakt en wordt uitgevoerd in een Docker-container, Start Visual Studio de Web-app in de browser.
    Als u van de browser Microsoft Edge gebruikmaakt of anders fouten hebben, Zie [probleemoplossing](vs-azure-tools-docker-troubleshooting-docker-errors.md) sectie.
2. Ga naar de pagina over die we willen waar onze wijzigingen aanbrengen.
3. Ga terug naar Visual Studio en open `Views\Home\About.cshtml`.
4. De volgende HTML-inhoud toevoegen aan het einde van het bestand en sla de wijzigingen.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```
5. Het uitvoervenster weergeven wanneer de build .NET is voltooid en u deze regels bekijken, gaat u terug naar uw browser en vernieuw de pagina over.

   ```
   Now listening on: http://*:80
   Application started. Press Ctrl+C to shut down
   ```
6. Uw wijzigingen zijn toegepast.

## <a name="4-debug-with-breakpoints"></a>4. Foutopsporing met onderbrekingspunten
Vaak moet wijzigingen meer controle, gebruik van de functies voor foutopsporing van Visual Studio.

1. Ga terug naar Visual Studio en openen`Controllers\HomeController.cs`
2. De inhoud van de methode About() vervangen door het volgende:

   ```
   string message = "Your application description page from within a Container";
   ViewData["Message"] = message;
   ````
3. Stel een onderbrekingspunt in aan de linkerkant van de `string message`... regel.
4. Klik op  **&lt;F5 >** foutopsporing te starten.
5. Navigeer naar de pagina over naar uw onderbrekingspunt.
6. Overschakelen naar Visual Studio om het onderbrekingspunt weer te geven en controleren van de waarde van het bericht.

   ![][2]

## <a name="summary"></a>Samenvatting
Met [Visual Studio 2015-Tools voor Docker](https://aka.ms/DockerToolsForVS), krijgt u de productiviteit van lokaal werkt met de productie realisme van het ontwikkelen van binnen een Docker-container.

## <a name="troubleshooting"></a>Problemen oplossen
[Het oplossen van Visual Studio Docker-ontwikkeling](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Meer informatie over Docker met Visual Studio, Windows en Azure
* [Docker-Tools voor Visual Studio](http://aka.ms/dockertoolsforvs) -ontwikkelen van uw .NET Core-code in een container
* [Docker-Tools voor Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - bouwen en implementeren van docker-containers
* [Docker-Tools voor Visual Studio Code](http://aka.ms/dockertoolsforvscode) -taal services voor het bewerken van docker-bestanden met meer e2e-scenario's, afkomstig is
* [Informatie over Windows-Container](http://aka.ms/containers)-informatie voor Windows Server en Nano Server
* [Azure Containerservice](https://azure.microsoft.com/services/container-service/) - [Azure Container Service-inhoud](http://aka.ms/AzureContainerService)
* Zie voor meer voorbeelden van het werken met Docker [werken met Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) van de [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Voor meer introductiehandleidingen van de demo van HealthClinic.biz, verwijzen wij u naar [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Diverse Docker-hulpprogramma 's
[Sommige hulpmiddelen geweldige docker (blog van Steve Lasker)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Goede artikelen
[Inleiding tot Microservices van NGINX](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Presentaties
* [Steve Lasker: VS Live Las Vegas 2016 - Docker e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [Inleiding tot ASP.NET Core @ build 2016 - waar u op Demo](https://channel9.msdn.com/Events/Build/2016/B810)
* [.NET-toepassingen in containers Channel 9 ontwikkelen](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
