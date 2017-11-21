---
title: Een .NET-app in een container implementeren in Azure Service Fabric | Microsoft Docs
description: "Leert u hoe u een .NET-app in Visual Studio in een Docker-Container van het pakket. Deze nieuwe 'container'-app wordt geïmplementeerd op een Service Fabric-cluster."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: mikhegn
ms.openlocfilehash: 31c1cee5ddc4c8893da729af884ae7b7b8a58093
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
---
# <a name="deploy-a-net-application-in-a-windows-container-to-azure-service-fabric"></a>Een .NET-toepassing in een Windows-container implementeren op Azure Service Fabric

Deze zelfstudie laat zien hoe u een bestaande ASP.NET-toepassing in een Windows-container in Azure implementeren.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een Docker-project in Visual Studio
> * Een bestaande toepassing containerize
> * Continue integratie met Visual Studio en VSTS instellen

## <a name="prerequisites"></a>Vereisten

1. Installeer [Docker CE voor Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) zodat u containers op Windows 10 uitvoeren kunt.
2. Vertrouwd raken met de [Windows 10 Containers Quick Start][link-container-quickstart].
3. Download de [Fabrikam Fiber CallCenter] [ link-fabrikam-github] voorbeeldtoepassing.
4. Installeer [Azure PowerShell][link-azure-powershell-install]
5. Installeer de [uitbreiding continue levering van hulpprogramma's voor Visual Studio 2017][link-visualstudio-cd-extension]
6. Maak een [Azure-abonnement] [ link-azure-subscription] en een [Visual Studio Team Services-account][link-vsts-account]. 
7. [Een cluster maken in Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md)

## <a name="create-a-cluster-on-azure"></a>Een cluster maken op Azure
Service Fabric-toepassingen worden uitgevoerd op een cluster, een set netwerk verbonden virtuele of fysieke machines. [Setup uitgevoerd in Azure Service Fabric-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) voordat u maken en implementeren van uw toepassing. Wanneer u het cluster maakt, kiest u een SKU die ondersteuning biedt voor actieve containers (zoals Windows Server 2016 Datacenter met Containers).

## <a name="containerize-the-application"></a>De toepassing containerize

Nu dat u een Service Fabric-cluster worden uitgevoerd in Azure hebt bent u klaar voor een beperkte toepassing maken en implementeren. Voor het starten van onze toepassing wordt uitgevoerd in een container, moeten we voegen **Docker ondersteuning** aan het project in Visual Studio. Bij het toevoegen van **Docker ondersteuning** aan de toepassing, twee dingen gebeuren. Eerst een _Dockerfile_ wordt toegevoegd aan het project. Dit nieuwe bestand wordt beschreven hoe de installatiekopie van de container kan worden opgebouwd. Vervolgens tweede, een nieuwe _docker compose_ project wordt toegevoegd aan de oplossing. Het nieuwe project bevat enkele docker compose bestanden. Docker compose bestanden kunnen worden gebruikt om te beschrijven hoe de container wordt uitgevoerd.

Meer informatie over het werken met [Container met Visual Studio Tools][link-visualstudio-container-tools].

>[!NOTE]
>Als dit de eerste keer dat u Windows-container installatiekopieën op uw computer uitvoert, moet de base afbeeldingen voor uw containers Docker CE halen. De afbeeldingen in deze zelfstudie gebruikt zijn 14 GB. Opwekken en voer de volgende terminal opdracht voor het ophalen van de basisinstallatiekopieën:
>```cmd
>docker pull microsoft/mssql-server-windows-developer
>docker pull microsoft/aspnet:4.6.2
>```

### <a name="add-docker-support"></a>Docker-ondersteuning toevoegen

Open de [FabrikamFiber.CallCenter.sln] [ link-fabrikam-github] bestand in Visual Studio.

Met de rechtermuisknop op de **FabrikamFiber.Web** project > **toevoegen** > **Docker ondersteuning**.

### <a name="add-support-for-sql"></a>Ondersteuning voor SQL toevoegen

Deze toepassing wordt SQL gebruikt als de gegevensprovider zodat een SQL-server vereist is voor het uitvoeren van de toepassing. Verwijzen naar een installatiekopie van SQL Server-container in onze docker-compose.override.yml-bestand.

Open in Visual Studio **Solution Explorer**, vinden **docker compose**, en open het bestand **docker compose.override.yml**.

Navigeer naar de `services:` knooppunt toevoegen van een knooppunt met de naam `db:` waarmee wordt gedefinieerd met de SQL Server-vermelding voor de container.

```yml
  db:
    image: microsoft/mssql-server-windows-developer
    environment:
      sa_password: "Password1"
      ACCEPT_EULA: "Y"
    ports:
      - "1433"
    healthcheck:
      test: [ "CMD", "sqlcmd", "-U", "sa", "-P", "Password1", "-Q", "select 1" ]
      interval: 1s
      retries: 20
```

>[!NOTE]
>U kunt SQL-servers die u liever voor lokale foutopsporing gebruiken, zolang deze bereikbaar is vanaf de host is. Echter, **localdb** biedt geen ondersteuning voor `container -> host` communicatie.

>[!WARNING]
>Met SQL Server in een container biedt geen ondersteuning voor vastleggen van gegevens. Wanneer de container wordt gestopt, worden uw gegevens worden gewist. Gebruik deze configuratie niet voor productie.

Navigeer naar de `fabrikamfiber.web:` knooppunt en het toevoegen van een onderliggend knooppunt met de naam `depends_on:`. Dit zorgt ervoor dat de `db` (de SQL Server-container)-service wordt gestart voordat onze webtoepassing (fabrikamfiber.web).

```yml
  fabrikamfiber.web:
    depends_on:
      - db
```

### <a name="update-the-web-config"></a>Bijwerken van de webconfiguratie

Terug in de **FabrikamFiber.Web** project, het bijwerken van de verbindingsreeks in de **web.config** bestand om te verwijzen naar de SQL-Server in de container.

```xml
<add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

<add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
```

>[!NOTE]
>Als u wilt gebruiken een ander wordt SQL Server tijdens het bouwen van een release bouwen van uw webtoepassing, nog een verbindingsreeks toevoegen aan uw web.release.config-bestand.

### <a name="test-your-container"></a>De container testen

Druk op **F5** uitvoeren en fouten opsporen in de toepassing in de container.

Rand opent van uw toepassing gedefinieerde starten-pagina met het IP-adres van de container op het interne netwerk met NAT (meestal 172.x.x.x). Zie voor meer informatie over foutopsporing in toepassingen in containers met behulp van Visual Studio 2017, [in dit artikel][link-debug-container].

![Voorbeeld van fabrikam in een container][image-web-preview]

De container is nu klaar om te worden gebouwd en verpakt in een Service Fabric-toepassing. Zodra u de installatiekopie van het container gebouwd op uw computer hebt, kunt u dit doorgeven aan een container-register en voeg alles naar beneden op elke host worden uitgevoerd.

## <a name="get-the-application-ready-for-the-cloud"></a>Bereid u voor de toepassing voor de cloud

Om de toepassing klaar is voor het uitvoeren in Service Fabric in Azure, moeten we twee stappen:

1. De poort waarop we willen kunnen bereiken onze webtoepassing in de Service Fabric-cluster worden blootgesteld.
2. Geef een productie gereed SQL-database voor de toepassing.

### <a name="expose-the-port-for-the-app"></a>De poort voor de app weergeven
Het Service Fabric-cluster dat we hebben geconfigureerd, heeft poort *80* standaard geopend in de Azure Load Balancer, die een compromis tussen de binnenkomende verkeer naar het cluster. We kunnen onze container op deze poort beschikbaar via onze docker-compose.yml-bestand.

Open in Visual Studio **Solution Explorer**, vinden **docker compose**, en open het bestand **docker-compose.yml**.

Wijzig de `fabrikamfiber.web:` knooppunt een onderliggend knooppunt met de naam toevoegen `ports:`.

Een tekenreeks-vermelding toevoegen `- "80:80"`. Dit is wat uw docker-compose.yml-bestand moet eruitzien als:

```yml
  version: '3'

  services:
    fabrikamfiber.web:
      image: fabrikamfiber.web
      build:
        context: .\FabrikamFiber.Web
        dockerfile: Dockerfile
      ports:
        - "80:80"
```

### <a name="use-a-production-sql-database"></a>Gebruik een SQL-database voor productie
Wanneer in productie wordt uitgevoerd, moeten we onze gegevens behouden in de database. Er is momenteel geen manier om te waarborgen permanente gegevens in een container, dus u productiegegevens niet opslaan in SQL Server in een container.

U wordt aangeraden gebruikmaken van een Azure SQL Database. Als u wilt instellen en uitvoeren van een beheerde SQL-Server in Azure, gaat u naar de [Azure SQL Database snelstartgidsen] [ link-azure-sql] artikel.

>[!NOTE]
>Houd er rekening mee te wijzigen van de verbindingsreeksen in de SQL-server in de **web.release.config** bestand de **FabrikamFiber.Web** project.
>
>Deze toepassing mislukt probleemloos als geen SQL-database bereikbaar is. U kunt doorgaan en de toepassing implementeren met geen SQL-server.

## <a name="deploy-with-visual-studio-team-services"></a>Met Visual Studio teamservices implementeren

Als u een implementatie met behulp van Visual Studio Team Services instelt, moet u voor het installeren van de [extensie continue levering van hulpprogramma's voor Visual Studio 2017][link-visualstudio-cd-extension]. Deze extensie kunt eenvoudig implementeren in Azure door Visual Studio Team Services configureren en ophalen van uw app geïmplementeerd voor uw Service Fabric-cluster.

Om te beginnen, moet uw code worden gehost in broncodebeheer. De rest van deze sectie wordt ervan uitgegaan dat **git** wordt gebruikt.

### <a name="set-up-a-vsts-repo"></a>Een opslagplaats VSTS instellen
Klik op de rechterbenedenhoek van Visual Studio **toevoegen aan broncodebeheer** > **Git** (of welke optie u liever).

![Druk op de knop bron controle][image-source-control]

In de _Team Explorer_ deelvenster, drukt u op **Git-opslagplaats publiceren**.

Selecteer de naam van de opslagplaats VSTS en druk op **opslagplaats**.

![opslagplaats naar VSTS publiceren][image-publish-repo]

Nu dat uw code wordt gesynchroniseerd met een bron VSTS opslagplaats, kunt u continue integratie en continue levering kunt configureren.

### <a name="setup-continuous-delivery"></a>Continue levering instellen

In _Solution Explorer_, met de rechtermuisknop op de **oplossing** > **continue levering configureren**.

Selecteer het Azure-abonnement.

Stel **Type Host** naar **Service Fabric-Cluster**.

Stel **doelhost** naar de service fabric-cluster die u in de vorige sectie hebt gemaakt.

Kies een **Container register** voor het publiceren van de container.

>[!TIP]
>Gebruik de **bewerken** om te maken van een container-register.

Druk op **OK**.

![Setup service fabric continue integratie][image-setup-ci]
   
   Zodra de configuratie is voltooid, wordt de container geïmplementeerd naar Service Fabric. Wanneer u push-updates naar de opslagplaats voor een nieuwe build en versie wordt uitgevoerd.
   
   >[!NOTE]
   >Het opbouwen van de container afbeeldingen nemen ongeveer 15 minuten.
   >De eerste implementatie voor de Service Fabric-cluster zorgt ervoor dat de basis Windows Server Core-container afbeeldingen worden gedownload. Het downloaden van de duurt aanvullende 5-10 minuten.

Blader naar de Fabrikam aanroep Center-toepassing met de url van het cluster: bijvoorbeeld *http://mycluster.westeurope.cloudapp.azure.com*

Nu dat u hebt beperkte en de oplossing Fabrikam aanroep Center geïmplementeerd, kunt u opent de [Azure-portal] [ link-azure-portal] en de toepassing die wordt uitgevoerd in de Service Fabric zien. Probeer de toepassing, open een webbrowser en Ga naar de URL van uw Service Fabric-cluster.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Maak een Docker-project in Visual Studio
> * Een bestaande toepassing containerize
> * Continue integratie met Visual Studio en VSTS instellen

In het volgende gedeelte van de zelfstudie, informatie over het instellen van [bewaking voor de container](service-fabric-tutorial-monitoring-wincontainers.md).

<!--   NOTE SURE WHAT WE SHOULD DO YET HERE

Advance to the next tutorial to learn how to bind a custom SSL certificate to it.

> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps](app-service-web-tutorial-custom-ssl.md)

## Next steps

- [Container Tooling in Visual Studio][link-visualstudio-container-tools]
- [Get started with containers in Service Fabric][link-servicefabric-containers]
- [Creating Service Fabric applications][link-servicefabric-createapp]
-->

[link-debug-container]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-fabrikam-github]: https://aka.ms/fabrikamcontainer
[link-container-quickstart]: /virtualization/windowscontainers/quick-start/quick-start-windows-10
[link-visualstudio-container-tools]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-azure-powershell-install]: /powershell/azure/install-azurerm-ps
[link-servicefabric-create-secure-clusters]: service-fabric-cluster-creation-via-arm.md
[link-visualstudio-cd-extension]: https://aka.ms/cd4vs
[link-servicefabric-containers]: service-fabric-get-started-containers.md
[link-servicefabric-createapp]: service-fabric-create-your-first-application-in-visual-studio.md
[link-azure-portal]: https://portal.azure.com
[link-sf-clustertemplate]: https://aka.ms/securepreviewonelineclustertemplate
[link-azure-pricing-calculator]: https://azure.microsoft.com/en-us/pricing/calculator/
[link-azure-subscription]: https://azure.microsoft.com/en-us/free/
[link-vsts-account]: https://www.visualstudio.com/team-services/pricing/
[link-azure-sql]: /azure/sql-database/

[image-web-preview]: media/service-fabric-host-app-in-a-container/fabrikam-web-sample.png
[image-source-control]: media/service-fabric-host-app-in-a-container/add-to-source-control.png
[image-publish-repo]: media/service-fabric-host-app-in-a-container/publish-repo.png
[image-setup-ci]: media/service-fabric-host-app-in-a-container/configure-continuous-integration.png
