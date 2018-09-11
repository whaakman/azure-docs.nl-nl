---
title: Foutopsporing van apps in een lokale Docker-container | Microsoft Docs
description: Meer informatie over het wijzigen van een app die wordt uitgevoerd in een lokale Docker-container, de container via bewerken en vernieuwen vernieuwen en Stel onderbrekingspunten foutopsporing
services: azure-container-service
documentationcenter: na
author: ghogen
manager: douge
editor: ''
ms.assetid: 480e3062-aae7-48ef-9701-e4f9ea041382
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/22/2016
ms.author: mlearned
ms.openlocfilehash: 07a7c1e11d8ca20ff4f42abcb84961cb7cd9e0e1
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44298202"
---
# <a name="debugging-apps-in-a-local-docker-container"></a>Fouten in apps in een lokale Docker-container opsporen
## <a name="overview"></a>Overzicht
Visual Studio 2017 biedt een consistente manier om te ontwikkelen in een Linux-Docker-container en uw toepassing lokaal valideren.
U hebt geen opnieuw opstarten van de container telkens wanneer u een code wijzigen.
In dit artikel ziet u hoe u de functie 'Bewerken en vernieuwen' gebruiken om een ASP.NET Core Web-app te starten in een lokale Docker-container, breng eventueel benodigde wijzigingen en vernieuw de browser om deze wijzigingen te bekijken.
In dit artikel leest u ook hoe om in te stellen onderbrekingspunten voor foutopsporing.

> [!NOTE]
> Ondersteuning voor Windows-containers wordt in een toekomstige release binnenkort
>
>

## <a name="prerequisites"></a>Vereiste onderdelen
De volgende hulpprogramma's moeten worden geïnstalleerd.

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)
* [Microsoft ASP.NET Core 1.0-SDK](https://go.microsoft.com/fwlink/?LinkID=809122)

Als u wilt uitvoeren van Docker-containers lokaal, moet u een lokale docker-client.
U kunt de [Docker werkset](https://www.docker.com/products/docker-toolbox), hiervoor Hyper-V moet worden uitgeschakeld of kunt u [Docker voor Windows](https://www.docker.com/get-docker), die gebruikmaakt van Hyper-V en Windows 10 is vereist.

Als u Docker-werkset gebruikt, moet u naar [de Docker-client configureren](vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. Een web-app maken
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Docker-ondersteuning toevoegen
[!INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-edit-your-code-and-refresh"></a>3. Bewerken van uw code en vernieuwen
U kunt snel Voortborduren wijzigingen, kunt u uw toepassing binnen een container te starten, om te blijven wijzigingen aanbrengen, weergeven zoals u zou met IIS Express doen.

1. De configuratie van de oplossing ingesteld op `Debug` en druk op  **&lt;CTRL + F5 >** uw docker-installatiekopie bouwen en lokaal uitvoeren.

    Zodra de container-installatiekopie is gemaakt en wordt uitgevoerd in een Docker-container, wordt de Web-app in uw standaardbrowser gestart in Visual Studio.
    Als u van de browser Microsoft Edge gebruikmaakt of anders fouten bevatten, Zie [probleemoplossing](vs-azure-tools-docker-troubleshooting-docker-errors.md) sectie.
2. Ga naar de pagina over, dit is waar we gaan onze wijzigingen aanbrengen.
3. Ga terug naar Visual Studio en open `Views\Home\About.cshtml`.
4. De volgende HTML-inhoud toevoegen aan het einde van het bestand en sla de wijzigingen op.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```
5. Het uitvoervenster weergeven wanneer de .NET-build is voltooid en u deze regels bekijken, gaat u terug naar uw browser en vernieuw de pagina over.

   ```
   Now listening on: http://*:80
   Application started. Press Ctrl+C to shut down
   ```
6. Uw wijzigingen zijn toegepast.

## <a name="4-debug-with-breakpoints"></a>4. Fouten opsporen met onderbrekingspunten
Vaak moet wijzigingen verdere controle, gebruik te maken van de foutopsporing functies van Visual Studio.

1. Ga terug naar Visual Studio en open `Controllers\HomeController.cs`
2. Vervang de inhoud van de methode About() door het volgende:

   ```
   string message = "Your application description page from within a Container";
   ViewData["Message"] = message;
   ````
3. Stel een onderbrekingspunt aan de linkerkant van de `string message`... regel.
4. Raak  **&lt;F5 >** foutopsporing te starten.
5. Navigeer naar de pagina over naar uw onderbrekingspunt bereikt.
6. Ga naar Visual Studio om het onderbrekingspunt weer te geven, en controleren van de waarde van het bericht.

   ![][2]

## <a name="summary"></a>Overzicht
Met Docker-ondersteuning in Visual Studio 2017 krijgt u de productiviteit van lokaal, werken met de productie-realistischer van het ontwikkelen van binnen een Docker-container.

## <a name="troubleshooting"></a>Probleemoplossing
[Oplossen van problemen met Visual Studio Docker-ontwikkeling](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Meer informatie over Docker met Visual Studio, Windows en Azure
* [Docker-hulpprogramma's voor Azure DevOps](http://aka.ms/dockertoolsforvsts) : bouwen en implementeren van docker-containers
* [Docker-hulpprogramma's voor Visual Studio Code](http://aka.ms/dockertoolsforvscode) -Language-services voor het bewerken van docker-bestanden, met meer e2e-scenario's die afkomstig zijn
* [Informatie voor Windows-Container](http://aka.ms/containers)-informatie voor Windows Server en Nano Server
* [Azure Containerservice](https://azure.microsoft.com/services/container-service/) - [Azure Container Service-inhoud](http://aka.ms/AzureContainerService)
* Zie voor meer voorbeelden van het werken met Docker [werken met Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) uit de [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Voor meer introductiehandleidingen van de demo van HealthClinic.biz, verwijzen wij u naar [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Verschillende Docker-hulpprogramma 's
[Enkele geweldige docker-hulpprogramma's (blog van Steve Lasker)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Goede artikelen
[Inleiding tot Microservices van NGINX](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Presentaties
* [Steve Lasker: VS Live Las Vegas 2016 - Docker-e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [Inleiding tot ASP.NET Core @ build 2016 - waar u op Demo](https://channel9.msdn.com/Events/Build/2016/B810)
* [Het ontwikkelen van .NET-apps in containers, Channel 9](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
