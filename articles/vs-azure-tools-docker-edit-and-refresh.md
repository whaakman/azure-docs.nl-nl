---
title: Foutopsporing van apps in een lokale Docker-container | Microsoft Docs
description: Meer informatie over het wijzigen van een app die wordt uitgevoerd in een lokale Docker-container, de container via bewerken en vernieuwen vernieuwen en Stel onderbrekingspunten foutopsporing
services: container-service
author: ghogen
manager: douge
ms.assetid: 480e3062-aae7-48ef-9701-e4f9ea041382
ms.service: multiple
ms.topic: article
ms.workload: multiple
ms.date: 09/11/2018
ms.author: ghogen
ms.openlocfilehash: 0f3f323cb4486c06f6f18de4c695efaf8dce4d99
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44715717"
---
# <a name="debugging-apps-in-a-local-docker-container"></a>Fouten in apps in een lokale Docker-container opsporen
## <a name="overview"></a>Overzicht
Visual Studio 2017 biedt een consistente manier om te ontwikkelen in een Docker-container en uw toepassing lokaal valideren.
U hebt geen opnieuw opstarten van de container telkens wanneer u een code wijzigen.
In dit artikel ziet u hoe u de functie 'Bewerken en vernieuwen' gebruiken om een ASP.NET Core Web-app te starten in een lokale Docker-container, breng eventueel benodigde wijzigingen en vernieuw de browser om deze wijzigingen te bekijken.
In dit artikel leest u ook hoe om in te stellen onderbrekingspunten voor foutopsporing.

## <a name="prerequisites"></a>Vereisten
De volgende hulpprogramma's moeten worden geïnstalleerd.

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de ontwikkeling van Web-werkbelasting geïnstalleerd.

Als u wilt uitvoeren van Docker-containers lokaal, moet u een lokale docker-client.
U kunt de [Docker werkset](https://www.docker.com/products/docker-toolbox), hiervoor Hyper-V moet worden uitgeschakeld of kunt u [Docker voor Windows](https://www.docker.com/get-docker), die gebruikmaakt van Hyper-V en Windows 10 is vereist.

Als u Docker-werkset gebruikt, moet u naar [de Docker-client configureren](vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. Een webtoepassing maken
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-edit-your-code-and-refresh"></a>2. Bewerken van uw code en vernieuwen
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

## <a name="3-debug-with-breakpoints"></a>3. Fouten opsporen met onderbrekingspunten
Vaak moet wijzigingen verdere controle, gebruik te maken van de foutopsporing functies van Visual Studio.

1. Ga terug naar Visual Studio en open `About.cshtml.cs`
2. Vervang de inhoud van de methode OnGet() door het volgende:

   ```cs
       Message = "Your application description page from within a Container";
   ```

3. Stel een onderbrekingspunt aan de linkerkant van de coderegel.
4. Raak  **&lt;F5 >** foutopsporing te starten.
5. Navigeer naar de pagina over naar uw onderbrekingspunt bereikt.
6. Ga naar Visual Studio om het onderbrekingspunt weer te geven, en controleren van de waarde van het bericht.

   ![][2]

## <a name="summary"></a>Samenvatting
Met Docker-ondersteuning in Visual Studio 2017 krijgt u de productiviteit van lokaal, werken met de productie-realistischer van het ontwikkelen van binnen een Docker-container.

## <a name="troubleshooting"></a>Problemen oplossen
[Oplossen van problemen met Visual Studio Docker-ontwikkeling](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Meer informatie over Docker met Visual Studio, Windows en Azure
* [Ontwikkeling van containers met Visual Studio](/visualstudio/containers) -een container ontwikkeling hub-pagina
* [Integratie met docker voor Azure-pijplijnen](http://aka.ms/dockertoolsforvsts) : bouwen en implementeren van docker-containers
* [Docker-hulpprogramma's voor Visual Studio Code](http://aka.ms/dockertoolsforvscode) -Language-services voor het bewerken van docker-bestanden, met meer e2e-scenario's die afkomstig zijn
* [Informatie voor Windows-Container](http://aka.ms/containers)-informatie voor Windows Server en Nano Server
* [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/) - [documentatie voor Azure Kubernetes Service](/azure/aks)

## <a name="various-docker-tools"></a>Verschillende Docker-hulpprogramma 's
[Enkele geweldige docker-hulpprogramma's (blog van Steve Lasker)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Goede artikelen
[Inleiding tot Microservices van NGINX](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Presentaties
* [Steve Lasker: VS Live Las Vegas 2016 - Docker-e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [Inleiding tot ASP.NET Core @ build 2016 - waar u op Demo](https://channel9.msdn.com/Events/Build/2016/B810)
* [Het ontwikkelen van .NET-apps in containers, Channel 9](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
